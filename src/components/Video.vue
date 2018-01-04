<template>
  <div>
    <td class="col-xs-6">
      <iframe height="315" class="col-xs-12" :src="video.url" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe>
    </td>
    <td class="col-xs-6">
      <div class="row col-xs-12">
        <div class="col-xs-8">
          <p class="en_gros ">{{video.titre}}</p>
        </div>
        <div class="col-xs-4 btn-group">
          <button type="button" data-toggle="dropdown" class="btn btn-primary pull-right dropdown-toggle"><span class="glyphicon glyphicon-menu-hamburger"></span></button>
          <div class="dropdown-menu pull-right" role="menu" >
            <div class="btn-group-vertical pull-right">
              <button type="button" class="btn btn-primary" v-on:click=modifierVideo(video) data-toggle="modal" data-target="#modifierModal" ><span class="glyphicon glyphicon-edit"></span> Modifier</button>
              <button type="button" class="btn btn-danger" v-on:click=rmNotes(video)><span class="glyphicon glyphicon-refresh"></span> Supprimer notes</button>
              <button type="button" class="btn btn-danger" v-on:click=rmVideo(video)><span class="glyphicon glyphicon-trash"></span> Supprimer</button>
            </div>
          </div>
        </div>
      </div>
      <div class="row col-xs-12">
        <div class="col-xs-12 text-justify">
          {{video.desc}}<br/><br/>
        </div>
      </div>
      <div class="row col-xs-12">
        <div class="col-xs-8">
          <div class="row col-xs-12 noteBtn" v-for="note in 5" v-on:click="addNote(video, 6-note)">
            <div class="col-xs-6" :style="getStarsStyle(6-note)">
              <span v-for="iStar in 6-note" class="glyphicon glyphicon-star"></span><span v-for="iStar2 in note-1" class="glyphicon glyphicon-star-empty"></span> ({{getNote(video,6-note)}})
            </div>
            <div class="col-xs-6">
              <div class="progress ">
                <div class="progress-bar progress-bar-striped active" role="progressbar" :style="getWidth(6-note)"></div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-xs-4 text-center">
          <span class="badge" :style="getBadgeStyle()">{{getVideoRank(video)}}</span>
          <p :style="getMoyStyle()"><strong>{{getMoyenne(video)}}</strong></p>
        </div>
      </div>
    </td>
  </div>
</template>

<script>
  export default {
    name: 'Video',
    props: ['getVideoRank','getMoyenne','video','rmVideo','getTotal','getNote','addNote','myNotes', 'rmNotes', 'totalVideos','modifierVideo'],
    methods:{
      getStarsStyle(note){
        if(this.myNotes[this.video['.key']]===note)
          return 'min-width:130px;color:blue;'
        return 'min-width:130px'
      },
      getMoyStyle(){
        let moy = this.getMoyenne(this.video)
        if(moy!=='NaN')
          return 'font-family: "Lucida Console", Monaco, monospace;color:rgb('+(255-(moy-1)/4*255).toFixed(0)+','+((moy-1)/4*255).toFixed(0)+',0);font-size:300%;'
        return 'font-family: "Lucida Console", Monaco, monospace;font-size:300%;'
      },
      getBadgeStyle(){
        let rank = this.getVideoRank(this.video)
        return 'background-color:rgb('+((rank-1)/this.totalVideos*255).toFixed(0)+','+(255-(rank-1)/this.totalVideos*255).toFixed(0)+',0);font-size:300%;'
      },
      getWidth(note){
        return 'width:' + ( this.getNote(this.video, note) / this.getTotal(this.video) *100 || 0) + '%;background-color:rgb('+(255-(note-1)/4*255)+','+((note-1)/4*255)+',0);'
      }
    }
  }
</script>
