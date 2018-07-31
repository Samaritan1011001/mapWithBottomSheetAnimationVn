<template>
	<animated:view class="itemContainer" :style="{top:top}">
    <parallax-swiper
      :speed="0.5"
      :animatedValue="this.myCustomAnimatedValue"
      :dividerWidth="12"
      dividerColor="transparent"
      backgroundColor="transparent"
      :on-momentum-scroll-end="activePageMethod"
      :showProgressBar="true"
      progressBarBackgroundColor="rgba(0,0,0,0.25)"
      progressBarValueBackgroundColor="white"
      :scroll-to-index="tapped"
    >
    <parallax-swiper-page
      v-for="marker in markers"
      :key="marker.id"
      :BackgroundComponent="renderBackgroundComponent(marker,top)"
    />
    </parallax-swiper>
  </animated:view>
</template>
 
<script>
const { width, height } = Dimensions.get("window");
import GestureRecognizer, {
  swipeDirections
} from "react-native-swipe-gestures";

import React from "react";
import {
  ParallaxSwiper,
  ParallaxSwiperPage
} from "react-native-parallax-swiper";
import {
  Image,
  Dimensions,
  Animated,
  Easing,
  View,
  Text,
  TouchableOpacity,
  PanResponder
  // Icon
} from "react-native";

export default {
  props: {
    markers: Array,
    tapped: Number,
    top: Number,
    activePageMethod: Function
  },
  data: function() {
    return {
      initialCoords: {
        latitude: 12.9107,
        longitude: 77.6018,
        latitudeDelta: 0.0122,
        longitudeDelta: 0.0221
      },
      myCustomAnimatedValue: 0,
      width: width,
      pageTop: this.top,
      cardTapped: 0,
      _panResponder: 0
    };
  },
  created: function() {
    this.myCustomAnimatedValue = new Animated.Value(0);
  },
  methods: {
    renderBackgroundComponent: function(marker, top1) {
      console.log(top1);
      return (
        <GestureRecognizer
          onSwipe={(direction, state) => this.onSwipe(direction, state)}
          onSwipeUp={state => this.onSwipeUp(state, marker)}
          onSwipeDown={state => this.onSwipeDown(state, marker)}
          style={{
            flex: 1,
            backgroundColor: "transparent"
          }}
        >
          <View
            style={{
              width: width - 20,
              height: height - 100,
              backgroundColor: "white",
              marginRight: 10,
              marginLeft: 10,
              paddingBottom: 150,
              borderRadius: 10
            }}
          >
            <Image
              style={{
                height: 150,
                borderRadius: 10
              }}
              source={{
                uri:
                  "https://i1.wp.com/www.turningleftforless.com/wp-content/uploads/2017/05/hotel-1979406_1920.jpg?zoom=2&resize=777%2C437"
              }}
            />
            <View>
              <View
                style={{
                  flexDirection: "row",
                  paddingTop: 10,
                  justifyContent: "space-between"
                }}
              >
                <Text>{marker.data.title}</Text>
                <Image
                  style={{
                    height: 15,
                    width: 15
                  }}
                  source={require("./assets/star-icon.png")}
                />
                <Image
                  style={{
                    height: 15,
                    width: 15
                  }}
                  source={require("./assets/star-icon.png")}
                />
                <Image
                  style={{
                    height: 15,
                    width: 15
                  }}
                  source={require("./assets/star-icon.png")}
                />
                <Image
                  style={{
                    height: 15,
                    width: 15
                  }}
                  source={require("./assets/star-icon.png")}
                />
                <Image
                  style={{
                    height: 15,
                    width: 15
                  }}
                  source={require("./assets/star-icon.png")}
                />
                <Text style={{ fontWeight: "bold" }}>${marker.cost}</Text>
              </View>
              <Text>{marker.data.address} </Text>
            </View>
          </View>
        </GestureRecognizer>
      );
    },
    onSwipe: function(gestureName, gestureState) {
      console.log("swiped");
    },
    onSwipeUp: function(gestureState, marker) {
      this.tapped = marker.id;
      console.log("You swiped up!", gestureState.dy);
      this.animate();
      this.top = this.myCustomAnimatedValue.interpolate({
        inputRange: [0, 1],
        outputRange: [height - 150 - 64, 50]
      });
    },

    onSwipeDown: function(gestureState, marker) {
      this.tapped = marker.id;
      console.log("You swiped down!");
      this.animate();
      this.top = this.myCustomAnimatedValue.interpolate({
        inputRange: [0, 1],
        outputRange: [50, height - 150 - 64]
      });
    },
    animate: function() {
      this.myCustomAnimatedValue.setValue(0);
      Animated.timing(this.myCustomAnimatedValue, {
        toValue: 1,
        duration: 1000,
        easing: Easing.linear
      }).start();
    },
    expandCard: function(marker, cardTappedId) {
      console.log("in exapnd card: ", this.markers[cardTappedId].top);
      this.animate();
      this.top = this.myCustomAnimatedValue.interpolate({
        inputRange: [0, 1],
        outputRange: [height - 150 - 64, 50]
      });
    }
  },
  components: {
    ParallaxSwiper,
    ParallaxSwiperPage
  }
};
</script>
<style>
.pageDecor {
  position: absolute;
}
.itemContainer {
  position: absolute;
  background-color: transparent;
  flex-direction: row;
}
</style>
