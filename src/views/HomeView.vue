<template>
    <v-container>
      <dialog-imagen />
      <v-row>
        <v-col
          v-for="(imagen,n) in imagenes"
          :key="n"
          cols="12"
          md="4"
        >
          <v-item >
            <!-- <v-img class="imagenGrid" :src="imagen.url" :alt="n" ></v-img> -->
            <dialog-imagen :cat="imagen"/>
          </v-item>
        </v-col>
      </v-row> 
    </v-container>
</template>

<script>
import axios from 'axios'
import DialogImagen from '@/components/DialogImagen.vue';

  export default {
  
    name: 'Home',
    components: { DialogImagen },
    data(){
      return{
        dialog:false,
        pagina:0
      }
    },
    computed:{
      imagenes(){
        return this.$store.state.imagenes;
      },
      infinityS(){
        return this.$store.state.infinityS;
      },
      pagina(){
        return this.$store.state.pagina;
      }
    },
    created(){
      this.pagina=0;
      this.getImagenes(this.pagina);
      this.infinity();
    },
    methods:{
     async getImagenes(pagina){
        var _this=this;

        const response=await axios.create({
          baseURL:'https://api.thecatapi.com/',
          headers:{
            'Accept':'application/json',
            'Content-Type':'application/json',
            'x-api-key':'36ada3b5-a041-4790-869b-2f995031a592',
          }}).get('/v1/images/search?limit=9&page=' + pagina);
        
        if(pagina==0){
          _this.$store.state.imagenes=response.data;
        }else{
          response.data.forEach(function(item) {
            _this.$store.state.imagenes.push(item);
          });
          
        }

      },
      seleccionaImagen(value){
        console.log(value);
      },
      infinity(){
        var _this=this;

        window.onscroll = () => {
          let bottomOfWindow = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight;
          if (bottomOfWindow && _this.infinityS==true) {
            _this.getImagenes(_this.pagina);
            _this.pagina++;
            console.log(_this.pagina)
          }
        }
        
      }
    }
  }
</script>

