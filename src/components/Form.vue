<template lang="pug">
    .register-form
        h1 Register Form
        .form
            .input
                label(
                    for="first-name-input"
                ) First name:
                input(
                    type="text"
                    id="first-name-input"
                    name="first-name"
                    placeholder="Your first name"
                    v-model="form.firstName"
                )
            .input
                label(
                    for="last-name-input"
                ) Last name:
                input(
                    type="text"
                    id="last-name-input"
                    name="last-name"
                    placeholder="Your last name"
                    v-model="form.lastName"
                )
            .input
                label(
                    for="username-input"
                ) Username*:
                input(
                    type="text"
                    id="username-input"
                    name="username"
                    placeholder="Your username"
                    v-model="form.username"
                )
            .input
                label(
                    for="password-input"
                ) Password*:
                input(
                    type="password"
                    id="password-input"
                    name="password"
                    placeholder="Your password"
                    v-model="form.password"
                )
            .input
                label(
                    for="confirm-password-input"
                ) Confirm password*:
                input(
                    type="password"
                    id="confirm-password-input"
                    name="confirm-password"
                    placeholder="Confirm your password"
                    v-model="form.confirmPassword"
                )
            //- Display errors
            .error-card(:class="{hidden: errorHidden}")
                span.error-message() {{ errorMessage }}
            .submit-button
                button(
                    @click="sendRegisterForm"
                ) Submit
            p.error-message(
            ) Entries with (*) symbol are required.
</template>

<script>
export default {
    data() {
        return {
            form: {
                firstName: "",
                lastName: "",
                username: "",
                password: "",
                confirmPassword: ""
            },
            errorMessage: "Please do",
            errorHidden: true
        }
    },
    methods: {
        setAndDisplayError(text) {
            this.errorMessage = text
            this.errorHidden = false
        },
        checkLettersAndSpaces(text) {
            return /^[a-zA-Z\s]+$/.test(text)
        },
        checkLettersAndUnderscores(text) {
            return /^[a-zA-Z_]+$/.test(text)
        },
        ensuresValidFirstName() {
            if (this.form.firstName !== '' && !this.checkLettersAndSpaces(this.form.firstName)) {
                throw new Error("Please introduce only letters and spaces on your first name.")
            }
        },
        ensuresValidLastName() {
            if (this.form.lastName !== '' && !this.checkLettersAndSpaces(this.form.lastName)) {
                throw new Error("Please introduce only letters and spaces on your last name.")
            }
        },
        ensuresValidUsername() {
            if (!this.checkLettersAndUnderscores(this.form.username)) {
                throw new Error("Please introduce only letters and underscores on your username.")
            }
        },
        ensuresValidPassword() {
            if (!/(?=.{9,}).*[^0-9].*/.test(this.form.password)) {
                throw new Error("Please introduce more than 8 characters and not only numbers on your password.")
            }

            if (this.form.password !== this.form.confirmPassword) {
                throw new Error("The password must match.")
            }
        },
        sendRegisterForm() {
            // Hide the error message
            this.errorHidden = true

            // Check variables
            try {
                this.ensuresValidFirstName()
                this.ensuresValidLastName()
                this.ensuresValidUsername()
                this.ensuresValidPassword()
                alert("Register form fullfilled!")
            } catch (error) {
                this.setAndDisplayError(error.message)
            }
        }
    },
}
</script>

<style>
.register-form {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.form {
    width: 70%;
    border-style: solid;
    border-color: rgb(163, 163, 163);
    border-radius: 1%;
    border-width: 2px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    /* align-items: center; */
    padding: 20px 20px 10px 20px;
    background-color:rgb(243, 243, 243);
}

.hidden {
    display: none;
}

.input {
    margin-bottom: 10px;
}

.input > label {
    margin-right: 10px;
}

.error-card {
    background-color: rgb(216, 183, 183);
    border: 1px solid rgb(197, 138, 138);
    border-radius: 4px;
    padding: 7px;
    margin-bottom: 10px;
}

.error-message {
    color: rgb(190, 39, 39);
    font-style: italic;
}
</style>
