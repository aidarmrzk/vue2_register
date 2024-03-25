<template>
    <select @change="updateValue($event.target.value)" :class="{ 'custom-option': isCustomOption }">
    <option value="" disabled selected hidden class="placeholder-option">{{ placeholder }}</option>
    <option
        v-for="option in options"
        :key="option.value"
        :value="option.value"
    >{{ option.text }}</option>
    </select>
</template>

<script>
export default {
    name: "FormSelect",

    data() {
        return {
            isCustomOption: true,
        }
    },

    props: {
        value: [String, Number],
        placeholder: {
            type: String,
            default: ''
        },
        options: {
            type: Array,
            required: true
        },
    },

    methods: {
        updateValue(newValue) {
            this.isCustomOption = newValue === '';
            
            const parsedValue = parseInt(newValue, 10);
            this.$emit('input', parsedValue);
        },
    },
}
</script>

<style scoped lang="scss">
select {
    padding: 10px;
    width: 100%;
    border-radius: 10px;
    border: 1px solid #E6E6EB;
    font-size: 14px;
    font-weight: 400;
    line-height: 19px;
    color: black;
    moz-appearance: none;
	-webkit-appearance: none;
    background: url("@/assets/carret.png") no-repeat right center;
    background-position: right 10px center;
    background-color: #fff;
    cursor: pointer;
    outline: none;
    &.custom-option {
        color: #9292A0;
    }
    &:focus {
        border-color: #3586FF;
    }
    option {
        cursor: pointer;
    }
}


</style>
