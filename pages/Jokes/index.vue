<template>
  <!-- single joke page -->
  <div>
    <SearchJokes v-on:search-text="searchText" />
    <Joke v-for="joke in jokes" :key="joke.id" :id="joke.id" :joke="joke.joke" />
  </div>
</template>

<script>
import axios from 'axios'
import Joke from '../../components/Joke'
import SearchJokes from '../../components/SearchJokes'

export default {
  components: {
    Joke,
    SearchJokes
  },
  head() {
    return {
      title: 'Dad Jokes',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Best place for corny dad jokes'
        }
      ]
    }
  },
  data() {
    return {
      jokes: []
    }
  },
  async created() {
    // request API
    const config = {
      headers: {
        Accept: 'application/json'
      }
    }
    try {
      const res = await axios.get('https://icanhazdadjoke.com/search', config)
      // console.log(res.data)
      this.jokes = res.data.results
    } catch (error) {
      console.log(error)
    }
  },
  methods: {
    async searchText(text) {
      const config = {
        headers: {
          Accept: 'application/json'
        }
      }
      try {
        const res = await axios.get(
          `https://icanhazdadjoke.com/search?term=${text}`,
          config
        )
        // console.log(res.data)
        this.jokes = res.data.results
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>
