<script setup lang="ts">
const castNumber = (node: any) => {
  console.log('node is', typeof node)
  node.hook.input((value: any, next: any) => next(Number(value)))
}
</script>

<template>
  <div class="max-w-xl mx-auto p-2 space-y-4">
    <h1 class="font-bold text-5xl text-gray-700">First form</h1>
    <FormKit
      type="text"
      name="name"
      id="name"
      label="Name"
      help="Your full name"
      placeholder="“John Doe“"
    />
    <FormKit
      type="range"
      name="strength"
      id="strength"
      label="Strength"
      value="5"
      validation="min:2|max:9"
      validation-visibility="live"
      min="1"
      max="10"
      step="1"
      help="How many strength points should this character have?"
      :plugin="castNumber"
    />
  </div>
</template>
