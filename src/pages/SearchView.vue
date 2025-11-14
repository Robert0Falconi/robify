<template>
  <q-page class="bg-dark text-white q-pa-xl">
    <div class="text-h5 text-weight-bold hover-underline cursor-pointer">Browse all</div>

    <q-separator dark spaced class="q-my-md" color="grey-8" />

    <div class="row q-col-gutter-md q-mt-sm">
      <div v-for="(item, index) in categoryData" :key="index" class="col-12 col-sm-6 col-md-3 col-lg-3 col-xl-2">
        <CategorySelect :category="item.category" :image="item.image" />
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import CategorySelect from 'src/components/CategorySelect.vue'
import axios from 'axios'

// Separiamo nome visivo e nome per la query
const categories = ref([
  { label: 'Musica', query: 'Music' },
  { label: 'Podcast', query: 'Podcast' },
  { label: 'Estate', query: 'Summer Music' },
  { label: 'Rap', query: 'Rap Music' },
  { label: 'Pop', query: 'Pop Music' },
  { label: 'Indie', query: 'Indie Music' },
  { label: 'Rock', query: 'Rock Music' },
  { label: 'Jazz', query: 'Jazz Music' },
  { label: 'Relax', query: 'Relax Music' },
  { label: 'Gaming', query: 'Gaming Music' },
  { label: 'Reggaeton', query: 'Reggaeton Music' },
  { label: 'Italiana', query: 'Italian Music' },
])

const categoryData = ref([])

const UNSPLASH_ACCESS_KEY = 'jCHKslSsqx28YEkeXIVQCH-NzwI04waFZxegbqNl2OE'

// Funzione per ottenere un’immagine da Unsplash in base al nome della categoria
async function fetchImageForCategory(query) {
  try {
    const res = await axios.get('https://api.unsplash.com/search/photos', {
      params: {
        query,
        per_page: 1,
        orientation: 'squarish',
        client_id: UNSPLASH_ACCESS_KEY,
      },
    })
    const result = res.data.results[0]
    return result ? result.urls.small : 'https://picsum.photos/300/300'
  } catch (error) {
    console.error(`Errore caricando immagine per ${query}`, error)
    return 'https://picsum.photos/300/300'
  }
}

// Caricamento immagini al montaggio
onMounted(async () => {
  const fetched = await Promise.all(
    categories.value.map(async (cat) => ({
      category: cat.label,
      image: await fetchImageForCategory(cat.query),
    })),
  )
  categoryData.value = fetched
})
</script>



<style scoped>
.hover-underline:hover {
  text-decoration: underline;
}
</style>
