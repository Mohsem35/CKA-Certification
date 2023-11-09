### VI editor

vim is improved version of vi (built in text editor in linux)
The VI EDITOR has three operation modes.

1. **Command Mode**: When the vi editor opens a file, it always goes to the COMMAND MODE first. In this mode, the editor only understands the commands

2. **Insert Mode**: This mode allows you to write text into the file
3. **Last Line Mode**: Pressing the **`:`** key will take you to the LAST LINE MODE



Command mode we can run these:

- `dd` = delete entire line
- `d10d` = delete next 10 lines
- `u` = undo
- `r` = redo
- `yy` = copy a line
- `p` = paste
- `x` = delete a letter
- `A` = jump to end of line and switch to insert mode
- `0`(zero) = jump to beginning of the line 
- `$` = jump to both end and start of the line
- `12G` = go to line 12
- `n` = jump to next match
- `N` =jump to previous match
- `:%s/<old_name>/<new_name>` = replace old with new throughout the file
- `q!` = quit vim without saving the changes


