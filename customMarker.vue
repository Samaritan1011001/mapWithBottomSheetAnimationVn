<template>
	<animated:view class="container" >
        <animated:view class="bubble" :style="{backgroundColor:background,borderColor:border}">
          <text class="dollar">$</text>
          <text class="amount" :style="{ fontSize: fontsize }">{{marker.cost}}</text>
        </animated:view>
        <view class="arrowBorder" :style="{borderTopColor:border}"/>
        <view class="arrow" :style="{borderTopColor:background}"/>
      </animated:view>
</template>
 
 <script>
import React from "react";
import {
  StyleSheet,
  View,
  Dimensions,
  Animated,
  PanResponder,
  TouchableOpacity,
  Easing
} from "react-native";

const { width, height } = Dimensions.get("window");

export default {
  data: function() {
    return {
      fontsize: 13,
      background: "#ff5a5f",
      border: "#ff5a5f"
    };
  },
  props: {
    marker: Object,
    greenFlag: Boolean
  },
  created: function() {
    // console.log("this.selectedMarker: ", this.selectedMarker);
    if (this.greenFlag) {
      this.background = "green";
      this.border = "green";
    } else {
      this.background = "#ff5a5f";
      this.border = "#ff5a5f";
    }
  },
  methods: {},
  components: {}
};
</script>
<style>
.container {
  flex-direction: column;
  align-self: center;
}
.bubble {
  width: 40;
  height: 30;
  flex: 0;
  flex-direction: row;
  align-self: center;
  background-color: #ff5a5f;
  padding: 2;
  border-radius: 3;
  border-color: #d23f44;
  border-width: 0.5;
}
.dollar {
  color: #ffffff;
  font-size: 10;
}
.amount {
  color: #ffffff;
  font-size: 13;
}
.arrow {
  background-color: transparent;
  border-width: 4;
  border-color: transparent;
  border-top-color: #ff5a5f;
  align-self: center;
  margin-top: -9;
}
.arrowBorder {
  background-color: transparent;
  border-width: 4;
  border-color: transparent;
  border-top-color: #d23f44;
  align-self: center;
  margin-top: -0.5;
}
</style>
