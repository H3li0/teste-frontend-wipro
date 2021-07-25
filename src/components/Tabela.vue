<template>
  <div id="app">
    <vue-bootstrap4-table
      :rows="rows"
      :columns="columns"
      :config="config"
      :classes="classes"
      @on-select-row="selecionaLinha">

      <!-- Customização para paginação da tabela -->
      <template slot="paginataion-previous-button">
          <i>Anterior</i>
      </template>
      <template slot="paginataion-next-button">
          <i>Próximo</i>
      </template>
      <template slot="pagination-info" slot-scope="props">
        Exibindo {{props.currentPageRowsLength}} registros de {{props.originalRowsLength}}
      </template>

      <!-- Mensagem para registros vazios -->
      <template slot="empty-results">
        Nenhum dado encontrado.
      </template>

      <!-- Ícones para função sort -->
      <template slot="sort-asc-icon">
        <i class="fas fa-sort-up"></i>
      </template>
      <template slot="sort-desc-icon">
          <i class="fas fa-sort-down"></i>
      </template>
      <template slot="no-sort-icon">
          <i class="fas fa-sort"></i>
      </template>

      <!-- Botão de Reset -->
      <template slot="reset-button-text">
        <i class="fas fa-broom"></i> Resetar
      </template>

    </vue-bootstrap4-table>
  </div>
</template>

<script>
import VueBootstrap4Table from 'vue-bootstrap4-table';
import {EventBus} from '../utils/event-bus';

export default {
  name: 'Tabela',
  props: ['rows'],
  components: {
    VueBootstrap4Table
  },
  data: function() {
    return {
      columns: [
        {
          label: "id",
          name: "id",
          uniqueId: true,
          visibility: false
        },
        {
          label: "País",
          name: "pais",
          sort: true
        },
        {
          label: "Número de Casos",
          name: "confirmados",
          sort: true
        },
        {
          label: "Recuperados",
          name: "recuperados",
          sort: true
        },
        {
          label: "Número de Mortes",
          name: "mortes",
          sort: true
        }
      ],
      config: {
        global_search: {
          placeholder: "Pesquisar...",
          visibility: true
        },

        show_refresh_button: false,
        show_reset_button: true,

        pagination: true,
        pagination_info: true,
        num_of_visibile_pagination_buttons: 5,
        per_page: 10,
        selected_rows_info: false,
        per_page_options: [5, 10, 20, 30],

        checkbox_rows: false,
        highlight_row_hover: true,
        rows_selectable: true,
        multi_column_sort: false,

        highlight_row_hover_color: "#DFF2FF",
        card_title: "COVID-19: Estatística de Casos por País",
        card_mode: false,
      },
      classes: {
        table: "table-bordered table-striped"
      }
    }
  },
  methods: {
    selecionaLinha: function(payload) {
      EventBus.$emit("tabela-clicked", {
        nomePais: payload.selected_item.pais,
        series: [payload.selected_item.confirmados, payload.selected_item.recuperados, payload.selected_item.mortes]
      });
    }
  }
}
</script>

<style scoped>

</style>