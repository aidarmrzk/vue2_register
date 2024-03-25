<template>
<div class="success">
    <div class="success__popup">
        <svg width="80px" height="80px" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <g>
                <path fill="none" d="M0 0h24v24H0z"/>
                <path style="fill: green;" d="M12 22C6.477 22 2 17.523 2 12S6.477 2 12 2s10 4.477 10 10-4.477 10-10 10zm-.997-6l7.07-7.071-1.414-1.414-5.656 5.657-2.829-2.829-1.414 1.414L11.003 16z"/>
            </g>
        </svg>
        <h3>Регистрация прошла успешно</h3>
    </div>
</div>
</template>

<script>
export default {
    name: "SuccessComponent",

    // props: {
    //     placeholder: {
    //         type: String,
    //         default: ''
    //     },
    //     type: {
    //         type: String,
    //         default: 'text'
    //     },
    // },
}
</script>

<style scoped lang="scss">
.success {
    position: fixed;
    top: 0;
    left: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
    background-color: #000000d9;
    .success__popup {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 20px 50px;
        background-color: #fff;
        border-radius: 15px;
        box-shadow: 0 0 8px 0 rgba(0,0,0,.3);
        h3 {
            font-size: 16px;
            font-weight: 500;
            line-height: 19px;
        }
    }
}
</style>