<template>
  <footer class="footer">
    <div class="social-icons">
      <a href="https://github.com/" target="_blank" rel="noopener noreferrer">
        <IconGithub />
      </a>
      <a href="https://www.instagram.com/" target="_blank" rel="noopener noreferrer">
        <IconInstagram />
      </a>
      <a href="https://www.linkedin.com/in/" target="_blank" rel="noopener noreferrer">
        <IconLinkedin />
      </a>
    </div>
  </footer>
</template>

<script>
import IconGithub from "@/components/icons/IconGithub.vue"
import IconInstagram from "@/components/icons/IconInstagram.vue"
import IconLinkedin from "@/components/icons/IconLinkedin.vue"

export default {
  name: 'Footer',
  components: { IconGithub, IconInstagram, IconLinkedin }
};
</script>

<style scoped>
.footer {
  text-align: center;
  padding: 20px;
  background-color: rgb(20, 20, 20);
}

.social-icons a {
  width: 30px;
  margin: 0 10px;
  fill: aliceblue;
}
</style>