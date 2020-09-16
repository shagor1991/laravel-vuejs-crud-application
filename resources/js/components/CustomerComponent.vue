<template>
    <div>
        <div class="row justify-content-center">
                <div class="col-md-12">
                    <div class="card">
                        <div class="card-header">
                        <h4 class="card-title">Customers</h4>
                        <div class="card-tools" style="position: absolute;right: 1rem;top: .5rem;">
                        <button type="button" class="btn btn-info" @click="create">
                            Add New
                            <i class="fas fa-plus"></i>
                        </button>
                        
                        </div>
                        </div>

                        <div class="card-body">
                            <form class="mb-3">
                                <div class="row">
                                    <div class="col-md-2">
                                        <strong> Search By: </strong>
                                    </div>
                                    <div class="col-md-3">
                                        <select v-model="queryField" id="field" class="form-control">
                                            <option value="name">Name</option>
                                            <option value="email">Email</option>
                                            <option value="phone">Phone</option>
                                            <option value="age">age</option>
                                        </select>
                                    </div>
                                    <div class="col-md-7">
                                        <input v-model="query" type="text" class="form-control" placeholder="Search">
                                    </div>
                                </div>
                            </form>
                            <div class="table-responsive">
                                <table class="table table-hover table-bordered table-striped">
                                    <thead>
                                    <tr>
                                        <th scope="col">#</th>
                                        <th scope="col">Name</th>
                                        <th scope="col">Email</th>
                                        <th scope="col">Phone</th>
                                        <th scope="col">Age</th>
                                        <th scope="col">Action</th>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr v-for="(customer, index) in customers" :key="customer.id">
                                        <th scope="row">{{pagination.from +index}}</th>
                                        <td>{{customer.name}}</td>
                                        <td>{{customer.email}}</td>
                                        <td>{{customer.phone}}</td>
                                        <td>{{customer.age}}</td>
                                        <td>
                                            <button type="button" @click="show(customer)" class="btn btn-info btn-sm">
                                                <i class="fas fa-eye"></i>
                                            </button>
                                            <button type="button" @click="edit(customer)"  class="btn btn-primary btn-sm">
                                                <i class="fas fa-edit"></i>
                                            </button>
                                            <button type="button" @click="destroy(customer)" class="btn btn-danger btn-sm">
                                                <i class="fas fa-trash"></i>
                                            </button>
                                        </td>
                                    </tr>

                                    
                                    
                                    </tbody>
                                </table>
                                <pagination
                                :pagination='pagination'
                                :offset="5"
                                @paginate=" query =='' ? getData() : searchData()"
                                > </pagination>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Modal -->
            <div class="modal fade" id="customerModalLong" tabindex="-1" role="dialog" aria-labelledby="customerModalLongTitle" aria-hidden="true">
                <div class="modal-dialog" role="document">
                    <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="customerModalLongTitle">{{editStatus? 'Update Customer' : "Add Customer"}}</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <form @submit.prevent="editStatus ? update() : store()" @keydown="form.onKeydown($event)">
                    <div class="modal-body">

                        <alert-error :form="form"></alert-error>

                            <div class="form-group">
                            <label>Name</label>
                            <input v-model="form.name" type="text" name="name"
                                class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                            <has-error :form="form" field="name"></has-error>
                            </div>

                            <div class="form-group">
                            <label>Email</label>
                            <input v-model="form.email" type="text" name="email"
                                class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                            <has-error :form="form" field="email"></has-error>
                            </div>

                            <div class="form-group">
                            <label>Phone</label>
                            <input v-model="form.phone" type="text" name="phone"
                                class="form-control" :class="{ 'is-invalid': form.errors.has('phone') }">
                            <has-error :form="form" field="phone"></has-error>
                            </div>

                            <div class="form-group">
                            <label>Address</label>
                            <textarea v-model="form.address" type="text" name="address"
                                class="form-control" :class="{ 'is-invalid': form.errors.has('address') }">
                            </textarea>
                            <has-error :form="form" field="address"></has-error>
                            </div>

                            <div class="form-group">
                            <label>Age</label>
                            <input v-model="form.age" type="text" name="age"
                                class="form-control" :class="{ 'is-invalid': form.errors.has('age') }">
                            <has-error :form="form" field="age"></has-error>
                            </div>
                            
                        
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        <button type="submit" class="btn btn-primary">Save</button>
                    </div>
                    </form>
                    </div>
                </div>
            </div>

            <div
            class="modal fade"
            id="showModal"
            tabindex="-1"
            role="dialog"
            aria-labelledby="showModalLabel"
            aria-hidden="true"
            >
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="showModalLabel">{{ form.name }}</h5>

                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <strong>Email : {{ form.email }}</strong>
                    <br>
                    <strong>Phone : {{ form.phone }}</strong>
                    <br>
                    <strong>Total : {{ form.age }}</strong>
                    <br>
                    <strong>Address :</strong>
                    <address>{{ form.address }}</address>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                </div>
                </div>
            </div>
            </div>

            <vue-progress-bar></vue-progress-bar>
            <vue-snotify></vue-snotify>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                editStatus: false,
                customers: [],
                pagination: {
                    current_page: 1,
                    from: 1
                },
                query: '',
                queryField: 'name',
                form: new Form({
                    id: '',
                    name: '',
                    email: '',
                    phone: '',
                    address: '',
                    age: ''
                })
            }
        },
        watch:{
           query:function(newQ, oldQ){
               if(newQ===''){
                   this.getData();
               }else{
                   this.searchData();
               }
           } 
        },
        mounted() {
            console.log('Component mounted.')
            this.getData()
        },
        methods: {
            getData(){
                this.$Progress.start()
                axios.get('/api/customers?page=' + this.pagination.current_page)
                .then((response => {
                    console.log(response);
                    this.customers= response.data.data;
                    this.pagination= response.data.meta;
                    this.$Progress.finish()
                    
                }))
                .catch((error=> {
                    this.$Progress.fail()
                }))
            },
            searchData(){
                this.$Progress.start()
                axios.get('/api/search/customer/'+ this.queryField+ '/'+ this.query+'?page=' + this.pagination.current_page)
                .then((response => {
                    console.log(response);
                    this.customers= response.data.data;
                    this.pagination= response.data.meta;
                    this.$Progress.finish()
                }))
                .catch((error=> {
                    this.$Progress.fail()
                }))
            },
            create(){
                this.editStatus= false;
                this.form.reset();
                this.form.clear();
                $('#customerModalLong').modal('show');
            },
            show(customer) {
            this.form.reset();
            this.form.fill(customer);
            $("#showModal").modal("show");
            console.log(customer);
            },
            store(){
                this.$Progress.start();
                this.form.busy=true;
                this.form.post('/api/customers')
                .then(response=>{
                    this.getData();
                    $('#customerModalLong').modal('hide');                        
                        
                    if(this.form.successful){
                        this.$Progress.finish();
                        this.$snotify.success("Customer created successfully!", "Success");                        
                    }else{
                        this.$Progress.fail();
                        this.$snotify.error("Something went wrong try again later", "Error");
                    }
                })
                .catch(error=>{
                    this.$Progress.fail();
                })
            },
            edit(customer){
                this.editStatus= true;
                this.form.reset();
                this.form.clear();
                this.form.fill(customer);
                console.log(customer);
                $('#customerModalLong').modal('show');
            },
            update(){
                this.$Progress.start();
                this.form.busy=true;
                this.form.put('/api/customers/'+ this.form.id)
                .then(response=>{
                    this.getData();
                    $('#customerModalLong').modal('hide');                        
                        
                    if(this.form.successful){
                        this.$Progress.finish();
                        this.$snotify.success("Customer updated successfully!", "Success");                        
                    }else{
                        this.$Progress.fail();
                        this.$snotify.error("Something went wrong try again later", "Error");
                    }
                })
                .catch(error=>{
                    this.$Progress.fail();
                })
            },
            destroy(customer) {
                this.$snotify.clear();
                this.$snotify.confirm(
                "You will not be able to recover this data!",
                "Are you sure?",
                {
                    showProgressBar: false,
                    closeOnClick: false,
                    pauseOnHover: true,
                    buttons: [
                    {
                        text: "Yes",
                        action: toast => {
                        this.$snotify.remove(toast.id);
                        this.$Progress.start();
                        axios
                            .delete("/api/customers/" + customer.id)
                            .then(response => {
                            this.getData();
                            this.$Progress.finish();
                            this.$snotify.success(
                                "Customer Successfully Deleted",
                                "Success"
                            );
                            })
                            .catch(e => {
                            this.$Progress.fail();
                            console.log(e);
                            });
                        },
                        bold: true
                    },
                    {
                        text: "No",
                        action: toast => {
                        this.$snotify.remove(toast.id);
                        },
                        bold: true
                    }
                    ]
                }
                );
            }


        }
    }
</script>
