<template>
  <div>
    <v-container mt-5 v-show="!scan" fluid fill-height>
      <v-layout align-center justify-center>
        <v-jumbotron
          mt-5
          align-center
          justify-center
          class="elevation-10"
          gradient="to top right, rgba(63,81,181, .7), rgba(25,32,72, .7)"
          dark
          src="https://cdn.vuetifyjs.com/images/parallax/material2.jpg"
        >
          <v-container mt-2 fill-height>
            <v-layout align-center>
              <v-flex>
                <h3 class="display-3">Welcome to BreakHis Scanner</h3>
                <h3 class="display-1">A Breast Cancer Histopathological Image Classifier</h3>
                <span
                  class="subheading"
                >This application is consuming a model built from the breakhis DataSet wich contains more than 7000 histological cuts scan images helping to classify tumors into Malignant or benign .</span>

                <v-divider class="my-3"></v-divider>

                <div class="title mb-3">Click here to import an image !</div>

                <v-btn class="mx-0" color="pink" large dark @click="onPickFile">Import image</v-btn>
              </v-flex>
            </v-layout>
          </v-container>
        </v-jumbotron>
      </v-layout>
    </v-container>

    <v-container mt-4 v-show="scan" grid-list-md text-xs-center>
      <input
        type="file"
        style="display:none"
        ref="fileInput"
        accept="image/*"
        @change="onFilePicked"
      >

      <v-layout row wrap>
        <v-flex xs12>
          <v-card color="blue lighten-1" class="white--text">
            <div class="headline">Breast histological cut</div>
          </v-card>
          <v-card color="blue lighten-1" class="white--text" mt-2>
            <v-img
              height="400px"
              v-show="scan"
              style="margin: 0rem"
              :src="imageURL"
              aspect-ratio="1.25"
            ></v-img>

            <v-card-actions>
              <v-btn raised class="primary" @click="onPickFile">Upload image</v-btn>

              <v-btn dark raised color="red" v-show="scan" @click="scanImage">Scan image</v-btn>
              <v-progress-circular v-show="scannig" :size="30" :width="4" color="red" indeterminate></v-progress-circular>
            </v-card-actions>
          </v-card>
        </v-flex>

        <v-flex xs12 v-show="!scan" text-xs-center>
          <v-card color="blue lighten-5">
            <v-card-title primary-title>
              <div>
                <div v-if="malign > 0" class="headline">Malignant</div>
                <div v-if="benin > 0" class="headline">Benign</div>

                <span>
                  <br>Please select a valid histological cut image by clicking on the button up here
                  You will be able after that to analyse it ^_^
                </span>
              </div>
            </v-card-title>
          </v-card>
        </v-flex>

        <v-flex xs3 v-show="scan">
          <v-card color="blue lighten-4">
            <v-card-title primary-title>
              <div>
                <div class="headline">Malignant</div>

                <v-progress-circular
                  style="margin: 0.5rem"
                  :value="this.malign"
                  :size="200"
                  :width="19"
                  color="red"
                >{{this.malign}} %</v-progress-circular>
              </div>
            </v-card-title>
            <v-card-actions></v-card-actions>
          </v-card>
        </v-flex>

        <v-flex xs6 v-show="scan">
          <v-card color="blue lighten-5">
            <v-card-title primary-title>
              <div>
                <div v-if="malign > 50" class="headline">Malignant</div>
                <div v-if="benin > 50" class="headline">Benign</div>

                <span v-if="malign == 0">
                  <br>Please select a valid histological cut image by clicking on the button up here
                  You will be able after that to analyse it ^_^
                </span>

                <span v-if="malign > 50">
                  <br>
                  This image has bean detected to be containing a Malignant tumour with {{malign}} %
                </span>
                <span v-if="benin > 50">
                  <br>
                  This image has bean detected to be Benign with {{benin}} %
                </span>
              </div>
            </v-card-title>
          </v-card>
        </v-flex>

        <v-flex xs3 v-show="scan">
          <v-card color="blue lighten-4">
            <v-card-title primary-title>
              <div>
                <div class="headline">Benign</div>
                <v-progress-circular
                  :value="this.benin"
                  :size="200"
                  :width="19"
                  color="green"
                  style="margin: 0.5rem"
                >{{this.benin}} %</v-progress-circular>
              </div>
            </v-card-title>
            <v-card-actions></v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>
<script>
import * as tf from "@tensorflow/tfjs";

export default {
  name: "Second",
  data() {
    return {
      scannig: false,
      num: 12,
      image: null,
      imageURL: "",
      scan: false,
      kerasTraindedModel: null,
      // KERAS_MODEL_JSON: "http://localhost:8081/model.json",
      KERAS_MODEL_JSON: "http://localhost:8080/model.json",
      malign: 0,
      benin: 0
    };
  },
  created: () => {},
  mounted() {
    this.loadtheModel();
  },
  methods: {
    onPickFile() {
      this.image = null;
      this.imageURL = "";
      this.$refs.fileInput.click();
    },
    onFilePicked(event) {
      const files = event.target.files;
      let filename = files[0].name;
      if (filename.lastIndexOf(".") <= 0) {
        return alert("Please join a valid file");
      }
      const fileReader = new FileReader();
      fileReader.addEventListener("load", () => {
        this.imageURL = fileReader.result;
      });
      fileReader.readAsDataURL(files[0]);
      this.image = files[0];
      this.scan = true;
    },
    async loadtheModel() {
      /* eslint-disable */
      console.log("Loading model");
      tf.loadLayersModel(this.KERAS_MODEL_JSON)
        .then(result => {
          console.log("Model Loaded");
          this.kerasTraindedModel = result;
          this.kerasTraindedModel.summary();
        })
        .catch(error => {
          console.log(error);
        });
    },
    /*,
     async load(){
          const Model_URL = 'http://localhost:8081/model.json';
          // eslint-disable
          console.log("loading model");
          const model = await tf.loadLayersModel(Model_URL);
          console.log("model loaded")
          
          return model;
        }*/ async scanImage() {
      this.scannig = true;
      var img = new Image();
      img.src = this.imageURL;

      let tensor = tf.browser
        .fromPixels(img)
        .resizeNearestNeighbor([224, 224])
        .toFloat()
        .div(255)
        .expandDims();

      //const prediction = this.kerasTraindedModel.predict(tf.reshape(img, shape = [1, 48, 48, 3]));
      const prediction = await this.kerasTraindedModel.predict(tensor);
      //prediction.dataSync()[0];
      for (let index = 0; index < 2000; index++) {
        const element = 0;
      }
      console.log(prediction.dataSync());

      this.scannig = false;
      this.benin = (prediction.dataSync()[0] * 100).toFixed(2);
      this.malign = (prediction.dataSync()[1] * 100).toFixed(2);
    }
  }
};
</script>
