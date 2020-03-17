<template>
    <div class="paginate">

        <!-- Cambiar el v/if por alguna clase que solo se vea desabilitado el boton -->
        <button v-if="paginate.page > 1" v-on:click="changePage(paginate.page - 1)"> Anterior </button>
        
        <p>{{ paginate.page }}</p>
        
        <button v-if="paginate.page < paginate.pages" v-on:click="changePage(paginate.page + 1)"> Siguiente </button>
    </div>
</template>

<script>
import { getCharacter, getEpisode } from 'rickmortyapi';

export default {
    name: 'AppPaginate',
    props:{
        paginate:{
            type: Object,
            default: () => {}
        }
    },

    methods: {
        async changePage(page){
            this.paginate.page = page <= 0 || page > this.paginate.pages ? this.paginate.page : page;
            this.characters = await getCharacter({page});
        },
    }
}
</script>

<style>

</style>