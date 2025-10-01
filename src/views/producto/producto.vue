<script>
import AdminLayout from '@/components/layout/AdminLayout.vue';
import ComponentCard from "@/components/common/ComponentCard.vue";
import axios from 'axios';
import Swal from 'sweetalert2';
import Cookies from 'js-cookie';
import CryptoJS from 'crypto-js'
import Complementos from './Complementos.vue';
import Proveedor from '../proveedor/proveedor.vue';
import flatPickr from 'vue-flatpickr-component'
import 'flatpickr/dist/flatpickr.css'

export default{
    data(){
        return{
            personas:[
                {id:1,nombre:'pedro'},
                {id:2,nombre:'pepe'},
                {id:3,nombre:'juan'},
                {id:4,nombre:'gago'}
            ],
            detalle:[],
            producto:null,
            clave:'inventario1234' ,
            tipo:[
                {value:1,text:'Administrador'},
                {value:2,text:'Operador'},
            ],
            flatpickrTimeConfig:{
            enableTime: true,
            noCalendar: true,
            dateFormat: 'H:i',
            time_24hr: false,
            minuteIncrement: 1,
            wrap: false,
            },
            selectedTipo:'',
            laboratorio:'',
            presentacion:'',
            uso:'',
            laboratorio_options:null,
            presentacion_options:null,
            uso_options:null,
            nombre_comercial:'',
            nombre_cientifico:'',
            contenido:'',
            medicion:'',
            precio_unit:'',
            fecha:null,
            mostrarModal:false,
            proveedor:null,
            showCompra:false          
        }
    },
    methods:{
        async getProducto(){
            try {
                const token= CryptoJS.AES.decrypt(Cookies.get('token'), this.clave).toString(CryptoJS.enc.Utf8);
                const response = await axios.get('http://localhost:3000/inventario/getProducto',{
                    headers:{
                        Authorization:`Bearer ${token}`
                    }
                });
                
                this.producto=response.data.datos;
                
            } catch (error) {
                console.error(error); 
            }
        },
        async getProveedor(){
            try {
                const token= CryptoJS.AES.decrypt(Cookies.get('token'), this.clave).toString(CryptoJS.enc.Utf8);
                const responseProveedor= await axios.get('http://localhost:3000/inventario/getProveedor',{
                    headers:{
                        Authorization:`Bearer ${token}`
                    }
                })
                if (responseProveedor.data.msg==='ok') {
                    console.log(responseProveedor.data.datos)
                    this.proveedor=responseProveedor.data.datos;
                }else{ 
                    this.proveedor=null;
                }
            } catch (error) {
                console.log('ha ocurrido un error: ',error)                
            }
        },
        formatFecha(fecha) {
        if (!fecha) return ''; // Verifica si la fecha es nula o indefinida
        const date = new Date(fecha);
        const anio = date.getFullYear();
        const mes = String(date.getMonth() + 1).padStart(2, '0'); // Mes empieza en 0
        const Mes= mes ==='12' ? 'Diciembre' : mes==='11' ? 'Noviembre' : mes ==='10' ? 'Octubre' : mes ==='09' ? 'Septiembre' : mes==='08' ? 'Agosto' : mes === '07' ? 'Julio' : mes ==='06' ? 'Junio' : mes === '05' ? 'Mayo' : mes ==='04' ? 'Abril' : mes ==='03' ? 'Marzo' : mes==='02' ? 'Febrero' : mes==='01' ? 'Enero' : '00' 
        const dia = String(date.getDate()).padStart(2, '0');
        return `${dia}-${Mes}-${anio}`;
        },
        async getComplementos(){
            try {
                const token= CryptoJS.AES.decrypt(Cookies.get('token'), this.clave).toString(CryptoJS.enc.Utf8);
                const responseLaboratorio= await axios.get('http://localhost:3000/inventario/getLaboratorio',{
                    headers:{
                        Authorization:`Bearer ${token}`
                    }
                })

                this.laboratorio_options=responseLaboratorio.data.datos;

                const responsePresentacion= await axios.get('http://localhost:3000/inventario/getPresentacion',{
                    headers:{
                        Authorization:`Bearer ${token}`
                    }
                })
                this.presentacion_options=responsePresentacion.data.datos;

                const responseUso= await axios.get('http://localhost:3000/inventario/getUso',{
                    headers:{
                        Authorization:`Bearer ${token}`
                    }
                })
                this.uso_options=responseUso.data.datos;
            } catch (error) {
                console.log('ha ocurrido un error: ',error)  
    
            }
        },
        async addProducto(){
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
        /*try {
            const token= CryptoJS.AES.decrypt(Cookies.get('token'), this.clave).toString(CryptoJS.enc.Utf8);
            const responseProducto= await axios.post('http://localhost:3000/inventario/addProducto',{
                nombre_comercial:this.nombre_comercial,
                nombre_cientifico:this.nombre_cientifico,
                contenido:this.contenido,
                medicion:this.medicion,
                precio_unit:this.precio_unit,
                fecha_vencimiento:this.fecha,
                cod_laboratorio:parseInt(this.laboratorio),
                cod_presentacion:parseInt(this.presentacion),
                cod_uso:parseInt(this.uso),
                estado_int:1
            },{
                headers:{
                    Authorization:`Bearer ${token}`
                }
            })
            if (responseProducto.data.msg==='creado') {
                Toast.fire({
                    icon:'success',
                    title:'sistema de inventario',
                    text:'producto creado correctamente',
                    theme:'dark'
                })
                this.getProducto();
                this.nombre_comercial='';
                this.nombre_cientifico='';
                this.contenido='';
                this.medicion='';
                this.precio_unit='';
                this.laboratorio='';
                this.presentacion='';
                this.uso='';
                this.fecha='';

            }else{
                Toast.fire({
                    icon:'error',
                    title:'sistema de inventario',
                    text:'producto no creado',
                    theme:'dark'
                })
            }
        } catch (error) {
            console.log(error)
            Toast.fire({
                icon:'info',
                title:'sistema de inventario',
                text:'Problemas con el Servidor!!',
                theme:'dark'
            })
        } */

        
        this.detalle.push({
            nombre_comercial:this.nombre_comercial,
            nombre_cientifico:this.nombre_cientifico,
            contenido:this.contenido,
            medicion:this.medicion,
            precio_unit:this.precio_unit,
            fecha_vencimiento:this.fecha,
            cod_laboratorio:parseInt(this.laboratorio),
            cod_presentacion:parseInt(this.presentacion),
            cod_uso:parseInt(this.uso),
            estado_int:1
        })
        Toast.fire({
            icon:'success',
            title:'sistema de inventario',
            text:'medicamento agregado correctamente',
            theme:'dark'
        })
        this.nombre_comercial='';
        this.nombre_cientifico='';
        this.contenido='';
        this.medicion='';
        this.precio_unit='';
        this.laboratorio='';
        this.presentacion='';
        this.uso='';
        },        
        cerrarModal(){
            this.mostrarModal=false;
        },
        abrirModal(){
            this.mostrarModal=true;
        },
        detalleProducto(selected){
            console.log(selected)
        },
        showMedicamento(){
            this.showCompra=true;
        }

    },
    mounted(){
        this.getProducto();
        this.getComplementos();
        this.getProveedor();
    },
    watch:{
          
    },
    components:{
        AdminLayout, 
        ComponentCard,
        Complementos,
        Proveedor,
        flatPickr 
    },
}
</script>


<template>
    <AdminLayout>
        <Proveedor></Proveedor>
        <div class="grid grid-cols-1 gap-6">
            <transition enter-active-class="transition duration-300 ease-out"
                enter-from-class="opacity-0 scale-95"
                enter-to-class="opacity-100 scale-100"
                leave-active-class="transition duration-200 ease-in"
                leave-from-class="opacity-100 scale-100"
                leave-to-class="opacity-0 scale-95">
            <ComponentCard title="Registrar Compra" v-if="showCompra">
                <h3 class="text-gray-300 text-md font-semibold">Nuevo Producto</h3>
                <form method="post">
                    <div class=" grid grid-cols-2 xs:grid-cols-1 space-y-6 space-x-2 min-w-lg ">
                    <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">Nombre Comercial</label>
                        <input type="text" v-model="nombre_comercial" placeholder="ingrese nombre comercial" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"/>
                    </div>
                    <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">Nombre Comercial</label>
                        <input type="text" v-model="nombre_cientifico" placeholder="ingrese nombre cientifico" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"/>
                    </div>
                    </div>
                    <div class=" grid grid-cols-3 space-y-6 space-x-2 ">
                        <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">Contenido</label>
                        <input type="text" v-model="contenido" placeholder="ingrese el contenido" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"/>
                        </div>
                        <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">Medicion</label>
                        <input type="text" v-model="medicion" placeholder="ingrese la medicion" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"/>
                        </div>
                        <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">Precio unitario</label>
                        <input type="text" v-model="precio_unit" placeholder="ingrese el precio" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"/>
                        </div>
    
                    </div>
                    <div class=" grid xl:grid-cols-2 md:grid-cols-2 sm:grid-cols-1 space-y-6 space-x-2">
                        <div>
                            <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">Fecha vencimiento</label>
                            <div class="relative">
                                <flat-pickr
                                v-model="fecha"
                                :config="flatpickrConfig"
                                class="dark:bg-dark-900 h-11 w-full appearance-none rounded-lg border border-gray-700 bg-transparent bg-none px-4 py-2.5 pl-4 pr-11 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-700 focus:border-brand-700 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"
                                placeholder="ingrese fecha"/>
                                <span
                                class="absolute text-gray-500 -translate-y-1/2 pointer-events-none right-3 top-1/2 dark:text-gray-400">
                                <svg
                                class="fill-current"
                                width="20"
                                height="20"
                                viewBox="0 0 20 20"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg">
                                <path
                                fill-rule="evenodd"
                                clip-rule="evenodd"
                                d="M6.66659 1.5415C7.0808 1.5415 7.41658 1.87729 7.41658 2.2915V2.99984H12.5833V2.2915C12.5833 1.87729 12.919 1.5415 13.3333 1.5415C13.7475 1.5415 14.0833 1.87729 14.0833 2.2915V2.99984L15.4166 2.99984C16.5212 2.99984 17.4166 3.89527 17.4166 4.99984V7.49984V15.8332C17.4166 16.9377 16.5212 17.8332 15.4166 17.8332H4.58325C3.47868 17.8332 2.58325 16.9377 2.58325 15.8332V7.49984V4.99984C2.58325 3.89527 3.47868 2.99984 4.58325 2.99984L5.91659 2.99984V2.2915C5.91659 1.87729 6.25237 1.5415 6.66659 1.5415ZM6.66659 4.49984H4.58325C4.30711 4.49984 4.08325 4.7237 4.08325 4.99984V6.74984H15.9166V4.99984C15.9166 4.7237 15.6927 4.49984 15.4166 4.49984H13.3333H6.66659ZM15.9166 8.24984H4.08325V15.8332C4.08325 16.1093 4.30711 16.3332 4.58325 16.3332H15.4166C15.6927 16.3332 15.9166 16.1093 15.9166 15.8332V8.24984Z"
                                fill=""/></svg>
                                </span>
                            </div>
                        </div>
                        <div>
                            <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Seleccionar Laboratorio
                            </label>
                            <div class="relative z-20 bg-transparent">
                                <select
                                v-model="laboratorio"    
                                class="dark:bg-dark-900 h-11 w-full appearance-none rounded-lg border border-gray-300 bg-transparent bg-none px-4 py-2.5 pr-11 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-700 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"
                                :class="{ 'text-gray-800 dark:text-white/90':laboratorio }"
                                >
                                <option value="" selected disabled>Seleciona una opcion</option>
                                <option v-for="item in laboratorio_options " :key="item.cod_laboratorio" :value="item.cod_laboratorio" class="text-gray-700 dark:bg-gray-900 dark:text-gray-400">
                                    {{ item.laboratorio }}
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
                                <option v-for="item in presentacion_options" :key="item.cod_presentacion" :value="item.cod_presentacion" class="text-gray-700 dark:bg-gray-900 dark:text-gray-400">
                                    {{ item.presentacion }}
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
                            Seleccionar Uso
                            </label>
                            <div class="relative z-20 bg-transparent">
                                <select
                                v-model="uso"    
                                class="dark:bg-dark-900 h-11 w-full appearance-none rounded-lg border border-gray-300 bg-transparent bg-none px-4 py-2.5 pr-11 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"
                                :class="{ 'text-gray-800 dark:text-white/90':uso }"
                                >
                                <option value="" selected disabled>Seleciona una opcion</option>
                                <option v-for="item in uso_options" :key="item.cod_uso" :value="item.cod_uso" class="text-gray-700 dark:bg-gray-900 dark:text-gray-400">
                                    {{ item.uso }}
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
                    <button type="button" @click="addProducto" class="w-full text-white bg-blue-600 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Registrar</button>
                    
                </form>
                <hr class=" bg-gray-600">
                <div class=" justify-end flex "> 
                        <i class=" pi pi-plus text-white border-2 rounded-full p-2 border-gray-800 cursor-pointer hover:bg-gray-500 hover:transform hover:rotate-180 duration-300"></i>
                </div>
                 <div class=" grid grid-cols-3 gap-2 ">
                        <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">Cantidad inicial</label>
                        <input type="text"  placeholder="ingrese cantidad" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"/>
                        </div>
                        <div> 
                        <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">Precio</label>
                        <input type="text"  placeholder="ingrese cantidad" class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"/>
                        </div>
                        <div>
                            <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                            Seleccione proveedor
                            </label>
                            <div class="relative z-20 bg-transparent">
                                <select
                                v-model="laboratorio"    
                                class="dark:bg-dark-900 h-11 w-full appearance-none rounded-lg border border-gray-300 bg-transparent bg-none px-4 py-2.5 pr-11 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-700 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800"
                                :class="{ 'text-gray-800 dark:text-white/90':laboratorio }"
                                >
                                <option value="" selected disabled>Seleciona una opcion</option>
                                <option v-for="item in proveedor " :key="item.cod_proveedor" :value="item.nombre_proveedor" class="text-gray-700 dark:bg-gray-900 dark:text-gray-400">
                                    {{ item.nombre_proveedor }}
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
                        <button type="button" @click="abrirModal"  class=" mt-6 h-11 text-white bg-orange-500 hover:bg-orange-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-orange-500 dark:hover:bg-orange-700 dark:focus:ring-primary-800"><i class=" pi pi-eye mx-1"></i>Detalle</button>   
                        
                 </div>   
                 
        </ComponentCard>
        </transition> 
        <ComponentCard title="Lista de Producto">
            <div class=" flex justify-between">
                <div class=" justify-start flex">
                <input type="text" placeholder=" buscar producto" class=" bg-slate-600 min-w-lg rounded-lg p-2 text-sm text-slate-100">
            </div>
            <div class=" justify-end items-center flex space-x-4 ">
                <p class=" font-Outfit text-sm text-slate-200">Nuevo medicamento</p> 
                <i @click="showMedicamento" class=" pi pi-plus text-white border-2 rounded-full p-2 border-gray-800 cursor-pointer hover:bg-gray-500 hover:transform hover:rotate-180 duration-300"></i>
            </div>
            </div>
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
                    v-for="(item, index) in producto"
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
                            {{ item.nombre_comercial}}
                        </span>
                        </div>
                    </div>
                    </td>
                    
                    <td class="px-5 py-4 sm:px-6">
                    <div class="flex items-center gap-3">
                        <!--<div class="w-10 h-10 overflow-hidden rounded-full">
                        <img :src="user.avatar" :alt="user.name" />
                        </div>-->
                        <div>
                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">
                            {{ item.nombre_cientifico}}
                        </span>
                        </div>
                    </div>
                    </td>
                    <td class="px-5 py-4 sm:px-6">
                    <div class="flex items-center gap-3">
                        <!--<div class="w-10 h-10 overflow-hidden rounded-full">
                        <img :src="user.avatar" :alt="user.name" />
                        </div>-->
                        <div>
                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">
                            {{ item.contenido}}
                        </span>
                        </div>
                    </div>
                    </td>
                    <td class="px-5 py-4 sm:px-6">
                    <div class="flex items-center gap-3">
                        <!--<div class="w-10 h-10 overflow-hidden rounded-full">
                        <img :src="user.avatar" :alt="user.name" />
                        </div>-->
                        <div>
                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">
                            {{ item.medicion}}
                        </span>
                        </div>
                    </div>
                    </td>
                    <td class="px-5 py-4 sm:px-6">
                    <div class="flex items-center gap-3">
                        <!--<div class="w-10 h-10 overflow-hidden rounded-full">
                        <img :src="user.avatar" :alt="user.name" />
                        </div>-->
                        <div>
                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">
                            {{ item.precio_unit}}
                        </span>
                        </div>
                    </div>
                    </td>
                    <td class="px-5 py-4 sm:px-6">
                    <div class="flex items-center gap-3">
                        <!--<div class="w-10 h-10 overflow-hidden rounded-full">
                        <img :src="user.avatar" :alt="user.name" />
                        </div>-->
                        <div>
                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">
                            {{ formatFecha(item.fecha_vencimiento)}}
                        </span>
                        </div>
                    </div>
                    </td>
                    <td class="px-5 py-4 sm:px-6">
                    <div class="flex items-center gap-3">
                        <!--<div class="w-10 h-10 overflow-hidden rounded-full">
                        <img :src="user.avatar" :alt="user.name" />
                        </div>-->
                        <div>
                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">
                            {{ item.laboratorio}}
                        </span>
                        </div>
                    </div>
                    </td>
                    <td class="px-5 py-4 sm:px-6">
                    <div class="flex items-center gap-3">
                        <!--<div class="w-10 h-10 overflow-hidden rounded-full">
                        <img :src="user.avatar" :alt="user.name" />
                        </div>-->
                        <div>
                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">
                            {{ item.presentacion}}
                        </span>
                        </div>
                    </div>
                    </td>
                    <td class="px-5 py-4 sm:px-6">
                    <div class="flex items-center gap-3">
                        <!--<div class="w-10 h-10 overflow-hidden rounded-full">
                        <img :src="user.avatar" :alt="user.name" />
                        </div>-->
                        <div>
                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">
                            {{ item.uso}}
                        </span>
                        </div>
                    </div>
                    </td>
                    <!--<td class="px-5 py-4 sm:px-6">
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
                    </td>-->
                </tr>
                </tbody>
            </table>
            </div>
        </div>
        </ComponentCard>
        </div>
        <Complementos></Complementos>
            <transition
                enter-active-class="transition duration-300 ease-out"
                enter-from-class="opacity-0 scale-95"
                enter-to-class="opacity-100 scale-100"
                leave-active-class="transition duration-200 ease-in"
                leave-from-class="opacity-100 scale-100"
                leave-to-class="opacity-0 scale-95">

        <div v-if="mostrarModal" class="fixed inset-0 flex items-center justify-center z-50">
            <div class="bg-black bg-opacity-80 text-white max-w-2xl w-full p-6 rounded-xl shadow-2xl flex flex-col gap-2">
                <div class="flex items-center justify-between">
        <h2 class="text-lg font-semibold">Detalle de Compra</h2>
        <button
          @click="cerrarModal"
          class="ml-2 text-white bg-red-600 hover:bg-red-700 rounded-full w-8 h-8 flex items-center justify-center"
          aria-label="Cerrar">&times;</button>
                </div>
                    <!-- tabla contenido-->
                <div class="overflow-hidden rounded-xl border border-gray-200 bg-white dark:border-gray-800 dark:bg-white/[0.03]">
                    <div class="max-w-full overflow-x-auto custom-scrollbar">
                        <table class="min-w-full">
                            <thead>
                                <tr class="border-b border-gray-200 dark:border-gray-700">
                                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Medicamento</p>
                                    </th>
                                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Contenido</p>
                                    </th>
                                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Precio unit</p>
                                    </th>
                                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Cantidad</p>
                                    </th>
                                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Subtotal</p>
                                    </th>
                                    <th class="px-5 py-3 text-left w-3/11 sm:px-6">
                                    <p class="font-medium text-gray-500 text-theme-xs dark:text-gray-400">Quit</p>
                                    </th>
                                </tr>
                            </thead> 
                            <tbody>
                                <tr class="border-t border-gray-100 dark:border-gray-800" v-for="(item,index) in detalle" :key="index">
                                    <td class="px-5 py-4 sm:px-6">
                                        <div class="flex items-center gap-3">
                                        <div>
                                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">{{ item.nombre_comercial }}</span>
                                        </div>
                                        </div>
                                    </td>
                                    <td class="px-5 py-4 sm:px-6">
                                        <div class="flex items-center gap-3">
                                        <div>
                                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">{{ item.contenido }}</span>
                                        </div>
                                        </div>
                                    </td>
                                    <td class="px-5 py-4 sm:px-6">
                                        <div class="flex items-center gap-3">
                                        <div>
                                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90">{{ item.precio_unit }}</span>
                                        </div>
                                        </div>
                                    </td>
                                    <td class="px-5 py-4 sm:px-6">
                                        <span class="block font-medium text-gray-800 text-theme-sm dark:text-white/90 space-x-1">
                                        <button @click="detalleProducto(item)"><i class="pi pi-trash rounded-full bg-red-500 p-1"></i></button>
                                        </span>
                                    </td>
                                </tr>
                            </tbody>   
                    </table>
            </div>
       </div>
    </div>
  </div>
</transition>      
    </AdminLayout>
    
</template>