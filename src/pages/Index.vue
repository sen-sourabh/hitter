<!-- <template>
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
              dark
              class="text-grey "
              active-color="primary"
              indicator-color="primary"
              align="justify"
              narrow-indicator
            >
              <q-tab name="config" label="Config" />
              <q-tab name="headers" label="Headers" />
              <q-tab name="request" label="Request" />
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
            </q-tab-panels>
          </q-card>
        </div>
      </div>
    </div>
  </div>
</template>  -->

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
    <div>
      <q-splitter
        v-model="splitterModel"
        style="height: 500px;border: 1px solid lightblue;"
      >

        
        <template v-slot:before>
          <q-splitter
            v-model="insideModel"
            horizontal
          >

            <template v-slot:before>
              <div class="q-pa-md">
                <div class="text-h4 q-mb-md">Before</div>
                <div v-for="n in 20" :key="n" class="q-my-md">{{ n }}. Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quis praesentium cumque magnam odio iure quidem, quod illum numquam possimus obcaecati commodi minima assumenda consectetur culpa fuga nulla ullam. In, libero.</div>
              </div>
            </template>

            <template v-slot:after>
              <div class="q-pa-md">
                <div class="text-h4 q-mb-md">After</div>
                <div v-for="n in 20" :key="n" class="q-my-md">{{ n }}. Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quis praesentium cumque magnam odio iure quidem, quod illum numquam possimus obcaecati commodi minima assumenda consectetur culpa fuga nulla ullam. In, libero.</div>
              </div>
            </template>

          </q-splitter>
        </template>
        
        
        
        <template v-slot:after>
          <div class="q-pa-md">
            <div class="text-h4 q-mb-md">Response</div>
            <!-- <div > -->
              <pre class="q-my-md formatter" style="display:none;" id="pre-text"></pre>
            <!-- </div> -->
          </div>
        </template>

        

      </q-splitter>
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
      splitterModel: 50, // start at 50%
      insideModel: 50
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
        document.getElementById("pre-text").style.display = "block";
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
            document.getElementById("pre-text").innerHTML = this.syntaxHighlight(error);
          });
      }
      console.log("Got URL: ", this.request_type, " | ", this.request_url);
    },
    syntaxHighlight(json) {
      if (typeof json != "string") {
        json = JSON.stringify(json, undefined, 4);
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
          } else if (/Error:/.test(match)) {
            cls = "error";
          }
          return '<span class="' + cls + '">' + match + "</span>";
        }
      );
      console.log(jb);
      // this.text = jb;
      return jb;
    },
  },
};
</script>