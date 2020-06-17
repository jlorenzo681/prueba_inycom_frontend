<template>
    <div id="organization-form">
        <form @submit.prevent="handleSubmit">
            <label for="name">Organization name</label>
            <input
                    id="name"
                    ref="first"
                    type="text"
                    class="input is-rounded is-primary"
                    :class="{ 'is-danger': submitting && invalidName }"
                    v-model="organization.name"
                    @focus="clearStatus"
                    @keypress="clearStatus"
            />
            <label for="legalEntity">Legal Entity</label>
            <input
                    id="legalEntity"
                    type="text"
                    class="input is-rounded is-primary"
                    :class="{ 'is-danger': submitting && invalidLegalEntity }"
                    v-model="organization.legalEntity"
                    @focus="clearStatus"
            />
            <p v-if="error && submitting" class="error-message">
                ❗ Please fill out all required fields
            </p>
            <p v-if="success" class="success-message">
                ✅ Organization successfully added
            </p>
            <button class="button is-primary">Add Organization</button>
        </form>
    </div>
</template>

<script>
    export default {
        name: 'organization-form',
        data() {
            return {
                submitting: false,
                error: false,
                success: false,
                organization: {
                    name: '',
                    legalEntity: '',
                }
            }
        },
        methods: {
            handleSubmit() {
                this.submitting = true;
                this.clearStatus();

                if (this.invalidName || this.invalidLegalEntity) {
                    this.error = true;
                    return
                }

                this.$emit('add:organization', this.organization);
                this.$refs.first.focus();
                this.organization = {
                    name: '',
                    legalEntity: '',
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
                return this.organization.name === ''
            },

            invalidLegalEntity() {
                return this.organization.legalEntity === ''
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