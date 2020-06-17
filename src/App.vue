<template>
    <div id="app" class="container">
        <figure class="image">
            <img alt="Bulma logo" src="./assets/bulma.png">
        </figure>

        <section class="hero is-primary">
            <div class="hero-body">
                <div class="container">
                    <h1 class="title">
                        Organizations
                    </h1>
                </div>
            </div>
        </section>

        <OrganizationForm @add:organization="addOrganization"/>

        <OrganizationList :organizations="organizations"
                          @delete:organization="deleteOrganization"
                          @edit:organization="editOrganization"/>

    </div>
</template>

<script>
    import OrganizationList from './components/OrganizationList';
    import OrganizationForm from './components/OrganizationForm';

    export default {
        name: 'App',
        components: {
            OrganizationForm: OrganizationForm,
            OrganizationList: OrganizationList
        },
        data() {
            return {
                organizations: []
            }
        },
        mounted() {
            this.getOrganizations();
        },
        methods: {
            async getOrganizations() {
                try {
                    const response = await fetch('http://localhost:8000/api/organizations');
                    this.organizations = await response.json()
                } catch (error) {
                    alert(error);
                }
            },

            async addOrganization(organization) {
                try {
                    const response = await fetch('http://localhost:8000/api/organization', {
                        method: 'POST',
                        body: JSON.stringify(organization),
                        headers: {"Content-type": "application/json; charset=UTF-8"}
                    });
                    const data = await response.json();
                    this.organizations = [...this.organizations, data];
                } catch (error) {
                    alert(error)
                }
            },

            async editOrganization(id, updatedOrganizations) {
                try {
                    const response = await fetch(`http://localhost:8000/api/organization/${id}`, {
                        method: 'PUT',
                        body: JSON.stringify(updatedOrganizations),
                        headers: {"Content-type": "application/json; charset=UTF-8"}
                    });
                    const data = await response.json();
                    this.organizations = this.organizations.map(organization => organization.id === id ? data : organization)
                } catch (error) {
                    alert(error)
                }
            },

            async deleteOrganization(id) {
                try {
                    await fetch(`http://localhost:8000/api/organization/${id}`, {
                        method: 'DELETE'
                    });
                    this.organizations = this.organizations.filter(organization => organization.id !== id)
                } catch (error) {
                    alert(error)
                }
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
        width: 75%;
    }

    img, figure {
        width: 25%;
        height: auto;
    }

    section {
        margin-bottom: 1em;
    }
</style>
