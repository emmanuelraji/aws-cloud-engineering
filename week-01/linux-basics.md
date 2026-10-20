# cd (Change Directory)

## Essential Navigation Shortcuts

Navigating with full paths can be tedious. Fortunately, the shell provides several shortcuts to make moving around much faster.

- `.` (current directory): Represents the directory you are currently in.
- `..` (parent directory): Moves you one level up to the directory containing your current one.
- `~` (home directory): A shortcut to your personal home directory, like `/home/pete`.
- - (previous directory): Takes you back to the last directory you were in.

You can use these shortcuts with `cd`:

```bash
$ cd .
$ cd ..
$ cd ~
$ cd -
```

Go up two levels:

```bash
$ cd ../..
```

Go to a directory whose name contains spaces by quoting it:

```bash
$ cd "Vacation Photos"
```
# ls (List Directories)

## Viewing Hidden Files

Not all files in a directory are visible by default. In Linux, filenames that start with a dot (`.`) are hidden. You can view them with the `-a` option, which stands for all.

```bash
$ ls -a
.  ..  .bashrc  Documents  Pictures
```

## Getting Detailed Information

Another essential `ls` option is `-l` for long format. It shows file permissions, number of links, owner, group, size, modification time, and name.

```bash
$ ls -l
```

For easier file sizes, add `-h` for human-readable output:

```bash
$ ls -lh
```

## Sorting in Reverse Order

Sometimes you may want to change the sort order. The `-r` option lists files and directories in reverse order.

```bash
$ ls -r
```

You can sort by modification time with `-t`, then reverse it with `-r`:

```bash
$ ls -lt
$ ls -ltr
```

## Combining Command Flags

Commands have flags, also called options, to add more functionality. As we saw with `-a` and `-l`, you can combine them into a single command like `ls -la`. The order of the flags often does not matter, so `ls -al` works the same way.

```bash
$ ls -la
```

Useful combinations include:

```bash
$ ls -lh
$ ls -la
$ ls -ltr
```

## Common ls Options

- `a`: Show all files, including hidden files.
- `l`: Use long format.
- `h`: Show human-readable sizes with `l`.
- `r`: Reverse the sort order.
- `t`: Sort by modification time.
- `S`: Sort by file size.
- `d`: List the directory itself instead of its contents.

# find

## Choosing Where to Search

The basic syntax is:

```bash
find [PATH] [EXPRESSION]
```

The path chooses the starting point, and the expression selects or acts on entries below it.

This command searches `/home` and its descendants for entries named `puppies.jpg`:

```bash
$ find /home -name puppies.jpg
```

Recursion is the default. Use `.` as the starting path when you want to search the current directory tree.

## Matching Names and Types

The `-name` test accepts an exact basename or a shell-style pattern. Quote wildcard patterns so the current shell passes them unchanged to `find`:

```bash
$ find . -name "*.txt"
```

Without the quotes, the shell may expand `*.txt` against the current directory before `find` begins. Use `-iname` instead of `-name` when the name match should ignore letter case.

Add `-type d` to select directories or `-type f` to select regular files:

```bash
$ find /home -type d -name MyFolder
```

## Matching Size and Modification Time

Use `-size` with `+` for greater than the specified unit or `-` for less than it:

```bash
$ find . -type f -size +10M
$ find . -type f -size -1k
```

Here, uppercase `M` represents 1,048,576-byte units, while lowercase `k` represents 1,024-byte units. `find` rounds sizes up to the selected unit before applying the numeric comparison, so boundary behavior is based on those units.

Use `-mtime` to test the number of complete 24-hour periods since the file was modified:

```bash
$ find . -type f -mtime -7
$ find . -type f -mtime +30
```

- `mtime -7` matches a value less than 7, while `mtime +30` matches a value greater than 30. Because complete 24-hour periods are used, these tests are not based on calendar-midnight boundaries.

## Printing and Acting on Matches

If no action is supplied, GNU `find` prints matching paths. You can write `-print` explicitly when you want the expression's action to be clear:

Print matches explicitly:

```bash
$ find . -name "*.log" -print
```

Use `-exec` to run another command for matches:

```bash
$ find . -name "*.log" -exec ls -l {} \;
```

For the `\;` form, `{}` is replaced by one matching pathname for each command invocation. The semicolon terminates the `-exec` action and is escaped so the shell passes it to `find`.

Before using a destructive action such as `-delete` or an `-exec` command that changes files, run the same tests with `-print` and inspect every result. A narrower starting path and `-maxdepth N` can also limit the search.
