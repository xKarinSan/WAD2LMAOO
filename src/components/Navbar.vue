<template>
<div >
<nav class="navbar navbar-expand-sm fixed-top navbar-light" id = "barz">
  <div class="container-fluid">
    <router-link class = "nav-link" to="/home"><img src="../assets/projeck.png" width="60" height="47" ></router-link>
    <!-- <a class="navbar-brand" href="src\components\Home.vue" ></a> -->
    <button class="navbar-toggler"  type="button" data-bs-toggle="offcanvas" data-bs-target="#offcanvasNavbar" aria-controls="offcanvasNavbar">
      <span class="navbar-toggler-icon" ></span>
    </button>
    <div class="offcanvas offcanvas-start" tabindex="-1" id="offcanvasNavbar" aria-labelledby="offcanvasNavbarLabel">
      <div class="offcanvas-header">
        <h5 class="offcanvas-title" style = "color:white" id="offcanvasNavbarLabel">
          {{loggedUser.name}}
          </h5>
        <button type="button" class="btn-close text-reset" data-bs-dismiss="offcanvas" aria-label="Close"></button>
      </div>
      <div class="offcanvas-body">
        <ul class="navbar-nav justify-content-end flex-grow-1 pe-3" v-if="this.type === 'employer'">

          <!-- employer -->
          <li class="nav-item px-2">
            <router-link v-if = "pageType ==='Home'" class = "nav-link active" to="/home">Home</router-link>
            <router-link v-else class = "nav-link" to="/home">Home</router-link>
          </li>
          <li class="nav-item px-2">
            <router-link v-if = "pageType ==='Projects'" class = "nav-link active" to="/projects">Projects</router-link>
            <router-link  v-else class = "nav-link" to="/projects">Projects</router-link>
            <!-- <a class="nav-link" href="#">Projects</a> -->
          </li>
         <li class="nav-item px-2">
            <router-link v-if = "pageType ==='Calendar'" class = "nav-link active" to="/calendar">Calendar</router-link>
            <router-link  v-else class = "nav-link" to="/calendar">Calendar</router-link>
          </li>
         <li class="nav-item px-2">
           <router-link  v-if = "pageType ==='Employees'" class = "nav-link active" to="/employees">Employees</router-link>
           <router-link v-else class = "nav-link" to="/employees">Employees</router-link>
            <!-- <a class="nav-link" href="#">Employees</a> -->
          </li>
         <li class="nav-item px-2">
           <router-link v-if = "pageType ==='Review'" class = "nav-link active" to="/review">Review</router-link>
           <router-link v-else class = "nav-link" to="/review">Review</router-link>
            <!-- <a class="nav-link" href="#">Review</a> -->
          </li>
         <!-- <li class="nav-item px-2">
            <a class="nav-link" href="#">Settings</a>
          </li> -->
         <li class="nav-item px-2">
            <a class="nav-link" href="#" @click ="logout()">Logout</a>
          </li>
        </ul>

        <!-- employee -->
        <ul class="navbar-nav justify-content-end flex-grow-1 pe-3" v-else>
          <li class="nav-item px-3">
            <router-link v-if = "pageType ==='Home'" class = "nav-link active" to="/home">Home</router-link>
            <router-link v-else class = "nav-link" to="/home">Home</router-link>
          </li>
          <li class="nav-item px-3">
            <router-link v-if = "pageType ==='To-do'" class = "nav-link active" to="/emptasks">To-do</router-link>
            <router-link v-else class = "nav-link" to="/emptasks">To-do</router-link>
            <!-- <a class="nav-link" href="#">To-do</a> -->
          </li>
         <li class="nav-item px-3">
            <!-- <a class="nav-link" href="#">Calendar</a> -->
            <router-link v-if = "pageType ==='Calendar'" class = "nav-link active" to="/calendar">Calendar</router-link>
            <router-link  v-else class = "nav-link" to="/calendar">Calendar</router-link>
          </li>
         <li class="nav-item px-3">
           <router-link v-if = "pageType ==='Profile'" class = "nav-link active" to="/profile">Profile</router-link>
           <router-link v-else class = "nav-link" to="/profile">Profile</router-link>

          </li>
         <li class="nav-item px-3">
            <a class="nav-link" href="#" v-on:click ="logout()">Logout</a>
          </li>
        </ul>
        
      </div>
    </div>
  </div>
</nav>
</div>
</template>

<script>
import mixin from "../mixin"
import firebase from "firebase/compat"
import "firebase/compat/auth"
export default {
    name:"Navbar",
    // props: ["usertype"],
    mixins:[mixin],
    props:["pageType"],
    data(){
        return{
          type: ""
        }
    },
    methods: {
      logout() {
            firebase
              .auth()
              .signOut()
              .then(() => {
                alert('Successfully logged out');
                this.$router.push('/authenticate');
              })
              .catch(error => {
                alert(error.message);
                this.$router.push('/authenticate');
              });
          },
    },
    beforeMount(){
      firebase.auth().onAuthStateChanged((user) => {
            if (user) {
                firebase.database().ref('users/' + user.uid + '/user_type' ).on('value', (snapshot) => {
                    this.type = snapshot.val();  
                    console.log(this.type)
                }); 
            }
      })
      // this.getUserType();
      this.getLoggedUser();

      // console.log(this.usertype)
    }

}
</script>
<style scoped>
#barz, #offcanvasNavbar{
    /* background: linear-gradient(57.11deg, #86B0FF -4.9%, #5b59fd 101.23%, rgba(133, 175, 255, 0.61) 101.24%, rgba(52, 97, 184, 0.64) 101.24%);     */
    background: #AED4FF;
    padding-top: 0%;
    padding-bottom: 0%;
}
/* .active-page{
    background: rgb(3, 3, 117);
} */
.navbar-nav{
    display: flex;
    flex-wrap: wrap;
}

.nav-link{
  color: #424242 !important
}

.active{
  /* border-bottom:2px solid #424242; */
  border-bottom:2px solid rgb(255,158,158);
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
  color: rgb(255,128,128) !important;
}

.nav-item a:hover {
  /* border-bottom:2px solid #424242; */
  color:rgb(255,128,128) !important;
}

</style>