<template>
  <main class="main">
    
    <input type="text" class="input--search" v-model="search">
    <button class="btn--primary" @click="searchCharacter(search)">Buscar</button><br>
    
    <input type="radio" v-model="by_character" id="by_character">
    <label for="by_character">Character</label>
    <input type="radio" v-model="by_episode" id="by_episode">
    <label for="by_episode">Episode</label>
    <button>filters</button>
    <!-- Cambiar el v/if por alguna clase que solo se vea desabilitado el boton -->
    <button v-if="paginate.page > 1" v-on:click="changePage(paginate.page - 1)"> Anterior </button>
    <ul>
      <li>{{ paginate.page }}</li>
    </ul>
    <button v-if="paginate.page < paginate.pages" v-on:click="changePage(paginate.page + 1)"> Siguiente </button>

    <div v-for="character in characters.results" :key="character.id" class="articles">
      <div class="articles__paginate">
      </div>
      <AppArticleCharacter :character="character"/>
    </div>
  </main>
</template>

<script>
import AppArticleCharacter from "@/components/AppArticleCharacter.vue";
import { getCharacter } from 'rickmortyapi';

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
      search: '',
      by_character:true
    }
  },

  computed:{
    
  },

  methods:{
    async changePage(page){
      this.paginate.page = page <= 0 || page > this.paginate.pages ? this.paginate.page : page;
      this.characters = await getCharacter({page});
    },
    async searchCharacter(name){
      this.paginate.page = 1;
      this.characters = await getCharacter({name});
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
