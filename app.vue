<script setup>
const boxDD = ref('')
const boxMM = ref('')
const boxYY = ref('')

const inputDD = ref('')
const inputMM = ref('')
const inputYY = ref('')

const outputDD = ref('--')
const outputMM = ref('--')
const outputYY = ref('--')

const textHint = reactive({
    empty: 'This field is required',
    DD: 'Must be a valid Day',
    MM: 'Must be a valid Month',
    YY: 'Must be a in the past'
})

watchEffect(() => {
    const regexDD = /^[1-9]$|^[1-2][0-9]$|^3[0-1]$/;
    const regexMM = /^(1[0-2]|[0-9])$/;
    const regexYY = /^[0-9]+$/;

    if (inputDD.value === '') boxDD.value = textHint.empty
    else if (regexDD.test(inputDD.value) === false) boxDD.value = textHint.DD

    if (inputMM.value === '') boxMM.value = textHint.empty
    else if (regexMM.test(inputMM.value) === false) boxMM.value = textHint.MM

    if (inputYY.value === '') boxYY.value = textHint.empty
    else if (regexYY.test(inputYY.value) === false) boxYY.value = textHint.YY

})



function algorithm() {
    // I did the origin algorithm and let ChatGpt optimized it 
    const currentDate = new Date();
    const inputDate = new Date(Number(inputYY.value), Number(inputMM.value) - 1, Number(inputDD.value));
    const output = [outputYY, outputMM, outputDD];

    if (isNaN(inputDate.getTime()) || inputDate > currentDate) {
        output.forEach((el) => (el.value = '--'));
        return;
    }

    const diff = currentDate - inputDate;
    const yearsDiff = Math.floor(diff / (1000 * 60 * 60 * 24 * 365));
    const monthsDiff = Math.floor((diff % (1000 * 60 * 60 * 24 * 365)) / (1000 * 60 * 60 * 24 * 30));
    const daysDiff = Math.floor((diff % (1000 * 60 * 60 * 24 * 30)) / (1000 * 60 * 60 * 24));

    [outputYY.value, outputMM.value, outputDD.value] = [yearsDiff, monthsDiff, daysDiff];
}

</script>

<template lang="pug">
main#Age
    form#Age__Form(@submit.prevent="algorithm")
        #Age__Form__Box-Input
            #Age__Form__Box-Input__Box
                input#Age__Form__Box-Input__Box__Day(placeholder="DD" required pattern = "^[1-9]$|^[1-2][0-9]$|^3[0-1]$" v-model="inputDD") 
                label#Age__Form__Box-Input__Box__Label(for="Age__Form__Box-Input__Box__Day") DAY
                label#Age__Form__Box-Input__Box__Hint(for="Age__Form__Box-Input__Box__Day") {{ boxDD }}
            #Age__Form__Box-Input__Box
                input#Age__Form__Box-Input__Box__Month(placeholder="MM" required pattern="^(1[0-2]|[0-9])$"  v-model="inputMM" )
                label#Age__Form__Box-Input__Box__Label(for="Age__Form__Box-Input__Box__Month") MONTH
                label#Age__Form__Box-Input__Box__Hint(for="Age__Form__Box-Input__Box__Month") {{ boxMM }}
            #Age__Form__Box-Input__Box
                input#Age__Form__Box-Input__Box__Year(placeholder="YY" required pattern="^[0-9]+$"  v-model="inputYY")
                label#Age__Form__Box-Input__Box__Label(for="Age__Form__Box-Input__Box__Year") YEAR
                label#Age__Form__Box-Input__Box__Hint(for="Age__Form__Box-Input__Box__Year") {{ boxYY }}
        #Age__Form__Box-Btn
            hr#Age__Form__Box-Btn__Hr
            Btn#Age__Form__Box-Btn__Btn
    #Age__Box-Display
        p#Age__Box-Display__Text
            strong#Age__Box-Display__Text__Number {{ outputYY }} 
            strong#Age__Box-Display__Text__String  years
        p#Age__Box-Display__Text
            strong#Age__Box-Display__Text__Number {{ outputMM }}
            strong#Age__Box-Display__Text__String  months
        p#Age__Box-Display__Text
            strong#Age__Box-Display__Text__Number {{ outputDD }} 
            strong#Age__Box-Display__Text__String  days
</template>

<style lang="sass">
#Age
    display: grid
    gap: 1rem
    padding: 1.5rem
    background-color: var(--white)
    border-radius: 15px 15px 45px 15px
    &__Form
        &__Box-Input
            display: flex
            gap: 1.5rem
            &__Box
                display: grid
                grid-auto-flow: row
                gap: 0.25rem
                max-width: 140px
                &__Day, &__Month, &__Year
                    // grid-row: 2/3
                    display: block
                    padding: 1rem
                    border-radius: 10px
                    border: 1px solid var(--light-grey)
                    width: min(100%, 140px)
                    font-size: 1.5rem
                    font-weight: 700
                    &:hover
                        cursor: pointer
                    &:focus
                        border-color: var(--purple)
                        caret-color: var(--purple)
                        outline: none
                    &:invalid
                        border: 1px solid red
                        & ~ #Age__Form__Box-Input__Box__Label,
                        & ~ #Age__Form__Box-Input__Box__Hint  
                            color: red
                    &:valid
                        & ~ #Age__Form__Box-Input__Box__Hint
                            color: transparent
                &__Label
                    grid-row: 1/2
                    font-size: 1rem
                    color: var(--smokey-grey)       
                &__Hint
                    font-size: .75rem
        &__Box-Btn
            &__Hr
                position: relative
                top: 31px
                border-color: var(--light-grey)
            &__Btn
                position: relative
                display: inline-block
                left: 40%
    &__Box-Display
        &__Text
            font-size: 3rem
            &__Number
                color: var(--purple)

@media (width > 600px) 
    #Age
        gap: 0
        &__Form__Box-Btn
                &__Hr
                    width: 550px
                &__Btn 
                    left: 90%
</style>
