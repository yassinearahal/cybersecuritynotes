**<mark style="background: #FF5582A6;">Windows Privilege Escalation Simplified</mark>:**

Privilege escalation in Windows involves leveraging initial access as "user A" to gain unauthorized access to a more privileged account, "user B," by exploiting weaknesses in the target system. While the ultimate goal is often obtaining administrative rights, there are instances where escalating to other unprivileged accounts is necessary before achieving administrative privileges.

**<mark style="background: #FF5582A6;">Windows Tokens</mark>**

Windows uses tokens to ensure that accounts have the right privileges to carry out particular actions. Account tokens are assigned to an account when users log in or are authenticated. This is usually done by LSASS.exe(think of this as an authentication process).

This access token consists of:

- <mark style="background: #BBFABBA6;">User SIDs(security identifier)</mark>
- <mark style="background: #BBFABBA6;">Group SIDs</mark>
- <mark style="background: #BBFABBA6;">Privileges</mark>

Amongst other things. More detailed information can be found [here](https://docs.microsoft.com/en-us/windows/win32/secauthz/access-tokens).

There are two types of access tokens:

- <mark style="background: #BBFABBA6;">Primary access tokens</mark>: those associated with a user account that are generated on log on
- <mark style="background: #BBFABBA6;">Impersonation tokens</mark>: these allow a particular process(or thread in a process) to gain access to resources using the token of another (user/client) process

For an impersonation token, there are different levels:

- <mark style="background: #BBFABBA6;">SecurityAnonymous</mark>: current user/client cannot impersonate another user/client
- <mark style="background: #BBFABBA6;">SecurityIdentification</mark>: current user/client can get the identity and privileges of a client but cannot impersonate the client
- <mark style="background: #BBFABBA6;">SecurityImpersonation</mark>: current user/client can impersonate the client's security context on the local system
- <mark style="background: #BBFABBA6;">SecurityDelegation</mark>: current user/client can impersonate the client's security context on a remote system

Where the security context is a data structure that contains users' relevant security information.

The privileges of an account(which are either given to the account when created or inherited from a group) allow a user to carry out particular actions. Here are the most commonly abused privileges:

- <mark style="background: #BBFABBA6;">SeImpersonatePrivilege</mark>
- <mark style="background: #BBFABBA6;">SeAssignPrimaryPrivilege</mark>
- <mark style="background: #BBFABBA6;">SeTcbPrivilege</mark>
- <mark style="background: #BBFABBA6;">SeBackupPrivilege</mark>
- <mark style="background: #BBFABBA6;">SeRestorePrivilege</mark>
- <mark style="background: #BBFABBA6;">SeCreateTokenPrivilege</mark>
- <mark style="background: #BBFABBA6;">SeLoadDriverPrivilege</mark>
- <mark style="background: #BBFABBA6;">SeTakeOwnershipPrivilege</mark>
- <mark style="background: #BBFABBA6;">SeDebugPrivilege</mark>

There's more reading [here](https://www.exploit-db.com/papers/42556).

**<mark style="background: #FF5582A6;">Key Methods of Privilege Escalation</mark>:**

1. **<mark style="background: #BBFABBA6;">Credential Discovery</mark>:**

   Accessing different accounts may involve discovering credentials in unsecured files or spreadsheets. This straightforward method is effective when users inadvertently leave sensitive information exposed.

2. **<mark style="background: #BBFABBA6;">Exploiting Weaknesses</mark>:**

   Privilege escalation can exploit various weaknesses, including:
   - Misconfigurations in Windows services or scheduled tasks.
   - Excessive privileges assigned to user accounts.
   - Vulnerabilities in software applications.
   - Unpatched security vulnerabilities in Windows.

**<mark style="background: #FF5582A6;">Understanding Windows User Types</mark>:**

Windows users are broadly categorized into two types based on their access levels:
- **<mark style="background: #BBFABBA6;">Administrators</mark>:** Users with the highest privileges, capable of modifying system configurations and accessing all files.
- **<mark style="background: #BBFABBA6;">Standard Users</mark>:** Users with limited access, restricted from making significant changes to the system and limited to their own files.

**<mark style="background: #FF5582A6;">Built-in Accounts for Privilege Escalation</mark>:**

Special built-in accounts, managed by Windows, play a crucial role:
- **<mark style="background: #BBFABBA6;">SYSTEM / LocalSystem</mark>:** An account with extensive privileges, surpassing even administrators, used for internal tasks.
- **<mark style="background: #BBFABBA6;">Local Service</mark>:** A default account for running Windows services with minimal privileges, using anonymous network connections.
- **<mark style="background: #BBFABBA6;">Network Service</mark>:** Another default account for running Windows services with minimal privileges, authenticating through the computer credentials.


**<mark style="background: #FF5582A6;">Privilege Enumeration:</mark>**

```cmd
whoami /priv
```