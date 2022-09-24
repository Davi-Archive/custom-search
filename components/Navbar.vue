<!-- Please remove this file from your project -->
<template>
  <nav class="w-full py-4 bg-blue-800 shadow">
    <div class="w-full container mx-auto flex flex-wrap items-center justify-between">

      <nav class="w-full">
        <ul class="flex items-center justify-between font-bold text-sm text-white uppercase no-underline">
          <li>
            <NuxtLink to="/">Home page</NuxtLink>
          </li>
          <li>
            <nuxt-link to="/post?id=688159" class="button--green">
              Post Blog
            </nuxt-link>
          </li>
          <!-- <li><nuxt-link to="/about" class="button--green">
            About
          </nuxt-link></li> -->
          <li>
            <a href="https://github.com/davi38/custom-search" target="_blank"><button class="button-15" role="button">Github</button></a>
          </li>
        </ul>
      </nav>

      <div class="flex items-center text-lg no-underline text-white pr-6">
        <a class="" href="#">
          <i class="fab fa-facebook"></i>
        </a>
        <a class="pl-6" href="#">
          <i class="fab fa-instagram"></i>
        </a>
        <a class="pl-6" href="#">
          <i class="fab fa-twitter"></i>
        </a>
        <a class="pl-6" href="#">
          <i class="fab fa-linkedin"></i>
        </a>
      </div>
    </div>

  </nav>

  <!-- <div>
  <NuxtLink to="/">Home page</NuxtLink>

  <nuxt-link to="/post?id=688159" class="button--green">
    Post Blog
  </nuxt-link>
  <nuxt-link to="/articles" class="button--grey">
    Articles
  </nuxt-link>
</div> -->
</template>

<script>
export default {
  name: 'Navbar',
}
</script>
<style>
/* CSS */
    .button-15 {
      background-image: linear-gradient(#42A1EC, #0070C9);
      border: 1px solid #0077CC;
      border-radius: 4px;
      box-sizing: border-box;
      color: #FFFFFF;
      cursor: pointer;
      direction: ltr;
      display: block;
      font-family: "SF Pro Text", "SF Pro Icons", "AOS Icons", "Helvetica Neue", Helvetica, Arial, sans-serif;
      font-size: 17px;
      font-weight: 400;
      letter-spacing: -.022em;
      line-height: 1.47059;
      min-width: 30px;
      overflow: visible;
      padding: 4px 15px;
      text-align: center;
      user-select: none;
      -webkit-user-select: none;
      touch-action: manipulation;
      white-space: nowrap;
    }

    .button-15:disabled {
      cursor: default;
      opacity: .3;
    }

    .button-15:hover {
      background-image: linear-gradient(#51A9EE, #147BCD);
      border-color: #1482D0;
      text-decoration: none;
    }

    .button-15:active {
      background-image: linear-gradient(#3D94D9, #0067B9);
      border-color: #006DBC;
      outline: none;
    }

    .button-15:focus {
      box-shadow: rgba(131, 192, 253, 0.5) 0 0 0 3px;
      outline: none;
    }
</style>
