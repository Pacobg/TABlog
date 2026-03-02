<template>
  <div :class="clz" v-html="markdownHtml"></div>
  <div class="raw-link">
    <router-link :to="rawRoute" class="btn btn-sm btn-outline-secondary">
      View raw markdown
    </router-link>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'

const props = defineProps({
  title: String,
  clz: String,
  markdownHtml: String
})

const route = useRoute()
const rawRoute = computed(() => ({ name: 'PostRaw', params: { title: props.title } }))
</script>

<style scoped>
.raw-link {
  margin-top: 2rem;
}
</style>
