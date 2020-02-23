# Rsync

## Typical backup

Full backup with delete of missing files:

```bash
sudo rsync -aAv --delete-before --stats --progress <source path>/ <dest path>

sudo rsync -aAv --delete-before --stats --progress /mnt/p/ /mnt/b
```

The `-a` is equivalent to `-rlptgoD`.

The `-v` increases the verbosity and can be omitted.

Changing `--delete-before` to simply `--delete` can be faster but may produce issues if
the destination location does not have enough capacity to hold the new content without
first deleting the previously deleted content.

## Full backup with checksum verification

A `-c` can be added to perform a full checksum on the files. This can take an
**extremely LONG** time, but can be good from time-to-time to ensure no write errors in
the backup.

```bash
sudo rsync -aAvc --delete-before --stats --progress <source path>/ <dest path>
```

## Compression if not copying to local disks

If the rsync source and destinations are on different machines (regardless of how the
connection is being made - ssh, etc.), a `-z` should be added to compress the bits
before they are sent across the wire.

```bash
sudo rsync -aAvz --delete-before --stats --progress <source path>/ <dest path>
```

## A note on paths

It is **VERY** important that the source path end in a final /. This means to copy the
files _within_ that directory to the destination location, rather than just copying the
source folder or file over.

So something like:

```bash
sudo rsync -aAv --delete-before --stats --progress /mnt/disk1/some-folder/ /mnt/disk2/some-folder
```

Without the extra slash, it would create `some-folder` inside the destination disks
`some-folder` directory rather than syncronize the contents of the two.
