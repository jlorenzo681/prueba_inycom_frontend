<template>
    <div id="app" class="container">
        <img alt="Vue logo" src="./assets/logo.png">

        <OrganizationForm @add:organization="addOrganization"/>
        <OrganizationList :organizations="organizations"
                          @delete:organization="deleteOrganization"
                          @edit:organization="editOrganization"
        />

    </div>
</template>

<script>
    import OrganizationList from './components/OrganizationList'
    import OrganizationForm from './components/OrganizationForm';

    export default {
        name: 'App',
        components: {
            OrganizationForm: OrganizationForm,
            OrganizationList: OrganizationList
        },
        data() {
            return {
                organizations: [
                    {
                        id: 1,
                        name: 'Richard Hendricks',
                        legalEntity: 'richard@piedpiper.com',
                    },
                    {
                        id: 2,
                        name: 'Bertram Gilfoyle',
                        legalEntity: 'gilfoyle@piedpiper.com',
                    },
                    {
                        id: 3,
                        name: 'Dinesh Chugtai',
                        legalEntity: 'dinesh@piedpiper.com',
                    },
                ],
            }
        },
        methods: {
            addOrganization(organization) {
                const lastId =
                    this.organizations.length > 0
                        ? this.organizations[this.organizations.length - 1].id
                        : 0;
                const id = lastId + 1;
                const newOrganization = {...organization, id};

                this.organizations = [...this.organizations, newOrganization]
            },
            deleteOrganization(id) {
                this.organizations = this.organizations.filter(
                    organization => organization.id !== id
                )
            },
            editOrganization(id, updatedOrganization) {
                this.organizations = this.organizations.map(organization =>
                    organization.id === id ? updatedOrganization : organization
                )
            },
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
        width: 25%;
    }
</style>
