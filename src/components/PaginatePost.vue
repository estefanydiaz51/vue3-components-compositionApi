<script setup>
  defineProps(['inicio', 'fin', 'maxLength'])

</script>

<template>
  <div class="btn-group" role="group" aria-label="Basic example">
    <button 
      type="button" 
      class="btn btn-outline-primary"
      :disabled="inicio === 0"
      @click="$emit('prev')"
    >
      Previus
    </button>
    <button 
      type="button" 
      class="btn btn-outline-primary"
      :disabled="fin >= maxLength"
      @click="$emit('next')"
    >
      Next
    </button>
  </div>
</template>