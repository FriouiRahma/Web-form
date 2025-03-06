<template>
    <form @submit.prevent="handleSubmit">
        <label>Email: </label>
        <input type="email" required v-model="data.email" />

        <label>Password: </label>
        <input type="password" required v-model="data.password" />
        <div v-if="passwordError" class="error">{{ passwordError }}</div>

        <label>Role: </label>
        <select v-model="data.role">
            <option value="developer"> Web Developer </option>
            <option value="designer"> Web Designer</option>
        </select>

        <label>Skills</label>
        <input type="text" v-model="data.tempSkill" @keyup="addSkill">
        <div v-for="skill in data.skills" :key="skill" class="pill">
           <span @click="deleteSkill(skill)">{{ skill }}</span> 
        </div>

        <div class="terms">
            <input type="checkbox" required v-model="data.terms">
            <label>Accept terms and conditions</label>
        </div>
        <div class="submit">
            <button>Create an account</button>
        </div>

    </form>

    <p>email: {{ data.email }}</p>
    <p>password: {{ data.password }}</p>
    <p>role: {{ data.role }}</p>
    <p>terms accepted: {{ data.terms }}</p>
</template>

<script setup>
import { ref } from 'vue'

const data = ref({
    email: '',
    password: '',
    role: 'developer',
    terms:false,
    tempSkill: '',
    skills: []
})
const passwordError = ref('')

const addSkill = (e) => {
    if(e.key === ',' && data.value.tempSkill != ','){
        if(!data.value.skills.includes(data.value.tempSkill)) {
            data.value.skills.push(data.value.tempSkill)
        }
        data.value.tempSkill = ""
    }
}

const deleteSkill = (skill) => {
    data.value.skills = data.value.skills.filter((item) => {
        return item !== skill
    } )
}

const handleSubmit = () => {
    passwordError.value = data.value.password.length > 5 ? 
    '' : 'Password must be at least 6 chars long'

    if(!passwordError.value) {
        console.log('email:', data.value.email)
        console.log('password:', data.value.password)
        console.log('role:', data.value.role)
        console.log('skills:', data.value.skills)
        console.log('terms accepted:', data.value.terms)
    }
}

</script>

<style>
form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}

label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}
input, select {
    display: block;
    padding: 10px 6px;
    width: 100%;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
}
input[type = "checkbox"] {
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px;

}
.pill {
    display: inline-block;
    margin: 20px 10px 0 0;
    padding: 6px 12px;
    background: #eee;
    border-radius: 20px;
    font-size: 12px;
    letter-spacing: 1px;
    font-weight: bold;
}
button {
    background: #0b6dff;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
}
.submit {
    text-align: center;
}
.error {
    color: #ff0262;
    margin-top: 10px;
    font-size: 0.8em;
    font-weight: bold;
}
</style>