---
layout: default
title: A guide to your digital life
---


#A Better Vim Cheat Sheet


Note: If you’re decent at vim and want your mind blown, check out [Advanced Vim](/advanced.html).

My introduction to vim was harsh. I kept finding it both overwhelming and not useful. So I’ve compiled a list of *essential* commands that you will use every day using vim. I then give a few instructions to making vim as great as it should be, because it’s pretty terrible without configuration.

##Cursor movement (Inside command mode)

* todo image
* `w` - jump by start of words (punctuation considered words)
* `W` - jump by words (spaces separate words)
* `e` - jump to end of words (punctuation considered words)
* `E` - jump to end of words (no punctuation)
* `b` - jump backward by words (punctuation considered words)
* `B` - jump backward by words (no punctuation)
* `0` - (zero) start of line
* `^` - first non-blank character of line (same as 0w)
* `$` - end of line
* Advanced (in order of what I find useful)
    * `Ctrl+d` - move down half a page
    * `Ctrl+u` - move up half a page
    * `}` - go forward by paragraph (the next blank line)
    * `{` - go backward by paragraph (the next blank line)
    * `gg` - go to the top of the page
    * `G` - go the bottom of the page
    * `: [num] [enter]` - Go To that line in the document
    * Searching
        * `f [char]` - Move to the next char on the current line after the cursor
        * `F [char]` - Move to the next char on the current line before the cursor
        * `t [char]` - Move to before the next char on the current line after the cursor
        * `T [char]` - Move to before the next char on the current line before the cursor
        * All these commands can be followed by `;` (semicolon) to go to the next searched item, and `,` (comma) to go the the previous searched item

##Insert/Appending/Editing Text
* Results in insert mode
    * `i` - start insert mode at cursor
    * `I` - insert at the beginning of the line
    * `a` - append after the cursor
    * `A` - append at the end of the line
    * `o` - open (append) blank line below current line (no need to press return)
    * `O` - open blank line above current line
    * `cc` - change (replace) an entire line
    * `c  [movement command]` - change (replace) from the cursor to the move-to point.
    * ex. `ce` changes from the cursor to the end of the cursor word
* Esc - exit insert mode
* `r  [char]` - replace a single character with the specified char (does not use insert mode)
* `d` - delete
    * `d` - [movement command] deletes from the cursor to the move-to point.
    * ex. `de` deletes from the cursor to the end of the current word
* `dd` - delete the current line
* Advanced
    * `J` - join line below to the current one
    * `s` - delete character at cursor and substitute text
    * `S` - delete line at cursor and substitute text (same as cc)

##Marking text (visual mode)
* `v` - start visual mode, mark lines, then do command (such as y-yank)
* `V` - start Linewise visual mode
* `Ctrl+v` - start visual block mode
* Navigation is just like in normal mode
* `Esc` - exit visual mode
* Advanced
    * `O` - move to Other corner of block
    * `o` - move to other end of marked area

##Visual commands
* `y` - yank (copy) marked text
* `d` - delete marked text
* `c` - delete the marked text and go into insert mode (like c does above)

##Cut and Paste
* `yy` - yank (copy) a line
* `p` - put (paste) the clipboard after cursor
* `P` - put (paste) before cursor
* `dd` - delete (cut) a line
* `x` - delete (cut) current character
* `X` - delete previous character (like backspace)

##Exiting
* `:w` - write (save) the file, but don't exit
* `:wq` - write (save) and quit
* `:q` - quit (fails if anything has changed)
* `:q!` - quit and throw away changes

##Search/Replace
* `/pattern` - search for pattern
* `?pattern` - search backward for pattern
* `n` - repeat search in same direction
* `N` - repeat search in opposite direction
* `:%s/old/new/g` - replace all old with new throughout file
* `:%s/old/new/gc` - replace all old with new throughout file with confirmations

##Working with multiple files
* `:e filename` - Edit a file in a new buffer
* `:tabe` - make a new tab
* `gt` - go to the next tab
* `gT` - go to the previous tab
* Advanced
    * `:vsp` - vertically split windows
    * `ctrl+ws` - Split windows horizontally
    * `ctrl+wv` - Split windows vertically
    * `ctrl+ww` - switch between windows
    * `ctrl+wq` - Quit a window

##Marks
* `m{a-z}` - Set mark {a-z} at cursor position 
* A capital mark {A-Z} sets a global mark and will work between files
* `‘{a-z}` - move the cursor to the start of the line where the mark was set
* `‘’` - go back to the previous jump location

##General
* `u` - undo
* `Ctrl+r` - redo
* `.` - repeat last command

#Making Vim actually useful
Vim is pretty painful out of the box. Global search is terrible, typeing `:w` for every file save is awkward, indenting code, etc is all worse than normal text editors. But a few changes and you’ll be much closer to the editor of your dreams.

##.vimrc
* [https://github.com/theicfire/my-vim/blob/master/.vimrc](https://github.com/theicfire/my-vim/blob/master/.vimrc)
    * Copy this to your home directory and restart vim. Read through it to see what you can now do (like  `[space]w` to save a file)
        * mac users - making a hidden normal file is suprisingly tricky. Here’s one way:
            * in the command line, go to the home directory
            * type `nano .vimrc`
            * paste in the contents of the .vimrc file
            * `ctrl+x`, `y`, `[enter]` to save
    * This is a minimal vimrc that focuses on three priorities:
        * adding options that are strictly better (like more information showing in autocomplete)
        * more convenient keystrokes (like  `[space]w` for write, instead of `:w [enter]`)
        * a similar workflow to normal text editors (like enabling the mouse)
    * You should now be able to press  `[space]w` in normal mode to save a file. And  `[space]p` to paste from outside of vim.
        * If you can’t paste, it’s probably because vim was not built with the system clipboard option. To check, run `vim --version` and see if `+clipboard` exists. If it says `-clipboard`, you will not be able to copy from outside of vim.
        * For mac users, homebrew does this right. Install homebrew and then run `brew install vim`.
            * then move the old vim binary: `$ mv /usr/bin/vim /usr/bin/vimold`
            * restart your terminal and you should see `vim --version` now with `+clipboard`

##Plugins
* The easiest way to make vim more powerful is to use Vintageous in sublime (version 3). This gives you Vim mode inside sublime, giving you the best of both world.
* Vintageous is great, but there’s a few things you have to do to tame it.
    * Get the [current version](https://bitbucket.org/guillermooo/vintageous/downloads/Vintageous.sublime-package) of Vintageous (I last used 3.5.1), and extract it to `~/.config/sublime-text-3/Packages/Vintageous` (or wherever the Packages directory is) (Do not use the package manager; we don’t want compiled python. We want to edit the python)
    * edit `Vintageous/Default.sublime-keymap`, and comment out (put `//` in front of lines) the references to certain useful shortcuts that you don’t want Vintageous to overwrite (first check each to see if it’s overwritten)
        * `ctrl+w` - close
        * `ctrl+s` - save
        * `tab` - indent
        * `ctrl+n` - new file
        * `ctrl+a` - select all
        * `ctrl+f` - find
            * `/` from vim also works, but it’s not as useful sometimes
    * Change the user settings (`User/Preferences.sublime-settings`) to include:
        * `"caret_style": "solid"`
        * This will make the cursor not blink, like in vim.
        * sublime might freeze when you do this. It’s a bug; just restart sublime after changing the file.
    * Change `Vintageous/Preferences.sublime-settings`, and change the following fields:
        * `"vintageous_use_ctrl_keys": true,`
        * `"vintageous_use_sys_clipboard": true,`
        * `"vintageous_reset_mode_when_switching_tabs": false,`
            * [https://github.com/guillermooo/Vintageous/pull/562/files](https://github.com/guillermooo/Vintageous/pull/562/files) will open new files in command mode, as expected
    * `ctrl+r` in vim means "redo". But there is a handy ctrl+r shortcut in sublime that gives an "outline" of a file. I remapped it to alt+r by putting this in the User keymap
        * `{ "keys": ["alt+r"], "command": "show_overlay", "args": {"overlay": "goto", "text": "@"} },`
    * [Add the ability to toggle vintageous on and off](https://github.com/guillermooo/Vintageous/wiki/Toggling-Vintageous)
    * Mac users: you will not have the ability to hold down a navigation key (like holding j to go down). To fix this, run the commands specified here: [https://gist.github.com/kconragan/2510186](https://gist.github.com/kconragan/2510186)
    * Get sublime to not copy when highlighting and pasting (pressing p) by doing the following:
        * go into `sublime-text-3/Packages/Vintageos/actions.py` and comment out the line (and wrapping if statement) that says `state.registers['"'] = prev_text`. I bet there’s a better way to do this, but this seems to work.
* recent work on Vintageous moved the file to xactions.py. It will probably change soon in the future. Here’s the diff of what I removed, so hopefully you can find the same diff in the future:

~~~~~~~
        --- a/xactions.py
        +++ b/xactions.py
        @@ -1542,9 +1542,9 @@ class _vi_big_p(ViTextCommandBase):
                     fragments = state.registers['"']
         
        -        if state.mode == modes.VISUAL:
        +        #if state.mode == modes.VISUAL:
                     # Populate registers with the text we're about to paste.
        -            state.registers['"'] = prev_text
        +            # state.registers['"'] = prev_text
         
                 sels = list(self.view.sel())
                 if len(sels) == len(fragments):
        @@ -1640,14 +1640,14 @@ class _vi_p(ViTextCommandBase):
                     print("Vintageous: Nothing in register \".")
                     return
         
        -        if state.mode == modes.VISUAL:
        -            # force register population. We have to do it here
        -            # vi_cmd_data = {
        -            #     "synthetize_new_line_at_eof": True,
        -            #     "yanks_linewise": False,
        -            # }
        -            prev_text = state.registers.get_selected_text(self)
        -            state.registers['"'] = prev_text
        +      #  if state.mode == modes.VISUAL:
        +      #      # force register population. We have to do it here
        +      #      # vi_cmd_data = {
        +      #      #     "synthetize_new_line_at_eof": True,
        +      #      #     "yanks_linewise": False,
        +      #      # }
        +      #      prev_text = state.registers.get_selected_text(self)
        +      #      state.registers['"'] = prev_text
~~~~~~~
 
* Now you should be able to restart sublime and have a great vim environment! Sweet Dude.

##Switch Caps Lock and Escape
* I highly recommend you switch the mapping of your caps lock and escape keys. You'll love it, promise! Switching the two keys is platform independent; google should get you the answer

##Other
I don’t personally use these yet, but I’ve heard other people do!

* `:wqa` - Write and quit all open tabs (thanks Brian Zick)

If you’ve got this under your belt, you should check out Advanced Vim!
