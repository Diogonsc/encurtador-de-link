<template>
  <div class="box container">
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
        class="btn-link"
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
  <div class="link">
    <ul>
      <li>
        <a class="a-linkcurto" :href="this.linkCurto" target="blank">{{
          this.linkCurto
        }}</a>
      </li>
    </ul>
  </div>
  <!-- <div class="q-pa-md tabela">
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
              @click="excluirUrl(props.row)"
              title="Excluir"
              text-color="white"
              icon="delete_forever"
            />
          </div>
        </q-td>
      </template>
    </q-table>

    <div class="row justify-center q-mt-md">
      <q-pagination
        v-model="pagination.page"
        active-color="amber-6"
        color="white"
        :max="pagesNumber"
        size="md"
      />
    </div>
  </div> -->
</template>

<script>
import { useQuasar } from "quasar";
// import { ref, computed } from 'vue';
import Button from "../components/Button.vue";

export default {
  components: { Button },
  mounted() {},
  methods: {
    encurtarUrl() {
      if (this.nameUrl == "" || this.nameUrl == null) {
        this.msgErro();
      } else {
        if ((this.linkcurrent = "")) {
          this.shortenUrl();
        } else {
          this.adcionar();
        }
      }

      this.nameUrl = "";
    },
    shortenUrl() {
      const apiKey = "05d6a635a4e0c77d96ab81cd7c6b3ccdfde3025e";
      const url = "https://api-ssl.bitly.com";

      const urlToShorten =
        url + "/v3/shorten?access_token=" + apiKey + "&longUrl=" + this.nameUrl;
      fetch(urlToShorten)
        .then(
          (response) => {
            if (response.ok) {
              return response.json();
            }

            throw new Error("Request failed!");
          },
          (networkError) => console.log(networkError.message)
        )
        .then((jsonResponse) => {
          this.linkCurto = jsonResponse.data.url;
          console.log(this.linkCurto);
        });
    },

    adcionar() {
      //html elements
      this.shortenUrl();
      //AJAX functions

      this.rows.push({ name: this.linkCurto });
      this.msgAdcionada(); // notificação
    },
    copiarUrl() {
      this.msgCopiada(); // notificação
    },
    excluirUrl(row) {
      console.log("Index", row.name);
      this.rows.splice(row, 1);
      this.msgExcluida(row);
    },
  },
  data() {
    return {
      title: "Encurtador de URL",
      nameUrl: "",
      linkCurto: "",
      // columns: [
      //   { name: "id", align: "center", sortable: false },
      //   {
      //     label: "Nome da Url",
      //     align: "left",
      //     field: (row) => row.name,
      //     format: (val) => `${val}`,
      //   },
      //   { name: "option", align: "center", label: "Opções", sortable: false },
      // ],

      // rows: [],
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
.container {
  padding-left: 15px;
  padding-right: 15px;
}
.link {
  display: flex;
  justify-content: center;
  padding-top: 50px;

  li {
    list-style: none;
  }

  .a-linkcurto {
    color: #fff;
    text-decoration: none;
  }
}
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

@media screen and (max-width: 500px) {
  .tabela {
    margin: auto;
    width: 100%;
  }
  .btn-link {
    width: 100%;
  }
}
</style>
