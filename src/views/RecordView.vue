<template>
    <v-data-table-server
      v-model:items-per-page="itemsPerPage"
      :headers="headers"
      :items-length="totalItems"
      :items="serverItems"
      :loading="loading"
      :search="search"
      item-value="UUID"
      @update:options="loadItems"
    >
    <template v-slot:[`item.actions`]="{ item }">
      <v-icon
        size="small"
        class="me-2"
        @click="sendItem(item)"
      >
        mdi-send
      </v-icon>
      <v-icon
        size="small"
        @click="cancelItem(item)"
        color="red"
      >
        mdi-cancel
      </v-icon>
    </template>
    </v-data-table-server>
  </template>
  

<script>
import axios from 'axios';

export default {
  data() {
    return {
      apiUrl: 'http://localhost:8000/invoice',
      json: null,
      itemsPerPage: 13,
      headers: [
        { title: 'Tipo de documento', value: 'TipoDocumento' },
        { title: 'Serie', value: 'Serie' },
        { title: 'Folio', value: 'FolioNumero' },
        { title: 'Total', value: 'Total' },
        { title: 'Fecha', value: 'FechaTimbrado' },
        { title: 'Estatus', value: 'Status' },
        { title: 'Opciones', value: 'actions' },
      ],
      search: '',
      serverItems: [],
      loading: true,
      totalItems: 0,
    };
  },

  created() {
    this.fetchData();
  },

  methods: {
    async fetchData() {
      try {
        const response = await axios.get(`${this.apiUrl}/index`, {
          params:{
            'year': '',
            'rfc': '',
            'type_document': '',
            'page': 1, // Esto debería ser fijo en 1 para la primera carga
            'per_page': this.itemsPerPage // Utiliza this.itemsPerPage para hacer referencia a los elementos por defecto
          }
        });
        this.totalItems = response.data.total;
        this.serverItems = response.data.data;
        this.loading = false;
      } catch (error) {
        console.error('Error al obtener datos:', error);
      }
    },

    async loadItems(newOptions) {
      try {
        const response = await axios.get(`${this.apiUrl}/index`, {
          params:{
            'year': '',
            'rfc': '',
            'type_document': '',
            'page': newOptions.page, // Toma el valor de la pagina en la que navegamos
            'per_page': newOptions.itemsPerPage // Para la actualización se obtienen los datos de el paginador
          }
        });
        this.totalItems = response.data.total;
        this.serverItems = response.data.data;
        this.loading = false;
      } catch (error) {
        console.error('Error al obtener datos:', error); 
      }
    },

    async sendItem(item) {
      // Lógica para enviar el item
      try {
        await axios.post(`${this.apiUrl}/send/${item.UID}`, {
          // Parametros extra
        })
        console.log('Enviar item:', item);
        
      } catch (error) {
        console.error('Error no se puede enviar tu CFDI', error);
      }
      
    },

    async cancelItem(item) {
      try {
        await axios.post(`${this.apiUrl}/cancel/${item.UID}`, {
      
            'motivo': '02',
            'folioSustituto': ''
        });
        console.log('Cancelar item:', item);
        
      } catch (error) {
        console.error('Error no se puede cancelar tu CFDI', error);
      }
      
    },

  },
};
</script>
