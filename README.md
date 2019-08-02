## Learning Basics of Bash, Vim & Regex

## 1.1 Bash

---

### 1.1.1 File Management

###### Copy File

`cp <source> <destination>`

source and destination can be directories.

Recursively:
`cp -R <source> <destination>`

###### Move command

mv command is used to rename and overwrite files in directories.
To rename a file, set the first argument to the original file name and
second argument to the new file name or destination directory.

You can also rename directories.

`mv <source> <destination>`

###### Wild cards

    cat *.txt ===> cat file1.txt file2.txt ....
    cat b{ee,oo}p.txt ===> cat beep.txt book.txt
    mv b{ee,oo}p.txt ===> mv beep.txt boop.txt

###### Delete file:

`rm <file-name>`

Delete directory
`rm -r <dir-name>`

###### Absolute and relative paths

`.` and `..` are relative paths.

paths starting with `/` are absolute paths

###### Writing and reading file

`echo hello > <file-name> (saves the text hello in file name)`

Append to a file:
`echo world >> <file-name>`

### 1.1.2 Bash Scripts

whenever you find yourself typing same sequence of commands several times, consider making a script. Just the put the commands you would normally type into a file and add `#!/bin/bash` to the top of the file.

**_Naming_** <br/>
You can name your bash script without any extension. **e.g** If I want to make a script named **logger**, I can just make a file called **_logger_** and put my commands in it.
Bash scripts can have an extension of **sh** but it's not necessary.
e.g **logger.sh**

###### Sample Script (move in directory "home" and delete file `cool.txt`)

```
#!bin/bash
cd home && rm cool.txt
```

**_Permissions_** <br/>
You can not run bash script directly after creating them, first you have to give the script the permission to `execute`

Give the `executable` permission to the bash file.
Usually setting the **_executable_** bit on the file does the trick.

Give the permission by following command.

```
chmod +x <bash-file-name>
```

**_Execute Script_** <br/>
To run the bash script you need to provide a relative path for the script in command line. e.g **./logger**

**You can use arguments in your script.**
What ever argument you pass it will be passed to you as environment variables as `$1`, `$2`, `$3` and so on.

For the following script `$1` argument will be passed when the user will execute the script.

_argument_script.sh_

```
#!/bin/bash
echo $1
```

Running the above script with `./argument_script hello` , **hello** will be the argument passed.
If argument is multiple words like `hello world` , you can use backticks around the argument. Or you can use **\$\*** for the argument.

**READ** command <br/>
if you want to ask the user for input, there is a command called `read`. This sets the result in the environment variable.

For the following script, running the script will prompt the user for the input. This input will be saved in the environment variable and will be used from the name given to the variable.

```
#!/bin/bash
read INPUT
echo $INPUT
```

Whatever input you'll give will be printed out to the terminal as a result of the above script. <br/><br/><br/>

### 1.1.3 Permissions

###### There are three kinds of permissons for files and directories:

1. User (Owner of the file) represented by `u`
2. Group (Group of the file) represented by `g`
3. Other (everyone else) represented by `o`

To check the permissions use `ls -l` command.

**how to read permissions**
`-rwxr-xr-x` -> First 3 bits for `u`, Middle three bits for `g`, last bits for `o`. The `-` means the permission is not available.
`rwx -> read write executable`

Set permissions with `chmod`
**_In octal_**
644 -> ugo

```
#4 for write
#2 to read
#1 to execute
```

`-rwxr-xr-x`
Each character describes a permission. (r)ead, (w)rite, and e(x)cute.
A '-' in place of one of those letters mean the permission is not available.

**_x is the executable bit_**
=> If set for directory, that means you can list the directory contents
=> If set for file, that means you can run it

**Example of setting a permission.**
If I want to add a (w)rite permission to the group.
`chmod g+w <file>`. (Grant the permission)
`chmod g-w <file>`. (Revoke the permission)

### 1.1.4 Exit Codes, Operators and Subshells

If you run a command and it's successfull, the exit code will be 0.

You can read out what the exit code it by `$?`

```
echo $?

```

###### If-else

```
if test -f cool.txt; then echo true; else echo false; fi
```

`test -f` runs the while loop silently

### 1.1.5 Job Control

Bash is built to handle multiple programs in parallel

###### Put process in background

- `ctrl + z` puts the process in background, when you want to resume that process type `fg`
- type `jobs` to see what jobs are running. If you type `fg` it'll put the last job in the foreground.
- If you want to specify the job, give it an argument like `fg %1` to specify job
- You can also use the command `kill` to send the signal to program to end like this `kill %1`. Sometimes this command won't end the program, because some programs don't accept end signal.
- End Forcefully - `kill -9 %1`
- `kill %%` kills last job
- All signals are like `SIG<name>`
- Another way to put program to background is to use `&:`

###### pgrep

Search for a process by it's name
`pgrep <name> -lf (additional info)`

**Kill by id**
`kill <id>`

<br/><br/><br/>

## 1.2 Regular Expressions

---

Regex is a pattern matching language. Many programming languages and system tools have a regex engine

**_System tools with regex:_**<br/>

- sed
- grep
- perl
- vim
- less

**_regex in javascript_**<br/>

- str.split(re)
- str.match(re)
- str.replace(re)
- re.test(str)
- re.exec(str)

### 1.2.1 Flags

```
/PATTERN/FLAGS
s/PATTERN/REP/FLAGS
```

- `i` - case insensitive
- `g` - match all occurences (global)
- `m` - treat string as multiple lines
- `s` - treat string as single line

### 1.2.2 Meta Characters

- `.` - matches any character
- `?` - zero or one time
- `*` - zero or more times
- `+` - one or more times
- `[]` - character class
- `^` - anchor at the beginning
- `$` - anchor to the end
- `(a|b)` - match `a` or `b`
- `()` - capture group
- `(?:)` - non capture group
- `\d` - digit `[0-9]`
- `\w` - word `[A-Za-z0-9_]`
- `\s` - whitespace `[ \t\r\n\f]`

**Examples**

```
echo 'hello beep boop' | sed 's/b..p/honk/'

Output: hello honk boop
```

```
echo 'hello beep boop' | sed 's/b..p/honk/g'

Output: hello honk honk
```

###### Wild card

`.` is the wild card

###### Quantifiers

- `?` - zero or one time
- `*` - zero or more times
- `+` - one or more times

how many times a thing should appear

**When to escape meta characters**
In some engines you need to escape metacharacters such as `+` and `?`. In others you don't.

In javascript and perl, you generally don't need to escape metacharacters. To use `grep` and `sed` in a similar way, use:

- `sed -r`
- `grep -E`

### 1.2.3 Character Classes

Character classes are ways of specifiying different characters that can match.

```
echo 'dog days cats' | sed -E 's/d[ao]/DA/g'

Output: DAg DAys cats
```

```
echo 'doooog daaays cats' | sed -E 's/d[ao]/DA/g'

Output: DAooog DAaays cats
```

**_you can put quantifiers around character classes_**

```
echo 'doooog daaays cats' | sed -E 's/d[ao]+/DA/g'

Output: DAg DAys cats
```

**You can also do a negation of character class**
That means it will match everything outside of that character class

`e.g I want to replace every character other than vowels with *`

```
echo 'replace all non vowels in this sentance' | sed -E 's/[^aeiou]/*/g'

Output: *e**a*e*a****o***o*e***i****i***e**a**e
```

###### Ranges to specify character classes

`Remove all digits from stirng`

```
echo 'hello1233 what9999 834958734' | sed -E 's/[0-9]+//g'

Output: hello what
```

`Remove all alphabets from string`

```
echo 'hello1233 what9999 834958734' | sed -E 's/[a-z]+//g'

Output: 1233 9999 834958734
```

**_You can mix and match ranges_**

```
echo 'HYIUello1233 what9999 834958734' | sed -E 's/[A-Za-z]+//g'

Output: 1233 9999 834958734
```

```
echo 'HYIUello1233__ __ what9999 834958734' | sed -E 's/[A-Za-z_]+//g'

Output: 1233 9999 834958734
```

**Practice**
`A password field with minimum 8 characters, it has to container number, a lower case letter, a capital letter and one of a handful of symbols`

```
Input: pw = 'abc432~A';

/^[\w!@$~]{8,}$/.test(pw) && /[a-z]/.test(pw) && /[A-Z]/.test(pw) && /\d/.test(pw) && /[!~@#$%^&]/.test(pw)


Output: true
```

###### Character class sequences

- `\w` - Word character : `[A-Za-z0-9_]`
- `\W` - non word character : `[^A-Za-z0-9_]`
- `\s` - whitespace : `[ \t\r\n\f]`
- `\S` - non-whitespace : `[^ \t\r\n\f]`
- `\d` - digit : `[0-9]`
- `\D` - non-digit : `[^0-9]`

### 1.2.4 Anchors, groups and sed

###### Anchors

- `^` - anchor at the beginning
- `$` - anchor to the end

`Following will replace the word 'beans' if it's at the start of string`

```
echo 'cool beans' | sed -E 's/^beans/BEANS/'

Output: cool beans
```

`Following will replace the word 'beans' if it's at the end of string`

```
echo 'cool beans' | sed -E 's/beans$/BEANS/'

Output: cool BEANS
```

###### Groups

(a|b) - match a or b

- `()` capture group
- `?:` non capture group

This is the way of telling the engine, either one of these patterns is valid. These patterns can be multiple characters not just the single characters

```
echo 'beep boop' | sed -E 's/b(ee|oo)p/XXX/g'

Output: XXX XXX
```

```
echo 'beep boop' | sed -E 's/b(e|o){2}p/XXX/g'

Output: XXX XXX
```

```javascript
var str = 'cool <beans> zzzz';
var m = /<([^>]+)>/.exec(str);


Output m : [ '<beans>',
  'beans',
  index: 5,
  input: 'coll <beans> zzzz',
  groups: undefined ]

m[1] = 'beans'

```

**\_You can refer to this capture group by **\$1**\_**

```javascript
"cool <beans> zzz".replace(/<([^>]+)>/, "$1");

Output: "cool beans zzz";
```

```javascript
"cool <beans> zzz".replace(/<([^>]+)> (\w+)/g, "$1 :$2");

Output: "cool beans :zzz";
```

`In sed`

```
echo 'cool <beans> zzz' | sed -E 's/<([^>]+)>/\1/g'

Output: cool beans zzz
```

```
echo 'cool <beans> zzz' | sed -E 's/<([^>]+)>/~~\1~~/g'

Output: cool ~~beans~~ zzz
```

###### Back References

**_TODO_**

The following commands gives pretty handy reference to Regexp.

`perldoc perlreref`

<br/><br/><br/>

## 1.3 Vim

---

###### Cheat Sheets

- [Vim cheat sheet (fprintf)](https://www.fprintf.net/vimCheatSheet.html)
- [Yannesposito Blog](http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/)

`Vim is a popular text editor`

#### Why vim?

- edit files on a remote server over ssh
- works without graphical desktop environment
- many programming specific features
- `really fast editing`
- Easily change indentation
- Syntax highlighting

#### Alternatives

- nano
- emacs
- vi

**_Vim uses ANSI codes to control a cursor and position blocks of text on the screen_**

### 1.3.1 Using Vim

- Go to `INSERT MODE` by pressing `i`
- If you want to go back to command mode, hit `esc`
- To quit, from command mode hit `:q`
- To save, from command mode hit `:w`
- If you want to do saving and quitting at the same time, from command mode hit `:wq`
- If you want to quit without saving, from command mode hit `:q!`
- **If you already opened vim using `vim` command and you want to open a file,
  hit `:o` and name of the file from command mode**

### 1.3.2 Modes and Moving around

You can use the arrow keys to move around.

###### Move around

- `k` to go `up`
- `j` to go `down`
- `h` to go `left`
- `l` to go `right`
  You can use `k` and `j` to go `up` and `down` respectively from command mode
- `^` or `0` - Move to the beginning of line
- `$` - Move to the end on line
- `gg` - Move to the beginning of the file
- `G` - Move to the end of file

### 1.3.3 Deleting and Searching

- `x` - Delete highlighted character from command mode
- `d$` - Delete from current character to the end of line
- `d0` or `d^` - Delete from current character to the beginning of line
- `u` - Undo the operation
- `ctrl + r` - Redo the operation

Combine these commands

- `dj` - delete current and next line
- `dk` - delete current and previous line
- `dgg` - delete from current line to beginning of file
- `dG` - delete from current line to end of file
- `2dd` - delete 2 lines (including current)
- `3dd` - delete 3 lines (including current) and so on
- `dl` - delete character to the right
- `dh` - delete character to the left

###### Searching

**You can search for text using Regular Expressions**

From command mode, type in regular expression for the word you want searched like `/word` and press enter. You will be skipped ahead of the next occurence of the word you searched.

**_After searching_**

- `n` - To move to the next occurence
- `N` - To go to the previous occurence

`If you want to search backwords try using '?word'`

`/[0-9]` search for numbers

###### Combine delete with regexp

`From command mode, press d and then type the pattern. This will delete everything from current position to that pattern. (not including the pattern)`

- `d/PATTERN` - delete to the next match of the pattern
- `d?PATTERN` - delete to the previous match of the pattern
- `dn` - delete the next already matched pattern
- `dN` - delete the previous already matched pattern

###### Jump around

- `f<character>` - jump forward to the character in the line
- `t<character>` - jump forward to the start of the character in the line
- `F<character>` - jump backword to the character in the line
- `T<character>` - jump backword to the start of the character in the line

**Combine with delete**

- `dt<character>` - delete to the character (not including the character)
- `df<character>` - delete to the character (including the character)
- `dT<character>` - delete backword to the character (not including the character)
- `dF<character>` - delete backword to the character (including the character)

### 1.3.4 Search and Replace

`:s/PATTERN/REPLACEMENT/FLAGS`

Try these on the line with string cats

**_Line Substitution_**

```
:s/cat/dog
:s/cat/dog/g
:s/cat/dog/i
```

**_Global Substitution_**

```
:%/cat/dog
:%/cat/dog/g
:%/cat/dog/ig
```

### 1.3.4 Visual Select

Press `v` to go into visual select mode. Move the cursor around to select the text.

###### Visual Modes

- `v` - select by characters
- `V` - select by lines
- ctrl + `v` - select in a block

Once you've selected the block, you can press:

- `y` - "yank" the text into paste buffer
- `x` or `d` - delete the selected text
- `>>` - indent text right by shiftwidth
- `<<` - indent the text left by shiftwidth

You can push `.` to indent again.

You can also use all the ways of moving around like `0`, `$`, `PATTERN`

**There is also visual line mode**
You can switch to visual line mode by entering `V`. It will only apply to whole lines.

Following command indents the spaces by two instead of four.
`s/ / /g`

Whenever you delete something in `Visual` mode, it actually gets into paste buffer. If you navigate to anywhere else in the file and press `p`, it will get pasted. If you want to copy without deleting, Press `y` and paste with `p`.

### 1.3.4 Paste buffer and Insert modes

There are more ways to insert mode that just `i`.

- `o` - Goto insert mode, inserting a line below the current line
- `O` - Goto insert mode, inserting a line above the current line
- `a` - Goto insert mode, at one character to the right
- `A` - Goto insert mode, at the end of the current line
- `J` - Move the next line to the end of the current line
- `(backtick) + .` - jump to the last edit

**_If you really really like vim, you can make your terminal follow the rules of vim_**
From you terminal, run
`set -o vi`
