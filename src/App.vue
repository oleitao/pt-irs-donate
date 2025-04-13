<template>
  <v-app>
    <v-btn
      elevation="5"
      fab
      absolute
      :small="!$vuetify.breakpoint.mdAndUp"
      class="github-btn"
      target="_blank"
      href="https://github.com/oleitao/irs-donate/"
      ><v-icon>mdi-github</v-icon></v-btn
    >
    <v-container class="mb-10">
      <div
        class="
          font-weight-bold
          display-1
          mb-3
          mt-2
          d-flex
          justify-center
          align-center
        "
        :class="$vuetify.breakpoint.mdAndUp ? 'text-center' : ''"
      >
        A quem posso doar 0.5% do IRS?
        <v-img class="ml-5" :src="logo" max-height="30" max-width="30"></v-img>
      </div>
      <v-row align="end" class="mb-1">
        <v-col cols="12" md="6" style="margin-bottom: 0.35rem">
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Procurar (qualquer campo)"
            single-line
            hide-details
          ></v-text-field>
        </v-col>

        <v-col cols="12" md="6" class="py-0">
          <v-autocomplete
            chips
            dense
            clearable
            deletable-chips
            v-model="localitiesFilter"
            :items="localities"
            label="Localidade"
            multiple
            no-data-text="Esta localidade não existe"
          />
        </v-col>
      </v-row>
      <v-data-table
        :headers="headers"
        :items="filteredEntities"
        class="elevation-1"
        :search="search"
        no-data-text="Sem dados disponíveis"
        no-results-text="Sem dados encontrados"
        :footer-props="{
          showFirstLastPage: true,
          'items-per-page-text': 'Entidades por página',
          'items-per-page-all-text': 'Todas',
          'items-per-page-options': [5, 10, 15, 30, 50, 100],
        }"
        :header-props="{
          showFirstLastPage: true,
          'sort-by-text': 'Ordenar por',
        }"
      >
      </v-data-table>
    </v-container>
    <v-footer
      app
      bottom
      fixed
      padless
      align="center"
      justify="center"
      max-height="5em"
      class="overflow-auto mt-3"
    >
      <v-col cols="12">
        <v-divider></v-divider>

        Dados recolhidos no
        <a
          href="https://info.portaldasfinancas.gov.pt/pt/apoio_contribuinte/IRS/Pages/IRS_entidades_beneficiarias_consignacao.aspx"
          target="_blank"
          >portal das finanças</a
        >
      </v-col>
    </v-footer>
  </v-app>
</template>

<script>
import entities from "/assets/data/entidades.json";
import localidades from "/assets/data/localidades.json";

export default {
  data() {
    return {
      localities: [],
      localitiesFilter: [],
      search: "",
      entities: [],
      headers: [
        { text: "NIPC", value: "NIPC" },
        { text: "Nome", value: "NOME" },
        { text: "Localidade", value: "LOCALIDADE" },
      ],
      logo: require("/assets/images/logo.png"),
    };
  },
  async created() {
    this.entities = entities;
    this.localities = localidades;
  },
  computed: {
    filteredEntities() {
      if (this.localitiesFilter.length > 0) {
        return this.entities.filter((e) =>
          this.localitiesFilter.includes(e.LOCALIDADE)
        );
      }
      return this.entities;
    },
  },
};
</script>
<style >
.v-application {
  background-color: #f4f4f4 !important;
}
.github-btn {
  right: 0.5em;
  top: 0.5em;
}
</style>