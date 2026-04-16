---
id: KD-004
challenge: challenges/detect-this/challenge-001.md
---

# Solution KD-004 — Ghost in the Memory

## ✅ Answer / Respuesta

```kql
DeviceProcessEvents
| where TimeGenerated > ago(24h)
| where
    // Technique 1: comsvcs.dll MiniDump via rundll32
    (
        ProcessCommandLine has_all ("comsvcs.dll", "MiniDump")
        or ProcessCommandLine has_all ("comsvcs", "minidump")
    )
    // Technique 2: Direct procdump targeting lsass
    or (
        FileName =~ "procdump.exe"
        and ProcessCommandLine has "lsass"
    )
    // Technique 3: Mimikatz patterns
    or ProcessCommandLine has_any ("sekurlsa", "lsadump", "mimikatz")
    // Technique 4: sqldumper targeting lsass PID range
    or (
        FileName =~ "sqldumper.exe"
        and ProcessCommandLine matches regex @"\d{3,5}\s+0\s+0x"
    )
| project
    TimeGenerated,
    DeviceName,
    AccountName = InitiatingProcessAccountName,
    InitiatingProcessFileName,
    ProcessCommandLine,
    FileName,
    FolderPath
| order by TimeGenerated desc
```

---

## 🔎 Explanation / Explicación

### Reading the log sample

The three events in the challenge represent a complete `comsvcs.dll MiniDump` attack chain:

1. **`cmd.exe` spawns `rundll32.exe`** with `comsvcs.dll, MiniDump 624 C:\Users\Public\lsass.dmp full`
   - `comsvcs.dll` is a Windows DLL that exports `MiniDump`, a function normally used by crash dump utilities
   - `624` is the PID of `lsass.exe` — the attacker obtained it beforehand (via `tasklist` or similar)
   - The output path `C:\Users\Public\` is world-writable — deliberate choice for exfil staging

2. **`rundll32.exe` opens `lsass.exe`** — this is the actual memory access event. `lsass.exe` holds NTLM hashes, Kerberos tickets, and plaintext passwords in memory.

3. **`lsass.dmp` is created** — the credential store is now on disk, ready to be parsed offline by tools like Mimikatz.

### Detection logic breakdown

**`has_all`** checks that both substrings appear in the command line — necessary because `comsvcs.dll` alone appears in legitimate crash dump operations, and `MiniDump` alone is too generic. The combination targeting a process ID is the signal.

**Case-insensitive matching** covers attacker obfuscation like `MINIDUMP`, `MiniDump`, `minidump`.

**`procdump.exe` + `lsass`** catches the Sysinternals `procdump` technique: `procdump -ma lsass.exe lsass.dmp`. Procdump is signed by Microsoft, making it a common LOLBIN choice.

**`sekurlsa` / `lsadump`** are Mimikatz module names that appear in command line invocations. These are not present in any legitimate software.

**`sqldumper.exe`** is a Microsoft SQL Server binary that can be abused similarly to `comsvcs.dll`. The regex `\d{3,5}\s+0\s+0x` matches the PID + thread ID + flags pattern used in the abuse technique.

[ES]

### Lectura de la muestra de logs

Los tres eventos representan una cadena de ataque completa con `comsvcs.dll MiniDump`:

1. `cmd.exe` lanza `rundll32.exe` con `comsvcs.dll, MiniDump 624 C:\Users\Public\lsass.dmp full`. El PID 624 corresponde a `lsass.exe`.
2. `rundll32.exe` abre `lsass.exe` — el acceso a memoria real ocurre aquí.
3. Se crea `lsass.dmp` — el almacén de credenciales está ahora en disco.

### Lógica de detección

- `has_all` verifica que ambas subcadenas aparezcan en la línea de comando — la combinación es la señal, no los términos individuales.
- Se cubren variantes de capitalización con coincidencia insensible a mayúsculas.
- `procdump.exe` + `lsass` captura el abuso del LOLBIN de Sysinternals.
- `sekurlsa` y `lsadump` son nombres de módulos de Mimikatz que no aparecen en ningún software legítimo.

---

## ⚡ Performance Notes / Notas de rendimiento

- `has_all` is more efficient than multiple `contains` calls — it evaluates the full-text index once.
- The `regex` match on `sqldumper.exe` is the most expensive clause. It's last in the `or` chain intentionally — KQL short-circuits, so the cheaper string checks run first and most rows never reach the regex.
- For production use at scale, consider splitting this into separate `AlertRules` per technique category — it makes tuning individual false-positive sources easier.

---

## 📚 References / Referencias

- [Microsoft: DeviceProcessEvents schema](https://learn.microsoft.com/en-us/microsoft-365/security/defender/advanced-hunting-deviceprocessevents-table)
- [MITRE ATT&CK T1003.001: LSASS Memory](https://attack.mitre.org/techniques/T1003/001/)
- [LOLBAS: comsvcs.dll](https://lolbas-project.github.io/lolbas/Libraries/Comsvcs/)
- [LOLBAS: sqldumper.exe](https://lolbas-project.github.io/lolbas/OtherMSBinaries/Sqldumper/)
- [Mimikatz GitHub](https://github.com/gentilkiwi/mimikatz)
