<script setup>
    import { ref, onMounted } from 'vue';
    import api from '../../api';
    const urlapi = 'http://127.0.0.1:8000';
    const posts = ref([]);
    const fetchDataPosts = async () => {
        await api.get('/api/posts')
        .then(response => {
            posts.value = response.data.data.data
        });
    }
    const deletePost = async (id) => {
        await api.delete(`/api/posts/${id}`)
        .then(() => {
            fetchDataPosts();
        })
    };
    onMounted(() => {
        fetchDataPosts();
    });
</script>

<template>
    <div class="container mt-5 mb-5">
        <div class="row">
            <div class="col-md-12">
                <router-link :to="{ name: 'posts.create' }" class="btn btn-md btn-success rounded shadow border-0 mb-3">ADD NEW POST</router-link>
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <table class="table table-bordered">
                            <thead class="bg-dark text-white">
                                <tr>
                                    <th scope="col">Image</th>
                                    <th scope="col">Title</th>
                                    <th scope="col">Content</th>
                                    <th scope="col" style="width:15%">Actions</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-if="posts.length == 0">
                                    <td colspan="4" class="text-center">
                                        <div class="alert alert-danger mb-0">
                                            Data Belum Tersedia!
                                        </div>
                                    </td>
                                </tr>
                                <tr v-else v-for="(post, index) in posts" :key="index">
                                    <td class="text-center">
                                        <img :src="urlapi + post.image" width="200" class="rounded-3"/>
                                    </td>
                                    <td>{{ post.title }}</td>
                                    <td>{{ post.content }}</td>
                                    <td class="text-center">
                                        <router-link :to="{ name: 'posts.edit', params:{id: post.id} }" class="btn btn-sm btn-primary rounded-sm shadow border-0 me-2">EDIT</router-link>
                                        <button @click.prevent="deletePost(post.id)" class="btn btn-sm btn-danger rounded-sm shadow border-0">DELETE</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>