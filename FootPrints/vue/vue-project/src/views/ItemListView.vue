<template>
  <div>
      <tool-bar></tool-bar>
      배달 리스트 뷰 ~.~
      <router-link to="/writing" class="link">글 작성</router-link> 
  </div>
</template>

<script>
import ToolBar from '../components/ToolBar.vue'

export default {
    components:{
        ToolBar,
    }
}
</script>

<style>

</style>