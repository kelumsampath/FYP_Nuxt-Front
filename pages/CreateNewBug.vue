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
        <div>
          <label for="summar_txt" class="sr-only">Summary*</label>
          <div class="relative rounded-md shadow-sm">
            <input
              v-model="summary"
              required
              name="summary"
              id="summar_txt"
              class="form-input block w-full py-3 px-4 placeholder-gray-500 transition ease-in-out duration-150"
              placeholder="Summary*"
              style="width:40%;margin-left: 10%; background-color:white;"
            />
          </div>
        </div>
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
                style="width:40%;margin-left: 10%; background-color:white;"
              ></textarea>
            </div>
          </div>
        </div>
        <div>
          <label for="selectedDevs" class="sr-only">Developers</label>
          <div class="relative rounded-md shadow-sm">
            <input
              v-model="selectedDevs"
              name="selectedDevs"
              id="selectedDevs"
              class="form-input block w-full py-3 px-4 placeholder-gray-500 transition ease-in-out duration-150"
              placeholder="Developer*"
              style="width:40%;margin-left: 10%; background-color:white;"
            />
          </div>
        </div>

        <div class="">
          <span class="inline-flex rounded-md shadow-sm" style="margin-left:40%; ">
            <button
               @click="()=>{this.summary='';this.description='';this.selectedDevs=[]}"
               style="background: #a7bbc7; padding: 3%; border-radius: 8px;margin-top:5%"
              class="inline-flex justify-center py-3 px-6 border border-transparent text-base leading-6 font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-500 focus:outline-none focus:border-indigo-700 focus:shadow-outline-indigo active:bg-indigo-700 transition duration-150 ease-in-out"
            >
              Clear
            </button>
            <button
              type="submit"
               style="background: #a7bbc7; padding: 3%; border-radius: 8px; "
              class="inline-flex justify-center py-3 px-6 border border-transparent text-base leading-6 font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-500 focus:outline-none focus:border-indigo-700 focus:shadow-outline-indigo active:bg-indigo-700 transition duration-150 ease-in-out"
            >
              {{ loading ? "Creating..." : "Create" }}
            </button>
            
          </span>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      loading: false,
      success: false,
      errored: false,
      summary: "",
      developers: [],
      selectedDevs:"",
      description: "",
    };
  },
   fetch(){
    this.$axios.$get("/developers")
        .then((response) => {
          console.log(response)
          developers=response
        })
        .catch((error) => {
          
        })
        .finally(() => {
          
        });
  },
  methods: {
    sendBugReport() {
      this.loading = true;
      this.$axios
        .$post("/bug", {
          summary: this.summary,
          developers: this.selectedDevs.split(",").map(Number),
          description: this.description,
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
  },
};
</script>