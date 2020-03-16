<template>
  <main class="main">

    <div class="main__title">
      <h1>The Rick And Morty</h1>
    </div>
    
    <input type="text" class="input--search" v-model="searchPayload.name" placeholder="Search by name...">
    <button class="btn--primary" @click="search(searchPayload)">Buscar</button><br>
    
    <input type="radio" v-model="typeSearch" id="character" value="character">
    <label for="character">Character</label>
    <input type="radio" v-model="typeSearch" id="episode" value="episode">
    <label for="episode">Episode</label><br>
    <!--  Poner estilos de curosr al tag a-->
    <a @click="searchFilters = !searchFilters">filters</a>
    <div v-if="searchFilters">
      <div v-if="typeSearch == 'character'" class="search--filters">
        <!-- filtros para personajes -->
        <!--input type="text" class="input--search" v-model="searchPayload.status" placeholder="Search by status"><br-->
        <!-- Busqueda por estaus -->
        <label for="by-status">Estatus</label>
        <input type="radio" v-model="searchPayload.status" id="alive" value="alive">
        <label for="alive">Alive</label>
        <input type="radio" v-model="searchPayload.status" id="dead" value="dead">
        <label for="dead">Dead</label>
        <input type="radio" v-model="searchPayload.status" id="unknown" value="unknown">
        <label for="unknown">Unknown</label><br>

        <label for="by-gender">Gender</label>
        <input type="radio" v-model="searchPayload.gender" id="female" value="female">
        <label for="female">Female</label>
        <input type="radio" v-model="searchPayload.gender" id="male" value="male">
        <label for="male">Male</label>
        <input type="radio" v-model="searchPayload.gender" id="genderless" value="genderless">
        <label for="genderless">Genderless</label>
        <input type="radio" v-model="searchPayload.gender" id="unknown" value="unknown">
        <label for="unknown">Unknown</label><br>
        
        <input type="text" class="input--search" v-model="searchPayload.especies" placeholder="Search by species"><br>
        <input type="text" class="input--search" v-model="searchPayload.type" placeholder="Search by type"><br>
        
        <!--input type="text" class="input--search" v-model="searchPayload.gender" placeholder="Search by gender"><br-->
      </div>
      
      <div v-if="typeSearch == 'episode'">
        <!-- filtros para episodios -->
        <input type="text" class="input--search" v-model="searchPayload.episode" placeholder="Search by number of episode"><br>
      </div>
    </div>

    <!-- Cambiar el v/if por alguna clase que solo se vea desabilitado el boton -->
    <button v-if="paginate.page > 1" v-on:click="changePage(paginate.page - 1)"> Anterior </button>
    <ul>
      <li>{{ paginate.page }}</li>
    </ul>
    <button v-if="paginate.page < paginate.pages" v-on:click="changePage(paginate.page + 1)"> Siguiente </button>

    <div class="articles">
      
      <AppArticleCharacter 
        v-for="character in characters.results" 
        :key="character.id" 
        :character="character" />

    </div>
    
  </main>
</template>

<script>
import AppArticleCharacter from "@/components/AppArticleCharacter.vue";
import { getCharacter, getEpisode } from 'rickmortyapi';

export default {
  name: "Home",
  components: { AppArticleCharacter },

  data() {
    return {
      characters: {},
      paginate: {
        pages: 1,
        page: 1
      },
      searchPayload: {
        name: '',
        status: '',
        species: '',
        type: '',
        gender: '',
        episode: 1,
      },
      by_character:true,
      by_episodes:false,
      
      searchFilters:false,
      typeSearch: 'character',
      //searchEstatus



    }
  },

  computed:{
    
  },

  methods:{
    async changePage(page){
      this.paginate.page = page <= 0 || page > this.paginate.pages ? this.paginate.page : page;
      this.characters = await getCharacter({page});
    },
    async search(payload){
      this.paginate.page = 1;
      
      if(this.typeSearch == 'character'){
        this.characters = await getCharacter({
          name:payload.name, 
          status:payload.status,
          especies:payload.especies,
          type:payload.type,
          gender:payload.gender
        });
      }else{
        this.characters = await getEpisode({
          name:payload.name,
          episode:payload.episode
          });
      }
      //this.paginate.page = 1;
      //this.characters = await getCharacter({name});
    }
  },

  async created(){

    this.characters = await getCharacter();
    
    this.paginate.count = this.characters.info.count;
    this.paginate.pages = this.characters.info.pages;
    this.paginate.next = this.characters.info.next;
    this.paginate.prev = this.characters.info.prev;
  }

}
</script>
<style lang="scss" scoped>

.main__title {
  background-color: #202329;
  margin: 0px 20%;
  
  h1 {
    text-align: center;
    color: #ff9800;
    text-transform: uppercase;
    padding: 10px 0px;
  }
}

.articles {
  width: 100%;
  max-width: 1260px;
  margin: 0 auto;
   
  display: grid;
   
  grid-template-columns: 1fr;
  grid-template-rows: auto;
  grid-gap: 20px;

  background-color: #202329;
  padding-top: 30px;
}

.btn--primary {
  outline: none;
  border: 0px;
  //border-radius: 10px;
  padding: 5px;
  cursor: pointer;
  background-color: #ff9800;
  color: #202329;
}
.input--search {
  
  border: 0px;
  border-bottom: 1px solid black;
  padding: 5px;
  outline: none;
}

@media only screen and (min-width: 660px) {
  .articles {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media only screen and (min-width: 960px) {
  .articles {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media only screen and (min-width: 1260px) {
  .articles {
    grid-template-columns: repeat(4, 1fr);
  }
}
  
</style>
