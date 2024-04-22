<script setup lang="ts">
import { onMounted } from 'vue'
import { getNode } from '@formkit/core'

type Attributes = {
  strength: number
  skill: number
  dexterity: number
}

const CHARACTER_BASE_STATS: Record<string, Attributes> = {
  Warrior: {
    strength: 9,
    skill: 1,
    dexterity: 5
  },
  Mage: {
    strength: 9,
    skill: 10,
    dexterity: 8
  },
  Assassin: {
    strength: 5,
    skill: 4,
    dexterity: 10
  }
}

const classOptions: string[] = ['Warrior', 'Mage', 'Assassin']
const castRangeToNumber = (node: any) => {
  // We add a check to add the cast only to range inputs
  if (node.props.type !== 'range') return

  node.hook.input((value: any, next: (arg: any) => any) => next(Number(value)))
}

const createCharacter = async (fields: any) => {
  await new Promise((r: any) => setTimeout(r, 1000))
  console.log(fields)
  alert(JSON.stringify(fields))
}

// We add it inside a onMounted to make sure the node exists
onMounted(() => {
  // Use the IDs of the inputs you want to get, for our case the class and the attributes group
  const classNode = getNode('class')
  const attributesNode = getNode('attributes')

  // Here we are listening for the 'commit' event
  classNode?.on('commit', ({ payload }) => {
    attributesNode?.input(CHARACTER_BASE_STATS[payload])
  })
})
</script>

<template>
  <div class="bg-white max-w-md p-4 mx-auto shadow-md rounded-md">
    <div><h4 class="form-label">Creating the form</h4></div>
    <h1>New Character</h1>

    <!-- form is also an input, so it also accepts plugins -->
    <FormKit
      type="form"
      @submit="createCharacter"
      :plugins="[castRangeToNumber]"
      submit-label="Create Character"
      #default="{ value }"
    >
      <FormKit
        type="text"
        name="name"
        id="name"
        validation="required|not:Admin"
        label="Name"
        help="Your full name"
        placeholder="Please add your name"
      />

      <FormKit
        type="select"
        name="class"
        label="Class"
        id="class"
        placeholder="Select a class"
        :options="classOptions"
      />

      <FormKit type="group" name="attributes" id="attributes">
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
        />

        <FormKit
          type="range"
          name="skill"
          id="skill"
          validation="required|max:10"
          label="Skill"
          value="5"
          min="1"
          max="10"
          step="1"
          help="How many skill points should this character have?"
        />

        <FormKit
          type="range"
          name="dexterity"
          id="dexterity"
          validation="required|max:10"
          label="Dexterity"
          value="5"
          min="1"
          max="10"
          step="1"
          help="How many dexterity points should this character have?"
        />
      </FormKit>
      <pre wrap>{{ value }}</pre>
    </FormKit>
  </div>
</template>

<style scoped>
.form-label {
  @apply inline-block rounded-md bg-gray-700 text-white p-2;
}
</style>
