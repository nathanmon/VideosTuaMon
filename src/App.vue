

<template>
  <div id="app">
    <!-- titre et logo -->
    <div class="row">
      <div class="col-xs-4 col-xs-offset-4 text-center">
        <h1>Vidéos MBDS</h1>
      </div>
      <div class="col-xs-4">
        <img src="src/logo_mbds.jpg" class="img-responsive pull-right"/>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading" data-toggle="collapse" href="#addVideoPanel">
        <h2>Ajouter une vidéo</h2>
      </div>
      <div id="addVideoPanel" class="panel-collapse collapse panel-body">
        <div class="row col-xs-offset-1">
          <form id="form" v-on:submit.prevent="addVideo(newVideo)">
            <div class="row">
              <div :class=getTitreClass(newVideo.titre) >
                <label class="form-control-label" for="titre">Titre:</label>
                <input type="text" id="titre" class="form-control" v-model="newVideo.titre" >
              </div>
              <div :class=getUrlClass(newVideo.url)>
                <label class="form-control-label" for="url">URL:</label>
                <input type="text" id="url" class="form-control" v-model="newVideo.url" >
              </div>
            </div>
            <div class="row">
              <div :class=getDescClass(newVideo.desc) >
                <label class="form-control-label" for="desc">Description:</label>
                <input type="text" id="desc" class="form-control" v-model="newVideo.desc" >
              </div>
            </div>
            <div class="row col-xs-12"><br/></div>
            <div class="row col-xs-12">
              <input type="submit" :class=getAddBtnClass(newVideo) value="Ajouter">
            </div>
          </form>
        </div>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading" data-toggle="collapse" href="#searchVideoPanel">
        <h2>Rechercher une vidéo</h2>
      </div>
      <div id="searchVideoPanel" class="panel-collapse collapse panel-body">
        <div class="row col-xs-offset-1">
          <div class="row col-xs-10">
            <label class="form-control-label" for="searchInput">Titre:</label>
          </div>
          <div class="row">
            <div class="col-xs-4">
              <input id="searchInput" type="text" class="form-control col-xs-3" v-model="searchInput">
            </div>
            <div class="col-xs-1">
              <button type="button" class="btn btn-danger" v-on:click="clearSearch()"><span class="glyphicon glyphicon-remove"></span></button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading" data-toggle="collapse" href="#listVideoPanel">
          <h2>Liste des vidéos</h2>
      </div>
      <div id="listVideoPanel" class="panel-collapse collapse panel-body">
        <div class="row col-xs-12">
          <div class="btn-group pull-right">
            <div class="btn-group">
              <button type="button" class="btn btn-primary dropdown-toggle" data-toggle="dropdown">
                <span class="glyphicon glyphicon-sort"></span> Trier par ...</button>
              <div class="dropdown-menu pull-right" role="menu" >
                <div class="btn-group-vertical pull-right">
                  <button type="button" class="btn btn-primary" v-on:click="orderByDate()">Date</button>
                  <button type="button" class="btn btn-primary" v-on:click="orderByMoy()">Moyenne</button>
                </div>
              </div>
            </div>
            <button type="button" class="btn btn-danger"><span class="glyphicon glyphicon-trash"></span> Tout supprimer</button>
          </div>
        </div>
        <div class="row col-xs-12" align="center">
          <div class="btn-group">
            <button type="button" :class=prevBtnClass v-on:click="goPrevPage()"><span class="glyphicon glyphicon-chevron-left"></span></button>
            <button type="button" class="btn btn-primary" style="cursor:default" onmouseover="this.style.background='#337ab7';this.style.borderColor='#2e6da4'">{{currentPage}}</button>
            <button type="button" :class=nextBtnClass v-on:click="goNextPage()"><span class="glyphicon glyphicon-chevron-right"></span></button>
          </div>
        </div>
        <table class="table">
          <tr v-for="video in orderedVideos()" class="row col-xs-12">
            <Video :getVideoRank=getVideoRank :getMoyenne=getMoyenne :video=video :rmVideo=rmVideo :getTotal=getTotal :getNote=getNote
                   :addNote=addNote :myNotes=myNotes :rmNotes=rmNotes :totalVideos=videos.length :modifierVideo=modifierVideo></Video>
          </tr>
        </table>
        <div class="row col-xs-12" align="center">
          <div class="btn-group">
            <button type="button" :class=prevBtnClass v-on:click="goPrevPage()"><span class="glyphicon glyphicon-chevron-left"></span></button>
            <button type="button" class="btn btn-primary" style="cursor:default" onmouseover="this.style.background='#337ab7';this.style.borderColor='#2e6da4'">{{currentPage}}</button>
            <button type="button" :class=nextBtnClass v-on:click="goNextPage()"><span class="glyphicon glyphicon-chevron-right"></span></button>
          </div>
        </div>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading" data-toggle="collapse" href="#listVideoYT">
          <h2>Liste des vidéos de la chaine youtube</h2>
      </div>

      <div id="listVideoYT" class="panel-collapse collapse panel-body">
        <div class="btn-group pull-right">
          <button type="button" class="btn btn-primary" v-on:click="recupererChaineYT()"><span class="glyphicon glyphicon-import"></span> Importer chaîne MBDS</button>
        </div>
        <table class="table" id="YTData">
          <tr v-for="item in YTList" class="row col-xs-12">
            <VideoYT :item=item></VideoYT>
          </tr>
        </table>
      </div>
    </div>




    <div class="modal fade" id="modifierModal" role="dialog">
      <div class="modal-dialog modal-lg" >
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal"><span class="glyphicon glyphicon-remove"></span></button>
            <h2 class="modal-title">Modifier Vidéo</h2>
          </div>
          <div class="modal-body">
            <div class="row">
              <div class="col-xs-12">
                <div class="row">
                  <div :class=getTitreClass(videoAModifier.titre) >
                    <label class="form-control-label" for="titre2">Titre:</label>
                    <input type="text" id="titre2" class="form-control" v-model="videoAModifier.titre" >
                  </div>
                </div>
                <div class="row">
                  <div :class=getDescClass(videoAModifier.desc) >
                    <label class="form-control-label" for="desc2">Description:</label>
                    <input type="text" id="desc2" class="form-control" v-model="videoAModifier.desc" >
                  </div>
                </div>
                <div class="row col-xs-12"><br/></div>
                <div class="row col-xs-12">
                  <input :class=getRegBtnClass(videoAModifier) value="Enregistrer">
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>



<script>
  import Firebase from 'firebase'
  import Video from './components/Video.vue'
  import VideoYT from './components/VideoYT.vue'

  let config = {
    apiKey: "AIzaSyAxbhUwyrGYVlKe10Mr4tA8IS0iFeYwW4g",
    authDomain: "mbds-tua-mon.firebaseapp.com",
    databaseURL: "https://mbds-tua-mon.firebaseio.com",
    projectId: "mbds-tua-mon",
    storageBucket: "mbds-tua-mon.appspot.com",
    messagingSenderId: "594185516115"
  };
  let app = Firebase.initializeApp(config)
  let db = app.database()
  let videosRef = db.ref('videos')

  export default {
    name: 'app',
    components: {
      Video,
      VideoYT
    },
    firebase: {
      videos: videosRef
    },
    data () {
        return {
          newVideo: {
            titre: '',
            url: '',
            desc: ''
          },
          videoAModifier:  {
            titre: '',
            desc: '',
            url: ''
          },
          YTList: {},
          prevBtnClass: '',
          nextBtnClass: '',
          order: 'date',
          currentPage: 1,
          searchInput: '',
          myNotes: {}
        }
    },
    watch: {
      "searchInput": function(){
        this.currentPage=1
      }
    },
    methods: {
      recupererChaineYT() {
        var _this = this;
        $.getJSON('https://www.googleapis.com/youtube/v3/search?part=snippet&channelId=UC0eTHgZgfkQq3_rgpdggCEA&order=date&key=AIzaSyBpu8hgnXbkqFVWrAvwRUEz7T13ii3I7WM', function (json) {
            //console.log(json.items);
            json.items.pop();
            _this.YTList = json.items;
        });
      },
      addVideo(video){
        if(video.titre !== '' && video.desc !== '' && this.isUrlOk(video.url)) {
          video.url = this.urlToEmbed(video.url)
          videosRef.push(video)
          this.newVideo.titre = ''
          this.newVideo.url = ''
          this.newVideo.desc = ''
        }
      },
      modifierVideo(video){
        this.videoAModifier = Object.assign({},video)
      },
      regVideo(video){
        if(video.titre !== '' && video.desc !== '')
            videosRef.child(video['.key']).update({"titre":video.titre,"desc":video.desc})
      },
      rmVideo(video) {
        videosRef.child(video['.key']).remove()
      },
      rmNotes(video){
        videosRef.child(video['.key']).child("notes").remove()
        delete this.myNotes[video['.key']]
      },
      addNote(video, note){
        videosRef.child(video['.key']).child("notes").child(note).transaction(function(current){
          return (current || 0) + 1;
        })
        videosRef.child(video['.key']).child("notes").child("total").transaction(function(current){
          return (current || 0) + 1;
        })
        if(this.myNotes[video['.key']]!= undefined){
          videosRef.child(video['.key']).child("notes").child(this.myNotes[video['.key']]).transaction(function(current){
            return current - 1;
          })
          videosRef.child(video['.key']).child("notes").child("total").transaction(function(current){
            return current - 1;
          })
        }
        this.myNotes[video['.key']]=note;
      },
      getNote(video, note){
        if(video.notes!=null){
          return video.notes[note] || 0
        }
        return 0
      },
      orderedVideos(){
        let orderedCopy = this.searchByTitre().reverse()
        if (this.order === 'moy'){
          let orderedCopy2 = []
          for (let rank in this.videos)
            for (let i in orderedCopy)
              if (this.getVideoRank(orderedCopy[i]) === (Number(rank) + 1))
                orderedCopy2.push(orderedCopy[i])
          orderedCopy = orderedCopy2
        }
        if(this.hasNextPage())
          this.nextBtnClass = 'btn btn-primary'
        else
          this.nextBtnClass = 'btn btn-primary disabled'
        if(this.hasPrevPage())
          this.prevBtnClass = 'btn btn-primary'
        else
          this.prevBtnClass = 'btn btn-primary disabled'
        return orderedCopy.slice(5*(this.currentPage-1),5+5*(this.currentPage-1))
      },
      hasNextPage(){
        return this.searchByTitre().length>5+5*(this.currentPage-1)
      },
      hasPrevPage(){
        return this.currentPage>1
      },
      goNextPage(){
          if(this.hasNextPage())
              this.currentPage++
      },
      goPrevPage(){
          if(this.hasPrevPage)
              this.currentPage--
      },
      getVideoRank(video){
        let moy = this.getMoyenne(video)
        let rank = 1
        for(let i in this.videos) {
          if (moy === 'NaN') {
            if (this.getMoyenne(this.videos[i]) !== 'NaN')
              rank++
          }
          else if (this.getMoyenne(this.videos[i])!=='NaN'&&this.getMoyenne(this.videos[i]) > moy)
            rank++
        }
        return rank
      },
      orderByDate(){
        this.order='date'
      },
      orderByMoy(){
          this.order='moy'
      },
      urlToEmbed(url){
        let video_id = url.split("v=")[1]
        let ampersandPosition = video_id.indexOf('&')
        if(ampersandPosition != -1) {
          video_id = video_id.substring(0, ampersandPosition);
        }
        return "https://www.youtube.com/embed/"+video_id
      },
      getTotal(video){
        let total = 0;
        total+= this.getNote(video,5)
        total+= this.getNote(video,4)
        total+= this.getNote(video,3)
        total+= this.getNote(video,2)
        total+= this.getNote(video,1)
        return total
      },
      getMoyenne(video){
        let total = 0;
        total+= this.getNote(video,5)*5
        total+= this.getNote(video,4)*4
        total+= this.getNote(video,3)*3
        total+= this.getNote(video,2)*2
        total+= this.getNote(video,1)
        return (total/this.getTotal(video)).toFixed(2);
      },
      isUrlOk(url){
          if(url.length<1)
              return false
          if(url.split("v=").length<2)
              return false
        let embed = this.urlToEmbed(url)
        for ( let video in this.videos ) {
          if (embed === this.videos[video].url)
            return false
        }
        return true
      },
      getTitreClass(value){
        if(value !== '')
          return 'col-xs-4 has-success'
        return 'col-xs-4 has-error'
      },
      getDescClass(value){
        if(value !== '')
          return 'col-xs-10 has-success'
        return 'col-xs-10 has-error'
      },
      getUrlClass(url){
        if(this.isUrlOk(url))
          return 'col-xs-6 has-success'
        return 'col-xs-6 has-error'
      },
      getAddBtnClass(video) {
        if (video.titre !== '' && video.desc !== '' && this.isUrlOk(video.url))
          return 'btn btn-primary'
        return 'btn btn-primary disabled'
      },
      getRegBtnClass(video) {
        if (video.titre !== '' && video.desc !== '')
          return 'btn btn-primary'
        return 'btn btn-primary disabled'
      },
      searchByTitre(){
        let result = []
        for (let video in this.videos) {
          if (this.videos[video].titre.includes(this.searchInput))
            result.push(this.videos[video])
        }
        return result
      },
      clearSearch(){
          this.searchInput =''
      }
    }
  }
</script>
