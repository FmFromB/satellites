<template>
  <mu-col span="8" xl="8">
    <mu-container class="workspace">
      <mu-row direction="column"
              align-content="start"
              v-for="workspace in workspaces"
              align-items="end"
              v-autoscroll:noscroll="'bottom'">
        <span>{{workspace.date}}</span>
        <p><strong>{{workspace.text}}</strong></p>
        <img :src="'http://127.0.0.1:8000'+ workspace.map" width="500" class="map" negative=yes/>
      </mu-row>
    </mu-container>
    <mu-container class="message"id="app">
      <mu-row>
        <label>
          File
          <input id="uploadimage" type="FILE" name="uploadimage">
        </label>
        <mu-col span="12" lg="4" sm="6">
          <mu-select label="Алгоритм" v-model="normal.value2" full-width>
            <mu-option v-for="option in options" :key="option" :label="option" :value="option"></mu-option>
          </mu-select>
        </mu-col>
        <mu-button round color="primary" @click="Submit">Загрузить данные</mu-button>
      </mu-row>
    </mu-container>
  </mu-col>
</template>

<script>
  import $ from 'jQuery'
  export default {
    name: 'Workspace',
    props: {
      id: '',
    },
    data() {
      return {
        workspaces: '',
        textarea: '',
        file: '',
        options: [
          'Алгоритм 1', 'Алгоритм 2', 'Алгоритм 3'
        ],
        normal: {
          value1: '',
          value2: '',
          value3: '',
          value4: 'Option 1',
          value5: 'Option 2'
        }
      }
    },
    created() {
      $.ajaxSetup({
        headers: { 'Authorization': "Token " + localStorage.getItem('auth_token') },
      });
      this.loadWorkspace()
    },
    methods: {
      loadWorkspace() {
        $.ajax({
          url: "http://127.0.0.1:8000/api/workspace/project/",
          type: "GET",
          data: {
            room: this.id
          },
          success: (response) => {
            this.workspaces = response.data.data
          }
        })
      },
      handleFileUpload() {
        this.file = this.$refs.file.files[0];
      },
      Submit() {
        var formData = new FormData()
        var $input = $("#uploadimage");
        formData.append('map', $input.prop('files')[0])
        formData.append('text', this.textarea)
        formData.append('room', this.id)
        $.ajax({
          url: "http://127.0.0.1:8000/api/workspace/project/",
          type: "POST",
          processData: false,
          contentType: false,
          data: formData,
          success: (response) => {
            this.loadDialog()
          },
          error: (response) => {
            alert("Вы не можете отправить пустое сообщение")
          }
        }),
          this.textarea = ''
      },
    }
  }
</script>

<style scoped>
  .workspace {
    position: absolute;
    overflow: auto;
    max-height: 746px;
    min-height: 0px;
    width: 150%;
    top: 15%;
    background-color: white;
    border-radius: 10px;
  }
  .map{
      margin-left: 30%
  }
  .message {
    background-color: white;
    position: fixed;
    bottom: -12%;
    width: 50%;
    height: 15%;
    margin: 5% 0%;
    border-radius: 10px;
  }
</style>
