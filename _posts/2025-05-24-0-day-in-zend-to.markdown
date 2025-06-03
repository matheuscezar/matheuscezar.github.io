---
layout: post
title:  "How I found a Unauthenticated Remote Code Execution in Zend.To"
date:   2025-05-24
---
How I found a Unauthenticated Remote Code Execution in Zend.To?

During a penetration testing project, I encountered a target using the [Zend.To](http://zend.to/) file transfer software. Zend.To is an open-source solution developed since 2010, offering a range of features and integration options for secure file sharing.

Initially, I searched for any existing CVEs associated with Zend.To, but found nothing critical reported at the time. Given the lack of public vulnerabilities, I decided to conduct a deeper analysis.

During my research, I came across an unofficial code repository hosted at:

👉 https://github.com/ergosteur/zendto-localized

Although it’s not the official source, I chose to review the codebase for potential security issues.

While analyzing the code, I discovered that the file `NSSDropoff.php` contains two calls to the `exec()` function where file names are passed without proper sanitization (Screenshot-1).

![NSSDropoff-php.png](/assets/img/NSSDropoff-php.png)

These file names are stored in the `$jkffilelist` variable, which is populated based on the `tmp_name` parameter from file uploads — a value that can be manipulated by an attacker (Screenshot-2).

![Reverse-Shell-Request.png](/assets/img/Reverse-Shell-Request.png)

This creates a **command injection** vulnerability, allowing a malicious user to execute arbitrary system commands during the file upload process

![Reverse-Shell.png](/assets/img/Reverse-Shell.png)