<template>
    <div class="modal-overlay" v-if="isVisible">
      <div class="modal-content">
        <h3>Confirm Delete</h3>
        <p>Are you sure you want to delete this user?</p>
        <div class="modal-actions">
          <button class="btn-cancel" @click="closeModal">Cancel</button>
          <button class="btn-delete" @click="confirmDelete">Delete</button>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, defineProps, emit } from 'vue';
  
  const props = defineProps({
    isVisible: Boolean,
  });
  
  const emitEvent = emit;
  
  const closeModal = () => {
    emitEvent('close');
  };
  
  const confirmDelete = () => {
    emitEvent('confirmDelete');
  };
  </script>
  
  <style scoped>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .modal-content {
    background: white;
    padding: 20px;
    border-radius: 8px;
    text-align: center;
    max-width: 400px;
    width: 100%;
  }
  
  .modal-actions {
    margin-top: 20px;
    display: flex;
    justify-content: space-around;
  }
  
  .btn-cancel {
    background: #ccc;
    color: #333;
    padding: 8px 16px;
    border: none;
    cursor: pointer;
  }
  
  .btn-delete {
    background: red;
    color: white;
    padding: 8px 16px;
    border: none;
    cursor: pointer;
  }
  </style>
  