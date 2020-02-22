# ffmpeg & media manipulation

## Convert mkv to mp4 without re-encoding

```bash
for i in *.mkv; do
    ffmpeg -i "${i}" -map_metadata -1 -acodec copy -vcodec copy -movflags +faststart "${i%.*}.mp4"
    if [[ $? == 0 ]]; then
      rm "${i}"
    fi
done
```

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
  if [[ $(mediainfo "${i}" | grep -i "Movie name") ]]; then
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

## Media Verification

This one will print out any movie (\*.mp4) files that do NOT have an audio track.

```bash
for i in **/*.mp4; do
  if [[ ! $(mediainfo "${i}" | grep -i "audio") ]]; then
    echo -e "${i} has no audio!"
  fi
done
```
