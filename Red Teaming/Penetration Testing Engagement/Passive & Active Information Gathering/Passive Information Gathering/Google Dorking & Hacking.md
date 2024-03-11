#### What is it ?
Google Dorking, also known as Google Hacking, is a technique used in penetration testing to discover sensitive information and vulnerabilities in online systems. It involves utilizing <mark style="background: #FF5582A6;">advanced search operators</mark> and techniques to effectively leverage Google's search engine for reconnaissance purposes. By using specific search queries, known as Google dorks, we can uncover information that is not typically indexed or easily accessible. This method can reveal <mark style="background: #FF5582A6;">exposed directories, leaked credentials, vulnerable web applications</mark>, and other valuable details that can aid in <mark style="background: #FF5582A6;">assessing</mark> the security posture of a target. However, it's important to note that Google Dorking should only be performed within legal and ethical boundaries, with proper authorization, and with respect for privacy and security.

##### <mark style="background: #BBFABBA6;">Examples</mark> of Google Dorks:

```markdown
1. Site Search: site:example.com
2. File Type Search: filetype:pdf
3. Directory Listing: intitle:index.of mp3
4. Login Pages: inurl:login or intitle:login
5. Error Messages: intext:error or intitle:error
6. Vulnerable Applications: inurl:/phpinfo.php
```

##### Google Dorks related to Penetration Testing:

```markdown
1. Vulnerable Web Applications:
   - inurl:/phpmyadmin
   - inurl:/wp-login.php

2. Server Misconfigurations:
   - intitle:Apache default page
   - intitle:IIS welcome page

3. Exposed Databases:
   - intitle:index of intext:database
   - inurl:/dbbackup

4. Exposed Git Repositories:
   - inurl:.git

5. Network Devices:
   - inurl:/cgi-bin/cmh/webcam.sh
   - intitle:Netcam Live Image

6. Cross-Site Scripting (XSS):
   - inurl:.php?cid=
   - inurl:/cgi-bin/feedback.cgi?subject=
```

#### Web Crawlers ?
Web crawlers, also known as <mark style="background: #FF5582A6;">spiders or bots</mark>, are <mark style="background: #FF5582A6;">automated software programs</mark> that <mark style="background: #FF5582A6;">systematically</mark> browse the internet to discover and index web pages. They start by visiting a seed URL and then follow links on that page to other pages, creating a network of <mark style="background: #FF5582A6;">interconnected web pages</mark>. Web crawlers typically operate by sending HTTP requests to web servers, retrieving the HTML content of a page, and analyzing its links to determine which pages to visit next. They traverse through websites, recursively visiting linked pages, and storing relevant data about each page, such as its URL, content, and metadata. This process continues until the crawler has covered a significant portion of the web or has reached a predefined limit.

And here's an ASCII art representation of a web crawler:

```sql
            +-------------+
            | Web Crawler |
            +-------------+
                  |
                  | 1. Start from a seed URL
                  |
                  V
       +-------------------+
       |   Fetch HTML      |
       |   content of page |
       +-------------------+
                  |
                  | 2. Extract links from the page
                  |
                  V
       +-------------------+
       |   Follow links    |
       |   to other pages  |
       +-------------------+
                  |
                  | 3. Repeat steps 1 and 2 for each page
                  |
                  V
       +-------------------+
       |   Store data      |
       |   about each page |
       +-------------------+

```
