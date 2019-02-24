<template>
    <div class="container">
      <h1>New Contact</h1>
      <div>
        First Name: <input v-model="newContactFirstName">
        Last Name: <input v-model="newContactLastName">
        Email:  <input v-model="newContactEmail">
        Phone Number: <input v-model="newContactPhoneNumber">
        Middle Name: <input v-model="newContactMiddleName">
        Bio:  <input v-model="newContactBio">
        <button v-on:click="createContact()">Create</button>
      </div>
    
      <h1>All Contacts</h1>
      <div v-for="contact in contacts">
        <h2>{{ contact.first_name + " " + contact.last_name}}</h2>
        <button v-on:click="showContact(contact)">Show More</button>
        <div v-if="currentContact === contact">
        <p>First Name: {{ contact.first_name }}</p>
        <p>Last Name: {{ contact.last_name }}</p>
        <p>Email: {{contact.email}} </p>
        <p>Phone Number: {{contact.phone_number}} </p>
        <p>Middle Name: {{contact.middle_name}} </p>
        <p>Bio: {{contact.bio}} </p>

        <div>
          <h4>Edit Contact</h4>
        </div>
        <div>
          First Name: <input v-model="contact.first_name">
          Last Name: <input v-model="contact.last_name">
          Email: <input v-model="contact.email">
          Phone Number: <input v-model="contact.phone_number">
          Middle Name: <input v-model="contact.middle_name">
          Bio: <input v-model="contact.bio">
          <button v-on:click="updateContact(contact)" class="btn btn-success">Update</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      contacts: [],
      newContactFirstName: "",
      newContactLastName: "",
      newContactEmail: "",
      newContactPhoneNumber: "",
      newContactMiddleName: "",
      newContactBio: "",
      currentContact: {}
    };
  },
  created: function() {
    axios.get("/api/contacts").then(response => {
      this.contacts = response.data;
    });
  },
  methods: {
    createContact: function() {
      console.log("Create the Contact...");
        var params = {
                      first_name: this.newContactFirstName,
                      last_name: this.newContactLastName,
                      email: this.newContactEmail,
                      phone_number: this.newContactPhoneNumber,
                      middle_name: this.newContactMiddleName,
                      bio: this.newContactBio
                      };
        axios.post("api/contacts", params)
          .then(response => {
            console.log("Success", response.data);
            this.contacts.push(response.data);
          });
    },
    showContact: function(contact) {
      if (this.currentContact === contact) {
        this.currentContact = {};
        } else {
          this.currentContact = contact;
        }
      },
    updateContact: function(inputContact) {
      var params = {
                    first_name: inputContact.first_name,
                    last_name: inputContact.last_name, 
                    email: inputContact.email,
                    phone_number: inputContact.phone_number,
                    middle_name: inputContact.middle_name,
                    bio: inputContact.bio
                    };

                    axios.patch("/api/contacts/" + inputProduct.id, params)
                      .then(response => {
                        console.log("Success", response.data);
                        inputProduct = response.data;
                      });
    }
  }
    
};
</script>