<template>
  <div class="navbar">
    <ul class="navbar-menu">
      <li>
        <a>
          <IconLogo />
        </a>
      </li>
      <li><a>
          <RouterLink to="/">Sobre</RouterLink>
        </a>
      </li>
      <li><a>
          <RouterLink to="/about">Sudoku</RouterLink>
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
import IconLogo from "@/components/icons/IconLogo.vue"

export default {
  name: "NavBar",
  components: { IconLogo }
}
</script>

<style scoped>
.navbar {
  background-color: rgb(20, 20, 20);
  color: #fff;
  display: flex;
  align-items: center;
  padding: 16px
}

.navbar-menu {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
}

.navbar-menu li {
  margin-right: 16px;
  display: flex;
  align-items: center;
}

.navbar-menu li a {
  text-decoration: none;
  color: #fff;
}

@media (max-width: 768px) {
  .navbar {
    flex-direction: column;
  }

  .navbar-menu {
    display: flex;
    flex-direction: column;
  }

  .navbar-menu li {
    margin-right: 0;
  }

  .navbar-menu>*:not(:last-child) {
    margin-bottom: 16px;
  }
}
</style>