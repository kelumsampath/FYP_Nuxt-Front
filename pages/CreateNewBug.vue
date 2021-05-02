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
        Great! Your message has been sent successfully. I will try to respond
        quickly.
      </div>
      <form
        v-else
        v-on:submit.prevent="sendBugReport"
        class="grid grid-cols-1 gap-y-6"
      >
        <div v-if="errored" class="rounded bg-red-200 text-lg p-4">
          Bummer, Something went wrong. Did you fill out all of the fields?
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
              ></textarea>
            </div>
          </div>
        </div>
        <div>
          <label for="phone" class="sr-only">Phone</label>
          <div class="relative rounded-md shadow-sm">
            <input
              v-model="phone"
              name="phone"
              id="phone"
              class="form-input block w-full py-3 px-4 placeholder-gray-500 transition ease-in-out duration-150"
              placeholder="Phone"
            />
          </div>
        </div>

        <div class="">
          <span class="inline-flex rounded-md shadow-sm">
            <button
              type="submit"
              class="inline-flex justify-center py-3 px-6 border border-transparent text-base leading-6 font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-500 focus:outline-none focus:border-indigo-700 focus:shadow-outline-indigo active:bg-indigo-700 transition duration-150 ease-in-out"
            >
              {{ loading ? "Sending Message..." : "Submit" }}
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
      email: "",
      phone: "",
      description: "",
    };
  },
  methods: {
    sendBugReport() {
       let axiosConfig = {
      headers: {
          'Content-Type': 'application/json;'
      }
    };
      this.loading = true;
      this.$axios
        .$post("/bug", {
          summary: this.summary,
          developers:[2,3],
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