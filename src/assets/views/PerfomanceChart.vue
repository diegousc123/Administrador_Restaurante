<template>
    <div class="space-y-4">
      <div class="flex justify-between items-center">
        <h3 class="text-lg font-medium">Rendimiento del Personal</h3>
        <select class="px-3 py-2 border rounded-md w-[180px]" v-model="selectedDepartment">
          <option value="all">Todos los departamentos</option>
          <option value="kitchen">Cocina</option>
          <option value="service">Servicio</option>
          <option value="bar">Bar</option>
          <option value="admin">Administración</option>
        </select>
      </div>
      <div class="h-[400px] w-full">
        <canvas ref="chartCanvas" width="800" height="400" class="w-full h-full"></canvas>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <div class="rounded-lg border bg-card text-card-foreground shadow-sm">
          <div class="p-6 pb-2">
            <h3 class="text-sm font-medium">Mejor Departamento</h3>
          </div>
          <div class="p-6 pt-0">
            <div class="text-2xl font-bold">Cocina</div>
            <p class="text-xs text-muted-foreground">90% rendimiento promedio</p>
          </div>
        </div>
        <div class="rounded-lg border bg-card text-card-foreground shadow-sm">
          <div class="p-6 pb-2">
            <h3 class="text-sm font-medium">Empleado del Mes</h3>
          </div>
          <div class="p-6 pt-0">
            <div class="text-2xl font-bold">Carlos Rodríguez</div>
            <p class="text-xs text-muted-foreground">Chef - 95% rendimiento</p>
          </div>
        </div>
        <div class="rounded-lg border bg-card text-card-foreground shadow-sm">
          <div class="p-6 pb-2">
            <h3 class="text-sm font-medium">Tendencia</h3>
          </div>
          <div class="p-6 pt-0">
            <div class="text-2xl font-bold text-green-500">+5%</div>
            <p class="text-xs text-muted-foreground">Desde el mes anterior</p>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  
  const chartCanvas = ref(null)
  const selectedDepartment = ref('all')
  
  onMounted(() => {
    const ctx = chartCanvas.value.getContext('2d')
    if (!ctx) return
  
    // Datos de ejemplo para el gráfico
    const months = ["Ene", "Feb", "Mar", "Abr", "May", "Jun", "Jul", "Ago", "Sep"]
    const performanceData = [75, 78, 80, 79, 85, 83, 90, 88, 87]
  
    // Configuración del gráfico
    const width = ctx.canvas.width
    const height = ctx.canvas.height
    const padding = 40
    const chartWidth = width - padding * 2
    const chartHeight = height - padding * 2
  
    // Limpiar canvas
    ctx.clearRect(0, 0, width, height)
  
    // Dibujar ejes
    ctx.beginPath()
    ctx.moveTo(padding, padding)
    ctx.lineTo(padding, height - padding)
    ctx.lineTo(width - padding, height - padding)
    ctx.strokeStyle = "#e2e8f0"
    ctx.stroke()
  
    // Dibujar líneas de cuadrícula horizontales
    for (let i = 0; i <= 5; i++) {
      const y = padding + (chartHeight / 5) * i
      ctx.beginPath()
      ctx.moveTo(padding, y)
      ctx.lineTo(width - padding, y)
      ctx.strokeStyle = "#e2e8f0"
      ctx.stroke()
  
      // Etiquetas del eje Y
      const label = 100 - i * 20
      ctx.fillStyle = "#64748b"
      ctx.font = "12px sans-serif"
      ctx.textAlign = "right"
      ctx.fillText(`${label}%`, padding - 10, y + 4)
    }
  
    // Dibujar línea de rendimiento
    ctx.beginPath()
    for (let i = 0; i < performanceData.length; i++) {
      const x = padding + (chartWidth / (performanceData.length - 1)) * i
      const y = padding + chartHeight - (chartHeight * performanceData[i]) / 100
  
      if (i === 0) {
        ctx.moveTo(x, y)
      } else {
        ctx.lineTo(x, y)
      }
  
      // Etiquetas del eje X
      ctx.fillStyle = "#64748b"
      ctx.font = "12px sans-serif"
      ctx.textAlign = "center"
      ctx.fillText(months[i], x, height - padding + 20)
    }
    ctx.strokeStyle = "#0ea5e9"
    ctx.lineWidth = 3
    ctx.stroke()
  
    // Dibujar puntos en la línea
    for (let i = 0; i < performanceData.length; i++) {
      const x = padding + (chartWidth / (performanceData.length - 1)) * i
      const y = padding + chartHeight - (chartHeight * performanceData[i]) / 100
  
      ctx.beginPath()
      ctx.arc(x, y, 6, 0, Math.PI * 2)
      ctx.fillStyle = "#0ea5e9"
      ctx.fill()
      ctx.strokeStyle = "#fff"
      ctx.lineWidth = 2
      ctx.stroke()
    }
  })
  </script>