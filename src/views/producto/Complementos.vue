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
            laboratorio:null,
            presentacion:null,
            uso:null,
            clave:'inventario1234'
        }
    },
    methods:{
        async addLaboratorio(){
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
                const responseLaboratorio= await axios.post('http://localhost:3000/inventario/addLaboratorio',{
                    laboratorio:this.laboratorio,
                    estado_int:1
                },
                {
                    headers:{
                        Authorization:`Bearer ${token}`
                    } 
                })

                if(responseLaboratorio.data.msg==='creado'){
                    Toast.fire({
                        icon:'success',
                        title:'Sistema inventario',
                        text:'laboratorio creado',
                        theme:'dark'
                    })
                }else{
                    Toast.fire({
                        icon:'info',
                        title:'Sistema inventario',
                        text:'error al crear laboratorio',
                        theme:'dark'
                    })
                }
            } catch (error) {
                console.log(error)
                Toast.fire({
                    icon:'error',
                    title:'Sistema inventario',
                    text:'error en el servidor',
                    theme:'dark'
                })
            } 
        },
        async addPresentacion(){
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
                const responsePresentacion= await axios.post('http://localhost:3000/inventario/addPresentacion',{
                    presentacion:this.presentacion,
                    estado_int:1
                },
                {
                    headers:{
                        Authorization:`Bearer ${token}`
                    }
                })

                if(responsePresentacion.data.msg==='creado'){
                    Toast.fire({
                        icon:'success',
                        title:'Sistema inventario',
                        text:'presentacion creado',
                        theme:'dark'
                    })
                }else{
                    Toast.fire({
                        icon:'info',
                        title:'Sistema inventario',
                        text:'error al crear presentacion',
                        theme:'dark'
                    })
                }
            } catch (error) {
                console.log(error)
                Toast.fire({
                    icon:'error',
                    title:'Sistema inventario',
                    text:'eror en el servidor!',
                    theme:'dark'
                })
            }
        },
        async addUso(){
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
                const responseUso= await axios.post('http://localhost:3000/inventario/addUso',{
                    uso:this.uso,
                    estado_int:1
                },
                {
                    headers:{
                        Authorization:`Bearer ${token}`
                    }
                })

                if(responseUso.data.msg==='creado'){
                    Toast.fire({
                        icon:'success',
                        title:'Sistema inventario',
                        text:'uso creado',
                        theme:'dark'
                    })
                }else{
                    Toast.fire({
                        icon:'info',
                        title:'Sistema inventario',
                        text:'error al crear uso',
                        theme:'dark'
                    })
                }
            } catch (error) {
                console.log(error)
                Toast.fire({
                    icon:'error',
                    title:'Sistema inventario',
                    text:'error en el servidor',
                    theme:'dark'
                })   
            }
        }
    },
    components:{
        AdminLayout,
        ComponentCard,
        
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
                    <button type="button" @click="addLaboratorio" class="w-full text-white bg-blue-600 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Registrar</button>
                    
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
                    <button type="button" @click="addPresentacion" class="w-full text-white bg-blue-600 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Registrar</button>
                    
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
                    <button type="button" @click="addUso" class="w-full text-white bg-blue-600 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Registrar</button>
                    
                </form>
            
        </ComponentCard>
        </div>
    
</template>