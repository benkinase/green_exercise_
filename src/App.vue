<template>
  <div id="app">
    <KpiTree msg="KPI: Value Tree" />
  </div>
</template>

<script>
import KpiTree from "./components/KpiTree.vue";

export default {
  name: "App",
  components: {
    KpiTree,
  },
};
</script>

<style>
@import url("https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css");

:root {
  --primary: #171b25;
  --secondary: #283046;
  --text: #d0d2d6;
  --line: #faad07;
  --line-1: #79756e;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
  margin: auto;
  background: var(--primary);
  min-height: 100vh;
  padding: 1rem;
}

#app .name {
  font-weight: 700;
  color: white;
}

#app h1 {
  color: var(--secondary);
}
</style>
