<template>
  <div class="hello">
    {{titre+url}}
  </div>
</template>

<script>
export default {
  name: 'Video',
  props: ['titre', 'url']
}
</script>
