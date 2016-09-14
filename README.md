# Pseudo-live HLS server
The point of this project is to load a pre-segmented HLS stream and serve it as if it was a live stream, tracking the time since the server started serving the playlists and segments based upon their duration

Run:
```
$ pip install flask
$ python live_server.py 127.0.0.1 5000
```

Assumptions:
1. HLS streams will be presegmented and in their own directores under static/
2. A playlist named abr.m3u8 will be in the root of the stream directory, i.e. static/big_buck_bunny/abr.m3u8, that includes the path to the playlist for each variant
3. Each variant will be it it's own subdirectory, i.e. static/big_buck_bunny/1000k/, with a playlist with all segements and any required discontinutities.
4. ffprobe, or a link to a recent build of ffprobe, is in the script directory.
  * ffprobe is used to read the actual segment duration, it is important for caclulating which segments should be included in the playlist


Variant playlist: http://localhost:5000/[stream directory].m3u8
Playlists: http://localhost:5000/[stream directory]_[variant].m3u8

For example if you have Big Buck Bunny under stat

Known issues

Thank you to @jbochi for the orginal script that I build this from.
