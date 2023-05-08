<script>
import allContacts from "./contacts.json"
export default {
  data() {
    return {
      contacts: [],
      sortBy: '',
    }
  },created() {
    this.contacts = allContacts.slice(0, 5);
  },methods: {
    AddRandomContact() {
      const randomContact = allContacts[Math.floor(Math.random() * allContacts.length)];

      if(!this.contacts.find(contact => contact.name === randomContact.name)){
        this.contacts.push(randomContact);

        if(this.sortBy === 'name'){
          this.SortByName();
        }else if(this.sortBy === 'popularity'){
          this.SortByPopularity();
        }

      }else {
        this.AddRandomContact();
      }
      
    },
    SortByPopularity() {
      this.contacts.sort((a, b) => b.popularity - a.popularity);
      this.sortBy = 'popularity';
    },
    SortByName() {
      this.contacts.sort((a, b) => a.name.localeCompare(b.name));
      this.sortBy = 'name';
    },
    Delete(_contact) {
      const indexContact = this.contacts.findIndex(contact => contact.name === _contact.name);

      this.contacts.splice(indexContact,1);
    }
  }
}

</script>

<template>
  <div>
    <h1>IronContacts</h1>
    <button @click="AddRandomContact()">Add Random contact</button>
    <button @click="SortByPopularity()">Sort by popularity</button>
    <button @click="SortByName()">Sort by name</button>
    <table>
      <tr>
        <th>Picture</th>
        <th>Name</th>
        <th>Popularity</th>
        <th>Won Oscar</th>
        <th>Won Emmy</th>
        <th>Actions</th>
      </tr>
      <tr v-for="contact in contacts" >
        <td><img :src="contact.pictureUrl"/></td>
        <td>{{ contact.name }}</td>
        <td>{{ contact.popularity }}</td>
        <td><span v-show="contact.wonOscar">X</span></td>
        <td><span v-show="contact.wonEmmy">X</span></td>
        <td><button @click="Delete(contact)">Delete</button></td>
      </tr>
    </table>
  </div>
</template>



<style>
  table{
    text-align: center;
  }

  table th{
    font-weight: 700;
  }

  table img{
    width: 120px;
  }
</style>
