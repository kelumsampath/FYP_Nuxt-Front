<template>
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
    <p>Bug Id: {{ bug[0].Id }}</p>
    <h1>{{ bug[0].Summary }}</h1>
    <br />
    <v-container>
      <v-row no-gutters>
        <v-col cols="12" sm="6" md="8" style="text-align: justify">
          <h3>{{ bug[0].Description }}</h3>
          <br />
          <h3>Comments Story Point:</h3>
          <h4
            v-for="cmnt in bug[0].Comment"
            :key="cmnt"
            style="margin-left: 10%"
          >
            {{ cmnt }}
            <v-icon light v-on:click="deleteComment(cmnt)">
              mdi-delete
            </v-icon>
          </h4>
          <form v-on:submit.prevent="comment" class="grid grid-cols-1 gap-y-6">
            <div v-if="errored" class="rounded bg-red-200 text-lg p-4">
              Something went wrong!
            </div>

            <v-container>
              <v-row no-gutters>
                <v-col cols="12" sm="6" md="8" style="text-align: justify">
                  <div>
                    <div class="relative rounded-md shadow-sm">
                      <textarea
                        required
                        v-model="cmnt"
                        name="cmnt"
                        id="cmnt"
                        rows="4"
                        class="form-input block w-full py-3 px-4 placeholder-gray-500 transition ease-in-out duration-150"
                        style="width:100%;margin-left: 10%; background-color:white;"
                        placeholder="Post a comment*"
                      ></textarea>
                    </div>
                  </div>
                </v-col>
                <v-col cols="6" md="4" style="padding-left: 3%">
                  <div class="">
                    <span class="inline-flex rounded-md shadow-sm">
                      <button
                        type="submit"
                        style="background: #a7bbc7; margin-left: 5%; border-radius: 8px"
                        class="inline-flex justify-center py-3 px-6 border border-transparent text-base leading-6 font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-500 focus:outline-none focus:border-indigo-700 focus:shadow-outline-indigo active:bg-indigo-700 transition duration-150 ease-in-out"
                      >
                        {{ loading ? "Posting..." : "Post" }}
                      </button>
                    </span>
                  </div>
                </v-col>
              </v-row>
            </v-container>
          </form>
        </v-col>
        <v-col cols="6" md="4" style="padding-left: 3%">
          <h3>Developers:</h3>
          <h4 v-for="dev in bug[0].Name" :key="dev" style="margin-left: 10%">
            {{ dev }}
          </h4>
          <br /><br />
          <h3>Story point :{{ bug[0].StroyPoint }}</h3>
          <h3>Predicted Story Point: {{ bug[0].PredictedStoryPoint }}</h3>
          <button
            v-on:click="gensp()"
            style="background: #a7bbc7; padding: 3%; border-radius: 8px"
          >
            {{ genarating ? "Genarating..." : "Genarate" }}
          </button>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id: this.$route.params.id,
      bug: [],
      Comments: [],
      developers: [],
      loading: false,
      genarating:false,
      success: false,
      errored: false,
      cmnt: "",
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
  methods: {
    gensp() {
      this.genarating=true
      this.$axios
        .$post("/storypointgen", {
          bug: this.bug[0],
        })
        .then((response) => {
          this.$fetch();
        })
        .catch((error) => {})
        .finally(() => {
          this.genarating=false
        });
    },
    comment() {
      this.loading = true;
      this.$axios
        .$post("/comment", {
          Id: this.bug[0].Id,
          comment: this.cmnt,
        })
        .then((response) => {
          this.success = true;
          this.errored = false;
          this.cmnt=""
          this.$fetch();
        })
        .catch((error) => {
          this.errored = true;
        })
        .finally(() => {
          this.loading = false;
        });
    },
    deleteComment(cmnt) {
      this.$axios
        .$post("/deletecomment", {
          bugId:this.bug[0].Id,
          comment: cmnt,
        })
        .then((response) => {

          this.$fetch();
        })
        .catch((error) => {})
        .finally(() => {});
    },
  },
};
</script>
