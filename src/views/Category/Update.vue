<template>
  <div class="container mt-4">
    <h1 class="mb-4">Edit Category</h1>
    <Form @submit="updateCategory" v-slot="{ errors }">
      <div class="mb-3">
        <label for="category-name" class="form-label">Name</label>
        <Field
          name="name"
          type="text"
          class="form-control"
          id="category-name"
          v-model="category.name"
          rules="required|min:3"
          validateOnInput
          :style="{ borderColor: errors && errors['name'] ? 'red' : '' }"
        />
         <ErrorMessage name="name" class="text-danger" />
      </div>
      <div class="mb-3">
        <label for="category-description" class="form-label">Description</label>
        <textarea
          class="form-control"
          id="category-description"
          v-model="category.description"
          required
        ></textarea>
      </div>
      <button type="submit" class="btn btn-primary">Update Category</button>
    </Form>
  </div>
</template>

<script>
import { Form, Field, defineRule, ErrorMessage } from "vee-validate";
import { required, min } from '@vee-validate/rules';
import axios from "axios";
defineRule('required', required);
defineRule('min', min);
export default {
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  name: "edit-category",
  props: ["id"],
  data() {
    return {
      category: {
        name: "",
        description: "",
      },
    };
  },
  mounted() {
    this.fetchCategoryDetails();
  },
  methods: {
    async fetchCategoryDetails() {
      try {
        const response = await axios.get(
          `http://127.0.0.1:8000/api/category/show/${this.id}`
        );
        if (response.data.success) {
          this.category = response.data.data;
        }
      } catch (error) {
        console.error("Error fetching category details:", error);
      }
    },
    async updateCategory() {
      try {
        const response = await axios.put(
          `http://127.0.0.1:8000/api/category/update/${this.id}`,
          this.category
        );
        if (response.data.success) {
          this.$router.push("/categories");
        }
      } catch (error) {
        console.error("Error updating category:", error);
      }
    },
  },
};
</script>
