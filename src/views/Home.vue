<template>
  <main class="main">
    
    
    <div class="main__search">
      <input type="text" class="input--search" v-model="searchPayload.name" placeholder="Search by name of character...">
      <button class="btn--primary" @click="search(searchPayload)">Buscar</button>
      
      <!--  Poner estilos de curosr al tag a-->
      <a class="btn--showFilters" @click="viewSearchFilters">{{ searchFilters ? 'Less filters':'More filters' }}</a>
      
      <div v-if="searchFilters" class="main__search--filters">
        
        <div class="main__search--type">
          <input type="radio" v-model="typeSearch" id="character" value="character">
          <label for="character">Character</label>
          <input type="radio" v-model="typeSearch" id="episode" value="episode">
          <label for="episode">Episode</label><br>
        </div>

        <div v-if="typeSearch == 'character'" class="search--filters">
          <!-- filtros para personajes -->
          <!--input type="text" class="input--search" v-model="searchPayload.status" placeholder="Search by status"><br-->
          <!-- Busqueda por estaus -->
          <div class="filters--bloque">
            <div class="filters--title">
              <p>Status</p>
            </div>
            <div class="filters--options">
              <div>
                <label for="alive">Alive</label>
                <input type="radio" v-model="searchPayload.status" id="alive" value="alive">
              </div>
              <div>
                <label for="dead">Dead</label>
                <input type="radio" v-model="searchPayload.status" id="dead" value="dead">
              </div>
              <div>
                <label for="unknown">Unknown</label>
                <input type="radio" v-model="searchPayload.status" id="unknown" value="unknown">
              </div>  
            </div>
          </div>

          <div class="filters--bloque">
            <div class="filters--title">
              <p>Gender</p>
            </div>
            <div class="filters--options"> 
              <div>
                <label for="female">Female</label>
                <input type="radio" v-model="searchPayload.gender" id="male" value="male">
              </div>
              <div>
                <input type="radio" v-model="searchPayload.gender" id="female" value="female">
                <label for="male">Male</label>
              </div>
              <div>
                <input type="radio" v-model="searchPayload.gender" id="genderless" value="genderless">
                <label for="genderless">Genderless</label>
              </div>
              <div>
                <input type="radio" v-model="searchPayload.gender" id="unknown" value="unknown">
                <label for="unknown">Unknown</label>
              </div>
            </div>
          </div>
          
          
            <input type="text" class="input--search" v-model="searchPayload.species" placeholder="Search by species">
            <input type="text" class="input--search" v-model="searchPayload.type" placeholder="Search by type">
          
          
          <!--input type="text" class="input--search" v-model="searchPayload.gender" placeholder="Search by gender"><br-->
        </div>
        
        <div v-if="typeSearch == 'episode'">
          <!-- filtros para episodios -->
          <input type="text" class="input--search" v-model="searchPayload.episode" placeholder="Search by episode code ex: S01E10"><br>
        </div>
      </div>

    </div>

    <div v-if="characters.results" class="articles">
      
      <AppArticleCharacter 
        v-for="character in characters.results" 
        :key="character.id" 
        :character="character" />

    </div>
    <div v-if="episodes.results" class="articles">

      <app-article-episode
        v-for="episode in episodes.results"
        :key="episode.id"
        :episode="episode"/>

    </div>

    <div class="paginate">
      <button 
        :disabled="paginate.page > 1 ? false : true"
        :class="paginate.page > 1 ? 'btn--paginate' : 'btn--paginate-dis'" 
        v-on:click="changePage(paginate.page - 1)">
          Anterior
      </button>
      
      <p>{{ paginate.page }}</p>

      <button
        :disabled="paginate.page < paginate.pages ? false : true"
        :class="paginate.page < paginate.pages ? 'btn--paginate' : 'btn--paginate-dis'" 
        v-on:click="changePage(paginate.page + 1)">
          Siguiente
      </button>
    </div>
    
  </main>
</template>

<script>
import AppArticleCharacter from "@/components/AppArticleCharacter.vue";
import AppArticleEpisode from "@/components/AppArticleEpisode.vue";
import { getCharacter, getEpisode } from 'rickmortyapi';

export default {
  name: "Home",
  components: { AppArticleCharacter, AppArticleEpisode },

  data() {
    return {
      characters: {},
      episodes: {},
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
        episode: '',
      },
      searchFilters:false,
      typeSearch: 'character',
    }
  },
  methods:{
    async changePage(page){
      /*
      * Valido que la pagina no sea menor o igual a 0 y que no sea mayor al numero de paginas maximas.
      * Entonces realizo el request
      */
      this.paginate.page = page <= 0 || page > this.paginate.pages ? this.paginate.page : page;
      this.search({page});

    },
    async search(payload){
      this.paginate.page = !payload.page ? 1 : payload.page;
      
      if(this.typeSearch == 'character'){
        this.episodes = {};
        this.characters = await getCharacter({
          name:payload.name || this.searchPayload.name, 
          status:payload.status || this.searchPayload.status,
          species:payload.species || this.searchPayload.species,
          type:payload.type || this.searchPayload.type,
          gender:payload.gender || this.searchPayload.gender,
          page:this.paginate.page
        });
        this.paginate.pages = this.characters.info.pages;
      }else{
        this.characters = {};
        this.episodes = await getEpisode({
          name:payload.name,
          episode:payload.episode
          });
        this.paginate.pages = this.episodes.info.pages;
      }
    },
    viewSearchFilters(){
      this.searchFilters = !this.searchFilters;
      this.typeSearch = 'character';
      this.searchPayload = {
        name: '',
        status: '',
        species: '',
        type: '',
        gender: '',
        episode: '',
      };

    }
  },

  async created(){

    this.characters = await getCharacter();
    
    //this.paginate.count = this.characters.info.count;
    this.paginate.pages = this.characters.info.pages;
    //this.paginate.next = this.characters.info.next;
    //this.paginate.prev = this.characters.info.prev;
  }

}
</script>
<style lang="scss" scoped>

.filters--bloque {
  display: flex;
  flex-direction: column;
  border: 1px solid;
}
.filters--title {
  p{
    text-align: center;
  }
}
.filters--options {
  display: flex;
  justify-content: space-evenly;
}
.main__search--type {
  display: flex;
  justify-content: center;
}

.main__search {
  display: flex;
  flex-direction: column;
}

.btn--showFilters {
  cursor: pointer;
  color: #ff9800;
  text-align: center;
  margin-bottom: 10px;
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
  color: white;
}

.paginate {
  display: flex;
  justify-content: flex-end;
  margin: 10px 0px;

  .btn--paginate {
    background-color: #ff9800;
    color: white;
    cursor: pointer;
    outline: none;
    border-style: hidden;
  }
  .btn--paginate-dis {
    background-color: #fbcb83;
    color: white;
    outline: none;
    border-style: hidden;
  }
  p {
    //margin: 15px;
    margin: 0px;
    padding: 10px;
    border-bottom: 1px solid #202329;
    border-top: 1px solid #202329;
  }
}
.input--search {
  
  border: 0px;
  border-bottom: 1px solid #202329;
  padding: 5px;
  outline: none;
  margin-bottom: 10px;
}

@media only screen and (min-width: 660px) {
  .articles {
    grid-template-columns: repeat(2, 1fr);
  }
  .main__search {
    width: 300px;
    float: right;
  }
}

@media only screen and (min-width: 960px) {
  .articles {
    grid-template-columns: repeat(3, 1fr);
  }
  .main__search {
    width: 400px;
    float: right;
  }
}

@media only screen and (min-width: 1260px) {
  .articles {
    grid-template-columns: repeat(4, 1fr);
  }
  .main__search {
    width: 400px;
    float: right;
  }
}
  
</style>
