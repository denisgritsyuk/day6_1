<template>
<div>
    <h1>Счетчик студентов</h1>
    <br>
    Счетчик студентов - {{studentsCount}}   
    <table>
                <tr>
                    <th>Photo</th>
                    <th>Name</th>
                    <th>Group</th>
                    <th>Mark</th>
                    <th>PR is Done</th>
                </tr>
    
       <td> <img v-bind:src="student.photo" width="50px"></td>
        <td>{{student.name}}</td>
        <td>{{student.group}}</td>
         <td>{{student.mark}}</td>
         <td><input type="checkbox" v-bind:checked="student.isDonePr"></td>
         
        </table>
</div>
</template>
<script>
import Vue from 'vue'
import Vuex from 'vuex'
 
 export default {
     props: {
         id:'',
     },
     data: function(){
         return {
             name:"",
             group:"",
             student:{},
         };
     },
 mounted: function(){
     Vue.axios.get("http://46.101.212.195:3000/students/"+this.id)
     .then( (response)=>{
         console.log(response.data)
         this.student = response.data;
     })
 },
         computed: {
         studentsCount () {
         return this.$store.getters.getCount
    }
  }
 }

 </script>