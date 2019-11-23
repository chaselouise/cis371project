<template>
    <v-simple-table id="bg">
    <h1>{{currentUser}}</h1>
        <v-container id="FirstTable">
                <v-toolbar-title class="tbody">Admin Books</v-toolbar-title>
            <br><br>
        <v-row class="title">
            <v-col >Title</v-col>
            <v-col >Author</v-col>
        </v-row>
        <v-row class="adminbook" v-for="adminBook in adminBooks"  v-bind:key="adminBook.id">
            <v-col>   <v-chip >{{adminBook.genre}}</v-chip><br><br> {{adminBook.title}}</v-col>
            <v-col> {{adminBook.author}}</v-col>
            <router-link class="router" v-bind:to="{name: 'view-book', params: {book_id: adminBook.ID}}">
                <v-icon>mdi-eye</v-icon></router-link> 
        </v-row>
        <br><br>
        <!-- <router-link to="/new" class="router"><v-icon>mdi-plus-circle</v-icon></router-link> -->
    
    </v-container>


  <v-container id="SecondTable">
        <v-toolbar-title  class="tbody">User Books</v-toolbar-title>
          <br><br>
        <v-row class="title">
            <v-col >Title</v-col>

            <v-col >Author</v-col>
        </v-row>
        <v-row class= "userbook" v-for="book in books"  v-bind:key="book.id">
            <v-col><v-chip>{{book.genre}}</v-chip><br><br>{{book.title}}</v-col> 
           
            <v-col> {{book.author}}</v-col>
            <router-link class="router" v-bind:to="{name: 'user-new-book', params: {book_id: book.ID}}">
                <v-icon>mdi-eye</v-icon></router-link> 
        </v-row>
        <br><br>
        <router-link to="/new" class="router"><v-icon>mdi-plus-circle</v-icon></router-link>
    
    </v-container>



    </v-simple-table>



</template>


<script>
import { AppDB } from './firebaseInit'
import firebase from 'firebase';
export default {
    name: 'dashboard',
    data(){
        return{
            adminBooks:[],
            books: [],
            currentUser: false,
            userEmail:[],
            userList:'',
           
        }
    },

    created(){
      
          if(firebase.auth().currentUser){
         
            this.currentUser = firebase.auth().currentUser.email;
       
                this.userEmail =   this.currentUser.split("@")
          this.userList='User/'+this.userEmail[0];



        AppDB.ref(this.userList).on('value', (snapshot) => {
            snapshot.forEach((element) => {
                const data = {
                    'title': element.val().title,
                    'author': element.val().author,
                    'genre': element.val().genre,
                    'published': element.val().published,
                    'bookCount': element.val().bookCount,
                    'ID': element.val().ID

                }
               
                this.books.push(data);
            })
        })


        AppDB.ref('Books').on('value', (snapshot) => {
            snapshot.forEach((element) => {
                const data = {
                    'title': element.val().title,
                    'author': element.val().author,
                    'genre': element.val().genre,
                    'published': element.val().published,
                    'bookCount': element.val().bookCount,
                    'ID': element.val().ID

                }
               
                this.adminBooks.push(data);
            })
        })




     }}
}
</script>

<style  scoped>
@import url('https://fonts.googleapis.com/css?family=Vidaloka&display=swap');
.router{
      text-decoration: none; 
}

#FirstTable{
   
    text-align: center;
    width:49%;
   float: left;
   margin-left:auto; 
   margin-right:auto;
   border-style: solid;
  border-width: small;
  padding: 20px;
  border-color:black;
  font-family: 'Vidaloka', sans-serif;
 
  background: lightgray;
  border-color: black;
  box-shadow:5px 10px;
   }
   #FirstTable .title {
    
    font-size: 25px;
   margin-left:auto; 
   margin-right:auto;
   background-color:hsla(228, 70%, 19%, 0.75);
   border-radius: 25px;
   }
  #FirstTable  .adminbook {
   padding: 0.5em;
   border-style:'1px solid white';
   
   }
  .tbody {
      width: 20vw;
      align: center;
      font-size: 30px;
      font-weight: bold;
   border-style: solid;
  border-width: small;
   margin-left:auto; 
   margin-right:auto;
   border-radius: 25px;
   box-shadow:5px 10px;
   
  }
   
   #FirstTable .adminbook:nth-child(odd)
   {
     background:hsla(0, 2%, 58%, 0.84);
      border-radius: 25px;
      border-color: white;
   }
  #FirstTable .adminbook:nth-child(even)
   {
     background:hsla(230, 78%, 48%, 0.46);
      border-radius: 25px;
   }
   #SecondTable{
    
    text-align: center;
    width:49%;
   float: left;
   margin-left:auto; 
   margin-right:auto;
   border-style: solid;
  border-width: small;
  padding: 20px;
  border-color:black;
  font-family: 'Vidaloka', sans-serif;
  
  background: lightgray;
  border-color: black;
  box-shadow:5px 10px;
  margin-bottom:100px;
  
   }
   #SecondTable .title {
    
    font-size: 25px;
   margin-left:auto; 
   margin-right:auto;
   background-color:hsla(0, 77%, 20%, 0.82);
   border-radius: 25px;
   
   }
  #SecondTable  .userbook {
   padding: 0.5em;
   border-style:'1px solid black';
   
   
   }
  
   #SecondTable .userbook:nth-child(odd)
   {
     background:hsla(0, 2%, 58%, 0.84);
      border-radius: 25px;
      border-color: black;
   }
  #SecondTable .userbook:nth-child(even)
   {
     background:hsla(0, 84%, 16%, 0.52);
      border-radius: 25px;
   }
   
   
</style>
