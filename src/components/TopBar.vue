<template>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <span>CATSTAGRAM</span>
      </div>

      <v-spacer></v-spacer>
      <div class="topSelect" >
        <v-select
          :v-model="raza"
          :items="razas"
          item-text="name"
          item-value="id"
          label="Seleccione raza"
          @change="seleccionaRaza"
        ></v-select>
      </div>
      
    </v-app-bar>
</template>

<script>

  import axios from 'axios'

  export default {
    data(){
      return{
        razas:[],
        raza:{}
      }
    },
    created(){
      this.getRazas();
    },
    methods:{
      async getRazas(){
        var _this=this;

        const response=await axios.create({
        baseURL:'https://api.thecatapi.com/',
        headers:{
            'Accept':'application/json',
            'Content-Type':'application/json',
            'x-api-key':'36ada3b5-a041-4790-869b-2f995031a592',
        }}).get('v1/breeds');
        
        _this.razas=response.data;

        console.log("Razas",this.razas);
      },
      async seleccionaRaza(value){
        var _this=this;
        _this.$store.state.infinityS=false;
        _this.$store.state.pagina=0;
        _this.$store.state.imagenes=[];

        const response=await axios.create({
        baseURL:'https://api.thecatapi.com/',
        headers:{
            'Accept':'application/json',
            'Content-Type':'application/json',
            'x-api-key':'36ada3b5-a041-4790-869b-2f995031a592',
        }}).get('/v1/images/search?limit=6&breed_ids=' + value);
        
        _this.$store.state.imagenes=response.data;
      }
    }
  }
</script>

<style>
.topSelect{
  margin-top: 20px;
  max-width: 180px;
}
</style>