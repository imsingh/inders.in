---
layout: post
title: Introducing rxjs-audio
subtitle: No Fuss Library to add audio playback in your JavaScript apps
category: blog
comments: true
---

**TL;DR:** This is the `release` announcement for [`rxjs-audio`](https://github.com/imsingh/rxjs-audio/), a [`rxjs`](https://github.com/ReactiveX/rxjs) based audio playback library.

# Introduction
It's very often in our web apps, we need audio `playback` functionality. Every time I had to do it, I felt miserable doing similar things again and again. Something that makes us Developers I suppose. Managing `Media Events` and `State` is rather a difficult task. To ease this pain, I created this tiny [library](https://github.com/imsingh/rxjs-audio){:target="_new"}.

## Installation

```bash
# npm
npm install rxjs-audio --save
# yarn
yarn install rxjs-audio
```

## Usage
In order to `play` the audio files/urls. You need to create first `AudioStream` Object via `RxjsAudioService.create` method, as shown below:

### Creating a Stream
```ts
import { RxJSAudioService } from 'rxjs-audio';
...
rxjsAudioService = new RxJSAudioService();
audio: AudioStream;
    constructor() {
        this.audio = this.rxjsAudioService.create(this.tracks);
    }
}
```

`RxJSAudioService.create` method returns and `AudioStream` Object. This `AudioStream` gives you all the playback methods like `play` and `pause` etc.

### Track Input and Configuration

The `RxJSAudioService.create` method takes either a single track or Array of audio track as an Input and an optional `config` object. Take a look at following examples.

#### 1. Single Track Input

Input to [`RxJSAudioService.create`](https://imsingh.github.io/rxjs-audio/classes/rxjsaudioservice.html#create){:target="_new"} can be a single track as shown below:

```ts
track:string = 'https://ia801609.us.archive.org/16/items/nusratcollection_20170414_0953/Man%20Atkiya%20Beparwah%20De%20Naal%20Nusrat%20Fateh%20Ali%20Khan.mp3';
this.audio = this.rxjsAudioService.create(this.track);
```

#### 2. Multi Track Input

```ts
tracks:Array<string> = [
    'https://ia801504.us.archive.org/3/items/EdSheeranPerfectOfficialMusicVideoListenVid.com/Ed_Sheeran_-_Perfect_Official_Music_Video%5BListenVid.com%5D.mp3',
    'https://ia801609.us.archive.org/16/items/nusratcollection_20170414_0953/Man%20Atkiya%20Beparwah%20De%20Naal%20Nusrat%20Fateh%20Ali%20Khan.mp3',
    'https://ia801503.us.archive.org/15/items/TheBeatlesPennyLane_201805/The%20Beatles%20-%20Penny%20Lane.mp3',
];
this.audio = this.rxjsAudioService.create(this.tracks);
```

#### 3. Complex Track Input and Configuration

In real world application, it's higly unlikely that you would have an `array` of `strings`, instead you would use `array` of `objects`, as shown below:

```ts
tracks:Array<any> = [
    { 
        url: 'https://ia801504.us.archive.org/3/items/EdSheeranPerfectOfficialMusicVideoListenVid.com/Ed_Sheeran_-_Perfect_Official_Music_Video%5BListenVid.com%5D.mp3',
        name: 'Perfect',
        artist: 'Ed Sheeran'
    },
    {
        url: 'https://ia801609.us.archive.org/16/items/nusratcollection_20170414_0953/Man%20Atkiya%20Beparwah%20De%20Naal%20Nusrat%20Fateh%20Ali%20Khan.mp3',
        name: 'Man Atkiya Beparwah',
        artist: 'Ustad Nusrat Fateh Ali Khan',
    },
    {
        url: 'https://ia801503.us.archive.org/15/items/TheBeatlesPennyLane_201805/The%20Beatles%20-%20Penny%20Lane.mp3',
        name: 'Penny Lane',
        artist: 'The Beatles'
    }
];
```

You can use this structure by telling the service about the `url` of Audio by configuring `urlKey` of tracks, as shown below:
```ts
this.audio = this.rxjsAudioService.create(this.tracks, { urlKey: 'url' })
```

#### Configuration

There are two more configuration options:
1. `initialTrack:number`: It's used to set the Initial/First Track of the Playlist, that you want to play.
2. `autoPlayNext:boolean`: If set to `true`, it will play the next track after the current track is `ended`.

### Audio Playback

`rxjs-audio` provides lots of playback features like `play`, `pause`, `next`, `previous` out of the box. Check the documentation of [`AudioStream`](https://imsingh.github.io/rxjs-audio/classes/audiostream.html){:target="_new"} class for more detail.

#### Playing a stream

To `play` the media. Run the following:

```ts
this.audio.play();
```

#### Pausing a stream

To `pause` the media. Run the following:

```ts
this.audio.pause();
```

#### Playing `next` track

To play the `next` track in list. Run the following:

```ts
this.audio.next();
this.audio.play();
```

Check the [Documentation](https://imsingh.github.io/rxjs-audio){:target="_new"} and [Demo](https://imsingh.github.io/ngx-audio-app){:target="_new"}. application for more detail.

### Listening to Audio Events

It's fairly easy to listen to audio media events like `playing`, `ended`. 

You just need to `subscribe` to the `Observable` return by `AudioStream.events()` method, as shown below:
```ts
this.audio.events()
.subscribe(event => {
   // console.log(event);
});
```

### Listening to State Changes
`rxjs-audio` also provides an Observable to listen to state changes. You can use it as shown below:

```ts
// Update State
this.audio.getState()
.subscribe(state => {
    this.state = state;
});
```

The `state:StreamState` Object gives us folowing information:
```ts
{
    playing: boolean; // If media is currently playing or not.
    isFirstTrack: boolean; // If first track is playing or not.
    isLastTrack: boolean; // If last track is playing or not.
    trackInfo: StreamTrackInfo // Check the definition below
}
```

And `trackInfo:StreamTrackInfo` provide us following information:

```ts
{
    currentTrack: number | undefined, // index of the current playing track
    readableCurrentTime: string, // currentTime in human readable form(HH:MM:ss)
    readableDuration:string,    // duration of media in readable form(HH:MM:ss)
    duration: number | undefined, // duration of media
    currentTime: number | undefined // currentTime of media
}
```

### Error Handling

Following is a very simple example of Error Handing

```ts
this.audio.events()
.subscribe(event => {
    if(event.type === 'canplay') {
        this.error = false;
    }
    else if(event.type === 'error') {
        this.error = true;
    }
});
```

### Demo app
Check out the this demo application with @angular [here](https://imsingh.github.io/ngx-audio-app){:target="_new"}. and it's source code [here](https://github.com/imsingh/ngx-audio-app){:target="_new"}.

# Conclusion
In the post, I introduced `rxjs-audio` to easily add Audio Playback into web applications. I hope it will help fellow developers in making incredible audio applications.