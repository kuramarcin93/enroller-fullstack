<template>
  <div>
    <new-meeting-form @added="addNewMeeting($event)"></new-meeting-form>

    <span v-if="meetings.length == 0">
               Brak zaplanowanych spotkań.
           </span>

    <h3 v-else>
      Zaplanowane zajęcia ({{ meetings.length }})
    </h3>

    <meetings-list :meetings="meetings"
                   :username="username"
                   @attend="addMeetingParticipant($event)"
                   @unattend="removeMeetingParticipant($event)"
                   @delete="deleteMeeting($event)"></meetings-list>
  </div>
</template>

<script>
    import NewMeetingForm from "./NewMeetingForm";
    import MeetingsList from "./MeetingsList";

    export default {
        components: {NewMeetingForm, MeetingsList},
        props: ['username', 'meetings'],
        data() {
            return {
				message: '',
				isError: false
            };
        },
        methods: {
            addNewMeeting(meeting) {
				this.$http.post('meetings', meeting)
                    .then((response) => {
                        this.meetings.push({id: response.body.id, name:meeting.name, description: meeting.description, participants: meeting.participants});
						this.success('');
                    })
                    .catch(response => this.failure('Błąd przy tworzeniu spotkania. Kod odpowiedzi: ' + response.status));
            },
            addMeetingParticipant(meeting) {
				meeting.participants.push(this.username);
            },
            removeMeetingParticipant(meeting) {
            	 meeting.participants.splice(meeting.participants.indexOf(this.username), 1);
            },
            deleteMeeting(meeting) {
            	this.$http.delete(`meetings/${meeting.id}`)
                    .then(() => {
                         this.meetings.splice(this.meetings.indexOf(meeting), 1);
                         this.success('');
                    })
                    .catch(response => this.failure('Błąd podczas usuwania spotkania. Kod odpowiedzi: ' + response.status));
                
            },
			success(message) {
                this.message = message;
                this.isError = false;
            },
            failure(message) {
                this.message = message;
                this.isError = true;
            },
        }
    }
</script>
