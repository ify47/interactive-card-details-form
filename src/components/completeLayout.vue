<script setup>
import icon from '../assets/icon-complete.svg';

const iconComplete = icon


</script>

        
<template>
    <div class="complete">
        <img :src="iconComplete" alt="icon complete" />
        <h1>Thank you!</h1>
        <p>We've added your card details</p>
        <button>Continue</button>

    </div>
</template>


<style lang="scss" scoped>
.complete {
    margin: auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 22px;

    h1 {
        font-weight: 100;
        font-size: 1.5rem;
        letter-spacing: 2px;
        text-transform: uppercase;
    }

    p {
        color: hsl(279, 6%, 55%);
        margin-top: -5px;
    }

    button {
        width: 310px;
        font-size: 15px;
        letter-spacing: 1px;
        border-radius: 5px;
        padding: 11px;
        background-color: hsl(278, 68%, 11%);
        color: white;
        margin-top: 19px;
    }
}
</style>