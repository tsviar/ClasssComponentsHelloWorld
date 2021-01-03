<template>
  <div id="app">
    <div>
      <img alt="Vue logo" src="./assets/logo.png" />
      <HelloWorld msg="Welcome to Your Vue.js + TypeScript App" />
    </div>

    <div class="box">
      <p><strong>Basic Toggle Button</strong></p>
      <ToggleButton />
    </div>

    <div class="box">
      <p><strong>Another Toggle Button with default state</strong></p>
      <ToggleButton idIn="secondary" defaultState="true" />
    </div>

    <div class="box">
      <p><strong>Handle Event</strong></p>
      <div :class="{ active: isActive }" class="toggle_container">
        <!-- 
              on button sending a 'change' event
              by calling: this.$emit('change', newValue);
               call triggerEvent(newValue) 
         -->

  
          <!-- 
                v-bind / connect parent funcs to child 
                Emitted mesage : toggledState
                which calls onChildChange()  respectively
            -->
      
        <ToggleButton idIn="event_handle" @toggledState="onChildChange" />

    <!-- <Child @close-dialog="close" @open-dialog="save"/> -->
    <!-- <ToggleButton idIn="event_handle" v-on:change="triggerEvent" />  -->
    <!-- <ToggleButton idIn="event_handle" @change="order = $event"></ToggleButton> -->

      </div>
    </div>
    <div class="box">
      <p><strong>Custom Label Example</strong></p>
      <ToggleButton
        idIn="label_example"
        labelEnableText="True"
        labelDisableText="False"
      />
    </div>
    <div class="box">
      <p><strong>Disabled Example</strong></p>
      <ToggleButton idIn="disabled_example" :disabled="true" />
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Emit, Vue, Watch } from "vue-property-decorator";
import HelloWorld from "./components/HelloWorld.vue";
import ToggleButton from "./components/ToggleButton.vue";

// HelloWorld class will be a Vue component
@Component({
  components: {
    HelloWorld,
    ToggleButton
  }
})
export default class App extends Vue {

  // Declared as component data
  // @Prop(config} name: type
  // @Prop({default:false}) 
  //type is inferred when a property is assigned (or boolean or false)
  protected active = true;
 
//  old way:
//   data() {
//     return {
//       active: false
//     }
//   },

  get isActive(): boolean {
    console.log(`App isActive this.active ${this.active}`); 
    return this.active;
  }

  // declare as component method
  //============================
  
  onChildChange(value:boolean) {
     console.log(`App onChildChange Emitted this.active ${this.active}`); 
       this.active = value;  
  }
  

  // old way:
  // methods: {
  //   triggerEvent(value) {
  //     this.active = value;
  //   }
  // }

  // @Watch('change')
  //   toggleChanged(newValue: boolean) {
  //     // eslint-disable-next-line no-console
  //     this.active = newValue; 
  //     console.log(`App Watch('change') ${newValue}`);
  //   }


  mounted() {
    this.active = false;
    console.log(`App mounted `);
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
