<template>
  <div id="app">
    <img src="./assets/logo.png">
    <div class="panel-body">
    <table class="table table-striped  table-bordered">
      <thead class="thead-dark">
        <tr> 
          <th scope="col">Title</th>
          <th scope="col">Author</th>
          <th scope="col">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr scope="row" v-for="(book,index) in books" v-bind:key="index">
          <td><a v-bind:href="book.url">{{book.title}}</a></td>
          <td>{{book.name}}</td>
           <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeBook(book)"></span></td>
        </tr>
      </tbody>
    </table>
  </div>
    <!-- <router-view/> -->

    <!-- input form -->
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Add New Books</h3>
      </div>
      <div class="panel-body">
         <form id="form" class="form-inline" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="bookTitle">Title:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
          </div>
          <div class="form-group">
            <label for="bookAuthor">Author:</label>
            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.name">
          </div>
          <div class="form-group">
            <label for="bookUrl">Url:</label>
            <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
          </div>
          <input type="submit" class="btn btn-primary" value="Add Book">
        </form>
      </div>
    </div>
  </div>

<!-- input form -->


</template>

<script>
import Firebase from "firebase";
import toastr from 'toastr';
var config = {
  apiKey: "AIzaSyCxJlR4Gsx1Mtnd0giLxT0BBwxGJz8Tb2I",
  authDomain: "vue-firebase-fc786.firebaseapp.com",
  databaseURL: "https://vue-firebase-fc786.firebaseio.com",
  projectId: "vue-firebase-fc786",
  storageBucket: "vue-firebase-fc786.appspot.com",
  messagingSenderId: "359936481125"
};

var app = Firebase.initializeApp(config);

var db = app.database();

var booksRef = db.ref("books");

export default {
  name: "app",
  firebase: {
    books: booksRef
  },
  data() {
    return {
      newBook: {
        title: "",
        name: "",
        url: "http://"
      }
    };
  },
  methods: {
    addBook: function() {
      booksRef.push(this.newBook);
      this.newBook.name = "";
      this.newBook.name = "";
      this.newBook.url = "http://";
    },
    removeBook: function(book) {
      booksRef.child(book[".key"]).remove();
      toastr.success("Book removed successfully");
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>



