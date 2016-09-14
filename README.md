# Pseudo-live HLS server
The point of this project is to load a pre-segmented HLS stream and serve it as if it was a live stream, tracking the time since the server started serving the playlists and segments based upon their duration

Run:
```
$ pip install flask
$ python live_server.py 127.0.0.1 5000
```

Assumptions:

1. HLS streams will be presegmented and in their own directores under static/
* A playlist named _abr.m3u8_ will be in the root of the stream directory, i.e. _static/big_buck_bunny/abr.m3u8_, that includes the path to the playlist for each variant
* Each variant will be it it's own subdirectory, i.e. _static/big_buck_bunny/1000k/_, with a playlist with all segements and any required discontinutities.
* ffprobe, or a link to a recent build of ffprobe, is in the script directory.  You can download the latest version at http://ffmpeg.org
  * ffprobe is used to read the actual segment duration, it is important for caclulating which segments should be included in the playlist


Master playlist: *http://localhost:5000/[stream directory].m3u8*

Variant playlists: *http://localhost:5000/[stream directory]_[variant].m3u8*

For example if you have Big Buck Bunny under static/big_buck_bunny then the master playlist is called using:

http://localhost:5000/big_buck_bunny.m3u8

If this is the first time the stream has been requested it will cause the server to read in the master and variant playlists and it will check each segments duration using ffprobe.  The stream request may result in a 503 for the first few requests, until a few segments are read in.

Known issues

* There can be varient alignment slip over time.  I have seen that one or more variants can serve different segments than the other variants, i.e. the 1000k is serving segment 017 while 2000k and 3000k are serving segment 016.  This will can cause repeats or jumps when a bit rate switch occurs.
* The segments are not properly rotated until the reading of segments for that variant is complete.

Thank you to @jbochi for the orginal script that I build this from.
