<template>
  <v-container fluid grid-list-md>
    <v-layout>
      <v-flex text-xs-center>
        <h3>Данные из открытого источника - holidayapi.com</h3>
        <v-alert :value="true" type="warning" v-if="error">
          {{error}}
        </v-alert>
      </v-flex>
    </v-layout>
    <v-layout align-center>
      <v-flex xs12 sm6 offset-sm3 d-flex>
        <v-select
          :value="country"
          :items="countries"
          @change="chengeCountry($event)"
          label="Страна"
          outline
        ></v-select>
      </v-flex>
    </v-layout>

    <v-layout align-center>
      <v-flex xs12>
        <v-data-table v-if="holidays" :headers="headers" :items="holidays" class="elevation-1">
          <template v-slot:items="props">
            <td>{{ props.item.name }}</td>
            <td>{{ props.item.date }}</td>
          </template>
        </v-data-table>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import {mapGetters} from 'vuex';

export default {
  created() {
    this.$store.dispatch("holidays/LOAD_HOLIDAYS", null, { root: true });
  },
  data() {
    return {
      headers: [
        { text: "Название", value: "name" },
        { text: "Дата", value: "date" },
      ],
      countries: [
        { text: "Украина", value: "ua" },
        { text: "США", value: "us" },
        { text: "Германия", value: "de" }
      ],
    };
  },
  computed: {
    ...mapGetters('holidays', [
      'country',
      'holidays',
      'error'
    ])
  },
  methods: {
    chengeCountry(country) {
      this.$store.dispatch("holidays/SET_COUNTRY", country, { root: true });
    }
  }
};
</script>
