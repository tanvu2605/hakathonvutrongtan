<template>
    <div v-if="isVisible" class="modal-overlay">
      <div class="modal-content">
        <h3>{{ isEditMode ? 'Edit User' : 'Add User' }}</h3>
        <form @submit.prevent="handleSubmit">
          <div class="form-group">
            <label for="name">Name</label>
            <input type="text" id="name" v-model="formData.name" required />
          </div>
          <div class="form-group">
            <label>Gender</label>
            <div>
              <input type="radio" value="Male" v-model="formData.gender" /> Male
              <input type="radio" value="Female" v-model="formData.gender" /> Female
              <input type="radio" value="Other" v-model="formData.gender" /> Other
            </div>
          </div>
          <div class="form-group">
            <label for="dob">Date Of Birth</label>
            <input type="date" id="dob" v-model="formData.dob" :max="maxDate" required />
          </div>
          <div class="form-group">
            <label for="email">Email</label>
            <input type="email" id="email" v-model="formData.email" required />
          </div>
          <div class="form-actions">
            <button type="button" @click="closeForm">Close</button>
            <button type="submit">{{ isEditMode ? 'Update' : 'Save' }}</button>
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, watch, computed } from 'vue';
  
  const props = defineProps({
    isVisible: Boolean,
    userToEdit: Object,
  });
  
  const emit = defineEmits(['update:isVisible', 'userAdded', 'userUpdated']);
  
  const formData = ref({
    name: '',
    gender: 'Male',
    dob: '',
    email: '',
  });
  
  const isEditMode = computed(() => !!props.userToEdit);
  const maxDate = new Date().toISOString().split('T')[0];
  
  watch(
    () => props.userToEdit,
    (newUser) => {
      if (newUser) {
        formData.value = { ...newUser };
      }
    }
  );
  const handleSubmit = () => {
  if (!formData.value.name || !formData.value.email) {
    alert('Name and Email are required.');
    return;
  }

  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!emailPattern.test(formData.value.email)) {
    alert('Please enter a valid email address.');
    return;
  }

  const existingUsers = JSON.parse(localStorage.getItem('users') || '[]');
  const emailExists = existingUsers.some(
    (user) => user.email === formData.value.email && user.email !== props.userToEdit.email
  );
  if (emailExists) {
    alert('Email already exists. Please use a different email.');
    return;
  }

  if (new Date(formData.value.dob) > new Date()) {
    alert('Date of birth cannot be in the future.');
    return;
  }

  console.log('Form data before emitting:', formData.value);

  if (isEditMode.value) {
    emit('userUpdated', formData.value);
  } else {
    emit('userAdded', formData.value);
  }

  closeForm();
};
  
  const closeForm = () => {
    emit('update:isVisible', false);
  };
  </script>
  
<style scoped>
.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal-content {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    width: 400px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.form-group {
    margin-bottom: 15px;
}

.button-group {
    display: flex;
    justify-content: space-between;
}

button {
    padding: 10px 15px;
    cursor: pointer;
}

button[type="submit"] {
    background-color: #007bff;
    color: white;
    border: none;
}

button[type="submit"]:hover {
    background-color: #0056b3;
}
</style>
