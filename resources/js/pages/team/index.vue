

<template>
<div>
  <button
      type="button"
      class="btn"
      @click="showModal"
    >
      Open Modal!
    </button>

    <modal
      v-show="isModalVisible"
      @close="closeModal"
    />
  <transition name="modal-fade"></transition>
  <div id="edit-form" v-if="modal">
    <div v-if="success != ''" class="alert alert-success" role="alert">
      {{success}}
    </div>
    <form>
      <div class="container" style="    border: 1px solid #ddd;
      border-radius: 3px;
      position: relative;">
        <button class="button-close"  @click.prevent="close">X</button>
        <div class="row">
          <div class="input-group mb-3 mt-3">
          <div class="col">
            <input type="text" v-model="name" class="form-control" placeholder="First name">
          </div>
          <div class="col">
            <input type="text" class="form-control" placeholder="Last name">
          </div>
        </div>

                        <strong>Image:</strong>
                        <input type="file" class="form-control" v-on:change="onImageChange">
      </div>
      <div class="input-group mb-3">
        <div class="btn btn-success btn-sm"><button class="btn">Save</button></div>
      </div>
      </div>
    </form>
  </div>
  <div class="col-md-12">
    <Usernav/>
         <div class="card-header d-flex justify-content-between align-items-center">
                        <h5 class="mb-0">My Team</h5>

                    </div>
    </div>

     <div class="content col-md-12">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th style="width: 100px"> Id </th>
                                    <th> Image </th>
                                    <th> Title </th>
                                    <th> Slug </th>
                                    <th style="width: 170px"> Action </th>
                                </tr>
                            </thead>
                            <tbody v-if="products.length">
                                <tr v-for="product in products" :key="product.id">
                                    <td style="width: 100px"> {{ product.id }} </td>
                                    <td>
                                        <div style="max-width: 150px; max-height: 150px; overflow:hidden">
                                            <img :src="product.image" alt="" class="img-fluid">
                                        </div>
                                    </td>
                                    <td> {{ product.title }} </td>
                                    <td> {{ product.slug }} </td>
                                    <td style="width: 170px">
                                        <button  @click.prevent="showModal(1)">Edit</button>
                                        <a @click.prevent="deleteProduct(product)" href="#" class="btn btn-danger btn-sm">Delete</a>
                                    </td>
                                </tr>
                            </tbody>
                            <tbody v-else>
                                <tr>
                                    <td colspan="4">
                                        <h5 class="text-center mt-4 mb-4">No products found.</h5>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>

</div>

</template>

<script>

//import { mapGetters } from 'axios'

import { mapGetters } from 'vuex'

//Vue.use(axios);
import Usernav from '../../components/Usernav.vue'
import modal from './modal.vue';

export default {
  components: {
    Usernav: Usernav, modal,
  },
  mounted() {
        this.loadProducts();
        //this.createOverlay();
    },
  data: () => ({
    appName: window.config.appName,
      products: [],
      modal:false
  }),

  computed: mapGetters({
    user: 'auth/user'
  }),

  methods: {
    createOverlay() {
      this.overlay = document.createElement('div')
      this.overlay.className = 'overlay'
      //this.overlay.addEventListener('click', () => this.close())
      document.body.appendChild(this.overlay)
      document.body.classList.add('no-scrollable')
    },

    showModal: function (index) {

      this.modal=true;
      this.createOverlay();
    },
    close() {
      // Trigger Modal Close Here
      this.modal = false;
      //this.overlay.removeEventListener('click', this.removeOverlay())
      document.body.removeChild(this.overlay)
      document.body.classList.remove('no-scrollable')
      this.overlay.addEventListener('click', () => this.close())
    },
    async logout () {
      // Log out the user.
      await this.$store.dispatch('auth/logout')

      // Redirect to login.
      this.$router.push({ name: 'login' })
    },
     loadProducts(){
            axios.get('/api/team').then(response => {
                this.products = response.data;
                console.log(response);

            });
        },
        onImageChange(e){
          console.log(e.target.files[0]);
          this.image = e.target.files[0];
      },
      formSubmit(e) {
          e.preventDefault();
          let currentObj = this;

          const config = {
              headers: { 'content-type': 'multipart/form-data' }
          }

          let formData = new FormData();
          formData.append('image', this.image);

          axios.post('/formSubmit', formData, config)
          .then(function (response) {
              currentObj.success = response.data.success;
          })
          .catch(function (error) {
              currentObj.output = error;
          });
      },

        async deleteProduct(product){
            await this.axios.delete(`/api/team/${product.id}`).then(() => {
                this.$toast.success({
                    title:'Success!',
                    message:'Product deleted successfully.'
                });
            });

            let index = this.products.indexOf(product);
            this.products.splice(index, 1);
        }
  }
}
</script scope>

<style>
  .overlay {
    background: rgba(0, 0, 0, 0.6);
    height: 100%;
    left: 0;
    position: fixed;
    top: 0;
    width: 100%;
    z-index: 1040;

  }

  body.no-scrollable {
    overflow: visible;
  }
  div#edit-form {
    z-index: 99999;
    padding: 10px 0px;
    position: fixed;
    max-width: 100%;
    border-radius: 5px;
    display: block;
    overflow: visible;
    text-align: 100%;
    top: 10%;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgb(248, 250, 247);
  border: 1px solid #BEBEBE;

  -webkit-transition: opacity 0.3s ease-out, bottom 0.3s ease-out;
  -moz-transition: opacity 0.3s ease-out, bottom 0.3s ease-out;
  -o-transition: opacity 0.3s ease-out, bottom 0.3s ease-out;
  transition: opacity 0.3s ease-out, bottom 0.3s ease-out;

}
.button-close{
  text-align: right;
    position: absolute;
    right: -30px;
    top: -30px;
    background: transparent;
    border: 1px solid;
    border-radius: 25px;
    width: 25px;
    height: 25px;
    line-height: 1;
    background: #000;
    color: #fff;
}
.button-close:hover{
  background: #777;
}
</style>
