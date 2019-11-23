<template>
    <v-toolbar id="blue">
        <v-spacer></v-spacer>

        <v-toolbar-title class="headlinetext-uppercase">
            
            <router-link to="/" class="router"><span>GVSU Library Admin</span></router-link>
        </v-toolbar-title>

        <v-spacer></v-spacer>
        <v-spacer></v-spacer>
        <v-spacer></v-spacer>
        <v-spacer></v-spacer>
        <v-spacer></v-spacer>

        <v-toolbar-items>
            <v-btn text v-if="isLoggedIn" ><span class="emailwhite-text">{{currentUser}}</span></v-btn>
            <v-btn text v-if="isLoggedIn"><router-link class="home" to="/">Home</router-link></v-btn>
            <v-btn text v-if="!isLoggedIn"><router-link class="ls" to="/auth">Login</router-link></v-btn>
            <v-btn text v-if="!isLoggedIn"><router-link class="ls" to="/register">Sign Up</router-link></v-btn>
            <v-btn color="error" text v-if="isLoggedIn"><button v-on:click="logout" class="btnwhiteblack-text">Logout</button></v-btn>
        </v-toolbar-items>
        <v-spacer></v-spacer>

    </v-toolbar>
</template>



<script>
import firebase from 'firebase';
export default {
    name: 'navbar',
    data(){
        return{
            isLoggedIn: false,
            isAdmin: false,
            currentUser: false
        }
    },
    created() {
        if(firebase.auth().currentUser){
            this.isLoggedIn = true;
            this.currentUser = firebase.auth().currentUser.email;
        }
    },
    methods: {
        logout: function(){
            firebase.auth().signOut().then(() => {
                this.$router.go({ path: this.$router.path });
            });
        }
    }
};
</script>


<style scoped>
@import url('https://fonts.googleapis.com/css?family=Proza+Libre');
@import url('https://fonts.googleapis.com/css?family=Lalezar&display=swap');
.email{
    padding-right: 10px;
}
#blue {
    background-color: lightgray;
}
.ls {
    font-family: 'Lalezar', sans-serif;
    color: blue;
    font-size: 15px;
}

.router{
      text-decoration: none; 
      font-family: 'Lalezar', sans-serif;
    color: blue;
    font-size: 30px;
   
   
}
.home {
    font-family: 'Lalezar', sans-serif;
    color: blue;
    font-size: 20px;
 background:lightgray;
     border-radius: 5px;
     text-decoration: none;
     padding: 5px;
     box-shadow: 5px 5px;
}


.headlinetext-uppercase {
    font-family: 'Lalezar', sans-serif;
    color: blue;
    font-size: 30px;
    
}
.emailwhite-text {
    font-family: 'Lalezar', sans-serif;
    color: blue;
    font-size: 15px;
    

}
.btnwhiteblack-text {
font-family: 'Lalezar', sans-serif;
    color: blue;
    font-size: 20px;
    background:lightgray;
     border-radius: 5px;
      padding: 5px;
      box-shadow: 5px 5px;

}
</style>


