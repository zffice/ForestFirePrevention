<template>
  <div class="components-container">
    <div>
      <!-- <tinymce v-model="content"
               :height="300" /> -->
      <three-dome />
    </div>
  </div>
</template>

<script>
import Tinymce from '@/components/Tinymce'
import ThreeDome from './threeDome.vue'

export default {
  name: 'TinymceDemo',
  components: {
    Tinymce,
    ThreeDome
  },
  data () {
    return {
    }
  }
}
</script>

<style scoped>
.editor-content {
  margin-top: 20px;
}
</style>

