<template>
  <v-row justify="center" align="center">
    <h1>Bug Reports</h1>
    <div style="
        margin: 0% 10%;
        border: 3px solid white;
        padding: 2% 10%;
        min-width: 80%;
        background-color: #845460;
      ">
   <v-data-table dense :headers="headers" :items="bugs" class="elevation-1" @click:row="openBug">
      
    </v-data-table>
    </div>
  </v-row>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  data() {
    return {
      bugs:[],
      headers : [
    { text: "id", value: "Id" },
    { text: "Summary", value: "Summary" },
    { text: "StroyPoint", value: "StroyPoint" },
    { text: "Predicted SP", value: "PredictedStoryPoint" }
  ],
    };
  },
  async fetch(){
    this.$axios.$get("/bug")
        .then((response) => {
          console.log(response.bugs)
          this.bugs=response.bugs
        })
        .catch((error) => {
          
        })
        .finally(() => {
          
        });
  },
  components: {
    Logo,
    VuetifyLogo
  },
  methods: {
    openBug(value){
      console.log(value.Id)
      this.$router.push({ path: `/bug/${value.Id}` })
    }

  }
}
</script>
