<template>
  <div id="app" class="font-sans antialiased text-black">
    <div id="nav" class="bg-indigo-800">
      <div class="container mx-auto flex items-center justify-between py-6">
        <div>
          <a href="/">
            <img src="./assets/logo.svg" alt />
          </a>
        </div>
        <div>
          <router-link to="/" class="text-white hover:text-grey-800">Home</router-link>
          <router-link to="/about" class="text-white hover:text-grey-800 pl-6">About</router-link>
        </div>
      </div>
    </div>

    <router-view />
  </div>
</template>


