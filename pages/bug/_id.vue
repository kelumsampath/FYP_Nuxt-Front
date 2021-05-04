<template>
  <v-row justify="center" align="center">
    <div
      v-if="bug[0] !== undefined"
      style="
        margin: 5% 10%;
        border: 3px solid white;
        padding: 2% 2%;
        min-width: 80%;
        background-color: #845460;
      "
    >
      <div>
        <p>Bug Id: {{ bug[0].Id }}</p>
        <h1>{{ bug[0].Summary }}</h1>
        <h3>{{ bug[0].Description }}</h3>
        <h3>Story point :{{ bug[0].StroyPoint }}</h3>
        <h3>Predicted Story Point: {{ bug[0].PredictedStoryPoint }}</h3>
        
      </div>
    </div>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      id: this.$route.params.id,
      bug: [],
      Comments: [],
      developers: [],
    };
  },
  async fetch() {
    this.$axios
      .$get("/bugreport/" + this.id)
      .then((response) => {
        console.log(response.bugreport);
        this.bug = response.bugreport;
        
        
      })
      .catch((error) => {})
      .finally(() => {});
  },
};
</script>
