---
title: OWASP Web Application Security Testing Checklist
date: 2023-03-28 14:30:00 +/-TTTT
categories: [WEB,OWASP,TESTING,SECURITY]
tags: [WEB,OWASP,TESTING,SECURITY]     # TAG names should always be lowercase
---


# OWASP Web Application Security Testing Checklist

## Table of Contents

* [Information Gathering](#Information)
* [Configuration Management](#Configuration)
* [Secure Transmission](#Transmission)
* [Authentication](#Authentication)
* [Session Management](#Session)
* [Authorization](#Authorization)
* [Data Validation](#Validation)
* [Denial of Service](#Denial)
* [Business Logic](#Business)
* [Cryptography](#Cryptography)
* [Risky Functionality - File Uploads](#File)
* [Risky Functionality - Card Payment](#Card)
* [HTML 5](#HTML)
* [WAF Detection](#waf-detection)


-------
### <a name="Information">Information Gathering</a>
- [ ] Manually explore the site
- [ ] Spider/crawl for missed or hidden content
- [ ] Check for files that expose content, such as robots.txt, sitemap.xml, .DS_Store
- [ ] Check the caches of major search engines for publicly accessible sites
- [ ] Check for differences in content based on User Agent (eg, Mobile sites, access as a Search engine Crawler)
- [ ] Perform Web Application Fingerprinting
- [ ] Identify technologies used
- [ ] Identify user roles
- [ ] Identify application entry points
- [ ] Identify client-side code
- [ ] Identify multiple versions/channels (e.g. web, mobile web, mobile app, web services)
- [ ] Identify co-hosted and related applications
- [ ] Identify all hostnames and ports
- [ ] Identify third-party hosted content


### <a name="Configuration">Configuration Management</a>

- [ ] Check for commonly used application and administrative URLs
- [ ] Check for old, backup and unreferenced files
- [ ] Check HTTP methods supported and Cross Site Tracing (XST)
- [ ] Test file extensions handling
- [ ] Test for security HTTP headers (e.g. CSP, X-Frame-Options, HSTS)
- [ ] Test for policies (e.g. Flash, Silverlight, robots)
- [ ] Test for non-production data in live environment, and vice-versa
- [ ] Check for sensitive data in client-side code (e.g. API keys, credentials)


### <a name="Transmission">Secure Transmission</a>

- [ ] Check SSL Version, Algorithms, Key length
- [ ] Check for Digital Certificate Validity (Duration, Signature and CN)
- [ ] Check credentials only delivered over HTTPS
- [ ] Check that the login form is delivered over HTTPS
- [ ] Check session tokens only delivered over HTTPS
- [ ] Check if HTTP Strict Transport Security (HSTS) in use



### <a name="Authentication">Authentication</a>
- [ ] [Test for user enumeration](#test-for-user-enumeration)
- [ ] Test for authentication bypass
- [ ] Test for bruteforce protection
- [ ] Test password quality rules
- [ ] Test remember me functionality
- [ ] Test for autocomplete on password forms/input
- [ ] Test password reset and/or recovery
- [ ] Test password change process
- [ ] Test CAPTCHA
- [ ] Test multi factor authentication
- [ ] Test for logout functionality presence
- [ ] Test for cache management on HTTP (eg Pragma, Expires, Max-age)
- [ ] Test for default logins
- [ ] Test for user-accessible authentication history
- [ ] Test for out-of channel notification of account lockouts and successful password changes
- [ ] Test for consistent authentication across applications with shared authentication schema / SSO
- [ ] Inputs from ChatGPT :- 
  - Test for SQL injection vulnerabilities in login pages.
  - [Test for cross-site scripting (XSS) vulnerabilities in login pages](#xss-vulnerabilities-in-login-pages) 
  - Test for directory traversal vulnerabilities in login pages.
  - Test for brute force attacks on login pages.
  - Test for weak password policies or controls in the login process.
  - Test for session fixation vulnerabilities in the login process.
  - Test for authentication bypass through session hijacking.
  - Test for authentication bypass through cookie manipulation.
  - Test for authentication bypass through CSRF attacks.
  - Test for authentication bypass through parameter tampering.
  - Test for authentication bypass through hidden form fields.
  - Test for authentication bypass through HTTP method tampering.
  - Test for authentication bypass through URL parameter tampering.
  - Test for authentication bypass through parameter pollution.
  - Test for authentication bypass through HTTP request smuggling.
  - Test for authentication bypass through server-side request forgery (SSRF) attacks.
  - Test for authentication bypass through XML external entity (XXE) attacks.
  - Test for authentication bypass through business logic vulnerabilities.
  - Test for authentication bypass through broken access controls.
  - Test for authentication bypass through insecure session management.




### <a name="Session">Session Management</a>
- [ ] Establish how session management is handled in the application (eg, tokens in cookies, token in URL)
- [ ] Check session tokens for cookie flags (httpOnly and secure)
- [ ] Check session cookie scope (path and domain)
- [ ] Check session cookie duration (expires and max-age)
- [ ] Check session termination after a maximum lifetime
- [ ] Check session termination after relative timeout
- [ ] Check session termination after logout
- [ ] Test to see if users can have multiple simultaneous sessions
- [ ] Test session cookies for randomness
- [ ] Confirm that new session tokens are issued on login, role change and logout
- [ ] Test for consistent session management across applications with shared session management
- [ ] Test for session puzzling
- [ ] Test for CSRF and clickjacking



### <a name="Authorization">Authorization</a>
- [ ] Test for path traversal
- [ ] Test for bypassing authorization schema
- [ ] Test for vertical Access control problems (a.k.a. Privilege Escalation)
- [ ] Test for horizontal Access control problems (between two users at the same privilege level)
- [ ] Test for missing authorization


### <a name="Validation">Data Validation</a>
- [ ] Test for Reflected Cross Site Scripting
- [ ] Test for Stored Cross Site Scripting
- [ ] Test for DOM based Cross Site Scripting
- [ ] Test for Cross Site Flashing
- [ ] Test for HTML Injection
- [ ] Test for SQL Injection
- [ ] Test for SOQL Injection
- [ ] Test for LDAP Injection
- [ ] Test for ORM Injection
- [ ] Test for XML Injection
- [ ] Test for XXE Injection
- [ ] Test for SSI Injection
- [ ] Test for XPath Injection
- [ ] Test for XQuery Injection
- [ ] Test for IMAP/SMTP Injection
- [ ] Test for Code Injection
- [ ] Test for Expression Language Injection
- [ ] Test for Command Injection
- [ ] Test for Overflow (Stack, Heap and Integer)
- [ ] Test for Format String
- [ ] Test for incubated vulnerabilities
- [ ] Test for HTTP Splitting/Smuggling
- [ ] Test for HTTP Verb Tampering
- [ ] Test for Open Redirection
- [ ] Test for Local File Inclusion
- [ ] Test for Remote File Inclusion
- [ ] Compare client-side and server-side validation rules
- [ ] Test for NoSQL injection
- [ ] Test for HTTP parameter pollution
- [ ] Test for auto-binding
- [ ] Test for Mass Assignment
- [ ] Test for NULL/Invalid Session Cookie

### <a name="Denial">Denial of Service</a>
- [ ] Test for anti-automation
- [ ] Test for account lockout
- [ ] Test for HTTP protocol DoS
- [ ] Test for SQL wildcard DoS


### <a name="Business">Business Logic</a>
- [ ] Test for feature misuse
- [ ] Test for lack of non-repudiation
- [ ] Test for trust relationships
- [ ] Test for integrity of data
- [ ] Test segregation of duties


### <a name="Cryptography">Cryptography</a>
- [ ] Check if data which should be encrypted is not
- [ ] Check for wrong algorithms usage depending on context
- [ ] Check for weak algorithms usage
- [ ] Check for proper use of salting
- [ ] Check for randomness functions


### <a name="File">Risky Functionality - File Uploads</a>
- [ ] Test that acceptable file types are whitelisted
- [ ] Test that file size limits, upload frequency and total file counts are defined and are enforced
- [ ] Test that file contents match the defined file type
- [ ] Test that all file uploads have Anti-Virus scanning in-place.
- [ ] Test that unsafe filenames are sanitised
- [ ] Test that uploaded files are not directly accessible within the web root
- [ ] Test that uploaded files are not served on the same hostname/port
- [ ] Test that files and other media are integrated with the authentication and authorisation schemas


### <a name="Card">Risky Functionality - Card Payment</a>
- [ ] Test for known vulnerabilities and configuration issues on Web Server and Web Application
- [ ] Test for default or guessable password
- [ ] Test for non-production data in live environment, and vice-versa
- [ ] Test for Injection vulnerabilities
- [ ] Test for Buffer Overflows
- [ ] Test for Insecure Cryptographic Storage
- [ ] Test for Insufficient Transport Layer Protection
- [ ] Test for Improper Error Handling
- [ ] Test for all vulnerabilities with a CVSS v2 score > 4.0
- [ ] Test for Authentication and Authorization issues
- [ ] Test for CSRF


### <a name="HTML">HTML 5</a>
- [ ] Test Web Messaging
- [ ] Test for Web Storage SQL injection
- [ ] Check CORS implementation
- [ ] Check Offline Web Application

Source: [OWASP](https://www.owasp.org/index.php/Web_Application_Security_Testing_Cheat_Sheet)

# Content Specific 

### <a name="WAF Detection">WAF Detection</a>

- [ ] Send a request to the web application with a known attack pattern, such as SQL injection or cross-site scripting.
- [ ] Check the response from the web application. If the response is a WAF error message or an HTTP error code, such as 403 or 404, it is likely that a WAF is present.
- [ ] Analyze the response headers to see if any WAF-specific headers are present, such as "X-Mod-Security" or "X-WebApplication-Firewall".
- [ ] Check if the web application is using a third-party WAF service, such as Cloudflare or Akamai, by doing a DNS lookup on the website's domain name.
- [ ] Use a WAF detection tool: There are several tools available online that can detect the presence of a WAF on a web application. Examples of such tools include Wafw00f, WhatWeb, and WAFNinja.
- [ ] Analyze network traffic: You can use a network traffic analysis tool such as Wireshark to capture and analyze network traffic between your computer and the web application. Look for WAF-specific signatures or patterns in the network traffic, such as HTTP headers or response codes.
- [ ] Analyze response times: Some WAFs may introduce a delay in the response time of the web application. You can use tools like curl or wget to send requests to the web application and compare the response times for requests with known attack patterns and requests without known attack patterns.
- [ ] Analyze error messages: Some WAFs may generate specific error messages in response to certain types of attacks. Analyzing the error messages generated by the web application can give you clues as to whether a WAF is present and what type of WAF it may be.
- [ ] A sample python code to find the presence of AWS WAF on a targeted website

```python
import requests
url = "https://example.com/"
headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3'}
response = requests.get(url, headers=headers)
if 'x-amz-cf-id' in response.headers or 'x-amz-cf-pop' in response.headers:
    print("AWS WAF is present")
else:
    print("AWS WAF is not present")
```

- [ ] A sample python code to find the presence of AZURE WAF on a targeted website
```python
import requests
url = 'https://<your_web_app>.azurewebsites.net'
# Set the Azure WAF-specific header
headers = {'X-MS-Client-Application-Name': 'web'}
# Send a request to the web application
response = requests.get(url, headers=headers)
# Check the response for the presence of the Azure WAF-specific header
if 'X-MS-Azure-Ref' in response.headers:
    print('Azure WAF is present')
else:
    print('Azure WAF is not present')
```


### <a name="xss-vulnerabilities-in-login-pages">Test for cross-site scripting (XSS) vulnerabilities in login pages</a>

- [ ] Inject Script Code: You can attempt to inject script code into login page fields, such as the username or password field, and see if the script code is executed when the login page is loaded. You can use various payloads for this, such as `<script>alert('XSS');</script>` or `<img src=x onerror=alert('XSS')>`. If the script code is executed, then the login page is vulnerable to XSS.
- [ ] Test Input Validation: You can attempt to input malicious script code into login page fields and see if the web application filters or sanitizes the input. If the web application does not properly filter or sanitize the input, then it is vulnerable to XSS.
- [ ] Test Cookies: You can check if the login page sets any cookies that could be vulnerable to XSS. This can be done by manipulating the cookie values and seeing if any script code is executed when the login page is loaded.
- [ ] Test Hidden Fields: You can check if the login page contains any hidden fields that could be vulnerable to XSS. This can be done by attempting to inject script code into the hidden fields and seeing if the script code is executed when the login page is loaded.
- [ ] Test for DOM-based XSS vulnerabilities by manipulating the DOM elements on the login page.
- [ ] Test for reflected XSS vulnerabilities by injecting script code into the URL parameters used by the login page.
- [ ] Test for stored XSS vulnerabilities by injecting script code into input fields that are stored in the web application's database.
- [ ] Test for XSS vulnerabilities in error messages or validation messages displayed on the login page.
- [ ] Test for XSS vulnerabilities in any JavaScript code used by the login page.
- [ ] Test for XSS vulnerabilities in the HTTP response headers returned by the login page.
- [ ] Test for XSS vulnerabilities in the HTML comments on the login page.
- [ ] Test for XSS vulnerabilities in the HTML attributes used by the login page.
- [ ] Test for XSS vulnerabilities in the CSS styles used by the login page.
- [ ] Test for XSS vulnerabilities in any third-party JavaScript libraries or plugins used by the login page.
- [ ] Test for XSS vulnerabilities in the web application's session management system.
- [ ] Test for XSS vulnerabilities in the web application's error handling and logging systems.
- [ ] Test for XSS vulnerabilities in the web application's user input validation and sanitization routines.
- [ ] Test for XSS vulnerabilities in any user-generated content that is displayed on the login page, such as comments or reviews.
- [ ] Test for XSS vulnerabilities in any search or filter functionality on the login page.
- [ ] Test for XSS vulnerabilities in any redirection or forwarding functionality on the login page.
- [ ] Test for XSS vulnerabilities in any AJAX or dynamic content loading functionality on the login page.
- [ ] Test for XSS vulnerabilities in any internationalization or localization functionality on the login page.
- [ ] Test for XSS vulnerabilities in any browser-specific features or APIs used by the login page.
- [ ] Test for XSS vulnerabilities in any mobile-specific features or APIs used by the login page.



### <a name="Test for user enumeration">Test for user enumeration</a>

- [ ] Verify if the error messages shown on the login page differ for valid and invalid usernames. If the error message is different for a valid username and an invalid username, an attacker can determine whether a username is valid or not. You can use tools like Burp Suite to automate this process.
- [ ] Attempt to login with common usernames and observe whether the response differs for valid and invalid usernames. You can use a tool like Burp Suite or a web browser's developer console to capture the login requests and responses.
- [ ] Check whether the password reset feature reveals whether a username is valid or not. If the password reset feature reveals that a username is valid, an attacker can use this information to launch a brute force attack on the login page.
- [ ] Check the user registration page for any indication of valid usernames, such as a list of already registered users or a search feature that allows users to search for other users.
- [ ] Use a tool like DirBuster or DirSearch to brute-force the web application's directories and files. This can reveal hidden login pages and directories that are not visible to normal users.
- [ ] Check for differences in the response times for valid and invalid usernames. If the response time is different, it could indicate that the web application is performing an additional check to determine whether the username is valid or not.
- [ ] Attempt to login with a large number of randomly generated usernames to see if there is a pattern in the response. If there is a pattern, it could indicate that the web application is vulnerable to user enumeration attacks.
- [ ] Check the HTML source code of the login page for any hidden fields that may reveal information about valid usernames or user IDs.
- [ ] Analyze the network traffic using a tool like Wireshark to look for any patterns or differences in the response for valid and invalid usernames.
