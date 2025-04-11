<template>
    <div class="w-full">
      <table class="w-full">
        <thead>
          <tr class="border-b">
            <th class="h-12 px-4 text-left align-middle font-medium w-12">
              <input type="checkbox" :checked="selectedEmployees.length === employees.length" @change="toggleAll" />
            </th>
            <th class="h-12 px-4 text-left align-middle font-medium">Nombre</th>
            <th class="h-12 px-4 text-left align-middle font-medium hidden md:table-cell">Puesto</th>
            <th class="h-12 px-4 text-left align-middle font-medium hidden md:table-cell">Departamento</th>
            <th class="h-12 px-4 text-left align-middle font-medium hidden lg:table-cell">Fecha Inicio</th>
            <th class="h-12 px-4 text-left align-middle font-medium">Rendimiento</th>
            <th class="h-12 px-4 text-left align-middle font-medium">Estado</th>
            <th class="h-12 px-4 text-right align-middle font-medium">Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="employee in employees" :key="employee.id" class="border-b">
            <td class="p-4 align-middle">
              <input 
                type="checkbox" 
                :checked="selectedEmployees.includes(employee.id)" 
                @change="toggleEmployee(employee.id)" 
              />
            </td>
            <td class="p-4 align-middle font-medium">{{ employee.name }}</td>
            <td class="p-4 align-middle hidden md:table-cell">{{ employee.position }}</td>
            <td class="p-4 align-middle hidden md:table-cell">{{ employee.department }}</td>
            <td class="p-4 align-middle hidden lg:table-cell">{{ employee.startDate }}</td>
            <td class="p-4 align-middle">
              <span 
                class="inline-flex items-center rounded-full px-2.5 py-0.5 text-xs font-semibold text-white"
                :class="getPerformanceBadgeClass(employee.performance)"
              >
                {{ employee.performance }}
              </span>
            </td>
            <td class="p-4 align-middle">
              <span 
                class="inline-flex items-center rounded-full px-2.5 py-0.5 text-xs font-semibold"
                :class="employee.status === 'Activo' ? 'bg-blue-100 text-blue-800' : 'bg-gray-100 text-gray-800'"
              >
                {{ employee.status }}
              </span>
            </td>
            <td class="p-4 align-middle text-right">
              <div class="relative inline-block text-left">
                <button @click="toggleDropdown(employee.id)" class="p-2 hover:bg-gray-100 rounded-md">
                  <MoreHorizontal class="h-4 w-4" />
                </button>
                <div 
                  v-if="openDropdown === employee.id" 
                  class="absolute right-0 z-10 mt-2 w-56 rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5"
                >
                  <div class="py-1 px-2 text-sm text-gray-700">Acciones</div>
                  <div class="h-px bg-gray-200 my-1"></div>
                  <router-link :to="`/employees/${employee.id}`" class="block px-4 py-2 text-sm hover:bg-gray-100">
                    Ver detalles
                  </router-link>
                  <router-link :to="`/employees/${employee.id}/edit`" class="flex items-center px-4 py-2 text-sm hover:bg-gray-100">
                    <Edit class="mr-2 h-4 w-4" />
                    Editar
                  </router-link>
                  <button class="flex items-center px-4 py-2 text-sm text-red-600 hover:bg-gray-100 w-full text-left">
                    <Trash2 class="mr-2 h-4 w-4" />
                    Eliminar
                  </button>
                </div>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  import { MoreHorizontal, Edit, Trash2 } from 'lucide-vue-next'
  
  // Datos de ejemplo para la tabla
  const employees = [
    {
      id: "1",
      name: "Carlos Rodríguez",
      position: "Chef",
      department: "Cocina",
      status: "Activo",
      startDate: "15/03/2022",
      performance: "Excelente",
    },
    {
      id: "2",
      name: "María López",
      position: "Mesera",
      department: "Servicio",
      status: "Activo",
      startDate: "10/05/2022",
      performance: "Bueno",
    },
    {
      id: "3",
      name: "Juan Martínez",
      position: "Bartender",
      department: "Bar",
      status: "Activo",
      startDate: "22/01/2023",
      performance: "Bueno",
    },
    {
      id: "4",
      name: "Ana García",
      position: "Gerente",
      department: "Administración",
      status: "Activo",
      startDate: "05/11/2021",
      performance: "Excelente",
    },
    {
      id: "5",
      name: "Pedro Sánchez",
      position: "Sous Chef",
      department: "Cocina",
      status: "Activo",
      startDate: "18/07/2022",
      performance: "Regular",
    },
    {
      id: "6",
      name: "Laura Fernández",
      position: "Anfitriona",
      department: "Servicio",
      status: "Inactivo",
      startDate: "30/09/2022",
      performance: "Bueno",
    },
  ]
  
  const selectedEmployees = ref([])
  const openDropdown = ref(null)
  
  const toggleEmployee = (employeeId) => {
    if (selectedEmployees.value.includes(employeeId)) {
      selectedEmployees.value = selectedEmployees.value.filter(id => id !== employeeId)
    } else {
      selectedEmployees.value.push(employeeId)
    }
  }
  
  const toggleAll = () => {
    if (selectedEmployees.value.length === employees.length) {
      selectedEmployees.value = []
    } else {
      selectedEmployees.value = employees.map(employee => employee.id)
    }
  }
  
  const toggleDropdown = (employeeId) => {
    if (openDropdown.value === employeeId) {
      openDropdown.value = null
    } else {
      openDropdown.value = employeeId
    }
  }
  
  const getPerformanceBadgeClass = (performance) => {
    switch (performance) {
      case "Excelente":
        return "bg-green-500"
      case "Bueno":
        return "bg-emerald-500"
      case "Regular":
        return "bg-yellow-500"
      case "Deficiente":
        return "bg-red-500"
      default:
        return "bg-gray-500"
    }
  }
  </script>