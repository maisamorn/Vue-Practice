<template>
  <div class="container mt-4">
    <div v-if="student" class="card">
      <div class="card-header">
        <h2 class="card-title">{{ student.name }}</h2>
        <router-link :to="{ name: 'edit-student', params: { id: student.id } }" class="btn btn-secondary float-end">Edit</router-link>
        <router-link to="/students" class="btn btn-primary float-end me-2">Back to Students</router-link>
      </div>
      <div class="card-body">
        <p class="card-text"><strong>Age:</strong> {{ student.age }}</p>
        <p class="card-text"><strong>Province:</strong> {{ student.province }}</p>
        <p class="card-text"><strong>Score:</strong> {{ student.score}}</p>
        <p class="card-text"><strong>Phone number: </strong> {{ student.phone_number}}</p>
        

        <div v-if="student.products && student.products.length" class="mt-4">
          <h3>Products</h3>
          <div class="row">
            <div class="col-md-4" v-for="product in student.products" :key="product.id">
              <div class="card mb-3">
                <div class="card-body">
                  <h5 class="card-title">{{ product.name }}</h5>
                  <p class="card-text"><strong>Student:</strong> {{ product.category }}</p>
                  <router-link :to="{ name: 'show-product', params: { id: product.id } }" class="btn btn-info btn-sm">View</router-link>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <p>Loading...</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ShowStudent',
  props: ['id'],
  data() {
    return {
      student: null,
    };
  },
  mounted() {
    this.fetchStudentDetails();
  },
  methods: {
    async fetchStudentDetails() {
      try {
        const response = await axios.get(`http://127.0.0.1:8000/api/student/show/${this.id}`);
        if (response.data.success) {
          this.student = response.data.data;
        }
      } catch (error) {
        console.error('Error fetching student details:', error);
      }
    },
  },
};
</script>

