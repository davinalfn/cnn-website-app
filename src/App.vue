<template>
  <div id="app">
    <header class="app-header">
      <div class="container">
        <router-link to="/" class="logo">
          📰 CNN
        </router-link>
        <nav class="nav-menu">
          <router-link to="/" class="nav-link">Beranda</router-link>
        </nav>
      </div>
    </header>

    <main class="app-main">
      <RouterView />
    </main>

    <footer class="app-footer">
      <div class="container">
        <p>&copy; CNN News.</p>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: #f8f9fa;
  color: #2c3e50;
}

#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.app-header {
  background: white;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  position: sticky;
  top: 0;
  z-index: 100;
}

.app-header .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
}

.logo {
  font-size: 24px;
  font-weight: 800;
  text-decoration: none;
  color: #2c3e50;
  transition: color 0.3s ease;
}

.logo:hover {
  color: #3498db;
}

.nav-menu {
  display: flex;
  gap: 20px;
}

.nav-link {
  text-decoration: none;
  color: #7f8c8d;
  font-weight: 600;
  transition: color 0.3s ease;
}

.nav-link:hover,
.nav-link.router-link-active {
  color: #3498db;
}

.app-main {
  flex: 1;
  padding: 20px 0;
}

.app-footer {
  background: #2c3e50;
  color: white;
  padding: 30px 0;
  margin-top: 60px;
  text-align: center;
}

.app-footer p {
  opacity: 0.8;
}

@media (max-width: 768px) {
  .app-header .container {
    flex-direction: column;
    gap: 15px;
  }
}
</style>