---
layout: post
title: Code Searching Keyboard Shortcuts
---

Developers should be flexible and creative. Being able to work on any editor is an advantage for a developer who works with other developers on various stacks. When things go well, touch typing on a foreign keyboard in a new interface can look like below.

![Amadeus Touch Typing](https://i.imgur.com/60G7oBj.gif "Amadeus Touch Typing")

I often find myself finding files by names and searching for code snippets by strings throughout a folder structure. IDEs do this and more when everything is set up properly. Unfortunately, sometimes it takes longer to setup the project than to do basic text based operations. The following is a table of some common operations in a code base for various code bases.

| Editor |  Find File | Find Text |
| --- | --- | --- |
| Atom (Linux/Windows) | ctrl-p/ctrl-t  | ctrl-shift-f |
| Atom (Mac) | cmd-p/cmd-t  | cmd-shift-F |
| Bash | `find . -iname filename` | `grep -R text .` |
| Bash and Git | `git ls-files | grep filename` | `git grep text` |
| Clion | ctrl-shift-N | Double Shift |
| Sublime | ctrl-p  | ctrl-shift-F |
| Vim | `:vs **/*filename` | `:grep -R text .` |
| Visual Studio Code | ctrl-p | ctrl-shift-F |

I always forget the order of find's arguments and end up doing `find . | grep -i filename`. Also, for finding system files on Unix like systems, mlocate works well. To use it once installed, run `sudo updatedb` to populate the database. Then, `locate filename` searches the whole system quickly.
