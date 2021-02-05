<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && error">
        {{ error }}
      </p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">
        Nothing stored, add something
      </p>
      <ul v-else-if="!isLoading && results.length > 0">
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
      results: [],
      error: null,
      isLoading: false
    };
  },
  mounted() {
    this.loadExperiences();
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
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
          this.isLoading = false;
        })
        .catch(() => {
          this.error = 'Uh Oh we got a problem';
          this.isLoading = false;
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
