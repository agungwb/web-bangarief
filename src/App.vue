<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
@font-face {
  font-family: "Roboto-Light";
  src: local("Roboto-Light"),
   url(./assets/fonts/Roboto/Roboto-Light.ttf) format("truetype");
}

@font-face {
  font-family: "Roboto-Bold";
  src: local("Roboto-Bold"),
   url(./assets/fonts/Roboto/Roboto-Bold.ttf) format("truetype");
}

@font-face {
  font-family: "Roboto-Medium";
  src: local("Roboto-Medium"),
   url(./assets/fonts/Roboto/Roboto-Medium.ttf) format("truetype");
}

@font-face {
  font-family: "Roboto-Regular";
  src: local("Roboto-Regular"),
   url(./assets/fonts/Roboto/Roboto-Regular.ttf) format("truetype");
}

@font-face {
  font-family: "RobotoMono-Bold";
  src: local("RobotoMono-Bold"),
   url(./assets/fonts/Roboto_Mono/static/RobotoMono-Bold.ttf) format("truetype");
}

@font-face {
  font-family: "RobotoMono-Regular";
  src: local("RobotoMono-Regular"),
   url(./assets/fonts/Roboto_Mono/static/RobotoMono-Regular.ttf) format("truetype");
}

@font-face {
  font-family: "DancingScript-Medium";
  src: local("DancingScipt-Medium"),
   url(./assets/fonts/Dancing_Script/static/DancingScript-Medium.ttf) format("truetype");
}

@font-face {
  font-family: "DancingScript-Regular";
  src: local("DancingScipt-Regular"),
   url(./assets/fonts/Dancing_Script/static/DancingScript-Regular.ttf) format("truetype");
}

@font-face {
  font-family: "DancingScript-SemiBold";
  src: local("DancingScipt-SemiBold"),
   url(./assets/fonts/Dancing_Script/static/DancingScript-SemiBold.ttf) format("truetype");
}

@font-face {
  font-family: "Lora-Regular";
  src: local("Lora-Regular"),
   url(./assets/fonts/Lora/static/Lora-Regular.ttf) format("truetype");
}

* {
  background-color:rgb(245, 245, 245);
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  overflow-x: hidden;
}
</style>
