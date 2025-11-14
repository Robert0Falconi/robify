<template>
  <q-layout view="lHh Lpr lFf" class="bg-black text-white">
    <!--TOP NAV -->
    <q-header style="background-color: #101010; opacity: 0.8" class="flex items-center justify-between"
      height-hint="60">
      <div class="flex items-center ml-4">
        <q-btn dense round flat @click="goBack">
          <ChevronLeft fillColor="#FFFFFF" :size="30" />
        </q-btn>
        <q-btn dense round flat class="ml-2" @click="goForward">
          <ChevronRight fillColor="#FFFFFF" :size="30" />
        </q-btn>
      </div>

      <!-- Menu utente -->
      <div flat round dense class="bg-black hover:bg-[#282828] mr-8" @click="openMenu = !openMenu">
        <div class="flex items-center align-center justify-center">
          <img class="rounded-full" width="27" style="margin-right: 10px;" src="public/icons/grimer.png" />
          <div class="text-white text-[14px] ml-1.5 font-semibold pe-3 text-uppercase">Roberto Falconi</div>
          <div class="d-flex align-center justify-center">
          </div>
        </div>
      </div>

      <!-- Dropdown menu -->
      <q-menu v-model="openMenu" transition-show="jump-down" transition-hide="jump-up">
        <q-list style="min-width: 180px" class="bg-[#282828] text-gray-200 font-semibold">
          <q-item clickable v-ripple>
            <q-item-section>Profile</q-item-section>
          </q-item>
          <q-separator dark />
          <q-item clickable v-ripple>
            <q-item-section>Log out</q-item-section>
          </q-item>
        </q-list>
      </q-menu>
    </q-header>

    <!--SIDENAV -->
    <q-drawer show-if-above width="240" class="bg-black text-white">
      <div class="pt-6 flex flex-col items-center">
        <RouterLink to="/">
          <img class="mt-4 logomenu" width="125" src="/images/icons/spotify-logo.png" />
        </RouterLink>

        <div class="my-8"></div>

        <q-list class="w-full">
          <RouterLink to="/">
            <MenuItem class="no-decoration ml-[1px]" :iconSize="23" name="Home" iconString="home" pageUrl="/" />
          </RouterLink>
          <RouterLink to="/search">
            <MenuItem class="no-decoration ml-[1px]" :iconSize="24" name="Search" iconString="search" pageUrl="/search" />
          </RouterLink>
          <RouterLink to="/library">
            <MenuItem class="no-decoration ml-[2px]" :iconSize="23" name="Your Library" iconString="library" pageUrl="/library" />
          </RouterLink>

          <!-- <div class="py-3.5"></div> -->

          <!-- <MenuItem :iconSize="24" name="Create Playlist" iconString="playlist" pageUrl="/playlist" />
          <MenuItem class="-ml-[1px]" :iconSize="27" name="Liked Songs" iconString="liked" pageUrl="/liked" /> -->

        </q-list>
      </div>
    </q-drawer>

    <!--CONTENUTO PRINCIPALE -->
    <q-page-container class="bg-gradient-to-b from-[#1C1C1C] to-black overflow-auto">
      <div class="mt-[70px]"></div>
      <router-view />
      <div class="mb-[100px]"></div>
    </q-page-container>

    <!--MUSIC PLAYER -->
    <MusicPlayer v-if="currentTrack" />
  </q-layout>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import MenuItem from 'components/MenuItem.vue'
import MusicPlayer from 'components/MusicPlayer.vue'
import ChevronRight from 'vue-material-design-icons/ChevronRight.vue'
import ChevronLeft from 'vue-material-design-icons/ChevronLeft.vue'

import { useSongStore } from 'stores/song'
import { storeToRefs } from 'pinia'

const router = useRouter()
const openMenu = ref(false)

const useSong = useSongStore()
const { isPlaying, currentTrack } = storeToRefs(useSong)

onMounted(() => {
  isPlaying.value = false
})

const goBack = () => router.back()
const goForward = () => router.forward()
</script>
