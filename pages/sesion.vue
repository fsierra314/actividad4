<template>
  <div class="background">
    <div class="title-container">
      <h1 class="main-title">¡BIENVENIDO!, ESTE ES TU PLAN</h1>
    </div>
    <div class="plans">
      <div
        class="plan-container"
        v-for="(plan, index) in plans"
        :key="index"
      >
        <p v-if="!plan.isEditing">{{ plan.text }}</p>
        <input v-else v-model="plan.editedText" @blur="saveText(index)" @keyup.enter="saveText(index)" />
        <v-btn block class="mb-8" color="yellow" size="large" type="submit" @click="toggleEditing(index)">
          <v-icon>
            {{ plan.isEditing ? 'mdi-content-save' : 'mdi-pencil' }}
          </v-icon>
          {{ plan.isEditing ? 'Guardar' : 'Editar' }}
        </v-btn>
      </div>
    </div>
    <v-img class="logo" max-width="100" src=""></v-img>
  </div>
</template>

<script>
import Swal from 'sweetalert2';

export default {
  data() {
    return {
      plans: [
        { text: "Mucha proteina, cardio y pesas", editedText: "", isEditing: false },
        { text: "0 grasas, 0 Carbohidratos Solo cardio ", editedText: "", isEditing: false },
        { text: "0 grasas, 0 Carbohidratos Solo cardio", editedText: "", isEditing: false },
        { text: "0 grasas, 0 Carbohidratos Solo cardio", editedText: "", isEditing: false },
        { text: "0 grasas, 0 Carbohidratos Solo cardio", editedText: "", isEditing: false },
        { text: "0 grasas, 0 Carbohidratos Solo cardio", editedText: "", isEditing: false },
      ],
    };
  },
  methods: {
    async toggleEditing(index) {
      if (this.plans[index].isEditing) {
        const { isConfirmed } = await Swal.fire({
          title: '¿Estás seguro de editar este plan?',
          icon: 'question',
          showCancelButton: true,
          confirmButtonText: 'Sí',
          cancelButtonText: 'No',
        });

        if (!isConfirmed) {
          return; // Si no se confirma la edición, no cambies el modo de edición
        }
      }

      this.plans[index].isEditing = !this.plans[index].isEditing;
      if (this.plans[index].isEditing) {
        this.plans[index].editedText = this.plans[index].text;
      }
    },
    saveText(index) {
      this.plans[index].isEditing = false;
      this.plans[index].text = this.plans[index].editedText;
    },
  },
};
definePageMeta({
    layout: "blank",
  });
</script>

<style scoped>
.plans {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 columnas */
  grid-template-rows: repeat(2, 1fr);    /* 2 filas */
  gap: 20px; /* Espacio entre los recuadros de los planes */
}
.plan-container {
  border: 1px solid #000000;
  padding: 10px;
  text-align: center;
}
.title-container {
  text-align: center;
  padding: 10px;
  display: flex;               /* Utiliza flexbox */
  align-items: center;         /* Alinea los elementos verticalmente en el centro */
  justify-content: center;
}
.background {
  background-image: url('https://image.jimcdn.com/app/cms/image/transf/none/path/s2752d2e2017e8bd9/image/i0dcfda564d8b4ff4/version/1558430526/image.jpg');  /* Ajusta la URL según la ubicación de tu imagen */
  background-size: cover; 
  background-position: center;  /* Centra la imagen en el contenedor */
  height: 120vh; 
}
.main-title {
  text-align: center;        /* Centra el texto */
  font-size: 40px;            /* Tamaño de la fuente */
  font-weight: bold;          /* Hace que el texto sea en negrita */
  color: rgb(0, 0, 0);               /* Color del texto */
  margin-top: 10px;           /* Margen en la parte superior */
  margin-bottom: 10px; 
  font-family: 'Montserrat', sans-serif;      /* Margen en la parte inferior */
}
</style>