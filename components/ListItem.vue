<template>
  <div class="w-full p-4 border b-gray-200 flex items-center justify-between">
    <span class="text-xl font-semibold"> 
      <slot/> 
    </span>
    <!-- Action Buttons-->
    <div class="w-32 space-x-2">
      <slot name="actions"></slot>
    </div>
  </div>
</template>