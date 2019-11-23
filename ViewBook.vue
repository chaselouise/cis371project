
<template class="bgcard">
  <v-card id="card" max-width="600" tile>
      
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title class="one"><b>Book ID:</b>  {{ID}}</v-list-item-title>
      </v-list-item-content>
    </v-list-item>

    <v-list-item two-line>
      <v-list-item-content >
        <v-list-item-title class="one"><b>Author:</b> {{author}}</v-list-item-title>
      </v-list-item-content>
    </v-list-item>

    <v-list-item three-line>
      <v-list-item-content>
        <v-list-item-title class="one"><b>Genre:</b> {{genre}}</v-list-item-title>
      </v-list-item-content>
    </v-list-item>


    <v-list-item four-line>
      <v-list-item-content>
        <v-list-item-title class="one"><b>Date of Publication:</b> {{published}}</v-list-item-title>
      </v-list-item-content>
    </v-list-item>


    <v-list-item five-line>
      <v-list-item-content>
        <v-list-item-title class="one"><b>Stock:</b> {{bookCount}}</v-list-item-title>
      </v-list-item-content>
    </v-list-item>
<v-btn rounded color="primary" dark class ="button"> <router-link to="/" class="router"> Back </router-link></v-btn>
<v-btn rounded color="primary" dark class ="button" v-on:click="deleteBook" > Delete </v-btn>
  </v-card>

</template>
<script>
import {AppDB} from './firebaseInit'
export default {
    name: 'view-book',
    data(){
        return{
            ID: null,
            title: null,
            author: null,
            genre: null,
            published: null,
            bookCount: null
        }
    },
    beforeRouteEnter(to, from, next){
        AppDB.ref('Books').on('value', (snapshot) => {
            const data = snapshot.val();
            const keys = Object.keys(data);

            let find;
            keys.forEach((key) => {
                let book = data[key];
                if(book.ID == to.params.book_id){
                    find = book;
                }
            });

            next(vm => {
                vm.ID = find.ID;
                vm.title = find.title;
                vm.author = find.author;
                vm.genre = find.genre;
                vm.published = find.published;
                vm.bookCount = find.bookCount;
            })
        })
    },
    methods: {
        deleteBook(){
            if(confirm('Are you sure you want to delete this book?')){
                let uID;
                 AppDB.ref('Books').on('value', (snapshot)=>{
                    const data = snapshot.val();
                    const keys = Object.keys(data);
                    
                    keys.forEach((key) => {
                        let book = data[key];
                       // console.log(book);
                        if(book.ID == this.$route.params.book_id){
                            uID = key;
                        }
                    });

                })

                AppDB.ref('Books/' + uID).remove();
                this.$router.push("/");
            }
        }      
    }
}
</script>
<style scoped>
@import url('https://fonts.googleapis.com/css?family=Vidaloka&display=swap');

#card {
 
   margin-top: 100px;
   margin-left: auto;
   margin-right: auto; /* Added */
        float: none; /* Added */
        margin-bottom: 400px; /* Added */
        background-color: darkgray;
         border-style: solid;
         border-color: black;
          border-radius: 25px;
          box-shadow: 5px 10px black;
          font-family: 'Vidaloka', sans-serif;
          padding: 100px;
          background:hsla(228, 70%, 40%, 0.75);
          
}
.bgcard {
  background: lightgray;

}

.router{
      text-decoration: none;
      color:white;
       
      
}
.one{
  font-size:1.5em;
}
.button{
    margin:5%;
     margin-left:auto; 
   margin-right:auto;
   float: right;
   
}


</style>