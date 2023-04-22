<template>
  <div>

    <new-student-form v-on:student-added="newStudentAdded"></new-student-form>
    <student-table
      v-bind:students="students"
      v-on:student-arrived-or-left="studentArrivedOrLeft"
      v-on:delete-student="studentDeleted">
    </student-table>
    <student-message v-bind:student="mostRecentStudent"></student-message>

    <!--v-on: is making sure that parent can recieve messages from child components and then calls a method to deal with the message-->
    <!--v-bind: syntax after v-bind is name of prop="name of data"-->

  </div>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'
import StudentMessage from './components/StudentMessage.vue'
import StudentTable from './components/StudentTable.vue'


export default {
  name: 'App',
  components: {
    NewStudentForm,
    StudentMessage,
    StudentTable
  },
  data() {
    return {
      students: [], //setting array that will recieve messages from NewStudentForm and send to StudentTable
      mostRecentStudent: {}
    }
  },
  methods:{
    newStudentAdded(student) {
      this.students.push(student) //when NewStudentForm sends a data about a new student over add the data to the students array
      this.students.sort(function(s1, s2) {
        return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : +1
      })
    },
    studentArrivedOrLeft(student, present) {
      // find the student in the array of students
      // update present attribute

      let updateStudent = this.students.find (function(s) {
        if (s.name === student.name && s.starID === student.starID) {
          //find method will search an array for the first matching value
          //so above we;re searching the student array for the exact name and starID match
          // and if found we're returning true to update the present/absent status
          return true
        }
      })

      if (updateStudent) {
        updateStudent.present = present
        this.mostRecentStudent = updateStudent
      }
    },
    studentDeleted(student) {
      //filter returns a new array of all students for whom the function returns true
      this.students = this.students.filter( function(s) {
        if (s != student) {
          return true
        }
      })
      //clear welcome or goodbye message after deleting a student
      this.mostRecentStudent = {}
    }
  }
}
</script>

<style>

@import "https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css";

</style>
