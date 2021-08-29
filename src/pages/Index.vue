<template>
  <div>
    <div class="row">
      <div class="col-12 col-md-2">
        <q-select
          dense
          outlined
          v-model="request_type"
          :options="request_options"
        />
      </div>
      <div class="col-12 col-md-8">
        <q-input
          dense
          outlined
          v-model="request_url"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || '']"
          placeholder="https://www.example.com"
          class="expectedRequestURL"
        />
      </div>
      <div class="col-12 col-md-2">
        <q-btn
          size="md"
          color="primary"
          label="Send"
          @click="getRequestUrl"
          icon-right="send"
          class="full-width"
        />
      </div>
    </div>
    <div class="row">
      <div class="content-panel">
        <div class="q-gutter-y-md data-panel">
          <q-card>
            <q-tabs
              v-model="tab"
              dense
              class="text-grey"
              active-color="primary"
              indicator-color="primary"
              align="justify"
              narrow-indicator
            >
              <q-tab name="config" label="Config" />
              <q-tab name="headers" label="Headers" />
              <q-tab name="request" label="Request" />
              <q-tab name="response" label="Response" />
            </q-tabs>

            <q-separator />

            <q-tab-panels v-model="tab" animated>
              <q-tab-panel name="config">
                Lorem ipsum dolor sit amet consectetur adipisicing elit.
              </q-tab-panel>

              <q-tab-panel name="headers">
                Lorem ipsum dolor sit amet consectetur adipisicing elit.
              </q-tab-panel>

              <q-tab-panel name="request">
                Lorem ipsum dolor sit amet consectetur adipisicing elit.
              </q-tab-panel>

              <q-tab-panel name="response">
                <div class="output-response">
                  <pre id="pre-text"></pre>
                  <!-- <pre
                    >{{ text.length != 0 ? syntaxHighlight2(text) : "" }} </pre
                  > -->
                </div>
              </q-tab-panel>
            </q-tab-panels>
          </q-card>
        </div>
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
      request_url: "https://pegasusapi.genesisapps.in/holidays/getAllHolidays",
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
      tab: "request",
      text: [],
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
      this.text = [];
      if (this.request_type.label == "GET") {
        axios
          .get(this.request_url)
          .then((response) => {
            console.log(response);
            // let str = JSON.stringify(response.data, null, 2);
            document.getElementById("pre-text").innerHTML =
              this.syntaxHighlight(response.data);
          })
          .catch((error) => {
            console.log(error);
            document.getElementById("pre-text").innerHTML = "";
          });
      }
      console.log("Got URL: ", this.request_type, " | ", this.request_url);
    },
    syntaxHighlight(json) {
      if (typeof json != "string") {
        json = JSON.stringify(json, undefined, 2);
      }
      json = json
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;");
      let jb = json.replaceAll(
        /("(\\u[a-zA-Z0-9]{4}|\\[^u]|[^\\"])*"(\s*:)?|\b(true|false|null)\b|-?\d+(?:\.\d*)?(?:[eE][+\-]?\d+)?)/g,
        function (match) {
          console.log("hello");
          var cls = "number";
          if (/^"/.test(match)) {
            if (/:$/.test(match)) {
              cls = "key";
            } else {
              cls = "string";
            }
          } else if (/true|false/.test(match)) {
            cls = "boolean";
          } else if (/null/.test(match)) {
            cls = "null";
          }
          return '<span class="' + cls + '">' + match + "</span>";
        }
      );
      console.log(jb);
      // this.text = jb;
      return jb;
    },
    syntaxHighlight2(json) {
      if (typeof json != "string") {
        json = JSON.stringify(json, undefined, 2);
      }
      console.log(json);
      let jb = json.replaceAll(
        /("(\\u[a-zA-Z0-9]{4}|\\[^u]|[^\\"])*"(\s*:)?|\b(true|false|null)\b|-?\d+(?:\.\d*)?(?:[eE][+\-]?\d+)?)/g,
        function (match) {
          console.log("hello");
          var cls = "number";
          if (/^"/.test(match)) {
            if (/:$/.test(match)) {
              cls = "key";
            } else {
              cls = "string";
            }
          } else if (/true|false/.test(match)) {
            cls = "boolean";
          } else if (/null/.test(match)) {
            cls = "null";
          }
          return '<span class="' + cls + '">' + match + "</span>";
        }
      );
      console.log(jb);
      return jb;
    },
  },
};
</script>