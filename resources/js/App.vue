<template>
  <div id="wrapper">
    <!-- Sidebar -->
    <sidebar-view></sidebar-view>
    <!-- End Sidebar -->
    <div id="content-wrapper" class="d-flex flex-column">
      <div id="content">
        <!-- TopBar -->
        <header-view></header-view>
        <!-- End Topbar -->

        <!-- Container Fluid-->
        <div class="container-fluid" id="container-wrapper">
          <router-view></router-view>
        </div>
        <!-- End Container Fluid-->
      </div>
      <!-- Footer -->
      <footer-view></footer-view>
      <!-- End Footer -->
    </div>
    <vue-progress-bar></vue-progress-bar>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Sidebar from './components/Sidebar.vue'
import Footer from './components/Footer.vue'
export default {
  components: {
    'header-view': Header,
    'sidebar-view': Sidebar,
    'footer-view': Footer,
  },
  mounted() {
    this.$Progress.finish();
  },
  created() {
    this.$Progress.start();
    this.$router.beforeEach((to, from, next) => {
      if (to.meta.progress !== undefined) {
        let meta = to.meta.progress;
        this.$Progress.parseMeta(meta);
      }
      this.$Progress.start();
      next();
    });
    this.$router.afterEach((to, from) => {
      this.$Progress.finish();
    });
  },
};
</script>