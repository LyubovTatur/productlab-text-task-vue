<template>
    <input 
        type='text'
        v-bind:value='inputValue' 
        v-on:input='inputChangeHandler' 
        placeholder='Оставьте комментарий'  
        style='display:block;'/>
    <button v-on:click="showInput">Отправить</button>
</template>
