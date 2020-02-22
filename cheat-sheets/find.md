# Find

First off, I don't like the way the find tool was designed or written. Despite that it
is an extremely powerful tool and therefore is necessary to use to accomplish certain
things easily and efficiently.

The fact that they decided to throw all Linux command-line parameter norms out the
window and come up with their own "interface" is terrible and makes it extremely
difficult to remember how to use the tool with any consistency. So, this cheat sheet
contains some examples of fairly "basic" uses (as well as a few more advanced) of find
simply because of the complexity of the interface.

## Remove empty directories

```bash
find /path/to/dir -empty -type d -delete

find . -empty -type d -delete
```

Not sure if the above example does it recursively (need to test), this command will:

```bash
find /path/to/dir -empty -type d | tail -r | xargs rmdir
```

## Count all files in a dir tree (recursively searches through the folders)

```bash
find /path/to/dir -type f | wc -l
```

## Flatten a directory (move all files in sub-folders to some other folder - the current parrent directory in this example)

```bash
find . -mindepth 2 -type f -exec mv -t . -i '{}' +
```
