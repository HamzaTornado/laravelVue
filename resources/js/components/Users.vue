<template>
    <div class="container">
        <div class="row mt-3">
          <div class="col-md-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title">Users Table</h3>

                <div class="card-tools">
                    <button class="btn btn-success" data-toggle="modal" data-target="#addNew">Add User
                        <i class="fas fa-user-plus text-right fa-fw"></i>
                    </button>
                </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0">
                <table class="table table-hover">
                  <thead>
                    <tr>
                      <th>ID</th>
                      <th>Name</th>
                      <th>Email</th>
                      <th>Type</th>
                      <th>Bio</th>
                      <th>Registred At</th>
                      <th>Modify</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="user in users" :key="user.id">
                      <td>{{user.id}}</td>
                      <td>{{user.name}}</td>
                      <td>{{user.email}}</td>
                      <td>{{user.type | upText}}</td>
                      <td>{{user.bio}}</td>
                      <td>{{user.created_at | myTime}}</td>
                      <td>
                          <a href="" class="btn btn-primary">
                              <i class="fa fa-edit text-white"></i>
                          </a>
                          <span>&nbsp;|&nbsp;</span>
                          <a href="" class="btn btn-danger">
                              <i class="fa fa-trash text-white"></i>
                          </a>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
              <!-- /.card-body -->
            </div>
            <!-- /.card -->
          </div>
    </div>


    <!-- Modal -->
    <div class="modal fade" id="addNew" tabindex="-1" role="dialog" aria-labelledby="addNewTitle" aria-hidden="true"
     data-backdrop="static" data-keyboard="false">
        <div class="modal-dialog modal-dialog-centered" role="document">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="addNewLongTitle">Add New </h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
                </button>
            </div>

            <!-- Form-->
            <form @submit.prevent="createUser">
            <div class="modal-body">

                <div class="form-group">
                    <input v-model="form.name" type="text" name="name" placeholder="Full Name"
                        class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                    <has-error :form="form" field="name"></has-error>
                </div>

                <div class="form-group">
                    <input v-model="form.email" type="email" name="email" placeholder="Email Adresse"
                        class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                    <has-error :form="form" field="email"></has-error>
                </div>

                <div class="form-group">
                    <select name="type" v-model="form.type" type="text" id="type" placeholder="User Type"
                        class="form-control" :class="{ 'is-invalid': form.errors.has('type') }">
                        <option value="">Select User Type</option>
                        <option value="admin">Admin</option>
                        <option value="user">Standard User</option>
                        <option value="author">Auther</option>
                    </select>
                    <has-error :form="form" field="type"></has-error>
                </div>
                <div class="form-group">
                    <textarea v-model="form.bio" type="text" name="bio"
                        placeholder="Short Bio for User"
                        class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }">
                    </textarea>
                    <has-error :form="form" field="bio"></has-error>
                </div>

                <div class="form-group">
                    <input v-model="form.password" type="password" name="password" placeholder="Password"
                        class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                    <has-error :form="form" field="password"></has-error>
                </div>
            </div>

            <div class="modal-footer">
                <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                <button type="submit" class="btn btn-primary">Create</button>
            </div>
            <!--end Form-->
            </form>
            </div>
        </div>
    </div>
</div>

</template>

<script>
export default {
    data(){
        return {
            users: {},
            form: new Form({
                name: '',
                email: '',
                password: '',
                type: '',
                bio: '',
                photo: ''
            })
        }
    },
    methods:{
        loadUsers(){
            axios.get("api/user").then(
                ({data}) => (this.users = data.data)
            );
        },
        createUser(){
            this.$Progress.start();
            this.form.post('api/user');
            this.$Progress.finish();
        }
    },
    created() {
        this.loadUsers();

    }
}
</script>



