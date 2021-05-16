<template>
  <div>
    <v-row>
      <v-col class="text-center">
        <h1>Create New Bug</h1>
      </v-col>
    </v-row>
    <div
      style="
        margin: 0% 10%;
        border: 3px solid white;
        padding: 2% 10%;
        background-color: #845460;
      "
    >
      <div v-if="success" class="rounded bg-indigo-500 text-white text-lg p-4">
        Bug successfully created!
      </div>
      <form
        v-else
        v-on:submit.prevent="sendBugReport"
        class="grid grid-cols-1 gap-y-6"
      >
        <div v-if="errored" class="rounded bg-red-200 text-lg p-4">
          Something went wrong!
        </div>
        <v-row>
        <v-col>
        <div>
          <label for="summar_txt" class="sr-only">Summary</label>
          <div class="relative rounded-md shadow-sm">
            <input
              v-model="summary"
              required
              name="summary"
              id="summar_txt"
              class="form-input block w-full py-3 px-4 placeholder-gray-500 transition ease-in-out duration-150"
              placeholder="Summary*"
              style="width: 100%; background-color: white"
            />
          </div>
        </div><br/>
        <div>
          <div>
            <label for="description" class="sr-only">Description</label>
            <div class="relative rounded-md shadow-sm">
              <textarea
                required
                v-model="description"
                name="description"
                id="description"
                rows="4"
                class="form-input block w-full py-3 px-4 placeholder-gray-500 transition ease-in-out duration-150"
                placeholder="Description*"
                style="width: 100%; background-color: white"
              ></textarea>
            </div>
          </div>
        </div><br/>

        <div>
          <label for="stroypoint" class="sr-only">Stroy Point</label>
          <div class="relative rounded-md shadow-sm">
            <input
              v-model="stroypoint"
              name="stroypoint"
              id="stroypoint"
              class="form-input block w-full py-3 px-4 placeholder-gray-500 transition ease-in-out duration-150"
              placeholder="stroypoint*"
              style="width: 100%; background-color: white"
            />
          </div>
        </div><br/>

    <!-- <label for="selectedDevs" class="sr-only">Assign to:</label>
        <v-container fluid style="margin-left:10%">
          <v-checkbox
            v-for="dev in developers"
            :key="dev[0]"
            @click="
              () => {
                selectDevs(dev[0]);
              }
            "
            :label="dev[1]"
            :value="dev[0]"
          ></v-checkbox>
        </v-container> -->

        <div class="">
          <span
            class="inline-flex rounded-md shadow-sm"
            style="margin-left: 40%"
          >
            <button
              @click="
                () => {
                  this.summary = '';
                  this.description = '';
                }
              "
              style="
                background: #a7bbc7;
                padding: 3%;
                border-radius: 8px;
                margin-top: 5%;
              "
              class="inline-flex justify-center py-3 px-6 border border-transparent text-base leading-6 font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-500 focus:outline-none focus:border-indigo-700 focus:shadow-outline-indigo active:bg-indigo-700 transition duration-150 ease-in-out"
            >
              Clear
            </button>
            <button
              type="submit"
              style="background: #a7bbc7; padding: 3%; border-radius: 8px"
              class="inline-flex justify-center py-3 px-6 border border-transparent text-base leading-6 font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-500 focus:outline-none focus:border-indigo-700 focus:shadow-outline-indigo active:bg-indigo-700 transition duration-150 ease-in-out"
            >
              {{ loading ? "Creating..." : "Create" }}
            </button>
          </span>
        </div>
        </v-col>
        <v-col>
        <label for="selectedDevs" class="sr-only">Assign to:</label>
        <v-container fluid style="margin-left:10%">
          <!-- <p>{{ selected }}</p> -->
          <v-checkbox
            v-for="dev in developers"
            :key="dev[0]"
            @click="
              () => {
                selectDevs(dev[0]);
              }
            "
            :label="dev[1]"
            :value="dev[0]"
          ></v-checkbox>
        </v-container>
        
        </v-col>
        </v-row>
      </form>
    </div>
  </div>
</template>
<script>
import VueLodash from 'vue-lodash'
import lodash from 'lodash'
export default {
  data() {
    return {
      loading: false,
      success: false,
      errored: false,
      summary: "",
      stroypoint:null,
      developers: [],
      description: "",
      selectedDevs:[],
    };
  },
  fetch() {
    this.$axios
      .$get("/developers")
      .then((response) => {
        console.log(response);
        this.developers = response;
      })
      .catch((error) => {})
      .finally(() => {});
  },
  methods: {
    sendBugReport() {
      this.loading = true;
      this.$axios
        .$post("/bug", {
          summary: this.summary,
          developers: this.selectedDevs,
          description: this.description,
          stroypoint:this.stroypoint,
        })
        .then((response) => {
          this.success = true;
          this.errored = false;
        })
        .catch((error) => {
          this.errored = true;
        })
        .finally(() => {
          this.loading = false;
        });
    },
    selectDevs(id){
          if(this.selectedDevs.includes(id)){
              this.selectedDevs=_.without(this.selectedDevs,id)
          }else{
              this.selectedDevs.push(id)
          }
      }
  },
};
</script>