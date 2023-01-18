<script setup>
  import { ref, computed, onMounted } from 'vue';
  import PaginatePost from './components/PaginatePost.vue';
  import BlogPost from './components/BlogPost.vue';
  import LoadingSpinner from './components/LoadingSpinner.vue';


  const favorito = ref('')
  const postXPage = 10
  const inicio = ref(0)
  const fin = ref(postXPage)
  const loading = ref(true)

  const posts = ref([])

  const cambiarFavorito = (title) => {
    favorito.value = title
  }

  const next = () => {
     inicio.value = inicio.value + postXPage
     fin.value = fin.value + postXPage 
  }
  const prev = () => {
    inicio.value = inicio.value - postXPage
    fin.value = fin.value - postXPage
  }

  // onMounted(async() => {
  //   loading.value = true
  //   try{
  //     const resp = await fetch('https://jsonplaceholder.typicode.com/posts')
  //     posts.value = await resp.json()
  //   }catch(error){
  //     console.log(error)
  //   }finally{
  //     loading.value = false
  //   }
  // })
  const fetchData = async() => {
    loading.value = true
    try{
      const resp = await fetch('https://jsonplaceholder.typicode.com/posts')
      posts.value = await resp.json()
    }catch(error){
      console.log(error)
    }finally{
      loading.value = false
    }
  }

  fetchData()
  // fetch('https://jsonplaceholder.typicode.com/posts')
  //   .then(res => res.json())
  //   .then((data) => posts.value = data)
  //   .finally(() => loading.value = false)
  

  const maxLength = computed(() => posts.value.length)
</script>

<template>
  <LoadingSpinner v-if="loading"/>
  <div class="container" v-else>
    <h1>App</h1>
    <h2>Mi post Favorito: {{favorito}} </h2>
    <ButtonCounter />
    <PaginatePost 
      class="mb-2"
      :inicio="inicio"
      :fin="fin"
      :maxLength="maxLength"
      @next="next"
      @prev="prev"
    />
    <BlogPost
      v-for="post in posts.slice(inicio,fin)"
      :key="post.id"
      :body="post.body"
      :id="post.id"
      :title="post.title"
      @cambiarFavoritoNombre="cambiarFavorito"
      class="mb-2"
    />
  </div>
</template>