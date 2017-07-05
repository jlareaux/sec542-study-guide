
SEC542 Notes
===
A study guide for SEC542: Web App Penetration Testing and Ethical Hacking. Visit
the [SEC542][01] webpage For additional information.

### Study Guide Menu
- [Appendix](appendix) of concepts, methods and tools in the SEC542 course.
    - [Concepts](appendix#concepts)
    - [Methods](appendix#methods)
    - [Tools](appendix#tools)
    - [Snippets](appendix#snippets)
- [Cheatsheets](cheatsheets) for quick reference.
- [Glossary](glossary) of terms in the SEC542 course.
- [Index](index) of references to terms in the SEC542 course.
- [Labs](Labs) in the SEC542 course, abridged versions.
    - [542.1 Introduction and Information Gathering](#5421-introduction-and-information-gathering)
        - [Why the Web](#why-the-web)
        - [Understanding the Web](#understanding-the-web)
        - [Course Logistics](#course-logistics)
        - [Web App Pen Tester's Toolkit](#web-app-pen-testers-toolkit)
        - [Interception Proxies](#interception-proxies)
        - [WHOIS and DNS](#whois-and-dns)
        - [Exercise: DNS Harvesting](#exercise-dns-harvesting)
        - [Open Source Information](#open-source-information)
        - [HTTP Protocol](#http-protocol)
        - [HTTP Methods](#http-methods)
        - [HTTP Status Codes](#http-status-codes)
        - [WebSocket](#websocket)
        - [Exercise: Examining HTTP Requests and Responses](#exercise-examining-http-requests-and-responses)
        - [HTTPS](#https)
        - [Testing for Weak Ciphers](#testing-for-weak-ciphers)
        - [Exercise: Testing HTTPS](#exercise-testing-https)
        - [Heartbleed](#heartbleed)
        - [Exercise: Exploiting Heartbleed](#exercise-exploiting-heartbleed)
        - [Demo: Burp Suite Introduction](#demo-burp-suite-introduction)
    - [542.2 Configuration, Identity and Authentication Testing](#5422-configuration-identity-and-authentication-testing)
        - [Scanning with Nmap](#scanning-with-nmap)
        - [Exercise: Gathering Server Info](#exercise-gathering-server-info)
        - [Testing Software Configuration](#testing-software-configuration)
        - [Shellshock](#shellshock)
        - [Exercise: Shellshock](#exercise-shellshock)
        - [Spidering Web Applications](#spidering-web-applications)
        - [Exercise: Spidering](#exercise-spidering)
        - [Analyzing Spidering Results](#analyzing-spidering-results)
        - [Exercise: ZAP Forced Browse](#exercise-zap-forced-browse)
        - [Fuzzing](#fuzzing)
        - [Exercise: Burp Fuzzing](#exercise-burp-fuzzing)
        - [Information Leakage](#information-leakage)
        - [Exercise: Directory Browsing](#exercise-directory-browsing)
        - [Authentication](#authentication)
        - [Exercise: Authentication](#exercise-authentication)
        - [Username Harvesting](#username-harvesting)
        - [Exercise: Username Harvesting](#exercise-username-harvesting)
    - [542.3 Injection](#5423-injection)
        - [Session Tracking](#session-tracking)
        - [Session Fixation](#session-fixation)
        - [Bypass Flaws](#bypass-flaws)
        - [Exercise: Authentication Bypass](#exercise-authentication-bypass)
        - [Vulnerable Web Apps: Mutillidae](#vulnerable-web-apps-mutillidae)
        - [Command Injection](#command-injection)
        - [Exercise: Command Injection](#exercise-command-injection)
        - [File Inclusion and Directory Traversal](#file-inclusion-and-directory-traversal)
        - [Exercise: Local/Remote File Inclusion](#exercise-localremote-file-inclusion)
        - [SQL Injection Primer](#sql-injection-primer)
        - [Discovering SQLi](#discovering-sqli)
        - [Exercise: Error-Based SQLi](#exercise-error-based-sqli)
        - [Exploiting SQLi](#exploiting-sqli)
        - [SQLi Tools](#sqli-tools)
        - [Exercise: sqlmap + ZAP](#exercise-sqlmap--zap)
    - [542.4 JavaScript and XSS](#5424-javascript-and-xss)
        - [JavaScript](#javascript)
        - [Document Object Model \(DOM\)](#document-object-model-dom)
        - [Exercise: JavaScript](#exercise-javascript)
        - [Cross-Site Scripting](#cross-site-scripting)
        - [Exercise: Reflective XSS](#exercise-reflective-xss)
        - [XSS Tools](#xss-tools)
        - [XSS Fuzzing](#xss-fuzzing)
        - [Exercise: HTML Injection](#exercise-html-injection)
        - [XSS Exploitation](#xss-exploitation)
        - [BeEf](#beef)
        - [Exercise: BeEf](#exercise-beef)
        - [AJAX](#ajax)
        - [API Attacks](#api-attacks)
        - [Data Attacks](#data-attacks)
        - [Exercise: AJAX XSS](#exercise-ajax-xss)
    - [542.5 CSRF, Logic Flaws and Advanced Tools](#5425-csrf-logic-flaws-and-advanced-tools)
- [Outline](outline) of the SEC542 course.
    - [542.1 Introduction and Information Gathering](outline#5421-introduction-and-information-gathering)
    - [542.2 Configuration, Identity and Authentication Testing](outline#5422-configuration-identity-and-authentication-testing)
    - [542.3 Injection](outline#5423-injection)
    - [542.4 JavaScript and XSS](outline#5424-javascript-and-xss)
    - [542.5 CSRF, Logic Flaws and Advanced Tools](outline#5425-csrf-logic-flaws-and-advanced-tools)
- [Cheatsheets](cheatsheets) for quick reference of the SEC542 course.
    - [Foo](cheatsheets#foo)
    - [Bar](cheatsheets#bar)

---

Study Guide: <!--[Home](home), -->[Outline](outline), [Index](index), [Labs](labs), [Glossary](glossary), [Appendix](appendix) or [Cheatsheets](cheatsheets).

<!-- Referenced External Links -->

[01]: https://www.sans.org/course/web-app-penetration-testing-ethical-hacking (SEC542: Web App Penetration Testing and Ethical Hacking)
