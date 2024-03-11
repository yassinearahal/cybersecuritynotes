**<mark style="background: #FF5582A6;">Introduction</mark>:**

In the realm of privilege escalation, administrators utilize "Capabilities" to manage privileges at a more granular level. This technique proves valuable when specific tasks require elevated permissions without granting broader access. Here's a concise guide to leveraging capabilities:

**1. <mark style="background: #FF5582A6;">Understanding Capabilities</mark>:**

   - Capabilities allow fine-grained privilege management.
   - Ideal for scenarios where specific tasks need elevated permissions without granting full privileges.

**2. <mark style="background: #FF5582A6;">Capabilities Management</mark>:**

   - Use the capabilities man page for detailed information.
   - Modify capabilities to empower binaries without escalating user privileges.

**3. <mark style="background: #FF5582A6;">Identifying Enabled Capabilities</mark>:**

   - Utilize the <mark style="background: #BBFABBA6;">`getcap`</mark> tool to list enabled capabilities.
   - Example: (Redirect errors to /dev/null for clean output).
```bash
getcap -r / 2>/dev/null
```

**4. <mark style="background: #FF5582A6;">Unique Privilege Escalation</mark>:**

   - Unlike SUID, capabilities provide a less apparent privilege escalation vector.
   - Set capabilities enable specific binaries to perform tasks without elevated user privileges.

**5. <mark style="background: #FF5582A6;">Practical Example - Leveraging Vim</mark>:**

   - Vim, without the SUID bit, can still be leveraged for privilege escalation.
   - Utilize GTFOBins for a curated list of binaries exploitable with set capabilities.