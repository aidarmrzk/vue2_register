<template>
<div>
    <label class="label__checkbox">
        <input
            class="input__checkbox"
            type="checkbox"
            :checked="value"
            @change="$emit('input', $event.target.checked)"
        >
        <span class="custom-checkbox"></span>
    </label>
</div>
</template>

<script>
export default {
name: "FormCheckbox",

props: {
    value: {
    type: Boolean,
    required: true
    }
},

}
</script>

<style scoped lang="scss">
.label__checkbox {
  position: relative;
}

.input__checkbox {
  position: absolute;
  width: 12px;
  height: 11px;
  opacity: 0;
  z-index: 1;
  cursor: pointer;
}

.custom-checkbox {
  position: relative;
  display: inline-block;
  width: 19px;
  height: 17px;
  background-color: #fff;
  border: 1px solid #D9D9D9;
  border-radius: 5px;
  transition: all 0.3s;
  &::after {
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 9px;
    height: 7px;
    background-image: url(@/assets/vector.svg);
    background-size: cover;
    visibility: hidden;
  }
}

.input__checkbox:checked + .custom-checkbox {
  background-color: #3586FF;
  border: 1px solid #3586FF;

  &::after {
    visibility: visible;
  }
}
</style>