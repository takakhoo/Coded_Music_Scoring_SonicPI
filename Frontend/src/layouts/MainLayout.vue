<template>
  <div>
    <div class="text-center py-2 mb-2" style="background-color: #1A4731;">
      <img src="@/assets/images/musicagent.png" alt="MusicAgent" style="max-width: 100%; height: 200px;">
    </div>
    <div class="container main_container bg-dark text-white">
      <nav class="navbar navbar-expand-lg navbar-light bg-dark text-white">
        <div class="container-fluid">
          <a class="navbar-brand" href="/">MusicAgent</a>
          <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
          </button>
          <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
              <li class="nav-item">
                <router-link class="nav-link" to="/home">
                  <i class="fas fa-music"></i> New Song
                </router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/songs-library">
                  <i class="fas fa-book"></i> Song Library
                </router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/creative-mode">
                  <i class="fas fa-paint-brush"></i> Creative Mode
                </router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/samples">
                  <i class="fas fa-drum"></i> Samples
                </router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/settings">
                  <i class="fas fa-cog"></i> Settings
                </router-link>
              </li>
            </ul>
          </div>
        </div>
      </nav>

      <div class="slot-container bg-white text-dark">
        <slot></slot>
      </div>
    </div>

    <footer class="container footer_container bg-dark text-white text-center py-3 mb-4">
      <div>
        <p class="mb-0">&copy; 2024 Coded Music Scoring with Sonic Pi by takakhoo. All rights reserved.</p>
        <p class="mb-0">
          <a href="https://github.com/takakhoo/Coded_Music_Scoring_SonicPI" class="text-white"><i class="fab fa-github"></i> GitHub</a>
        </p>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'MainLayout'
};
</script>

<style scoped>
.main_container {
  width: 100% !important;
  padding: 5px;
}

.slot-container {
  padding: 20px;
  border-radius: 5px;
}

.footer_container {
  width: 100%;
}
</style>
