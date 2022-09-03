# Search Engine Dorking

Search Engine Dorking is a very important information gathering technique consisting of advanced search operators included in a search engine. Search Engines sometimes hold information that cannot be found anywhere else, for example:

- Subdomains.
- Hidden services.
- Backup files.
- Directories that directory brute forcers would not find.
- And more juicy information!

## Google Dorking 

Here is a couple of google dork examples:

- `site:*.google.com -site:www.google.*` - Show subdomains of "google.com".
- `filetype:bak` - Show potential backup files.
- `intitle:"Index of /*"` - Show hidden files provided by the web server.

A useful cheetsheet is listed [here](https://gist.github.com/sundowndev/283efaddbcf896ab405488330d1bbc06) by sundowndev.

## Github dorking

Similar to Google dorks, but with different built-in operators and keywords. You first have to login to github in order to find specific strings in the source code of various projects. Examples of github dorks include:

- `repo:<owner>/<repository> extension:png` - Show all PNG files in a repository.

Github has documentation for the search operators and keywords [here](https://docs.github.com/en/search-github/searching-on-github). If you want to find an exact match in a repository, cloning the repository to the local machine and `git grep`'ing bypasses the problem.

## Search Engines for extracting information

- Google
- Bing
- Github
- Gitlab
- Bitbucket
- DuckDuckGo
- Yahoo
- Baidu
- Yandex
- Excite
- Aol
- Ask
- Shodan
- Censys
- VirusTotal
- Searx
- YaCy
- Ecosia
- Wayback
- Coding Search Engines
    - Codeseek
    - Krugle
    - Openhub.net

## Extra info

- [List of search engines](https://en.wikipedia.org/wiki/List_of_search_engines#Source_code)
- [Cipher387's Dorks Collection List](https://github.com/cipher387/Dorks-collections-list)