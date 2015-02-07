---
layout: default
title: Advanced Cheat Sheet
---

# Advanced Vim Cheat Sheet
The best possible resource on vim is the book [Practical Vim](http://www.amazon.com/Practical-Vim-Thought-Pragmatic-Programmers/dp/1934356980). I’ve outlined some of my personal favorites, and will also put in some plugins that might just change your world.

##General

* Text Objects
	* Say you have `def (arg1, arg2, arg3)`, where your cursor is somewhere in the middle of the parenthesis.
    * `di(` deletes everything between the parenthesis. That says "change everything inside the nearest parenthesis". Without text objects, you would need to do `T(dt)`.
	* Learn more [here](http://blog.carbonfive.com/2011/10/17/vim-text-objects-the-definitive-guide/)
* automatic marks
    * [http://vim.wikia.com/wiki/Using\_marks](http://vim.wikia.com/wiki/Using_marks) under "special marks"
* `%` - jumps between matching `()` or `{}`
* [jump list](http://vim.wikia.com/wiki/Jumping_to_previously_visited_locations)
	* `ctrl+o` - go backwards in the jump list
	* `ctrl+i` - go forwards in the jump list
* [change list](http://vim.wikia.com/wiki/Jumping_to_previously_visited_locations)
	* `g;` - go backwards in the change list
    * `g,` - go forwards in the change list
* `Ctrl+r + 0` in insert mode inserts the last yanked text (or in command mode)
* `gv` - reselect (select last selected block of text, from visual mode)

## Plugins
The most helpful part of plugins is that they make vim a better fully featured IDE. Concepts like global search and finding a file to open are solved with plugins. They also can bring out some missing features of vim, like automatic commenting.

### Essential Plugins
* *vim-pathogen*: Either this or Vundle are good ways to manage your plugins.
* *vim-sensible*: tiny set of reasonable defaults that every vim user should have. This allows you to keep your .vimrc a bit smaller.
* *ag.vim*: Fantastically fast global searches.
* *ctrlp.vim*: Open up any file or buffer.
* *nerdcommenter*: Allows you to comment some block of code

### Awesome Plugins
* indentLine: Shows you visibly your tabs or spaces, like sublime
* neocomplete.vim (or you complete me): Autocomplete
* nerdtree: File browser
* vim-surround: Quicker way to add or delete some characters *around* something
* tagbar: Similar to the "outline" feature of many IDE's

##Regex and Searching
* Searching in vim is very unintuitive. There are 4 modes of searching, ranging from "very magic" to "very no magic". They determine what needs to be escaped with a `\` in your search term. If you just want "regex on" or "regex off" then always search in Very Magic or Very No Magic mode.
* Check out the details by running `:h \\v`.

### Pearl style regexes (Very Magic Mode)

Start a search with `\v`. Everything else can act like a normal regex search, except you have to escape `/` and `\`. If you are searching backwards, you’ll also have to escape `?`.

* E.g. search for a url starting with `www` and ending with `.com`: `\vwww\..*\.com`

#### Differences from Pearl Regex
The `<` and `>` characters are special for start and end of word. Escape them to search for them literally.

###Literal Search (Very No Magic Mode)
Start a search with `\V`. Now you *only have to escape* `/` and `\`. It would be nice if you didn't have to escape *anything*, but alas vim is not like this.

###Advanced: Match a subsection of your search
Let's say you have this file

```
www.yahoo.com
blah some other stuff
www.google.com
www.ebay.org
```
And you want to change it to

```
www.chase.com
blah some other stuff
www.chase.com
www.chase.org
```
What you're doing is saying *find all domain names and change the inner part to `chase`*. You can do this by specifying the part of the search to *match*.
Put `\zs` before where you want to match and `\ze` after where you want to match.
	
So in this case: `\v` `www\.` **\zs** `.*` **\ze** `\.com`

* An extension to this is looking for the start of a word without matching it. For example looking for the variable `i`. We could then use this search: `\v\W\zsi\ze\W`. Note: Use `\W` matches everything except `[a-zA-Z0-9_]`
	* There’s a shortcut called word boundaries, with `<` and `>`:
		* `\v<i>` ([More info](http://vim.wikia.com/wiki/Search_patterns))
        * You can get this same behavior by pressing `*` in normal mode, while having your cursor over the word you want to search.

### Faster Search and Replace
* Vim 7.4 has an [amazing "gn" command](http://vimcasts.org/episodes/operating-on-search-matches-using-gn/) that allows you to search and replace faster. `gn` means "select the next search term".


