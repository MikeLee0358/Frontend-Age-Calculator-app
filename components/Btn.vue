<template lang="pug">
div
    button#Btn 
        #Btn__Left
        #Btn__Middle 
        #Btn__Right
</template>   

<style lang="sass">
#Btn
    position: relative
    background-color: var(--purple)
    width: 57.5px
    height: 57.5px
    border-radius: 50px
    border-style: none
    &:hover
        background-color: var(--black)
        & #Btn__Left::after, & #Btn__Right::after
            background: var(--black)
    &__Left 
        position: absolute
        left: 26%
        top: 51%
        width: 15px
        height: 15px
        border-radius: 0 100px 0 0
        background-color: var(--white)
        &::after
            content: ""
            position: absolute
            inset: 10% 10% 0% 0%
            background-color: var(--purple)
            border-radius: 0 100px 0 0
    &__Middle
        position: absolute
        left: 50%
        top: 25%
        width: 1.75px
        height: 29px   
        background: var(--white)
    &__Right
        position: absolute
        left: 50%
        top: 51%
        width: 15px
        height: 15px
        border-radius: 100px 0 0 0
        background-color: var(--white)
        &::after
            content: ""
            position: absolute
            inset: 10% 0% 0% 10%
            background-color: var(--purple)
            border-radius: 100px 0 0 0






</style>