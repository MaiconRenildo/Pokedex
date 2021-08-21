<template>
  <div id="pokemon">
    <!-- <small>{{url}}</small>  -->
    <div class="card">

      <div class="card-image">
        <figure>
          <img :src="currentImg" alt="Placeholder image">
        </figure>
      </div>

      <div class="card-content">

        <div class="media">
          <div class="media-content">
            <p class="title is-4">{{num}} {{name | upper}}</p>
            <p class="subtitle is-6">{{Pokemon.type}}</p>
          </div>
        </div>

        <div class="content">
          <button class="button is-medium is-fullwidth" @click="mudarSprite">Mudar Sprite</button>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  created: function(){  //É executado logo após a instancia ser criada
    axios.get(this.url).then(res=>{
      this.Pokemon.type=res.data.types[0].type.name;
      this.Pokemon.front=res.data.sprites.front_default;
      this.Pokemon.back=res.data.sprites.back_default;
      this.currentImg=this.Pokemon.front;
    })
  },
  data(){
    return{
      isFront:true,
      currentImg:'',
      Pokemon:{
        type:'',
        front:'',
        back:''
      }
    }
  },
  props:{ //Define propriedades do componente
    num:Number,
    name:String,
    url:String
  },
  filters:{ //Trata determinado conteudo
    upper: function(value){
      let firstWord=value[0].toUpperCase()
      let newName=firstWord + value.slice(1)
      return newName
    }
  },
  methods:{ //Declaração de funções
    mudarSprite: function(){

      if(this.isFront){
        this.isFront=false;
        this.currentImg=this.Pokemon.back;
      }else{
        this.isFront=true;
        this.currentImg=this.Pokemon.front;
      }

    }
  }
}
</script>

<style>
  #pokemon{
    margin-top: 2%;
  }
</style>