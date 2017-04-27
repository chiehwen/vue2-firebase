<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue 2 & Firebase Sample Applcation</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add Film</h3>
      </div>
      <div class="panel-body">
        <form id="form" class="form-inline" v-on:submit.parent="addFilm">
          <div class="form-group">
            <label for="filmName">Name:</label>
            <input type="text" id="filmName" class="form-control" v-model="newFilm.name">
          </div>
          <div class="form-group">
            <label for="filmDirector">Director:</label>
            <input type="text" id="filmDirector" class="form-control" v-model="newFilm.director">
          </div>
          <div class="form-group">
            <label for="filmUrl">Url:</label>
            <input type="text" id="filmUrl" class="form-control" v-model="newFilm.url">
          </div>
          <input type="submit" class="btn btn-primary" value="Add Film">
        </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Films Lists</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>
                Name
              </th>
              <th>
                Director
              </th>
              <th>
                Delete
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="film in films">
              <td>
                <a v-bind:href="film.url">{{film.name}}</a>
              </td>
              <td>
                {{film.director}}
              </td>
              <td>
                <span class="glyphicon glyphicon-trash" v-on:click="removeFilm(film)"></span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    
    <router-view></router-view>
  </div>
</template>

<script>
import Firebase from 'firebase'
import toastr from 'toastr'

let config = {
  apiKey: "AIzaSyDlSnNZfs2ufgIv_PRImclFfki3jtMnUJo",
  authDomain: "vue2-firebase-9f4b9.firebaseapp.com",
  databaseURL: "https://vue2-firebase-9f4b9.firebaseio.com",
  projectId: "vue2-firebase-9f4b9",
  storageBucket: "vue2-firebase-9f4b9.appspot.com",
  messagingSenderId: "1053687295341"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let filmsRef = db.ref('films');

export default {
  name: 'app',
  firebase: {
    films: filmsRef
  },
  data () {
    return {
      newFilm: {
        name: '',
        director: '',
        url: ''
      }
    }
  },
  methods: {
    addFilm: function() {
      console.log(this.newFilm);
      filmsRef.push(this.newFilm);
      toastr.success("Add successful")
      this.newFilm.name = '';
      this.newFilm.director = '';
      this.newFilm.url = '';
    },
    removeFilm: function(film) {
      console.log();
      filmsRef.child(film['.key']).remove();
      toastr.warning("Film removed")
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
</style>
