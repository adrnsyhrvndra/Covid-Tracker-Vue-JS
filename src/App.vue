<template>

  <Header/>

  <div class="container text-center mx-auto bg-slate-400">

    <router-view/>

  </div>

</template>

<script>

import Header from "./components/Header.vue";

export default{

  components:{

    Header

  }

}

</script>
