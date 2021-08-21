<template>
  <div>
    <div class="column is-half is-offset-one-quarter">
      <img src="../assets/logo.png">
      <hr>
      <h4 class="is-size-4">Pokedex</h4>

      <input type="text" placeholder="Buscar Pokemon" class="input is-rounded" v-model="busca" >
      <button class="button is-fullwidth is-success" id="BuscaBtn" @click="buscar" >Buscar</button>

      <div v-for="(poke,index) in filteredPokemons" :key="poke.url">  <!--Mudei a key devido ao bug de mostrar o mesmo pokemon ao variar a pesquisa ->poke.url é unica de cada elemento -->
        <!-- <h5>{{index +1}} - {{poke.name}}</h5>  -->
        <Pokemon :name="poke.name" :url="poke.url" :num="index+1" />
      </div>
    </div>
  </div>
</template>

<script>
import Pokemon from "./Pokemon.vue"
import axios from 'axios';
export default {
  data(){
    return {
      pokemons:[],
      filteredPokemons:[],
      busca:''
    }
  },
  created: function(){ //Executa quando a página é carregada
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res=>{
      this.pokemons=res.data.results;
      this.filteredPokemons=res.data.results
      /*
      Object.keys(this.pokemons).forEach((key) => {
        console.log(this.pokemons[key].name);
      });
      */
    })
  },
  components:{
    Pokemon
  },
  methods:{
    buscar:function(){

      this.filteredPokemons=this.pokemons
      if(this.busca.trim()==''){
        this.filteredPokemons=this.pokemons
      }else{
        let result=this.filteredPokemons.filter(pokemon=>pokemon.name.toLowerCase()==this.busca.toLowerCase());
        if(result.length!=0){
          this.filteredPokemons=result
        }
      }
    }
  }
}
</script>

<style>
  #BuscaBtn{
    margin-top: 2%;
    margin-bottom: 3%;
  }
</style>