<template>
    <div class = "page" style="background:#FFFAFA">
        <Navbar :pageType = "'Projects'"/>
        <div class = "add-employee-container">
            <h1 id = "add-emp-header"  class="mb-4">Add Project</h1>
            <div class="row">
            <div class = "col-12 col-md-9 col-lg-7 card onboarding-form mb-5">
                <table class = "form-table">
                    <tr>
                        <td id = "impt-info">
                            <table class ="input-table">
                                <tr>
                                    <th>Project Name:</th>
                                    <td><input type  ="text" placeholder = "Project Name" class = "form-control" v-model="proj_name"/></td>
                                </tr>
                                <tr>
                                    <th>Assigned To:</th>
                                    <td class="text-start">
                                        <select v-model="selected" >
                                            <option disabled value="">Please select an assignee</option>
                                            <option v-for="user in users" v-bind:key="user.user_id">{{user.name}}</option>
                                        </select>
                                    </td>
                                </tr>

                                <tr>
                                    <th>Points:</th>
                                    <td><input type ="number" min="0" placeholder = "Number of points" class = "form-control" v-model="pointsAwarded"></td>
                                </tr>
                                <tr>
                                    <th>Due Date:</th>
                                    <td><input type ="date" maxlength="10" placeholder = "YYYY-MM-DD" class = "form-control" v-model="duedate"></td>
                                </tr>
                            </table>
                        </td>
                    </tr>

                </table>
                <div class = "button-container justify-content-center">
                    <router-link to = "/projects" class = "btn fire-btn btn-danger">Cancel</router-link>
                    <!-- <button class = "btn fire-btn">Cancel</button> -->
                    <button class = "btn view-btn" v-on:click ="submit()">Add</button>
                </div>
            </div>
        </div>
        </div>
    </div>
</template>
<script>
import Navbar from "./Navbar.vue"
import mixin from "../mixin"
import firebase from "firebase/compat"
import "firebase/compat/auth"
export default {
    name:"AddProject",
    components:{
        Navbar
    },
    mixins:[mixin],
    data() {
        return {
            proj_name: '',
            users: [],
            selected: "",
            todaydate: "",
            duedate: "",
            pointsAwarded:0,
            userID:""
            // len: 0,
        }
    },
    methods:{
        currentDateTime() {
            const current = new Date();
            const date = current.getFullYear()+'-'+(current.getMonth()+1)+'-'+current.getDate();
            const time = current.getHours() + ":" + current.getMinutes() + ":" + current.getSeconds();
            const dateTime = date +' '+ time;
            return dateTime;
        },
        submit(){
            let date = this.duedate.split("-");
            console.log(this.duedate)
            date = new Date(date[0],date[1]-1,date[2])
            console.log(date)
            console.log(date.getTime()  > new Date().getTime() )
            if(this.proj_name ===""){
                alert("Project Name cannot be empty")
            }
            else if(this.selected ===""){
                alert("Please select")
            }
            else if(this.duedate ===""){
                alert("Please select a due date.")

            }
            else if(date.getTime()  < new Date().getTime() ){
                alert("Due date cannot be earlier than today or due today!")
            }

            else if(this.pointsAwarded <= 0){
                alert("Points cannot be negative!")
            }
            else{
// get user id
            firebase.database().ref("users/").on('value',(snapshot)=>{
                        snapshot.forEach((childSnapshot) => {
                var user = childSnapshot.val();
                //   console.log(project.project_id)
                // console.log(this.projectName == project.project_name)
                if(user.name ==  this.selected){
                    this.userID = user.user_id
                    // this.tasks.push(task);
                }
                console.log(this.userID)
                })
            })





            var myref = firebase.database().ref('/projects').push()
            var key = myref.key;
                
            var input_data = {
                user_id:this.userID,
                assigned_date: this.currentDateTime(),
                assignee: this.selected,
                completion_date: "",
                due_date: this.duedate,
                project_id: key,
                project_name: this.proj_name,
                project_status: "Incomplete",
                reward: this.pointsAwarded
            }
            console.log(input_data)

            var updates = {};
            updates['/projects/' + key] = input_data;

            firebase.database().ref().update(updates)


            // this.num++
            this.$router.push("/projects")
            }

        }
    },

    created() {
        firebase.database().ref('users/').on('value', (snapshot) => {
            this.users = []
            snapshot.forEach((childSnapshot) => {
                var user = childSnapshot.val();
                this.users.push(user);
            });
        }); 
    }

}
</script>
<style scoped>

table {
  /* border: 1px solid black; */
  table-layout: fixed;
  width: 200px;
}

td {
  /* border: 1px solid black; */
  width: 100px;
  overflow: hidden;
}
th{
    /* border: 1px solid black; */
    width: 20px;
    overflow: hidden;
}
.card{
    max-width: 100%;
    height: auto;
}


#add-emp-header{
    width: max-content;
    margin: auto;
    /* margin-top: 80px; */
    box-shadow: 0px 5px 0px rgba(83, 90, 249, 0.81);
}
.page{
    min-height: 100vh;
    width:100%;
    background:rgba(248, 248, 248, 1);
    overflow: hidden;
    /* display: flex; */
}
.add-employee-container{
    margin-top:80px;
    display: grid;
    align-content: center;
}
.onboarding-form{
    background:#F8F8F8;
    margin: auto;
    /* width: 50%; */
    /* padding:10px; */
    align-items: center;
    /* max-width: 100%; */
}

@media (max-width: 800px) {
    .onboarding-form{
        width: auto;
    }
}

/* form */
.form-table{
    width:90%;
    /* max-width: 100%; */
}
#impt-info{
    width:60%;
}
.form-table tr td{
    padding:10px;
}
/* impt data */
.input-table{
/* width: 60%; */
width: 100%;
/* background:purple; */

}
textarea{
        width: 100%;

}
.input-table tr th{
    text-align: right;
    padding-right: 8px;
    /* width: 40px; */
}

/* photo */
.add-photo{
    background:white;
    height: 200px;
}
#add-photo{
    height:100%;
    /* background:purple; */
}

/* buttons */
.button-container{
    display: flex;
    justify-content:space-evenly;
    width:100%;
    margin:10px;

}
/* #exampleFormControlFile1{
    text-align: right;
} */

/* .view-btn{
    background: #6360FF;
border-radius: 5px;
width:200px;
padding:10px;
color: white;
margin: 5px;
}
.fire-btn{
  border-radius: 5px;
width:200px;
padding:10px;
color: white;
background:  rgba(147, 7, 7, 1);
margin: 5px;

} */
.view-btn{
    /* background: #6360FF; */
    background:#504DFF;
    border-radius: 5px;
    width:100px;
    padding:10px;
    color: white;
    margin: 15px;
    border: 1px solid black;
    box-shadow: 0 2px 2px 0 rgba(0,0,0,0.2), 0 2px 2px 0 rgba(0,0,0,0.19);
}
.fire-btn{
    border-radius: 5px;
    width:100px;
    padding:10px;
    color: white;
    margin: 15px;
    border: 1px solid black;
    box-shadow: 0 2px 2px 0 rgba(0,0,0,0.2), 0 2px 2px 0 rgba(0,0,0,0.19);

}
</style>