<script setup>
import { ref } from 'vue'
import AppHeader from '@/components/AppHeader.vue'
import HomeView from '@/views/HomeView.vue'
import BookingModal from '@/components/BookingModal.vue'

const isBookingOpen = ref(false)
</script>

<template>
  <div class="min-h-screen bg-white flex flex-col font-sans">
    <AppHeader @open-booking="isBookingOpen = true" />
    <HomeView />
    <BookingModal :isOpen="isBookingOpen" @close="isBookingOpen = false" />
  </div>
</template>
