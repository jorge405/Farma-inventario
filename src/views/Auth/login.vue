<script>
import router from '@/router';
import axios from 'axios';
import Swal from 'sweetalert2';
import Cookies from 'js-cookie';
import CryptoJS from 'crypto-js';
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
                    Cookies.set('usuario', usuario);
                    Cookies.set('cod_usuario', cod_usuario);  
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
        
    },  

}
</script>


<template>
    <FullScreenLayout>
        <section class="bg-gray-50 dark:bg-gray-900">
  <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen lg:py-0">
      <div class="w-full bg-white rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0 dark:bg-gray-800 dark:border-gray-700">
          <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
              <h1 class="text-xl font-bold leading-tight tracking-tight text-gray-900 md:text-2xl text-center dark:text-white">
                  Sistema de Inventario
              </h1>
              <form class="space-y-4 md:space-y-6" method="POST" action="#">
                  <div>
                      <label for="usuario" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Usuario</label>
                      <input type="text" name="usuario" id="usuario" v-model="usuario" class="bg-gray-50 border border-gray-300 text-gray-900 rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="ingresa usuario" required="">
                  </div>
                  <div>
                      <label for="password" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Contraseña</label>
                      <input type="password" name="password" id="password" v-model="pass" placeholder="••••••••" class="bg-gray-50 border border-gray-300 text-gray-900 rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required="">
                  </div>
                  <div class="flex items-center justify-between">
                  </div>
                  <button type="button" @click="login" class="w-full text-white bg-blue-600 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Ingresar</button>
              </form>
          </div>
      </div>
  </div>
</section>
    </FullScreenLayout>
</template>