### Downloading Files:

The ``wget`` command allows us to download files from the web via HTTP protocol.
### Syntax:

```bash
wget  [ option ]   [ URL ]
```

Or we can use Certutility to downlod files !

```bash
certutil -urlcache -f http://example.com/binary/file.exe C:\destination\file.exe
```
### Important Switches:

```bash
wget -r              # Download files recursively, including all linked files
wget -o <logfile> or wget --output-file=<logfile> #Specifies the log file to save the download progress and messages.
wget -O <filename> or wget --output-document=<filename> #Specifies the output filename for the downloaded file.
wget -np             # Prevent ascending to parent directories during recursive downloads
wget -nd             # Download all files into the current directory without creating subdirectories
wget -nc             # Do not overwrite existing files
wget -N              # Download files only if they are newer than the local copies
wget -A .pdf,.txt    # Accept files with specified extensions (e.g., .pdf, .txt)
wget -R .exe,.zip    # Reject files with specified extensions (e.g., .exe, .zip)
wget --user=USERNAME # Provide a username for HTTP basic authentication
wget --password=PASS # Provide a password for HTTP basic authentication
wget --header=HEADER # Add custom headers to the HTTP request
wget --post-data=DATA # Send POST data as part of the request
wget --referer=URL   # Set the referer header to a specified URL
wget --user-agent=UA # Specify a custom user agent string in the HTTP request
```

### Transferring Files & Directories:

Secure copy, or SCP, is just that -- a means of securely copying files. Unlike the regular cp command, this command allows us to transfer files between two computers using the SSH protocol to provide both authentication and encryption.

### Syntax:

**Copy a local file to a remote server:**
```ruby
scp /path/to/local/file.txt user@remote:/path/on/remote/
```

**Copy a remote file to the local machine:**
```ruby
scp user@remote:/path/to/remote/file.txt /path/on/local/
```

**Copy an entire directory from the local machine to a remote server:**
```ruby
scp -r /path/to/local/directory/ user@remote:/path/on/remote/
```

**Copy an entire directory from a remote server to the local machine:**
```ruby
scp -r user@remote:/path/to/remote/directory/ /path/on/local/
```

**Copy a file using a specific SSH key file:**
```ruby
scp -i /path/to/private/key.pem /path/to/local/file.txt user@remote:/path/on/remote/
```

**Copy a file using a specific port (e.g., port 2222):**
```ruby
scp -P 2222 /path/to/local/file.txt user@remote:/path/on/remote/
```

**Preserve file permissions and timestamps during copy:**
```ruby
scp -p /path/to/local/file.txt user@remote:/path/on/remote/
```

**Compress the files during transfer (faster for slow connections):**
```ruby
scp -C /path/to/local/file.txt user@remote:/path/on/remote/
```

### Serving Files:

Ubuntu machines come pre-packaged with python3. Python helpfully provides a lightweight and easy-to-use module called "HTTPServer". This module turns our computer into a quick and easy web server that you can use to serve our own files, where we can download it using `curl`  or  `wget`

### Syntax:

```python
python3 -m http.server [port]
```

### Important Switches:

```python
python3 -m http.server               # Serve files using the default settings (port 8000, current directory)
python3 -m http.server --bind IP     # Bind the server to a specific IP address or hostname
python3 -m http.server --directory DIR  # Specify the directory to serve files from
python3 -m http.server --port PORT   # Specify the port number to listen on
python3 -m http.server --cgi         # Enable CGI support for executing scripts
python3 -m http.server --cgi-undo    # Disable CGI script execution
python3 -m http.server --cgi-suffix SUFFIX  # Set the CGI script file extension
python3 -m http.server --ip-restrict IP     # Restrict access to the server to a specific IP address or range
python3 -m http.server --help        # Display the help message
```

#### Alternative:

Updog is a replacement for Python's `SimpleHTTPServer`. It allows uploading and downloading via HTTP/S.

#### Installation:
```bash
pip3  install  updog
```

### Syntax:

**Serve from our current directory:**
```bash
updog
```

### Important Switches:
```bash
updog                          # Share files from the current directory on port 9090
updog -p PORT                  # Specify the port number to listen on
updog -d DIRECTORY             # Serve files from a specific directory
updog -i INTERFACE             # Bind to a specific network interface
updog -c USERNAME:PASSWORD     # Provide basic authentication credentials
updog -a                       # Automatically open the shared URL in a web browser
updog -n                       # Disable automatically opening the shared URL in a web browser
updog -H HOSTNAME              # Specify the hostname or IP address to display in the URL
updog -S                       # Enable SSL/TLS encryption for HTTPS connections
```




