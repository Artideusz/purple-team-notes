# AudioEdit

## Points: 160

## Solution
First, I attempted to send a mp3 file to the website, and was greeted by a page where I can modify the sent file via setting on the website. I noticed that there is a **Author** and **Title** field and assumed that means the metadata (ID3v2 headers) of the file since I cannot find any setting that would change those parameters on the website.

### The query parameter entrypoint
First, I noticed that the website uses a `?file` query parameter to show the content of the mp3 file. When providing a nonexistent filename, we get an error message saying it could not fetch the audio file from the database, which probably means that partial content or even the whole file is saved in the database.

### 1. Modify the ID3v2 header values
To do that, I created a nodejs script and used a npm library called `node-id3` to read and write ID3v2 headers. The script also uploads the modified mp3 file to the server and outputs the HTML page to the console.

### 2. Potential SQL Injection
First, I wanted to test if special characters were to crash something, and it did!. If we provide a `'` character to the `title` or `author` tag, we get an `Error inserting into database!` error. We can assume the database is a SQL-type database, since I know that this is a common sight when dealing with SQL Injection.

### 3. Figuring out the query
First, I assumed we are dealing with an `INSERT`, since the error says that. So I made an example query: `INSERT INTO something VALUES ('title', 'author');`. I have tried the following payload: `', '123');#` for `title`, but got an error. Then, I tried the same for `author` and it worked! It gave me an empty `author` and a `title` with a value `123`. So I think the query probably looks something like this: `INSERT into something VALUES ('author', 'title');`.

### 4. Extracting data
Since we can see the data displayed on the site, we can easily extract information using a couple of techniques. The first one is using a subquery to extract information. For example the version of the database, the current database the application is using, all the tables that it owns (I will not spoil the ways we can do that, a couple of simple searches will get you what you need).

### 5. The "specified twice" problem
I got stuck for a while when attempting to extract information from the table while at the same time inserting the resulting value. If you were to try it on your own MySQL instance you would notice that there is an error (A little hint). After solving this problem, I extracted the first row of the table, accessed the file by directly requesting the filename I extracted and got the flag.