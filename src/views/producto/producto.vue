<script>
import AdminLayout from '@/components/layout/AdminLayout.vue';
import ComponentCard from "@/components/common/ComponentCard.vue";
import axios from 'axios';
import Swal from 'sweetalert2';
import Cookies from 'js-cookie';
import CryptoJS from 'crypto-js';
import inputPersonalizado from '@/components/forms/FormElements/inputPersonalizado.vue';
export default{
    data(){
        return{
            users:[],
            clave:'inventario1234' ,
            tipo:[
                {value:1,text:'Administrador'},
                {value:2,text:'Operador'},
            ],
            selectedTipo:'',
            laboratorio:'',
            presentacion:'',
            uso:''           
        }
    },
    methods:{
        async getUsers(){
            try {
                const token= CryptoJS.AES.decrypt(Cookies.get('token'), this.clave).toString(CryptoJS.enc.Utf8);
                const response = await axios.get('http://localhost:3000/inventario/getUser',{
                    headers:{
                        Authorization:`Bearer ${token}`
                    }
                });
                this.users = response.data;
            } catch (error) {
                console.error(error);
            }
        }
    },
    mounted(){
        this.getUsers();
    },
    components:{
        AdminLayout,
        ComponentCard,
        inputPersonalizado
    },
}
</script>


<template>
    <AdminLayout>
        <div class="grid grid-cols-2 gap-6">
            <ComponentCard title="Crear Producto">
                <form method="post">
                    <div class=" grid grid-cols-2 sm:grid-col-1 space-y-6 space-x-2">
                    <inputPersonalizado :label="'Nombre Comercial'" placeholder="ingrese nombre comercial" />
                    <inputPersonalizado :label="'Nombre Cientifico'" placeholder="ingrese nombre cientifico" />
                    </div>
                    <div class=" grid grid-cols-3 space-y-6 space-x-2 ">
                        <input-personalizado :label="'Contenido'" placeholder="ingrese el contenido"></input-personalizado>
                        <input-personalizado :label="'Medicion'" placeholder="ingrese la medicion"></input-personalizado>
                        <input-personalizado :label="'Precio unitario'" placeholder="ingrese el precio"></input-personalizado>
                    </div>
                    <div class=" grid xl:grid-cols-2 md:grid-cols-2 sm:grid-cols-1 space-y-6 space-x-2">
                        <div>
                            <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Seleccionar Fecha
                            </label>
                            <input type="date" class="dark:bg-dark-900 h-11 w-full appearance-none rounded-lg border border-gray-300 bg-transparent bg-none px-4 py-2.5 pl-4 pr-11 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"/>
                        </div>    
                        <div>
                            <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Seleccionar Laboratorio
                            </label>
                            <div class="relative z-20 bg-transparent">
                                <select
                                v-model="laboratorio"    
                                class="dark:bg-dark-900 h-11 w-full appearance-none rounded-lg border border-gray-300 bg-transparent bg-none px-4 py-2.5 pr-11 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"
                                :class="{ 'text-gray-800 dark:text-white/90':laboratorio }"
                                >
                                <option value="" selected disabled>Seleciona una opcion</option>
                                <option v-for="item in tipo" :key="item.value" :value="item.text" class="text-gray-700 dark:bg-gray-900 dark:text-gray-400">
                                    {{ item.text }}
                                </option>
                                </select>
                                <span
                                class="absolute z-30 text-gray-700 -translate-y-1/2 pointer-events-none right-4 top-1/2 dark:text-gray-400"
                                >
                                <svg
                                    class="stroke-current"
                                    width="20"
                                    height="20"
                                    viewBox="0 0 20 20"
                                    fill="none"
                                    xmlns="http://www.w3.org/2000/svg"
                                >
                                    <path
                                    d="M4.79175 7.396L10.0001 12.6043L15.2084 7.396"
                                    stroke=""
                                    stroke-width="1.5"
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    />
                                </svg>
                                </span>
                            </div>
                        </div>
                        <div>
                            <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Seleccionar presentacion
                            </label>
                            <div class="relative z-20 bg-transparent">
                                <select
                                v-model="presentacion"    
                                class="dark:bg-dark-900 h-11 w-full appearance-none rounded-lg border border-gray-300 bg-transparent bg-none px-4 py-2.5 pr-11 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"
                                :class="{ 'text-gray-800 dark:text-white/90':presentacion }"
                                >
                                <option value="" selected disabled>Seleciona una opcion</option>
                                <option v-for="item in tipo" :key="item.value" :value="item.text" class="text-gray-700 dark:bg-gray-900 dark:text-gray-400">
                                    {{ item.text }}
                                </option>
                                </select>
                                <span
                                class="absolute z-30 text-gray-700 -translate-y-1/2 pointer-events-none right-4 top-1/2 dark:text-gray-400"
                                >
                                <svg
                                    class="stroke-current"
                                    width="20"
                                    height="20"
                                    viewBox="0 0 20 20"
                                    fill="none"
                                    xmlns="http://www.w3.org/2000/svg"
                                >
                                    <path
                                    d="M4.79175 7.396L10.0001 12.6043L15.2084 7.396"
                                    stroke=""
                                    stroke-width="1.5"
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    />
                                </svg>
                                </span>
                            </div>
                        </div>
                        <div>
                            <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Seleccionar Laboratorio
                            </label>
                            <div class="relative z-20 bg-transparent">
                                <select
                                v-model="uso"    
                                class="dark:bg-dark-900 h-11 w-full appearance-none rounded-lg border border-gray-300 bg-transparent bg-none px-4 py-2.5 pr-11 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"
                                :class="{ 'text-gray-800 dark:text-white/90':uso }"
                                >
                                <option value="" selected disabled>Seleciona una opcion</option>
                                <option v-for="item in tipo" :key="item.value" :value="item.text" class="text-gray-700 dark:bg-gray-900 dark:text-gray-400">
                                    {{ item.text }}
                                </option>
                                </select>
                                <span
                                class="absolute z-30 text-gray-700 -translate-y-1/2 pointer-events-none right-4 top-1/2 dark:text-gray-400"
                                >
                                <svg
                                    class="stroke-current"
                                    width="20"
                                    height="20"
                                    viewBox="0 0 20 20"
                                    fill="none"
                                    xmlns="http://www.w3.org/2000/svg"
                                >
                                    <path
                                    d="M4.79175 7.396L10.0001 12.6043L15.2084 7.396"
                                    stroke=""
                                    stroke-width="1.5"
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    />
                                </svg>
                                </span>
                            </div>
                        </div>

                    </div>
                    <button type="button" class="w-full text-white bg-blue-600 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Registrar</button>
                    
                </form>
            
        </ComponentCard>
        <ComponentCard title="Lista de Usuarios">
        <div class="overflow-hidden rounded-xl border border-gray-200 bg-white dark:border-gray-800 dark:bg-white/[0.03]">
            <div class="max-w-full overflow-x-auto custom-scrollbar">
            <table class="min-w-full">
                <thead>
                <tr class="border-b border-gray-200 dark:border-gray-700">
                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Nombre comercial</p>
                    </th>
                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Nombre cientifico</p>
                    </th>
                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Contenido</p>
                    </th>
                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Medicion</p>
                    </th>
                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Precio unitario</p>
                    </th>
                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">fecha vencimiento</p>
                    </th>
                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Laboratorio</p>
                    </th>
                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Presentacion</p>
                    </th>
                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Uso</p>
                    </th>
                </tr>
                </thead>
                <tbody class="divide-y divide-gray-200 dark:divide-gray-700">
                <tr
                    v-for="(user, index) in users"
                    :key="index"
                    class="border-t border-gray-100 dark:border-gray-800"
                >
                    <td class="px-5 py-4 sm:px-6">
                    <div class="flex items-center gap-3">
                        <!--<div class="w-10 h-10 overflow-hidden rounded-full">
                        <img :src="user.avatar" :alt="user.name" />
                        </div>-->
                        <div>
                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">
                            {{ user.usuario }}
                        </span>
                        </div>
                    </div>
                    </td>
                    
                    <td class="px-5 py-4 sm:px-6">
                    <span
                        :class="[
                        'rounded-full px-2 py-0.5 text-theme-xs font-medium',
                        {
                            'bg-success-50 text-success-700 dark:bg-success-500/15 dark:text-success-500':
                            user.estado_int === 1,
                            'bg-error-50 text-error-700 dark:bg-error-500/15 dark:text-error-500':
                            user.status === 0,
                        },
                        ]"
                    >
                        {{ user.estado_int === 1 ? 'Activo' : 'Inactivo' }}
                    </span>
                    </td>
                </tr>
                </tbody>
            </table>
            </div>
        </div>
        </ComponentCard>
        </div>
    </AdminLayout>
</template>