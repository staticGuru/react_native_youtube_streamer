<h1 align="center">
  React Native Youtube Streamer
</h1>

<h4 align="center">A cross-platform Youtube video player with customizable controls.</h4>

<br>

This library provide a fully customisable Youtube video player that work both on Android and iOS. It also come with common use case documentation of things that you would like to implements.

<p align="center">
  <img src="./assets/youtube.png" alt="" width=400>
</p>
<p align="center">
  <img src="./assets/videoImage.png" alt="" width=400>
</p>

By default there are two controls slots that are displayed respectively on different part of the parent container and you can use default components provided by this library:

- **Middle**. Contain by default a grid display two buttons:
  - One with play / pause alternating.
  - Another that will restart the video.
- **Bottom**. Contain the video current time, a progress bar and the total duration.
- **Loader**. There is also a loader that will trigger while video is charging (network issues, bootstraping, ...).
- **VideoId**. Contain the valid Youtube video Id.

## Documentation

- [Installation chapter](./doc/install.md)
- [Render a FullScreen Video player](./doc/full-screen-player.md)
- [Implement your own controls bar](./doc/custom-controls-bar.md)

# Quick documentation

This is simple as that.

VideoPlayer ship around any video component, but fits well with react-video. In v2 you've total control on the video component.

- **autoStart** - Whether or not the video should start when rendered (Default to true).
- **mainControl** - The component used to render the main control bar, you can use the default one provided by this lib or your own.
- **bottomControl** - The component used to render the bottom control bar, you can use the default one provided by this lib or your own.
- **VideoId**. Contain the valid Youtube video Id.

For advanced configuration, such as infinite loop, check the rest of the documentation and custom controls bar.
**youtubeCustomUrl** - Youtube video URLs extracted from YouTube
**stream** - Get all the details about YouTube videos

```jsx
import React, { Component } from "react";
import { View } from "react-native";
import Video from "react-native-video";
import {
  VideoPlayer,
  DefaultMainControl,
  DefaultBottomControlsBar,
  videoId,
} from "react_native_youtube_streamer";

export default class App extends Component {
  render() {
    return (
      <VideoPlayer
        autoStart={false}
        mainControl={(args) => <DefaultMainControl {...args} />}
        bottomControl={(args) => <DefaultBottomControlsBar {...args} />}
        videoId="tsPSBLX1GPg" //<-- youtube-video-id -->
      >
        {(args) =>
          args.youtubeCustomUrl && (
            <Video
              ref={args.playerRef}
              source={{
                uri: args.youtubeCustomUrl,
              }}
              style={styles.backgroundVideo}
              resizeMode="cover"
              paused={args.videoPaused}
              onLoad={args.onLoad}
              onProgress={args.onProgress}
              onEnd={args.onEnd}
            />
          )
        }
      </VideoPlayer>
    );
  }
}
var styles = StyleSheet.create({
  backgroundVideo: {
    position: "absolute",
    top: 0,
    left: 0,
    bottom: 0,
    right: 0,
    backgroundColor: "black",
  },
});
```
