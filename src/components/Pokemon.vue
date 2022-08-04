<script >
import axios from 'axios'

export default {
    created: function(){
        axios.get(this.url).then(res =>{
            this.pokemon.id = res.data.id
            this.pokemon.type = res.data.types[0].type.name            
            this.pokemon.ability = res.data.abilities[0].ability.name
            this.pokemon.abilitysecond = res.data.abilities[1].ability.name
            this.pokemon.front = res.data.sprites.front_default
            this.pokemon.back = res.data.sprites.back_default  
            this.pokemon.weight = res.data.weight                
            this.pokemon.height = res.data.height                 
            this.pokemon.points = Math.floor(Math.random() * 11)                 
            this.currentImg = this.pokemon.front                   
        })
    },
    data(){
        return {
            isFront: true,
            currentImg:'',
            locationImg:'',            
            pokemon:{
                type:'',
                typesecond:'',
                ability:'',
                abilitysecond:'',
                front:'',                
                back:'',
                location:'',
                id:'',
                points:'',
            }
        }
    },
  props: {
      name: String,
      url: String,
  },
  methods: {
        upper: function(value){
            let newName = value[0].toUpperCase() + value.slice(1)
            return newName
        },

        changeSprite: function(){
            if(this.isFront ){
                this.isFront = false                
                this.currentImg = this.pokemon.back                
            }else{
                this.isFront = true                
                this.currentImg = this.pokemon.front                
            }
        },

        unitConvert: function(value){
            let newvalue = value * 0.1
            return newvalue.toFixed(1)
        }
    }
}

</script>

<template>
  <div id="pokemon">                     
     <div class="card">
        <div class="card-image ">
            <figure class="image is-4by3">
                <img :src="currentImg" alt="Placeholder image">
            </figure>
        </div>        
            <div class="card-content">
                <div class="media">      
                <div class="media-content has-text-centered">
                    <p class="title is-4">{{pokemon.id}} - {{upper(name)}} </p>
                    <p class="subtitle is-6">{{pokemon.type}}</p>
                    <p class="subtitle is-6">{{pokemon.typesecond}}</p>
                    <p class="subtitle is-6">{{unitConvert(pokemon.weight)}}Kg</p>
                    <p class="subtitle is-6">{{unitConvert(pokemon.height)}}m</p>
                    <p class="subtitle is-6">{{pokemon.points}} pontos</p>
                </div>
            </div>
            <div class="content">
                <button class="button is-medium is-fullwidth" @click="changeSprite">Mudar sprite</button>
            </div>
        </div>
     </div>
  </div>
  
</template>

<style >

#pokemon{
    margin-top: 2%;
}

#tipos{
    display: flex ;
    flex-direction: column;    
    justify-content: space-evenly;  
    margin-bottom: 2%;
    width: 100%;
}
#conteudo{
    display: block ;
}

#informações{
    display:contents;
}
</style>
