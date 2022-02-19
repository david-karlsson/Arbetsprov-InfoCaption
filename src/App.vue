<template>

<header>
<img src="https://support.infocaption.com/images/ic-logo50.png"  alt="">
 </header>
 
  <SearchResults />
</template>
<script>
import SearchResults from './components/SearchResults.vue'

export default {
  name: 'App',
  components: {
    SearchResults
  }
}
</script>

<style>

h1{
    z-index: 99;
    margin: 1rem;
}
h2{
  font-size: 1.7em;

}
h3{
  font-size: 1.3em;
  }

*{
  font-size: 0.8rem;

}

header img{
  max-width: 15rem;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
padding-top: 5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
   background-color: rgb(255, 249, 243);
  height: 100vh;
}
</style>
