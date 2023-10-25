<template>
  <link href="https://fonts.googleapis.com/css2?family=TimesNewRoman&display=swap" rel="stylesheet">
  <div class="background">
    <div class="login-container">
      <v-card class="mx-auto pa-12 pb-8" elevation="2" max-width="448" rounded="lg" color="" 
        :style="{ 'background-color': 'rgba(255, 255, 255, 0.603)' }">
        <div class="text-subtitle-1 text-medium-emphasis" style="color: #000000; font-weight: bold;">Accede a tu cuenta</div>
        <v-form fast-fail @submit.prevent @submit="login">
          <div class="text-subtitle-1 text-medium-emphasis text-white">Correo electrónico</div>
  
          <v-text-field v-model="email" density="compact" :rules="emailRules" placeholder="Correo electrónico"
            prepend-inner-icon="mdi-email-outline" variant="underlined"></v-text-field>
  
          <div class="text-subtitle-1 text-medium-emphasis d-flex align-center justify-space-between">
            Contraseña   
            
          </div>
  
          <v-text-field v-model="password" :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
            :type="visible ? 'text' : 'password'" density="compact" placeholder="Ingresa tu contraseña"
            prepend-inner-icon="mdi-lock-outline" variant="underlined"
            @click:append-inner="visible = !visible"></v-text-field>
          
          <v-btn block class="mb-8" color="yellow" size="large"  type="submit" >
            <NuxtLink class="text-black text-decoration-none" to="/sesion" >
            INICIAR SESION
            </NuxtLink>
          </v-btn>
          <a class="text-body-2 font-weight-regular text-black" href="#" rel="noopener noreferrer" target="_blank">
              ¿Olvidaste tu contraseña?</a>
        </v-form>
      </v-card>
    </div>
  </div> 
</template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        email: '',
        password: '',
        visible: false,
        users: [],
        emailRules: [
          value => {
            if (value) return true
            return 'El campo es obligatorio.'
          },
          value => {
            if (/[^[^@]+@[^@]+\.[a-zA-Z]{2,}$/.test(value)) return true
            return 'Correo no valido.'
          }
        ],
      };
    },
    methods: {
      async getUsers() {
        try {
          const response = await axios.get('http://localhost:3001/users');
          this.users = response.data;
        } catch (error) {
          console.error('Error al obtener usuarios:', error);
        }
      },
      async login() {
        await this.getUsers();
  
        const foundUser = this.users.find(
          user =>
            user.email === this.email && user.password === this.password
        );
  
        if (foundUser) {
          console.log('Inicio de sesión exitoso para el usuario:', foundUser);
          this.$router.push('/sesion');
        } else {
          console.error('Credenciales incorrectas. Inicio de sesión fallido.');
        }
      },
    },
  };
  definePageMeta({
    layout: "blank",
  });
  </script>
  
<style scoped>

.background {
  background-image: url('https://t2.uc.ltmcdn.com/es/posts/1/0/4/que_es_el_fitness_y_sus_beneficios_52401_orig.jpg');  /* Ajusta la URL según la ubicación de tu imagen */
  background-size: cover; 
  background-position: center;  /* Centra la imagen en el contenedor */
  height: 120vh; 
}
.login-container {
  display: flex;
  flex-direction: column; 
  justify-content: center;
  align-items: center;
  height: 100vh;
}
  
.login-container v-card {
  width: 100%;
}
</style>