# Go Buster

## 01 - Overview

Go buster is a tool that will search for available pages on a website.
It will take a word list and iterate through this wordlist to find (hidden) pages

## 02 - Command

-u "set the website to be scanned"
-w "set the word list to be used"

Example:

`gobuster -u http://fakelink.html -w wordlist.txt dir`
