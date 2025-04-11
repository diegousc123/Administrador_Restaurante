<script setup>
  import { ChevronLeft } from 'lucide-vue-next'
  </script>

<template>
    <div class="mx-auto max-w-2xl">
      <div class="flex items-center gap-2 mb-6">
        <router-link to="/employees" class="p-2 border rounded-md hover:bg-gray-100">
          <ChevronLeft class="h-4 w-4" />
        </router-link>
        <h1 class="text-2xl font-bold">Agregar Nuevo Empleado</h1>
      </div>
      
      <div class="rounded-lg border bg-card text-card-foreground shadow-sm">
        <div class="flex flex-col space-y-1.5 p-6">
          <h3 class="text-2xl font-semibold leading-none tracking-tight">Información del Empleado</h3>
          <p class="text-sm text-muted-foreground">Ingrese los datos del nuevo empleado para agregarlo al sistema.</p>
        </div>
        <div class="p-6 space-y-4">
          <div class="grid grid-cols-2 gap-4">
            <div class="space-y-2">
              <label for="first-name" class="text-sm font-medium">Nombre</label>
              <input id="first-name" placeholder="Nombre" class="w-full px-3 py-2 border rounded-md" />
            </div>
            <div class="space-y-2">
              <label for="last-name" class="text-sm font-medium">Apellido</label>
              <input id="last-name" placeholder="Apellido" class="w-full px-3 py-2 border rounded-md" />
            </div>
          </div>
          <div class="space-y-2">
            <label for="email" class="text-sm font-medium">Correo Electrónico</label>
            <input id="email" type="email" placeholder="correo@ejemplo.com" class="w-full px-3 py-2 border rounded-md" />
          </div>
          <div class="space-y-2">
            <label for="phone" class="text-sm font-medium">Teléfono</label>
            <input id="phone" placeholder="+34 600 000 000" class="w-full px-3 py-2 border rounded-md" />
          </div>
          <div class="space-y-2">
            <label for="position" class="text-sm font-medium">Puesto</label>
            <select id="position" class="w-full px-3 py-2 border rounded-md">
              <option value="" disabled selected>Seleccionar puesto</option>
              <option value="chef">Chef</option>
              <option value="sous-chef">Sous Chef</option>
              <option value="waiter">Mesero</option>
              <option value="host">Anfitrión</option>
              <option value="manager">Gerente</option>
              <option value="bartender">Bartender</option>
            </select>
          </div>
          <div class="space-y-2">
            <label for="start-date" class="text-sm font-medium">Fecha de Inicio</label>
            <input id="start-date" type="date" class="w-full px-3 py-2 border rounded-md" />
          </div>
          <div class="space-y-2">
            <label for="salary" class="text-sm font-medium">Salario</label>
            <input id="salary" type="number" placeholder="0.00" class="w-full px-3 py-2 border rounded-md" />
          </div>
          <div class="space-y-2">
            <label for="notes" class="text-sm font-medium">Notas</label>
            <textarea id="notes" placeholder="Información adicional sobre el empleado..." class="w-full px-3 py-2 border rounded-md h-24"></textarea>
          </div>
        </div>
        <div class="flex items-center p-6 justify-between">
          <router-link to="/employees" class="px-4 py-2 border rounded-md hover:bg-gray-100">Cancelar</router-link>
          <button class="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700">Guardar Empleado</button>
        </div>
      </div>
    </div>
  </template>