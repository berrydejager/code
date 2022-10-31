---
layout: post
title: 
date: 2016-09-25 13:37:00 +0100
description:  
img: header/pimusicbox-not-showing-spotify-playlist.gif
tags: [Python, RaspberryPi, Linux, PiMusicBox, Spotify]
---
As some point my PiMusicBox stopped showing the Spotify playlists in the PiMusicBox web interface.

I noticed this happened since a specific time, as one PiMusicbox is still showing the playlists and a newly installed PiMusicBox v0.60, which contains Mopidy 0.19.5, didn’t show the Spotify playlists anymore. This was very frustrating. Many ‘solutions’ will tell you to remove the local cache directory but i found one post, at the [Github issue list from Fatg3erman](https://github.com/mopidy/mopidy-spotify/issues/27#issuecomment-60792946), which was the answer to my quest, changing the Spotify session manager python script.

#### Fixing the problem!

This can be fixed by editing the Mopidy session manager.

Just log in as root to your PiMusicBox and edit the Spotify session manager script.

    nano /usr/local/lib/python2.7/dist-packages/mopidy_spotify/session_manager.py

The installed PiMusicBox (version 0.60) needs some commented lines, from ```if …``` (line #173) to ```return``` (line #175);

    def refresh_playlists(self):
    """Refresh the playlists in the backend with data from Spotify"""
    # if not self._initial_data_receive_completed:
    # logger.debug('Still getting data; skipped refresh of playlists')
    # return
    playlists = []
    folders = []
    for spotify_playlist in self.session.playlist_container():

Reboot your PiMusicBox and have a look at ```http://musicbox.local/#playlists``` again.

#### Changing your Spotify account.

I have noticed that changing your Spotify account doesn’t always work as desired. Just remove the Spotify cache from the Mopidy directory and restarting the Mopidy daemon again.

    rm /var/lib/mopidy/spotify/Users/* -rf
    service mopidy restart
