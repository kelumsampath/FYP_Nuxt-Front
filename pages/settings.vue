<template>
  <v-row justify="center" align="center">
    <h1>Prediction Model Configurations</h1>
    <div
      style="
        margin: 0% 10%;
        border: 3px solid white;
        padding: 2% 5%;
        min-width: 80%;
        background-color: #845460;
      "
    >
      <div>
        <h3>Train new prediction model:</h3>
        <div class="container">
          <div class="large-12 medium-12 small-12 cell">
            <label
              >Select CSV File
              <input
                type="file"
                id="file"
                ref="file"
                v-on:change="handleFileUpload()"
              />
            </label>
            <button
              v-on:click="submitFile()"
              style="background: green; padding: 1%; border-radius: 8px"
            >
              {{ Training ? "Training..." : "Train" }}
            </button>
          </div>
        </div>
      </div>

      <div>
        <v-container class="grey lighten-5">
          <v-row class="mb-6" no-gutters>
            <v-col sm="5" md="6">
              <v-card class="pa-2" outlined tile> Accurecy Meassures 1</v-card>
              <p style="color:black;">Mean Absolute Error: {{this.AccuracyMeassures1["Mean Absolute Error"]}}</p>
              <p style="color:black;">Mean Squared Error: {{this.AccuracyMeassures1["Mean Squared Error"]}}</p>
              <p style="color:black;">Root Mean Squared Error: {{this.AccuracyMeassures1["Root Mean Squared Error"]}}</p>
            </v-col>
            <v-col sm="5" offset-sm="2" md="6" offset-md="0">
              <v-card class="pa-2" outlined tile> Accurecy Meassures 2</v-card>
              <p style="color:black;">Mean Absolute Error: {{this.AccuracyMeassures2["Mean Absolute Error"]}}</p>
              <p style="color:black;">Mean Squared Error: {{this.AccuracyMeassures2["Mean Squared Error"]}}</p>
              <p style="color:black;">Root Mean Squared Error: {{this.AccuracyMeassures2["Root Mean Squared Error"]}}</p>
            </v-col>
          </v-row>
        </v-container>
      </div>

    <div v-if="load" style="margin-top:5%;">
        <chart :lables="label" :actual="actual" :textScore="textScore" :predictedSp="predictedSp"></chart>
    </div>
    <br/>
    <p ><v-icon style="color:#FDF50E">mdi-checkbox-blank-circle</v-icon>Actual Storypoint</p>
    <p><v-icon  style="color:#250EFD">mdi-checkbox-blank-circle</v-icon>Estimated Text-Score</p>
    <p ><v-icon style="color:#42b883">mdi-checkbox-blank-circle</v-icon>Estimated Storypoint</p>
    </div>
  </v-row>
</template>

<script>
import chart from '@/components/chart';
export default {
  data() {
    return {
      file: "",
      Training:false,
      load:false,
      AccuracyMeassures1: {},
      AccuracyMeassures2: {},
      label:[],
      actual:[],
      textScore:[],
      predictedSp:[],
    };
  },
  
  fetch() {
    this.$axios
      .$get("/accuracy")
      .then((response) => {
        this.AccuracyMeassures1 = response.AccuracyMeassures1;
        this.AccuracyMeassures2 = response.AccuracyMeassures2;
      })
      .catch((error) => {})
      .finally(() => {});

      this.$axios
      .$get("/stats")
      .then((response) => {
        this.label = response.id;
        this.actual = response.actual;
        this.textScore = response.textscore;
        this.predictedSp = response.predicted;
        this.load=true
        console.log(this.label)
      })
      .catch((error) => {})
      .finally(() => {});
  },

  methods: {
   
    submitFile() {
      this.Training=true
      let formData = new FormData();
      formData.append("file", this.file);
      this.$axios
        .post("/trainmodel", formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        })
        .then((response) => {
          console.log(response.data);
          this.AccuracyMeassures1 = response.data.AccuracyMeassures1;
          this.AccuracyMeassures2 = response.data.AccuracyMeassures2;
          console.log(this.AccuracyMeassures1["Mean Absolute Error"]);
        })
        .catch(function () {
          console.log("FAILURE!!");
        })
        .finally(() => {this.Training=false});
    },

    handleFileUpload() {
      this.file = this.$refs.file.files[0];
    },
  },
};
</script>