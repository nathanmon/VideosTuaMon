

<template>
  <div id="app">

    <div class="panel panel-default">
      <div class="panel-heading">
        <h2>Ajouter une vidéo</h2>
      </div>
      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.prevent="addVideo">
          <div class="form-group">
            <label for="titre">Titre:</label>
            <input type="text" id="titre" class="form-control" v-model="newVideo.titre">
          </div>
          <div class="form-group">
            <label for="titre">URL:</label>
            <input type="text" id="url" class="form-control" v-model="newVideo.url">
          </div>
          <input type="submit" class="btn btn-primary" value="Ajouter">
        </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h2>Liste des vidéos</h2>
      </div>
      <div class="panel-body">
        <table>
          <tr v-for="video in videos">
            <td>
              <h3>{{video.titre}}</h3>
              {{video.url}}
              <Video titre="a" url="b"></Video>
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
      videos: videosRef
    },
    data () {
        return {
            newVideo: {
              titre: '',
              url: ''
            }
        }
    },
    methods: {
        addVideo: function(){
            videosRef.push(this.newVideo)
        }
    }
  }
</script>
