<template>
  <div>


<div class="card text-center">
  <div class="card-header">
   <h4> Crud Operation</h4>
  </div>

<div class="card-body">   
  <div class="container">
   <div class="row">
     <div class="col-md-7">

<h4 class="text-center">All Product</h4>
  <table class="table">
  <thead>
    <tr>
      <th scope="col">No</th>
      <th scope="col">Name Product</th>
      <th scope="col">Price</th>
      <th scope="col">Action</th>
    </tr>
  </thead>
  <tbody>

    <tr v-for="product in products" v-bind:key="product.id">
      <th scope="row">{{ product.id }}</th>
      <td>{{ product.product_name }}</td>
      <td>{{ product.price }}</td>
      <td>
            <div class="btn-group">

            <button type="button" class="btn btn-success" @click="edit(product)"> <i class="las la-edit"></i></button>

            <button type="button" class="btn btn-danger" @click="remove(product)"> <i class="las la-trash"></i> </button>
        
            </div>

      </td>
    </tr>

  </tbody>
</table>

</div>

<div class="col-md-5">
  <h4 class="text-center">Create Product</h4>

  <form @submit.prevent="save">

      <div class="mb-3">
        <label for="product_name" class="form-label">Product Name</label>
        <input type="text" class="form-control" v-model="product.product_name" id="product_name"  placeholder="product name">
      </div>

      <div class="mb-3">
        <label for="price" class="form-label">Price</label>
        <input type="number" v-model="product.price" class="form-control" id="price" placeholder="price">
      </div>

  <button type="submit" class="btn btn-primary">Submit</button>
</form>
</div>

            </div>
         </div>
      </div>

   </div>

</div>
</template>



<script>

import axios from 'axios';



export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },



// start...

data(){
  return{



    products: {},

    resetForm(){
   
      this.product={
              id: '',
              product_name: '',
              price: '',
            
               }

    },


    product:{
              id: '',
              product_name: '',
              price: '',
            
               }
            }
   
    },


// get product.........

created() {
        this.ProductLoad();
    },

    mounted() {
          console.log("mounted() called.......");
      },

    methods: {
      ProductLoad()
            {
                 var page = "http://127.0.0.1:8000/api/product";
                 axios.get(page)
                  .then(
                      ({data})=>{
                        console.log(data);
                        this.products = data;
                      }
                 );
              },


// store data
save()
           {
            if(this.product.id == '')
              {
                this.saveData();
              }
              else
              {
                this.updateData();
              }      
 
           },
           saveData()
           {
            axios.post("http://127.0.0.1:8000/api/product/store", this.product)
            .then(

          
            ()=>{
            alert(" Product successfully added");
                this.ProductLoad();
               
                this.resetForm(); // call the resetForm
              }
          
            )
 
           },


// edit part start

edit(product)
           {
            this.product =  product;
          
           },
           updateData()
           {
              var editrecords = 'http://127.0.0.1:8000/api/product/update/'+this.product.id;
              axios.put(editrecords, this.product)
              .then(
                ({data})=>{
                  this.product.product_name = '';
                  this.product.price = '',
                  this.id = ''
                  alert("Product Updated....!!!");
                  this.ProductLoad();
                  console.log(data);
                  this.resetForm(); // call the resetForm 
                }
              );
 
           },
// delete product

remove(product){
 
 var url = `http://127.0.0.1:8000/api/product/delete/${product.id}`;
  axios.delete(url);
  alert("Product Deleteddd");
  this.ProductLoad();
   }
      }

          }
</script>

