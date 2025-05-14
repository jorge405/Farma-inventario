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

        }
    },
    components:{
        AdminLayout,
        ComponentCard,
        inputPersonalizado
    }

}

</script>


<template>
    
        <div class="grid grid-cols-3 gap-6 mt-5">
            <ComponentCard title="Crear Laboratorio">
                <form method="post">
                    <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Laboratorio
                        </label>
                        <input type="text" v-model="laboratorio" placeholder="ingrese el laboratorio" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800 mb-4"/>
                    </div>
                    <button type="button" class="w-full text-white bg-blue-600 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Registrar</button>
                    
                </form>
            
        </ComponentCard>
        <ComponentCard title="Crear Presentacion">
                <form method="post">
                    <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Presentacion
                        </label>
                        <input type="text" v-model="presentacion" placeholder="ingrese el laboratorio" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800 mb-4"/>
                    </div>
                    <button type="button" class="w-full text-white bg-blue-600 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Registrar</button>
                    
                </form>
            
        </ComponentCard>
        <ComponentCard title="Crear Uso">
                <form method="post">
                    <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Uso
                        </label>
                        <input type="text" v-model="uso" placeholder="ingrese el laboratorio" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800 mb-4"/>
                    </div>
                    <button type="button" class="w-full text-white bg-blue-600 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Registrar</button>
                    
                </form>
            
        </ComponentCard>
        </div>
    
</template>