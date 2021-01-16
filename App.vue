<template>
  <app-alert :alert="alert" @close="alertClear"></app-alert>
  <div class="container column">
    <app-form @add="addBlock"></app-form>
    <div class="card card-w70">
      <app-resume
        :blocks="blocks"
        :dbKey="dbKey"
        :needSave="needSave"
        @save="saveDb"
        @delete="deleteDb"
      ></app-resume>
    </div>
  </div>
  <div class="container">
    <app-comments :comments="comments" @upload="uploadComments"></app-comments>
    <app-loader v-if="loading"></app-loader>
  </div>
</template>

<script>
import AppForm from "./AppForm";
import AppResume from "./AppResume";
import AppComments from "./AppComments";
import AppLoader from "./AppLoader";
import AppAlert from "./AppAlert";
import axios from "axios";
export default {
  data() {
    return {
      blocks: [],
      comments: [],
      loading: false,
      dbKey: "",
      needSave: false,
      alert: { type: "", title: "", text: "" },
    };
  },
  mounted() {
    this.loadDb();
  },
  methods: {
    addBlock(tp, val) {
      let id = this.blocks.length + 1;
      this.blocks.push({ id, tp, val });
      this.needSave = true;
    },
    async uploadComments() {
      try {
        this.comments = [];
        this.loading = true;
        let resp = await axios.get(
          "https://jsonplaceholder.typicode.com/comments?_limit=22"
        );
        this.loading = false;
        if (resp.data.length === 0) {
          throw new Error("Комментариев еще нет");
        }
        this.comments = resp.data;
      } catch (e) {
        this.loading = false;
        this.alarm("danger", "Ошибка загрузки", e.message);
      }
    },
    saveDb() {
      if (!this.dbKey) {
        this.insertDb();
      } else {
        this.updateDb();
      }
    },
    async insertDb() {
      if (this.blocks.length === 0) {
        return;
      }
      try {
        const resp = await fetch(
          "https://vue-with-http-2b85c-default-rtdb.firebaseio.com/resume.json",
          {
            method: "POST",
            headers: {
              "contente-type": "application/json",
            },
            body: JSON.stringify(this.blocks),
          }
        );
        const firebaseData = await resp.json();
        this.dbKey = firebaseData.name;
        this.needSave = false;
        this.alarm("primary", "Успешно", "Данные сохранены");
      } catch (e) {
        this.alarm("danger", "Ошибка записи в БД", e.message);
      }
    },
    async updateDb() {
      if (this.blocks.length === 0 || !this.dbKey) {
        return;
      }
      try {
        await fetch(
          `https://vue-with-http-2b85c-default-rtdb.firebaseio.com/resume/${this.dbKey}.json`,
          {
            method: "PUT",
            headers: {
              "contente-type": "application/json",
            },
            body: JSON.stringify(this.blocks),
          }
        );
        this.needSave = false;
        this.alarm("primary", "Успешно", "Данные изменены");
      } catch (e) {
        this.alarm("danger", "Ошибка записи в БД", e.message);
      }
    },
    async deleteDb() {
      if (!this.dbKey) {
        return;
      }
      try {
        await axios.delete(
          `https://vue-with-http-2b85c-default-rtdb.firebaseio.com/resume/${this.dbKey}.json`
        );
        this.dbKey = "";
        this.blocks = [];
        this.needSave = true;
        this.alarm("primary", "Успешно", "Данные удалены");
      } catch (e) {
        this.alarm("danger", "Ошибка удаления в БД", e.message);
      }
    },
    alarm(type, title, text) {
      this.alert.type = type;
      this.alert.title = title;
      this.alert.text = text;
    },
    alertClear() {
      this.alert.type = "";
      this.alert.title = "";
      this.alert.text = "";
    },
    async loadDb() {
      try {
        this.loading = true;
        const { data } = await axios.get(
          "https://vue-with-http-2b85c-default-rtdb.firebaseio.com/resume.json"
        );
        if (!data) {
          //throw new Error("Данные еще не сохранены в БД");
          this.loading = false;
          return;
        }
        Object.keys(data).map((key) => {
          var tmp = data[key];
          this.dbKey = key;
          Object.keys(tmp).map((key) => {
            this.blocks.push(tmp[key]);
          });
        });
        this.loading = false;
      } catch (e) {
        this.loading = false;
        this.alarm("danger", "Ошибка загрузки резюме из БД", e.message);
      }
    },
  },
  components: { AppForm, AppResume, AppComments, AppLoader, AppAlert },
};
</script>

<style>
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>
