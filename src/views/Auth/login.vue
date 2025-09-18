<script>
import router from '@/router';
import axios from 'axios';
import Swal from 'sweetalert2';
import Cookies from 'js-cookie';
import CryptoJS from 'crypto-js';
import FullScreenLayout from '@/components/layout/FullScreenLayout.vue';
export default{
    data(){
        return{
            usuario:null,
            pass:null,
            clave:'inventario1234'
        }
    },
    methods:{
        async login(){
            try {

                const response = await axios.post('http://localhost:3000/inventario/authUser',{
                    usuario:this.usuario,
                    pass: this.pass 
                    });
                    console.log(response.data);
                    if(response.data.msg==='contraseña y usuario correcto'){
                        Swal.fire({
                        icon:'success',
                        title:'Bienvenido',
                        text:'usuario autenticado',
                        theme:'dark',
                        timer: 2000
                    })
                    const usuario= CryptoJS.AES.encrypt(this.usuario, this.clave).toString();
                    const cod_usuario= CryptoJS.AES.encrypt(response.data.cod_usuario, this.clave).toString();
                    const token= CryptoJS.AES.encrypt(response.data.token, this.clave).toString();
                    Cookies.set('usuario', usuario);
                    Cookies.set('cod_usuario', cod_usuario);  
                    Cookies.set('token',token);
                    this.$router.push({name:'Dashboard'})
                    }else{
                        Swal.fire({
                            icon:'error',
                            title:'Error',
                            text:'usuario o contraseña incorrecto',
                            theme:'dark',
                            timer: 2000
                        })
                    }
                    
            } catch (error) {
                console.log(error); 
                alert('Error al iniciar sesión');
            }

        }
    },
    components:{
        FullScreenLayout
    },  

}
</script>


<template>
    <FullScreenLayout>
        <div class="relative min-h-screen flex items-center justify-center">
            <img src="@/assets/fondo.jpg" alt="" class="absolute inset-0 w-full h-full object-cover z-0 brightness-50">
                
        <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen lg:py-0">
            <div class="w-full  min-w-sm text-white  backdrop-blur z-20 rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0 bg-gray-900/90 dark:backdrop-invert dark:backdrop-opaccity-10 dark:border-gray-700 mx-auto ">
                <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
                    <h1 class="text-xl font-bold leading-tight tracking-tight c md:text-2xl text-center text-white">
                        Sistema de Inventario
                    </h1>
                    <form class="space-y-4 md:space-y-6" method="POST" action="#">
                        <div>
                            <label for="usuario" class="block mb-2 text-sm font-medium   text-white">Usuario</label>
                            <input type="text" name="usuario" id="usuario" v-model="usuario" class=" border text-md  rounded-lg  block w-full p-2.5 bg-gray-700 border-gray-600 placeholder-gray-400 text-white focus:outline focus:outline-gray-600 focus:border-gray-600 text-sm " placeholder="ingresa usuario" required="">
                        </div>
                        <div>
                            <label for="password" class="block mb-2 text-sm font-medium text-white">Contraseña</label>
                            <input type="password" name="password" id="password" v-model="pass" placeholder="••••••••" class=" border  rounded-lg  block w-full p-2.5 bg-gray-700 border-gray-600 placeholder-gray-400text-white focus:outline focus:border-gray-600 focus:outline-gray-600 text-white text-sm" required="">
                        </div>
                        <div class="flex items-center justify-between">
                        </div>
                        <button type="button" @click="login" class="w-full text-white bg-blue-600 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Ingresar</button>
                    </form>
                </div>
            </div>
        </div>
        
        </div>
        
    </FullScreenLayout>
</template>