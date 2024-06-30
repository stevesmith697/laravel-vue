<script setup>
import { ref, onMounted, reactive } from 'vue';
import axios from 'axios';

const users = ref([]);

const form = reactive({
  name: '',
  email: '',
  password: ''
});

const getUsers = () => {
  axios.get('/api/users')
    .then(response => {
      users.value = response.data;
    })
    .catch(error => {
      console.error(error);
    });
};

const createUser = (e) => {
  e.preventDefault();
  axios.post('/api/users', form)
    .then(response => {
      getUsers(); // Refresh the user list
      form.name = "";
      form.email = "";
      form.password = "";
      $('#userFormModal').modal('hide');
    })
    .catch(error => {
      console.error(error);
    });
};

onMounted(() => {
  getUsers();
});
</script>

<template>
  <div class="content-header">
    <div class="container-fluid">
      <div class="row mb-2">
        <div class="col-sm-6">
          <h1 class="m-0">Users</h1>
        </div>
        <div class="col-sm-6">
          <ol class="breadcrumb float-sm-right">
            <li class="breadcrumb-item"><a href="#">Home</a></li>
            <li class="breadcrumb-item active">Users</li>
          </ol>
        </div>
      </div>
    </div>
  </div>

  <div class="content">
    <div class="container-fluid">
      <div class="d-flex justify-content-between">
        <div class="d-flex">
          <button type="button" class="mb-2 btn btn-primary" data-toggle="modal" data-target="#userFormModal">
            <i class="fa fa-plus-circle mr-1"></i>
            Add New User
          </button>
        </div>
      </div>
      <div class="card">
        <div class="card-body">
          <table class="table table-bordered">
            <thead>
              <tr>
                <th><input type="checkbox" /></th>
                <th style="width: 10px">#</th>
                <th>Name</th>
                <th>Email</th>
                <th>Registered Date</th>
                <th>Role</th>
                <th>Options</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(user, index) in users" :key="user.id">
                <td><input type="checkbox" /></td>
                <td>{{ index + 1 }}</td>
                <td>{{ user.name }}</td>
                <td>{{ user.email }}</td>
                <td>-</td>
                <td>-</td>
                <td>-</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>

  <div class="modal fade" id="userFormModal" data-backdrop="static" tabindex="-1" role="dialog"
    aria-labelledby="staticBackdropLabel" aria-hidden="true">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="staticBackdropLabel">
            <span>Add New User</span>
          </h5>
          <button type="button" class="close" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <form @submit="createUser">
          <div class="modal-body">
            <div class="form-group">
              <label for="name">Name</label>
              <input v-model="form.name" name="name" type="text" class="form-control" id="name"
                aria-describedby="nameHelp" placeholder="Enter full name" />
              <span class="invalid-feedback"></span>
            </div>
            <div class="form-group">
              <label for="email">Email</label>
              <input v-model="form.email" name="email" type="email" class="form-control" id="email"
                aria-describedby="nameHelp" placeholder="Enter email" />
              <span class="invalid-feedback"></span>
            </div>
            <div class="form-group">
              <label for="password">Password</label>
              <input v-model="form.password" name="password" type="password" class="form-control" id="password"
                aria-describedby="nameHelp" placeholder="Enter password" />
              <span class="invalid-feedback"></span>
            </div>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Cancel</button>
            <button type="submit" class="btn btn-primary">Save</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
