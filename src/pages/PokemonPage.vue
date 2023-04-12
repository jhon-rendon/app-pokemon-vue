<template>
    <h1 v-if="!pokemon">Espere por favor ...</h1>
    <div v-else>
      <h1> ¿Quién es este pokémon?</h1>
        <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
        <PokemonOptions 
                :pokemons="pokemonArr"
                @selectionPokemon="checkAnswer"
        />

        <template v-if="resp">
            {{msgResp}}
            <button @click="reiniciar()"> Reiniciar</button>
        </template>
    </div>
</template>

<script>
import PokemonPicture from '@/components/PokemonPicture.vue'
import PokemonOptions from '@/components/PokemonOptions.vue'

import getPokemonOptios from '@/helpers/getPokemonOptions'



export default {
  components: { PokemonPicture, PokemonOptions },
  data(){
    return {
        pokemonArr: [],
        pokemon:null,
        showPokemon:false,
        resp:false,
        msgResp:''
    }
  },
  methods:{
    async mixPokemonArray(){
        this.pokemonArr = await getPokemonOptios();

        const aleatorio = Math.floor( Math.random() * 4 );
        this.pokemon    = this.pokemonArr[ aleatorio ];
    },
    checkAnswer( id ){
        console.log( 'checkAnswer',id);
        
        //this.msgResp = ( id === this.pokemon.id )? 'Es correcto' : 'Es incorrecto';
        if( id === this.pokemon.id ){
            this.msgResp = 'Es correcto'; 
            this.showPokemon = true;
        }
        else{
            this.msgResp = 'Es incorrecto';
        }
        this.resp = true;
    },
    async reiniciar (){
        
        this.showPokemon = false;
        this.resp        = false;
        this.msgResp     = '';
        this.pokemonArr  = [];
        this.pokemon     = null;
        await this.mixPokemonArray();
        

    }

  },
  mounted(){
     this.mixPokemonArray();
  },


}
</script>

<style>

</style>