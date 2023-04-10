<template>
  <div class="editor">
    <div class="editor__wrapper">
      <!-- <ParagraphBlock />
      <ImageBlock /> -->
      <EditorBlock />
    </div>
  </div>
</template>

<script>

import EditorBlock from '@/components/EditorBlock.vue'
// import ParagraphBlock from '@/components/ParagraphBlock.vue'
// import ImageBlock from '@/components/ImageBlock.vue'

export default {
  name: 'HomeView',

  components: {
    // ParagraphBlock,
    // ImageBlock,
    EditorBlock
  },
  data() {
    return {
      blocks: [
        { type: 'paragraph' },
        { type: 'image' },
        { type: 'list' }
      ]
    }
  }
}
</script>