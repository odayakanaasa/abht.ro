<template>
  <div class="form-group">
    <label> {{ step.name }} </label>
    <br />

    <button type="text"
    @click.prevent = "onClick(currentStep)"
    :class="statusClass"
    :disabled = "isDisabled"
    >

  {{ step.label }} </button>&nbsp;
  </div>
</template>


<script>
export default{
  props: ['formState', 'currentStep'],

  data() {
    return {

    }
  },

  methods: {
    onClick(data){
      this.$emit('showFormStep', data)
    }
  },

  computed: {

    isDisabled(){
      let item = this.formState.steps[this.currentStep]
      let canBeClicked = false
      if( this.currentStep > 0 ){ // check if previous step exists and is valid, then item can be clicked
        let previousItem = this.formState.steps[this.currentStep -1]
        canBeClicked = previousItem.status == 'valid'
      }else{
        canBeClicked = item.status == 'valid'
      }
      return !canBeClicked || this.currentStep == this.formState.currentIndex
    },

    statusClass(){
      let btnClass = {btn: true}
      btnClass['btn-primary'] = this.formState.currentIndex == this.currentStep
      btnClass['btn-danger'] = this.step.status == 'invalid'
      btnClass['btn-success'] = !this.isDisabled

      return btnClass

      //return {currentStep: this.formState.currentIndex == this.currentStep, invalidStep: this.step.status == 'invalid' }
    },

    step(){
      return this.formState.steps[this.currentStep]
    }
  },

  components: {

  },


}
</script>


<style scoped>
.currentStep{
  border: 1px solid blue;
}

.invalidStep{
  background-color: red;
}
</style>
