<template>
  <div v-if="details.length !== 0" >
    <hr>
    <span><a class="pull-right" @click="$emit('pray','delete-list',details)" >Delete</a></span>
    <h1>{{details.method}}{{details.url}}</h1>
    <p>{{details.description}}</p><br/>

    <h3>Fields</h3>
    <ul>
        <li v-for="field in details.fields">{{field.value}} <span class="badge">{{field.datatype}} </span></li>
    </ul>
    <br>
    <div class="item-container">
        <a class="success"> 200 success</a>
        <a class='pull-right' v-show="!showsuccess" @click="showsuccess = true" >Show</a>
        <a class='pull-right' v-show="showsuccess" @click="showsuccess = false" >Hide</a>
    </div>
    <div v-show="showsuccess" >
        <h4>Response</h4>
        <ul>
            <li v-for="response in details.response" > {{response.field}} <pre style="margin-top:0px">{{response.description}}</pre> </li>
        </ul>
        <h4>Sample</h4>
        <div class="code-container">
            <pre>{{pre}}</pre>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ApiList',
  props: {
      details: {
          default: {}
      }
  },
  data: function(){
    return {
        showsuccess: false
    }
  },
  computed: {
      pre: function(){
var toreturn = `{
    {
        'status' : 'success',
        'code' : 200,`;
        if(this.details.length !== 0){
            toreturn += `
        'data' : {`;
            for(let x in this.details.response){
              toreturn += `
            '${this.details.response[x].field}' : '${this.details.response[x].sample}',`;
            }
            toreturn += `
        },`;
        }
          
          return toreturn+`
        'message' : 'Data has been successfully retrieved.'
    }
}`;
      }
  },
  methods: {
      
  }
}
</script>

<style scoped>
    h1{
        letter-spacing: 3px;
    }
    .item-container{
        min-height:20px;
        background-color:#e4ffec;
        padding:15px;
    }
    .code-container{
        min-height:20px;
        background-color:black;
        color:white;
        padding:15px;
    }
    a.success{
        color:#1d945e;
    }
    a.pull-right{
        font-size:13px;
        float:right;
    }
</style>
