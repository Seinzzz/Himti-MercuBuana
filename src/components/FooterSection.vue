<script setup>
import { useRoute, useRouter } from 'vue-router'
import { computed, onMounted, onUnmounted, ref } from 'vue'

const route = useRoute()
const router = useRouter()
const platforms = [
  { name: 'Facebook', href: '#' },
  { name: 'Instagram', href: '#' },
  { name: 'Twitter', href: '#' },
  { name: 'GitHub', href: '#' },
  { name: 'Linkedin', href: '#' },
]

const handleNavigation = async (item) => {
  const scrollToElement = (selector) => {
    const element = document.querySelector(selector)
    if (element) {
      element.scrollIntoView({ behavior: 'smooth' })
    } else {
      console.warn(`Element with selector '${selector}' not found.`)
    }
  }

  if (item.path.startsWith('#')) {
    if (route.name !== 'home') {
      // Jika bukan di halaman Home, navigasi ke Home terlebih dahulu
      await router.push('/')
      setTimeout(() => {
        if (item.path === '#') {
          // Gulir ke atas jika path adalah '#'
          window.scrollTo({ top: 0, behavior: 'smooth' })
        } else {
          // Gulir ke elemen target
          scrollToElement(item.path)
        }
      }, 100)
    } else {
      // Jika sudah di halaman Home
      if (item.path === '#') {
        window.scrollTo({ top: 0, behavior: 'smooth' })
      } else {
        scrollToElement(item.path)
      }
    }
  } else {
    // Navigasi ke path lain
    await router.push(item.path)
  }
}

const navItems = computed(() => {
  const pageSpecificItems = {
    Home: [
      { name: 'Home', path: '#' }, // Path diubah menjadi '#' untuk halaman Home
      { name: 'About', path: '#about' },
      { name: 'Program', path: '#program' },
      { name: 'Members', path: '#team' },
      { name: 'Contact us', path: '#contact' },
    ],
    Blog: [
      { name: 'Home', path: '/' }, // Path tetap '/' untuk halaman selain Home
      { name: 'About', path: '#about' },
      { name: 'Program', path: '#program' },
      { name: 'Members', path: '#team' },
      { name: 'Contact us', path: '#contact' },
    ],
  }
  // Kembalikan item spesifik untuk halaman aktif, fallback ke item umum jika tidak ditemukan
  return pageSpecificItems[route.name] || [{ name: 'Home', path: '/' }]
})
</script>

<template>
  <!-- Foooter -->
  <section class="bg-gray-900">
    <div
      class="mx-auto max-w-screen-xl space-y-8 overflow-hidden px-4 py-12 sm:px-6 lg:px-8"
    >
      <nav class="-mx-5 -my-2 flex list-none flex-wrap justify-center">
        <li v-for="(item, index) in navItems" :key="index" class="px-5 py-2">
          <a
            href="#"
            @click="handleNavigation(item)"
            class="nav-link text-base leading-6 text-gray-500 hover:text-main-4"
          >
            {{ item.name }}
          </a>
        </li>
      </nav>
      <div class="mt-8 flex justify-center space-x-6">
        <!-- Iterasi SocMed -->
        <a
          v-for="(platform, index) in platforms"
          :key="index"
          :href="platform.href"
          class="text-gray-400 transition-colors hover:text-main-4"
        >
          <span class="sr-only">{{ platform.name }}</span>
          <Icon
            :icon="`simple-icons:${platform.name.toLocaleLowerCase()}`"
            width="23"
            height="23"
          />
        </a>
      </div>
      <p
        class="mt-8 select-none text-center text-sm leading-6 text-gray-400 lg:text-base"
      >
        © 2024 HIMTI Mercu Buana, Inc. All rights reserved.
      </p>
    </div>
  </section>
  <!-- ========== END FOOTER ========== -->
</template>

<style scoped>
.nav-link:after {
  display: block;
  content: '';
  border-bottom: solid 3px #581c87;
  transform: scaleX(0);
  transition: transform 250ms ease-in-out;
}
.nav-link:hover:after {
  transform: scaleX(1);
}
</style>
