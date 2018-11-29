<template>
  <div id="app">
    <nav class="navbar navbar-dark bg-dark">
    <a class="navbar-brand" href="#">
      Quick Challenge
    </a>
  </nav>
    <div class="row mt-3">
      <div class="col-md-12 col-lg-5 text-center">
        <h2>Lista de Razas</h2>
        <div class="list-group" v-for="(item,index) in items" :key="index" v-show="(pag - 1) * pagMax <= index  && pag * pagMax > index">
          <a href="#" v-text="item" v-on:click="getIMG(item)" class="list-group-item list-group-item-action"></a>
        </div>

        <nav aria-label="Page navigation" class="text-center mt-2">
          <ul class="pagination text-center justify-content-between">
            <li class="page-item mr-2" v-bind:class="{ disabled:isDisabled }">
              <a href="#" class="page-link" @click.prevent="pag -=1">
                Anterior
              </a>
            </li>
            <li class="page-item" v-bind:class="{disabled:isDisabledNext}">
              <a href="#" class="page-link" @click.prevent="pag +=1">
                Siguiente
              </a>
            </li>
          </ul>
        </nav>
      </div>
      <div class="col-md-12 col-lg-7 text-center">
        <h2>Imagen</h2>
        <img class="img-fluid rounded" :src="result">
        <h3 v-text="name"></h3>
      </div>
    </div>
  </div>
</template>

<script>
const axios = require('axios');
export default {
  name: 'app',
  data: function(){
    return {
      items : [],
      result: '',
      pagMax:10,
      pag: 1,
      name: '',
    }
  },
  methods:{
    getIMG: function(nombre){
      let app = this;
      let url = 'https://dog.ceo/api/breed/'+nombre+'/images/random';
      axios.get(url)
            .then(function(response){
              app.result = response.data.message;
              app.name = nombre;
            })
            .catch(function(error){
              console.log(error);
            })
    }
  },
  computed:{
    isDisabled: function(){
      return this.pag == 1;
    },
    isDisabledNext: function(){
      return this.pag* this.pagMax / this.items.length > 1
    }
  },
  mounted(){

    let app = this;
    axios.get('https://dog.ceo/api/breeds/list')
        .then(function(response){
          app.items = response.data.message;
        })
        .catch(function(error){
          console.log(error);
        })
}
}
</script>

<style>
  #app{
    margin:1em;
  }
  img.image{
    width: 95%;
    height: auto;
  }
</style>
