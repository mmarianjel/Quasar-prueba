<template>
  <div class="q-pa-md q-gutter-sm">
<q-editor
      v-model="editor"
      :definitions="{
        save: {
          tip: 'Guardar task',
          icon: 'save',
          label: 'Guardar',
          handler: saveWork
        }
      }"
      :toolbar="[
        ['bold', 'italic', 'strike', 'underline'],
        ['upload', 'save']
      ]"
    />

    <q-card class="row" flat bordered v-for="(item, index) in task" :key="index">
      <q-card-section  :class="item.estado ? 'tachar':''" 
       class="col" v-html="item.texto" />
      <q-btn flat color="blue" @click="item.estado = !item.estado">Guardar</q-btn>
      <q-btn flat color="red" @click="eliminar(index)">Eliminar</q-btn>
    </q-card>

    <div class="flex flex-center" v-if="task.length==0">
      <h6>Sin notas</h6>
    </div>
  </div>
</template>

<script>
export default {

 data() {
   return {
      editor: '',
      task:[
        {texto:"texto #1",estado:false},
        //{texto:"texto #2",estado:true},
        //{texto:"texto #3",estado:false},
      ]
   }
 },
 methods: {
    saveWork () {
      this.task.push({
        texto:this.editor,
        estado:false
      }),
      this.$q.notify({
        message: 'Tarea Guardada',
        color: 'green-4',
        textColor: 'white',
        icon: 'cloud_done'
      })
    },
    eliminar(index){
      this.$q.dialog({
        title: 'Confirmar accion!',
        message: 'Realmente desea eliminar?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.task.splice(index,1);
      })
    },
  }
 }
</script>
<style lang="stylus">
.tachar{
  text-decoration:line-through;
}
</style>