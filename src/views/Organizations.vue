<template>
    <div>
        <base-header class=" pb-6 pt-5 pt-md-2 bg-gradient-success"></base-header>

        <b-container>
            <div class="text-right">
                <b-button type="submit" variant="primary" class="btn btn-primary mt-5" v-b-modal.add-org>Add Organization</b-button>
                
                <!-- Dialogbox_org_form -->
                
                <div class="mt-3"></div>

                <b-modal
                    id="add-org"
                    ref="modal"
                    title="Add Organization"
                    @show="resetModal"
                    @hidden="resetModal"
                    @ok="handleOk"
                    ok-title="Add"
                    cancel-title="Close"
                    size="lg"
                    
                >
                    <form @submit.prevent="validate">
                        <div class="form-group">
                            <input type="text" class="form-control" v-bind:class="{ 'is-invalid': nameError }" id="name" placeholder="Organization name" v-model="name">
                            <div class="invalid-feedback" id="feedback-1" v-if="errors[0]">
                                {{ errors[0].message }}
                            </div>
                        </div>
                        <div class="form-group">
                            <input type="text" class="form-control" v-bind:class="{ 'is-invalid': emailError }" id="email" placeholder="Email" v-model="email">
                            <div class="invalid-feedback" id="feedback-2" v-if="errors[1]">
                                {{ errors[1].message }}
                            </div>
                        </div>
                        <div class="form-group">
                            <input type="text" class="form-control" v-bind:class="{ 'is-invalid': contactError }" id="contact" placeholder="Contact No" v-model="contact">
                            <div class="invalid-feedback" id="feedback-3" v-if="errors[2]">
                                {{ errors[2].message }}
                            </div>
                        </div>
                        <div class="form-group">
                            <input type="text" class="form-control" v-bind:class="{ 'is-invalid': anameError }" id="aname" placeholder="Admin name" v-model="aname">
                            <div class="invalid-feedback" id="feedback-4" v-if="errors[3]">
                                {{ errors[3].message }}
                            </div>
                        </div>
                        <div class="form-group">
                            <input type="text" class="form-control" v-bind:class="{ 'is-invalid': aemailError }" id="aemail" placeholder="Admin email" v-model="aemail">
                            <div class="invalid-feedback" id="feedback-5" v-if="errors[4]">
                                {{ errors[4].message }}
                            </div>
                        </div>
                        <div class="form-group">
                            <input type="text" class="form-control" v-bind:class="{ 'is-invalid': acontactError }" id="acontact" placeholder="Admin Contact No" v-model="acontact">
                            <div class="invalid-feedback" id="feedback-6" v-if="errors[5]">
                                {{ errors[5].message }}
                            </div>
                        </div>
                        <!-- <button class="btn btn-primary" type="submit">Validate</button> -->
                    </form>
                    <template v-slot:modal-footer="{}">
                        <div @click="$refs.modal.hide()">
                            <button class="btn btn-primary" @click="resetModal">Close</button>
                        </div>
                        <div @click="addOrg">
                            <button class="btn btn-primary" type="submit" @click="validate">Add Oranization</button>
                        </div>
                    </template>
                    
                </b-modal>

            </div>

        <!-- Organization Cards -->
        <div v-if="items.length">
            <b-row>
                <b-col cols="12" sm="4" class="my-1" :key="index" v-for="(item, index) in paginatedItems">
                    <b-card
                    class=" mt-4"
                    >
                    <b-card-title align='left' style="color:#7C8DA0" class="text-uppercase d-flex justify-content-between mt-1">
                        {{item.oname}}
                        <!-- Dropdown option -->
                        <b-dropdown  variant="link" toggle-class="text-decoration-none" no-caret class="mt--3">
                            <template #button-content>
                                <i class="fa fa-ellipsis-v" aria-hidden="true"></i>
                            </template>
                            <b-dropdown-item-button>View</b-dropdown-item-button>
                            <b-dropdown-item-button>Edit</b-dropdown-item-button>
                            <b-dropdown-item-button @click="deleteCard(item.id)">Delete</b-dropdown-item-button>
                        </b-dropdown>
                    </b-card-title>
                        <h3 class="card-text text-dark fw-bold">{{item.title}}</h3>
                        <small class="card-text" >{{item.body}}</small>
                        <footer class="mt-4">
                            <small><cite title="Source Title" class="text-success">4.56%</cite> Since last month </small>
                        </footer>
                    </b-card>
                </b-col>
            </b-row>
        </div>

        <div class="text-center mt-8 mb-8" v-else> 
                <b-icon icon="search" font-scale="10px"></b-icon>
                <div class="text-muted text-center mt-2">NO ORGANIZATIONS FOUND</div>
            </div>   
        <!-- Pagination -->
        <div class="pagination-container">
            <b-row  v-if="items.length > perPage">
                <b-col class="my-3">
                    <b-pagination
                    @change="onPageChanged"
                    :total-rows="totalRows"
                    :per-page="perPage"
                    v-model="currentPage"
                    class="my-0"
                    align="right"
                    />
                </b-col>
            </b-row>
        </div>
        </b-container>
    </div>
</template>

<script>
const items = [
    {
    id: 1,
    oname: "Hello",
    title: "Primary",
    body: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Amet minim mollit non deserunt ullamco est sit aliqua dolor do amet sint.",
    
  },
  {
    id: 2,
    oname: "Hello",
    title: "Secondary",
    body: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Amet minim mollit non deserunt ullamco est sit aliqua dolor do amet sint.",
    
  },
  {
    id: 3,
    oname: "Hello",
    title: "Success",
    body: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Amet minim mollit non deserunt ullamco est sit aliqua dolor do amet sint.",
    
  },
  {
    id: 4,
    oname: "Hello",
    title: "Info",
    body: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Amet minim mollit non deserunt ullamco est sit aliqua dolor do amet sint.",
    
  },
  {
    id: 5,
    oname: "Hello",
    title: "Warning",
    body: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Amet minim mollit non deserunt ullamco est sit aliqua dolor do amet sint.",
    
  },
  {
    id: 6,
    oname: "Hello",
    title: "Danger",
    body: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Amet minim mollit non deserunt ullamco est sit aliqua dolor do amet sint.",
    
  },
  {
    id: 7,
    oname: "Hello",
    title: "Rffff",
    body: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Amet minim mollit non deserunt ullamco est sit aliqua dolor do amet sint.",
    
  },
  {
    id: 8,
    oname: "Hello World",
    title: "Fdddd",
    body: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Amet minim mollit non deserunt ullamco est sit aliqua dolor do amet sint.",
    
  }
];

import BaseHeader from '../components/BaseHeader.vue'
export default {
  components: { BaseHeader },

  data() {
      return {

        // Validations
        name: '',
        email: '',
        contact: '',
        aname: '',
        aemail: '',
        acontact: '',
        nameError: false,
        emailError: false,
        contactError: false,
        anameError: false,
        aemailError: false,
        acontactError: false,
        errors: [],

        // Pagination
        items: items,
        paginatedItems: items,
        currentPage: 1,
        perPage: 6,
        totalRows: items.length

      }
    },

    mounted() {
        this.paginate(this.perPage, 0);
        
      },
    
    computed: {},

    methods: {

        // Validations
        validate() {
				this.errors = [];
				var len = this.name.length;
				if (len < 5 || len > 20) {
					this.nameError = true;
					this.errors.push({
						'message': 'Name must be between 5 to 20 characters long.'
					});
				} else {
					document.getElementById('name').className = "form-control is-valid";
					this.errors.push({
						'message': ''
					});
					document.getElementById('feedback-1').className = "valid-feedback";
				}
				// email validate
                var regex = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/;
				if(this.email.length < 20 || this.email.search(regex) == null) {
					this.emailError = true;
					this.errors.push({
						'message': 'Please provide a valid email address.'
					});
				} else {
					document.getElementById('email').className = "form-control is-valid";
					this.errors.push({
						'message': 'Validated.'
					});
					document.getElementById('feedback-2').className = "valid-feedback";
				}
				// contact validate
				var regex = /^[0-9 .-]+$/;
				if(this.contact.length !== 10 || this.contact.match(regex) == null) {
					this.contactError = true;
					this.errors.push({
						'field': 'contact',
						'message': 'Please provide a valid number'
					});
				} else {
					document.getElementById('contact').className = "form-control is-valid";
					this.errors.push({
						'message': 'Validated.'
					});
					document.getElementById('feedback-3').className = "valid-feedback";
				}
                // Admin name validate
                var len = this.aname.length;
				if (len < 5 || len > 20) {
					this.anameError = true;
					this.errors.push({
						'message': 'Name must be between 5 to 20 characters long.'
					});
				} else {
					document.getElementById('aname').className = "form-control is-valid";
					this.errors.push({
						'message': ''
					});
					document.getElementById('feedback-4').className = "valid-feedback";
				}
                // Admin email validate
                var regex = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/;
				if(this.aemail.length < 20 || this.aemail.search(regex) == null) {
					this.aemailError = true;
					this.errors.push({
						'message': 'Please provide a valid email address.'
					});
				} else {
					document.getElementById('aemail').className = "form-control is-valid";
					this.errors.push({
						'message': 'Validated.'
					});
					document.getElementById('feedback-5').className = "valid-feedback";
				}
                // Admin contact validate
				var regex = /^[0-9 .-]+$/;
				if(this.acontact.length !== 10 || this.acontact.match(regex) == null) {
					this.acontactError = true;
					this.errors.push({
						'field': 'contact',
						'message': 'Please provide a valid number'
					});
				} else {
					document.getElementById('acontact').className = "form-control is-valid";
					this.errors.push({
						'message': 'Validated.'
					});
					document.getElementById('feedback-6').className = "valid-feedback";
				}
                if (this.errors.length === 0) {
                    this.formIsValid = true;
                }
			},

            //clear form
            resetModal() {
                this.name = '';
                this.email = '';
                this.contact = '';
                this.aname = '';
                this.aemail = '';
                this.acontact = '';
                this.nameError= false,
                this.emailError= false,
                this.contactError= false,
                this.anameError= false,
                this.aemailError= false,
                this.acontactError= false,
                this.errors = [];
            },

        // Pagination    
        paginate(page_size, page_number) {
            let itemsToParse = this.items;
            this.paginatedItems = itemsToParse.slice(
                page_number * page_size,
                (page_number + 1) * page_size
            );
            },
            onPageChanged(page) {
            this.paginate(this.perPage, page - 1);

        },

        //add cards
        addOrg(){
            
            if(this.name.length === 0) return;

           if( this.validate){
            this.items.push({
                    oname: this.name,
                });
                this.paginate(this.perPage, this.currentPage - 1);
                this.totalRows = this.items.length;
                this.$refs.modal.hide();
           }
                // this.name= '',
                // this.email= '',
                // this.contact= '',
                // this.aname= '',
                // this.aemail= '',
                // this.acontact= '',
                // Close the modal
        },

        deleteCard(id){
            // this.items = this.items.filter(item => item.id !== id)
            const index = this.items.findIndex(item => item.id === id)
            this.items.splice(index, 1);
            this.paginate(this.perPage, this.currentPage - 1);
            this.totalRows = this.items.length;

            if (this.currentPage > 1 && this.paginatedItems.length === 0) {
                this.currentPage -= 1;
            }
            this.paginate(this.perPage, this.currentPage - 1);
        }
    }

}
</script>

<style>
.dropdown-toggle::after {
  display: none;
}


</style>