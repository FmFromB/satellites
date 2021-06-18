<template>
  <mu-col span="4" sm="4" class="projectSelector">
    <mu-button @click="addProject">Создать проект</mu-button>
    <div v-for="room in rooms">
      <h3 @click="openWorkspace(room.id)">{{room.creator.username}}</h3>
      <small>{{room.date}}</small>
    </div>
  </mu-col>
</template>

<script>
  import $ from 'jQuery'
  export default {
    name: 'ProjectRoom',
    data() {
      return {
        rooms: '',
        workspace: {
          id: '',
          show: false,
        },
      }  
    },
    created() {
      $.ajaxSetup({
        headers: { 'Authorization': "Token " + localStorage.getItem('auth_token') },
      });
      this.loadRoom()
    },
    methods: {
      loadRoom() {
        $.ajax({
          url: "http://127.0.0.1:8000/api/workspace/room",
          type: "GET",
          success: (response) => {
            this.rooms = response.data.data
          }
        })
      },
      openWorkspace(id) {
        this.$emit("openWorkspace", id)
      },
      addProject() {
        $.ajax({
          url: "http://127.0.0.1:8000/api/workspace/room/",
          type: "POST",
          success: (response) => {
            this.rooms = response.data.data
          }
        })
      }
    }
  }
</script>

<style>
  h3 {
    cursor: pointer;
  }
</style>
