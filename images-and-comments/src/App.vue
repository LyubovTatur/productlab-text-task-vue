<script setup>
</script>
<script >
export default{
    data(){
      return{
        images:[],
        isModalopen:false,
        currImage:{},
        commentText:'', 
        nameText:''
      }
    },
    methods:{
      getImages(){
        fetch('https://boiling-refuge-66454.herokuapp.com/images')
        .then((response) => {
         return response.json();
        })
        .then((data) => {
             this.images=data;
        });
      },
      toggleModalState(){
        this.isModalopen=!this.isModalopen
      },
      getInfoAboutImage(id){
        fetch('https://boiling-refuge-66454.herokuapp.com/images/'+id)
        .then((response) => {
         return response.json();
        })
        .then((data) => {
            console.log(data)
            this.currImage=data;
        });
       
      },
      onImageClick(id){
        this.toggleModalState()
        this.getInfoAboutImage(id)
      },
      nameTextChangeHandler(e){
        this.nameText=e.target.value
      },
      commentTextChangeHandler(e){
        this.commentText=e.target.value
      },
      async postComment(){

        const url = `https://boiling-refuge-66454.herokuapp.com/images/${this.currImage.id}/comments`;
        const data = { name:this.nameText, comment: this.commentText };
        try {
          const response = await fetch(url, {
            method: 'POST',
            body: JSON.stringify(data),
            headers: {
                'Content-Type': 'application/json'
              }
          });
          if(response.status===204)
            alert('Успех! Код ответа 204.');
          else  throw new Error('Неудовлетворительный код ответа. '+response.status)
        }
        catch (error) {
          alert('Ошибка, возможно вы не ввели имя и(или) комментарий.');
          console.log('Ошибка.', error);
        }


        this.commentText=''
        this.nameText=''
      }
    },
    mounted:function(){
      this.getImages()
    } 
  }
</script>

<template>
  <main>
    <ul v-if="images.length">
      <li v-for='image in images'>
        <img v-on:click='onImageClick(image.id)' v-bind:src='image.url'/>
        <div v-if='isModalopen' class="hystmodal" id="myModal">
          <div class="hystmodal__window">
            <img  v-bind:src="currImage.url" alt="Изображение в окне" style='display:block; width:100%; border-radius:10px;margin:5px;'/>
              <input type='text' v-on:input='nameTextChangeHandler' v-bind:value='nameText'  placeholder='введите имя'  style='display:block; border-radius:4px;margin:5px;'/>
              <textarea  v-on:input='commentTextChangeHandler' v-bind:value='commentText' placeholder='введите комментарий'   style='display:block; border-radius:4px;margin:5px; rows:3;    resize: none;  width:100%'/>
              <button v-on:click='postComment'  style='display:block; border-radius:4px;margin:5px;'>Отправить комментарий</button>
              <ul v-if='currImage?.comments.length' style='margin:5px; padding:0px; display:block;'>
                <h2>Комментарии</h2>
                <li v-for="comment in currImage.comments" style=' padding:3px; color:black; border-radius:3px; background:gray; text-align:left; display:block; border-width:1px'>
                {{comment.text}}
                </li>
              </ul>
            <button v-on:click='toggleModalState' class="hystmodal__close" style='display:block; border-radius:4px;margin:5px;'>Закрыть</button>  
          </div>
        </div>
      </li>
    </ul>
   
  </main>
</template>
  
<style>
@import './assets/base.css';
@import url('https://fonts.googleapis.com/css2?family=Montserrat+Alternates:wght@400&display=swap');
#app {
  font-family: 'Montserrat Alternates', sans-serif;
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  font-weight: normal;
}
.hystmodal {
    position: fixed;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    overflow: hidden;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
    display: flex;
    flex-flow: column nowrap;
    justify-content: center; /* см. ниже */
    align-items: center;
    z-index: 99;
    padding:30px ;
    background: rgba(0,0,0,0.5);
    opacity: 0.5;


    
}
.hystmodal__window{
   border-radius:20px;
    padding:20px 20px;
  background: rgba(84,84,84,0.48);
    width: 600px;
    max-width: 100%;

}
li{
  display:inline;
  
}
input,button,textarea{
  font-family: 'Montserrat Alternates', sans-serif;
}
::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
  color: black;
  opacity: 1; /* Firefox */
}

</style>
