<template>
  <v-container fluid>
    <p>{{selected }}</p>
    <v-checkbox
      v-for="dev in ab" :key="dev[0]"
      @click="()=>{selectDevs(dev[0])}"
      :label="dev[1]"
      :value="dev[0]"
    ></v-checkbox>
    
  </v-container>
</template>

<script>
import VueLodash from 'vue-lodash'
import lodash from 'lodash'
export default {
    
    data() {
    return {
      ab:[[1,"dd"],[2,"ddd"],[3,"kksse"],[4,"kske"]] ,
      selected:[],
    };
  },
  methods: {
      selectDevs(id){
          if(this.selected.includes(id)){
              this.selected=_.without(this.selected,id)
          }else{
              this.selected.push(id)
          }
      }
  }
}
</script>
// v-model="selected"