<template>
  <div class="min-h-screen bg-gray-50">
    <div class="grid md:grid-cols-[280px_1fr]">
      <!-- Sidebar -->
      <aside class="hidden md:flex flex-col h-screen bg-white border-r border-gray-200 shadow-sm">
        <!-- Navigation -->
        <div class="flex-1 overflow-auto py-6 px-4">
          <div class="mb-8">
            <button 
              @click="exportToPDF" 
              :disabled="!isPdfLibsReady || isPdfLibsLoading"
              class="w-full flex items-center justify-center gap-2 rounded-lg px-4 py-3 text-sm font-medium transition-all bg-gray-100 text-gray-800 hover:bg-gray-200 disabled:opacity-50 disabled:cursor-not-allowed"
            >
              <svg v-if="isPdfLibsLoading" class="animate-spin -ml-1 mr-2 h-4 w-4 text-gray-800" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.0 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
              </svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
                <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
                <polyline points="7 10 12 15 17 10"></polyline>
                <line x1="12" y1="15" x2="12" y2="3"></line>
              </svg>
              {{ isPdfLibsLoading ? 'Ielādē PDF...' : 'Eksportēt PDF' }}
            </button>
          </div>
          
          <nav class="space-y-1.5">
            <button 
              @click="currentTab = 'orders'" 
              :class="[ 
                'w-full flex items-center gap-3 rounded-lg px-4 py-3 text-sm font-medium transition-all duration-200', 
                currentTab === 'orders' 
                  ? 'bg-gray-900 text-white shadow-md' 
                  : 'text-gray-600 hover:text-gray-900 hover:bg-gray-100' 
              ]"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" class="flex-shrink-0">
                <path d="M16 4h2a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V6a2 2 0 0 1 2-2h2"></path>
                <rect x="8" y="2" width="8" height="4" rx="1" ry="1"></rect>
              </svg>
              Pasūtījumi
            </button>
            
            <button 
              @click="currentTab = 'materials'" 
              :class="[ 
                'w-full flex items-center gap-3 rounded-lg px-4 py-3 text-sm font-medium transition-all duration-200', 
                currentTab === 'materials' 
                  ? 'bg-gray-900 text-white shadow-md' 
                  : 'text-gray-600 hover:text-gray-900 hover:bg-gray-100' 
              ]"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" class="flex-shrink-0">
                <path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"></path>
                <polyline points="3.27 6.96 12 12.01 20.73 6.96"></polyline>
                <line x1="12" y1="22.08" x2="12" y2="12"></line>
              </svg>
              Materiāli
            </button>
            
            <button 
              @click="currentTab = 'workers'" 
              :class="[ 
                'w-full flex items-center gap-3 rounded-lg px-4 py-3 text-sm font-medium transition-all duration-200', 
                currentTab === 'workers' 
                  ? 'bg-gray-900 text-white shadow-md' 
                  : 'text-gray-600 hover:text-gray-900 hover:bg-gray-100' 
              ]"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" class="flex-shrink-0">
                <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
                <circle cx="9" cy="7" r="4"></circle>
                <path d="M23 21v-2a4 4 0 0 0-3-3.87"></path>
                <path d="M16 3.13a4 4 0 0 1 0 7.75"></path>
              </svg>
              Darbinieki
            </button>
            
            <button 
              @click="currentTab = 'shifts'" 
              :class="[ 
                'w-full flex items-center gap-3 rounded-lg px-4 py-3 text-sm font-medium transition-all duration-200', 
                currentTab === 'shifts' 
                  ? 'bg-gray-900 text-white shadow-md' 
                  : 'text-gray-600 hover:text-gray-900 hover:bg-gray-100' 
              ]"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" class="flex-shrink-0">
                <rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect>
                <line x1="16" y1="2" x2="16" y2="6"></line>
                <line x1="8" y1="2" x2="8" y2="6"></line>
                <line x1="3" y1="10" x2="21" y2="10"></line>
              </svg>
              Maiņu statistika
            </button>
            
            <button 
              @click="currentTab = 'stats'" 
              :class="[ 
                'w-full flex items-center gap-3 rounded-lg px-4 py-3 text-sm font-medium transition-all duration-200', 
                currentTab === 'stats' 
                  ? 'bg-gray-900 text-white shadow-md' 
                  : 'text-gray-600 hover:text-gray-900 hover:bg-gray-100' 
              ]"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
                <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
                <polyline points="7 10 12 15 17 10"></polyline>
                <line x1="12" y1="15" x2="12" y2="3"></line>
              </svg>
              Statistika
            </button>
          </nav>
        </div>
      </aside>


      <main class="p-6 md:p-8 overflow-auto h-screen">
        <!-- Add/Edit Dialog -->
        <div v-if="showAddDialog || showEditDialog" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50">
          <div class="bg-white p-6 rounded-xl shadow-2xl w-full max-w-md transform transition-all duration-300 scale-100 max-h-screen overflow-y-auto">
            <h2 class="text-xl font-semibold mb-6 text-gray-800">
              {{ showEditDialog ? 'Rediģēt' : 'Pievienot' }}
            </h2>
            <form @submit.prevent="showEditDialog ? saveEdit() : (currentTab === 'orders' ? addOrder() : currentTab === 'materials' ? addMaterial() : addEmployee())">
              <!-- Order Form -->
              <div v-if="currentTab === 'orders'" class="space-y-4">
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Nosaukums</label>
                  <input 
                    v-model="formData.nosaukums"
                    class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                    placeholder="Nosaukums"
                  />
                </div>
                <div class="mb-4">
                  <label class="block text-sm font-medium text-gray-700 mb-1">Daudzums</label>
                  <input 
                    v-model.number="formData.daudzums"
                    type="number"
                    min="0.01"
                    step="0.01"
                    class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                    placeholder="Daudzums"
                  />
                </div>

                <!-- Material search input -->
                <div class="mb-4">
                  <label class="block text-sm font-medium text-gray-700 mb-1">Materiāli</label>
                  <div class="relative">
                    <input 
                      v-model="materialSearch"
                      type="text"
                      class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                      placeholder="Meklēt materiālus..."
                      @input="filterMaterials"
                    />
                    <div v-if="materialSearch && filteredMaterials.length > 0" class="absolute z-10 w-full mt-1 bg-white border border-gray-300 rounded-lg shadow-lg max-h-60 overflow-y-auto">
                      <div 
                        v-for="material in filteredMaterials" 
                        :key="material.id"
                        class="p-2 hover:bg-gray-100 cursor-pointer"
                        @click="addMaterialToOrder(material)"
                      >
                        {{ material.nosaukums }} ({{ material.daudzums }} {{ material.vieniba }})
                      </div>
                    </div>
                  </div>
                </div>

                <div v-if="orderMaterials.length" class="mt-4 bg-gray-50 p-4 rounded-lg border border-gray-200">
                  <h4 class="text-sm font-semibold mb-3 text-gray-700">Materiāli pasūtījumā:</h4>
                  <div v-for="(mat, index) in orderMaterials" :key="mat.material_id || mat.id" class="flex items-center gap-3 mb-2 p-2 bg-white rounded-lg border border-gray-100 shadow-sm">
                    <span class="flex-1 font-medium text-gray-800">{{ getMaterialName(mat.material_id || mat.id) }}</span>
                    <div class="flex items-center gap-2">
                      <input
                        type="number"
                        v-model.number="mat.quantity"
                        min="0.01"
                        step="0.01"
                        class="w-20 border border-gray-300 p-1.5 rounded-md text-center"
                      />
                      <span class="text-sm text-gray-500">{{ getMaterialUnit(mat.material_id || mat.id) }}</span>
                      <button
                        type="button"
                        @click="orderMaterials.splice(index, 1)"
                        class="text-red-500 hover:text-red-700 transition-colors p-1"
                      >
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                          <path d="M3 6h18"></path>
                          <path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"></path>
                          <path d="M10 11v6"></path>
                          <path d="M14 11v6"></path>
                          <path d="M5 6l1 16h12l1-16"></path>
                        </svg>
                      </button>
                    </div>
                  </div>
                </div>
                
                <div class="mb-4">
                  <label class="block text-sm font-medium text-gray-700 mb-1">Statuss</label>
                  <select
                    v-model="formData.status"
                    class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                  >
                    <option value="Nav sākts">Nav sākts</option>
                    <option value="Pieņemts">Pieņemts</option>
                    <option value="Pabeigts">Pabeigts</option>
                    <option value="Atcelts">Atcelts</option>
                  </select>
                </div>
              </div>

              <!-- Material Form -->
              <div v-else-if="currentTab === 'materials'" class="space-y-4">
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Nosaukums</label>
                  <input 
                    v-model="formData.nosaukums"
                    class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                    placeholder="Nosaukums"
                  />
                </div>
                <div class="mb-4">
                  <label class="block text-sm font-medium text-gray-700 mb-1">Daudzums</label>
                  <input 
                    v-model.number="formData.daudzums"
                    type="number"
                    min="0.01"
                    step="0.01"
                    class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                    placeholder="Daudzums"
                  />
                </div>
                <div class="mb-4">
                  <label class="block text-sm font-medium text-gray-700 mb-1">Vienība</label>
                  <input 
                    v-model="formData.vieniba"
                    class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                    placeholder="Vienība"
                  />
                </div>
                <div class="mb-4">
                  <label class="block text-sm font-medium text-gray-700 mb-1">Noliktava</label>
                  <select 
                    v-model="formData.noliktava"
                    class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                  >
                    <option v-for="option in warehouseOptions" :key="option" :value="option">{{ option }}</option>
                  </select>
                </div>
                <div class="mb-4">
                  <label class="block text-sm font-medium text-gray-700 mb-1">Vieta</label>
                  <input 
                    v-model="formData.vieta"
                    class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                    placeholder="Vieta noliktavā"
                  />
                </div>
              </div>

              <!-- Employee Form -->
              <div v-else-if="currentTab === 'workers'" class="space-y-4">
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Vārds</label>
                  <input 
                    v-model="formData.vards"
                    class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                    placeholder="Vārds"
                  />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Uzvārds</label>
                  <input 
                    v-model="formData.uzvards"
                    class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                    placeholder="Uzvārds"
                  />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Kods</label>
                  <input 
                    v-model="formData.kods"
                    class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                    placeholder="Kods"
                  />
                </div>
                <div class="grid grid-cols-2 gap-4">
                  <div>
                    <label class="block text-sm font-medium text-gray-700 mb-1">Amats *</label>
                    <select
                      v-model="formData.amats"
                      required
                      class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                    >
                      <option value="">Izvēlieties amatu</option>
                      <option value="Administrators">Administrators</option>
                      <option value="Noliktavas pārzinis">Noliktavas pārzinis</option>
                      <option value="Ražotājs">Ražotājs</option>
                    </select>
                  </div>
                </div>
                <div v-if="formData.amats === 'Administrators'" class="grid grid-cols-2 gap-4">
                  <div>
                    <label class="block text-sm font-medium text-gray-700 mb-1">Parole {{ !editData ? '*' : '' }}</label>
                    <input
                      v-model="formData.password"
                      type="password"
                      :required="!editData"
                      class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                    />
                  </div>
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-700 mb-1">Statuss</label>
                  <select
                    v-model="formData.status"
                    class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                  >
                    <option value="Aktīvs">Aktīvs</option>
                    <option value="Neaktīvs">Neaktīvs</option>
                  </select>
                </div>
              </div>

              <div class="mt-6 flex justify-end space-x-3">
                <button 
                  type="button" 
                  @click="closeDialog" 
                  class="px-5 py-2.5 rounded-lg text-gray-600 border border-gray-300 hover:bg-gray-100 transition-colors"
                >
                  Atcelt
                </button>
                <button 
                  type="submit" 
                  class="px-5 py-2.5 rounded-lg bg-gray-800 text-white hover:bg-gray-900 transition-colors"
                >
                  {{ showEditDialog ? 'Saglabāt izmaiņas' : 'Pievienot' }}
                </button>
              </div>
            </form>
          </div>
        </div>

        <!-- Transfer Material Dialog -->
        <div v-if="showTransferDialog" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50">
          <div class="bg-white p-6 rounded-xl shadow-2xl w-full max-w-md transform transition-all duration-300 scale-100">
            <h2 class="text-xl font-semibold mb-6 text-gray-800">Pārsūtīt materiālu: {{ transferData.nosaukums }}</h2>
            <form @submit.prevent="transferMaterial">
              <div class="mb-4">
                <label class="block text-sm font-medium text-gray-700 mb-1">Daudzums (Max: {{ transferData.max }} {{ transferData.vieniba }})</label>
                <input
                  v-model.number="transferData.daudzums"
                  type="number"
                  min="0.01"
                  :max="transferData.max"
                  step="0.01"
                  class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                />
              </div>
              <div class="mb-4">
                <label class="block text-sm font-medium text-gray-700 mb-1">No noliktavas</label>
                <input
                  v-model="transferData.fromNoliktava"
                  type="text"
                  disabled
                  class="p-3 w-full bg-gray-100 border border-gray-300 rounded-lg outline-none"
                />
              </div>
              <div class="mb-4">
                <label class="block text-sm font-medium text-gray-700 mb-1">Uz noliktavu</label>
                <select
                  v-model="transferData.toNoliktava"
                  class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                >
                  <option value="">Izvēlēties noliktavu</option>
                  <option v-for="option in warehouseOptions.filter(w => w !== transferData.fromNoliktava)" :key="option" :value="option">{{ option }}</option>
                </select>
              </div>
              <div class="mt-6 flex justify-end space-x-3">
                <button type="button" @click="closeDialog" class="px-5 py-2.5 rounded-lg text-gray-600 border border-gray-300 hover:bg-gray-100 transition-colors">Atcelt</button>
                <button type="submit" class="px-5 py-2.5 rounded-lg bg-gray-800 text-white hover:bg-gray-900 transition-colors">Pārsūtīt</button>
              </div>
            </form>
          </div>
        </div>

        <!-- Order Details Dialog -->
        <div v-if="showDetailsDialog" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50">
          <div class="bg-white p-6 rounded-xl shadow-2xl w-full max-w-lg transform transition-all duration-300 scale-100 max-h-screen overflow-y-auto">
            <h2 class="text-xl font-semibold mb-6 text-gray-800">Pasūtījuma detaļas</h2>
            <div class="space-y-4">
              <div>
                <p class="text-sm font-medium text-gray-700">Nosaukums:</p>
                <p class="text-sm text-gray-900">{{ detailsData.nosaukums }}</p>
              </div>
              <div>
                <p class="text-sm font-medium text-gray-700">Daudzums:</p>
                <p class="text-sm text-gray-900">{{ detailsData.daudzums }}</p>
              </div>
              <div>
                <p class="text-sm font-medium text-gray-700">Statuss:</p>
                <p class="text-sm text-gray-900">{{ detailsData.status }}</p>
              </div>
              <div v-if="detailsData.materials && detailsData.materials.length">
                <p class="text-sm font-medium text-gray-700">Materiāli:</p>
                <ul class="mt-2 space-y-2">
                  <li v-for="material in detailsData.materials" :key="material.id" class="text-sm text-gray-900">
                    {{ material.nosaukums }} - {{ material.daudzums }} {{ material.vieniba }}
                  </li>
                </ul>
              </div>
            </div>
            <div class="mt-6 flex justify-end">
              <button 
                type="button" 
                @click="closeDialog" 
                class="px-5 py-2.5 rounded-lg text-gray-600 border border-gray-300 hover:bg-gray-100 transition-colors"
              >
                Aizvērt
              </button>
            </div>
          </div>
        </div>

        <!-- Change Employee Password Dialog -->
        <div v-if="showPasswordChange" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50">
          <div class="bg-white p-6 rounded-xl shadow-2xl w-full max-w-md transform transition-all duration-300 scale-100">
            <h2 class="text-xl font-semibold mb-6 text-gray-800">Mainīt darbinieka paroli</h2>
            <form @submit.prevent="saveEmployeePasswordChange">
              <div class="mb-4">
                <label class="block text-sm font-medium text-gray-700 mb-1">Jaunā parole</label>
                <input
                  v-model="formData.password"
                  type="password"
                  class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                  placeholder="Jaunā parole"
                />
              </div>
              <div class="mb-4">
                <label class="block text-sm font-medium text-gray-700 mb-1">Apstiprināt paroli</label>
                <input
                  v-model="formData.confirmPassword"
                  type="password"
                  class="p-3 w-full border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 focus:border-gray-500 outline-none transition-colors"
                  placeholder="Apstiprināt paroli"
                />
              </div>
              <div class="mt-6 flex justify-end space-x-3">
                <button type="button" @click="closeDialog" class="px-5 py-2.5 rounded-lg text-gray-600 border border-gray-300 hover:bg-gray-100 transition-colors">Atcelt</button>
                <button type="submit" class="px-5 py-2.5 rounded-lg bg-gray-800 text-white hover:bg-gray-900 transition-colors">Saglabāt</button>
              </div>
            </form>
          </div>
        </div>
        
        <!-- Main content views -->
        <div v-if="currentTab === 'orders'">
          <h1 class="text-3xl font-bold text-gray-900 mb-6">Pasūtījumi</h1>
          <div class="flex justify-between items-center mb-6">
            <input 
              v-model="searchQuery" 
              type="text" 
              placeholder="Meklēt pasūtījumus..."
              class="max-w-xs w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 outline-none transition-colors"
            />
            <div class="flex gap-4">
              <select 
                v-model="sortBy"
                class="p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 outline-none transition-colors"
              >
                <option value="nosaukums">Nosaukums</option>
                <option value="daudzums">Daudzums</option>
                <option value="status">Statuss</option>
                <option value="responsible_employee">Atbildīgais</option>
              </select>
              <button 
                @click="sortDirection = sortDirection === 'asc' ? 'desc' : 'asc'"
                class="p-3 border border-gray-300 rounded-lg hover:bg-gray-100 transition-colors flex items-center justify-center w-12"
              >
                <i class="fas" :class="sortDirection === 'asc' ? 'fa-sort-up' : 'fa-sort-down'"></i>
              </button>
            </div>
            <button 
              @click="openAddDialog('orders')" 
              class="px-5 py-2.5 rounded-lg bg-gray-800 text-white hover:bg-gray-900 transition-colors flex items-center gap-2"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
              Pievienot pasūtījumu
            </button>
          </div>
          <div class="bg-white p-6 rounded-xl shadow-sm">
            <div class="overflow-x-auto">
              <table class="w-full text-left table-auto">
                <thead class="bg-gray-50 text-gray-600 uppercase text-sm">
                  <tr>
                    <th class="px-4 py-3">Nosaukums</th>
                    <th class="px-4 py-3">Daudzums</th>
                    <th class="px-4 py-3">Statuss</th>
                    <th class="px-4 py-3">Darbības</th>
                  </tr>
                </thead>
                <tbody class="text-gray-800 text-sm divide-y divide-gray-100">
                  <tr v-for="order in filteredOrders" :key="order.id" class="hover:bg-gray-50">
                    <td class="px-4 py-3">{{ order.nosaukums }}</td>
                    <td class="px-4 py-3">{{ order.daudzums }}</td>
                    <td class="px-4 py-3">
                      <span 
                        :class="{
                          'px-2 py-1 rounded-full text-xs font-semibold': true,
                          'bg-blue-100 text-blue-800': order.status === 'Nav sākts',
                          'bg-yellow-100 text-yellow-800': order.status === 'Pieņemts',
                          'bg-green-100 text-green-800': order.status === 'Pabeigts',
                          'bg-red-100 text-red-800': order.status === 'Atcelts'
                        }"
                      >
                        {{ order.status }}
                      </span>
                    </td>
                    <td class="px-4 py-3 space-x-2">
                      <button @click="openDetailsDialog(order)" class="text-gray-600 hover:text-gray-900 transition-colors">
                        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M2 12s5-10 10-10 10 10 10 10-5 10-10 10-10-10-10-10z"></path><circle cx="12" cy="12" r="3"></circle></svg>
                      </button>
                      <button @click="openEditDialog(order, 'orders')" class="text-blue-600 hover:text-blue-800 transition-colors">
                        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"></path><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"></path></svg>
                      </button>
                      <button @click="deleteOrder(order.id, order.status, order.materials)" class="text-red-600 hover:text-red-800 transition-colors">
                        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M3 6h18"></path><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"></path><path d="M10 11v6"></path><path d="M14 11v6"></path><path d="M5 6l1 16h12l1-16"></path></svg>
                      </button>
                    </td>
                  </tr>
                  <tr v-if="filteredOrders.length === 0">
                    <td colspan="5" class="px-4 py-3 text-center text-gray-500">Nav pasūtījumu</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>

        <div v-else-if="currentTab === 'materials'">
          <h1 class="text-3xl font-bold text-gray-900 mb-6">Materiāli</h1>
          <div class="flex justify-between items-center mb-6">
            <input 
              v-model="searchQuery" 
              type="text" 
              placeholder="Meklēt materiālus..."
              class="max-w-xs w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 outline-none transition-colors"
            />
            <div class="flex gap-4">
              <select 
                v-model="sortBy"
                class="p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 outline-none transition-colors"
              >
                <option value="nosaukums">Nosaukums</option>
                <option value="daudzums">Daudzums</option>
                <option value="vieniba">Vienība</option>
                <option value="noliktava">Noliktava</option>
                <option value="vieta">Vieta</option>
                <option value="reserved_quantity">Rezervēts</option>
              </select>
              <button 
                @click="sortDirection = sortDirection === 'asc' ? 'desc' : 'asc'"
                class="p-3 border border-gray-300 rounded-lg hover:bg-gray-100 transition-colors flex items-center justify-center w-12"
              >
                <i class="fas" :class="sortDirection === 'asc' ? 'fa-sort-up' : 'fa-sort-down'"></i>
              </button>
            </div>
            <button 
              @click="openAddDialog('materials')" 
              class="px-5 py-2.5 rounded-lg bg-gray-800 text-white hover:bg-gray-900 transition-colors flex items-center gap-2"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
              Pievienot materiālu
            </button>
          </div>
          <div class="bg-white p-6 rounded-xl shadow-sm">
            <div class="overflow-x-auto">
              <table class="w-full text-left table-auto">
                <thead class="bg-gray-50 text-gray-600 uppercase text-sm">
                  <tr>
                    <th class="px-4 py-3">Nosaukums</th>
                    <th class="px-4 py-3">Daudzums</th>
                    <th class="px-4 py-3">Vienība</th>
                    <th class="px-4 py-3">Noliktava</th>
                    <th class="px-4 py-3">Vieta</th>
                    <th class="px-4 py-3">Rezervēts</th>
                    <th class="px-4 py-3">Darbības</th>
                  </tr>
                </thead>
                <tbody class="text-gray-800 text-sm divide-y divide-gray-100">
                  <tr v-for="material in filteredMaterials" :key="material.id" class="hover:bg-gray-50">
                    <td class="px-4 py-3">{{ material.nosaukums }}</td>
                    <td class="px-4 py-3">{{ material.daudzums }}</td>
                    <td class="px-4 py-3">{{ material.vieniba }}</td>
                    <td class="px-4 py-3">{{ material.noliktava }}</td>
                    <td class="px-4 py-3">{{ material.vieta }}</td>
                    <td class="px-4 py-3">{{ material.reserved_quantity || 0 }}</td>
                    <td class="px-4 py-3 space-x-2">
                      <button @click="openEditDialog(material, 'materials')" class="text-blue-600 hover:text-blue-800 transition-colors">
                        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"></path><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"></path></svg>
                      </button>
                      <button @click="deleteMaterial(material.id)" class="text-red-600 hover:text-red-800 transition-colors">
                        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M3 6h18"></path><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"></path><path d="M10 11v6"></path><path d="M14 11v6"></path><path d="M5 6l1 16h12l1-16"></path></svg>
                      </button>
                      <button @click="openTransferDialog(material)" class="text-purple-600 hover:text-purple-800 transition-colors">
                        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><polyline points="17 1 21 5 17 9"></polyline><path d="M3 11V9a4 4 0 0 1 4-4h14"></path><polyline points="7 23 3 19 7 15"></polyline><path d="M21 13v2a4 4 0 0 1-4 4H3"></path></svg>
                      </button>
                    </td>
                  </tr>
                  <tr v-if="filteredMaterials.length === 0">
                    <td colspan="8" class="px-4 py-3 text-center text-gray-500">Nav materiālu</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>

        <div v-else-if="currentTab === 'workers'">
          <h1 class="text-3xl font-bold text-gray-900 mb-6">Darbinieki</h1>
          <div class="flex justify-between items-center mb-6">
            <input 
              v-model="searchQuery" 
              type="text" 
              placeholder="Meklēt darbiniekus..."
              class="max-w-xs w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 outline-none transition-colors"
            />
            <div class="flex gap-4">
              <select 
                v-model="sortBy"
                class="p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 outline-none transition-colors"
              >
                <option value="vards">Vārds</option>
                <option value="uzvards">Uzvārds</option>
                <option value="kods">Kods</option>
                <option value="amats">Amats</option>
                <option value="status">Statuss</option>
              </select>
              <button 
                @click="sortDirection = sortDirection === 'asc' ? 'desc' : 'asc'"
                class="p-3 border border-gray-300 rounded-lg hover:bg-gray-100 transition-colors flex items-center justify-center w-12"
              >
                <i class="fas" :class="sortDirection === 'asc' ? 'fa-sort-up' : 'fa-sort-down'"></i>
              </button>
            </div>
            <button 
              @click="openAddDialog('employees')" 
              class="px-5 py-2.5 rounded-lg bg-gray-800 text-white hover:bg-gray-900 transition-colors flex items-center gap-2"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
              Pievienot darbinieku
            </button>
          </div>
          <div class="bg-white p-6 rounded-xl shadow-sm">
            <div class="overflow-x-auto">
              <table class="w-full text-left table-auto">
                <thead class="bg-gray-50 text-gray-600 uppercase text-sm">
                  <tr>
                    <th class="px-4 py-3">Vārds</th>
                    <th class="px-4 py-3">Uzvārds</th>
                    <th class="px-4 py-3">Kods</th>
                    <th class="px-4 py-3">Amats</th>
                    <th class="px-4 py-3">Statuss</th>
                    <th class="px-4 py-3">Darbības</th>
                  </tr>
                </thead>
                <tbody class="text-gray-800 text-sm divide-y divide-gray-100">
                  <tr v-for="employee in filteredEmployees" :key="employee.id" class="hover:bg-gray-50">
                    <td class="px-4 py-3 font-medium">{{ employee.vards }}</td>
                    <td class="px-4 py-3">{{ employee.uzvards }}</td>
                    <td class="px-4 py-3">{{ employee.kods }}</td>
                    <td class="px-4 py-3">{{ employee.amats }}</td>
                    <td class="px-4 py-3">{{ employee.status }}</td>
                    <td class="px-4 py-3 space-x-2">
                      <button @click="openEditDialog(employee, 'employees')" class="text-blue-600 hover:text-blue-800 transition-colors">
                        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"></path><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"></path></svg>
                      </button>
                      <button @click="deleteEmployee(employee.id)" class="text-red-600 hover:text-red-800 transition-colors">
                        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M3 6h18"></path><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"></path><path d="M10 11v6"></path><path d="M14 11v6"></path><path d="M5 6l1 16h12l1-16"></path></svg>
                      </button>
                    </td>
                  </tr>
                  <tr v-if="filteredEmployees.length === 0">
                    <td colspan="6" class="px-4 py-3 text-center text-gray-500">Nav darbinieku</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>

        <div v-else-if="currentTab === 'shifts'">
          <h1 class="text-3xl font-bold text-gray-900 mb-6">Maiņu statistika</h1>
          <div class="flex justify-between items-center mb-6">
            <input 
              v-model="searchQuery" 
              type="text" 
              placeholder="Meklēt maiņas..."
              class="max-w-xs w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 outline-none transition-colors"
            />
            <div class="flex gap-4">
              <select 
                v-model="shiftSortKey"
                class="p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 outline-none transition-colors"
              >
                <option value="worker_name">Darbinieks</option>
                <option value="start_time">Sākums</option>
                <option value="duration">Ilgums</option>
              </select>
              <button 
                @click="shiftSortAsc = !shiftSortAsc"
                class="p-3 border border-gray-300 rounded-lg hover:bg-gray-100 transition-colors flex items-center justify-center w-12"
              >
                <i class="fas" :class="shiftSortAsc ? 'fa-sort-up' : 'fa-sort-down'"></i>
              </button>
            </div>
            <div class="flex gap-4">
              <input 
                type="date"
                v-model="dateRange.start"
                class="p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 outline-none transition-colors"
              />
              <input 
                type="date"
                v-model="dateRange.end"
                class="p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-gray-500 outline-none transition-colors"
              />
            </div>
          </div>
          <div class="bg-white p-6 rounded-xl shadow-sm">
            <div class="overflow-x-auto">
              <table class="w-full text-left table-auto">
                <thead class="bg-gray-50 text-gray-600 uppercase text-sm">
                  <tr>
                    <th class="px-4 py-3">Darbinieks</th>
                    <th class="px-4 py-3">Sākums</th>
                    <th class="px-4 py-3">Ilgums</th>
                  </tr>
                </thead>
                <tbody class="text-gray-800 text-sm divide-y divide-gray-100">
                  <tr v-for="shift in filteredShiftsByDate" :key="shift.id" class="hover:bg-gray-50">
                    <td class="px-4 py-3">{{ shift.employee ? `${shift.employee.vards} ${shift.employee.uzvards}` : 'Nav zināms' }}</td>
                    <td class="px-4 py-3">{{ shift.start_time ? new Date(shift.start_time).toLocaleDateString('lv-LV') : 'Nav sākts' }}</td>
                    <td class="px-4 py-3">{{ shift.end_time ? calculateShiftDuration(shift.start_time, shift.end_time) : 'Nav beidzts' }}</td>
                  </tr>
                  <tr v-if="filteredShiftsByDate.length === 0">
                    <td colspan="3" class="px-4 py-3 text-center text-gray-500">Nav maiņu</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>

        <div v-else-if="currentTab === 'stats'">
          <div class="bg-white rounded-lg shadow-md p-4 mb-4" v-if="currentTab === 'stats'">
            <h2 class="text-xl font-semibold mb-4">Vispārējā statistika</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
              <div class="bg-gray-100 p-3 rounded-md text-center">
                <p class="text-sm text-gray-600">Maiņu skaits</p>
                <p class="text-2xl font-bold text-gray-800">{{ stats.total_shifts }}</p>
              </div>
              <div class="bg-gray-100 p-3 rounded-md text-center">
                <p class="text-sm text-gray-600">Materiālu skaits</p>
                <p class="text-2xl font-bold text-gray-800">{{ stats.total_materials }}</p>
              </div>
              <div class="bg-gray-100 p-3 rounded-md text-center">
                <p class="text-sm text-gray-600">Darbinieku skaits</p>
                <p class="text-2xl font-bold text-gray-800">{{ stats.total_workers }}</p>
              </div>
              <div class="bg-gray-100 p-3 rounded-md text-center">
                <p class="text-sm text-gray-600">Pasūtījumu skaits</p>
                <p class="text-2xl font-bold text-gray-800">{{ stats.total_orders }}</p>
              </div>
            </div>
          </div>

          <div class="bg-white rounded-lg shadow-md p-4" v-if="currentTab === 'stats'">
            <h2 class="text-xl font-semibold mb-4">Top 5 visvairāk izmantotie materiāli</h2>
            <div class="overflow-x-auto">
              <table class="min-w-full divide-y divide-gray-200">
                <thead class="bg-gray-50">
                  <tr>
                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Materiāls</th>
                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Izmantots</th>
                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Vienība</th>
                  </tr>
                </thead>
                <tbody class="bg-white divide-y divide-gray-200">
                  <tr v-for="(material, index) in topMaterialStats" :key="index">
                    <td class="px-6 py-4 whitespace-nowrap">{{ material.nosaukums }}</td>
                    <td class="px-6 py-4 whitespace-nowrap">{{ material.totalUsed }}</td>
                    <td class="px-6 py-4 whitespace-nowrap">{{ material.vieniba }}</td>
                  </tr>
                  <tr v-if="topMaterialStats.length === 0">
                    <td colspan="3" class="px-6 py-4 whitespace-nowrap text-center text-gray-500">Nav datu par materiālu patēriņu.</td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div class="mt-4">
              <p class="text-lg font-semibold">Kopējais izmantoto materiālu daudzums: {{ totalMaterialUsage }}</p>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>

  <!-- Delete Confirmation Dialog -->
  <div v-if="showDeleteConfirmDialog" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50">
    <div class="bg-white p-6 rounded-xl shadow-2xl w-full max-w-md transform transition-all duration-300 scale-100">
      <div class="text-center">
        <div class="mx-auto flex items-center justify-center h-12 w-12 rounded-full bg-red-100 mb-4">
          <svg class="h-6 w-6 text-red-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
          </svg>
        </div>
        <h3 class="text-lg font-medium text-gray-900 mb-2">
          {{ deleteType === 'orders' ? 'Vai tiešām vēlaties dzēst šo pasūtījumu?' :
             deleteType === 'materials' ? 'Vai tiešām vēlaties dzēst šo materiālu?' :
             'Vai tiešām vēlaties dzēst šo darbinieku?' }}
        </h3>
        <p class="text-sm text-gray-500 mb-4">
          {{ deleteType === 'orders' ? itemToDelete?.nosaukums :
             deleteType === 'materials' ? itemToDelete?.nosaukums :
             `${itemToDelete?.vards} ${itemToDelete?.uzvards}` }}
        </p>
        <div class="flex justify-center space-x-4">
          <button
            @click="confirmDelete"
            class="px-4 py-2 bg-red-600 text-white rounded-lg hover:bg-red-700 transition-colors"
          >
            Dzēst
          </button>
          <button
            @click="showDeleteConfirmDialog = false"
            class="px-4 py-2 bg-gray-200 text-gray-800 rounded-lg hover:bg-gray-300 transition-colors"
          >
            Atcelt
          </button>
        </div>
      </div>
    </div>
  </div>

  <OrderDetailsModal
    :show="showOrderDetails"
    :order="selectedOrder"
    @close="showOrderDetails = false"
  />
</template>

<script setup>
import { ref, computed, onMounted, watch, h, onUnmounted } from 'vue';
import { useToast } from 'vue-toastification';
import axios from 'axios';
import OrderDetailsModal from '../components/OrderDetailsModal.vue'
import { jsPDF } from 'jspdf';
import 'jspdf-autotable';

const toast = useToast();

// Initialize PDF libraries
const pdfLib = ref(jsPDF);
const isPdfLibsReady = ref(true);

// --- State variables ---
const currentTab = ref('orders');
const selectedCategory = ref('orders');

// Core data arrays
const orders = ref([]);
const materials = ref([]);
const employees = ref([]);
const shifts = ref([]);

// Stats object - initialized with default empty arrays for nested data
const stats = ref({
  shifts: [],
  material_usage_data: [],
  total_materials: 0,
  total_workers: 0,
  total_orders: 0,
  total_shifts: 0
});

// Counts/summaries
const totalMaterials = ref(0);
const totalOrders = ref(0);
const totalEmployees = ref(0);
const totalShifts = ref(0);
const totalAvailableMaterials = ref(0);
const totalUsedMaterials = ref(0); // This was previously present, re-adding.
const completedOrdersCount = ref(0);

// Dialog states
const showAddDialog = ref(false);
const showEditDialog = ref(false);
const showTransferDialog = ref(false);
const showDetailsDialog = ref(false);
const showMaterialSearch = ref(false); // Likely for a search dropdown in forms
const showChart = ref(false); // If there's a chart toggle
const showPasswordChange = ref(false);

const currentEditType = ref(''); // 'orders', 'materials', 'employees'
const editData = ref(null); // Data for the item being edited
const detailsData = ref({}); // Data for item details view
const orderMaterials = ref([]); // Materials specifically attached to an order form (new/edit)

const transferData = ref({ // Data for material transfer form
  material_id: '',
  nosaukums: '',
  daudzums: 0,
  max: 0,
  fromNoliktava: '',
  vieniba: ''
});

// General form data object, structured for different entities
const formData = ref({
  nosaukums: '',
  daudzums: 0,
  vieniba: 'kg',
  noliktava: 'Centrālā noliktava',
  vieta: '',
  password: '',
  kods: '',
  status: 'Aktīvs',
  vards: '',
  uzvards: '',
  amats: '',
  epasts: '',
  telefons: '',
  role: 'employee'
});

// Search and sort states
const searchQuery = ref('');
const sortBy = ref('nosaukums'); // Default sort field
const sortDirection = ref('asc'); // Default sort direction
const materialSearch = ref(''); // For searching materials within order form

// Specific refs for shift filtering and sorting
const dateRange = ref({ start: null, end: null });
const shiftSortKey = ref('worker_name'); // Default sort field for shifts
const shiftSortAsc = ref(true); // Default sort direction for shifts

// PDF library status
const isPdfLibsLoading = ref(false);

// Material quantity validation (if used for specific input element ref)
const materialQuantityInput = ref(null); 

// Add confirmation dialog state
const showDeleteConfirmDialog = ref(false);
const itemToDelete = ref(null);
const deleteType = ref(''); // 'orders', 'materials', 'employees'

// Computed properties for filtering and sorting orders
const filteredOrders = computed(() => {
  let filtered = orders.value;
  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase();
    filtered = filtered.filter(order =>
      order.nosaukums.toLowerCase().includes(query) ||
      (order.employee && `${order.employee.vards} ${order.employee.uzvards}`.toLowerCase().includes(query)) ||
      order.status.toLowerCase().includes(query)
    );
  }
  if (sortBy.value) {
    filtered.sort((a, b) => {
      const aValue = a[sortBy.value];
      const bValue = b[sortBy.value];
      if (typeof aValue === 'string' && typeof bValue === 'string') {
        return sortDirection.value === 'asc' ? aValue.localeCompare(bValue) : bValue.localeCompare(aValue);
      }
      return sortDirection.value === 'asc' ? aValue - bValue : bValue - aValue;
    });
  }
  return filtered;
});

// Computed properties for filtering and sorting materials
const filteredMaterials = computed(() => {
  let filtered = materials.value;
  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase();
    filtered = filtered.filter(material =>
      material.nosaukums.toLowerCase().includes(query) ||
      material.noliktava.toLowerCase().includes(query) ||
      material.vieniba.toLowerCase().includes(query)
    );
  }
  if (sortBy.value) {
    filtered.sort((a, b) => {
      const aValue = a[sortBy.value];
      const bValue = b[sortBy.value];
      if (typeof aValue === 'string' && typeof bValue === 'string') {
        return sortDirection.value === 'asc' ? aValue.localeCompare(bValue) : bValue.localeCompare(aValue);
      }
      return sortDirection.value === 'asc' ? aValue - bValue : bValue - aValue;
    });
  }
  return filtered;
});

// Computed properties for filtering and sorting employees
const filteredEmployees = computed(() => {
  let filtered = employees.value;
  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase();
    filtered = filtered.filter(employee =>
      employee.vards.toLowerCase().includes(query) ||
      employee.uzvards.toLowerCase().includes(query) ||
      employee.amats.toLowerCase().includes(query) ||
      employee.kods.toLowerCase().includes(query) ||
      (employee.status || '').toLowerCase().includes(query)
    );
  }
  if (sortBy.value) {
    filtered.sort((a, b) => {
      const aValue = a[sortBy.value];
      const bValue = b[sortBy.value];
      if (typeof aValue === 'string' && typeof bValue === 'string') {
        return sortDirection.value === 'asc' ? aValue.localeCompare(bValue) : bValue.localeCompare(aValue);
      }
      return sortDirection.value === 'asc' ? aValue - bValue : bValue - aValue;
    });
  }
  return filtered;
});

// Computed property for filtering and sorting shifts by date and search query
const filteredShiftsByDate = computed(() => {
  if (!shifts.value || !Array.isArray(shifts.value)) {
    console.log('No shifts data available:', shifts.value);
    return [];
  }
  
  let filtered = [...shifts.value];
  console.log('Initial shifts:', filtered);
  
  // Apply date filter
  if (dateRange.value?.start && dateRange.value?.end) {
    const startDate = new Date(dateRange.value.start);
    const endDate = new Date(dateRange.value.end);
    endDate.setHours(23, 59, 59, 999);
    
    filtered = filtered.filter(shift => {
      if (!shift.start_time) return false;
      const shiftDate = new Date(shift.start_time);
      return shiftDate >= startDate && shiftDate <= endDate;
    });
    console.log('After date filter:', filtered);
  }
  
  // Apply search filter
  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase();
    filtered = filtered.filter(shift => {
      const workerName = (shift.employee?.vards || '') + ' ' + (shift.employee?.uzvards || '');
      return workerName.toLowerCase().includes(query);
    });
    console.log('After search filter:', filtered);
  }
  
  // Apply sorting
  if (shiftSortKey.value) {
    filtered.sort((a, b) => {
      let aValue = a[shiftSortKey.value];
      let bValue = b[shiftSortKey.value];
      
      if (shiftSortKey.value === 'start_time' || shiftSortKey.value === 'end_time') {
        aValue = new Date(a[shiftSortKey.value] || 0).getTime();
        bValue = new Date(b[shiftSortKey.value] || 0).getTime();
        return shiftSortAsc.value ? aValue - bValue : bValue - aValue;
      }
      
      if (shiftSortKey.value === 'worker_name') {
        aValue = (a.employee?.vards || '') + ' ' + (a.employee?.uzvards || '');
        bValue = (b.employee?.vards || '') + ' ' + (b.employee?.uzvards || '');
        return shiftSortAsc.value ? aValue.localeCompare(bValue) : bValue.localeCompare(aValue);
      }

      if (typeof aValue === 'string' && typeof bValue === 'string') {
        return shiftSortAsc.value 
          ? aValue.localeCompare(bValue)
          : bValue.localeCompare(aValue);
      }
      
      return shiftSortAsc.value 
        ? (aValue || 0) - (bValue || 0)
        : (bValue || 0) - (aValue || 0);
    });
    console.log('After sorting:', filtered);
  }
  
  return filtered;
});

// Computed properties for material statistics
const materialStatsSearchQuery = ref("");

const filteredMaterialStats = computed(() => {
  const query = materialStatsSearchQuery.value.toLowerCase();
  return (stats.value.material_usage_data || []).filter(stat =>
    (stat.nosaukums || "").toLowerCase().includes(query)
  );
});

const totalMaterialUsage = computed(() => {
  const total = filteredMaterialStats.value.reduce((sum, stat) => sum + (stat.totalUsed || 0), 0);
  return Number(total).toLocaleString('lv-LV', {
    minimumFractionDigits: 2,
    maximumFractionDigits: 2
  });
});

const maxMaterialUsage = computed(() => {
  return Math.max(...filteredMaterialStats.value.map(stat => (stat.totalUsed || 0)), 0);
});

const mostUsedMaterial = computed(() => {
  const material = filteredMaterialStats.value.reduce((prev, current) => (
    (prev.totalUsed || 0) > (current.totalUsed || 0) ? prev : current
  ), {});
  
  return {
    ...material,
    totalUsed: material.totalUsed ? Number(material.totalUsed).toLocaleString('lv-LV', {
      minimumFractionDigits: 2,
      maximumFractionDigits: 2
    }) : '0'
  };
});

const topMaterialStats = computed(() => {
  return filteredMaterialStats.value
    .sort((a, b) => (b.totalUsed || 0) - (a.totalUsed || 0))
    .slice(0, 5)
    .map(stat => ({
      ...stat,
      totalUsed: Number(stat.totalUsed).toLocaleString('lv-LV', {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      })
    }));
});

// Chart colors (ensure unique values for proper display)
const chartColors = ['#4A5568', '#2D3748', '#718096', '#A0AEC0', '#CBD5E0'];

const getChartColor = (index) => {
  return chartColors[index % chartColors.length];
};

const getRanking = (stat) => {
  const sortedStats = [...filteredMaterialStats.value].sort((a, b) => (b.totalUsed || 0) - (a.totalUsed || 0));
  return sortedStats.findIndex(s => s.id === stat.id) + 1;
};

// Fetch data functions
const fetchOrders = async () => {
  try {
    const response = await axios.get('https://backend-psi-blush-35.vercel.app/orders', {
      headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
    });
    orders.value = response.data;
    completedOrdersCount.value = response.data.filter(order => order.status === 'Pabeigts').length;
  } catch (error) {
    handleError(error, 'Neizdevās ielādēt pasūtījumus.');
  }
};

const fetchMaterials = async () => {
  try {
    const response = await axios.get('https://backend-psi-blush-35.vercel.app/materials', {
      headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
    });
    materials.value = response.data;
    totalMaterials.value = response.data.length;
    totalAvailableMaterials.value = materials.value.filter(m => m.daudzums > 0).length;
  } catch (error) {
    handleError(error, 'Neizdevās ielādēt materiālus.');
  }
};

const fetchEmployees = async () => {
  try {
    const response = await axios.get('https://backend-psi-blush-35.vercel.app/employees', {
      headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
    });
    employees.value = response.data;
    totalEmployees.value = response.data.length;
  } catch (error) {
    handleError(error, 'Neizdevās ielādēt darbiniekus.');
  }
};

const fetchShifts = async () => {
  try {
    const response = await axios.get('https://backend-psi-blush-35.vercel.app/api/shifts', {
      headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
    });
    shifts.value = response.data;
    totalShifts.value = response.data.length;
  } catch (error) {
    handleError(error, 'Neizdevās ielādēt maiņas.');
  }
};

const fetchStats = async () => {
  try {
    const response = await axios.get('https://backend-psi-blush-35.vercel.app/api/stats', {
      headers: { 
        'Authorization': `Bearer ${localStorage.getItem('token')}`,
        'Content-Type': 'application/json'
      }
    });
    
    if (response.data) {
      // Update stats with the response data
      stats.value = {
        shifts: response.data.shifts || [],
        material_usage_data: response.data.material_usage_data || [],
        total_materials: response.data.total_materials || 0,
        total_workers: response.data.total_workers || 0,
        total_orders: response.data.total_orders || 0,
        total_shifts: response.data.total_shifts || 0
      };

      // Update other related stats
      totalMaterials.value = stats.value.total_materials;
      totalOrders.value = stats.value.total_orders;
      totalEmployees.value = stats.value.total_workers;
      totalShifts.value = stats.value.total_shifts;
      totalAvailableMaterials.value = stats.value.total_materials_quantity;
      
      // Calculate completed orders count
      completedOrdersCount.value = orders.value.filter(order => order.status === 'Pabeigts').length;
    }
  } catch (error) {
    console.error('Error loading stats:', error);
    toast.error('Neizdevās ielādēt statistiku');
    
    // Reset stats to default values
    stats.value = {
      shifts: [],
      material_usage_data: [],
      total_materials: 0,
      total_workers: 0,
      total_orders: 0,
      total_shifts: 0
    };
  }
};

// Function to load PDF libraries and fonts
const loadPdfLibs = async () => {
  isPdfLibsLoading.value = true;
  try {
    // Use jsPDF's built-in font support
    const doc = new jsPDF();
    doc.setFont('helvetica');
    doc.setFontSize(10);
    
    isPdfLibsReady.value = true;
    toast.success('PDF bibliotēkas ielādētas');
  } catch (error) {
    console.error('Error loading PDF libraries or fonts:', error);
    toast.error('Neizdevās ielādēt PDF bibliotēkas vai fontus');
    isPdfLibsReady.value = false;
  } finally {
    isPdfLibsLoading.value = false;
  }
};

// Initial data fetch on component mount
onMounted(() => {
  fetchOrders();
  fetchMaterials();
  fetchEmployees();
  fetchShifts();
  fetchStats();
  loadPdfLibs(); // Load PDF libs when component mounts
});

// Watch for changes in currentTab to re-fetch relevant data or update counts
watch(currentTab, (newTab) => {
  if (newTab === 'orders') {
    fetchOrders();
  } else if (newTab === 'materials') {
    fetchMaterials();
  } else if (newTab === 'workers') {
    fetchEmployees();
  } else if (newTab === 'shifts') {
    fetchShifts();
  } else if (newTab === 'stats') {
    fetchStats();
  }
  searchQuery.value = ''; // Clear search query on tab change
});

// Helper function to handle API errors and show toasts
const handleError = (error, message) => {
  console.error(message, error);
  toast.error(message);
};

// --- Dialog and Form Management ---

// Warehouse options for material forms
const warehouseOptions = ['Centrālā noliktava', 'Balta noliktava'];

// Computed properties for current form data (add/edit)
const currentFormData = computed(() => {
  return showEditDialog.value ? editData.value : formData.value;
});

const currentMaterialFormData = computed(() => {
  return showEditDialog.value ? editData.value : formData.value;
});

// Function to open add dialog
const openAddDialog = (type) => {
  currentEditType.value = type;
  showAddDialog.value = true;
  // Reset form data based on type
  if (type === 'orders') {
    formData.value = { nosaukums: '', daudzums: 0, status: 'Nav sākts' };
    orderMaterials.value = [];
  } else if (type === 'materials') {
    formData.value = { nosaukums: '', daudzums: 0, vieniba: 'kg', noliktava: 'Centrālā noliktava', vieta: '' };
  } else if (type === 'employees') {
    formData.value = { vards: '', uzvards: '', kods: '', amats: '', status: 'Aktīvs', epasts: '', telefons: '', role: 'employee', password: '' };
  }
};

// Function to open edit dialog
const openEditDialog = (item, type) => {
  currentEditType.value = type;
  editData.value = item;
  showEditDialog.value = true;

  if (type === 'orders') {
    // Load data into formData for editing
    formData.value = {
      id: item.id,
      nosaukums: item.nosaukums,
      daudzums: item.daudzums,
      status: item.status
    };
    // Deep copy materials for editing and convert to the format expected by the form
    orderMaterials.value = item.materials ? item.materials.map(mat => ({
      material_id: mat.material_id,
      quantity: mat.daudzums
    })) : [];
  } else if (type === 'materials') {
    // Load data into formData for editing
    formData.value = {
      id: item.id,
      nosaukums: item.nosaukums,
      daudzums: item.daudzums,
      vieniba: item.vieniba || 'kg',
      noliktava: item.noliktava || 'Centrālā noliktava',
      vieta: item.vieta || ''
    };
  } else if (type === 'employees') {
    // Load data into formData for editing
    formData.value = {
      id: item.id,
      vards: item.vards,
      uzvards: item.uzvards,
      kods: item.kods,
      amats: item.amats,
      status: item.status,
      epasts: item.epasts || '',
      telefons: item.telefons || '',
      role: item.role || 'employee'
    };
  }
};

// Function to close dialogs
const closeDialog = () => {
  showAddDialog.value = false;
  showEditDialog.value = false;
  showTransferDialog.value = false;
  showDetailsDialog.value = false;
  showPasswordChange.value = false;
  editData.value = null; // Clear edit data
  detailsData.value = {}; // Clear details data
  orderMaterials.value = []; // Clear materials from order form
  // Reset formData for all types
  formData.value = {
    nosaukums: '',
    daudzums: 0,
    vieniba: 'kg',
    noliktava: 'Centrālā noliktava',
    vieta: '',
    password: '',
    kods: '',
    status: 'Aktīvs',
    vards: '',
    uzvards: '',
    amats: '',
    epasts: '',
    telefons: '',
    role: 'employee'
  };
};

// --- Order Management ---

const addOrder = async () => {
  if (!formData.value.nosaukums || !formData.value.daudzums || orderMaterials.value.length === 0) {
    toast.error('Lūdzu aizpildiet visus obligātos laukus un pievienojiet materiālus.');
    return;
  }

  const orderData = {
    nosaukums: formData.value.nosaukums,
    daudzums: formData.value.daudzums,
    status: formData.value.status,
    materials: orderMaterials.value.map(mat => ({
      material_id: mat.material_id || mat.id,
      daudzums: mat.quantity
    }))
  };

  try {
    const response = await axios.post('https://backend-psi-blush-35.vercel.app/orders', orderData, {
      headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
    });
    orders.value.push(response.data);
    closeDialog();
    toast.success('Pasūtījums veiksmīgi pievienots!');
    fetchMaterials();
    fetchStats();
  } catch (error) {
    handleError(error, 'Neizdevās pievienot pasūtījumu.');
    if (error.response && error.response.data && error.response.data.message) {
      toast.error(error.response.data.message);
    }
  }
};

const saveEdit = async () => {
  if (currentEditType.value === 'orders') {
    if (!formData.value.nosaukums || !formData.value.daudzums || orderMaterials.value.length === 0) {
      toast.error('Lūdzu aizpildiet visus obligātos laukus un pievienojiet materiālus.');
      return;
    }

    try {
      const originalOrderResponse = await axios.get(`https://backend-psi-blush-35.vercel.app/orders/${editData.value.id}`, {
        headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
      });
      const originalOrderMaterials = originalOrderResponse.data.materials || [];

      // If order is not started, we need to handle material reservations
      if (formData.value.status === 'Nav sākts') {
        // First, release old reservations
        for (const origMat of originalOrderMaterials) {
          const material = materials.value.find(m => m.id === origMat.material_id);
          if (material) {
            const oldReserved = material.reserved_quantity || 0;
            const newReserved = Math.max(0, oldReserved - (origMat.daudzums * formData.value.daudzums));
            await axios.put(`https://backend-psi-blush-35.vercel.app/materials/${origMat.material_id}`, {
              ...material,
              reserved_quantity: newReserved
            }, {
              headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
            });
          }
        }

        // Then, add new reservations
        for (const newMat of orderMaterials.value) {
          const material = materials.value.find(m => m.id === (newMat.material_id || newMat.id));
          if (material) {
            const oldReserved = material.reserved_quantity || 0;
            const newReserved = oldReserved + (newMat.quantity * formData.value.daudzums);
            await axios.put(`https://backend-psi-blush-35.vercel.app/materials/${newMat.material_id || newMat.id}`, {
              ...material,
              reserved_quantity: newReserved
            }, {
              headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
            });
          }
        }
      }

      const updatedOrderData = {
        nosaukums: formData.value.nosaukums,
        daudzums: formData.value.daudzums,
        status: formData.value.status,
        materials: orderMaterials.value.map(mat => ({
          material_id: mat.material_id || mat.id,
          daudzums: mat.quantity
        }))
      };

      const response = await axios.put(`https://backend-psi-blush-35.vercel.app/api/orders/${editData.value.id}`, updatedOrderData, {
        headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
      });

      // Update the order in the list
      const index = orders.value.findIndex(o => o.id === editData.value.id);
      if (index !== -1) {
        orders.value[index] = response.data;
      }

      closeDialog();
      toast.success('Pasūtījums veiksmīgi atjaunināts!');
      fetchMaterials();
      fetchStats();
    } catch (error) {
      handleError(error, 'Neizdevās atjaunināt pasūtījumu.');
    }
  } else if (currentEditType.value === 'materials') {
    if (!formData.value.nosaukums || !formData.value.daudzums || !formData.value.vieniba || !formData.value.noliktava) {
      toast.error('Lūdzu aizpildiet visus laukus!');
      return;
    }
    try {
      const response = await axios.put(`https://backend-psi-blush-35.vercel.app/materials/${formData.value.id}`, {
        nosaukums: formData.value.nosaukums,
        daudzums: formData.value.daudzums,
        vieniba: formData.value.vieniba,
        noliktava: formData.value.noliktava,
        vieta: formData.value.vieta
      }, {
        headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
      });
      const index = materials.value.findIndex(m => m.id === formData.value.id);
      if (index !== -1) {
        materials.value[index] = response.data;
      }
      closeDialog();
      toast.success('Materiāls veiksmīgi atjaunināts!');
      fetchMaterials(); // Refresh materials after update
      fetchStats();
    } catch (error) {
      handleError(error, 'Neizdevās atjaunināt materiālu.');
      if (error.response && error.response.data && error.response.data.message) {
        toast.error(error.response.data.message);
      }
    }
  } else if (currentEditType.value === 'employees') {
    if (!formData.value.vards || !formData.value.uzvards || !formData.value.kods || !formData.value.amats) {
      toast.error('Lūdzu aizpildiet visus obligātos laukus!');
      return;
    }
    try {
      const response = await axios.put(`https://backend-psi-blush-35.vercel.app/employees/${formData.value.id}`, formData.value, {
        headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
      });
      const index = employees.value.findIndex(e => e.id === formData.value.id);
      if (index !== -1) {
        employees.value[index] = response.data;
      }
      closeDialog();
      toast.success('Darbinieks veiksmīgi atjaunināts!');
      fetchStats();
    } catch (error) {
      handleError(error, 'Neizdevās atjaunināt darbinieku.');
      if (error.response && error.response.status === 409) {
        toast.error('Darbinieks ar šādu kodu jau eksistē.');
      } else if (error.response && error.response.data && error.response.data.message) {
        toast.error(error.response.data.message);
      }
    }
  }
};

const saveEmployeePasswordChange = async () => {
  if (!formData.value.password || !formData.value.confirmPassword) {
    toast.error('Lūdzu ievadiet jauno paroli un apstiprinājumu.');
    return;
  }
  if (formData.value.password.length < 6) {
    toast.error('Parolei jābūt vismaz 6 rakstzīmes garai.');
    return;
  }
  if (formData.value.password !== formData.value.confirmPassword) {
    toast.error('Paroles nesakrīt!');
    return;
  }

  try {
    await axios.patch(`https://backend-psi-blush-35.vercel.app/employees/${editData.value.id}/password`, {
      password: formData.value.password
    }, {
      headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
    });
    closeDialog();
    toast.success('Parole veiksmīgi mainīta!');
  } catch (error) {
    handleError(error, 'Neizdevās mainīt paroli.');
  }
};

const deleteOrder = (id, orderStatus, orderMaterials) => {
  const order = orders.value.find(o => o.id === id);
  if (order) {
    itemToDelete.value = order;
    deleteType.value = 'orders';
    showDeleteConfirmDialog.value = true;
  }
};

const deleteMaterial = (id) => {
  const material = materials.value.find(m => m.id === id);
  if (material) {
    itemToDelete.value = material;
    deleteType.value = 'materials';
    showDeleteConfirmDialog.value = true;
  }
};

const deleteEmployee = (id) => {
  const employee = employees.value.find(e => e.id === id);
  if (employee) {
    itemToDelete.value = employee;
    deleteType.value = 'employees';
    showDeleteConfirmDialog.value = true;
  }
};

const addMaterialToOrder = (material) => {
  const existingMaterial = orderMaterials.value.find(m => m.material_id === material.id);
  if (existingMaterial) {
    toast.info('Šis materiāls jau ir pievienots pasūtījumam.');
    return;
  }
  orderMaterials.value.push({ material_id: material.id, quantity: 1 });
  materialSearch.value = '';
};

const addMaterialToEdit = (material) => {
  const existingMaterial = orderMaterials.value.find(m => m.material_id === material.id);
  if (existingMaterial) {
    toast.info('Šis materiāls jau ir pievienots pasūtījumam.');
    return;
  }
  orderMaterials.value.push({ material_id: material.id, quantity: 1 });
  materialSearch.value = '';
};

const getMaterialName = (id) => {
  const material = materials.value.find(m => m.id === id);
  return material ? material.nosaukums : 'Nezināms materiāls';
};

const getMaterialUnit = (id) => {
  const material = materials.value.find(m => m.id === id);
  return material ? material.vieniba : '';
};

// --- Employee Management ---

const addEmployee = async () => {
  if (!formData.value.vards || !formData.value.uzvards || !formData.value.kods || !formData.value.amats) {
    toast.error('Lūdzu aizpildiet visus obligātos laukus!');
    return;
  }

  if (formData.value.password && formData.value.password.length < 6) {
    toast.error('Parolei jābūt vismaz 6 rakstzīmes garai.');
    return;
  }

  try {
    const employeeData = { ...formData.value };
    if (!employeeData.password) {
      delete employeeData.password;
    }

    const response = await axios.post('https://backend-psi-blush-35.vercel.app/employees', employeeData, {
      headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
    });
    employees.value.push(response.data);
    closeDialog();
    toast.success('Darbinieks veiksmīgi pievienots!');
    fetchStats();
  } catch (error) {
    handleError(error, 'Neizdevās pievienot darbinieku.');
    if (error.response && error.response.status === 409) {
      toast.error('Darbinieks ar šādu kodu jau eksistē.');
    } else if (error.response && error.response.data && error.response.data.message) {
      toast.error(error.response.data.message);
    }
  }
};

// --- Material Transfer ---

const openTransferDialog = (material) => {
  transferData.value = {
    material_id: material.id,
    nosaukums: material.nosaukums,
    daudzums: 0,
    max: material.daudzums - (material.reserved_quantity || 0),
    fromNoliktava: material.noliktava,
    vieniba: material.vieniba
  };
  showTransferDialog.value = true;
};

const transferMaterial = async () => {
  if (!transferData.value.material_id || transferData.value.daudzums <= 0 || !transferData.value.toNoliktava) {
    toast.error('Lūdzu aizpildiet visus laukus pārsūtīšanai!');
    return;
  }
  if (transferData.value.daudzums > transferData.value.max) {
    toast.error(`Nevar pārsūtīt vairāk, nekā pieejams. Maksimāli: ${transferData.value.max}`);
    return;
  }

  try {
    await axios.post(`https://backend-psi-blush-35.vercel.app/materials/transfer`, {
      material_id: transferData.value.material_id,
      daudzums: transferData.value.daudzums,
      toNoliktava: transferData.value.toNoliktava
    }, {
      headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
    });
    closeDialog();
    toast.success('Materiāls veiksmīgi pārsūtīts!');
    fetchMaterials();
  } catch (error) {
    handleError(error, 'Neizdevās pārsūtīt materiālu.');
    if (error.response && error.response.data && error.response.data.message) {
      toast.error(error.response.data.message);
    }
  }
};

// --- Material Management ---

const addMaterial = async () => {
  if (!formData.value.nosaukums || !formData.value.daudzums || !formData.value.vieniba || !formData.value.noliktava) {
    toast.error('Lūdzu aizpildiet visus laukus!');
    return;
  }
  try {
    const response = await axios.post('https://backend-psi-blush-35.vercel.app/materials', formData.value, {
      headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
    });
    materials.value.push(response.data);
    closeDialog();
    toast.success('Materiāls veiksmīgi pievienots!');
    fetchStats();
  } catch (error) {
    handleError(error, 'Neizdevās pievienot materiālu.');
  }
};

// Filtered materials for search input in order form
const filteredMaterialsForSearch = computed(() => {
  if (!materialSearch.value) return [];
  const query = materialSearch.value.toLowerCase();
  return materials.value.filter(material =>
    material.nosaukums.toLowerCase().includes(query) &&
    !orderMaterials.value.some(om => om.material_id === material.id)
  ).slice(0, 5);
});

// --- Shifts Management (moved from Login.vue for centralization) ---

const calculateShiftDuration = (start, end) => {
  if (!start || !end) return 'N/A';
  const startDate = new Date(start);
  const endDate = new Date(end);
  const durationMs = endDate.getTime() - startDate.getTime();
  const hours = Math.floor(durationMs / (1000 * 60 * 60));
  const minutes = Math.floor((durationMs % (1000 * 60 * 60)) / (1000 * 60));
  return `${hours}h ${minutes}m`;
};

const formatDuration = (durationInMinutes) => {
  const hours = Math.floor(durationInMinutes / 60);
  const minutes = durationInMinutes % 60;
  return `${hours}h ${minutes}m`;
};

const formatDate = (dateString) => {
  if (!dateString) return '';
  const date = new Date(dateString);
  return date.toLocaleDateString('lv-LV', {
    year: 'numeric',
    month: '2-digit',
    day: '2-digit'
  });
};

// --- PDF Generation ---

const fontPath = '/api/fonts/DejaVuSans.ttf';

const generatePDF = async (type) => {
  if (!isPdfLibsReady.value) {
    toast.error('PDF bibliotēkas vēl nav ielādētas. Lūdzu, mēģiniet vēlreiz.');
    return;
  }

  try {
    const now = new Date();
    const doc = new jsPDF({
      orientation: 'portrait',
      unit: 'mm',
      format: 'a4',
      compress: true,
      encoding: 'UTF-8'
    });

    // Use standard font that supports Latvian characters
    doc.setFont('helvetica');
    doc.setFontSize(16);
    
    const encodeText = (text) => {
      if (!text) return '';
      return text
        .replace(/ā/g, 'a')
        .replace(/č/g, 'c')
        .replace(/ē/g, 'e')
        .replace(/ģ/g, 'g')
        .replace(/ī/g, 'i')
        .replace(/ķ/g, 'k')
        .replace(/ļ/g, 'l')
        .replace(/ņ/g, 'n')
        .replace(/š/g, 's')
        .replace(/ū/g, 'u')
        .replace(/ž/g, 'z')
        .replace(/Ā/g, 'A')
        .replace(/Č/g, 'C')
        .replace(/Ē/g, 'E')
        .replace(/Ģ/g, 'G')
        .replace(/Ī/g, 'I')
        .replace(/Ķ/g, 'K')
        .replace(/Ļ/g, 'L')
        .replace(/Ņ/g, 'N')
        .replace(/Š/g, 'S')
        .replace(/Ū/g, 'U')
        .replace(/Ž/g, 'Z');
    };

    if (type === 'stats') {
      // Add general statistics
      doc.text(encodeText('Vispārīgā statistika'), 14, 20);
      doc.setFontSize(10);
      doc.text(encodeText(`Izveidots: ${now.toLocaleDateString('lv-LV')} ${now.toLocaleTimeString('lv-LV')}`), 14, 35);

      // Add statistics table
      doc.autoTable({
        head: [[encodeText('Statistika'), encodeText('Vērtība')]],
        body: [
          [encodeText('Maiņu skaits'), stats.value.total_shifts],
          [encodeText('Materiālu skaits'), stats.value.total_materials],
          [encodeText('Darbinieku skaits'), stats.value.total_workers],
          [encodeText('Pasūtījumu skaits'), stats.value.total_orders]
        ],
        startY: 45,
        margin: { top: 40 },
        styles: {
          font: 'helvetica',
          fontSize: 10,
          cellPadding: 5,
          overflow: 'linebreak',
          fontStyle: 'normal'
        },
        headStyles: {
          fillColor: [41, 41, 41],
          textColor: 255,
          fontStyle: 'bold',
          halign: 'center',
          font: 'helvetica'
        },
        alternateRowStyles: {
          fillColor: [245, 245, 245],
          font: 'helvetica'
        }
      });

      // Add material usage statistics
      doc.setFontSize(14);
      doc.text(encodeText('Materiālu patēriņa statistika'), 14, doc.lastAutoTable.finalY + 20);

      // Add top materials table
      doc.autoTable({
        head: [[encodeText('Materiāls'), encodeText('Izmantots'), encodeText('Vienība')]],
        body: topMaterialStats.value.map(stat => [
          encodeText(stat.nosaukums),
          stat.totalUsed,
          encodeText(stat.vieniba)
        ]),
        startY: doc.lastAutoTable.finalY + 30,
        margin: { top: 40 },
        styles: {
          font: 'helvetica',
          fontSize: 10,
          cellPadding: 5,
          overflow: 'linebreak',
          fontStyle: 'normal'
        },
        headStyles: {
          fillColor: [41, 41, 41],
          textColor: 255,
          fontStyle: 'bold',
          halign: 'center',
          font: 'helvetica'
        },
        alternateRowStyles: {
          fillColor: [245, 245, 245],
          font: 'helvetica'
        }
      });

      // Add material usage summary
      doc.autoTable({
        head: [[encodeText('Kopējais izmantoto materiālu daudzums')]],
        body: [[totalMaterialUsage.value]],
        startY: doc.lastAutoTable.finalY + 20,
        margin: { top: 40 },
        styles: {
          font: 'helvetica',
          fontSize: 10,
          cellPadding: 5,
          overflow: 'linebreak',
          fontStyle: 'normal'
        },
        headStyles: {
          fillColor: [41, 41, 41],
          textColor: 255,
          fontStyle: 'bold',
          halign: 'center',
          font: 'helvetica'
        },
        alternateRowStyles: {
          fillColor: [245, 245, 245],
          font: 'helvetica'
        }
      });
    } else {
      doc.text(encodeText(type === 'orders' ? 'Pasūtījumu saraksts' :
              type === 'materials' ? 'Materiālu saraksts' :
              type === 'workers' ? 'Darbinieku saraksts' :
              'Maiņu saraksts'), 14, 20);

      doc.setFontSize(10);
      doc.text(encodeText(`Izveidots: ${now.toLocaleDateString('lv-LV')} ${now.toLocaleTimeString('lv-LV')}`), 14, 35);

      let tableData = [];
      let columns = [];

      if (type === 'orders') {
        columns = [
          { header: encodeText('Nosaukums'), dataKey: 'nosaukums' },
          { header: encodeText('Daudzums'), dataKey: 'daudzums' },
          { header: encodeText('Statuss'), dataKey: 'status' }
        ];
        tableData = (filteredOrders.value || []).map(order => ({
          nosaukums: encodeText(order.nosaukums),
          daudzums: order.daudzums,
          status: encodeText(order.status)
        }));
      } else if (type === 'materials') {
        columns = [
          { header: encodeText('Nosaukums'), dataKey: 'nosaukums' },
          { header: encodeText('Daudzums'), dataKey: 'daudzums' },
          { header: encodeText('Vienība'), dataKey: 'vieniba' },
          { header: encodeText('Noliktava'), dataKey: 'noliktava' },
          { header: encodeText('Vieta'), dataKey: 'vieta' },
          { header: encodeText('Rezervēts'), dataKey: 'reserved_quantity' }
        ];
        tableData = (filteredMaterials.value || []).map(material => ({
          nosaukums: encodeText(material.nosaukums),
          daudzums: material.daudzums,
          vieniba: encodeText(material.vieniba),
          noliktava: encodeText(material.noliktava),
          vieta: encodeText(material.vieta),
          reserved_quantity: material.reserved_quantity
        }));
      } else if (type === 'workers') {
        columns = [
          { header: encodeText('Vārds'), dataKey: 'vards' },
          { header: encodeText('Uzvārds'), dataKey: 'uzvards' },
          { header: encodeText('Kods'), dataKey: 'kods' },
          { header: encodeText('Amats'), dataKey: 'amats' },
          { header: encodeText('Statuss'), dataKey: 'status' }
        ];
        tableData = (filteredEmployees.value || []).map(employee => ({
          vards: encodeText(employee.vards),
          uzvards: encodeText(employee.uzvards),
          kods: employee.kods,
          amats: encodeText(employee.amats),
          status: encodeText(employee.status)
        }));
      } else if (type === 'shifts') {
        columns = [
          { header: encodeText('Darbinieks'), dataKey: 'worker_name' },
          { header: encodeText('Sākums'), dataKey: 'start_time' },
          { header: encodeText('Ilgums'), dataKey: 'duration' }
        ];
        tableData = (filteredShiftsByDate.value || []).map(shift => ({
          worker_name: shift.employee ? encodeText(`${shift.employee.vards} ${shift.employee.uzvards}`) : encodeText('Nav zināms'),
          start_time: shift.start_time ? new Date(shift.start_time).toLocaleDateString('lv-LV') : encodeText('Nav sākts'),
          duration: shift.end_time ? encodeText(calculateShiftDuration(shift.start_time, shift.end_time)) : encodeText('Nav beidzts')
        }));
      }

      doc.autoTable({
        head: [columns.map(col => col.header)],
        body: tableData.map(row => columns.map(col => row[col.dataKey])),
        startY: 45,
        margin: { top: 40 },
        styles: {
          font: 'helvetica',
          fontSize: 10,
          cellPadding: 5,
          overflow: 'linebreak',
          fontStyle: 'normal'
        },
        headStyles: {
          fillColor: [41, 41, 41],
          textColor: 255,
          fontStyle: 'bold',
          halign: 'center',
          font: 'helvetica'
        },
        alternateRowStyles: {
          fillColor: [245, 245, 245],
          font: 'helvetica'
        },
        columnStyles: {
          0: { cellWidth: 40 },
          1: { cellWidth: 40 },
          2: { cellWidth: 30 },
          3: { cellWidth: 40 },
          4: { cellWidth: 30 }
        },
        didDrawPage: function(data) {
          doc.setFontSize(10);
          doc.text(
            encodeText('Lapa ' + doc.internal.getNumberOfPages()),
            data.settings.margin.left,
            doc.internal.pageSize.height - 10
          );
        }
      });
    }

    doc.save(`${type}_${now.toISOString().split('T')[0]}.pdf`);
    toast.success('PDF veiksmīgi izveidots');
  } catch (error) {
    console.error('Error generating PDF:', error);
    toast.error('Neizdevās izveidot PDF');
  }
};

// Function to handle PDF export button click
const exportToPDF = () => {
  generatePDF(currentTab.value);
};

// Auto-refresh data every 3 seconds
let refreshInterval = null;
onMounted(() => {
  fetchOrders();
  fetchMaterials();
  fetchEmployees();
  fetchShifts();
  fetchStats();

  refreshInterval = setInterval(() => {
    fetchOrders();
    fetchMaterials();
    fetchEmployees();
    fetchShifts();
    fetchStats();
  }, 3000);
});

onUnmounted(() => {
  if (refreshInterval) {
    clearInterval(refreshInterval);
  }
});

// Add new confirmDelete function
const confirmDelete = async () => {
  if (!itemToDelete.value) return;
  
  try {
    if (deleteType.value === 'orders') {
      if (itemToDelete.value.status === 'Nav sākts') {
        for (const mat of itemToDelete.value.materials) {
          const materialToUpdate = materials.value.find(m => m.id === mat.material_id);
          if (materialToUpdate) {
            const newReservedQuantity = (materialToUpdate.reserved_quantity || 0) - mat.daudzums;
            await axios.put(`https://backend-psi-blush-35.vercel.app/materials/${mat.material_id}`, {
              ...materialToUpdate,
              reserved_quantity: Math.max(0, newReservedQuantity)
            }, {
              headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
            });
          }
        }
        toast.info('Materiāli atbrīvoti no rezervācijas.');
      }

      await axios.delete(`https://backend-psi-blush-35.vercel.app/orders/${itemToDelete.value.id}`, {
        headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
      });
      orders.value = orders.value.filter(order => order.id !== itemToDelete.value.id);
      toast.success('Pasūtījums veiksmīgi dzēsts!');
      fetchMaterials();
      fetchStats();
    } else if (deleteType.value === 'materials') {
      await axios.delete(`https://backend-psi-blush-35.vercel.app/materials/${itemToDelete.value.id}`, {
        headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
      });
      materials.value = materials.value.filter(material => material.id !== itemToDelete.value.id);
      toast.success('Materiāls veiksmīgi dzēsts!');
      fetchStats();
    } else if (deleteType.value === 'employees') {
      await axios.delete(`https://backend-psi-blush-35.vercel.app/employees/${itemToDelete.value.id}`, {
        headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
      });
      employees.value = employees.value.filter(employee => employee.id !== itemToDelete.value.id);
      toast.success('Darbinieks veiksmīgi dzēsts!');
      fetchStats();
    }
  } catch (error) {
    handleError(error, `Neizdevās dzēst ${deleteType.value === 'orders' ? 'pasūtījumu' : 
                       deleteType.value === 'materials' ? 'materiālu' : 'darbinieku'}.`);
  } finally {
    showDeleteConfirmDialog.value = false;
    itemToDelete.value = null;
    deleteType.value = '';
  }
};

// Expose to template
const showOrderDetails = ref(false)
const selectedOrder = ref(null)

const openDetailsDialog = (order) => {
  selectedOrder.value = order
  showOrderDetails.value = true
}

// Computed properties for formatting statistics
const formattedTotalMaterialsQuantity = computed(() => {
  return Number(stats.value.total_materials_quantity).toLocaleString('lv-LV', {
    minimumFractionDigits: 2,
    maximumFractionDigits: 2
  });
});

const formattedTotalOrdersValue = computed(() => {
  return Number(stats.value.total_orders_value).toLocaleString('lv-LV', {
    minimumFractionDigits: 2,
    maximumFractionDigits: 2
  });
});

const formattedMaterialUsage = computed(() => {
  return stats.value.material_usage_data.map(stat => ({
    ...stat,
    totalUsed: Number(stat.totalUsed).toLocaleString('lv-LV', {
      minimumFractionDigits: 2,
      maximumFractionDigits: 2
    })
  }));
});
</script>