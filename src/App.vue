<template>
<v-app>
  <router-view></router-view> <!--router-view akan membantu kita mengubah view website kita sesuai URL dan
                              komponen yang sudah kita tetapkan di router.js-->
</v-app>
</template>

<script>
  export default {
    name: "App",
  };
</script>