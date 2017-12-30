

<template>
  <div id="app">

    <div class="panel panel-default">
      <div class="panel-heading">
        <h2>Ajouter une vidéo</h2>
      </div>
      <div class="panel-body">
        <form id="form" v-on:submit.prevent="addVideo">
          <div class="form-group">
          <div class="row">
            <div class="col-xs-2">
              <label for="titre">Titre:</label>
              <input type="text" id="titre" class="form-control" v-model="newVideo.titre">
            </div>
            <div class="col-xs-6">
              <label for="url">URL:</label>
              <input type="text" id="url" class="form-control" v-model="newVideo.url">
            </div>
          </div>
          <div class="row">
            <div class="col-xs-8">
              <label for="desc">Description:</label>
              <input type="text" id="desc" class="form-control input-lg" v-model="newVideo.desc">
            </div>
          </div>
          <div class="row col-xs-12">
          </div>
          <div class="row">
            <div class="col-xs-12">
              <input type="submit" class="btn btn-primary" value="Ajouter">
            </div>
            </div>
          </div>
        </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h2>Liste des vidéos</h2>
      </div>
      <div class="panel-body">
        <table class="table">
          <tr v-for="video in videos">
            <td width="600">
              <Video :titre="video.titre" :url="video.url" :desc="video.desc"></Video>
            </td>
            <td>

              <div class="row col-xs-offset-2">
                <p style='font-family: "Lucida Console", Courier, monospace; font-size:300%;'><strong>{{getMoyenne(video)}}</strong></p>
              </div>

              <div class="row col-xs-12">
                <div class="row" role="button" v-on:click="addNote(video, '5')">
                  <div class="col-xs-2 text-right" style="min-width:130px">
                    <span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span> ({{getNote(video, '5')}})
                  </div>
                  <div class="col-xs-6">
                    <div class="progress progress-striped">
                      <div class="progress-bar progress-bar-success" role="progressbar" :style="getWidth(video, '5')">
                        <span class="sr-only"></span>
                      </div>
                    </div>
                  </div>
                </div>
                <div role="button" class="row" v-on:click="addNote(video, '4')">
                  <div class="col-xs-2 text-right"  style="min-width:130px">
                      <span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star-empty"></span> ({{getNote(video, '4')}})
                    </div>
                  <div class="col-xs-6">
                    <div class="progress progress-striped">
                      <div class="progress-bar progress-bar-success" role="progressbar" :style="getWidth(video, '4')">
                        <span class="sr-only"></span>
                      </div>
                    </div>
                  </div>
                </div>
                <div role="button" class="row" v-on:click="addNote(video, '3')">
                  <div class="col-xs-2 text-right"  style="min-width:130px">
                      <span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star-empty"></span><span class="glyphicon glyphicon-star-empty"></span> ({{getNote(video, '3')}})
                    </div>
                  <div class="col-xs-6">
                    <div class="progress progress-striped">
                      <div class="progress-bar progress-bar-info" role="progressbar" :style="getWidth(video, '3')">
                        <span class="sr-only"></span>
                      </div>
                    </div>
                  </div>
                </div>
                <div role="button" class="row" v-on:click="addNote(video, '2')">
                  <div class="col-xs-2 text-right"  style="min-width:130px">
                      <span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star-empty"></span><span class="glyphicon glyphicon-star-empty"></span><span class="glyphicon glyphicon-star-empty"></span> ({{getNote(video, '2')}})
                    </div>
                  <div class="col-xs-6">
                    <div class="progress progress-striped">
                      <div class="progress-bar progress-bar-warning" role="progressbar" :style="getWidth(video, '2')">
                        <span class="sr-only"></span>
                      </div>
                    </div>
                  </div>
                </div>
                <div role="button" class="row" v-on:click="addNote(video, '1')">
                  <div class="col-xs-2 text-right"  style="min-width:130px">
                      <span class="glyphicon glyphicon-star"></span><span class="glyphicon glyphicon-star-empty"></span><span class="glyphicon glyphicon-star-empty"></span><span class="glyphicon glyphicon-star-empty"></span><span class="glyphicon glyphicon-star-empty"></span> ({{getNote(video, '1')}})
                    </div>
                  <div class="col-xs-6">
                    <div class="progress progress-striped">
                      <div class="progress-bar progress-bar-danger" role="progressbar" :style="getWidth(video, '1')">
                        <span class="sr-only"></span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="row col-xs-12"><br/></div>
              <div class="row col-xs-12">
                <span class="btn glyphicon glyphicon-trash" v-on:click="rmVideo(video)" >Supprimer</span>
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
            }
        }
    },
    methods: {
      addVideo: function(){
        let video2= this.newVideo
        let video_id = video2.url.split("v=")[1]
        let ampersandPosition = video_id.indexOf('&')
        if(ampersandPosition != -1) {
          video_id = video_id.substring(0, ampersandPosition);
        }
        video2.url= "https://www.youtube.com/embed/"+video_id
        if(videosRef.push(video2)) {
          alert("video "+video2.titre+" ajoutée !")
          this.newVideo.titre = ''
          this.newVideo.url = ''
          this.newVideo.desc = ''
        }
      },
      rmVideo: function(video) {
        if(confirm("Supprimer vidéo "+video.titre+" ?")) {
          videosRef.child(video['.key']).remove();
        }
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
        if(this.videosObj[video['.key']].notes!=null)
          return this.videosObj[video['.key']].notes[note] || 0
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
      }
    }
  }
</script>
