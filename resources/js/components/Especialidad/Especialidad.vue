<template>
  <div>
    <h1 class="text-center">Gestionar especialidads</h1>
    <hr />

    <!-- Button to Open the Modal -->
    <button @click="modificar=false; abrirModal();" type="button" class="btn btn-primary my-4">Nuevo</button>

    <!-- The Modal -->
    <div class="modal" :class="{mostrar: modal}">
      <div class="modal-dialog">
        <div class="modal-content">
          <!-- Modal Header -->
          <div class="modal-header">
            <h4 class="modal-title">{{tituloModal}}</h4>
            <button @click="cerrarModal();"  type="button" class="close" data-dismiss="modal">
              &times;
            </button>
          </div>

          <!-- Modal body -->
          <div class="modal-body">
            <div class="my-4">
              <label for="nombre">Nombre</label>
              <input v-model="especialidad.nombre" type="text" class="form-control" id="nombre" placeholder="Nombre de la Especialidad">
            </div>
            <div class="my-4">
              <label for="descripcion">descripcion</label>
              <input v-model="especialidad.descripcion" type="text" class="form-control" id="descripcion" placeholder="Descripcion de la Especialidad">
            </div>

          <!-- Modal footer -->
          <div class="modal-footer">
            <button @click="cerrarModal();" type="button" class="btn btn-secondary" data-dismiss="modal">
              Cancelar
            </button>
            <button  @click="guardar();" type="button" class="btn btn-success" data-dismiss="modal">
              Guardar
            </button>


          </div>
        </div>
      </div>
    </div>

    <table class="table table-striped">
      <thead class="thead-dark">
        <tr>
          <th scope="col">#</th>
          <th scope="col">Nombre</th>
          <th scope="col">Descripcion</th>
          <th scope="col" colspan="2" class="text-center">Accion</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="espe in especialidades" :key="espe.id">
          <th scope="row">{{ espe.id }}</th>
          <td>{{ espe.nombre }}</td>
          <td>{{ espe.descripcion }}</td>
          <td>
            <button  @click="modificar=true; abrirModal(espe);" class="btn btn-warning">Editar</button>
          </td>
          <td>
            <button @click="eliminar(espe.id)" class="btn btn-danger">
              Eliminar
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      especialidad:{
        nombre:'yy',
        descripcion:'yuu',
      },
      id:0,
      modificar:true,
      modal:0,
      tituloModal:'',
      especialidades: [],
    };
  },
  methods: {
    async listar() {
      const res = await axios.get('/especialidades');
      this.especialidades = res.data;
    },
    async eliminar(id) {
      const res = await axios.delete('/especialidades/' + id);
      this.listar();
    },
    async guardar() {
      if(this.modificar){
        const res = await axios.put('/especialidades/'+this.id, this.especialidad);
        // console.log(this.id);

      }else{
        const res = await axios.post('/especialidades/', this.especialidad);
      }
      this.cerrarModal();
      this.listar();
    },
    abrirModal(data={}){
      this.modal=1;
      if(this.modificar){
        this.id=data.id;
        this.tituloModal="Modificar Especialidad";
        this.especialidad.nombre=data.nombre;
        this.especialidad.descripcion=data.descripcion;
      }else{
        this.id=0;
        this.tituloModal="Crear Especialidad";
        this.especialidad.nombre='';
        this.especialidad.descripcion='';
      }
    },
    cerrarModal(){
      this.modal=0;
    },
  },
  created() {
    this.listar();
  },
};
</script>

<style>
  .mostrar{
    display: list-item;
    opacity: 1;
    background: rgba(75, 56, 143, 0.705);
  }
</style>