<template>
  <!-- : is v-bind shorthand -->
  <dir>
    <h1>Toggle-button</h1>

    <label
      :for="idIn + '_button'"
      :class="{ active: isActive }"
      class="toggle__button"
    >
      <span v-if="isActive" class="toggle__label">{{ enableText }}</span>
      <span v-if="! isActive" class="toggle_label">{{ disableText }}</span>

        <!-- 
            1st way to emit messages to parent 
             on click calls @Emit checkedValue() method
            ============================================
        -->
        <!-- @check == v-bind:check -->
          
       <input type="checkbox" :id="idIn + '_button'" @click="toggeldCheck" />


        <!-- 
            2nd way to emit messages to parent 
            v-model + getters, setters
            ====================================
        -->
       <!-- 
           <input type="checkbox" :id="idIn + '_button'" 
            v-model="checkedValue" /> 
        -->
        
      <span class="toggle__switch"></span>
    </label>
  </dir>
</template>

<script lang="ts">
import { Vue, Component, Prop, Emit, Watch } from "vue-property-decorator";

@Component
export default class ToggleButton extends Vue {
  // Component properties, input from parent
  // Use @Prop decorator to define the class properties
  //----------------------------------------------------

  //Define the default value for the related properties.
  //add ! with property to fix the property initializer Error
  // @Prop(config} name: type
  
  @Prop({ default: false })
  disabled!: boolean;

  @Prop({ default: false })
  defaultState!: boolean;

  @Prop({ default: "On" })
  labelEnableText!: string;

  @Prop({ default: "off" })
  labelDisableText!: string;

  @Prop({ default: "primary" })
  idIn!: string;


  /**
   * Initial data / properties
   **/
  //======================================
  // instance properties
  //======================================
  currentState: boolean = this.defaultState;
  id_in: string = this.idIn;


 //=========================================
  //Computed Properties
  // Getters Setters
  //========================================

  get enableText(): string {
    return this.labelEnableText;
  }

  get disableText(): string {
    return this.labelDisableText;
  }

  get isActive(): boolean {
    return this.currentState;
  }
  //Which is similar to old:
  // computed: {
  //     isActive() {
  //        return this.currentState;
  //     }
  // }

  get checkedValue(): boolean {
    return this.currentState;
  }

 // arrives from: type: "checkbox" v-model="checkedValue" 
  set checkedValue(newValue: boolean) {
    this.currentState = newValue;  // change state
    console.log(`set checkedValue ${newValue} this.currentState ${this.currentState} `);
    // Passing toggledState message back to parent
    console.log(`set checkedValue  emit ${newValue}`);
    this.$emit('toggledState', newValue);  // send a 'change' event
    
  }

  // old style:
  // checkedValue: {
  //         get() {
  //             return this.currentState;
  //         },
  //         set(newValue) {
  //             this.currentState = newValue;
  //             this.$emit('change', newValue);
  //         }
  //     }


   // Child passing data to parent component

    // Emits 'toggledState' msg 
    // with this.currentState returned by close()

    @Emit("toggledState" )
    toggeldCheck(): boolean {
        this.currentState = !this.currentState;  // change state

        console.log(` Child @Emit toggeldCheck this.currentState ${this.currentState} `);

        return this.currentState;
    }

  //======================================
  // Watchers
  // @Watch(propertyString, config)
  //======================================

  @Watch("defaultState")
  onPropertyChanged() {
    this.currentState = this.defaultState;
    console.log(`onPropertyChanged this.defaultState ${this.defaultState}`);
  }

  // Which is similar to the former version:
  // watch: {
  //     defaultState: function defaultState() {
  //         this.currentState = Boolean(this.defaultState)
  //     }
  // }

  // no need for this, it is updated when props are updated
  // if there isn't an idIn prop, the default is set
  // @Watch('idIn')
  // onIdInhanged() {
  //       this.id_in = this.idIn
  //       console.log(`idIn ${this.idIn} id_in ${this.id_in}`)
  // }
}
</script>

<style scoped>
.toggle__button {
  vertical-align: middle;
  user-select: none;
  cursor: pointer;
}
.toggle__button input[type="checkbox"] {
  opacity: 0;
  position: absolute;
  width: 1px;
  height: 1px;
}
.toggle__button .toggle__switch {
  display: inline-block;
  height: 12px;
  border-radius: 6px;
  width: 40px;
  background: #bfcbd9;
  box-shadow: inset 0 0 1px #bfcbd9;
  position: relative;
  margin-left: 10px;
  transition: all 0.25s;
}
.toggle__button .toggle__switch::after,
.toggle__button .toggle__switch::before {
  content: "";
  position: absolute;
  display: block;
  height: 18px;
  width: 18px;
  border-radius: 50%;
  left: 0;
  top: -3px;
  transform: translateX(0);
  transition: all 0.25s cubic-bezier(0.5, -0.6, 0.5, 1.6);
}
.toggle__button .toggle__switch::after {
  background: #4d4d4d;
  box-shadow: 0 0 1px #666;
}
.toggle__button .toggle__switch::before {
  background: #4d4d4d;
  box-shadow: 0 0 0 3px rgba(0, 0, 0, 0.1);
  opacity: 0;
}
.active .toggle__switch {
  background: #adedcb;
  box-shadow: inset 0 0 1px #adedcb;
}
.active .toggle__switch::after,
.active .toggle__switch::before {
  transform: translateX(40px - 18px);
}
.active .toggle__switch::after {
  left: 23px;
  background: #53b883;
  box-shadow: 0 0 1px #53b883;
}
</style>
