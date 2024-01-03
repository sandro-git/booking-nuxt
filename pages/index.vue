<script setup lang="ts">
const route = useRoute()

function createTimeSlots(startTime:string, endTime:string, slotDurationMinutes:number) {
  const timeSlots = [];
  const start = new Date(`2022-01-01T${startTime}`);
  const end = new Date(`2022-01-01T${endTime}`);
  const slotDuration = slotDurationMinutes * 60000; // Convert minutes to milliseconds

  if (start >= end) {
    throw new Error("End time must be greater than start time.");
  }

  let currentSlot = start;

  while (currentSlot < end) {
    const slotEndTime = new Date(currentSlot.getTime() + slotDuration);
    timeSlots.push({
      startTime: currentSlot.toLocaleTimeString([], {hour12:false, hour: "2-digit", minute: "2-digit" }),
      endTime: slotEndTime.toLocaleTimeString([], {hour12:false, hour: "2-digit", minute: "2-digit" }),
    });

    currentSlot = slotEndTime;
  }

  return timeSlots;
}

// Example usage:
const startTime = "08:00"; // Start time in HH:MM format (24-hour clock)
const endTime = "16:00";   // End time in HH:MM format (24-hour clock)
const slotDurationMinutes = 30; // Duration of each time slot in minutes

const timeSlots = createTimeSlots(startTime, endTime, slotDurationMinutes);

const timeSlot =ref(timeSlots[0])

</script>

<template>
  <div class="container mx-auto">
    <h1 class="text-3xl text-center mb-8">Nuxt Booking</h1>
    <USelect v-model="timeSlot" :options="timeSlots" option-attribute="startTime" />
  </div>
</template>
