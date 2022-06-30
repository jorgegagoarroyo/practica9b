<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-12 mt-2">
        <h1>Usuarios</h1>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <formulario-usuario @crear-usuario="postUsuario"/>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <tabla-usuarios :usuarios="usuarios" @eliminar-usuario="deleteUsuario" @actualizar-usuario="putUsuario"/>
      </div>
    </div>  
  </div>
</template>

<script>
import TablaUsuarios from '@/components/TablaUsuarios.vue'
import FormularioUsuario from '@/components/FormularioUsuario.vue'
  export default {
    name: 'app',
    data() {
      return{
        usuarios: []
      }
    },
    components:{
      TablaUsuarios,
      FormularioUsuario
    },
    methods: {
      async getUsuarios(){
        try {
          // Obtenemos los datos usando await
          const response = await  fetch('https://jsonplaceholder.typicode.com/users');
          // Respuesta en formato JSON
          this.usuarios = await response.json()
        } catch (error) {
          console.log(error)
        }
      },
      async postUsuario(usuario) {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users', {
            method: 'POST',
            body: JSON.stringify(usuario),
            headers: { 'Content-type': 'application/json; charset=UTF-8' },
          });
          
          const usuarioCreado = await response.json();
          this.usuarios = [...this.usuarios, usuarioCreado];
        } catch (error) {
          console.error(error);
        }
      },
      async putUsuario(usuario){
        try {
          const response = await fetch(`https://jsonplaceholder.typicode.com/users/${usuario.id}`,{
            method: 'PUT',
            body: JSON.stringify(usuario),
            headers: { 'content-type': 'application/json; charset=UFT-8'}
          });
          const usuarioAct = await response.json()
          this.usuarios = this.usuarios.map(u => (u.id === usuario.id ? usuarioAct : u))

        } catch (error) {
          console.log(error)
        }
      },
      async deleteUsuario(usuario){
        try {
          await fetch(`https://jsonplaceholder.typicode.com/users/${usuario.id}`, {
            method: 'DELETE'
          });

          this.usuarios = this.usuarios.filter(u => u.id !== usuario.id)
        } catch (error) {
          console.log(error)
        }
      }
    },
    mounted(){
      this.getUsuarios()
    }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
