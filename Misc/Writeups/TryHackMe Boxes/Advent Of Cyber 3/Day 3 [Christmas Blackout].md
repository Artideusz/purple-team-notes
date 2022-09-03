# Christmas Blackout

## Challenge
In this challenge, we learn about content discovery and directory brute-forcing using dirbuster.

## Solutions

### 1. What is the name of the folder of the admin dashboard?
I used gobuster to discover folders, and it showed me 2 folders, "admin" and "assets".

### 2. What is the password for "administrator"
There are 2 solutions for this. First, we can just guess that the password for administrator is "administator", the second method I used is inspecting the page and looking at "login-page.js", where we can see an if statement with the password check needed to access the page.

### 3. What is the value of the flag inside the admin panel?
The flag is shown on the screen when logging in.