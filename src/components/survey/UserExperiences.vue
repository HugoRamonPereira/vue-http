<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click='loadExperiences'>Load Submitted Experiences</base-button>
      </div>
      <h4 v-if='isLoading'>Loading results...</h4>
      <h4 v-else-if='!isLoading && (!results || results.length === 0)'>No experiences found. Please add some and try
        again!</h4>
      <ul v-else-if='!isLoading && results && results.length > 0'>
        <SurveyResult v-for="result in results" :key="result.id" :name="result.name" :rating="result.rating" />
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false
    }
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      fetch('https://vue-http-4e3f6-default-rtdb.firebaseio.com/surveys.json')
        .then((res) => {
          if (res.ok) {
            return res.json()
          }
        }).then((data) => {
          this.isLoading = false;
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating
            });
          }
          this.results = results;
        });
    }
  },
  mounted() {
    this.loadExperiences();
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