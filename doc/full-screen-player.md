# Handling fullscreen

The best way to deal with that is to display a full screen modal and render a `VideoPlayer` component.

Don't forget to add a background around your video. Here we use a black one.

```jsx
import PropTypes from "prop-types";
import React, { PureComponent } from "react";
import { StatusBar, View } from "react-native";
import VideoPlayer from "react-native-true-sight";
import Immersive from "react-native-immersive";

import CloseButton from "../../Components/Common/CloseButton";

export default class FullScreenVideoModal extends PureComponent {
  render() {
    return (
      <View style={{ flex: 1, backgroundColor: "black" }}>
        <StatusBar barStyle="light-content" />
        <CloseButton onPress={this.onClose} />
        <VideoPlayer source="https://somevideo.mp4" />
      </View>
    );
  }
}
```

# Android specific

For cross compatibility this library doesn't ship with a tool that allow fullscreen on android.

But you can easily implement this behavior using a full screen library like [React Native Immersive](https://github.com/mockingbot/react-native-immersive).

Here is an example :

```jsx
import React, { Component } from "react";
import PropTypes from "prop-types";
import { Platform, StatusBar, View } from "react-native";
import Video from "react-native-video";
import {
  VideoPlayer,
  DefaultMainControl,
  DefaultBottomControlsBar,
  videoId,
} from "react_native_youtube_streamer";
import Immersive from "react-native-immersive";
import CloseButton from "../../Components/Common/CloseButton";

export default class FullScreenVideoModal extends Component {
  constructor(props) {
    super(props);

    this.onClose = this.onClose.bind(this);
  }

  componentDidMount() {
    if (Platform.OS === "android") Immersive.on();
  }

  componentWillUnmount() {
    if (Platform.OS === "android") Immersive.off();
  }

  onClose() {
    // Dismiss modal
  }
  render() {
    return (
      <View style={{ flex: 1, backgroundColor: "black" }}>
        <StatusBar barStyle="light-content" />
        <CloseButton onPress={this.onClose} />
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
      </View>
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
