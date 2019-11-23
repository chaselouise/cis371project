
<template>
 <form class="newbookform" @submit.prevent="saveBook" >
   <h2> Enter New Book </h2>
  <v-container fluid>
    <v-row>
      <v-col class="columns" cols="12" sm="6">
        <v-text-field solo v-model="book_id"  label="Book ID" >   </v-text-field>
        <v-text-field solo v-model="title" label="Title">  </v-text-field>
        <v-text-field solo v-model="author" label="Author">    </v-text-field>
        <v-text-field solo v-model="published" label="Date Published">    </v-text-field>

        <v-text-field solo v-model="genre" label="Genre"></v-text-field>
        <v-text-field solo v-model="bookCount" label="Stock">    </v-text-field>
        <v-btn class="button" type="submit"> Submit</v-btn>
        <v-btn class="button"> <router-link  to="/" class="router">Cancel</router-link> </v-btn>
      </v-col>
    </v-row>

  </v-container>
    </form>
</template>

<script>
import {AppDB} from './firebaseInit'
import firebase from 'firebase';
export default {
    name: 'new-book',
    data(){
        return{
            book_id: null,
            title: null,
            author: null,
            genre: null,
            published: null,
            bookCount: null,
            currentUser:false,
            userEmail:[],
            userList:''
            
        }
    },
    methods: {
        saveBook(){

          
          if(firebase.auth().currentUser){
         
            this.currentUser = firebase.auth().currentUser.email;
        }
                this.userEmail =   this.currentUser.split("@")
          this.userList='User/'+this.userEmail[0];

          AppDB.ref( this.userList).push().set({
                ID: this.book_id,
                title: this.title,
                author: this.author,
                genre: this.genre,
                published: this.published,
                bookCount: this.bookCount
            })
            .then(() => { this.$router.push('/') })
            .catch()
        }
    },
    created(){

    }
}
</script>

<style scoped>

.router{
      text-decoration: none;
      color:black; 
}
.button{ margin:3%;}


.newbookform {
display:flexbox;
    border-style: solid;
  border-width: small;
  padding: 20px;
  padding-left: 30px;
  padding-right: 30px;
 text-align: center;
 margin: 10px;
 font-family: 'Vidaloka', sans-serif;
  border-radius: 25px;
  background:hsla(228, 70%, 40%, 0.75);
  box-shadow: 5px 10px;

}
.columns {
   margin-left: auto;
    margin-right: auto;
    width: 20em;
    
}
h2 {
  font-size: 2em;
   width: 25vw;
      font-weight: bold;
   border-style: solid;
  border-width: small;
   margin-left:auto; 
   margin-right:auto;
   border-radius: 25px;
   box-shadow: 5px 10px;
}



</style>
