<template>
  <div id="app">
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title" @click="onClickAddBook">Add New Books</h3>
      </div>
      <transition name="fade">
        <div class="panel-body" v-if="editing">
          <form id="form" class="form-inline" v-on:submit.prevent="addMusic">
            <div class="form-group">
              <label for="musicTitle">Title:</label>
              <input type="text" id="musicTitle" class="form-control" v-model="newMusic.title">
            </div>
            <div class="form-group">
              <label for="musicAuthor">Author:</label>
              <input type="text" id="musicAuthor" class="form-control" v-model="newMusic.author">
            </div>
            <div class="form-group">
              <label for="musicType">Type:</label>
              <input type="text" id="musicType" class="form-control" v-model="newMusic.type">
            </div>
            <div class="form-group">
              <label for="musicDate">Date:</label>
              <input type="text" id="musicDate" class="form-control" v-model="newMusic.date">
            </div>
            <div class="form-group">
              <label for="musicUrl">Url:</label>
              <input type="text" id="musicUrl" class="form-control" v-model="newMusic.url">
            </div>
            <input type="submit" class="btn btn-primary" value="Add music">
          </form>
        </div>
      </transition>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Title</th>
              <th>Author</th>
              <th>Type</th>
              <th>Date</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="music in musics">
              <td><a v-bind:href="music.url">{{music.title}}</a></td>
              <td>{{music.author}}</td>
              <td>{{music.type}}</td>
              <td>{{music.date}}</td>
              <td><span v-on:click="editMusic(music)">Edit</span><span v-on:click="removeMusic(music)">Remove</span></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import Firebase from 'firebase'

var config = {
    apiKey: "AIzaSyBPzNT5HNjWtDMBtLeSOZsurQ54Wph8AzM",
    authDomain: "musical-quizz.firebaseapp.com",
    databaseURL: "https://musical-quizz.firebaseio.com",
    projectId: "musical-quizz",
    storageBucket: "musical-quizz.appspot.com",
    messagingSenderId: "1064959468460"
  };
  
let firebase = Firebase.initializeApp(config)
let db = firebase.database()

let musicsRef = db.ref('musics')

export default {
  name: 'app',
  firebase: {
    musics: musicsRef
  },
  data () {
    return {
      editing: false,
      msg: 'Welcome to Your Vue.js App',
      newMusic: {
        title: '',
        author: '',
        type: '',
        date: '',
        url: ''
      }
    }
  },
  methods: {
    onClickAddBook () {
      this.editing = true
    },
    addMusic () {
      console.log(this.newMusic)
      if (this.newMusic.key) {
        // update
        musicsRef.child(this.newMusic.key).update(this.newMusic)
      } else {
        // add
        musicsRef.push(this.newMusic);
      }
      // reset newMusic
      this.newMusic.title = '';
      this.newMusic.author = '';
      this.newMusic.type = '';
      this.newMusic.date = '';
      this.newMusic.url = 'http://';
      this.editing = false
    },
    editMusic (music) {
      this.newMusic = music

    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.fade-leave, .fade-enter-to {
    opacity: 1;
}
.fade-leave-to,.fade-enter {
    opacity: 0;
}
.fade-leave-active, .fade-enter-active {
    transition: opacity 0.5s ease;
}
</style>
