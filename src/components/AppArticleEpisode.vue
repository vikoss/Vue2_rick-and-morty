<template>
  <article class="article">
      <div class="article__title">
          <h2>{{ episode.name }}</h2>
      </div>
      <div class="article__body">
          <div class="article__info--text">
              <span>Air Date</span>
              <p>{{ episode.air_date }}</p>
          </div>
          <div class="article__info--text">
              <span>Episode</span>
              <p>{{ episode.episode }}</p>
          </div>
          <div class="article__info--text">
              <span>Characters</span>
              <div class="article__info--list">
                <p class="article__info--list"
                    v-for="(character, key) in episode.characters"
                    :key="key">
                        {{ character }}
                </p>
              </div>
          </div>
      </div>
  </article>
</template>

<script>
export default {
    name: 'AppArticleEpisode',
    props: {
        episode: {
            type: Object,
            default: () => {}
        }
    },

}
</script>

<style>
.article__title {
    color: white;
    text-align: center;
}
.article__info--list {
    display: flex;
    flex-direction: column;
}
</style>