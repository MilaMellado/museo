<template>
  <nav class="navbar">
    <div class="logo">CLEVELAND<span>GALLERY</span></div>
  </nav>
</template>

<style scoped>
.navbar {
  padding: 20px 40px;
  background: rgba(8, 8, 8, 0.9);
  backdrop-filter: blur(15px);
  border-bottom: 1px solid #1a1a1a;
  position: sticky;
  top: 0;
  z-index: 50;
}
.logo { font-weight: 800; letter-spacing: 5px; font-size: 1.2rem; color: #fff; }
.logo span { font-weight: 300; color: #555; }
</style>