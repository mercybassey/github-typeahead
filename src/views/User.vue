<template>
  <section class="user-container">
      <img :src="image" alt="">
       <div class="second-div">
          <div class="intro">
            <h4>{{name}}</h4>
            <a :href="githubLink">{{githubLink}}</a>
          </div>
      </div> 
  </section>
</template>

<script>
export default {
  name : 'User',
  props:['name','image', 'githubLink']
}
</script>

