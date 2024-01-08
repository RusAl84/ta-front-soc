<template>
  <q-page>
    <q-card class="chatmessages">
      <div class="flexrow">
        <div>
          <q-card-section>
            <div class="text-h6">
              Найденные целевые сообщения социального инжиниринга:
            </div>
          </q-card-section>
          <q-card-section class="q-gutter-lg">
            <div>
              <q-select
                filled
                v-model="counts"
                :options="options"
                label="Нечеткость"
              />
            </div>
            <div>
              <q-btn color="primary" label="Поиск" @click="onFindCL" />
            </div>
          </q-card-section>
        </div>
      </div>
      <q-card-section v-if="ae_data.length > 0">
        <div v-for="(line, i) in ae_data" :key="i">
          <div>
            <b>Найденный фрагмент &nbsp; {{ i + 1 }}</b>
          </div>
          <div><b>Исходный текст сообщения:</b> {{ line.text }}</div>
          <div><b>Ключевые слова: </b>{{ line.KEYWORDS }}</div>
          <br />
        </div> </q-card-section
    ></q-card>
  </q-page>
</template>

<script>
import { defineComponent, ref } from "vue";
import axios from "axios";
import * as cfg from "../config.js";

export default {
  data() {
    return {
      file: "",
      hostae: cfg.hostae,
      hostae_uploadae: cfg.hostae_uploadae,
      chatmessages: "",
      proc_text: ref(""),
      counts: ref(2),
      resp_text: ref(""),
      resp_data: ref(""),
      all_data: ref([{}]),
      ae_data: ref([{}]),
      filename: ref(""),
      options: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
    };
  },
  methods: {
    handleFileUpload() {
      this.file = this.$refs.file.files[0];
    },
    fileUploaded({ files, xhr }) {
      let data = JSON.parse(xhr.response);
      this.chatmessages = data["text"];
      this.filename = data["filename"];
    },
    async onProc() {
      const response = await axios({
        method: "post",
        url: cfg.hostae + "get_pattern",
        data: {
          text: this.proc_text,
        },
      });
      console.log(response);
      this.resp_data = response.data;
      this.resp_text = response.data.print_text;
    },
    async onProcAdd() {
      const response = await axios({
        method: "post",
        url: cfg.hostae + "get_pattern_add",
        data: this.resp_data,
      });
      this.all_data = response.data;
      console.log("this.resp_data");
      console.log(this.all_data);
    },
    async onLoadDB() {
      const response = await axios({
        method: "get",
        url: cfg.hostae + "load_db",
        data: this.resp_data,
      });
      this.all_data = response.data;

      console.log(this.all_data);
    },
    async onClearDB() {
      const response = await axios({
        method: "get",
        url: cfg.hostae + "clear_db",
      });
      console.log(response);
    },
    async onFindCL() {
      const response = await axios({
        method: "post",
        url: cfg.hostae + "findae",
        data: {
          filename: this.filename,
          counts: this.counts,
        },
        headers: {
          "Content-Type": "application/json",
        },
      });
      // console.log(response);
      this.ae_data = response.data;
      console.log("ae_data");
      console.log(this.ae_data);
    },
  },
};
</script>

<style lang="sass">
.page
  padding-bottom: 10px
  padding-top: 10px
  padding-left: 10px
  padding-r: 10px

.chatmessages
  padding: 10px
  margin: 10px
.flexrowЁ
  display: flex
  flex-flow: row wrap
  justify-content: flex-start
.editorclass
  max-height: 500px
.inputtext
  width: 800px
</style>
