

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
        <h2>Liste des vidéos</h2>
      </div>
      <div class="panel-body">
        <table class="table">
          <tr v-for="video in orederedVideos()">
            <td width="600">
              <Video :titre="video.titre" :url="video.url" :desc="video.desc"></Video>
            </td>
            <td>
              <div class="row">
                <div class="col-xs-8">
                  <div class="row col-xs-offset-2 col-xs-6">
                    <p class="en_gros"><strong>{{getMoyenne(video)}}</strong></p>
                  </div>

                  <div class="row col-xs-12 noteBtn" role="button" v-on:click="addNote(video, '5')">
                    <div class="col-xs-6" style="min-width:130px">
                      <span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span> ({{getNote(video, '5')}})
                    </div>
                    <div class="col-xs-6">
                      <div class="progress progress-striped">
                        <div class="progress-bar progress-bar-success" role="progressbar" :style="getWidth(video, '5')"></div>
                      </div>
                    </div>
                  </div>
                  <div role="button" class="row col-xs-12 noteBtn" v-on:click="addNote(video, '4')">
                    <div class="col-xs-6"  style="min-width:130px">
                        <span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star-empty"></span> ({{getNote(video, '4')}})
                      </div>
                    <div class="col-xs-6">
                      <div class="progress progress-striped">
                        <div class="progress-bar progress-bar-success" role="progressbar" :style="getWidth(video, '4')"></div>
                      </div>
                    </div>
                  </div>
                  <div role="button" class="row col-xs-12 noteBtn" v-on:click="addNote(video, '3')">
                    <div class="col-xs-6"  style="min-width:130px">
                        <span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star-empty"></span><span class="glyphicon glyphicon-star-empty"></span> ({{getNote(video, '3')}})
                      </div>
                    <div class="col-xs-6">
                      <div class="progress progress-striped">
                        <div class="progress-bar progress-bar-info" role="progressbar" :style="getWidth(video, '3')"></div>
                      </div>
                    </div>
                  </div>
                  <div role="button" class="row col-xs-12 noteBtn" v-on:click="addNote(video, '2')">
                    <div class="col-xs-6"  style="min-width:130px">
                        <span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star-empty"></span><span class="glyphicon glyphicon-star-empty"></span><span class="glyphicon glyphicon-star-empty"></span> ({{getNote(video, '2')}})
                      </div>
                    <div class="col-xs-6">
                      <div class="progress progress-striped">
                        <div class="progress-bar progress-bar-warning" role="progressbar" :style="getWidth(video, '2')"></div>
                      </div>
                    </div>
                  </div>
                  <div role="button" class="row col-xs-12 noteBtn" v-on:click="addNote(video, '1')">
                    <div class="col-xs-6"  style="min-width:130px">
                        <span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star-empty"></span><span class="glyphicon glyphicon-star-empty"></span><span class="glyphicon glyphicon-star-empty"></span><span class="glyphicon glyphicon-star-empty"></span> ({{getNote(video, '1')}})
                      </div>
                    <div class="col-xs-6">
                      <div class="progress progress-striped">
                        <div class="progress-bar progress-bar-danger" role="progressbar" :style="getWidth(video, '1')"></div>
                      </div>
                    </div>
                  </div>
                </div>


                <div class="col-xs-4 btn-group">
                  <button type="button" data-toggle="dropdown" class="btn btn-primary pull-right dropdown-toggle"><span class="btn glyphicon glyphicon-menu-hamburger"></span></button>
                  <div class="dropdown-menu pull-right" role="menu" >
                    <div class="btn-group-vertical pull-right">
                      <button type="button" class="btn btn-warning " ><span class="btn glyphicon glyphicon-edit"> Modifier</span></button>
                      <button type="button" class="btn btn-warning " v-on:click=rmVideo(video)><span class="btn glyphicon glyphicon-trash"> Supprimer</span></button>
                    </div>
                  </div>
                </div>

              </div>
            </td>
          </tr>
        </table>
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
          addBtnClass:'btn btn-primary disabled',
          titreClass:'col-xs-4',
          urlClass:'col-xs-6'
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
        orederedVideos(){
            return this.videos.slice().reverse();
        },
      urlToEmbed:function(url){
        let video_id = this.newVideo.url.split("v=")[1]
        let ampersandPosition = video_id.indexOf('&')
        if(ampersandPosition != -1) {
          video_id = video_id.substring(0, ampersandPosition);
        }
        return "https://www.youtube.com/embed/"+video_id
      },
      addVideo: function(){
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
      rmVideo: function(video) {
          videosRef.child(video['.key']).remove();
      },
      addNote: function(video, note){
        videosRef.child(video['.key']).child("notes").child(note).transaction(function(current){
          return (current || 0) + 1;
        })
        videosRef.child(video['.key']).child("notes").child("total").transaction(function(current){
          return (current || 0) + 1;
        })
      },
      getNote: function(video, note){
        if(this.videosObj[video['.key']].notes!=null){
          return this.videosObj[video['.key']].notes[note] || 0
        }
        return 0
      },
      getTotal: function(video){
        let total = 0;
        total+= this.getNote(video,5)
        total+= this.getNote(video,4)
        total+= this.getNote(video,3)
        total+= this.getNote(video,2)
        total+= this.getNote(video,1)
        return total
      },
      getMoyenne: function(video){
        let total = 0;
        total+= this.getNote(video,5)*5
        total+= this.getNote(video,4)*4
        total+= this.getNote(video,3)*3
        total+= this.getNote(video,2)*2
        total+= this.getNote(video,1)
        return (total/this.getTotal(video)).toFixed(2);
      },
      getWidth: function(video, note){
          return 'width:' + ( this.getNote(video, note) / this.getTotal(video) *100 ) + '%'
      },
      isTitreOk:function(){
        for ( let video in this.videos ) {
          if (this.newVideo.titre === this.videos[video].titre || this.newVideo.titre === '') {
            return false
          }
        }
        return true
      },
      isUrlOk:function(){
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
      checkNewTitre: function(){
        if(this.isTitreOk())
          this.titreClass = 'col-xs-4 has-success'
        else
          this.titreClass = 'col-xs-4 has-error'
        this.checkNewVideo()
      },
      checkNewUrl: function(){
        if(this.isUrlOk())
          this.urlClass = 'col-xs-6 has-success'
        else
          this.urlClass = 'col-xs-6 has-error'
        this.checkNewVideo()
      },
      checkNewVideo: function() {
        if (this.isTitreOk() && this.isUrlOk())
          this.addBtnClass = 'btn btn-primary'
        else
          this.addBtnClass = 'btn btn-primary disabled'
      }
    }
  }
</script>
