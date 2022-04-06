<template>
  <div class="absolute bottom-10 rounded-xl bg-gray-800 p-5">
    <div
      class="h-10 w-10 rounded-lg p-2 text-gray-300 transition-all hover:bg-gray-700 hover:text-white active:scale-95 active:bg-gray-500"
      @click="$emit('playBtnClick')"
    >
      <svg viewBox="0 0 24 24" class="h-full w-full">
        <polygon class="fill-current stroke-current" points="5,0 5,24 24,12" />
      </svg>
    </div>
  </div>
</template>

<script>
export default {};
</script>
