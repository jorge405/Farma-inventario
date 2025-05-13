<script>
import AdminLayout from '@/components/layout/AdminLayout.vue';
import ComponentCard from "@/components/common/ComponentCard.vue";
import axios from 'axios';
import Swal from 'sweetalert2';
import Cookies from 'js-cookie';
import CryptoJS from 'crypto-js';


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
            usuario:'',
            pass:''           
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
                console.log(response.data) 
            } catch (error) {
                console.error(error);
            }
        },
        
        async addUser(){
            const Toast= Swal.mixin({
                        toast:true,
                        position:"bottom-end",
                        showConfirmButton:false,
                        timer:3000,
                        timerProgressBar:true,
                        didOpen:(toast)=>{
                            toast.onmouseenter=Swal.stopTimer;
                            toast.onmouseleave=Swal.resumeTimer
                        }
                    })
            
            try {
                const token= CryptoJS.AES.decrypt(Cookies.get('token'), this.clave).toString(CryptoJS.enc.Utf8);
                
                const data={
                    usuario:this.usuario,
                    pass:this.pass,
                    estado_int:1
                }
                const response = await axios.post('http://localhost:3000/inventario/addUser',data,{
                    headers:{
                        Authorization:`Bearer ${token}`
                    }
                })

                if (response.data.msg==='usuario creado') {
                   Toast.fire({
                    icon:'success',
                    title:'sistema inventario',
                    text:'usuario creado',
                    theme:'dark'
                   }) 
                    
                }else{
                    Toast.fire({
                        icon:'error',
                        title:'sistema inventario',
                        text:'error al crear usuario',
                        theme:'dark'
                    })
                }
            } catch (error) {
                console.log('a ocurrido un error: ',error)
                Toast.fire({
                        icon:'error',
                        title:'sistema inventario',
                        text:'problemas con el servidor',
                        theme:'dark'
                    })
            } 
        } 
    },
    mounted(){
        this.getUsers();
    },
    components:{
        AdminLayout,
        ComponentCard, 
    },
}
</script>


<template>
    <AdminLayout>
        <div class="grid grid-cols-2 gap-6">
            <ComponentCard title="Crear Usuario">
                <form method="post">
                    <div class=" space-y-6">
                        <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Usuario
                        </label>
                        <input type="text" v-model="usuario" placeholder="ingrese su usuario" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"/>
                        </div>
                        <div>
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Password
                        </label>
                        <input type="text" v-model="pass" placeholder="******" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"/>
                        </div>
                    <div>
                    <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                        Seleccionar Tipo
                    </label>
                    <div class="relative z-20 bg-transparent">
                        <select
                        v-model="selectedTipo"    
                        class="dark:bg-dark-900 h-11 w-full appearance-none rounded-lg border border-gray-300 bg-transparent bg-none px-4 py-2.5 pr-11 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"
                        :class="{ 'text-gray-800 dark:text-white/90':selectedTipo }"
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
                    <button type="button" @click="addUser" class="w-full text-white bg-blue-600 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Registrar</button>
                    </div>
                </form>
            
        </ComponentCard>
        <ComponentCard title="Lista de Usuarios">
        <div class="overflow-hidden rounded-xl border border-gray-200 bg-white dark:border-gray-800 dark:bg-white/[0.03]">
            <div class="max-w-full overflow-x-auto custom-scrollbar">
            <table class="min-w-full">
                <thead>
                <tr class="border-b border-gray-200 dark:border-gray-700">
                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Usuario</p>
                    </th>
                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Estado</p>
                    </th>
                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Acciones</p>
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
                    <td class="px-5 py-4 sm:px-6">
                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90 space-x-1">
                            <button><i class="pi pi-pen-to-square rounded-full bg-blue-900 p-1"></i></button>
                            <button><i class="pi pi-trash rounded-full bg-red-500 p-1"></i></button>
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