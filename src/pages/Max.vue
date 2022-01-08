<template>
  <q-page class="flex flex-center">
    <q-item
        v-for="data in character"
        :key="data.id"
        v-ripple>
        <q-item-section>
          <q-item-label caption>Id: {{ data.id }}</q-item-label>
          <q-item-label>Title: {{ data.title }}</q-item-label>
          <q-item-label>Value: {{ data.value }}</q-item-label>
        </q-item-section>
    </q-item>
    <q-btn style="background: goldenrod; color: white" label="Generate" @click="generate" />
  </q-page>
</template>

<script>
import axios from 'axios';

export default {
  data () {
    return {
      character: [],
      races: [],
    }
  },
  methods: {
    generate () {
      if (this.character.length == 0) return;
      if (this.races.length == 0) return;

      let result = '';
      for (let race of this.races) {
        for (let item of this.character) {
          if (item.id == 'race' && race.id == item.value) {
            for (let it of this.character) {
              result += `${it.id}: ${it.value == null ? '' : it.value}\n`;
            }
          }
        }
      }
      const blob = new Blob([result], { type: 'raw' })
      const url = window.URL.createObjectURL(blob);
      const link = document.createElement('a');
      link.href = url;
      link.setAttribute('download', 'generation.yml');
      document.body.appendChild(link);
      link.click();
    }
  },
  mounted () {
    axios
      .get('https://mocki.io/v1/ac05988f-e2e5-42f9-8088-9da6979926e3')
      .then(response => {
        this.character = response.data;
      })
      .catch(error => console.log('Error', error.message));
    
    axios
      .get('https://mocki.io/v1/b02d1de0-a465-4475-9ebb-94e8118c3757')
      .then(response => {
        this.races = response.data;
      })
      .catch(error => console.log('Error', error.message));
  }
}
</script>
c