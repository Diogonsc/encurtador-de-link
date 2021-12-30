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
  <div class="q-pa-md tabela" v-if="`${this.nameUrl}`">
    <q-table
      title="Urls encurtadas."
      :rows="rows"
      :columns="columns"
      row-key="name"
      hide-pagination
    >
      {{ nameUrl }}
      <Button
        icon="fas fa-trash-alt"
        no-caps
        rounded
        color="negative"
        label="Excluir"
        text-color="white"
        size="md"
      />
      <template>
        <q-tr>
          <q-td>
            {{ nameUrl }}
          </q-td>
        </q-tr>
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
import { useQuasar } from 'quasar';
// import { ref, computed } from 'vue';
import Button from '../components/Button.vue';

export default {
  components: { Button },
  methods: {
    encurtarUrl() {
      const url = document.getElementById('input-url').value;
      console.log(url);
      if (!url || null) {
        this.msgErro();
      } else {
        this.adcionar();
      }
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
      title: 'Encurtador de URL',
      nameUrl: '',
      columns: [
        {
          label: 'Nome da Url',
          align: 'left',
          field: (row) => row.name,
          format: (val) => `${val}`,
        },
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
          icon: 'check',
          type: 'warning',
          position: 'top-right',
          message: 'Link encurtado com sucesso.',
        });
      },
      msgErro() {
        $q.notify({
          icon: 'fas fa-times',
          type: 'negative',
          position: 'top-right',
          message: 'Por favor insira um link.',
        });
      },
      msgCopiada() {
        $q.notify({
          icon: 'check',
          type: 'warning',
          position: 'top-right',
          message: 'Link copiado com sucesso.',
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
