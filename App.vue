<template>
  <view class="container">
    <text>Location:</text>
    <text>{{location.latitude}}</text>
    <touchable-opacity :on-press="getLocation" >
        <text>get location</text>
    </touchable-opacity>
    <text>{{ errorMessage }}</text>
  </view>
</template>

 <script>
import Constants from "expo-constants";
import * as Location from "expo-location";
import * as Permissions from "expo-permissions";

export default {
  data: function() {
    return {
      location: {},
      errorMessage: ""
    };
  },
  methods: {
    getLocation: function() {
      Permissions.askAsync(Permissions.LOCATION).then(status => {
        if (status !== "granted") {
          this.errorMessage = "Permission to access location was denied";
        }
        Location.getCurrentPositionAsync({}).then(location1 => {
          this.location = location1;
        });
      }).catch((err)=>{
        console.log(err);
     });
    }
  }
};
</script>

<style>
.container {
  background-color: white;
  align-items: center;
  justify-content: center;
  flex: 1;
}
.text-color-primary {
  color: blue;
}
</style>
