<template>
  <div id="app">
    <a v-show="!addable" @click="add">Add</a>
    <ApiForm v-show="addable" @pray="listen" ></ApiForm><br><br/>
    <ApiList v-for="item in list" :details="item" @pray="listen"></ApiList>
    <hr><a @click="exportToJson">
    Export as Json</a> <br/>
    <input type="file" @change="file = $event.target.value" />
    <a @click="importJson">
    Import Json</a>
  </div>
</template>

<script>
import ApiForm from './components/ApiForm.vue'
import ApiList from './components/ApiList.vue'
import {url} from './variables.js'
import axios from 'axios'

export default {
  name: 'app',
  components: {
    ApiForm,
    ApiList
  },
  data: function(){
    return {
      addable : false,
      list : [],
      file: ''
    }
  },
  methods: {
      downloadObjectAsJson: function(exportObj, exportName){
        var dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(exportObj));
        var downloadAnchorNode = document.createElement('a');
        downloadAnchorNode.setAttribute("href",     dataStr);
        downloadAnchorNode.setAttribute("download", exportName + ".json");
        document.body.appendChild(downloadAnchorNode); // required for firefox
        downloadAnchorNode.click();
        downloadAnchorNode.remove();
      },
      add: function(){
          this.addable = true
      },
      exportToJson: function(){
          this.downloadObjectAsJson(this.list,'list')
          // axios.post(`${url}/jsonify`, {
          //       list : this.list
          //   })
          //   .then(function (response) {
          //       console.log(response.data);
          //   })
      },
      importJson: function(){
        console.log(this.file)
        axios.post(this.file)
        .then(function (response) {
            console.log(response);
        })
      },
      listen: function(prayer, value = ""){
          if(prayer == 'close'){
            this.addable = false
          }
          if(prayer == 'save'){
              this.list.splice(this.list.length, 0, value);
          }
          if(prayer == 'delete-list'){
              this.list.splice(this.list.indexOf(value), 1);
          }
      }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 60px;
}

a {
  color: #42b983;
}

.badge{
    margin-left:8px;
    color:white;
    font-size:12px;
    background-color:black;
    padding: 1px 10px;
    border-radius:7px;
}
</style>
