<template>
  <div>
    <td class="col-xs-6">
      <iframe height="315" class="col-xs-12" :src="idToUrl()" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>
    </td>
    <td class="col-xs-6">
      <div class="row col-xs-12">
        <div class="col-xs-8">
          <p class="en_gros ">{{item.snippet.title}}</p>
        </div>
      </div>
      <div class="row col-xs-12">
        <div class="col-xs-12 text-justify">
          {{item.snippet.description}}<br/><br/>
        </div>
      </div>
    </td>
  </div>
</template>

<script>
  export default {
    name: 'VideoYT',
    props: ['item'],
    methods:{
      idToUrl(){
        return 'https://www.youtube.com/embed/'+this.item.id.videoId;
      }
    }
  }
</script>