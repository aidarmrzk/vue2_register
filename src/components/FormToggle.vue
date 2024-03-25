<template>
  <div>
    <input
      type="checkbox"
      id="toggle"
      class="checkbox"
      :checked="isChecked"
      @change="$emit('input', $event.target.checked)"
    />  
    <label for="toggle" class="switch"></label>
  </div>
</template>

<script>
export default {
  name: "FormToggle",

  props: {
    value: {
      type: Boolean,
      required: true
    }
  },

  computed: {
    isChecked() {
      return this.value;
    }
  }
}
</script>

<style scoped lang="scss">
.switch {
  position: relative;
  display: inline-block;
  width: 40px;
  height: 20px;
  background-color: rgba(0, 0, 0, 0.25);
  border-radius: 20px;
  transition: all 0.3s;
  cursor: pointer;
  &::after {
    content: '';
    position: absolute;
    width: 18px;
    height: 18px;
    border-radius: 50%;
    background-color: white;
    border: 0.5px solid #CDCED8;
    top: 0;
    left: 0;
    transition: all 0.3s;
  }
}
.checkbox:checked + .switch {
  background-color: #3586FF;

  &::after {
    left: auto;
    right: 0;
  }
}
.checkbox {
  display: none;
}
</style>