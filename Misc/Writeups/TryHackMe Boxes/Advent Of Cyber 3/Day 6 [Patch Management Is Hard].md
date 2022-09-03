# Patch Management Is Hard

## Challenge
This challenge showcases a pretty popular vulnerability called "Local File Inclusion", it helps the atttacker in content discovery, where you can:
- View PHP source code of a page (php://filter wrapper)
- RCE (executing PHP code and using the system() function)

## Solutions 

### 1. What is the entry point for our web application?
When entering the challenge website, we can see that we are automatically greeted with an error saying that "We can not access this page! You need to login". In the URL, we notice that we have an interesting query parameter called "err", with a value of what looks like a file, "error.txt". The entry point for our app is "err", since we will be testing it first.

### 2. Perform LFI to read the /etc/flag file. What is the flag?
To read the file, we can simply input `/etc/flag` into the `err` query parameter. This way, we get the flag.

### 3. Use the PHP filter technique to read the source code of the `index.php` file. What is the `$flag` variable's value?
PHP filter is a very nice technique we can use to get the source code without actually running the php code. This is how usually php filter looks like in read teaming operations:
```
https://www.example.com/index.php?err=php://filter/convert.base64-encode/resource=<file>

Where <file> is a PHP page we want to see the source code for.
```

You can also use `php://filter/string.rot13/resource=<file>`, but bear in mind that there WILL be an attempt to parse the content by the browser, so you will see weird tags in the HTML code of the page, so use it if the base64-encode option does not work.
After using the `php://filter` technique, with base64, we can get the base64 encoded version of the source code of the page, decode it in cyberchef, and find the `$flag` variable.

### 4. What is the username and password of McSkidy's account?
If we look closely at the source code of the PHP page we just extracted, we notice that there is an `if` statement that checks if the `$_SESSION['username']` variable is equal `$USER`, but we do not see the variable in the source code declared. We do however, see that there is an `include` function, that is responsible for "copying" and "pasting" source code from a different file. If we check the file using the same technique we used earlier, we can extract credentials needeed to log into the website.

### 5. What is the password of the `flag.thm.aoc` server?
There are 2 solutions for this question. First, since we obtained credentials from `./includes/creds.php`, we can now login to the panel. We notice a link to `Password Recovery`, if we click it, we get the answer to the question.

The second, in case we did not find the password, we can use Remote Code Execution to get the password. In the `err` query parameter, we can view pages, but we can also generate pages with source code. I will use the `data` URI scheme to achieve this. If we do: `https://www.example.com/index.php?err=data:text/plain,<?php system("ls") ?>`, we get a list of directories. We notice a directory called "sensitive-data", if we go into that directory, we see a "recovery.json" file. Using the `cat` command, we get a json object with passwords, and one of them is the password for `flag.thm.aoc`.

### 6. What is the hostname of the webserver? The log location is at `./includes/logs/app_access.log`.
There are also many solutions for this, we don't even need the log file. But if the challenge says that we should use it, here's how we do it.
In the panel, we notice a link to "log access". If we go inside it, we see that it outputs a user-agent and the path that was requested. We can test for RCE by sending a request with a modified user-agent. We can easily achieve this using `curl` or `wget`: `curl -A "<?php echo test; ?>" https://www.example.com/index.php`. After sending this request, we simply LFI the log file and execute PHP code that the log file contains. To get the hostname, we can use `system("hostname")` or `phpinfo()` inside a `<?php?>` tag inside the user-agent.

Another method that we can use to extract the hostname of the server is using the `data` URI scheme again and using the payloads from the previous method.

One more method is using `PHP Session` content to inject PHP code. PHP, when initiating a new session, saves variables inside a file, usually the location is `/tmp`. The file name should be by default `sess_<phpsessid>`, where `phpsessid` is our `PHPSESSID` cookie value. If we get the file using LFI: `/tmp/sess_<id>`, we can inspect what data is stored in it. In this case, the username is stored. We can exploit this by logging in using the following username: `<?php system("hostname"); ?>` and a random password, does not matter. After getting an invalid username, we just include the `/tmp/sess_<id>` file that was generated and we get the hostname.