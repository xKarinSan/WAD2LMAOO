<template>
<!-- <div> -->
<div class = "page" v-if = "usertype !== 'na'">
    <div class="container">
        <Navbar :pageType = "'Home'"/>
    </div>

<!-- <div class = "pageContent"> -->
    <!-- {{usertype}} -->
    <div class="base container-fluid " style="background:#FFFAFA">
    <!-- <div v-if="usertype ==='employer'" class = "employer-projects"> -->
        <!-- <EmployerProject :projectName ="'Proj1'" :projectAssignees = "'Ruby Kurosawa'" :projectAssigned = "'14/7/2021'"
        :projectDue ="'14/11/2021'" :projectStatus = "'incomplete'"  :projectReward = "'500'"/>
        <EmployerProject :projectName ="'Proj2'" :projectAssignees = "'Karin'" :projectAssigned = "'11/6/2021'"
        :projectDue ="'12/10/2021'" :projectStatus = "'incomplete'"  :projectReward = "'460'"/>
        <EmployerProject :projectName ="'Proj2'" :projectAssignees = "'Karin'" :projectAssigned = "'11/6/2021'"
        :projectDue ="'12/10/2021'" :projectStatus = "'incomplete'"  :projectReward = "'460'"/>
        <EmployerProject :projectName ="'Proj2'" :projectAssignees = "'Karin'" :projectAssigned = "'11/6/2021'"
        :projectDue ="'12/10/2021'" :projectStatus = "'incomplete'"  :projectReward = "'460'"/>
        <EmployerProject :projectName ="'Proj2'" :projectAssignees = "'Karin'" :projectAssigned = "'11/6/2021'"
        :projectDue ="'12/10/2021'" :projectStatus = "'incomplete'"  :projectReward = "'460'"/>
        <AddItem :itemType="'project'"/> -->

<!-- Modal -->
<!-- <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h2 class="modal-title" id="exampleModalLabel" align="center">Add new project</h2>
      </div>
      <div class="modal-body">
        <table class="table">
          <tr>
            <th scope = "row">Project Name</th>
            <td><input type = "text" class = "form-control" /></td>
          </tr>
          <tr>
            <th scope = "row">Rewarded points:</th>
            <td><input type = "number" class = "form-control" /></td>
          </tr>
          <tr>
            <th scope = "row">Project due:</th>
            <td><input type = "date" class = "form-control" /></td>
          </tr>
          <tr>
            <th scope = "row">Add assignees:</th>
            <td><input type = "text" class = "form-control" /></td>
          </tr>
        </table>
        <button class = "btn new-project-btn" data-dismiss="modal">Assign project</button>
      </div>
    

    </div>
    <h5>Click anywhere to cancel</h5>
  </div>
</div> -->
        <div id="bg" class="row m-4">
            <div class="container-fluid">
                <div class="row">
                    <div>
                        <h1 class="heading">{{currentMonth()}} Summary</h1>
                    </div>
                    <div class="row justify-content-between" id="taskno">
                        <div class=" task-info g-2 col-12 col-xl-4 col-md-4 col-sm-12">
                            <div class="card">   
                                <div class="card-body">
                                    <h5 class="card-title">Total No. of Task(s)</h5>
                                    <h4 class="card-text pt-2">{{num_task}}</h4>
                                </div>
                            </div>
                        </div>   
                        <div class=" task-info g-2 col-12 col-xl-4 col-md-4 col-sm-12">
                            <div class="card">   
                                <div class="card-body">
                                    <h5 class="card-title">No. of Completed Task(s)</h5>
                                    <h4 class="card-text pt-2">{{completed_tasks}}</h4>
                                </div>
                            </div>
                        </div>  
                        <div class=" task-info g-2 col-12 col-xl-4 col-md-4 col-sm-12">
                            <div class="card">   
                                <div class="card-body">
                                    <h5 class="card-title">No. of Incomplete Task(s)</h5>
                                    <h4 class="card-text pt-2">{{incomplete_tasks}}</h4>
                                </div>
                            </div>
                        </div>  
                    </div>
                    
                </div>
                <br>

                <div class="container-fluid">
                    <!-- <div class="row p-2 my-4 border border-dark" id="bgcolor"> -->
                        <div class="container-fluid">
                            <div class="row justify-content-between">
                                <div class="col-md-6" width="100%">
                                    <b><highcharts :options="taskStatus_employer" class="chart col border border-dark"></highcharts></b>
                                </div>
                                <div class="col-md-6" width="100%">
                                    <b><highcharts :options="revenue" class="chart col border border-dark"></highcharts></b>
                                </div>
                            </div>
                        </div>
                        <div class="container-fluid">
                            <div class="row justify-content-between">
                                <div class="col-md-6 mt-3">
                                    <b><highcharts :options="taskDist_employer" class="chart border border-dark" width="100%"></highcharts></b>
                                </div>
                                <div class="col-md-6 mt-3">
                                    <div class="task_list border border-dark bg-white">
                                        <h6 class="pt-2 " style="font-size: 18px"><b>Project Task List</b></h6><input type="text" placeholder="Filter by task/status/project/person" class="w-100" v-model="filter" />
                                        <table class="table table-hover mt-2 table-striped">
                                            <thead >
                                                <tr style="background: #AED4FF">
                                                    <th scope="col" class="text-center">Task Status <button class="button" @click="sortTable('task_status', direction)"><img src="../assets/sort.png"></button></th>
                                                    <th scope="col" class="text-center">Task Name <button class="button" @click="sortTable('task_name', direction)"><img src="../assets/sort.png"></button></th>
                                                    <th scope="col" class="text-center">Project Name <button class="button" @click="sortTable('project_name', direction)"><img src="../assets/sort.png"></button></th>
                                                    <th scope="col" class="text-start" v-if="this.type =='employer'">Person In-charge <button class="button" @click="sortTable('user_name', direction)"><img src="../assets/sort.png"></button></th>
                                                </tr>
                                            </thead>
                                            <tbody>
                                                <tr v-for="(row, index) in filteredRows" :key="`task-${index}`">
                                                    <td class="text-center text-wrap">{{ row.task_status }}</td>
                                                    <td class="text-center text-wrap">{{ row.task_name }}</td>
                                                    <td class="text-center text-wrap">{{ row.project_name }}</td>
                                                    <td class="text-center text-wrap" v-if="this.type =='employer'">{{ row.user_name }}</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                                    
                            </div>
                        </div>
                        
                    <!-- </div> -->
                </div>
                
            </div>
        </div>
  </div>

</div>

<div v-else>
<h3>You are not logged in.</h3>
</div>
<!-- </div> -->
    
</template>
<script>
// import { onBeforeMount } from '@vue/runtime-core'
import {Chart} from 'highcharts-vue'
import Navbar from "./Navbar.vue"
import mixin from "../mixin"
import firebase from "firebase/compat"
// import EmployerProject from "./EmployerProject.vue"
// import AddItem from "./AddItem.vue"

export default {
    name:"Home",
    mixins:[mixin],
      components: {
    Navbar,
    // EmployerProject,
    // AddItem,
    highcharts: Chart 
  },
//   props:["usertype"],
  beforeMount(){
       this.usertype = this.getUserType();
        if(this.usertype === "na"){
            this.$router.push("/authenticate")
        }
  },
    methods: {
        currentMonth() {
            const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
            const current = new Date();
            const month = months[current.getMonth()];
            return month;
        },
        sortTable(key,direction) {
            this.sort = `${key} > ${direction}`
            if (direction === 'asc') {
                this.tasks.sort((a, b) => a[key] > b[key] ? 1: -1)
                this.direction = 'desc';
            } else {
                this.tasks.sort((a, b) => a[key] < b[key] ? 1: -1)
                this.direction = 'asc';
            }
        },
        
    },
  data(){
    return{
        tasks: [],
        completed_tasks: 0,
        incomplete_tasks: 0,
        num_task: 0,
        type: "",
        y:0,
      arr:[],
            taskStatus: {
                chart: {
                    type: 'pie'
                },
                title: {
                    text: 'Task Status'
                },
                plotOptions: {
                    pie: {
                        innerSize: '70%',
                        allowPointSelect: true,
                        cursor: 'pointer',
                        dataLabels: {
                            enabled: true,
                            format: '<b>{point.name}</b>: {y}'
                        }
                    }
                },
                series: [{
                    name: 'Count',
                    data: [{
                        name: 'Completed Task(s)',
                        y: 2,
                        color: "#1b3e6e"
                    }, {
                        name: 'Incomplete Task(s)',
                        y: 8,
                        color: "#6c9fe6"
                    }]
                }]
            },
            rewardHist: {
                chart: {
                    type: 'line'
                },
                title: {
                    text: 'Points Reward History'
                },
                xAxis: {
                    categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep']
                },
                series: [{
                    name: "Points Per Month",
                    data: [20, 10, 15, 7, 10, 16, 10, 14, 8]
                }],
            },
            taskDist: {
                chart: {
                    type: 'column'
                },
                title: {
                    text: 'Task Distribution'
                },
                xAxis: {
                    categories: ['Project 1', 'Project 2', 'Project 3', 'Project 4']
                },
                yAxis: {
                    min: 0,
                    title: {
                        text: 'Total No. of Tasks'
                    },
                    stackLabels: {
                        enabled: true,
                        
                    }
                },
                tooltip: {
                    headerFormat: '<b>{point.x}</b><br/>',
                    pointFormat: '{series.name}: {point.y}'
                },
                plotOptions: {
                    column: {
                        stacking: 'normal',
                    }
                },
                series: [{
                    name: 'Completed',
                    data: [0, 1, 1, 0],
                    color: "#1b3e6e"
                }, {
                    name: 'Incomplete',
                    data: [3, 3, 1, 1],
                    color: "#6c9fe6"
                }]
            },
            taskStatus_employer: {
                chart: {
                    type: 'pie'
                },
                title: {
                    text: 'Task Status'
                },
                plotOptions: {
                    pie: {
                        innerSize: '70%',
                        allowPointSelect: true,
                        cursor: 'pointer',
                        dataLabels: {
                            enabled: true,
                            format: '<b>{point.name}</b>: {y}'
                        }
                    }
                },
                series: [{
                    name: 'Count',
                    data: [{
                        name: 'Completed Task(s)',
                        y: 2,
                        color: "#1b3e6e"
                    }, {
                        name: 'Incomplete Task(s)',
                        y: 2,
                        color: "#ff9e9e"
                    }]
                }]
            },
            taskDist_employer: {
                chart: {
                    type: 'column'
                },
                title: {
                    text: 'Task Distribution'
                },
                xAxis: {
                    categories: []
                },
                yAxis: {
                    min: 0,
                    title: {
                        text: 'Total No. of Tasks'
                    },
                    stackLabels: {
                        enabled: true,
                        
                    }
                },
                tooltip: {
                    headerFormat: '<b>{point.x}</b><br/>',
                    pointFormat: '{series.name}: {point.y}'
                },
                plotOptions: {
                    column: {
                        stacking: 'normal',
                    }
                },
                series: [{
                    name: 'Completed',
                    data: [],
                    color: "#1b3e6e"
                }, {
                    name: 'Incomplete',
                    data: [],
                    color: "#ff9e9e"
                }]
            },
            revenue: {
                chart: {
                    type: 'line'
                },
                title: {
                    text: 'Revenue Growth'
                },
                xAxis: {
                    categories: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct']
                },
                series: [{
                    name: "Profits Per Month",
                    data: [30000, 60000, 50000, 70000, 100000, 160000, 120000, 99000, 110000, 115000]
                }],
            },
            filter: '',
            sort: '',
            direction: 'asc',
            columns: ['task','proj','status','person'],
            categories: [],
            completed: [],
            incomplete: []
            // rows: [
            //     {task:'Complete UI', proj: 'Project 5', status: 'Completed', person: 'James', deadline: '28/11/2021'},
            //     {task:'Prototype', proj: 'Project 1', status: 'New', person: 'James', deadline: '28/11/2021'},
            //     {task:'Draft Proposal', proj: 'Project 2', status: 'In Progress', person: 'James', deadline: '28/11/2021'},
            //     {task:'Setup Database', proj: 'Project 2', status: 'In Progress', person: 'John', deadline: '20/11/2021'},
            //     {task:'Draft Proposal', proj: 'Project 1', status: 'New', person: 'Kelly', deadline: '2/11/2021'},
            //     {task:'UI Testing', proj: 'Project 1',status: 'Completed', person: 'Den', deadline: '10/11/2021'}
            // ]
    }
  },
  computed: {
        filteredRows() {
                return this.tasks.filter(row => {
                const task_name = row.task_name.toLowerCase();
                const task_status = row.task_status.toLowerCase();
                const user_name = row.user_name.toLowerCase();
                const project_name = row.project_name.toLowerCase();
                const searchTerm = this.filter.toLowerCase();

                return task_name.includes(searchTerm) || task_status.includes(searchTerm) || user_name.includes(searchTerm) || project_name.includes(searchTerm);
            });
        },
  },
  created() {
      firebase.auth().onAuthStateChanged((user) => {
            if (user) {
                firebase.database().ref('users/' + user.uid + '/user_type' ).on('value', (snapshot) => {
                    this.type = snapshot.val();  
                }); 
                
                firebase.database().ref('tasks/').on('value', (snapshot) => {
                    console.log("type is " + this.type)
                    this.incomplete_tasks = 0
                    this.completed_tasks = 0
                    this.num_task = 0
                    this.tasks = []
                    if (this.type == "employer") {
                        // this.num_task = snapshot.val().length;
                        snapshot.forEach((childSnapshot) => {
                            var task = childSnapshot.val();
                            this.tasks.push(task);
                            if (task.task_status == "Completed") {
                                this.completed_tasks += 1;
                                this.num_task += 1;
                            } else {
                                this.incomplete_tasks += 1;
                                this.num_task += 1
                            }
                        });
                        // console.log("OVER HERE");
                        // console.log(this.taskStatus_employer.series[0].data[0].y);
                        // console.log(this.taskStatus_employer.series[0].data[1].y);
                        this.taskStatus_employer.series[0].data[0].y = this.completed_tasks;
                        this.taskStatus_employer.series[0].data[1].y = this.incomplete_tasks;
                        console.log("OVER HERE");
                        console.log(this.taskStatus_employer.series[0].data[0].y);
                        console.log(this.taskStatus_employer.series[0].data[1].y);

                        firebase.database().ref('projects/').on('value', (snapshot) => {
                            this.categories = []
                            snapshot.forEach((childSnapshot) => {
                                var name = childSnapshot.val().project_name
                                console.log(name)
                                this.categories.push(name)
                            })
                            console.log("CATEGORIES")
                            console.log(this.categories)
                            this.taskDist_employer.xAxis.categories = this.categories
                            console.log("xAXIS")
                            console.log(this.taskDist_employer.xAxis.categories)
                        
                        console.log("DISTCHARTHERE2");
                        firebase.database().ref('tasks/').on('value', (snapshot) => {
                            this.completed = []
                            this.incomplete = []
                            for (var proj of this.categories) {
                                var num_c = 0;
                                var num_inc = 0;
                                snapshot.forEach((childSnapshot) => {
                                    var task = childSnapshot.val();
                                    console.log(proj, task.project_name);
                                    if (proj == task.project_name) {
                                        if(task.task_status.toLowerCase() == "completed") {
                                            num_c += 1;
                                        } else {
                                            num_inc += 1;
                                        }
                                    }
                                });
                                console.log("DISTCHARTHERE1");
                                console.log(num_c,num_inc);
                                this.completed.push(num_c);
                                this.incomplete.push(num_inc);
                            }
                            console.log(this.completed);
                            this.taskDist_employer.series[0].data = this.completed;
                            this.taskDist_employer.series[1].data = this.incomplete;
                        });
                    });
                    } else {
                        // this.tasks = []
                        console.log("THISCATEGORIES")
                        console.log(this.categories)
                        this.categories = []
                        snapshot.forEach((childSnapshot) => {
                            var task = childSnapshot.val();
                            if (task.user_id == user.uid) {
                                if (!this.categories.includes(task.project_name)) {
                                    this.categories.push(task.project_name);
                                }
                                this.tasks.push(task);
                                this.num_task += 1;
                                if (task.task_status.toLowerCase() == "completed") {
                                    this.completed_tasks += 1;
                                } else {
                                    this.incomplete_tasks += 1;
                                }
                            } 
                        })

                        // console.log("OVER HERE");
                        // console.log(this.taskStatus_employer.series[0].data[0].y);
                        this.taskStatus_employer.series[0].data[0].y = this.completed_tasks;
                        this.taskStatus_employer.series[0].data[1].y = this.incomplete_tasks;
                        console.log("OVER HERE");
                        console.log(this.taskStatus_employer.series[0].data[0].y);
                        console.log(this.taskStatus_employer.series[0].data[1].y);

                        console.log("TASK DIST EMPLOYEE")
                        console.log(this.taskDist_employer)
                        this.taskDist_employer.xAxis.categories = this.categories

                        firebase.database().ref('tasks/').on('value', (snapshot) => {
                            this.completed = []
                            this.incomplete = []
                            for (var proj of this.categories) {
                                var num_c = 0;
                                var num_inc = 0;
                                snapshot.forEach((childSnapshot) => {
                                    var task = childSnapshot.val()
                                    if (proj == task.project_name && task.user_id == user.uid) {
                                        console.log("proj "+ task.task_status)
                                        if(task.task_status.toLowerCase() == "completed") {
                                            num_c += 1
                                        } else {
                                            num_inc += 1;
                                        }
                                    }
                                })
                                console.log(num_c,num_inc)
                                this.completed.push(num_c);
                                this.incomplete.push(num_inc);
                            }
                            console.log(this.completed)
                            this.taskDist_employer.series[0].data = this.completed;
                            this.taskDist_employer.series[1].data = this.incomplete;
                        });
                    }
                });

                // firebase.database().ref('projects/').on('value', (snapshot) => {
                //     snapshot.forEach((childSnapshot) => {
                //         var name = childSnapshot.val().project_name
                //         this.categories.push(name)
                //     })
                //     this.taskDist_employer.xAxis = this.categories
                //     console.log(this.taskDist_employer.xAxis)
                // });   
            }
        })
    }

}
</script>
<style>
.base {
    margin-top: 60px;
}

.heading{
    box-shadow: 0px 5px 0px rgba(83, 90, 249, 0.81);
    width:max-content;
    margin: 20px auto 30px auto;
} 
/* #bgcolor{
    background: rgb(255, 220, 156);
} */

.highcharts-title{
    font-family: Arial, Helvetica, sans-serif
}
.table{
    table-layout:fixed;
    width:100%;
}

td {
    /* word-wrap:break-word; */
    overflow: hidden;
}
.task_list{
    overflow-y:scroll;
}
.page{
    min-height: 100vh;
    width:100%;
    /* display: flex; */
}
.pageContent{
  min-height:100vh;
  /* margin-top:60px; */
  /* background:rgba(248, 248, 248, 1); */
  padding-top:60px;
  padding-bottom:60px;
}
</style>
<style scoped>
.employer-projects{
      display: flex;
    flex-wrap: wrap;
}
.new-project-btn{
background: linear-gradient(0deg, #504DFF, #504DFF), rgba(78, 74, 255, 0.61);
border-radius: 5px;
color: white;
}
.modal-title{
  margin: auto;
}
.modal-dialog h5{
  color:white;
}
/* employee homepage aka dashboard */
    .container {
        margin:auto;
    }
    h1 {
        text-align: left;
        /* padding-top: 25px; */
    }

    #taskno{
        /* width:100% */
        margin-left:25px;
        /* margin-right: auto; */
    }

    .task-info {
        height: 90px;
        color: white
        /* width: 270px;
        background: linear-gradient(57.11deg, #6D9DF8 -4.9%, #6461FF 101.23%, rgba(109, 157, 248, 0.64) 101.24%, rgba(109, 157, 248, 0) 101.24%);        color:white; */
    }

    .card{
        width: 90%;
        background: black;
    }

    .card-body {
        padding: 10px 10px;
    }

    .chart {
        padding: 0px;
        margin: 5px;
        border-radius: 2%;        

    }
    
    .task_list {
        margin: 5px;
        height: 400px;
    }
    img {
        width: 20px;
    }
    .button {
        border: none;
        background-color: transparent;
    }
</style>