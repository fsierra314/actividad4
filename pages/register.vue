    <template>
        <link href="https://fonts.googleapis.com/css2?family=Montserrat&display=swap" rel="stylesheet">
        <link href="https://fonts.googleapis.com/css2?family=Arial&display=swap" rel="stylesheet">
        <div class="background">
        <div class="container">
            <div  class="title-container" >
                <div class="text-subtitle-1 text-medium-emphasis"> <h1 class="main-title"> 
                    ¿QUE ESPERAS PARA CAMBIAR   <br> <br> TU VIDA?
                </h1></div>
                <br><br><br><br><br>
                <div class="text-subtitle-1 text-medium-emphasis"> <h1 class="main-text">
                ¿ESTAS LISTO PARA IR POR TU MEJOR VERSION?
                </h1></div>
                <br><br>
                <br><br><br>
                <div class="text-subtitle-1 text-medium-emphasis"> <h1 class="main-text">
                ¡ASUME EL RETO Y TRANSFORMA TU VIDA!  
                </h1></div>
            
            </div>
            <div class="login-container">
                
                <v-card class="mx-auto pa-12 pb-8" elevation="2" max-width="448" rounded="lg" color="" 
                :style="{ 'background-color': 'rgba(255, 255, 255, 0.603)' }">
                    <div class="text-subtitle-1 text-medium-emphasis">Nombre completo</div>

                    <v-text-field v-model="fullName" density="compact" placeholder="Nombre completo"
                        prepend-inner-icon="mdi-account-outline" variant="underlined"></v-text-field>

                    <div class="text-subtitle-1 text-medium-emphasis">Correo electrónico</div>

                    <v-text-field v-model="email" density="compact" :rules="emailRules" placeholder="Correo electrónico"
                        prepend-inner-icon="mdi-email-outline" variant="underlined"></v-text-field>

                    <div class="text-subtitle-1 text-medium-emphasis">Objetivo</div>

                    <v-select v-model="objective" :items="objectives" density="compact" dense placeholder="Selecciona tu objetivo" prepend-inner-icon="mdi-dumbbell" variant="underlined"></v-select>

                    <div class="text-subtitle-1 text-medium-emphasis">Fecha de Nacimiento</div>

                    <input v-model="birthdate" type="date" density="compact" class="v-text-field__input" prepend-inner-icon="mdi-calendar" variant="underlined">
                    <br><br>    
                    <div class="text-subtitle-1 text-medium-emphasis">Peso (kg)</div>

                    <v-text-field v-model="weight" dense placeholder="Peso en kilogramos" density="compact" variant="underlined" prepend-inner-icon="mdi-run"></v-text-field>

                    <div class="text-subtitle-1 text-medium-emphasis">Genero</div>

                    <v-select v-model="gender" :items="genders" density="compact" dense placeholder="Selecciona tu genero" prepend-inner-icon="mdi-human-female" variant="underlined"></v-select>    

                    <div class="text-subtitle-1 text-medium-emphasis">Contraseña</div>

                    <v-text-field v-model="password" :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
                        :type="visible ? 'text' : 'password'" density="compact" placeholder="Ingresa tu contraseña"
                        prepend-inner-icon="mdi-lock-outline" variant="underlined"
                        @click:append-inner="visible = !visible"></v-text-field>

                    <div class="text-subtitle-1 text-medium-emphasis">Confirmar Contraseña</div>

                    <v-text-field v-model="confirmPassword" :append-inner-icon="confirmVisible ? 'mdi-eye-off' : 'mdi-eye'"
                        :type="confirmVisible ? 'text' : 'password'" density="compact" placeholder="Confirma tu contraseña"
                        prepend-inner-icon="mdi-lock-outline" variant="underlined"
                        @click:append-inner="confirmVisible = !confirmVisible"></v-text-field>

                    <v-btn block class="mb-8" color="yellow" size="large"  @click="register">
                        Registrarse
                    </v-btn>
                    <v-card-text class="text-center">
                        <NuxtLink class="text-black text-decoration-none" to="/login" rel="noopener noreferrer">
                        ¿Ya estás registrado?    Inicia sesión <v-icon icon="mdi-chevron-right"></v-icon>
                        </NuxtLink>
                    </v-card-text>
                </v-card>
            </div>
        </div> 
    </div>   
    </template>
    
    <script>
    import axios from 'axios';
    import Swal from "sweetalert2";
    let nextUserId = 2;  // Contador para el ID secuencial

    export default {
        data() {
            return {
                fullName: '',
                email: '',
                objective: '',
                objectives: ['Bajar', 'Aumentar masa','Definir','Mantenerse','Resistencia','Lesión'],
                birthdate: '',
                weight: '',
                gender : '',
                genders: ['Masculino', 'Femenino','Prefiero no contestar'],
                password: '',
                confirmPassword: '',
                visible: false,
                confirmVisible: false,
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
                async register() {
                    await this.getUsers();

                    // Check if the email already exists
                    const emailExists = this.users.some(user => user.email === this.email);

                    if (emailExists) {
                        console.error('El correo electrónico ya está registrado.');
                    }
                    else if (this.password !== this.confirmPassword) {
                        Swal.fire({
                            title: "Error",
                            text: "Las contraseñas no coinciden",
                            icon: 'error',
                        })
                    }else if (!this.objective){
                        Swal.fire({
                            title: "Error",
                            text: "Por favor, selecciona tu objetivo",
                            icon: 'error',
                        })
                    }else if(!this.birthdate){
                        Swal.fire({
                            title: "Error",
                            text: "Por favor, selecciona tu fecha de nacimiento",
                            icon: 'error',
                        });
                    }else if(!this.weight){
                        Swal.fire({
                            title: "Error",
                            text: "Por favor, ingresa tu peso",
                            icon: 'error',                        
                        })
                    }else if(!this.gender){
                        Swal.fire({
                            title: "Error",
                            text: "Por favor, selecciona tu genero",
                            icon: 'error',                        
                        })
                    }else {  
                    // Generate a unique sequential ID for the new user
                    const userId = nextUserId;

                    // Increment the counter for the next user
                    nextUserId++;

                    // Register the new user with the generated sequential ID
                    const newUser = {
                        id: userId,
                        fullName: this.fullName,
                        email: this.email,
                        password: this.password
                    };

                    // Add the user to the server
                    await this.addUser(newUser);

                    Swal.fire(
                        {
                            icon: 'success',
                            title: 'Registro exitoso:'
                        }
                    )

                    console.log('', newUser);
                    // Optionally, you can redirect to a different page after successful registration
                    this.$router.push('./');
                }
            },
            async addUser(user) {
                try {
                    const response = await axios.post('http://localhost:3001/users', user);
                    console.log('Usuario agregado:', response.data);
                } catch (error) {
                    console.error('Error al agregar usuario:', error);
                }
            }
        }
    };
    definePageMeta({
        layout: "blank",
    });
    </script>
    
    <style scoped>
    .background {
    background-image: url('https://t2.uc.ltmcdn.com/es/posts/1/0/4/que_es_el_fitness_y_sus_beneficios_52401_orig.jpg');  /* Ajusta la URL según la ubicación de tu imagen */
    background-size: cover; 
    background-position: center;  
    height: 150vh; 
    }
    .main-text{
    text-align: center;        
    font-size: 27px;           
    color: white;  
    font-weight: 400;            
    font-family: 'Arial', sans-serif;  
    }
    .container{
        display: flex;
        justify-content: space-between; 
        align-items: center; /* Centra los contenedores verticalmente */
        height: 100vh;
    }
    .title-container{
        flex: 1; /* Toma el 50% del ancho disponible */
        justify-content: space-between; /* Esto separará los dos contenedores */
        align-items: center; /* Centra los contenedores verticalmente */
        height: 100vh;
    }
    .login-container {
        flex: 1; /* Toma el 50% del ancho disponible */
        height: 90vh;
    }

    .login-container v-card {
        width: 100%;
        /* Ajusta el ancho de la tarjeta al 100% del contenedor */
    }
    .main-title {
    text-align: center;        /* Centra el texto */
    font-size: 40px;            /* Tamaño de la fuente */
    font-weight: bold;          /* Hace que el texto sea en negrita */
    color: white;               /* Color del texto */
    margin-top: 100px;           /* Margen en la parte superior */
    margin-bottom: 10px; 
    font-family: 'Montserrat', sans-serif;       /* Margen en la parte inferior */
    }
    </style>