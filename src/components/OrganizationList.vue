<template>
    <div id="main">
        <div id="organization-list">
            <div class="columns" id="header">
                <div class="column">Name</div>
                <div class="column">Legal Entity</div>
                <div class="column">Actions</div>
            </div>
            <p v-if="organizations.length < 1" class="empty-table">
                No organizations
            </p>
            <ul>
                <li v-for="organization in organizations" :key="organization.id" class="columns">
                    <div id="name" class="column">
                        <div v-if="editing === organization.id">
                            <input type="text" v-model="organization.name"/>
                        </div>
                        <div v-else>{{organization.name}}</div>
                    </div>
                    <div id="legalEntity" class="column">
                        <div v-if="editing === organization.id">
                            <input type="text" v-model="organization.legalEntity"/>
                        </div>
                        <div v-else>{{organization.legalEntity}}</div>
                    </div>
                    <div id="actions" class="column">
                        <div v-if="editing === organization.id">
                            <button @click="editOrganization(organization)">Save</button>
                            <button class="muted-button" @click="cancelEdit(organization)">Cancel</button>
                        </div>
                        <div v-else>
                            <button @click="editMode(organization)">Edit</button>
                            <button @click="$emit('delete:organization', organization.id)">Delete</button>
                        </div>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'organization-list',
        props: {
            organizations: Array,
        },
        data() {
            return {
                editing: null,
            }
        },
        methods: {
            editMode(organization) {
                this.cachedOrganization = Object.assign({}, organization);
                this.editing = organization.id
            },
            editOrganization(organization) {
                if (organization.name === '' || organization.legalEntity === '') return;
                this.$emit('edit:organization', organization.id, organization);
                this.editing = null
            },
            cancelEdit(organization) {
                Object.assign(organization, this.cachedOrganization);
                this.editing = null;
            }

        }
    }
</script>

<style scoped>
    #header {
        font-weight: bold;
    }

    #organization-list {
        margin-top: 3rem;
    }
</style>