<template>
 <link href="https://fonts.googleapis.com/css2?family=TimesNewRoman&display=swap" rel="stylesheet">
  <div class="background">
    <div class="title-container">
      <v-img class="logo" max-width="100"
      src=""></v-img>
      <h1 class="main-title">My Fit Space</h1>
    </div>
    <div class="regis-container">
    <v-btn class="mb-8" color="yellow" type="submit" width="180" height="50">
    <NuxtLink class="text-black text-decoration-none" to="/register" >
      REGISTRARSE
    </NuxtLink>
    </v-btn>
    <v-card-text class="text-center link-container">
      <NuxtLink class="text-white text-decoration-none" to="/login" rel="noopener noreferrer">
        ¿Ya estás registrado? --> Iniciar sesión <v-icon icon="mdi-chevron-right"></v-icon>
      </NuxtLink>
    </v-card-text>
    </div>
  </div>
</template>

<script>
definePageMeta({
  layout: "blank",
});
</script>

<style scoped>
.title-container {
  text-align: center;
  padding: 10px;
  display: flex;               /* Utiliza flexbox */
  align-items: center;         /* Alinea los elementos verticalmente en el centro */
  justify-content: center;
}
.background {
  background-image: url('https://t2.uc.ltmcdn.com/es/posts/1/0/4/que_es_el_fitness_y_sus_beneficios_52401_orig.jpg');  /* Ajusta la URL según la ubicación de tu imagen */
  background-size: cover; 
  background-position: center;  /* Centra la imagen en el contenedor */
  height: 120vh; 
}
.main-title {
  text-align: center;        /* Centra el texto */
  font-size: 40px;            /* Tamaño de la fuente */
  font-weight: bold;          /* Hace que el texto sea en negrita */
  color: white;               /* Color del texto */
  margin-top: 10px;           /* Margen en la parte superior */
  margin-bottom: 10px; 
  font-family: 'Montserrat', sans-serif;      /* Margen en la parte inferior */
}
.regis-container {
  margin-top: 250px;           /* Margen en la parte superior */
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;       /* Los elementos se apilan verticalmente */
  justify-content: center;     /* Centrado vertical */
  align-items: center;         /* Centrado horizontal */
}
.link-container {
  text-align: center;
  width: 100%;
} 
.logo{
  margin-right: 10px;          
  /*background-color: rgba(99, 80, 80, 0.6); */
}
</style>