<template>
  <div class="box">
    <h2 class="title text-center text-white">{{ title }}</h2>
    <div class="box-input">
      <q-input
        id="input-url"
        input-class="text-white ellipsis"
        type="text"
        standout="bg-transparent text-amber-6"
        rounded
        outlined
        v-model="nameUrl"
        label-color="amber-6"
        label="Digite a Url que deseja encurtar."
        style="width: 500px"
        @keyup.enter="encurtarUrl()"
      />
      <Button
        no-caps
        rounded
        color="amber-6"
        label="Encurtar"
        text-color="#161a1d"
        size="lg"
        @click.prevent="encurtarUrl()"
      />
    </div>
  </div>
  <div class="q-pa-md tabela">
    <q-table
      title="Urls encurtadas."
      :rows="rows"
      :columns="columns"
      row-key="name"
      hide-pagination
    >
      <template v-slot:body-cell-option="props">
        <q-td :props="props">
          <div class="q-pa-sm q-gutter-sm">
            <q-btn
              size="sm"
              color="amber-6"
              @click="copiarUrl()"
              title="Copiar"
              text-color="white"
              icon="fas fa-pencil-alt"
            />
            <q-btn
              size="sm"
              color="negative"
              @click="excluirUrl()"
              title="Excluir"
              text-color="white"
              icon="delete_forever"
            />
          </div>
        </q-td>
      </template>
    </q-table>

    <!-- <div class="row justify-center q-mt-md">
      <q-pagination
        v-model="pagination.page"
        active-color="amber-6"
        color="white"
        :max="pagesNumber"
        size="md"
      />
    </div> -->
  </div>
</template>

<script>
import { useQuasar } from "quasar";
// import { ref, computed } from 'vue';
import Button from "../components/Button.vue";
import axios from "../boot/axios";

export default {
  components: { Button },
  methods: {
    encurtarUrl() {
      if (this.nameUrl == "" || this.nameUrl == null) {
        this.msgErro();
      } else {
        this.adcionar();
      }

      this.nameUrl = "";
    },
    adcionar() {
      this.rows.push({ name: this.nameUrl });
      this.msgAdcionada(); // notificação
      return this.rows.nameUrl;
    },
    copiarUrl() {
      this.msgCopiada(); // notificação
    },
    excluirUrl(i) {
      this.rows.splice(i, 1);
      this.msgExcluida();
    },

    fetchUrl(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}, ${this.query}, ${this.api_key}`)
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.linkRequest = results;
    },
  },
  data() {
    return {
      // Api
      apikey: "24ae553329394b3fbc182c08bd824cfb",
      url_base: "https://api.rebrandly.com/v1/links",
      query: "",
      linkRequest: {},

      //
      title: "Encurtador de URL",
      nameUrl: "",
      columns: [
        { name: "id", align: "center", label: "ID", sortable: false },
        {
          label: "Nome da Url",
          align: "left",
          field: (row) => row.name,
          format: (val) => `${val}`,
        },
        { name: "option", align: "center", label: "Opções", sortable: false },
      ],

      rows: [],
    };
  },
  setup() {
    const $q = useQuasar();
    // const pagination = ref({
    //   sortBy: 'desc',
    //   descending: false,
    //   page: 1,
    //   rowsPerPage: 10,
    // });
    return {
      // pagination,
      // columns,
      // rows,

      // pagesNumber: computed(() => Math.ceil(rows.length / pagination.value.rowsPerPage)),
      msgAdcionada() {
        $q.notify({
          icon: "fas fa-badge-check",
          color: "amber-6",
          position: "top-right",
          message: "Url encurtada com sucesso.",
        });
      },
      msgErro() {
        $q.notify({
          icon: "fas fa-times",
          type: "negative",
          position: "top-right",
          message: "Por favor insira uma url.",
        });
      },
      msgCopiada() {
        $q.notify({
          icon: "check",
          color: "amber-6",
          position: "top-right",
          message: "Url copiada com sucesso.",
        });
      },
      msgExcluida() {
        $q.notify({
          icon: "fas fa-minus-circle",
          type: "negative",
          position: "top-right",
          message: "Url excluida com sucesso.",
        });
      },
    };
  },
};
</script>

<style lang="scss" scoped>
.box {
  width: 100vw;
  .box-input {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 10px;
  }
}
.tabela {
  margin: auto;
  width: 80%;
}

@media screen and (max-width: 900px) {
  .tabela {
    margin: auto;
    width: 100%;
  }
}
</style>
