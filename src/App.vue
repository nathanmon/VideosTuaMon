

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
      <div class="panel-heading">
        <h2>Ajouter une vidéo</h2>
      </div>
      <div class="panel-body">
        <div class="row col-xs-offset-1">
          <form id="form" v-on:submit.prevent="addVideo">
            <div class="row">
              <div :class=titreClass >
                <label class="form-control-label" for="titre">Titre:</label>
                <input type="text" id="titre" class="form-control" v-model="newVideo.titre" >
              </div>
              <div :class=urlClass >
                <label class="form-control-label" for="url">URL:</label>
                <input type="text" id="url" class="form-control" v-model="newVideo.url" >
              </div>
            </div>
            <div class="row">
              <div class='col-xs-10' >
                <label class="form-control-label" for="desc">Description:</label>
                <input type="text" id="desc" class="form-control" v-model="newVideo.desc" >
              </div>
            </div>
            <div class="row col-xs-12"><br/></div>
            <div class="row">
              <div class="col-xs-12">
                <input type="submit" :class=addBtnClass value="Ajouter">
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <div class="row">
          <div class="col-xs-6">
            <h2>Liste des vidéos</h2>
          </div>
          <div class="col-xs-6">
            <div class="btn-group pull-right">
              <button type="button" class="btn btn-danger"><span class="btn glyphicon glyphicon-trash"></span>Tout supprimer</button>
              <button type="button" class="btn btn-primary"><span class="btn glyphicon glyphicon-import"></span>Importer chaîne MBDS</button>
              <div class="btn-group">
                <button type="button" class="btn btn-primary dropdown-toggle" data-toggle="dropdown">
                  <span class="btn glyphicon glyphicon-sort"></span>Trier par ...</button>
                <div class="dropdown-menu pull-right" role="menu" >
                  <div class="btn-group-vertical pull-right">
                    <button type="button" class="btn btn-primary" v-on:click="orderByDate()">Date</button>
                    <button type="button" class="btn btn-primary" v-on:click="orderByMoy()">Moyenne</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>


      <div class="panel-body">
        <div class="row col-xs-12" align="center">
          <div class="btn-group">
            <button type="button" :class=prevBtnClass v-on:click="goPrevPage()"><span class="glyphicon glyphicon-chevron-left"></span></button>
            <button type="button" class="btn btn-primary" style="cursor:default" onmouseover="this.style.background='#337ab7';this.style.borderColor='#2e6da4'">{{currentPage}}</button>
            <button type="button" :class=nextBtnClass v-on:click="goNextPage()"><span class="glyphicon glyphicon-chevron-right"></span></button>
          </div>
        </div>
        <table class="table">
          <tr v-for="video in orderedVideos()" class="row col-xs-12">
            <td class="col-xs-6">
              <Video :titre="video.titre" :url="video.url" :desc="video.desc"></Video>
            </td>
            <td class="col-xs-6">
              <div class="row">
                <div class="col-xs-8">

                  <div class="col-xs-6">
                    <span class="badge en_gros">{{getVideoRank(video)}}</span>
                  </div>
                  <div class="col-xs-6">
                    <p :style="getMoyStyle(video)"><strong>{{getMoyenne(video)}}</strong></p>
                  </div>

                  <div class="row col-xs-12 noteBtn" v-for="note in 5" v-on:click="addNote(video, 6-note)">
                    <div class="col-xs-6" :style="getStarsStyle(video, 6-note)">
                      <span v-for="iStar in 6-note" class="glyphicon glyphicon-star"></span><span v-for="iStar2 in note-1" class="glyphicon glyphicon-star-empty"></span> ({{getNote(video,6-note)}})
                    </div>
                    <div class="col-xs-6">
                      <div class="progress ">
                        <div class="progress-bar progress-bar-striped active" role="progressbar" :style="getWidth(video, 6-note)"></div>
                      </div>
                    </div>
                  </div>

                </div>


                <div class="col-xs-4 btn-group">
                  <button type="button" data-toggle="dropdown" class="btn btn-primary pull-right dropdown-toggle"><span class="btn glyphicon glyphicon-menu-hamburger"></span></button>
                  <div class="dropdown-menu pull-right" role="menu" >
                    <div class="btn-group-vertical pull-right">
                      <button type="button" class="btn btn-primary" ><span class="btn glyphicon glyphicon-edit"></span>Modifier</button>
                      <button type="button" class="btn btn-danger" v-on:click=rmVideo(video)><span class="btn glyphicon glyphicon-trash"></span>Supprimer</button>
                    </div>
                  </div>
                </div>

              </div>
            </td>
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
  </div>
</template>

<script>
  import Firebase from 'firebase'
  import Video from './components/Video.vue'


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
  let myNotes = {}

  export default {
    name: 'app',
    components: {
      Video
    },
    firebase: {
      videos: videosRef,
      videosObj: { // can use keys, but v-for doesn't loop
        source: videosRef,
        asObject: true
      }
    },
    data () {
        return {
          newVideo: {
            titre: '',
            url: '',
            desc: ''
          },
          addBtnClass: 'btn btn-primary disabled',
          prevBtnClass: 'btn btn-primary disabled',
          nextBtnClass: 'btn btn-primary disabled',
          titreClass: 'col-xs-4',
          urlClass: 'col-xs-6',
          order: 'date',
          currentPage: 1
        }
    },
    watch: {
      "newVideo.titre": function(){
        this.checkNewTitre()
      },
      "newVideo.url": function(){
        this.checkNewUrl()
      }
    },
    methods: {
      orderedVideos(){
        let orderedCopy = this.videos.slice().reverse();
        if (this.order === 'moy'){
          let orderedCopy2 = []
          for (let rank in orderedCopy)
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
        return this.videos.length>5+5*(this.currentPage-1)
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
      addVideo(){
          if(this.isTitreOk()&&this.isUrlOk()) {
            let video2 = this.newVideo
            video2.url = this.urlToEmbed(video2.url)
            if (videosRef.push(video2)) {
              this.newVideo.titre = ''
              this.newVideo.url = ''
              this.newVideo.desc = ''
            }
          }
      },
      rmVideo(video) {
          videosRef.child(video['.key']).remove();
      },
      addNote(video, note){
        videosRef.child(video['.key']).child("notes").child(note).transaction(function(current){
          return (current || 0) + 1;
        })
        videosRef.child(video['.key']).child("notes").child("total").transaction(function(current){
          return (current || 0) + 1;
        })
        if(myNotes[video['.key']]!= undefined){
          videosRef.child(video['.key']).child("notes").child(myNotes[video['.key']]).transaction(function(current){
            return current - 1;
          })
          videosRef.child(video['.key']).child("notes").child("total").transaction(function(current){
            return current - 1;
          })
        }
        myNotes[video['.key']]=note;
      },
      getNote(video, note){
        if(this.videosObj[video['.key']].notes!=null){
          return this.videosObj[video['.key']].notes[note] || 0
        }
        return 0
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
      getStarsStyle(video, note){
        if(myNotes[video['.key']]===note)
          return 'min-width:130px;color:blue;'
        return 'min-width:130px'
      },
      getMoyStyle(video){
        let moy = this.getMoyenne(video)
        if(moy!=='NaN')
          return 'font-family: "Lucida Console", Monaco, monospace;color:rgb('+(255-(moy-1)/4*255).toFixed(0)+','+((moy-1)/4*255).toFixed(0)+',0);font-size:300%;'
        return 'font-family: "Lucida Console", Monaco, monospace;font-size:300%;'
      },
      getWidth(video, note){
          return 'width:' + ( this.getNote(video, note) / this.getTotal(video) *100 || 0) + '%;background-color:rgb('+(255-(note-1)/4*255)+','+((note-1)/4*255)+',0);'
      },
      isTitreOk(){
        for ( let video in this.videos ) {
          if (this.newVideo.titre === this.videos[video].titre || this.newVideo.titre === '') {
            return false
          }
        }
        return true
      },
      isUrlOk(){
          if(this.newVideo.url.length<1)
              return false
          if(this.newVideo.url.split("v=").length<2)
              return false
        let embed = this.urlToEmbed(this.newVideo.url)
        for ( let video in this.videos ) {
          if (embed === this.videos[video].url)
            return false
        }
        return true
      },
      checkNewTitre(){
        if(this.isTitreOk())
          this.titreClass = 'col-xs-4 has-success'
        else
          this.titreClass = 'col-xs-4 has-error'
        this.checkNewVideo()
      },
      checkNewUrl(){
        if(this.isUrlOk())
          this.urlClass = 'col-xs-6 has-success'
        else
          this.urlClass = 'col-xs-6 has-error'
        this.checkNewVideo()
      },
      checkNewVideo() {
        if (this.isTitreOk() && this.isUrlOk())
          this.addBtnClass = 'btn btn-primary'
        else
          this.addBtnClass = 'btn btn-primary disabled'
      }
    }
  }
</script>
