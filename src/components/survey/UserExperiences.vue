<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <ul>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';
import axios from 'axios';

export default {
  components: {
    SurveyResult
  },
  data() {
    return {
      results: []
    };
  },
  methods: {
    loadExperiences() {
      axios
        .get(
          'https://vue-backend-3fdbc-default-rtdb.firebaseio.com/surveys.json'
        )
        .then(resp => {
          if (resp.ok) {
            return resp.data;
          }
          const results = [];
          for (const id in resp.data) {
            results.push({
              id: id,
              name: resp.data[id].name,
              rating: resp.data[id].rating
            });
          }
          this.results = results;
        });
    }
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
