---
title: OWASP Web Application Security Testing Checklist
date: 2023-03-28 14:30:00 +/-TTTT
categories: [Web Security, OWASP, Testing]
tags: [web-security, owasp, testing, checklist, security-testing, penetration-testing]     # TAG names should always be lowercase
---

# OWASP Web Application Security Testing Checklist

A comprehensive checklist for testing web application security based on OWASP guidelines.

## Table of Contents
{:.no_toc}

1. TOC
{:toc}

## Introduction

This checklist provides a systematic approach to testing web application security. It covers various aspects of security testing from information gathering to specific vulnerability testing.

## Information Gathering {#information-gathering}

### Initial Reconnaissance
- [ ] Manually explore the site
- [ ] Spider/crawl for missed or hidden content
- [ ] Check for files that expose content, such as:
  - `robots.txt`
  - `sitemap.xml`
  - `.DS_Store`
- [ ] Check the caches of major search engines for publicly accessible sites

### Technology Identification
- [ ] Perform Web Application Fingerprinting
- [ ] Identify technologies used
- [ ] Identify user roles
- [ ] Identify application entry points
- [ ] Identify client-side code
- [ ] Identify multiple versions/channels (e.g. web, mobile web, mobile app, web services)

### Infrastructure Analysis
- [ ] Identify co-hosted and related applications
- [ ] Identify all hostnames and ports
- [ ] Identify third-party hosted content

## Configuration Management {#configuration-management}

### Security Headers
- [ ] Test for security HTTP headers:
  ```http
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  X-XSS-Protection: 1; mode=block
  Content-Security-Policy: default-src 'self'
  ```

### File Management
- [ ] Check for commonly used application and administrative URLs
- [ ] Check for old, backup and unreferenced files
- [ ] Test file extensions handling
- [ ] Check for sensitive data in client-side code (e.g. API keys, credentials)

## Authentication Testing {#authentication}

### User Enumeration
- [ ] Test for user enumeration
- [ ] Test for authentication bypass
- [ ] Test for bruteforce protection
- [ ] Test password quality rules

### Session Management
- [ ] Test remember me functionality
- [ ] Test for autocomplete on password forms/input
- [ ] Test password reset and/or recovery
- [ ] Test password change process
- [ ] Test CAPTCHA
- [ ] Test multi factor authentication

## Data Validation {#data-validation}

### Injection Testing
- [ ] Test for SQL Injection
- [ ] Test for NoSQL Injection
- [ ] Test for LDAP Injection
- [ ] Test for Command Injection
- [ ] Test for XML Injection

### XSS Testing
- [ ] Test for Reflected XSS
- [ ] Test for Stored XSS
- [ ] Test for DOM-based XSS
- [ ] Test for Cross Site Flashing

## Risky Functionality {#risky-functionality}

### File Uploads
- [ ] Test file type whitelisting
- [ ] Test file size limits
- [ ] Test file content validation
- [ ] Test Anti-Virus scanning
- [ ] Test filename sanitization

### Payment Processing
- [ ] Test for known vulnerabilities
- [ ] Test for default credentials
- [ ] Test for injection vulnerabilities
- [ ] Test for buffer overflows
- [ ] Test for cryptographic storage

## Conclusion

This checklist provides a foundation for comprehensive web application security testing. Regular testing using this checklist can help identify and mitigate security vulnerabilities before they can be exploited.

## References

- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [OWASP Testing Checklist](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)

## License

This work is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
