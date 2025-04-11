<template>
  <div class="min-h-screen bg-gray-100">
    <!-- Login Page -->
    <div v-if="!isAuthenticated" class="flex items-center justify-center min-h-screen">
      <div class="w-full max-w-md p-8 space-y-8 bg-white rounded-lg shadow-md">
        <div class="text-center">
          <h1 class="text-3xl font-extrabold text-gray-900">ADMINISTRADOR DE RESTAURANTE</h1>
          <p class="mt-2 text-sm text-gray-600">Inicia sesión</p>
        </div>
        <form @submit.prevent="handleLogin" class="mt-8 space-y-6">
          <div class="rounded-md shadow-sm space-y-4">
            <div>
              <label for="email" class="block text-sm font-medium text-gray-700">Correo Electrónico</label>
              <input
                id="email"
                v-model="email"
                name="email"
                type="email"
                required
                class="relative block w-full px-3 py-2 mt-1 text-gray-900 placeholder-gray-500 border border-gray-300 rounded-md focus:outline-none focus:ring-emerald-500 focus:border-emerald-500 focus:z-10 sm:text-sm"
                placeholder="admin@restaurant.com"
              />
            </div>
            <div>
              <label for="password" class="block text-sm font-medium text-gray-700">Contraseña</label>
              <input
                id="password"
                v-model="password"
                name="password"
                type="password"
                required
                class="relative block w-full px-3 py-2 mt-1 text-gray-900 placeholder-gray-500 border border-gray-300 rounded-md focus:outline-none focus:ring-emerald-500 focus:border-emerald-500 focus:z-10 sm:text-sm"
                placeholder="Password"
              />
            </div>
          </div>

          <div>
            <button
              type="submit"
              class="relative flex justify-center w-full px-4 py-2 text-sm font-medium text-white bg-emerald-600 border border-transparent rounded-md group hover:bg-emerald-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500"
            >
              Inicia Sesión
            </button>
          </div>
        </form>
      </div>
    </div>

    <!-- Admin Dashboard -->
    <div v-else class="flex h-screen overflow-hidden">
      <!-- Sidebar -->
      <div class="hidden md:flex md:flex-shrink-0">
        <div class="flex flex-col w-64">
          <div class="flex flex-col flex-grow pt-5 overflow-y-auto bg-emerald-700">
            <div class="flex items-center flex-shrink-0 px-4">
              <h1 class="text-xl font-semibold text-white">Restaurant Admin</h1>
            </div>
            <div class="mt-5 flex-1 flex flex-col">
              <nav class="flex-1 px-2 space-y-1">
                <a 
                  v-for="item in navigation" 
                  :key="item.name"
                  @click="currentSection = item.id"
                  :class="[
                    currentSection === item.id 
                      ? 'bg-emerald-800 text-white' 
                      : 'text-emerald-100 hover:bg-emerald-600',
                    'group flex items-center px-2 py-2 text-sm font-medium rounded-md cursor-pointer'
                  ]"
                >
                  <component 
                    :is="item.icon" 
                    class="mr-3 flex-shrink-0 h-6 w-6" 
                    aria-hidden="true" 
                  />
                  {{ item.name }}
                </a>
              </nav>
            </div>
            <div class="p-4">
              <button 
                @click="handleLogout" 
                class="w-full flex items-center px-4 py-2 text-sm text-emerald-100 bg-emerald-800 rounded-md hover:bg-emerald-600"
              >
                <LogOut class="mr-3 h-5 w-5" />
                Logout
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Main content -->
      <div class="flex flex-col flex-1 overflow-hidden">
        <!-- Top navbar -->
        <div class="bg-white shadow-sm z-10">
          <div class="px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-16">
              <div class="flex">
                <div class="flex-shrink-0 flex items-center">
                  <button 
                    @click="mobileMenuOpen = !mobileMenuOpen" 
                    class="md:hidden inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-gray-500 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-emerald-500"
                  >
                    <Menu v-if="!mobileMenuOpen" class="block h-6 w-6" />
                    <X v-else class="block h-6 w-6" />
                  </button>
                </div>
                <div class="hidden md:ml-6 md:flex md:items-center">
                  <h1 class="text-xl font-semibold text-gray-900">
                    {{ navigation.find(nav => nav.id === currentSection)?.name }}
                  </h1>
                </div>
              </div>
              <div class="flex items-center">
                <div class="flex-shrink-0">
                  <span class="text-sm text-gray-500">Admin User</span>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Mobile menu -->
        <div v-if="mobileMenuOpen" class="md:hidden">
          <div class="pt-2 pb-3 space-y-1">
            <a 
              v-for="item in navigation" 
              :key="item.name"
              @click="currentSection = item.id; mobileMenuOpen = false"
              :class="[
                currentSection === item.id 
                  ? 'bg-emerald-50 border-emerald-500 text-emerald-700' 
                  : 'border-transparent text-gray-600 hover:bg-gray-50 hover:border-gray-300 hover:text-gray-800',
                'block pl-3 pr-4 py-2 border-l-4 text-base font-medium cursor-pointer'
              ]"
            >
              {{ item.name }}
            </a>
          </div>
          <div class="pt-4 pb-3 border-t border-gray-200">
            <div class="flex items-center px-4">
              <div class="flex-shrink-0">
                <div class="h-10 w-10 rounded-full bg-emerald-200 flex items-center justify-center">
                  <span class="text-emerald-600 font-semibold">A</span>
                </div>
              </div>
              <div class="ml-3">
                <div class="text-base font-medium text-gray-800">Admin User</div>
                <div class="text-sm font-medium text-gray-500">admin@restaurant.com</div>
              </div>
            </div>
            <div class="mt-3 space-y-1">
              <button 
                @click="handleLogout" 
                class="block w-full text-left px-4 py-2 text-base font-medium text-gray-500 hover:text-gray-800 hover:bg-gray-100"
              >
                Sign out
              </button>
            </div>
          </div>
        </div>

        <!-- Page content -->
        <main class="flex-1 relative overflow-y-auto focus:outline-none p-6">
          <!-- Dashboard -->
          <div v-if="currentSection === 'dashboard'" class="space-y-6">
            <div class="grid grid-cols-1 gap-5 sm:grid-cols-2 lg:grid-cols-3">
              <div class="bg-white overflow-hidden shadow rounded-lg">
                <div class="p-5">
                  <div class="flex items-center">
                    <div class="flex-shrink-0 bg-emerald-100 rounded-md p-3">
                      <Users class="h-6 w-6 text-emerald-600" />
                    </div>
                    <div class="ml-5 w-0 flex-1">
                      <dl>
                        <dt class="text-sm font-medium text-gray-500 truncate">Total Employees</dt>
                        <dd class="flex items-baseline">
                          <div class="text-2xl font-semibold text-gray-900">{{ employees.length }}</div>
                        </dd>
                      </dl>
                    </div>
                  </div>
                </div>
              </div>

              <div class="bg-white overflow-hidden shadow rounded-lg">
                <div class="p-5">
                  <div class="flex items-center">
                    <div class="flex-shrink-0 bg-emerald-100 rounded-md p-3">
                      <Clock class="h-6 w-6 text-emerald-600" />
                    </div>
                    <div class="ml-5 w-0 flex-1">
                      <dl>
                        <dt class="text-sm font-medium text-gray-500 truncate">Active Shifts</dt>
                        <dd class="flex items-baseline">
                          <div class="text-2xl font-semibold text-gray-900">3</div>
                        </dd>
                      </dl>
                    </div>
                  </div>
                </div>
              </div>

              <div class="bg-white overflow-hidden shadow rounded-lg">
                <div class="p-5">
                  <div class="flex items-center">
                    <div class="flex-shrink-0 bg-emerald-100 rounded-md p-3">
                      <Calendar class="h-6 w-6 text-emerald-600" />
                    </div>
                    <div class="ml-5 w-0 flex-1">
                      <dl>
                        <dt class="text-sm font-medium text-gray-500 truncate">Today's Date</dt>
                        <dd class="flex items-baseline">
                          <div class="text-2xl font-semibold text-gray-900">{{ currentDate }}</div>
                        </dd>
                      </dl>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <div class="bg-white shadow rounded-lg">
              <div class="px-4 py-5 sm:px-6 flex justify-between items-center">
                <h3 class="text-lg leading-6 font-medium text-gray-900">Recent Activity</h3>
              </div>
              <div class="border-t border-gray-200 px-4 py-5 sm:p-6">
                <div class="flow-root">
                  <ul class="-mb-8">
                    <li v-for="(activity, index) in recentActivities" :key="index" class="relative pb-8">
                      <div class="relative flex space-x-3">
                        <div>
                          <span class="h-8 w-8 rounded-full bg-emerald-100 flex items-center justify-center ring-8 ring-white">
                            <component :is="activity.icon" class="h-5 w-5 text-emerald-600" />
                          </span>
                        </div>
                        <div class="min-w-0 flex-1 pt-1.5 flex justify-between space-x-4">
                          <div>
                            <p class="text-sm text-gray-500">{{ activity.content }}</p>
                          </div>
                          <div class="text-right text-sm whitespace-nowrap text-gray-500">
                            <time>{{ activity.time }}</time>
                          </div>
                        </div>
                      </div>
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>

          <!-- Employees -->
          <div v-if="currentSection === 'employees'" class="space-y-6">
            <div class="flex justify-between items-center">
              <h2 class="text-xl font-semibold text-gray-900">Employee Management</h2>
              <button 
                @click="showAddEmployeeModal = true" 
                class="inline-flex items-center px-4 py-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-emerald-600 hover:bg-emerald-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500"
              >
                <UserPlus class="mr-2 -ml-1 h-5 w-5" />
                Add Employee
              </button>
            </div>

            <div class="bg-white shadow overflow-hidden sm:rounded-md">
              <ul class="divide-y divide-gray-200">
                <li v-for="employee in employees" :key="employee.id" class="px-6 py-4 flex items-center justify-between">
                  <div class="flex items-center">
                    <div class="h-10 w-10 rounded-full bg-emerald-100 flex items-center justify-center text-emerald-700 font-semibold">
                      {{ employee.firstName.charAt(0) }}{{ employee.lastName.charAt(0) }}
                    </div>
                    <div class="ml-4">
                      <div class="text-sm font-medium text-gray-900">{{ employee.firstName }} {{ employee.lastName }}</div>
                      <div class="text-sm text-gray-500">{{ employee.position }}</div>
                    </div>
                  </div>
                  <div class="flex space-x-2">
                    <button 
                      @click="editEmployee(employee)" 
                      class="inline-flex items-center p-2 border border-transparent rounded-full shadow-sm text-white bg-emerald-600 hover:bg-emerald-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500"
                    >
                      <Edit2 class="h-4 w-4" />
                    </button>
                    <button 
                      @click="deleteEmployee(employee.id)" 
                      class="inline-flex items-center p-2 border border-transparent rounded-full shadow-sm text-white bg-red-600 hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500"
                    >
                      <Trash2 class="h-4 w-4" />
                    </button>
                  </div>
                </li>
                <li v-if="employees.length === 0" class="px-6 py-4 text-center text-gray-500">
                  No employees found. Add your first employee!
                </li>
              </ul>
            </div>
          </div>

          <!-- Progress Tracking -->
          <div v-if="currentSection === 'progress'" class="space-y-6">
            <div class="flex justify-between items-center">
              <h2 class="text-xl font-semibold text-gray-900">Employee Progress Tracking</h2>
            </div>

            <div class="bg-white shadow overflow-hidden sm:rounded-md">
              <ul class="divide-y divide-gray-200">
                <li v-for="employee in employees" :key="employee.id" class="px-6 py-4">
                  <div class="flex items-center justify-between mb-2">
                    <div class="flex items-center">
                      <div class="h-10 w-10 rounded-full bg-emerald-100 flex items-center justify-center text-emerald-700 font-semibold">
                        {{ employee.firstName.charAt(0) }}{{ employee.lastName.charAt(0) }}
                      </div>
                      <div class="ml-4">
                        <div class="text-sm font-medium text-gray-900">{{ employee.firstName }} {{ employee.lastName }}</div>
                        <div class="text-sm text-gray-500">{{ employee.position }}</div>
                      </div>
                    </div>
                    <div>
                      <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800">
                        Active
                      </span>
                    </div>
                  </div>
                  <div class="mt-4">
                    <div class="flex justify-between mb-1">
                      <span class="text-sm font-medium text-gray-700">Performance</span>
                      <span class="text-sm font-medium text-gray-700">{{ employee.performance || 75 }}%</span>
                    </div>
                    <div class="w-full bg-gray-200 rounded-full h-2.5">
                      <div class="bg-emerald-600 h-2.5 rounded-full" :style="{ width: `${employee.performance || 75}%` }"></div>
                    </div>
                  </div>
                  <div class="mt-4 grid grid-cols-3 gap-4 text-center text-xs">
                    <div class="bg-gray-50 p-2 rounded">
                      <p class="text-gray-500">Hours this week</p>
                      <p class="font-semibold text-gray-900 mt-1">{{ employee.hoursThisWeek || 32 }}</p>
                    </div>
                    <div class="bg-gray-50 p-2 rounded">
                      <p class="text-gray-500">Tasks completed</p>
                      <p class="font-semibold text-gray-900 mt-1">{{ employee.tasksCompleted || 12 }}</p>
                    </div>
                    <div class="bg-gray-50 p-2 rounded">
                      <p class="text-gray-500">Training status</p>
                      <p class="font-semibold text-gray-900 mt-1">{{ employee.trainingStatus || 'Completed' }}</p>
                    </div>
                  </div>
                </li>
                <li v-if="employees.length === 0" class="px-6 py-4 text-center text-gray-500">
                  No employees found to track progress.
                </li>
              </ul>
            </div>
          </div>

          <!-- Information Management -->
          <div v-if="currentSection === 'information'" class="space-y-6">
            <div class="flex justify-between items-center">
              <h2 class="text-xl font-semibold text-gray-900">Restaurant Information</h2>
              <button 
                @click="editRestaurantInfo = true" 
                class="inline-flex items-center px-4 py-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-emerald-600 hover:bg-emerald-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500"
              >
                <Edit class="mr-2 -ml-1 h-5 w-5" />
                Edit Information
              </button>
            </div>

            <div class="bg-white shadow overflow-hidden sm:rounded-lg">
              <div class="px-4 py-5 sm:px-6">
                <h3 class="text-lg leading-6 font-medium text-gray-900">Restaurant Details</h3>
                <p class="mt-1 max-w-2xl text-sm text-gray-500">Basic information about your restaurant.</p>
              </div>
              <div class="border-t border-gray-200">
                <dl>
                  <div class="bg-gray-50 px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
                    <dt class="text-sm font-medium text-gray-500">Restaurant name</dt>
                    <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">{{ restaurantInfo.name }}</dd>
                  </div>
                  <div class="bg-white px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
                    <dt class="text-sm font-medium text-gray-500">Address</dt>
                    <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">{{ restaurantInfo.address }}</dd>
                  </div>
                  <div class="bg-gray-50 px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
                    <dt class="text-sm font-medium text-gray-500">Phone number</dt>
                    <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">{{ restaurantInfo.phone }}</dd>
                  </div>
                  <div class="bg-white px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
                    <dt class="text-sm font-medium text-gray-500">Email</dt>
                    <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">{{ restaurantInfo.email }}</dd>
                  </div>
                  <div class="bg-gray-50 px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
                    <dt class="text-sm font-medium text-gray-500">Opening hours</dt>
                    <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">{{ restaurantInfo.hours }}</dd>
                  </div>
                </dl>
              </div>
            </div>
          </div>
        </main>
      </div>
    </div>

    <!-- Add Employee Modal -->
    <div v-if="showAddEmployeeModal" class="fixed z-10 inset-0 overflow-y-auto" aria-labelledby="modal-title" role="dialog" aria-modal="true">
      <div class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" aria-hidden="true" @click="showAddEmployeeModal = false"></div>
        <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>
        <div class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full">
          <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
            <div class="sm:flex sm:items-start">
              <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left w-full">
                <h3 class="text-lg leading-6 font-medium text-gray-900" id="modal-title">
                  {{ editingEmployee ? 'Edit Employee' : 'Add New Employee' }}
                </h3>
                <div class="mt-4 space-y-4">
                  <div class="grid grid-cols-2 gap-4">
                    <div>
                      <label for="firstName" class="block text-sm font-medium text-gray-700">First Name</label>
                      <input 
                        type="text" 
                        id="firstName" 
                        v-model="newEmployee.firstName" 
                        class="mt-1 focus:ring-emerald-500 focus:border-emerald-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
                      />
                    </div>
                    <div>
                      <label for="lastName" class="block text-sm font-medium text-gray-700">Last Name</label>
                      <input 
                        type="text" 
                        id="lastName" 
                        v-model="newEmployee.lastName" 
                        class="mt-1 focus:ring-emerald-500 focus:border-emerald-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
                      />
                    </div>
                  </div>
                  <div>
                    <label for="email" class="block text-sm font-medium text-gray-700">Email</label>
                    <input 
                      type="email" 
                      id="email" 
                      v-model="newEmployee.email" 
                      class="mt-1 focus:ring-emerald-500 focus:border-emerald-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
                    />
                  </div>
                  <div>
                    <label for="phone" class="block text-sm font-medium text-gray-700">Phone</label>
                    <input 
                      type="text" 
                      id="phone" 
                      v-model="newEmployee.phone" 
                      class="mt-1 focus:ring-emerald-500 focus:border-emerald-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
                    />
                  </div>
                  <div>
                    <label for="position" class="block text-sm font-medium text-gray-700">Position</label>
                    <select 
                      id="position" 
                      v-model="newEmployee.position" 
                      class="mt-1 block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-emerald-500 focus:border-emerald-500 sm:text-sm"
                    >
                      <option value="">Select a position</option>
                      <option value="Manager">Manager</option>
                      <option value="Chef">Chef</option>
                      <option value="Waiter">Waiter</option>
                      <option value="Bartender">Bartender</option>
                      <option value="Host">Host</option>
                      <option value="Dishwasher">Dishwasher</option>
                    </select>
                  </div>
                  <div>
                    <label for="startDate" class="block text-sm font-medium text-gray-700">Start Date</label>
                    <input 
                      type="date" 
                      id="startDate" 
                      v-model="newEmployee.startDate" 
                      class="mt-1 focus:ring-emerald-500 focus:border-emerald-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
                    />
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
            <button 
              @click="saveEmployee" 
              type="button" 
              class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-emerald-600 text-base font-medium text-white hover:bg-emerald-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500 sm:ml-3 sm:w-auto sm:text-sm"
            >
              {{ editingEmployee ? 'Update' : 'Add' }}
            </button>
            <button 
              @click="showAddEmployeeModal = false" 
              type="button" 
              class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm"
            >
              Cancel
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Edit Restaurant Info Modal -->
    <div v-if="editRestaurantInfo" class="fixed z-10 inset-0 overflow-y-auto" aria-labelledby="modal-title" role="dialog" aria-modal="true">
      <div class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" aria-hidden="true" @click="editRestaurantInfo = false"></div>
        <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>
        <div class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full">
          <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
            <div class="sm:flex sm:items-start">
              <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left w-full">
                <h3 class="text-lg leading-6 font-medium text-gray-900" id="modal-title">
                  Edit Restaurant Information
                </h3>
                <div class="mt-4 space-y-4">
                  <div>
                    <label for="restaurantName" class="block text-sm font-medium text-gray-700">Restaurant Name</label>
                    <input 
                      type="text" 
                      id="restaurantName" 
                      v-model="restaurantInfo.name" 
                      class="mt-1 focus:ring-emerald-500 focus:border-emerald-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
                    />
                  </div>
                  <div>
                    <label for="address" class="block text-sm font-medium text-gray-700">Address</label>
                    <input 
                      type="text" 
                      id="address" 
                      v-model="restaurantInfo.address" 
                      class="mt-1 focus:ring-emerald-500 focus:border-emerald-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
                    />
                  </div>
                  <div>
                    <label for="phone" class="block text-sm font-medium text-gray-700">Phone</label>
                    <input 
                      type="text" 
                      id="phone" 
                      v-model="restaurantInfo.phone" 
                      class="mt-1 focus:ring-emerald-500 focus:border-emerald-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
                    />
                  </div>
                  <div>
                    <label for="email" class="block text-sm font-medium text-gray-700">Email</label>
                    <input 
                      type="email" 
                      id="email" 
                      v-model="restaurantInfo.email" 
                      class="mt-1 focus:ring-emerald-500 focus:border-emerald-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
                    />
                  </div>
                  <div>
                    <label for="hours" class="block text-sm font-medium text-gray-700">Opening Hours</label>
                    <input 
                      type="text" 
                      id="hours" 
                      v-model="restaurantInfo.hours" 
                      class="mt-1 focus:ring-emerald-500 focus:border-emerald-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
                      placeholder="Mon-Fri: 9AM-10PM, Sat-Sun: 10AM-11PM"
                    />
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
            <button 
              @click="saveRestaurantInfo" 
              type="button" 
              class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-emerald-600 text-base font-medium text-white hover:bg-emerald-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500 sm:ml-3 sm:w-auto sm:text-sm"
            >
              Save
            </button>
            <button 
              @click="editRestaurantInfo = false" 
              type="button" 
              class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm"
            >
              Cancel
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { 
  Menu, X, Users, Clock, Calendar, UserPlus, 
  Edit, Edit2, Trash2, LogOut, User, FileText, 
  BarChart2, Settings, Home, Coffee
} from 'lucide-vue-next'

// Authentication state
const isAuthenticated = ref(false)
const email = ref('')
const password = ref('')

// Navigation state
const currentSection = ref('dashboard')
const mobileMenuOpen = ref(false)

// Employee management
const employees = ref([])
const showAddEmployeeModal = ref(false)
const editingEmployee = ref(false)
const newEmployee = ref({
  id: null,
  firstName: '',
  lastName: '',
  email: '',
  phone: '',
  position: '',
  startDate: '',
  performance: 0,
  hoursThisWeek: 0,
  tasksCompleted: 0,
  trainingStatus: ''
})

// Restaurant information
const editRestaurantInfo = ref(false)
const restaurantInfo = ref({
  name: 'Delicious Restaurant',
  address: '123 Main Street, City, Country',
  phone: '+1 (555) 123-4567',
  email: 'info@deliciousrestaurant.com',
  hours: 'Mon-Fri: 9AM-10PM, Sat-Sun: 10AM-11PM'
})

// Navigation items
const navigation = [
  { id: 'dashboard', name: 'Dashboard', icon: Home },
  { id: 'employees', name: 'Employees', icon: Users },
  { id: 'progress', name: 'Progress Tracking', icon: BarChart2 },
  { id: 'information', name: 'Restaurant Info', icon: FileText }
]

// Recent activities
const recentActivities = ref([
  { 
    content: 'New employee John Doe was added', 
    time: '2 hours ago',
    icon: UserPlus
  },
  { 
    content: 'Restaurant information was updated', 
    time: '1 day ago',
    icon: Edit
  },
  { 
    content: 'Employee Maria Garcia completed training', 
    time: '2 days ago',
    icon: User
  }
])

// Current date
const currentDate = computed(() => {
  const date = new Date()
  return date.toLocaleDateString('en-US', { 
    year: 'numeric', 
    month: 'long', 
    day: 'numeric' 
  })
})

// Login handler
const handleLogin = () => {
  // In a real app, you would validate credentials against a backend
  if (email.value === 'admin@restaurant.com' && password.value === 'admin123') {
    isAuthenticated.value = true
    loadEmployees()
  } else {
    alert('Invalid credentials. Try admin@restaurant.com / admin123')
  }
}

// Logout handler
const handleLogout = () => {
  isAuthenticated.value = false
  email.value = ''
  password.value = ''
}

// Load sample employees
const loadEmployees = () => {
  employees.value = [
    {
      id: 1,
      firstName: 'John',
      lastName: 'Doe',
      email: 'john.doe@example.com',
      phone: '(555) 123-4567',
      position: 'Manager',
      startDate: '2023-01-15',
      performance: 85,
      hoursThisWeek: 40,
      tasksCompleted: 15,
      trainingStatus: 'Completed'
    },
    {
      id: 2,
      firstName: 'Maria',
      lastName: 'Garcia',
      email: 'maria.garcia@example.com',
      phone: '(555) 987-6543',
      position: 'Chef',
      startDate: '2023-02-10',
      performance: 92,
      hoursThisWeek: 38,
      tasksCompleted: 20,
      trainingStatus: 'Completed'
    },
    {
      id: 3,
      firstName: 'Robert',
      lastName: 'Johnson',
      email: 'robert.johnson@example.com',
      phone: '(555) 456-7890',
      position: 'Waiter',
      startDate: '2023-03-05',
      performance: 78,
      hoursThisWeek: 32,
      tasksCompleted: 12,
      trainingStatus: 'In Progress'
    }
  ]
}

// Edit employee
const editEmployee = (employee) => {
  editingEmployee.value = true
  newEmployee.value = { ...employee }
  showAddEmployeeModal.value = true
}

// Delete employee
const deleteEmployee = (id) => {
  if (confirm('Are you sure you want to delete this employee?')) {
    employees.value = employees.value.filter(emp => emp.id !== id)
  }
}

// Save employee
const saveEmployee = () => {
  if (editingEmployee.value) {
    // Update existing employee
    const index = employees.value.findIndex(emp => emp.id === newEmployee.value.id)
    if (index !== -1) {
      employees.value[index] = { ...newEmployee.value }
    }
  } else {
    // Add new employee
    const id = employees.value.length > 0 
      ? Math.max(...employees.value.map(emp => emp.id)) + 1 
      : 1
    employees.value.push({
      ...newEmployee.value,
      id,
      performance: Math.floor(Math.random() * 30) + 70, // Random performance between 70-100
      hoursThisWeek: Math.floor(Math.random() * 10) + 30, // Random hours between 30-40
      tasksCompleted: Math.floor(Math.random() * 15) + 5, // Random tasks between 5-20
      trainingStatus: Math.random() > 0.5 ? 'Completed' : 'In Progress' // Random training status
    })
  }
  
  // Reset form and close modal
  newEmployee.value = {
    id: null,
    firstName: '',
    lastName: '',
    email: '',
    phone: '',
    position: '',
    startDate: ''
  }
  editingEmployee.value = false
  showAddEmployeeModal.value = false
}

// Save restaurant info
const saveRestaurantInfo = () => {
  // In a real app, you would save this to a backend
  editRestaurantInfo.value = false
  
  // Add activity
  recentActivities.value.unshift({
    content: 'Restaurant information was updated',
    time: 'Just now',
    icon: Edit
  })
}

// Initialize with current date for new employee
onMounted(() => {
  const today = new Date()
  const year = today.getFullYear()
  const month = String(today.getMonth() + 1).padStart(2, '0')
  const day = String(today.getDate()).padStart(2, '0')
  newEmployee.value.startDate = `${year}-${month}-${day}`
})
</script>