<template>
  <main>
    <header>
      <h1 class="logo">SECRET APP</h1>

      <h2>LOGIN</h2>
      <p>Login or creater an account to start using this app</p>
    </header>

    <form @submit.prevent="Login">
      <label>
        <span>Enter your Email</span>
        <input type="email" v-model="email" placeholder="test@test.com" />
      </label>
      <label>
        <span>Enter your Password</span>
        <input type="password" v-model="password" placeholder="*********" />
      </label>
      <input type="submit" value="Login" />
    </form>

    <footer>
      <p>
        Dont have an account? <router-link to="/register">Register</router-link>
      </p>
    </footer>
  </main>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

const email = ref('');
const password = ref('');

const Login = async () => {
  if (!email.value || !password.value) {
    return alert('Please fill in all fields');
  }

  const res = await fetch('http://localhost:3333/login', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      email: email.value,
      password: password.value,
    }),
  }).then((res) => res.json());

  if (res.success) {
    localStorage.setItem('token', res.token);
    router.push('/');
  } else {
    alert(res.message);
  }
};
</script>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: var(--primary);
  color: #fff;
}
header {
  padding: 1.5rem;
}
footer {
  background-color: #fff;
  width: 40%;
  color: var(--dark);
  text-align: center;
  padding: 1.5rem;
  padding-bottom: 3rem;
}
h2 {
  font-size: 2.125rem;
  margin-bottom: 1rem;
}
h2 + p {
  font-weight: 500;
  font-size: 1 rem;
}

form {
  flex: 1 1 0%;
  display: block;
  border-radius: 1.5rem 1.5rem 0 0;
  background-color: #fff;
  box-shadow: 0px -4px 12px 4px rgba(0, 0, 0, 0.16);
  color: var(--dark);
  padding: 4rem 1.5rem;
  width: 40%;
}

label {
  display: block;
  margin-bottom: 1.5rem;
}
label span {
  display: block;
  color: var(--gray);
  font-size: 1rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
}

input:not([type='submit']) {
  display: block;
  width: 100%;
  border-radius: 0.5rem;
  padding: 1.5rem 1rem;
  font-size: 1rem;
  color: var(--dark);
  background-color: var(--light);
}
input:not([type='submit'])::placeholder {
  color: var(--gray);
  font-style: italic;
}

input[type='submit'] {
  display: block;
  width: fit-content;
  margin: 0 auto;
  font-size: 1.5rem;
  font-weight: 700;
  color: #fff;
  background-color: var(--primary);
  padding: 1rem;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: 0.2s ease;
}

input[type='submit']:hover {
  background-color: var(--primary-dark);
}
</style>
