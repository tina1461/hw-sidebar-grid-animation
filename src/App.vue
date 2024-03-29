<template>
  <div>
    <!-- nav -->
    <nav class="navigation">
      <h1></h1>
      <button @click="toggleSidebar">☰</button>
    </nav>

    <!-- main -->
    <main class="main-content">
      <GridAnimation />
    </main>

    <!-- side bar -->
    <SideBar :is-visible="isSidebarVisible" @close="isSidebarVisible = false" />
  </div>
</template>

<script>
import SideBar from './components/SideBar'
import GridAnimation from './components/GridAnimation';

export default {
  name: 'App',
  components: {
    SideBar,
    GridAnimation,
  },
  data() {
    return {
      isSidebarVisible: false,
    };
  },
  methods: {
    toggleSidebar() {
      this.isSidebarVisible = !this.isSidebarVisible;
    },
  },
};
</script>

<style>
body, html {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.navigation {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
  background-color: #333;
  color: #fff;
  height: 60px;
}

.main-content {
  height: calc(100vh - 60px);
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
