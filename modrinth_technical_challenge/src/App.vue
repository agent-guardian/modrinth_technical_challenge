
<script setup>
import { ref } from 'vue'

const modDownloads = ref(0)
const slug = ref(null)
const modName = ref(null)
const hasError = ref(false)
const errorText = ref(null)

async function fetchData(){
  //if(slug.value == null)
    //return;
  modName.value = null
  hasError.value = false
  
  const res = await fetch(
    `https://api.modrinth.com/v2/project/${slug.value}`)
  
  if(res.status !== 200){
    errorText.value = `No such mod with slug ${slug.value} or unauthorized`
    hasError.value = true
  }
  
  var resJson = await res.json()
  modName.value = resJson["title"]
  modDownloads.value = resJson["downloads"]
}
</script>

<template>
  <form @submit.prevent="fetchData()">
    <input v-model="slug" placeholder="Enter Mod Slug Here..." style="margin-right: .5rem;"/>
    <button>Submit</button>
  </form>

  <p v-if="modName !== null">{{ modName }} has {{ modDownloads }} downloads!</p>
  <p v-if="hasError" style="color: red;">{{ errorText }}</p>
</template>

