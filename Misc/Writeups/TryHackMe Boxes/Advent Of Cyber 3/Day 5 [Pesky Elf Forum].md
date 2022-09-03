# Pesky Elf Forum

## Challenge
This challenge showcases XSS, more specifically stored or persistant XSS, meaning that malicious javascript is stored in the database.

## Solutions 

### 1. What flag did you get when you disabled the plugin?
When logging into the "McSkidy" account, we are greeted with a list of posts, where we can create comments. When testing an application, it's always important to check if the user input is sanitized, we can do that using SQL injection or XSS attacks. I used the following payload: `<img style="display:none" src=x onerror="fetch('/settings?new_password=123')"/> Hello world!`. After 1 minute, I was able to login into the "grinch" account and retrieve the flag.