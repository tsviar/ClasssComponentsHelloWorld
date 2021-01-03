<template>
    <div id=parent>
            
        <!-- @click == v-bind:click -->
        <v-btn color="primary" dark @click="close">
                Open Dialog
        </v-btn>
        <Modal :display="showDialog">
            <!-- 
                v-bind / connect parent funcs to child 
                Emitted mesages : close-dialog, open-dialog
                which calls close() save() respectively
            -->
            <Child @close-dialog="close" @open-dialog="save"/>
        </Modal>
         
    </div>

</template>

<script lang="ts">

import { Vue, Component, Prop, Emit, Watch } from "vue-property-decorator";
import Modal from "./Modal.vue";
import Child from "./Child.vue";


@Component({
    components:{
        Modal,
        Child
    }
})

export default class Parent extends Vue {
  
  // a value that will help the parent send data to the child
  private dialog: Boolean=false; //interface Boolean

   showDialog() {
        this.dialog = !this.dialog;            
   }

   close(value) {
       // getting false value from the child
       this.dialog = value;
   }

   save() {
       this.dialog = !this.dialog;
   }



}
</script>