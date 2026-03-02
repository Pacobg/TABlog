<template>
  <div class="px-2 py-3" style="user-select: text;">
    <h1 class="post-title">{{ post.title }}</h1>
    <div class="post-attribute">
      <p><strong>Created:</strong> {{ post.created_time || 'n/a' }}</p>
      <p><strong>Tags:</strong>
        <span v-for="tag in post.tags" :key="tag" class="post-tag small">{{ tag }}</span>
      </p>
      <p><strong>Markdown file:</strong>
        <a :href="originalUrl" target="_blank">Open raw Markdown</a>
      </p>
    </div>
    <hr class="split-line" />
    <pre class="raw-md" v-text="rawMarkdown"></pre>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import { usePostStore } from '@/stores/post'

const route = useRoute()
const postStore = usePostStore()
const rawMarkdown = ref('')
const post = ref(postStore.getPostByTitle(route.params.title))
const originalUrl = ref('#')

const fetchRawMarkdown = async () => {
  if (!post.value) return
  try {
    const url = postStore.currentMarkdownUrl
    originalUrl.value = url
    const resp = await fetch(url)
    if (!resp.ok) throw new Error('Failed to load markdown')
    rawMarkdown.value = await resp.text()
  } catch (error) {
    rawMarkdown.value = `Error: ${error.message}`
  }
}

onMounted(fetchRawMarkdown)
</script>

<style scoped>
.raw-md {
  white-space: pre-wrap;
  font-family: 'JetBrains Mono', 'Fira Code', monospace;
  font-size: 0.95rem;
  background: rgba(0,0,0,0.05);
  padding: 1rem;
  border-radius: 6px;
}
</style>
