<template>
  <div>
    <form @submit.prevent="submitForm()">
      <label for="title">Título:</label>
      <input type="text" name="title" id="title" v-model="form.title"> <br/>
      <label for="file">Arquivo:</label>
      <input type="file" name="file" id="title"  @change="handlerFile($event)"> <br/>
      <label id="select-label" for="tag">Tags:</label>
      <select name="tag" id="tag"  v-model="form.tags" multiple>
        <option v-for="tag in tags" :key="tag">{{ tag }}</option>
      </select>  <br/>
      <input type="submit" value="Enviar">
    </form>
    <p v-if="success">Upload concluído!</p>
  </div>
</template>
<script>
import axios from 'axios';
export default {


  data() {
    return {
      success: false,
      tags:["DESIGN", "AUDIOVISUAL","GAME", "PROGRAMACAO"],
      form: {
        title:'',
        date: null,
        tags:[],
        files: []
      }
    }
  },
  created(){
    const date = new Date();
    this.form.date = `${ date.getFullYear() }-${ date.getMonth() + 1 < 10? ("0" + (date.getMonth() + 1)): date.getMonth() }-${ date.getDate() }`;
    console.log(this.form);
  },
  methods: {

    handlerFile($event){
      this.form.files.push($event.target.files[0]);
      console.log($event.target.files[0]);
    },
    submitForm($event){
      console.log($event);
      let formData = new FormData();
      formData.append('images',this.form.files[0]);
      formData.set('portfolio', JSON.stringify(this.form));
      axios.post('http://localhost:8000/portfolio',formData)
        .then((sucess) => {
          console.log("Sucesso: "+ JSON.stringify(sucess));
          this.success = true;
        })
        .catch((fail) => console.log("Erro: "+ fail));

    }
  }
}
</script>

<style scoped>
form{
  width: 100%;
}
input {
  margin: 10px;
  padding: 5px;
  width: 300px;
  text-align: center;
}
select {
  padding: 0px 20px
}

label#select-label, select{
    display: inline-block;
    vertical-align: middle;  
}
</style>