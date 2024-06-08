<script setup>
import { ref } from "vue";
import contactsData from "./contacts.json";

const initialContacts = ref(contactsData.slice(0, 5));

const sortByPopularity = () => {
  initialContacts.value.sort((a, b) => b.popularity - a.popularity);
};

const sortByName = () => {
  initialContacts.value.sort((a, b) => a.name.localeCompare(b.name));
};

const addRandomContact = () => {
  const remainingContacts = contactsData.filter(
    (contact) =>
      !initialContacts.value.some((initial) => initial.id === contact.id)
  );
  if (remainingContacts.length > 0) {
    const randomIndex = Math.floor(Math.random() * remainingContacts.length);
    const randomContact = remainingContacts[randomIndex];

    initialContacts.value.push(randomContact);
  } else {
    alert("No more contacts to add");
  }
};

const deleteContact = (id) => {
  initialContacts.value = initialContacts.value.filter(
    (contact) => contact.id !== id
  );
};
</script>

<template>
  <div class="container">
    <h1>IronContacts</h1>
    <div class="button-container">
      <button class="button" @click="addRandomContact">
        Add Random Contact
      </button>
      <button class="button" @click="sortByName">Sort by Name</button>
      <button class="button" @click="sortByPopularity">
        Sort by Popularity
      </button>
    </div>
    <table class="table">
      <thead>
        <th class="th">Picture</th>
        <th class="th">Name</th>
        <th class="th">Popularity</th>
        <th class="th">Won an Oscar</th>
        <th class="th">Won an Emmy</th>
      </thead>
      <tbody>
        <tr v-for="contact in initialContacts" :key="contact.id">
          <td class="td">
            <img :src="contact.pictureUrl" alt="contact.name" class="image" />
          </td>
          <td class="td">{{ contact.name }}</td>
          <td class="td">{{ contact.popularity.toFixed(2) }}</td>
          <td class="td">{{ contact.wonOscar ? "🏆" : "" }}</td>
          <td class="td">{{ contact.wonEmmy ? "🏆" : "" }}</td>
          <td>
            <button class="button" @click="deleteContact(contact.id)">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style>
body {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(75, 75, 75);
}

h1 {
  font-size: 35px;
  text-align: center;
  padding: 20px;
}

.button {
  padding: 15px;
  font-size: 20px;
  border-width: 2px;
  border-style: solid;
  border-color: #fff #888 #888 #fff; /* Light (top, left), Dark (bottom, right) */
  background-color: #f0f0f0;
}
.container {
  .button-container {
    display: flex;
    justify-content: space-around;
  }
  .table {
    margin-top: 40px;
    font-size: 18px;
    text-align: center;
    width: auto; 
    margin-left: auto;
    margin-right: auto;

    .image {
      width: 70px;
      height: auto;
    }

    .td {
      padding: 20px 10px 20px 0px;
    }

    .th {
      padding-left: 15px;
      padding-right: 35px;
      font-size: 20px;
    }
  }
}
</style>
