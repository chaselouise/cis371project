<template>
<div id="loginform">
  <v-form  @submit.prevent="saveUser">
        <v-text-field label="Username/Email" v-model="email"></v-text-field>
        <v-text-field type="password" label="Password" v-model="password"></v-text-field>

        <v-btn v-on:click="register" >Register</v-btn>

  </v-form>
  </div>
</template>

<script>
import firebase from 'firebase';
import {AppDB} from './firebaseInit'

export default {
 name: 'register',
 data: function() {
   return {
     email: '',
     password: ''
   };
 },
 methods: {
   register: function(e) {
     firebase
       .auth()
       .createUserWithEmailAndPassword(this.email, this.password)
       .then(
         u => {
          // console.log(user);

          AppDB.ref('Users').push().set({
                Email: this.email,
            })
           
           alert(`Account Created for ${u.user.email}`);
           
           this.$router.go({ path: this.$router.path });
         },
         err => {
           alert(err.message);
         }
       );
     e.preventDefault();
   },
   saveUser(){

       

   }
 }
};
</script>
<style>
#loginform {
  width: 50vw;
  margin-left:auto; 
   margin-right:auto;
   border-style: solid;
  border-width: small;
  padding: 10px;
  border-color:hsla(0, 100%, 50%, 1);
  
  font-family: 'Proza Libre', sans-serif;
    color: seagreen;
    font-weight: 300;
  /* other styles go here */
}
@import url('https://fonts.googleapis.com/css?family=Proza+Libre');
h1 {
    font-family: 'Proza Libre', sans-serif;
    color: seagreen;
    font-weight: 300;
}
</style>