<template>
  <div class="manager-user-container">
    <div class="header">
      <h2>Manager User</h2>
      <button class="add-user-button" @click="openAddUserForm">
        Add New User
      </button>
    </div>
    <AddUserForm
      :isVisible="showAddUserForm"
      :userToEdit="userToEdit"
      @update:isVisible="showAddUserForm = $event"
      @userAdded="addUser"
      @userUpdated="updateUser"
    />
    <table class="user-table">
      <thead>
        <tr>
          <th>STT</th>
          <th>Name</th>
          <th>Gender</th>
          <th>Date Of Birth</th>
          <th>Email</th>
          <th>Option</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(user, index) in users" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.gender }}</td>
          <td>{{ user.dob }}</td>
          <td>{{ user.email }}</td>
          <td>
            <button class="edit-button" @click="openEditUserForm(user)">
              Edit
            </button>
            <button class="delete-button" @click="deleteUser(index)">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <DeleteConfirmationModal
      :isVisible="showDeleteModal"
      @close="handleCloseDeleteModal"
      @confirmDelete="handleConfirmDelete"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import AddUserForm from "../components/AddUserForm.vue";

const showAddUserForm = ref(false);
const userToEdit = ref(null);
const users = ref([]);

const openAddUserForm = () => {
  userToEdit.value = null;
  showAddUserForm.value = true;
};

const openEditUserForm = (user) => {
  userToEdit.value = { ...user };
  showAddUserForm.value = true;
};

const addUser = (newUser) => {
  users.value.push(newUser);
  localStorage.setItem("users", JSON.stringify(users.value));
  loadUsers();
};

const updateUser = (updatedUser) => {
  console.log("Updating user:", updatedUser);

  const index = users.value.findIndex(
    (user) => user.email === updatedUser.email
  );
  if (index !== -1) {
    users.value[index] = updatedUser;
    localStorage.setItem("users", JSON.stringify(users.value));
    loadUsers();
  }
};

const deleteUser = (index) => {
  users.value.splice(index, 1);
  localStorage.setItem("users", JSON.stringify(users.value));
  loadUsers();
};

const loadUsers = () => {
  users.value = JSON.parse(localStorage.getItem("users") || "[]");
};

onMounted(() => {
  loadUsers();
});
</script>

<style scoped>
.manager-user-container {
  padding: 20px;
  background-color: #f9f9f9;
  min-height: 100vh;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.add-user-button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

.add-user-button:hover {
  background-color: #0056b3;
}

.user-table {
  width: 100%;
  border-collapse: collapse;
}

.user-table th,
.user-table td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.user-table th {
  background-color: #f2f2f2;
  color: #333;
}

.edit-button {
  background-color: #ffa500;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 3px;
  cursor: pointer;
  margin-right: 5px;
}

.edit-button:hover {
  background-color: #cc8400;
}

.delete-button {
  background-color: #ff4d4d;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 3px;
  cursor: pointer;
}

.delete-button:hover {
  background-color: #cc0000;
}
</style>
