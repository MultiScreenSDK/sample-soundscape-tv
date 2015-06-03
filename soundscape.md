# Sound Scape

Sound Scape is a sample Jukebox app for playing music on Samsung Radiant
Speakers and Samsung 2015 TVs. It demonstrates how to cast and control music from
smartphones (Android, iOS and Web) to Samsung speakers and Samsung TVs using
Samsung Multiscreen APIs.

The following models are supported: For 2015 TVs
J4500, J5500 and above, (Except for J6200). Models 110S9 are also compatible.
For Speakers WAM6500/1 and WAM7500/1.

## Links to Code Repos

- [Android App](https://github.com/MultiScreenSDK/android-audioplayer)
- [iOS App](https://github.com/MultiScreenSDK/ios-audioplayer)
- [TV App](https://github.com/MultiScreenSDK/webapp-audioplayer) (In addition to
the TV/speaker host app, it also contains code for mobile HTML5 client app under
the mobile directory).

## Application Overview

Music files (.mp3) are stored in the cloud.  When you launch the mobile app, device discovery reports any found devices. When you connect to the device, the speaker or TV app will assign the client a user color and the user can select music from the cloud library to add to the shared queue.  The speaker or TV will play the tracks in the order they were added. Users can skip or delete songs from the queue.

## Communication Protocol (Channel Events)

The communication protocol between the TV host app and the mobile client apps.

####assignColorRequest
**client -> host**
Sent to 'host' as a request for the host to assign the client a color used to
identify the songs from that client.
```javascript
{
    "event": "assignColorRequest",
    "to": "host"
}
```

####appStateRequest
**client -> host**
Sent to 'host' as a request for the host to send the current application state
```javascript
{
    "event": "appStateRequest",
    "to": "host"
}
```

####play
**client -> host**
Published to the TV (host) so it will resume playing the song. Clients should not need to listen to this event as they will receive a trackStatus event once playback starts.
```javascript
{
    "method": "ms.channel.emit",
    "params": {
        "event": "play",
        "to": "host"
    }
}
```

####pause
**client -> host**
Published to the TV (host) so it will pause playback of the song. Clients should not need to listen to this event as they will receive a trackStatus event once paused starts.
```javascript
{
    "method": "ms.channel.emit",
    "params": {
        "event": "pause",
        "to": "host"
    }
}
```

####next
**client -> host**
Published to the TV (host) so it skip to the next track. Clients should not need to listen to this event as they will receive a trackEnd, trackStart, and trackStatus event.
```javascript
{
    "method": "ms.channel.emit",
    "params": {
        "event": "pause",
        "to": "host"
    }
}
```

####addTrack
**client -> broadcast**
Published to everyone so they can add the track to the local list of songs. Payload is a track model.
```javascript
{
    "event": "addTrack",
    "data": {
        "id" : "129378641982734", // This id needs be generated and unique per add
        "artist": "Ladyhawke",
        "album": "Ladyhawke",
        "title": "My Delirium",
        "duration": 256,
        "file": "http://s3-us-west-1.amazonaws.com/dev-multiscreen-music-library/songs/Ladyhawke - Ladyhawke - My Delirium.mp3",
        "albumArt": "http://s3-us-west-1.amazonaws.com/dev-multiscreen-music-library/artwork/Ladyhawke-Ladyhawke.jpg",
        "albumArtThumbnail": "http://s3-us-west-1.amazonaws.com/dev-multiscreen-music-library/artwork/Ladyhawke-Ladyhawke-thumbnail.jpg",
        "color": "#01579b" // This should be the current clients color selected at start
    },
    "to": "broadcast"
}
```

####removeTrack
**client -> broadcast**
Published to everyone so they can remove the track from the local list of songs. Sends the id of the track to remove
```javascript
{
    "method": "ms.channel.emit",
    "params": {
        "event": "removeTrack",
        "data": "129378641982734", // The id of the track to remove
        "to": "broadcast"
    }
}
```

####assignColor
**host -> client**
Sent to an individual client as a response to a 'assignColorRequest' event. This response includes the hex code for a color assigned to that client.

```javascript
{
  	"event": "assignColor",
  	"data": "#1B5E20",
  	"from": "c0061370-057e-11e5-bbb0-a337c0e8525f"
}
```

####appState
**host -> client**
Sent to an individual client as a response to a 'appStateRequest' event. This response includes the current playlist (array of track models) and the nowPlaying which is a give information about the current track (a track status model)

```javascript
{
    "event": "appState",
    "data": {
        currentStatus : {
            id : "129378641982734",
            time : "127",
            state : "playing"
        },
        playlist : [
            {
                "id" : "129378641982734",
                "artist": "Ladyhawke",
                "album": "Ladyhawke",
                "title": "My Delirium",
                "duration": 256,
                "file": "http://s3-us-west-1.amazonaws.com/dev-multiscreen-music-library/songs/Ladyhawke - Ladyhawke - My Delirium.mp3",
                "albumArt": "http://s3-us-west-1.amazonaws.com/dev-multiscreen-music-library/artwork/Ladyhawke-Ladyhawke.jpg",
                "albumArtThumbnail": "http://s3-us-west-1.amazonaws.com/dev-multiscreen-music-library/artwork/Ladyhawke-Ladyhawke-thumbnail.jpg",
                "color": "#01579b"
            },
            {
                "id" : "12937fgfds2734",
                "artist": "City and Colour",
                "album": "Alternative Times, Volume 97",
                "title": "Sleeping Sickness",
                "duration": 248,
                "file": "http://s3-us-west-1.amazonaws.com/dev-multiscreen-music-library/songs/City and Colour - Alternative Times, Volume 97 - Sleeping Sickness.mp3",
                "albumArt": "http://s3-us-west-1.amazonaws.com/dev-multiscreen-music-library/artwork/City and Colour-Alternative Times, Volume 97.jpg",
                "albumArtThumbnail": "http://s3-us-west-1.amazonaws.com/dev-multiscreen-music-library/artwork/City and Colour-Alternative Times, Volume 97-thumbnail.jpg",
                "color": "#e65100"
            },
            {
                "id" : "1293453245982734",
                "artist": "k.flay",
                "album": "Eyes Shut",
                "title": "Sunburn",
                "duration": 237,
                "file": "http://s3-us-west-1.amazonaws.com/dev-multiscreen-music-library/songs/k.flay - Eyes Shut - Sunburn.mp3",
                "albumArt": "http://s3-us-west-1.amazonaws.com/dev-multiscreen-music-library/artwork/k.flay-Eyes Shut.jpg",
                "albumArtThumbnail": "http://s3-us-west-1.amazonaws.com/dev-multiscreen-music-library/artwork/k.flay-Eyes Shut-thumbnail.jpg",
                "color": "#e65100"
            }
        ]
    },
    "to": "<id of client who requested it>"
}
```

####trackStart
**host -> broadcast**
Published to all clients when the host begins playing a track, so they can update the local UI to reflect the currently playing song.
```javascript
{
    "method": "ms.channel.emit",
    "params": {
        "event": "trackStart",
        "data": "129378641982734",
        "to": "broadcast"
    }
}
```

####trackEnd
**host -> broadcast**
Published to all clients when the host finishes playing a track, so they can update the local UI.. and remove the track if desired.
```javascript
{
    "method": "ms.channel.emit",
    "params": {
        "event": "trackEnd",
        "data": "129378641982734",
        "to": "broadcast"
    }
}
```

####trackStatus
**host -> broadcast**
Published to all clients to report any status changes about the track, so they can update the local UI to reflect the change (current playhead time, or play/pause state. Payload is a track status model.
```javascript
{
    "method": "ms.channel.emit",
    "params": {
        "event": "trackStatus",
        "data": {
            id : "129378641982734",
            time : "127",
            state : "playing"
        },
        "to": "broadcast"
    }
}
```
