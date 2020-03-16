<template>
  <article class="article">
      <div class="article__image">
          <img :src="character.image" :alt="character.name">
      </div>
      <div class="article__info">
          <div class="article__info--text">
              <span>STATUS</span>
              <p>{{ character.status }}</p>
          </div>
          <div class="article__info--text">
              <span>SPECIES</span>
              <p>{{ character.species }}</p>
          </div>
          <div class="article__info--text">
              <span>GENDER</span>
              <p>{{ character.gender }}</p>
          </div>
          <div class="article__info--text">
              <span>ORIGIN</span>
              <p>{{ character.origin.name }}</p>
          </div>
          <div class="article__info--text">
              <span>LAST LOCATION</span>
              <p>{{ character.location.name }}</p>
          </div>
      </div>
  </article>
</template>

<script>
export default {
    name: 'AppArticleCharacter',
    props: {
        character: {
            type: Object,
            default: () => {}
        }
    },
    created(){
        console.log(this.character)
    }
    
}
</script>