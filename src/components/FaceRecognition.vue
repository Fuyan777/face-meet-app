<template>
  <div class="hello">
    <button @click="tapButton()">クリックしてください</button>
  </div>
</template>

<script>
import * as facemesh from "@tensorflow-models/facemesh";
// import * as tf from "@tensorflow/tfjs-core";
import "@tensorflow/tfjs-backend-webgl";
export default {
  method: {
    tapButton: function () {
      //consoleエラー
      console.log("aaaaaaaaaaaa");
    },
    setup: async function () {
      const model = await facemesh.load();

      const predictions = await model.estimateFaces(
        document.querySelector("video")
      );

      if (predictions.length > 0) {
        for (let i = 0; i < predictions.length; i++) {
          const keypoints = predictions[i].scaledMesh;

          // Log facial keypoints.
          for (let i = 0; i < keypoints.length; i++) {
            const [x, y, z] = keypoints[i];

            console.log(`Keypoint ${i}: [${x}, ${y}, ${z}]`);
          }
        }
      }
    },
    async getMedia() {
      let stream = null;
      let constraints = navigator.mediaDevices.getSupportedConstraints();

      try {
        stream = await navigator.mediaDevices.getUserMedia(constraints);
        console.log(stream);
      } catch (err) {
        console.log(err);
      }
    },
  },
  created() {
    // console.log("getMedia");
    // this.getMedia();
    // console.log("setup");
    // this.setup();
    // console.log("created end");
  },
};
</script>

<style>
</style>