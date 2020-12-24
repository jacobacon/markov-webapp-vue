<template>
  <v-app>
    <v-main>
      <v-container fluid fill-height>
        <Home></Home>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Home from "@/components/Home";
export default {
  name: "App",
  components: { Home },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background: rgb(92, 107, 115);
  background: linear-gradient(
    0deg,
    rgba(92, 107, 115, 1) 0%,
    rgba(0, 199, 230, 1) 100%
  );
}
</style>
