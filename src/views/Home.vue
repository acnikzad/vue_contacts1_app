<template>
  <div class="home">
    <div>
      <h1>Add Contact</h1>
      <p>First Name:<input type="text" v-model="newContactFirstName"></p>
      <p>Middle Name:<input type="text" v-model="newContactMiddleName"></p>
      <p>Last Name: <input type="text" v-model="newContactLastName"></p>
      <p>Email: <input type="text" v-model="newContactEmail"></p>
      <p>Phone Number: <input type="text" v-model="newContactPhoneNumber"></p>
      <button v-on:click="addContact()">Add Contact</button>

    </div>

    <div v-for="contact in contacts">
      <p>id: {{contact.id}}</p>
      <p>First Name: {{contact.first_name}}</p>
      <p>Middle Name: {{contact.middle_name}}</p>
      <p>Last Name: {{contact.last_name}}</p>
      <p>Email: {{contact.email}}</p>
      <p>Phone Number: {{contact.phone_number}}</p>
      <br>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {

      contacts: [],
      newContactFirstName: "",
      newContactMiddleName: "",
      newContactLastName: "",
      newContactEmail: "",
      newContactPhoneNumber: "",
      
    };
  },
  created: function() {
    console.log('Creating');
    axios.get("/api/contacts").then(response => {
      console.log(response.data);
      this.contacts = response.data;
    });

  },
  methods: {

  }
};
</script>