<script setup>
import ImageList from '@/components/ImageList.vue';
import CommAddForm from '@/components/CommAddForm.vue';
</script>
<script >
export default{
    data(){
      return{
        counter:0,
        inputValue:'ьдуь',
        images:[]
      }
    },
    methods:{
      showInput(){
        console.log('mlem')
      },
      inputChangeHandler(){
        console.log('mlem')
      },
      getImages(){
        fetch('https://boiling-refuge-66454.herokuapp.com/images')
        .then((response) => {
    return response.json();
  })
  .then((data) => {
    this.images=data;
  });
      }
    } 
  }
</script>

<template>
  <main>
    <button v-on:click='getImages'>Загрузить картинки</button>
    <ul v-if="images.length">
      <li v-for='image in images'>
        <img v-bind:src='image.url'/>
      </li>
    </ul>
   
  </main>
</template>
  
<style>
@import './assets/base.css';

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

</style>
