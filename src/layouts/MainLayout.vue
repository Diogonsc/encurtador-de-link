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
      <Button
        no-caps
        rounded
        color="amber-6"
        label="Copiar"
        text-color="#161a1d"
        size="lg"
        @click.prevent="copiarUrl()"
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
      {{ nameUrl }}
      <template v-slot:body-cell-option="props">
        <q-td :props="props">
          <div class="q-pa-sm q-gutter-sm">
            <q-btn
              size="sm"
              color="yellow-9"
              @click="excluirUrl()"
              icon="create"
            />
            <q-btn
              size="sm"
              color="red"
              @click="editarUrl()"
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

export default {
  components: { Button },
  methods: {
    encurtarUrl() {
      if (this.nameUrl == "" || this.nameUrl == null) {
        this.msgErro();
      } else {
        this.adcionar();
      }

      const headers = {
        "Content-Type": "application/json",
        apiKey: "24ae553329394b3fbc182c08bd824cfb",
      };

      const linkRequest = {
        destination: url,
        domain: { fullName: "rebrand.ly" },
      };

      fetch("https://api.rebrandly.com/v1/links", {
        methods: "POST",
        headers: headers,
        body: JSON.stringify(linkRequest),
      }).then((response) => response.json());
    },
    adcionar() {
      this.rows.push({ name: this.nameUrl });
      this.msgAdcionada(); // notificação
      return this.rows.nameUrl;
    },
    copiarUrl() {
      this.msgCopiada(); // notificação
    },
  },
  data() {
    return {
      title: "Encurtador de URL",
      nameUrl: "",
      columns: [
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
          icon: "check",
          type: "warning",
          position: "top-right",
          message: "Link encurtado com sucesso.",
        });
      },
      msgErro() {
        $q.notify({
          icon: "fas fa-times",
          type: "negative",
          position: "top-right",
          message: "Por favor insira um link.",
        });
      },
      msgCopiada() {
        $q.notify({
          icon: "check",
          type: "warning",
          position: "top-right",
          message: "Link copiado com sucesso.",
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
