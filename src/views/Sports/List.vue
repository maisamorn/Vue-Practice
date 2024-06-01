<template>
    <div class="container mt-4">
      <h1 class="mb-4">Fruit List</h1>
      <router-link to="/student/create" class="btn btn-primary mb-4">New Student</router-link>
      <ul class="list-group mt-4">
        <li v-for="student in students" :key="student.id" class="list-group-item">
          <h4>{{ student.name }}</h4>
          <p>{{ student.age}}</p>
          <p>{{ student.score}}</p>
          <p>{{ student.phone_number}}</p>
          <!-- <router-link :to="{ name: 'show-student', params: { id: student.id } }" class="btn btn-info btn-sm">Show</router-link>
          <router-link :to="{ name: 'edit-student', params: { id: student.id } }" class="btn btn-secondary btn-sm">Edit</router-link>
          <button class="btn btn-danger btn-sm" @click="deleteStudent(student.id)">Delete</button> -->

          <router-link class="btn btn-info btn-sm" :to="{ name: 'show-student', params: { id: student.id } }">Show</router-link>
          <router-link class="btn btn-secondary btn-sm" :to="{ name: 'edit-student', params: { id: student.id } }">Edit</router-link>
          <button class="btn btn-danger btn-sm"  @click="deleteStudent(student.id)">Delete</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name:'list-student',
    data() {
      return {
        students: [],
      };
    },
    mounted() {
      this.fetchStudents();
    },
    methods: {
      async fetchStudents() {
        try {
          const response = await axios.get('http://127.0.0.1:8000/api/student/list');
          console.log(response);
          if (response.data.success) {
            this.students = response.data.data;
          }
        } catch (error) {
          console.error('Error fetching students:', error);
        }
      },
       async deleteStudent(studentId) {
        try {
          const response = await axios.delete(`http://127.0.0.1:8000/api/student/delete/${studentId}`);
          if (response.data.success) {
            this.fetchStudents();
          }
        } catch (error) {
          console.error('Error deleting student:', error);
        }
      },
     
    },
  };
  </script>
  