<template>
  <div class="row">
  <div class="row col-xs-12">
    <iframe width="560" height="315" :src="url" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>
    <h3>{{titre}}</h3>
  </div>
  <div class="row col-xs-12">
    {{desc}}
  </div>
  </div>
</template>

<script>
export default {
  name: 'Video',
  props: ['titre', 'url', 'desc']
}
</script>
