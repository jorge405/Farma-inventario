<script>
import axios from 'axios';
import Swal from 'sweetalert2';
import Cookies from 'js-cookie';
import CryptoJS from 'crypto-js';
import ComponentCard from '@/components/common/ComponentCard.vue';
export default{
    data(){
        return{
            nombre_prov: '',
            direccion: '',
            celular: '',
            clave:'inventario1234'
        }
    },
    methods:{
        async registrarProveedor(){
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
            try{
                const token= CryptoJS.AES.decrypt(Cookies.get('token'), this.clave).toString(CryptoJS.enc.Utf8);
                const responseProveedor= await axios.post('http://localhost:3000/inventario/addProveedor',{
                    nombre_proveedor:this.nombre_prov,
                    direccion:this.direccion,
                    celular:this.celular,
                    estado_int:1
                },{
                    headers:{
                        Authorization:`Bearer ${token}`
                    }
                })

                if(responseProveedor.data.msg==='creado'){
                    Toast.fire({
                        icon:'success',
                        title:'Sistema inventario',
                        text:'Proveedor creado correctamente', 
                        theme:'dark'
                    })
                }else{
                    Toast.fire({
                        icon:'error',
                        title:'Sistema inventario',
                        text:'Error al crear el proveedor',
                        theme:'dark'
                    })
                }
            }catch(error){
                console.log(error);
                Toast.fire({
                    icon:'error',
                    title:'Sistema inventario',
                    text:'Error en el servidor',
                    theme:'dark'
                })
            }    
        }
    },
    components:{
        ComponentCard
    }
}

</script>


<template>
<div class="grid grid-cols-1 gap-6 mt-4 mb-4">
<ComponentCard title="Registrar Proveedor" >
    <div>
        <form method="post">
            <div class="grid grid-cols-4 xs:grid-cols-2 gap-2">
                <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Nombre
                        </label>
                        <input type="text" v-model="nombre_prov"  placeholder="ingrese el nombre" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800 mb-4"/>
                    </div>
                    <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Direccion
                        </label>
                        <input type="text" v-model="direccion"  placeholder="ingresa la direccion" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800 mb-4"/>
                    </div>
                    <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Celular
                        </label>
                        <input type="text" v-model="celular" placeholder=" numero de celular" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800 mb-4"/>
                    </div>
                    <button type="button" @click="registrarProveedor" class=" mt-6 h-11 text-white bg-blue-600 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Registrar</button>
            </div>        
        </form>
    </div>
    
</ComponentCard>
</div>
</template>