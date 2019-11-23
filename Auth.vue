<template>
<div id="loginform">
 <v-form >
      <v-text-field label="Username/Email" v-model="email"></v-text-field>
      <v-text-field type="password" label="Password" v-model="password"></v-text-field>
      <v-btn v-on:click="login" >Login</v-btn>
</v-form>
</div>
</template>

<script>
import firebase from 'firebase';
export default {
 name: 'login',
 data: function() {
   return {
     email: '',
     password: ''
   };
 },
 methods: {
   login: function(e) {
     firebase
       .auth()
       .signInWithEmailAndPassword(this.email, this.password)
       .then(
         () => {
            if(this.email==="admin@mail.gvsu.edu"){
             // alert("Hello Admin");
             this.$router.push('/admin')
            }

           //alert(`You are logged in as ${this.email}`);
           this.$router.go({ path: this.$router.path });
         },
         err => {
           alert(err.message);
         }
       );
     e.preventDefault();
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
  padding: 20px;
   border-radius: 25px;
  border-color:hsla(228, 70%, 19%, 0.75)
  /* other styles go here */
}
</style>
