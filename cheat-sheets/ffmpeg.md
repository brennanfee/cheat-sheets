# ffmpeg & media manipulation

## Convert mkv to mp4 without re-encoding

```bash
for i in **/*.mkv; do
    ffmpeg -i "${i}" -map_metadata -1 -codec copy -movflags +faststart "${i%.*}.mp4"
    if [[ $? == 0 ]]; then
      rm "${i}"
    fi
done
```

## Extract Subtitles

Note: This should be done before stripping of metadata.  This also should generally only work with subtitles of type "mov_text" in the file.

```bash
ffmpeg -i "input-file.mp4" -map 0:s:0 subs.srt
```

If there are multiple subtitles, you may need to change the last zero in -map 0:s:0 to select the correct one.

## Strip Metadata

This one _does not_ check for metadata info first, so use with caution.

```bash
for i in **/*.mp4; do
    ffmpeg -i "${i}" -map_metadata -1 -codec copy -movflags +faststart "fixed-${i##*/}"
    if [[ $? == 0 ]]; then
      rm "${i}"
      mv "fixed-${i##*/}" "${i}"
    fi
done
```

This one does check for metadata info first. You must have the `mediainfo` application
installed.

```bash
for i in **/*.mp4; do
  if [[ $(mediainfo "${i}" | grep -i "Movie name\|Title") ]]; then
    ffmpeg -i "${i}" -map_metadata -1 -codec copy -movflags +faststart "fixed-${i##*/}"
    if [[ $? == 0 ]]; then
      rm "${i}"
      mv "fixed-${i##*/}" "${i}"
    fi
  else
    echo -e "Skipping ${i}"
  fi
done
```

## Trim without re-encoding

Trim from the front of a file.  The time is in hh:mm:ss format.

```bash
ffmpeg -i "input-file.mp4" -codec copy -ss 1:30 -movflags +faststart "output-file.mp4"
```

## Concatinate two files without re-encoding

Create a file <name>.txt with all the files you want to have concatenated in the following form (lines starting with a # are ignored): 

```
# this is a comment
file '/path/to/file1.mp4'
file '/path/to/file2.mp4'
file '/path/to/file3.mp4'
```

The run this command:

```
ffmpeg -f concat -safe 0 -i <name>.txt -codec copy -movflags +faststart "output-file.mp4"
```

## Media Verification

This one will print out any movie (\*.mp4) files that do NOT have an audio track.

```bash
for i in **/*.mp4; do
  if [[ ! $(mediainfo "${i}" | grep -i "audio") ]]; then
    echo -e "${i} has no audio!"
  fi
done
```
