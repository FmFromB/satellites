<template>
  <mu-container>
    <mu-appbar style="width: 100%" color="black" class="bar">
      Обработчик спутниковых данных

      <mu-button flat slot="right" v-if="!auth" @click="goLogin">Вход</mu-button>
      <mu-button flat slot="right" v-if="!auth" @click="goCreate">Зарегаться</mu-button>
      <mu-button flat slot="right" v-else @click="goLogout">Выход</mu-button>
    </mu-appbar>
    <mu-row>

    </mu-row>
    <mu-row>
      <ProjectRoom v-if="auth" @openWorkspace="openWorkspace" class="projectSelector"></ProjectRoom>
      <Workspace v-if="workspace.show" :id="workspace.id"></Workspace>
      <Map></Map>
    </mu-row>
  </mu-container>
</template>

<script>
  import ProjectRoom from '@/components/rooms/ProjectRoom.vue'
  import Workspace from '@/components/rooms/Workspace.vue'
  import Map from '@/components/Map'

  export default {
    name: 'Home',
    components: {
      ProjectRoom,
      Workspace,
      Map,
    },
    data() {
      return {
        workspace: {
          id: '',
          show: false,
        }
      }  
    },
    computed: {
      auth() {
        if (localStorage.getItem("auth_token")) {
          return true
        }
      }
    },
    methods: {
      goLogin() {
        this.$router.push({name: "Login"})
      },
      goCreate() {
        this.$router.push({ name: "NewUser" })
      },
      goLogout() {
        localStorage.removeItem("auth_token")
        window.location = '/'
      },
      openWorkspace(id) {
        this.workspace.id = id
        this.workspace.show = true
      },
    },
  }
</script>

<style>
  .bar {
    position: fixed;
    left: 0%;
    top: 0%;
  }
</style>
