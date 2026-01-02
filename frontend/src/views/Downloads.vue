<template>
  <div class="min-h-screen m-2">
    <h1 class="m-4 text-xl">Downloads</h1>

    <div v-if="downloads.length === 0">
      <div class="alert alert-info shadow-lg">
        <span>No downloads available.</span>
      </div>
    </div>

    <div v-else class="grid gap-2">
      <div
        v-for="(file, index) in downloads"
        :key="index"
        class="card card-bordered shadow-lg bg-base-100"
      >
        <div class="card-body flex-row items-center justify-between">
          <span class="truncate">{{ file }}</span>

          <button class="btn btn-square btn-ghost" @click="download(file)">
            <Icon icon="clarity:download-line" class="h-6 w-6" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { Icon } from '@iconify/vue'
import { computed } from 'vue'
import { useDownloadsStore } from '../stores/downloads'

const store = useDownloadsStore()

const downloads = computed(() => store.downloads)

/**
 * Corrige URLs que vêm assim:
 * /downloads/%2Fdownloads%2FArquivo.mp3
 */
function normalizeDownloadUrl(url) {
  // Decodifica caso venha com %2F
  let decoded = decodeURIComponent(url)

  // Remove qualquer "/downloads/" duplicado no começo
  decoded = decoded.replace(/^\/?downloads\/+/g, '')

  // Monta a URL final correta
  return `/downloads/${decoded}`
}

function download(file) {
  const url = normalizeDownloadUrl(file)

  const a = document.createElement('a')
  a.href = url
  a.download = url.split('/').pop()
  document.body.appendChild(a)
  a.click()
  document.body.removeChild(a)
}
</script>

<style scoped></style>
