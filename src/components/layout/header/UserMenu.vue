<script setup>
import { UserCircleIcon, ChevronDownIcon, LogoutIcon, SettingsIcon, InfoCircleIcon } from '@/icons'
import { RouterLink,useRouter } from 'vue-router'
import { ref, onMounted, onUnmounted } from 'vue'
import Cookie from 'js-cookie'
import CryptoJS from 'crypto-js'  
const dropdownOpen = ref(false)
const dropdownRef = ref(null)
const clave='inventario1234'
const user=ref(null)
const menuItems = [
  { href: '/profile', icon: UserCircleIcon, text: 'Edit profile' },
  { href: '/chat', icon: SettingsIcon, text: 'Account settings' },
  { href: '/profile', icon: InfoCircleIcon, text: 'Support' },
]

const router= useRouter();
const getUser=()=>{
  const usuario = CryptoJS.AES.decrypt(Cookie.get('usuario'), clave).toString(CryptoJS.enc.Utf8);
  const cod_usuario = CryptoJS.AES.decrypt(Cookie.get('cod_usuario'), clave).toString(CryptoJS.enc.Utf8);
  user.value=usuario;
  console.log(user.value);
}
const toggleDropdown = () => {
  dropdownOpen.value = !dropdownOpen.value
}

const closeDropdown = () => {
  dropdownOpen.value = false
}

const signOut = () => {
  // Implement sign out logic here
  console.log('Signing out...')
  closeDropdown()
}
const cerrarSesion=()=>{
  Cookie.remove('usuario')
  Cookie.remove('cod_usuario')
  Cookie.remove('token')
  return router.push('/') 
}

const handleClickOutside = (event) => {
  if (dropdownRef.value && !dropdownRef.value.contains(event.target)) {
    closeDropdown()
  }
}

onMounted(() => {
  document.addEventListener('click', handleClickOutside),
  getUser()
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})
</script>




<template>
  <div class="relative" ref="dropdownRef">
    
      <div class="flex items-center text-gray-700 dark:text-gray-400">
      <span class="mr-3 overflow-hidden rounded-full h-11 w-11">
        <img src="/images/user/owner.jpg" alt="User" />
      </span>

      <span class="block mr-1 font-medium text-theme-sm">{{user}} </span>

      <button @click="cerrarSesion"><i class=" pi pi-sign-out text-gray-dark rounded-full border border-gray-200  shadow-theme-lg dark:border-gray-800 dark:text-white hover:bg-gray-300 dark:hover:bg-gray-500  opacity-50 p-3"></i></button>
      
      </div>
    <!-- Dropdown Start -->
    <div
      v-if="dropdownOpen"
      class="absolute right-0 mt-[17px] flex w-[260px] flex-col rounded-2xl border border-gray-200 bg-white p-3 shadow-theme-lg dark:border-gray-800 dark:bg-gray-dark"
    >
      <div>
        <span class="block font-medium text-gray-700 text-theme-sm dark:text-gray-400">
          Musharof Chowdhury
        </span>
        <span class="mt-0.5 block text-theme-xs text-gray-500 dark:text-gray-400">
          randomuser@pimjo.com
        </span>
      </div>

      <ul class="flex flex-col gap-1 pt-4 pb-3 border-b border-gray-200 dark:border-gray-800">
        <li v-for="item in menuItems" :key="item.href">
          <router-link
            :to="item.href"
            class="flex items-center gap-3 px-3 py-2 font-medium text-gray-700 rounded-lg group text-theme-sm hover:bg-gray-100 hover:text-gray-700 dark:text-gray-400 dark:hover:bg-white/5 dark:hover:text-gray-300"
          >
            <!-- SVG icon would go here -->
            <component
              :is="item.icon"
              class="text-gray-500 group-hover:text-gray-700 dark:group-hover:text-gray-300"
            />
            {{ item.text }}
          </router-link>
        </li>
      </ul>
      <router-link
        to="/signin"
        @click="signOut"
        class="flex items-center gap-3 px-3 py-2 mt-3 font-medium text-gray-700 rounded-lg group text-theme-sm hover:bg-gray-100 hover:text-gray-700 dark:text-gray-400 dark:hover:bg-white/5 dark:hover:text-gray-300"
      >
        <LogoutIcon
          class="text-gray-500 group-hover:text-gray-700 dark:group-hover:text-gray-300"
        />
        Sign out
      </router-link>
    </div>
    <!-- Dropdown End -->
  </div>
</template>

