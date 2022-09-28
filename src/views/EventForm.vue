<template>
  <div>
    <h1>Create an event</h1>
    <form @submit.prevent="saveEvent">
      <BaseInput v-model="event.category" type="text" label="Category" />
      <h3>Name & describe your event</h3>

      <BaseInput v-model="event.title" type="text" label="Title" />

      <BaseInput v-model="event.description" type="text" label="Description" />

      <h3>Where is your event?</h3>

      <BaseInput v-model="event.location" type="text" label="Location" />

      <h3>Who is your organizer?</h3>
      <label>Select an Organizer</label>
      <select v-model="event.organizer.id">
        <option
          v-for="option in GStore.organizer"
          :value="option.id"
          :key="option.id"
          :selected="option.id === event.organizer.id"
        >
          {{ option.name }}
        </option>
      </select>
      <button type="submit">Submit</button>
    </form>

    <pre>{{ event }}</pre>
  </div>
</template>

<script>
import EventService from '@/services/EventService.js'
export default {
  inject: ['GStore'],
  data() {
    return {
      event: {
        category: '',
        title: '',
        description: '',
        location: '',
        organizer: { id: '', name: '' }
      }
    }
  },
  methods: {
    saveEvent() {
      EventService.saveEvent(this.event)
        .then((response) => {
          console.log(response)
          this.$router.push({
            name: 'EventLayoutView',
            params: { id: response.data.id }
          })
          this.GStore.flashMessage =
            'You are succcessfully add a new event for ' + response.data.title
          setTimeout(() => {
            this.GStore.flashMessage = ''
          }, 3000)
        })
        .catch(() => {
          this.$router.push('NetworkError')
        })
    }
  }
}
</script>
