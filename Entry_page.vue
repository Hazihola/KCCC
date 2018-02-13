<template>
  <div class="entry_page container">
    <h1 class="page-header">Entry Page</h1>
    <form v-on:submit="addEntry">
        <div class="well">
            <h4>Participant Information</h4>
            <div class="form-group">
              <label>Name</label>
              <!--
              <select class="form-control" id="entryname" v-model="entry.name" v-for="user in users" :key="user.user_id" >
                <option value=""></option>
                <option value="RON"></option>
                <option value="Nik">Nik</option>
              -->
                <!--<option value="">{{user.fname}}</option>-->
              <!--
              </select>
              -->
              <select class="form-control" id="entryname" v-model="entry.name">
                <option v-for="user in users" v-bind:key="user.user_id" v-bind:value="user.fname">
                </option>
              </select>
              <br>
              <label>Event</label>
              <select class="form-control" id="entryevent" v-model="entry.event" >
                <option value=""></option>
                <option value="Soccer">Soccer</option>
                <option value="3v3 BasketBall">3v3 BasketBall</option>
                <!--<option value="">{{event.event_name}}</option>-->
              </select>
            </div>
        </div>
        <div class="well">
            <h4>Activity</h4>
            <div class="form-group">
              <label class="checkbox-inline"><input type="checkbox" value="Participate" >Participate</label>
              <label class="checkbox-inline"><input type="checkbox" value="Writeup" >Writeup</label>
              <label class="checkbox-inline"><input type="checkbox" value="Try Out" >Try Out</label>
              <label class="checkbox-inline"><input type="checkbox" value="Picture" >Picture</label>
              <label class="checkbox-inline"><input type="checkbox" value="Cheering" >Cheering</label>
              <label class="checkbox-inline"><input type="checkbox" value="Volunteer" >Volunteer</label>
            </div>
        </div>
        <div class="well">
              <div class="form-group">
                <h4>Event Date</h4>
                <input type="date" id="eventdate" v-model="entry.date"> <br>
              </div>
        </div>
        <div class="well">
          <div class="form-group">
            <h4>Coordinator Information</h4>
            <label>Coordinator ID</label>
            <input type="text" v-model="entry.coordinatorId">
          </div>
        </div>
        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'entry_page',
  data () {
    return {
      entry: {
        name: '',
        event: '',
        date: ''
      }
    }
  },
  methods: {
    fetchUser(){
        this.$http.get('http://localhost/kccc/public/index.php/api/users')
          .then(function(response){
              this.uses = response.body //JSON.parse(response.body)
          });
    },
    fetchEvent(){
        this.$http.get('http://localhost/kccc/public/index.php/api/events')
          .then(function(response){
              this.event = response.body //JSON.parse(response.body)
          });
    },
    addEntry(e){
      if(!this.entry.name || !this.entry.event || !this.entry.activity || !this.entry.date || !this.entry.coordinatorId){
        console.log('Please Fill in all required fields');
      }else{
            let newEntry = {
            name: this.entry.name,
            event: this.entry.event,
            activity: this.entry.activity,
            date: this.entry.date,
            coordinatorid: this.entry.coordinatorId
          }

          this.$http.post('http://kccc/public/index.php/event/add', newEntry)
           .then(function(response){
             this.$router.push({path: '/'});
           });
         e.preventDefault();
      }
      e.preventDefault();
    }
  },
  created: function(){
    this.fetchUser();
  },
  created: function(){
    this.fetchEvent();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>