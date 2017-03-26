<template>
  <main class="logo rattle">
    <section>
      <button class="slam" @click="hide">BAMF!</button>
      <!-- transition hook after-leave -->
      <transition name="slide-in" @after-leave="next">
        <p v-if="show">{{ punchLine }}</p>
      </transition>
    </section>
  </main>
</template>

<script>
export default {
  data () {
    return {
      batch: [],
      show: true,
      jokes: [],
    }
  },
  computed: {
    // Serves current joke after converting common HTML entities
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
    // Fetch joke data from ICNDB with jQuery and store in batch
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
    // Needed for transition conditional
    hide() {
      this.show = false;
    },
    // Call next joke
    next() {
      this.jokes.length < 3 ? this.update() : this.jokes.shift();
      // transition conditional
      this.show = true;
    },
    // Updates joke list from batch
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
@import 'main.scss';
</style>
