<template>
  <div>
    <div class="row">
      <div class="col-2">
        <q-select
          dense
          outlined
          v-model="request_type"
          :options="request_options"
        />
      </div>
      <div class="col-8">
        <q-input
          dense
          outlined
          v-model="request_url"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || '']"
          placeholder="https://www.example.com"
        />
      </div>
      <div class="col-2">
        <q-btn
          size="md"
          color="primary"
          label="Send"
          @click="getRequestUrl"
          icon-right="send"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
// import { defineComponent } from "vue";

export default {
  name: "PageIndex",
  data() {
    return {
      request_type: { label: "GET", value: "GET" },
      request_url: "",
      request_options: [
        {
          label: "GET",
          value: "GET",
        },
        {
          label: "POST",
          value: "POST",
        },
        {
          label: "PUT",
          value: "PUT",
        },
        {
          label: "OPTIONS",
          value: "OPTIONS",
        },
        {
          label: "DELETE",
          value: "DELETE",
        },
      ],
    };
  },
  created() {
    console.log("Home page");
  },
  methods: {
    getRequestUrl() {
      console.log(this.request_url);
      if (!this.request_url) {
        alert("Please enter request URL.");
        return false;
      }
      if (this.request_type.label == "GET") {
        axios
          .get(this.request_url)
          .then((response) => {
            console.log(response);
          })
          .catch((error) => {
            console.log(error);
          });
      }
      console.log("Got URL: ", this.request_type, " | ", this.request_url);
    },
  },
};
</script>
