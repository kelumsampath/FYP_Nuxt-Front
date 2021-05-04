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
              >File
              <input
                type="file"
                id="file"
                ref="file"
                v-on:change="handleFileUpload()"
              />
            </label>
            <button v-on:click="submitFile()">Submit</button>
          </div>
        </div>
      </div>
    </div>
  </v-row>
</template>

<script>
  export default {
    
    data(){
      return {
        file: ''
      }
    },

    methods: {
      /*
        Submits the file to the server
      */
      submitFile(){
        /*
                Initialize the form data
            */
            let formData = new FormData();

            /*
                Add the form data we need to submit
            */
            formData.append('file', this.file);

        /*
          Make the request to the POST /single-file URL
        */
            this.$axios.post( '/trainmodel',
                formData,
                {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
              }
            ).then(function(){
          console.log('SUCCESS!!');
        })
        .catch(function(){
          console.log('FAILURE!!');
        });
      },

      /*
        Handles a change on the file upload
      */
      handleFileUpload(){
        this.file = this.$refs.file.files[0];
      }
    }
  }
</script>