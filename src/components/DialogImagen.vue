<template>
  <div class="text-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-img
          class="imagenGrid"
          dark
          v-bind="attrs"
          v-on="on"
          :src="cat.url">
        </v-img>
      </template>

      <v-card>
        <v-card-title
          class="headline grey lighten-2"
          primary-title
        >
          <div v-if="breed.length>0">
            {{breed[0].name}}
          </div>
          <div v-else>
            Sin Raza
          </div>
        </v-card-title>

        <v-card-text>
          <v-img class="imagenContenido" :src="cat.url" :alt="cat.id" ></v-img> 
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <div v-if="breed.length>0">{{breed[0].description}}</div>
          <div v-else>Sin información por el momento, estamos trabajando en ello.</div>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="dialog = false"
          >
            Cerrar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        dialog: false,
        breed:[]
      }
    },
    props:{
      cat: {
        type: Object,
        required: true
    },
    },
    created(){
      console.log("Imagenes",this.cat);
      this.breed=this.cat.breeds;
      console.log("Razas",this.breed);
    }
  }
</script>

<style>
.imagenGrid{
  width: 100%;
  height: 40vh;
}
.imagenContenido{
  width: 100%;
  height: 40vh;
}
</style>
