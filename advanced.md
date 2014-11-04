---
layout: default
title: Advanced Cheat Sheet
---

# Advanced Vim Cheat Sheet
The best possible resource on vim is the book [Practical Vim](http://www.amazon.com/Practical-Vim-Thought-Pragmatic-Programmers/dp/1934356980). I’ve outlined some of my personal favorites, and will also put in some plugins that might just change your world.

##General
* `[numer]Ctrl+a, [number]Ctrl+x` - add/subtract `[number]` to the number at or after the cursor
    * Ex. the line `.news { background-position: -0px 0px }`
    * Enter `180Ctrl+a`
    * The line becomes `.news { background-position: -180px 0px }`
* text objects
    * There’s a few well known coding characters that are special: `(`, `{`, and `[`. Text objects give you a way to select text inside these (and more)
    * in visual mode:
        * `a(` - a `()` block
        * `a{` - a `{}` block
        * `a[` - a `[]` block
        * `i(` - inner `()` block
        * `i{` - inner `{}` block
        * `i[` - inner `[]` block
* automatic marks
    * [http://vim.wikia.com/wiki/Using\_marks](http://vim.wikia.com/wiki/Using_marks) under "special marks"
* `%` - jumps between matching parens
* `ctrl+o` - go backwards in navigation history
* `ctrl+i` - go forwards in navigation history
* `Ctrl+r + 0` in insert mode inserts the last yanked text (or in command mode, etc.)
* `gv` - reselect (select last selected block of text)

##Regex and Searching
* There are 4 modes of searching, ranging from "very magic" to "very no magic". Check out the details by running `:h \\v`. Here’s how they will help:
    * Start a search with `\v` to get closer to pearl style regexes, where you *only have to escape* `/` and `\`. If you are searching backwards, you’ll also have to escape `?`.
    * Start a search with `\V` where you *only have to escape* `/` and `\`. `<`, `(`, etc. are not special anymore! This is called *very no magic* mode.
    * `\zs`, `\ze` - put `\zs` before where you want to match and `\ze` after where you want to match. I.e.
        * `/\vhttp:\v\/\/.*\.com` will search for domains of the form `http://<something>.com`
        * If we want to just match the something, which searching for the whole url, we could do:
            * `/\vhttp:\v\/\/\zs.*\ze\.com`
        * An extension to this is looking for the start of a word and then skipping it. For example looking for the variable `i`. We could then use this search: `\v\W\zsi\ze\W`. Note: Use `\W` matches everything except `[a-zA-Z0-9_]`
            * There’s a shortcut called word boundaries, with `<` and `>`:
                * `\v<i>`
                * [More info](http://vim.wikia.com/wiki/Search_patterns)
            * You can get this same behavior by pressing `*` in normal mode, while having your cursor over the word you want to search.
* Vim 7.4 has an [amazing "gn" command](http://vimcasts.org/episodes/operating-on-search-matches-using-gn/) that allows you to search and replace faster. `gn` means "select the next search term".

