#### What are they ?
### Robots.txt
Robots.txt is a file placed on a website that guides web crawlers on <mark style="background: #FF5582A6;">how to interact with the site</mark>. It uses specific directives to control access to different sections. The "User-agent" field specifies which crawler the directives apply to. "Disallow" indicates which areas are restricted, while "Allow" overrides the restrictions for specific paths. The "Sitemap" field specifies the location of the XML sitemap. In penetration testing, analyzing the Robots.txt file can reveal <mark style="background: #FF5582A6;">restricted areas</mark> and aid in identifying potential vulnerabilities or sensitive information.

Here's a JavaScript representation:

```javascript
User-agent: *
Disallow: /admin/
Disallow: /private/

User-agent: Googlebot
Allow: /public/

Sitemap: https://www.example.com/sitemap.xml
```

### Sitemaps
Sitemaps: <mark style="background: #FF5582A6;">XML files</mark> that outline a website's structure and URLs, providing search engines with a roadmap to navigate and index the site's content. In penetration testing, analyzing sitemaps helps <mark style="background: #FF5582A6;">identify hidden or restricted areas, prioritize testing efforts, and uncover potential vulnerabilities or misconfigurations.</mark>

Sitemap Syntax:
```php
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
   <url>
      <loc>https://www.example.com/page1</loc>
      <lastmod>2023-05-30</lastmod>
      <priority>0.8</priority>
   </url>
   <url>
      <loc>https://www.example.com/page2</loc>
      <lastmod>2023-05-29</lastmod>
      <priority>0.6</priority>
   </url>
</urlset>
```