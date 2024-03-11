#### <mark style="background: #FF5582A6;">Unattended Windows Installations</mark>

When deploying Windows on multiple hosts via Windows Deployment Services, administrators may perform unattended installations without user interaction. Credentials used during the initial setup might be stored in various locations:

- `C:\Unattend.xml`
- `C:\Windows\Panther\Unattend.xml`
- `C:\Windows\Panther\Unattend\Unattend.xml`
- `C:\Windows\system32\sysprep.inf`
- `C:\Windows\system32\sysprep\sysprep.xml`

Example Credential Section:

```xml
<Credentials>
    <Username>Administrator</Username>
    <Domain>thm.local</Domain>
    <Password>MyPassword123</Password>
</Credentials>
```

#### <mark style="background: #FF5582A6;">Powershell History</mark>

Powershell commands, including passwords, are stored in a history file. Retrieve from a cmd.exe prompt:

```bash
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

**Note:** The command above will only work from cmd.exe, as Powershell won't recognize `%userprofile%` as an environment variable. To read the file from Powershell, w'd have to replace `%userprofile%` with `$Env:userprofile`.
#### <mark style="background: #FF5582A6;">Saved Windows Credentials</mark>

Windows allows us to use other users' credentials. This function also gives the option to save these credentials on the system. The command below will list saved credentials:

List saved credentials:

```bash
cmdkey /list
```

While we can't see the actual passwords, if we notice any credentials worth trying, we can use them with the `runas` command and the `/savecred` option, as seen below.

Use credentials with `runas` command:

```bash
runas /savecred /user:admin cmd.exe
```

#### <mark style="background: #FF5582A6;">IIS Configuration</mark>

IIS web server configurations in `web.config` files may store passwords. Search for database connection strings:

```bash
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```

#### <mark style="background: #FF5582A6;">Retrieve Credentials from PuTTY</mark>

PuTTY, a common SSH client on Windows, stores session configurations. Retrieve proxy credentials from the registry:

```bash
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

Note: Replace `%userprofile%` with `$Env:userprofile` in Powershell.

#### <mark style="background: #FF5582A6;">General Note</mark>

Software like PuTTY, browsers, email clients, FTP clients, SSH clients, and VNC software may store saved passwords. Explore respective methods for password recovery in each software.