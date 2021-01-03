<template>
    <v-card class=" promary white--text dark">
        <br/>
        <v-card-text class="white--text center">
           So this is how @Prop and @Emit work. Awesome!
        </v-card-text>

        <v-card-actions>  
            <v-spacer></v-spacer>
            
            <!-- @click == v-bind:click -->
           <!-- on click calls @Emit close() method-->
            <v-btn color="accent" small dark @click="close">
                  Close
            </v-btn>
           
            <!-- on click calls @Emit open() method-->
            <v-btn color="accent" small dark  @click="open">
                  Open
            </v-btn>



        </v-card-actions>  

    </v-card> 
</template>

<script lang="ts">
import { Vue, Component, Prop, Emit, Watch } from "vue-property-decorator";

@Component({
    
})

export default class Child extends Vue {

    // Prop recieded from parent
    @Prop() public display!: boolean;

    // computed method for display
    get showDisplay() {
        return this.display;
    }

   // child passing data to parent component

    // Emits 'close-dialog' msg 
    // with value=true returned by close()

    @Emit("close-dialog")
    close() {
        return false;
    }

     // Emits 'open-dialog' msg 
    // with value=true returned by open()
    
    @Emit('open-dialog')
    open() {
        return true;
    }

}

</script>