<script setup>
import { reactive, onBeforeMount, onMounted } from "vue";
import axios from "axios";
import router from '../router/index'
import { useToast } from "vue-toastification";
import { useRoute } from "vue-router";
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'

const toast = useToast();

const state = reactive({
    isLoading: true
});

const route = useRoute();
const jobId = route.params.id;

const formDefaultValues = {
    type: [
        'Full-Time',
        'Part-Time',
        'Remote',
        'Internship'
    ],
    salary: [
        'under $50K',
        '$50 - $60K',
        '$60 - $70K',
        '$70 - $80K',
        '$80 - $90K',
        '90 - $100K',
        '00 - $125K',
        '25 - $150K',
        '50 - $175K',
        '75 - $200K',
        'Over $200K',
    ],
}

const form = reactive({
    type: '',
    title: '',
    description: '',
    salary: '',
    location: '',
    company: {
        name: '',
        description: '',
        contactEmail: '',
        contactPhone: '',
    }
});

const getJob = async (jobId) => {
    try {
        const res = await axios.get(`/api/jobs/${jobId}`);
        Object.assign(form, res.data);
        if (res.data.company) Object.assign(form.company, res.data.company);
    } catch (error) {
        console.error("Error while geting a job", error);
    } finally {
        state.isLoading = false;
    }
}

onMounted(async () => {
    await getJob(jobId);
});

const handleSubmit = async () => {
    try {
        const newJob = {
            type: form.type,
            title: form.title,
            description: form.description,
            salary: form.salary,
            location: form.location,
            company: {
                name: form.company.name,
                description: form.company.description,
                contactEmail: form.company.contactEmail,
                contactPhone: form.company.contactPhone,
            }
        }

        const res = await axios.patch(`/api/jobs/${jobId}`, newJob);
        toast.success('Job Edit Sucessfully');
        router.push(`/jobs/${res.data.id}`);

    } catch (error) {
        console.error("Error while editing job", error);
        toast.error('Job not added, check console for more info')
    }
};

</script>
<template>
    <section v-if="!state.isLoading" class="bg-green-50">
        <div class="container m-auto max-w-2xl py-24">
            <div class="bg-white px-6 py-8 mb-4 shadow-md rounded-md border m-4 md:m-0">
                <form @submit.prevent="handleSubmit">
                    <h2 class="text-3xl text-center font-semibold mb-6">Update Job</h2>

                    <div class="mb-4">
                        <label for="type" class="block text-gray-700 font-bold mb-2">Job Type</label>
                        <select v-model="form.type" id="type" name="type" class="border rounded w-full py-2 px-3"
                            required>
                            <option v-for="val in formDefaultValues.type" :key="val" :value="val">
                                {{ val }}
                            </option>
                        </select>
                    </div>

                    <div class="mb-4">
                        <label class="block text-gray-700 font-bold mb-2">Job Listing Name</label>
                        <input v-model="form.title" type="text" id="name" name="name"
                            class="border rounded w-full py-2 px-3 mb-2" placeholder="eg. Beautiful Apartment In Miami"
                            required />
                    </div>
                    <div class="mb-4">
                        <label for="description" class="block text-gray-700 font-bold mb-2">Description</label>
                        <textarea v-model="form.description" id="description" name="description"
                            class="border rounded w-full py-2 px-3" rows="4"
                            placeholder="Add any job duties, expectations, requirements, etc"></textarea>
                    </div>

                    <div class="mb-4">
                        <label for="type" class="block text-gray-700 font-bold mb-2">Salary</label>
                        <select v-model="form.salary" id="salary" name="salary" class="border rounded w-full py-2 px-3"
                            required>
                            <option v-for="val in formDefaultValues.salary" :key="val" :value="val">{{ val }}</option>
                        </select>
                    </div>

                    <div class="mb-4">
                        <label class="block text-gray-700 font-bold mb-2">
                            Location
                        </label>
                        <input v-model="form.location" type="text" id="location" name="location"
                            class="border rounded w-full py-2 px-3 mb-2" placeholder="Company Location" required />
                    </div>

                    <h3 class="text-2xl mb-5">Company Info</h3>

                    <div class="mb-4">
                        <label for="company" class="block text-gray-700 font-bold mb-2">Company Name</label>
                        <input v-model="form.company.name" type="text" id="company" name="company"
                            class="border rounded w-full py-2 px-3" placeholder="Company Name" />
                    </div>

                    <div class="mb-4">
                        <label for="company_description" class="block text-gray-700 font-bold mb-2">Company
                            Description</label>
                        <textarea v-model="form.company.description" id="company_description" name="company_description"
                            class="border rounded w-full py-2 px-3" rows="4"
                            placeholder="What does your company do?"></textarea>
                    </div>

                    <div class="mb-4">
                        <label for="contact_email" class="block text-gray-700 font-bold mb-2">Contact Email</label>
                        <input v-model="form.company.contactEmail" type="email" id="contact_email" name="contact_email"
                            class="border rounded w-full py-2 px-3" placeholder="Email address for applicants"
                            required />
                    </div>
                    <div class="mb-4">
                        <label for="contact_phone" class="block text-gray-700 font-bold mb-2">Contact Phone</label>
                        <input v-model="form.company.contactPhone" type="tel" id="contact_phone" name="contact_phone"
                            class="border rounded w-full py-2 px-3" placeholder="Optional phone for applicants" />
                    </div>

                    <div>
                        <button
                            class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline"
                            type="submit">
                            Save Job
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </section>
    <div v-else class="text-center text-gray-500 py-6">
        <PulseLoader />
    </div>
</template>