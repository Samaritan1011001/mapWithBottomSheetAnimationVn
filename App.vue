<template>
	<view class="wrapper-view" >
		<map-view :initialRegion="initialCoords" class="container" provider="google" :on-long-press="startAnimation" >
      <marker :key="markerr.id" :coordinate="markerr.coordinate" v-for="markerr in markers"  v-if="swipedToMarker!=markerr.id">
        <custom-marker :marker="markerr" v-bind:greenFlag="false"></custom-marker>
      </marker>
      <marker :key="markerr.id" :coordinate="markerr.coordinate" v-for="markerr in markers" v-if="swipedToMarker==markerr.id">
        <custom-marker :marker="markerr" v-bind:greenFlag="true"></custom-marker>
      </marker>
    </map-view>
    <swipe :markers="markers" v-bind:tapped="tappedMarker" :top="top" :activePageMethod="(activePage)=>{onSwipedCardChangeMarker(activePage);}"/>
	</view>
</template>
 
<script>
import React from "react";
import MapView, {
  ProviderPropType,
  Animated as AnimatedMap,
  AnimatedRegion,
  Marker
} from "react-native-maps";
import { Dimensions, Animated, Easing } from "react-native";
import customMarker from "./customMarker";
import swipe from "./swipe";

const { width, height } = Dimensions.get("window");
const ITEM_SPACING = 10;
const ITEM_PREVIEW = 10;
const LATITUDE = 12.9107;
const LONGITUDE = 77.6018;

export default {
  data: function() {
    return {
      initialCoords: {
        latitude: 12.9107,
        longitude: 77.6018,
        latitudeDelta: 0.0122,
        longitudeDelta: 0.0221
      },
      animatedValue: 0,
      top: height - 150 - 64,
      toggleFlag: true,
      width: 0,
      height: 0,
      ITEM_PREVIEW_HEIGHT: 150,
      tappedMarker: 0,
      swipedToMarker: 0,
      markers: [
        {
          id: 0,
          amount: 99,
          coordinate: {
            latitude: LATITUDE,
            longitude: LONGITUDE
          },
          imageSrc: "./assets/image.jpg",
          cost: 500,
          data: {
            title: "The Pavillion",
            rating: 3,
            address: "154 Baker Street, Lodon A1",
            description:
              "The best in town.The best in town.The best in town.The best in town.The best in town.The best in town.The best in town.",
            food: 4,
            price: 5,
            location: 2
          }
        },
        {
          id: 1,
          amount: 199,
          coordinate: {
            latitude: LATITUDE + 0.004,
            longitude: LONGITUDE - 0.004
          },
          imageSrc: "./assets/image1.jpg",
          cost: 600,
          data: {
            title: "The Pavillion1",
            rating: 3,
            address: "154 Baker Street, Lodon A2",
            description:
              "The best in town.The best in town.The best in town.The best in town.The best in town.The best in town.The best in town.",
            food: 4,
            price: 5,
            location: 2
          }
        },
        {
          id: 2,
          amount: 199,
          coordinate: {
            latitude: LATITUDE + 0.008,
            longitude: LONGITUDE - 0.008
          },
          imageSrc: "./assets/image.jpg",
          cost: 700,
          data: {
            title: "The Pavillion3",
            rating: 3,
            address: "154 Baker Street, Lodon A3",
            description:
              "The best in town.The best in town.The best in town.The best in town.The best in town.The best in town.The best in town.",
            food: 4,
            price: 5,
            location: 2
          }
        }
      ]
    };
  },
  created: function() {
    this.width = width;
    this.height = height;
    this.animatedValue = new Animated.Value(0);
  },
  methods: {
    onSwipedCardChangeMarker: function(activePage) {
      this.tappedMarker = activePage;
      this.swipedToMarker = activePage;
    },
    tappedMarkerMethod: function(markerr) {
      this.tappedMarker = markerr.id;
      this.swipedToMarker = markerr.id;
    },
    startAnimation: function() {
      this.animate();
      if (this.toggleFlag) {
        this.top = this.animatedValue.interpolate({
          inputRange: [0, 1],
          outputRange: [height - 150 - 64, 1000]
        });
        this.toggleFlag = false;
      } else {
        this.top = this.animatedValue.interpolate({
          inputRange: [0, 1],
          outputRange: [1000, height - 150 - 64]
        });
        this.toggleFlag = true;
      }
    },
    animate: function() {
      this.animatedValue.setValue(0);
      Animated.timing(this.animatedValue, {
        toValue: 1,
        duration: 1000,
        easing: Easing.linear
      }).start();
    }
  },
  components: { MapView, swipe, Marker, customMarker }
};
</script>
<style>
.container {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
.wrapper-view {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
</style>
