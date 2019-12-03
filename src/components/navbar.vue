<template>
  <nav class="navbar has-shadow is-spaced">
    <div class="navbar-menu">
      <div class="navbar-start">
        <h4 class="title is-4">Sorting Algorithms Demo</h4>
      </div>

      <div class="navbar-end">
        <div class="navbar-item">
          <p class="buttons">
            <button class="button" v-on:click="goToGitHub()">
              <span class="icon">
                <i class="fab fa-github"></i>
              </span>
              <span>GitHub</span>
            </button>
          </p>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
import { GITHUB_URL } from '@/shared/constants';

export default {
  name: 'NavBar',

  methods: {
    goToGitHub() {
      window.location.href = GITHUB_URL;
    },
  },
};
</script>

<style scoped></style>
