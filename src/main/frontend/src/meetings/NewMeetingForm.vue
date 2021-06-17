<template>
  <div>
    <form @submit.prevent="addNewMeeting()" v-if="adding">
      <h3>Dodaj nowe spotkanie</h3>
      <label>Nazwa</label>
      <input type="text" v-model="meeting.title">
      <label>Opis</label>
      <textarea v-model="meeting.description"></textarea>
      <button>Dodaj</button>
      <span class="error" v-if="error">Spotkanie musi mieć nazwę!</span>
    </form>
    <button @click="adding = true" v-else>Dodaj nowe spotkanie</button>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                meeting: {participants: []},
                adding: false,
                error: false
            };
        },
        methods: {
            addNewMeeting() {
                this.error = false;
                if (this.meeting.title) {
                    this.$emit('added', this.meeting);
                    this.meeting = {participants: []};
                    this.adding = false;
                } else {
                    this.error = true;
                }
            }
        }
    }
</script>

<style scoped>
  .error {
    color: #F00;
  }
</style>
