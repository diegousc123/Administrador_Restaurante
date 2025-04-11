<script setup>
  import { ref } from 'vue'
  import { Plus, Search } from 'lucide-vue-next'
  import EmployeeTable from '../components/EmployeeTable.vue'
  
  const activeTab = ref('todos')
  const tabs = [
    { label: 'Todos', value: 'todos' },
    { label: 'Cocineros', value: 'cocineros' },
    { label: 'Meseros', value: 'meseros' },
    { label: 'Administrativos', value: 'administrativos' }
  ]
  </script>
  
<template>
    <div class="flex flex-col gap-6">
      <div class="flex items-center justify-between">
        <h1 class="text-3xl font-bold">Gestión de Empleados</h1>
        <router-link to="/employees/new" class="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700 flex items-center">
          <Plus class="mr-2 h-4 w-4" />
          Agregar Empleado
        </router-link>
      </div>
      <div class="flex items-center gap-4">
        <div class="relative flex-1 max-w-sm">
          <Search class="absolute left-2.5 top-2.5 h-4 w-4 text-muted-foreground" />
          <input type="search" placeholder="Buscar empleados..." class="w-full px-8 py-2 border rounded-md" />
        </div>
        <button class="px-4 py-2 border rounded-md hover:bg-gray-100">Filtros</button>
      </div>
      
      <div class="border-b">
        <div class="flex -mb-px space-x-6">
          <button 
            v-for="tab in tabs" 
            :key="tab.value" 
            @click="activeTab = tab.value"
            class="px-4 py-2 border-b-2 transition-colors"
            :class="activeTab === tab.value ? 'border-blue-500 text-blue-600' : 'border-transparent hover:text-blue-600'"
          >
            {{ tab.label }}
          </button>
        </div>
      </div>
      
      <div class="border rounded-md">
        <EmployeeTable />
      </div>
    </div>
  </template>