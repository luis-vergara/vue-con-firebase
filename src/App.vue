<template>
  <div id="app" class="container">
    <h1>Vue and Firebase</h1>
    <div class="card">
      <div class="card-header">
        <h3>Add A Link</h3>
      </div>
      <div class="card-body">
        <form v-on:submit.prevent="addLink" class="form-inline">
          <div class="form-group">
            <label for="">Title:</label>
            <input type="text" placeholder="Title" v-model="newLink.title" class="form-control" >
          </div>

          <div class="form-group">
            <label for="">Author:</label>
            <input type="text" placeholder="author" v-model="newLink.author" class="form-control" >
          </div>

          <div class="form-group">
            <label for="">URL:</label>
            <input type="text" placeholder="url" v-model="newLink.url" class="form-control" >
          </div>
          <div class="form-group">
            <input type="submit" class="btn btn-success form-control my-2" value="Add a Link">
          </div>
          
        </form>
      </div>
      <hr>

      <div class="car">
        <div class="card-header">
          <h3 class="card-title">
            Links List
          </h3>
        </div>
        <div class="card-body">
          <table class="table table-striped">
            <thead>
              <tr>
                  <th>Title</th>
                  <th>Author</th>
                  <th>Delete</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="link in links">
                  <td><a target="_blank" v-bind:href="link.url">{{link.title}}</a></td>
                  <td>{{link.author}}</td>
                  <td><button class="btn btn-danger" v-on:click="deleteLink(link)"><i class="fa fa-trash-o" aria-hidden="true"></i></button></td>
              </tr>
            </tbody>
          </table>
        </div>

      </div>
    </div>

  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import Firebase from 'firebase';
import toastr from 'toastr';

let config={
    apiKey: "AIzaSyCrHLZGgsSbOpdZkggjikixsabWlORim54",
    authDomain: "vuefire-def83.firebaseapp.com",
    databaseURL: "https://vuefire-def83.firebaseio.com",
    projectId: "vuefire-def83",
    storageBucket: "vuefire-def83.appspot.com",
    messagingSenderId: "339984649293"
};
let app =Firebase.initializeApp(config);
let db = app.database();
let linksRef = db.ref('links');
export default {
  name: 'App',
  firebase:{
    links: linksRef
  },
  data() {
    return {
      newLink:{
        title:'',
        author:'',
        url:''
      }
    }
  },
  methods:{
    addLink: function() {
     linksRef.push(this.newLink);
     this.newLink.title = '';
     this.newLink.author = '';
     this.newLink.url = '';
    },
    deleteLink:function (link){
      linksRef.child(link['.key']).remove();
      toastr.success('Link Eliminado')
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
