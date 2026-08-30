<script setup>
import { ref } from 'vue'

const posts = ref([
  {
    id: 1,
    title: 'test',
    image: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQAoVMNAU2XD0oxxc8l4fbdfficX4NB333EjjRl6UAxLw&s=10',
    likes: 10
  }
])

const form = ref({
  id: null,
  title: '',
  image: ''
})
const isEditing = ref(false)
const selectedPost = ref(null)

const savePost = () => {
  if (!form.value.title) {
    alert('กรุณากรอกข้อความ')
    return
  }

  if (isEditing.value) {
    const index = posts.value.findIndex(p => p.id === form.value.id)
    if (index !== -1) {
      posts.value[index] = { ...posts.value[index], ...form.value }
    }
  } else {
    const newPost = {
      id: Date.now(),
      title: form.value.title,
      image: form.value.image || 'https://picsum.photos',
      likes: 0
    }
    posts.value.unshift(newPost)
  }
  resetForm()
}

const editPost = (post) => {
  form.value = { ...post }
  isEditing.value = true
}

const deletePost = (id) => {
  if (confirm('ลบโพสนี้ใช่ไหม?')) {
    posts.value = posts.value.filter(post => post.id !== id)
  }
}

const viewPost = (post) => {
  selectedPost.value = post
}

const likePost = (id) => {
  const post = posts.value.find(p => p.id === id)
  if (post) {
    post.likes++
  }
}

const resetForm = () => {
  form.value = { id: null, title: '', image: '' }
  isEditing.value = false
}

</script>

<template>
  <div class="container">

    <div class="form-container">
      <h2>{{ isEditing ? 'แก้ไขโพสต์' : 'สร้างโพสต์ใหม่' }}</h2>
      <input v-model="form.title"  class="Post-1" type="text" placeholder="หัวข้อโพสต์" />
      <input v-model="form.image" class="Post-1" type="text" placeholder="URL รูปภาพ (เช่น https://picsum.photos)" />
      <button @click="savePost" class="btn-save">{{ isEditing ? 'Update' : 'Add' }}</button>
      <button v-if="isEditing" @click="resetForm" class="btn-cancel">Cancle</button>
    </div>

    <div class="posts-grid">
      <div v-for="post in posts" :key="post.id" class="post-card">
        <img :src="post.image || 'https://placeholder.com'" alt="Post Image" class="post-img" />
        
        <div class="post-body">
          <p>{{ post.title }}</p>
          <span class="like-count">{{ post.likes }} Like</span>
        </div>

        <div class="post-actions">
          <button @click="viewPost(post)" class="btn-view">View</button>
          <button @click="likePost(post.id)" class="btn-like">Like</button>
          <button @click="editPost(post)" class="btn-edit">Edit</button>
          <button @click="deletePost(post.id)" class="btn-delete">Delete</button>
        </div>
      </div>
    </div>

    <div v-if="selectedPost" class="modal-overlay" @click="selectedPost = null">
      <div class="modal-content" @click.stop>
        <h2>{{ selectedPost.title }}</h2>
        <img :src="selectedPost.image" alt="Full Image" class="modal-img" />
      </div>
      <button @click="selectedPost = null" class="btn-close">Close</button>
    </div>
  </div>
</template>

<style>
.btn-view {
    font-size: 20px;
    margin-right: 10px
}

.btn-like {
    font-size: 20px;
    margin-right: 10px
}

.btn-edit {
    font-size: 20px;
    margin-right: 10px
}

.btn-delete {
    font-size: 20px;
    margin-right: 10px
}

.btn-save{
    font-size: 20px;
    margin-right: 10px
}

.btn-cancel {
    font-size: 20px;
    margin-right: 10px
}
.Post-1 {
    font-size: 20px;
    margin-right: 10px
}

.post-img {
    margin-top: 15px;
}

.btn-close {
    font-size: 20px;
    margin-right: 10px
}
</style>