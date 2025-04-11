<script setup>
import { ref } from 'vue'
import EmployeeTable from '../components/EmployeeTable.vue'
import PerformanceChart from '../components/PerformanceChart.vue'
import RecentActivity from '../components/RecentActivity.vue'

const activeTab = ref('empleados')
const tabs = [
  { label: 'Empleados', value: 'empleados' },
  { label: 'Rendimiento', value: 'rendimiento' },
  { label: 'Actividad Reciente', value: 'actividad' }
]
</script>

<template>
    <div class="flex flex-col gap-6">
      <div class="flex items-center justify-between">
        <h1 class="text-3xl font-bold">Dashboard</h1>
        <router-link to="/employees/new" class="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700">
          Agregar Empleado
        </router-link>
      </div>
      <div class="grid gap-6 md:grid-cols-2 lg:grid-cols-4">
        <div class="rounded-lg border bg-card text-card-foreground shadow-sm">
          <div class="p-6 pb-2">
            <h3 class="text-sm font-medium">Total Empleados</h3>
          </div>
          <div class="p-6 pt-0">
            <div class="text-2xl font-bold">24</div>
            <p class="text-xs text-muted-foreground">+2 desde el mes pasado</p>
          </div>
        </div>
        <div class="rounded-lg border bg-card text-card-foreground shadow-sm">
          <div class="p-6 pb-2">
            <h3 class="text-sm font-medium">Horas Trabajadas</h3>
          </div>
          <div class="p-6 pt-0">
            <div class="text-2xl font-bold">1,420</div>
            <p class="text-xs text-muted-foreground">+180 desde el mes pasado</p>
          </div>
        </div>
        <div class="rounded-lg border bg-card text-card-foreground shadow-sm">
          <div class="p-6 pb-2">
            <h3 class="text-sm font-medium">Rendimiento Promedio</h3>
          </div>
          <div class="p-6 pt-0">
            <div class="text-2xl font-bold">87%</div>
            <p class="text-xs text-muted-foreground">+2% desde el mes pasado</p>
          </div>
        </div>
        <div class="rounded-lg border bg-card text-card-foreground shadow-sm">
          <div class="p-6 pb-2">
            <h3 class="text-sm font-medium">Ausencias</h3>
          </div>
          <div class="p-6 pt-0">
            <div class="text-2xl font-bold">3</div>
            <p class="text-xs text-muted-foreground">-2 desde el mes pasado</p>
          </div>
        </div>
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
        <div v-if="activeTab === 'empleados'">
          <EmployeeTable />
        </div>
        <div v-if="activeTab === 'rendimiento'" class="p-4">
          <PerformanceChart />
        </div>
        <div v-if="activeTab === 'actividad'">
          <RecentActivity />
        </div>
      </div>
    </div>
  </template>
  