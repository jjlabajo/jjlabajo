<template>
  <div>
    <a @click="$emit('pray','close')" >Close</a><br/><br/>
    <table class="form-table">
      <tr>
        <td class="th">Name</td>
        <td class="th"><input type="text" v-model="name" /></td>
      </tr>
      <tr>
        <td class="th">Description</td>
        <td class="th"><input type="text" v-model="description" /></td>
      </tr>
      <tr>
        <td class="th">Method</td>
        <td class="th">
          <select v-model="method">
            <option>GET</option>
            <option>POST</option>
            <option>PUT / PATCH</option>
            <option>DELETE</option>
          </select>
        </td>
      </tr>
      <tr>
        <td class="th">URL</td>
        <td><input type="text" v-model="url" /></td>
      </tr>
      <tr><td></td><td></td></tr>
      <tr>
        <td class="th"> Fields</td>
        <td></td>
      </tr>
      <tr>
        <td colspan=2>
          <table>
            <tr>
              <td> <input type="text" v-model="fieldvalueinput">
                <select v-model="fieldtypeinput">
                  <option>int</option>
                  <option>varchar</option>
                  <option>text</option>
                  <option>date</option>
                  <option>timestamp</option>
                </select>
              </td>
              <td> <a @click="add('fields',{'value':fieldvalueinput,'datatype':fieldtypeinput})"> Add </a></td>
            </tr>
            <tr v-for="field in fields" >
              <td>{{ field.value }} <span class="badge">{{ field.datatype }}</span></td>
              <td><span @click="remove('fields',field)" >x</span></td>
            </tr>
          </table>
        </td>
      </tr>
      <tr>
        <td class="th"> Response </td>
        <td></td>
      </tr>
      <tr>
        <td colspan=2>
          <table>
            <tr>
              <td> 
                <select v-model="responsefield">
                  <option v-for="field in fields" >{{ field.value }}</option>
                </select><br/>
                <input v-model="responsedescription" placeholder="description" />
                <input type="text" v-model="responsesample" placeholder="sample">
              </td>
              <td> <a @click="add('response',{'field':responsefield,'description':responsedescription,'sample':responsesample})"> Add </a></td>
            </tr>
            <tr v-for="item in response" >
              <td>{{ item.field }} <small> :: {{ item.sample }}</small><br/> <pre style="margin-top:0px;">{{item.description}}</pre> </td>
              <td><span @click="remove('response',item)" >x</span></td>
            </tr>
          </table>
        </td>
      </tr>
    </table>
    <br><br>
    <a @click="save" >Save</a>
  </div>
</template>

<script>
export default {
  name: 'ApiForm',
  data: function(){
    return {
      fieldvalueinput : '',
      fieldtypeinput : '',
      responsedescription: '',
      responsefield: '',
      responsesample: '',
      description:'',
      name:'',
      method:'',
      url:'',
      response: [
        {
          'field': 'id',
          'description' : 'Unique ID for the users table.',
          'sample' : '19439404'
        },
        {
          'field': 'created_at',
          'description' : 'When the data is created.',
          'sample' : '2018-07-20 1:05:03 PM'
        },
        {
          'field': 'update_at',
          'description' : 'Last update of data.',
          'sample' : '2018-07-20 1:05:03 PM'
        }
      ],
      fields: [
        {
          'value' : 'id',
          'datatype' : 'int'
        },
        {
          'value' : 'created_at',
          'datatype' : 'timestamp'
        },
        {
          'value' : 'updated_at',
          'datatype' : 'timestamp'
        }
      ]
    }
  },
  methods: {
    remove: function(type,value){
      this[type].splice(this[type].indexOf(value), 1);
    },
    add: function(type,value){
      this[type].splice(this[type].length, 0, value);
    },
    save: function(){
      var data = {
        'name' : this.name,
        'description' : this.description,
        'method' : this.method,
        'url' : this.url,
        'fields' : this.fields,
        'response' : this.response
      }
      this.$emit('pray','save',data)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .form-table tr td.th{
    font-weight:bold;
    padding-top:16px;
  }
  small{
    font-size:12px;
    color:gray;
  }
</style>
