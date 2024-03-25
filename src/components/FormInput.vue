<template>
<input
    v-bind="$attrs"
    :type="type"
    :placeholder="placeholder"
    @input="$emit('input', $event.target.value)"
>
</template>

<script>
export default {
    name: "FormInput",

    props: {
        placeholder: {
            type: String,
            default: ''
        },
        type: {
            type: String,
            default: 'text'
        },
    },
}
</script>

<style scoped lang="scss">
input {
    padding: 10px;
    width: 100%;
    border-radius: 10px;
    border: 1px solid #E6E6EB;
    font-size: 14px;
    font-weight: 400;
    line-height: 19px;
    outline: none;
    &::placeholder {
        color: #9292A0;
    }
    &:focus {
        border-color: #3586FF;
    }
}
</style>