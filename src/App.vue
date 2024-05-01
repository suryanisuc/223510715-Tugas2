<template>
  <div class="background">
    <img src="https://images.unsplash.com/photo-1524055988636-436cfa46e59e?q=80&w=1935&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D" />
  </div>
      <div class="Type1">
        <nav>
          <button @click="$router.go(-1)">Home</button>
          <button @click="$router.go(1)">About</button>
        </nav>
            <div class="content">
              <h1 :class="headerClass" :style="headerStyle">About</h1>
              <p :class="paragraphClass" :style="paragraphStyle">Kirimkan Email dan Isi pesan anda </p>
              <input type="text" v-model="description" class="p-2 border1" placeholder="Deskripsi">
              <input type="email" v-model="email" class="p-2 border1" @input="emailChanged" placeholder="Email">
              <p v-if="showValid" class="text-red-500">Email tidak Valid</p>
              <button :class="buttonClass" @click="buttonClicked"style="background-color: grey;">Submit</button>
            </div>
      </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import Swal from 'sweetalert2';

const description = ref('');
const email = ref('');
const headerClass = ref('text-4xl');
const headerStyle = ref({ color: 'blue' });
const paragraphClass = ref('text-gray-700');
const paragraphStyle = ref({ fontSize: '18px' });
const buttonClass = ref('bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded');
const showValid = ref(false);
const showSuccessToast = ref(false);

const emailIsValid = computed(() => /\S+@\S+\.\S+/.test(email.value));

function emailChanged() {
  showValid.value = !emailIsValid.value;
}

function buttonClicked() {
  if (emailIsValid.value && description.value !== '') {
    Swal.fire({
      title: 'Success!',
      text: `Email berhasil dikirim ke ${email.value} dengan deskripsi: ${description.value}`,
      icon: 'success'
    }).then(() => {
      showSuccessToast.value = true;
      description.value = '';
      email.value = '';
    });
  } else {
    Swal.fire({
      title: 'Error!',
      text: 'Mohon masukkan alamat email dan deskripsi yang valid.',
      icon: 'error'
    });
  }
}
</script>

<style scoped>
.Type1 {
  max-width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

nav {
  margin-bottom: 20px;
  left: 45.5%;
  top: 20%;
  position: fixed;
}

nav button {
  margin: 0 10px;
  background-color: grey;
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

nav button:hover {
  background-color: darkslategray;
}

.content {
  text-align: center;
  position: fixed;
}
.content button {
  margin-left: 50px;
}
h1{
  margin-left: 50px;
}
p{
  margin-left: 60px;
}

input {
  margin-bottom: 10px;
  display: flex;
  margin-left: 100px;
}

.text-red-500 {
  color: red;
}
.background img{
  position: fixed;
  top: 0;
  left: 0;
  min-height: 90%;
  min-width: 1500px;
  width: 100%;
  height: 100%;
  overflow: hidden;
  z-index: -2;
  object-fit: cover;
  -webkit-background-size:cover; -moz-background-size:cover; -o-background-size:cover; background-size: cover;
  filter: brightness(0.8);
}
</style>
