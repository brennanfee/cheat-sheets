# Bulk Renaming Files

These examples use the excellent Perl rename utility. The only complication is that on
some systems the command is simply named `rename` and on other systems it is named
`perl-rename`. On systems with `perl-rename` you can either alter the commands below
before executing or set an alias for rename, either just for your session or permanently
in your bash profile.

Most of these were centered around renaming files in TV shows episodes.

**NOTE:** These all have a -n at the end of their commands. When first executed it will
not make any changes to the files but instead list out for you what it would do. If you
approve of the changes you simply remove the -n at the end of the command and run again.

## Set an alias for perl-rename

```bash
alias rename="perl-rename"
```

## Fix periods before the final extension

Replace with space:

```bash
rename -f 's/\.(?![^.]+$)/ /g' * -n
```

## Gather just the season and episode #'s

Resulting in files like `The Show s01e06.mp4`.

Hard coded show name:

```bash
rename -f 's/.*[Ss](\d\d)[Ee](\d\d).*$/yourShowHere s$1e$2\.mp4/' *.mp4 -n
```

Gather the show name from the file (no adjustment will be made to the show name so be
careful).

```bash
rename -f 's/(.*)[\. ][Ss](\d\d)[Ee](\d\d).*$/$1 s$2e$3\.mp4/' *.mp4 -n
```

## Keep the episode name

Resulting in files like `The Show s01e06 - The Episode Title.mp4`. Note that no changes
are made to the episode name so after reformatting some edits may still be required to
clean things up.

```bash
rename -f 's/(.*) [Ss](\d\d)[Ee](\d\d) (.*) (1080p|720p).*$/$1 s$2e$3 - $4\.mp4/' *.mp4 -n
```
