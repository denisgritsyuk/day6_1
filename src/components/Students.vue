<template>
<div>
        
        <h1>Счетчик студентов</h1>
        Счетчик студентов - {{studentsCount}}   
    <table>
                <tr>
                    <th>Фото</th>
                    <th>Имя</th>
                    <th>Группа</th>
                    <th>Оценка</th>
                    <th>Практическая</th>
                </tr>
                <tr v-for="stud in students" v-bind:key="stud._id">
                    <td><img v-bind:src="stud.photo" width="50px"></td>
                    <td v-bind:style="stud.name.indexOf(search)>-1 && search.length >0  ? 'background-color:#CA2C2C' : 'background-color:#fff'">
                    <router-link :to="'/student-info/'+stud._id">
                    {{stud.name}}</router-link></td>
                    <td>{{stud.group}}</td>
                    <td>{{stud.mark}}</td>
                    <td><input type="checkbox" v-bind:checked="stud.isDonePr"></td>
                    <td><a href="#" v-on:click="deleteStudent(stud._id)">Удалить</a></td>
                    <td><button v-on:click="getData(stud._id,stud.name,stud.group,stud.isDonePr)"><img src="components/Pencil.svg.png" width="25px"></button></td>
                </tr>
            </table>

            <p><h3>Add new student</h3>
            <br>Input Name: <input v-model="name">
            <br> Input Group:
             <select name="group" v-model="group">
                <option value="RPZ 18 1/9">RPZ 18 1/9</option>
                <option value="RPZ 18 2/9">RPZ 18 2/9</option>
            </select>
            <br> Pr Is Done: <input type="checkbox" v-model="isDonePr">
            <br><button v-on:click="addStudent">Add student</button>

            <p><h3>Update student</h3>
            <br>Input Name: <input v-model="newStudent.name">
            <br> Input Group: 
            <select name="group" v-model="newStudent.group">
                <option value="RPZ 18 1/9">RPZ 18 1/9</option>
                <option value="RPZ 18 2/9">RPZ 18 2/9</option>
            </select>
            <br> Pr Is Done: <input type="checkbox" v-model="newStudent.isDonePr">
            <br><button v-on:click="updateStudent()" v-bind:style="showInput ? 'display:inline' : 'display:none'">Update student</button>
            
</div>

</template>
<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
import Vuex from 'vuex'


export default {
    data: function() {
           return {
  students: [],
            currency:[],
            search:"",
            
             newStudent: {},
            search:"",
            result:"",
            name:"",
            group:"",
            mark:0,
            isDonePr:false,
            names:"",
            groups:"",
            isDonePrs:false,
            names:"",
            groups:"",
            isDonePrs:false,
            studId:"",
             showInput:false,
            idTest:"",
            marks:0,
        }}, 
         mounted: function(){
        axios.get("http://46.101.212.195:3000/students").then((response)=>{
            console.log(response.data);
            this.students = response.data;
            this.$store.commit('setCount',this.students.length);
           })
        axios.get("https://api.privatbank.ua/p24api/pubinfo?json&exchange&coursid=5").then((response)=>{
            console.log(response.data);
            this.currency = response.data;
          })
                  },
         methods:{
                
            
        addStudent:function(){
            Vue.axios.post("http://46.101.212.195:3000/students", {
                name: this.name,
                group: this.group,
                mark: this.mark,
                isDonePr: this.isDonePr,
            })
            .then((response) => {
                console.log(response.data)
                axios.get("http://46.101.212.195:3000/students").then((response)=>{
                this.students = response.data;
                this.$store.commit('setCount',this.students.length);
            })
            })
        },
        deleteStudent:function(id){
            Vue.axios.delete("http://46.101.212.195:3000/students/"+id)
            .then((repsonse)=>{

            axios.get("http://46.101.212.195:3000/students").then((response)=>{
                this.students = response.data;
                this.$store.commit('setCount',this.students.length);
            })
            })
        },
        getData: function(id,name,group,mark,isDone){
            this.studId = id;
            this.name1 = name;
            this.group1 = group;
            this.mark1 = mark;
            this.isDonePr1 = isDone;
        },
        updateStudent:function(){
            Vue.axios.put("http://46.101.212.195:3000/students/"+this.studId, {
                name: this.name1,
                group: this.group1,
                mark: this.mark1,
                isDonePr: this.isDonePr1,
            })
            axios.get("http://46.101.212.195:3000/students").then((response)=>{
                this.students = response.data;
            })
        },
        
         },   
             filters:{
            roundValue: function(value){
                return parseFloat(value.toFixed(2));
            },
        }, 
            computed: {
            studentsCount () {
                return this.$store.getters.getCount
            },
        }
}
</script>