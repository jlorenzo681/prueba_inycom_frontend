<template>
    <div id="employee-form">
        <form @submit.prevent="handleSubmit">
            <label for="name">Employee name</label>
            <input
                    id="name"
                    ref="first"
                    type="text"
                    class="input is-rounded is-primary"
                    :class="{ 'is-danger': submitting && invalidName }"
                    v-model="employee.name"
                    @focus="clearStatus"
                    @keypress="clearStatus"
            />
            <label for="email">Employee Email</label>
            <input
                    id="email"
                    type="text"
                    class="input is-rounded is-primary"
                    :class="{ 'is-danger': submitting && invalidEmail }"
                    v-model="employee.email"
                    @focus="clearStatus"
            />
            <p v-if="error && submitting" class="error-message">
                ❗ Please fill out all required fields
            </p>
            <p v-if="success" class="success-message">
                ✅ Employee successfully added
            </p>
            <button class="button is-primary">Add Employee</button>
        </form>
    </div>
</template>

<script>
    export default {
        name: 'employee-form',
        data() {
            return {
                submitting: false,
                error: false,
                success: false,
                employee: {
                    name: '',
                    email: '',
                }
            }
        },
        methods: {
            handleSubmit() {
                this.submitting = true;
                this.clearStatus();

                if (this.invalidName || this.invalidEmail) {
                    this.error = true;
                    return
                }

                this.$emit('add:employee', this.employee);
                this.$refs.first.focus();
                this.employee = {
                    name: '',
                    email: '',
                };
                this.error = false;
                this.success = true;
                this.submitting = false;
            },

            clearStatus() {
                this.success = false;
                this.error = false;
            }
        },
        computed: {
            invalidName() {
                return this.employee.name === ''
            },

            invalidEmail() {
                return this.employee.email === ''
            },
        },
    }
</script>

<style scoped>
    form {
        margin-bottom: 2rem;
    }

    [class*='-message'] {
        font-weight: 500;
    }

    .error-message {
        color: #d33c40;
    }

    .success-message {
        color: #32a95d;
    }

    label {
        margin-top: 3em;
    }

    button {
        margin-top: 1rem;
    }
</style>