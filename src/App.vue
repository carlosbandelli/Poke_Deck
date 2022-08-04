<script >
import axios from 'axios';
import Pokemon from './components/Pokemon.vue';

export default {
  name: 'App',
  data(){
    return {
      pokemons: [], //pilha de cartas para sacar
      shuffledPokemons: [], //cartas na mão
    }
  },
  computed: {
    disableWithdraw: function(){
      return this.shuffledPokemons.length >= 8
    } 
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=10&offset=0").then(res => {
      console.log("Pegou a lista de pokemons!")
      this.pokemons = res.data.results

      for(var i = this.pokemons.length-1 ; i > 0 ;i--){
        let j = Math.floor( Math.random() * (i + 1) ); //random index
        [this.pokemons[i],this.pokemons[j]] = [this.pokemons[j],this.pokemons[i]]; // swap
      }

      this.shuffledPokemons = this.pokemons.splice(0,5);
   })
  },
  components:{
    Pokemon
  },
  methods:{
    shufflePokemons: function(){
      const temp = [...this.shuffledPokemons]

      for(var i = temp.length-1 ; i>0 ;i--){
        let j = Math.floor( Math.random() * (i + 1) ); //random index
        [temp[i],temp[j]]=[temp[j],temp[i]]; // swap
      }

      this.shuffledPokemons = temp;
    },
    getNewCard: function(){  
      if(this.shuffledPokemons.length >= 8) return;

      let j = Math.floor( Math.random() * (this.pokemons.length-1 + 1) ); //random index
      
      this.shuffledPokemons = [...this.shuffledPokemons, ...this.pokemons.splice(j, 1)];
    }
  }
}


</script>

<template>
  <div id="app" class=" is-desktop">
      <div class=" is-desktop content is-centered is-vcentered">        
        <hr>
        <h4 class="is-size-4 has-text-centered white text-white" style="color: #fff" >Pokedex</h4>        
        <button class="button is-fullwidth is-success mb-3" id="buscaBtn" @click="shufflePokemons()"><b>shuffle</b></button>
        <button class="button is-fullwidth is-success mb-3" id="buscaBtn" @click="getNewCard()" :disabled="shuffledPokemons.length >= 8"><b>new card</b></button>
        <div v-for="poke in shuffledPokemons" :key="poke.url" class="is-centered is-vcentered" >
          <Pokemon :name="poke.name" :url="poke.url" />        
        </div>
      </div>
      
    </div>
  
</template>

<style scoped>

</style>
