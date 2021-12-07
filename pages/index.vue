<template>
  <section id="app">
    <headerNuxt id="headerNuxt" />

    <main id="page">
      <!-- Filtro -->
      <div>
        <h3>Busca la película por su titulo</h3>
        <input type="search" minlength="3" id="search" placeholder="Introduce una sola palabra con más de 3 caracteres" @keyup="filterFilms(searchText)" @ v-model="searchText">
        <h4 class="infoSearch">{{message}}</h4>
        <h4 v-if="numberFilms>0" class="infoSearch">Películas encontradas: {{numberFilms}}</h4>
      </div>

    <!-- Pelis -->
      <div id="collectionFilms">
        <article id="filmCard" v-for="(film, index) in films" :key="index" >
          <img class="img" :src="`${film.Poster}`" onerror="this.onerror=null;this.src='https://www.publicdomainpictures.net/pictures/280000/velka/not-found-image-15383864787lu.jpg'">
          <h3 class="title">{{film.Title}}</h3>
          <h4 class="year"><span class="dates">Año: </span>{{film.Year}}</h4>
          <h4 class="id"><span class="dates">Identificador Imdb: </span>{{film.imdbID}}</h4>
        </article>
      </div>
    </main>

    <footerNuxt id="footerNuxt"/>
  </section>
</template>

<script>
import axios from 'axios'
import headerNuxt from "@/components/headerNuxt"
import footerNuxt from "@/components/footerNuxt"

export default{
  components: {
    headerNuxt,
    footerNuxt
  },
  data(){
    return{
      films: [],
      numberFilms: 0,
      message: "",
      searchText: "",
      res: "",
      watcherInput: ""
    }
  },
  async fetch(){
    const getFilm = axios.get(`https://www.omdbapi.com/?s=java&apikey=6c3de1aa`)
    const response = await getFilm
    response.data.Search.forEach(element => this.films.push(element))
  },
  methods: {
    async filterFilms(value) {
      this.reset()
      if(!value || value ===" "){
        this.message = "No utilice espacios"
        this.callApi('java')
      } else if(value.length <3){
        this.message = "Debe introducir al menos 3 caracteres"
      } else {
        this.message = ""
        await this.callApi(value)
      }
    },
    reset(){
      this.films=[]
      this.numberFilms = 0
    },
    async callApi(value){
      const getFilm = axios.get(`https://www.omdbapi.com/?s=${value}&apikey=6c3de1aa`)
      this.res = await getFilm
      this.numberFilms = this.res.data.totalResults
      if(this.res.data.Response){this.message = "Lo siento, no hay coincidencias. Intentelo con otra palabra"}

      if(this.res.data.Search){this.printData(this.res.data)}
    },
    printData(collection){
      collection.Search.forEach(film =>this.films.push(film))
      if(this.numberFilms>10){this.message = "Afine la búsqueda"}
    }
  }
}
</script>

<style>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
/* ID */
#app{
  background-color: #e9e8e8;
  min-height: 100vh;
}
#headerNuxt{
  height: 140px;
  padding-top: 12px;
  background-color: #141414;
  color: #e9e8e8;
  box-shadow: 0 3px 10px 1px red;
  text-align: center;
}
#page{
  width: 90vw;
  margin: auto;
  padding: 10px 0 20px 0;
  text-align: center;
  min-height: calc(100vh - 140px - 44px);
}
#footerNuxt{
  background-color: rgb(179, 178, 178);
  height: 46px;
  padding-top: 4px;
  opacity: 0.7;
  text-align: center;
  color: rgb(24, 23, 23);
}
#search{
  width: 60vw;
  border-radius: 5px;
  height: 28px;
  margin-top: 10px;
  background-color: rgb(233, 173, 173);
}
#isComing{
  position: relative;
  top: -30px
}
#imgCinema{
  position: absolute;
  top: 0;
  left: 0;
  margin: 12px 0 12px 4vw;
  height: 130px;
}
#imgPopcorn{
  position: absolute;
  top: 30px;
  right: -160px;
  margin: 12px 0 10px 4vw;
  height: 130px;
}
#collectionFilms{
  margin: 20px 0;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: space-around;
  color: #e9e8e8;

}
#filmCard{
  width: 230px;
  min-height: 360px;
  padding: 10px;
  border: 2px solid #810202;
  border-radius: 8px;
  box-shadow: 0 0 10px 1px red;
  background-image: url("~static/bg.jpg");
  background-size: cover;
  background-repeat: no-repeat;
  color: #e9e8e8;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
#navIcon{
  position: absolute;
  top: 0.6rem;
  right: 20px;
}
#insta:hover{
  background: radial-gradient(circle at 30% 107%, #fdf497 0%, #fdf497 5%, #fd5949 45%, #d6249f 60%, #285AEB 90%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  transition: 0.6s ease;
}
#twitter:hover{color: #00acee;}
#facebook:hover{color: #3b5998;}
#youtube:hover{color: #b2071d;}

/* CLASES */
.title{
  font-size: 18px;
  color: #000000
}
.dates{
  color: #810202;
  font-weight: 500;
}
.img{
  max-height: 270px;
  max-width: 210px;
  border: #000000 2px solid;
  border-radius: 3px;
}
.icon{font-size: 1.2rem;
  margin: 3px;
  color: #e9e8e8
}
.icon:hover{
  font-size: 1.6rem;
  transition: 0.6s ease;
}
.code{
  font-size: 70px;
  color: #e9e8e8;
  position: relative;
  top: -24px;
}
.infoSearch{
  text-align: center;
  font-size: 18px;
}
.underline{color: red}

/* ETIQUETAS */
h2{font-size: 1.2rem;}
h4{
  color: #000000;
  font-weight: 300;
  text-align: left;
  font-size: 15px;
}
@media (max-width: 800px) {#imgCinema, #imgPopcorn{display: none;}}
@media (max-width: 540px) {#navIcon{display: none;}}
@media (max-width: 425px) {
  .code{display: none}
  #isComing{padding-top: 15px}
}
</style>
