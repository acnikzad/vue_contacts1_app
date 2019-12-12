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

      <br>

      <button v-on:click="toggleInfo(contact)">See Contact Info</button>
      <div v-if="currentContact === contact">
        <p>Email: {{contact.email}}</p>
        <p>Phone Number: {{contact.phone_number}}</p>
        <button v-on:click="updateContact(contact)">Update Contact</button>
      </div>

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
      currentContact: {}
      
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
    addContact: function() {
      console.log('adding contact...');

      var params = {
        first_name: this.newContactFirstName,
        middle_name: this.newContactMiddleName,
        last_name: this.newContactLastName,
        email: this.newContactEmail,
        phone_number: this.newContactPhoneNumber,
      };

      axios.post("/api/contacts", params).then(response => {
        console.log(response.data);
        this.contacts.push(response.data);
        this.newContactFirstName = "";
        this.newContactMiddleName = "";
        this.newContactLastName = "";
        this.newContactEmail = "";
        this.newContactPhoneNumber = "";
      });
    },

    toggleInfo: function(theContact) {
      console.log('toggling info...');

      if (this.currentContact === theContact) {
        this.currentContact = null;
      } else {
        this.currentContact = theContact;
      }
    },

    updateContact: function(theContact) {
      console.log('updating contact...');
      console.log(theContact);

      var params = {
        first_name: theContact.first_name,
        middle_name: theContact.middle_name,
        last_name: theContact.last_name,
        email: theContact.email,
        phone_number: theContact.phone_number,
      };

      axios.patch(`/api/contacts/${theContact.id}`, params).then(response => {
        console.log(response.data);
        theContact.first_name = response.data.first_name;
        theContact.middle_name = response.data.middle_name;
        theContact.last_name = response.data.last_name;
        theContact.email = response.data.email;
        theContact.phone_number = response.data.phone_number;
      });
    }
  }
};
</script>