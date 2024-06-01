<template>
  <div class="container mt-4">
    <h1 class="mb-4">Create Student</h1>
    <Form @submit="createStudent" v-slot="{ errors }">
      <div class="mb-3">
        <label for="student-name" class="form-label">Name</label>
        <Field
          name="name"
          type="text"
          class="form-control"
          id="student-name"
          v-model="student.name"
          rules="required|min:3"
          validateOnInput
          :style="{ borderColor: errors && errors['name'] ? 'red' : '' }"
        />
          <ErrorMessage name="name" class="text-danger" />
      </div>
      <div class="mb-3">
        <label for="student-age" class="form-label">Age</label>
        <Field
          name="age"
          type="number"
          class="form-control"
          id="student-age"
          v-model="student.age"
          rules="required|min:3"
          validateOnInput
          :style="{ borderColor: errors && errors['age'] ? 'red' : '' }"
        />
        <ErrorMessage name="age" class="text-danger" />
      </div>
      <div class="mb-3">
        <label for="student-score" class="form-label">Score</label>
        <input
          type="number"
          class="form-control"
          id="student-score"
          v-model="student.score"
          required
        />
      </div>
      <div class="mb-3">
        <label for="student-province" class="form-label">Province</label>
        <input
          type="text"
          class="form-control"
          id="student-province"
          v-model="student.province"
          required
        />
      </div>
      <div class="mb-3">
        <label for="student-phone_number" class="form-label"
          >Phone number</label
        >
        <Field
          name="phone_number"
          type="text"
          class="form-control"
          id="student-phone_number"
          v-model="student.phone_number"
          rules="required|min:3"
          validateOnInput
          :style="{ borderColor: errors && errors['age'] ? 'red' : '' }"
        />
        <ErrorMessage name="phone_number" class="text-danger" />
      </div>
      <button type="submit" class="btn btn-primary">Create Student</button>
    </Form>
  </div>
</template>

<script>
import { Form, Field, defineRule, ErrorMessage } from "vee-validate";
import { required, min } from "@vee-validate/rules";
import axios from "axios";
defineRule("required", required);
defineRule("min", min);

export default {
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  name: "createStudent",
  data() {
    return {
      student: {
        name: "",
        age: "",
        score: "",
        province: "",
        phone_number: "",
      },
    };
  },
  methods: {
    async createStudent() {
      try {
        const response = await axios.post(
          "http://127.0.0.1:8000/api/student/create",
          this.student
        );
        if (response.data.success) {
          this.$router.push("/students");
        }
      } catch (error) {
        console.error("Error creating students:", error);
      }
    },
  },
};
</script>
