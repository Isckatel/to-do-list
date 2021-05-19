<template>
  <div id ="dolist">
    <div class="imgH2">
      <div><img alt="Pencil text" src="../assets/pencil-text.png"></div>
      <div><h2>Список дел</h2></div>
    </div>
    <p class="help"> &#9888; Интерактивный список дел с сохранением в локальном хранилище браузера.</p>
    <form v-on:submit.prevent="createDo">
      <label title="Введите текст. Нажмите Enter |&crarr;|">
        <input type="text" size="30"  v-model="this.newDo" placeholder="Введите текст. Нажмите Enter |&crarr;|">
      </label>
      <button class="ButtAdd">Добавить</button>
    </form>
    <ol>
      <li v-for="oneDo in arrDo" :key="oneDo.id">
        <div class="container">
          <div v-bind:class="arrVisuText[oneDo.id-1]">{{oneDo.text}}</div>
          <div>
            <label title="Введите текст. Нажмите Enter |&crarr;|">
              <input type="text" size="30" v-on:keyup.enter="editDo(oneDo.id)"
              v-bind:class="arrVisuInput[oneDo.id-1]" v-model="this.arrEditDo[oneDo.id]">
            </label>
         </div>
          <div class="buttons">
           <button  @click="deleteDo(oneDo.id)">Удалить</button> <button @click="buttEdit(oneDo.id)">Изменить</button>
          </div>
        </div>
      </li>
    </ol>
    <button @click="save">Сохранить в браузере</button>
  </div>
</template>

<script>
export default {
  name: 'Dolist',
  data(){
    return {
      arrDo : [
        {id:1, text:"Потренироваться на mastery.games"},
        {id:2, text:"Решить 2 задачи на JavaScript"}
      ],
      newDo: "",
      arrEditDo: ["",""],
      arrVisuInput:["noneDisplay","noneDisplay"],
      arrVisuText:["visuDisplay","visuDisplay"]
    }
  },
  //Сробатывает до рендеригна компонента
  mounted() {
      if (localStorage.getItem('arrDoTo')) {
        try {
          this.arrDo = JSON.parse(localStorage.getItem('arrDoTo'));
          let k = this.arrDo.length;
          let n = this.arrVisuInput.length;
          for (let i = n;i<k;i++ ) {
          this.arrVisuInput.push("noneDisplay");
          }
          let l = this.arrVisuText.length;
          for (let i = l;i<k;i++ ) {
          this.arrVisuText.push("visuDisplay");
          }
        } catch(e) {
          localStorage.removeItem('arrDoTo');
        }
      }
    },
  methods: {
    createDo: function () {
      console.log("Вызов createDo. Значение newDo: " +this.newDo);
      if (!this.newDo) return;
      let newId = this.arrDo.length + 1;
      this.arrDo.push({
        id: newId,
        text: this.newDo
      });
      this.arrVisuInput.push("noneDisplay");
      this.arrVisuText.push("visuDisplay");
      this.newDo="";

    },
    deleteDo: function(id) {
      console.log("Вызов deleteDo.");
      let newArr =[];
      let i = 1;
      this.arrDo.forEach( item => {
        if (item.id != id) {
          newArr.push({id: i, text: item.text});
          i++;
        }
      });
      this.arrDo = newArr;
      id--;
      this.arrVisuInput.splice(id,1);
      this.arrVisuText.splice(id,1);
      console.log(this.arrDo);
      console.log(this.arrVisuInput);
      console.log(this.arrVisuText);
    },
    editDo: function(id){
      console.log(this.arrEditDo);
      console.log("Вызов editDo. Значение newDo: " +this.arrEditDo[id]);
      if (this.arrEditDo[id] == "") return;
      this.arrDo[id-1].text = this.arrEditDo[id];
      this.arrEditDo[id]="";
      id--;
      this.arrVisuInput[id]="noneDisplay";
      this.arrVisuText[id]="visuDisplay";
    },
    buttEdit: function(id){
      id--;
      this.arrVisuInput[id]="visuDisplay";
      this.arrVisuText[id]="noneDisplay";
      this.arrEditDo[id+1]=this.arrDo[id].text;
    },
    save: function(){
      const parsed = JSON.stringify(this.arrDo);
      localStorage.setItem('arrDoTo', parsed);
    }
  }//Конец методов
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
#dolist {
  border: 2px solid silver;
  padding: 30px;
  width:500px;
  background-color: #f1f1f1;
}

.imgH2 {
  display: flex;
}

.help {
  font-size: 14px;
  color:#777777;
}

img {
  width: 50px;
}

h2 {
  margin-left: 20px;
  margin-bottom: 0px;
}

input[type=text]{
   height:25px;
}

input:focus {
  outline: 0;
  outline-offset: 0;
  border:2px solid silver;
  box-shadow: 1px 1px 1px 0 silver;
}

.ButtAdd{
   height:30px;
   margin-left: 5px;
}

ol {
  padding-left: 1em;
}

li {
  margin-bottom: 5px;
}

.container {
  display: flex;
}

.buttons {
  margin-left: 5px;
}

.noneDisplay {
  display:none
}

.visuDisplay {
  display:block;
}

button:hover {
  box-shadow: 1px 1px 1px 0 silver;
  cursor: pointer;
}

</style>
