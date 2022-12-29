<template>
    <div class="container mt-4">
        <div class="row">
            <div class="col">
                <p class="h3 text-success fw-bold">View Contact</p>
                <p class="fst-italic">Lorem ipsum dolor sit amet consectetur adipisicing elit. Molestiae enim ab
                    corrupti, animi omnis fugit possimus, officia excepturi repellendus nam, beatae amet iure
                    dignissimos aperiam deleniti soluta. Harum, vitae ad?</p>
            </div>
        </div>
    </div>


    <!-- Spinner -->

    <div v-if="loading">
        <div class="container">
            <div class="row">
                <div class="col">
                    <SpinNer />
                </div>
            </div>
        </div>
    </div>

    <!-- Error Message -->

    <div v-if="!loading && errorMessage">
        <div class="container">
            <div class="row">
                <div class="col">
                    <p class="h3 text-danger fw-bold">{{ errorMessage }}</p>
                </div>
            </div>
        </div>
    </div>

    <div class="container">
        <div class="row align-items-center" v-if="!loading && isDone()">
            <div class="col-md-4">
                <img :src="contact.photo" alt="" class="contact-img-big">
            </div>
            <div class="col-md-6">
                <ul class="list-group">
                    <li class="list-group-item">Name: <span class="fw-bold">{{ contact.name }}</span></li>
                    <li class="list-group-item">Email: <span class="fw-bold">{{ contact.email }}</span></li>
                    <li class="list-group-item">Phone: <span class="fw-bold">{{ contact.mobile }}</span></li>
                    <li class="list-group-item">Company: <span class="fw-bold">{{ contact.company }}</span></li>
                    <li class="list-group-item">Title: <span class="fw-bold">{{ contact.title }}</span></li>
                    <li class="list-group-item">Group: <span class="fw-bold">{{ group.name }}</span></li>
                </ul>
            </div>
        </div>
        <div class="row">
            <div class="col">
                <RouterLink class="btn btn-success" to="/"><i class="fa fa-arrow-alt-circle-left"></i> Back</RouterLink>
            </div>
        </div>
    </div>

</template>

<script>
import SpinNer from '@/components/SpinNer.vue';
import { ContactService } from '@/services/ContactService';

export default {
    name: "ViewContact",
    data: function () {
        return {
            contactId: this.$route.params.contactId,
            loading: false,
            contact: {},
            group: {},
            errorMessage: null,
        };
    },
    created: async function () {
        try {
            this.loading = true;
            let response = await ContactService.getContact(this.contactId);
            this.contact = response.data;
            let groupResponse = await ContactService.getGroup(response.data);
            this.group = groupResponse.data;
            this.loading = false;
        }
        catch (error) {
            this.errorMessage = error;
            this.loading = false;
        }
    },
    methods: {
        isDone: function () {
            return Object.keys(this.contact).length > 0 && Object.keys(this.group).length > 0;
        }
    },
    components: { SpinNer }
}
</script>

<style scoped>
.contact-img-big {
    width: 200px;
    border-radius: 50%;
}
</style>