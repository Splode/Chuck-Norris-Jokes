<template>
  <main class="logo">
    <section>
      <button @click="next">BAMF!</button>
      <p>{{ punchLine }}</p>
    </section>
  </main>
</template>

<script>
export default {
  data () {
    return {
      batch: [],
      jokes: [],
    }
  },
  computed: {
    punchLine() {
      let current = this.jokes[0];
      if (this.jokes.length > 0) {
        return current.replace(/&[#039]*;/g, "'").replace(/&[amp]*;/g, '&').replace(/&[quote]*;/g, '"');
      }
    }
  },
  created() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      let apiURL = 'https://api.icndb.com/jokes/random/10';
      let vm = this;
      $(document).ready(function() {
        $.getJSON(apiURL, function(data) {
          vm.batch = data.value.reduce(function(acc, item) {
          acc.push(item.joke);
          return acc;
        }, []);
        });
      });
    },
    next() {
      this.jokes.length < 3 ? this.update() : this.jokes.shift();
    },
    update() {
      let vm = this;
      this.jokes.shift();
      this.fetchData();
      vm.batch.forEach(function(joke) {
        vm.jokes.push(joke);
      });
    }
  },
}
</script>

<style lang="scss">
  @import 'main.scss'
</style>
