<template>
  <article class="article">
      <div class="article__image">
          <div class="article__image--img">
            <img :src="character.image" :alt="character.name">
          </div>
          <div class="article__image--name">
              <p>{{ character.name }}</p>
          </div>
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
    }
}
</script>
<style lang="scss">
.article {
    //max-width: 300px;
    //width: 100%;

    .article__image {
        position: relative;
        img {
            width: 100%;
        }
    }
}

.article__info--text {
    display: flex;
    justify-content: space-between;
    border-bottom: 1px solid #444;
    padding: 5px;

    span {
        color: #9e9e9e;
    }
    
    p {
        color: #ff9800;
        margin: 0;
    }
}

.article__info {
    background-color: #333;
    padding: 10px;
}

.article__image--name {
    position: absolute;
    bottom: 0px;
    width: 100%;
    background-color: #202329;
    opacity: 0.8;
    text-align: center;

    p {
        color: white;
        font-size: 20px;
        margin: 0px;
        padding: 10px 0px;
    }
}
</style>