<template>
  <div class="app">
    <header>
      <h1>the anime db</h1>
      <form class="search-box" @submit.prevent="handleSearch">
        <input
          type="search"
          class="search-field"
          placeholder="search..."
          required
          v-model="search_query"
        />
      </form>
    </header>
    <main>
      <div class="cards">
        <Card v-for="anime in animelist" :key="anime.mal_id" :anime="anime"/>
      </div>
    </main>
  </div>
</template>

<script>
import { ref } from 'vue'
import Card from './components/Card.vue'

export default {
  setup(){

    const search_query = ref("")
    const animelist = ref([])

    const handleSearch = async () => {
      animelist.value = await fetch(`https://api.jikan.moe/v3/search/anime?q=${search_query.value}`)
        .then(res => res.json())
        .then(data => data.results)

        console.log(animelist.value);
    }



    return {
      Card,
      search_query,
      animelist,
      handleSearch,
    }
  }
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Comfortaa', cursive;
}

a {
  text-decoration: none;
}

header {
  padding-top: 50px;
  padding-bottom: 50px;
}

h1 {
  text-align: center;
  text-transform: uppercase;
  font-weight: 700;
  padding-bottom: 30px;
}

.search-box {
  display: flex;
  justify-content: center;
  padding-left: 30px;
  padding-right: 30px;
}

.search-field {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  /* background-color: rgba(0,0,0,0.2); */
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);
  padding: 20px;
  display: block;
  width: 100%;
  max-width: 600px;
  border-radius: 10px;
  font-size: 20px;
}

.search-field:focus,
.search-field:valid {
  background-color: orange;
  color: black;
  font-weight: bolder;
}

main{
  max-width: 1200px;
  margin: 0 auto;
  padding-left: 30px;
  padding-right: 30px;
}

.cards{
  display: flex;
  flex-wrap: wrap;
  margin: 0 -8px;

}
</style>
