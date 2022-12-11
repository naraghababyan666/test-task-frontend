<template>
  <div id="app">
    <div class="container d-flex h-100 p-3 mx-auto flex-column">
      <header class="masthead ">
        <div class="inner">
          <h3 class="masthead-brand">Test task</h3>
          <nav class="nav nav-masthead justify-content-center">
            <a class="nav-link active" href="#">Home</a>
            <a class="nav-link" href="#includes">Includes API</a>
            <a class="nav-link" href="#">Contact</a>
          </nav>
        </div>
      </header>
      <main role="main" class="inner cover">
        <div class="get-categories">
          <div class="card text-white bg-primary mb-3" style="width:100%" @click="getCategoriesAPI">
            <div class="card-header">GET - /categories</div>
            <div class="card-body">
              <h5 class="card-title">Get all categories</h5>
              <p class="card-text">Get request returned all categories from database</p>
            </div>
          </div>
          <modal name="get-categories"
                 height="auto"
                 width="80%"
                 :scrollable="true"
                 :adaptive="true">
            <table class="table table-hover categories-parent">
              <thead>
              <tr>
                <th scope="col">#</th>
                <th scope="col">Name</th>
                <th scope="col">Parent id</th>
                <th scope="col">Created at</th>
              </tr>
              </thead>
              <tbody>
              <tr v-for="item in this.getCategoriesAPIData" :key="item.id" class="category">
                <th scope="row">{{ item.id }}</th>
                <td>{{ item.name }}</td>
                <td>{{ item.parent_id }}</td>
                <td>{{ item.created_at }}</td>
              </tr>
              </tbody>
            </table>
          </modal>
        </div>
        <div class="create-category">
          <div class="card text-white bg-success mb-3" style="width:100%">
            <div class="card-header">POST - /categories</div>
            <div class="card-body">
              <h5 class="card-title">Create category, insert name and parent category if exists</h5>
              <p class="card-text">Insert category name - <input type="text" v-model="categoryNameForCreateCategory"
                                                                 style="margin-left: 10px;"/></p>
              <p class="card-text d-flex">Choose parent category -
                <select class="form-select form-select-sm" style="width: auto; margin-left: 15px;" v-model="chooseParentCategoryForCreateCategory">
                  <option selected value="null">Categories</option>
                  <option v-for="item in getCategoriesAPIDataGlobal" :key="item.id"
                          :value="item.id">{{ item.name }}
                  </option>
                </select>
              </p>
              <button type="button" @click="createCategory" class="btn btn-dark">Test API</button>
            </div>
            <modal name="create-categories"
                   height="auto"
                   width="80%"
                   :scrollable="true"
                   :adaptive="true">
              <p class="success-message">Category successfully created</p>
            </modal>
          </div>
        </div>
        <div class="update-category">
          <div class="card text-white bg-secondary ">
            <div class="card-header">POST - /categories/{ID}</div>
            <div class="card-body">
              <h5 class="card-title">Update category by ID</h5>
              <div v-for="item in getCategoriesAPIDataGlobal" :key="item.id">
                <div class="d-flex justify-content-between content mb-2">
                  <span>{{ item.id }} | {{ item.name }}</span>
                  <button @click="getCategoryByIdAPI(item.id)" type="button" class="btn btn-dark">Edit</button>
                </div>
              </div>
            </div>
            <modal name="getCategoryById-modal"
                   height="auto"
                   width="80%"
                   :scrollable="true"
                   :adaptive="true"
                   v-if="getCategoryDataForUpdate != null">
              <div class="allContent">
                <div class="inputs">
                  <div class="name">
                    <label for="categoryName" style="margin-right: 20px;">Category name</label>
                    <input type="text" id="categoryName" class="dark-input" v-model="getCategoryDataForUpdate.name"/>
                  </div>
                  <div class="parent">
                    <label for="categoryParent" style="margin-right: 20px;">Parent category</label>
                    <input type="number" id="categoryName" class="dark-input"
                           v-model="getCategoryDataForUpdate.parent_id"/>
                  </div>
                  <div class="messages" v-if="successFalse">
                    <p style="color: red">{{ successFalseMessage }}</p>
                  </div>
                  <button class="btn btn-dark mt-3" @click="updateCategoryAPI(getCategoryDataForUpdate)">Edit</button>
                </div>

              </div>

            </modal>
          </div>
        </div>
        <hr/>
        <div class="get-products">
          <div class="card text-white bg-primary mb-3" style="width:100%" @click="getProductsAPI">
            <div class="card-header">GET - /products</div>
            <div class="card-body">
              <h5 class="card-title">Get products list</h5>
              <p class="card-text">Get request returned all products from database</p>
            </div>
          </div>
          <modal name="get-products"
                 width="80%"
                 height="auto"
                 :scrollable="true"
                 :adaptive="true">
            <table class="table table-hover products-parent">
              <thead>
              <tr>
                <th scope="col">#</th>
                <th scope="col">Name</th>
                <th scope="col">Parent id</th>
                <th scope="col">Created at</th>
              </tr>
              </thead>
              <tbody>
              <tr v-for="item in this.getProductsAPIData" :key="item.id" class="category">
                <th scope="row">{{ item.id }}</th>
                <td>{{ item.name }}</td>
                <td>{{ item.price }}</td>
                <td>{{ item.category_id }}</td>
                <td>{{ item.created_at }}</td>
              </tr>
              </tbody>
            </table>
          </modal>

        </div>
        <div class="create-products">
          <div class="card text-white bg-success mb-3" style="width:100%">
            <div class="card-header">POST - /products</div>
            <div class="card-body">
              <h5 class="card-title">Create product, insert name,price and category</h5>
              <p class="card-text">Insert product name - <input type="text" v-model="productNameForCreate"
                                                                style="margin-left: 10px;"/></p>
              <p class="card-text">Insert product price - <input type="number" v-model="productPriceForCreate"
                                                                 style="margin-left: 10px;"/></p>
              <p class="card-text d-flex">Choose category -
                <select class="form-select form-select-sm" style="width: auto; margin-left: 15px;" v-model="chooseCategoryForCreateProduct">
                  <option selected value="null">Categories</option>
                  <option v-for="item in getCategoriesAPIDataGlobal" :key="item.id"
                          :value="item.id">{{ item.name }}
                  </option>
                </select>
              </p>
              <button type="button" @click="createProduct" class="btn btn-dark">Test API</button>
            </div>
            <modal name="create-products"
                   height="auto"
                   width="80%"
                   :scrollable="true"
                   :adaptive="true">
              <p class="success-message">Product successfully created</p>
            </modal>
          </div>
        </div>
        <div class="update-products">
          <div class="card text-white bg-secondary mb-3">
            <div class="card-header">POST - /products/{ID}</div>
            <div class="card-body">
              <h5 class="card-title">Update product by ID</h5>
              <div v-for="item in getProductsAPIDataGlobal" :key="item.id">
                <div class="d-flex justify-content-between content mb-2">
                  <span>{{ item.id }} | {{ item.name }}</span>
                  <button @click="getProductByIdAPI(item.id)" type="button" class="btn btn-dark">Edit</button>
                </div>
              </div>
            </div>
            <modal name="getProductById-modal"
                   height="auto"
                   width="80%"
                   :scrollable="true"
                   :adaptive="true"
                   v-if="getProductDataForUpdate != null">
              <div class="allContent">
                <div class="productById">
                  <div class="name">
                    <label for="categoryName" style="margin-right: 20px;">Category name</label>
                    <input type="text" id="categoryName" class="dark-input" style="width: 100%"
                           v-model="getProductDataForUpdate.name"/>
                  </div>
                  <div class="price">
                    <label for="categoryParent" style="margin-right: 20px;">Price</label>
                    <input type="number" id="categoryName" class="dark-input" v-model="getProductDataForUpdate.price"/>
                  </div>
                  <div class="price">
                    <label for="categoryParent" style="margin-right: 20px;">Category id</label>
                    <input type="number" id="categoryName" class="dark-input"
                           v-model="getProductDataForUpdate.category_id"/>
                  </div>
                  <div class="messages" v-if="successFalse">
                    <p style="color: red">{{ successFalseMessage }}</p>
                  </div>
                  <button class="btn btn-dark mt-3" @click="updateProductAPI(getProductDataForUpdate)">Edit</button>
                </div>

              </div>

            </modal>
          </div>
        </div>
        <div class="delete-products">
          <div class="card text-white bg-danger mb-3" style="width:100%">
            <div class="card-header">DELETE - /products/{ID}</div>
            <div class="card-body">
              <h5 class="card-title">Delete product, insert ID.</h5>
              <p class="card-text">Insert product id - <input type="number" v-model="productIdForDelete"
                                                              style="margin-left: 10px;"/></p>

              <button type="button" @click="deleteProductById(productIdForDelete)" class="btn btn-dark">Test API
              </button>
            </div>
            <modal name="delete-products"
                   height="auto"
                   width="80%"
                   :scrollable="true"
                   :adaptive="true">
              <p class="success-message">Product successfully deleted</p>
            </modal>
          </div>
        </div>
        <hr/>
        <div class="includes" id="includes">
          <div class="categories-includeProducts">
            <p>GET - Получение списка категорий c вложенными товарами</p>
            <button type="button" class="btn btn-outline-warning" @click="getCategoriesWithIncludes">
              /categories?includeProducts=1
            </button>
          </div>
          <modal name="getCategoriesWithInclude"
                 height="auto"
                 width="80%"
                 :scrollable="true"
                 :adaptive="true">
              <div v-if="this.categoriesWithIncludes != null" class="includeChildsStyle" style="overflow-y: scroll">
                <json-viewer :value="categoriesWithIncludes" :expand-depth=5
                             copyable
                             boxed
                             sort>
                </json-viewer>
              </div>
          </modal>
        </div>


        <div class="includesByCategoryId" id="includesById">
          <div class="categoriesById-includeProducts">
            <p>GET - Получение одной категории c вложенными товарами</p>
            <select class="form-select form-select-sm includeByCategoryId" style="width: auto;"  @change="includeProductByCategoryId($event)">
              <option selected disabled value="null">/categories/{id}?includeProducts=1</option>
              <option v-for="item in getCategoriesAPIDataGlobal" :key="item.id"
                      :value="item.id">{{ item.name }}
              </option>
            </select>
          </div>
          <modal name="includeProductByCategoryId"
                 height="auto"
                 width="80%"
                 :scrollable="true"
                 :adaptive="true">
              <div v-if="this.includeProductByCategoryIdData != null" class="includeChildsStyle" style="overflow-y: scroll">
                <json-viewer :value="includeProductByCategoryIdData" :expand-depth=5
                             copyable
                             boxed
                             sort>
                </json-viewer>
              </div>
          </modal>
        </div>



        <div class="categoryIdProductsAll" id="categoryIdProductsAll">
          <div class="categoryIdProductsAllChild">
            <p>GET - Получение списка товаров из категории</p>
            <select class="form-select form-select-sm includeByCategoryId" style="width: auto;"  @change="categoryIdProductsAll($event)">
              <option selected disabled value="null">/categories/{id}/products</option>
              <option v-for="item in getCategoriesAPIDataGlobal" :key="item.id"
                      :value="item.id">{{ item.name }}
              </option>
            </select>
          </div>
          <modal name="categoryIdProductsAll"
                 height="auto"
                 width="80%"
                 :scrollable="true"
                 :adaptive="true">
              <div v-if="this.categoryIdProductsAllData != null" class="includeChildsStyle" style="overflow-y: scroll">
                <json-viewer :value="categoryIdProductsAllData" :expand-depth=5
                             copyable
                             boxed
                             sort>
                </json-viewer>
              </div>
          </modal>
        </div>



        <div class="categoryIdProductsIncludeChild" id="categoryIdProductsIncludeChild">
          <div class="categoryIdProductsIncludeChildren">
            <p>GET - Получение списка товаров из категории</p>
            <select class="form-select form-select-sm includeByCategoryId" style="width: auto;"  @change="categoryIdProductsIncludeChild($event)">
              <option selected disabled value="null">/categories/{id}/products</option>
              <option v-for="item in getCategoriesAPIDataGlobal" :key="item.id"
                      :value="item.id">{{ item.name }}
              </option>
            </select>
          </div>
          <modal name="categoryIdProductsIncludeChild"
                 height="auto"
                 width="80%"
                 :scrollable="true"
                 :adaptive="true">
              <div v-if="this.categoryIdProductsIncludeChildData != null" class="includeChildsStyle" style="overflow-y: scroll">
                <json-viewer :value="categoryIdProductsIncludeChildData" :expand-depth=5
                             copyable
                             boxed
                             sort>
                </json-viewer>
              </div>
          </modal>
        </div>
        <hr />
        <div class="makePassword mb-4">
          <p class="mb-0"><code>makePassword</code> использует цифры и буквы в нижнем и верхнем регистре  <code v-show="password" class="pinOrPassword"> - {{password}}</code></p>
          <input type="number" v-model="passwordLength" class="mr-2" style="padding: 6px 12px"/>
          <button type="button" class="btn btn-success ml-1" @click="generatePassword">Test API</button>
        </div>
        <div class="makePin">
          <p class="mb-0"><code>makePin</code> использует только цифры <code v-show="pin" class="pinOrPassword"> - {{pin}}</code></p>
          <button type="button" @click="makePin" class="btn btn-success">Test API</button>
        </div>

        <modal name="success-modal"
               height="auto"
               width="80%"
               :scrollable="true"
               :adaptive="true">
          <p class="success-message">Product successfully created</p>
        </modal>
      </main>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import 'vue-json-viewer/style.css'

export default {
  name: 'App',
  data() {
    return {
      getCategoriesAPIData: [],
      getProductsAPIData: [],
      getCategoriesAPIDataGlobal: [],
      chooseParentCategoryForCreateCategory: null,
      chooseCategoryForCreateProduct: null,
      categoryNameForCreateCategory: null,
      productPriceForCreate: 0,
      productNameForCreate: null,
      productIdForDelete: null,
      getCategoryDataForUpdate: null,
      getProductDataForUpdate: null,
      successFalse: false,
      successFalseMessage: '',
      parent_id_if_exists: null,
      getProductsAPIDataGlobal: {},
      categoriesWithIncludes: null,
      includeProductByCategoryIdData: null,
      categoryIdProductsAllData: null,
      categoryIdProductsIncludeChildData: null,
      pin: null,
      password: null,
      passwordLength: 6
    }
  },
  components: {
    // eslint-disable-next-line vue/no-unused-components
    axios
  },
  mounted() {
    this.getCategoriesAPIGlobal();
    this.getProductsAPIGlobal();
  },
  methods: {
    getCategoriesAPI() {
      axios.get("http://127.0.0.1:8000/api/categories").then((res) => {
        if (res.data.success) {
          this.getCategoriesAPIData = res.data.categories
          this.$modal.show('get-categories')
        }
      })
    },
    getCategoriesAPIGlobal() {
      axios.get("http://127.0.0.1:8000/api/categories").then((res) => {
        if (res.data.success) {
          this.getCategoriesAPIDataGlobal = res.data.categories
        }
      })
    },
    getCategoryByIdAPI(id) {
      axios.get("http://127.0.0.1:8000/api/categories/" + id).then((res) => {
        if (res.data.success === true) {
          this.getCategoryDataForUpdate = res.data.data;
          this.$modal.show('getCategoryById-modal', this.getCategoryDataForUpdate)
        } else if (res.data.success === false) {
          this.successFalse = true
        }
      })
    },
    getCategoriesWithIncludes() {
      axios.get("http://127.0.0.1:8000/api/categories?includeProducts=1").then((res) => {
        if(res.data.success){
          this.categoriesWithIncludes = res.data.categories
          this.$modal.show('getCategoriesWithInclude', this.categoriesWithIncludes)
        }
      })
    },
    updateCategoryAPI(data) {
      let categoryUpdatedData = {};
      if (data.parent_id !== '') {
        categoryUpdatedData.parent_id = data.parent_id
      }
      categoryUpdatedData.name = data.name
      console.log(categoryUpdatedData)
      axios.post("http://127.0.0.1:8000/api/categories/" + data.id, categoryUpdatedData).then((res) => {
        if (res.data.success === true) {
          this.getCategoriesAPIGlobal();
          this.$modal.hide('getCategoryById-modal')
          this.$modal.show('success-modal')
        } else if (res.data.success === false) {
          this.successFalse = true
          this.successFalseMessage = res.data.message
        }
      })
    },
    includeProductByCategoryId(event){
      let id = event.target.value;
      axios.get("http://127.0.0.1:8000/api/categories/"+id+"?includeProducts=1").then((res) => {
        if(res.data.success){
          this.includeProductByCategoryIdData = res.data.categories
          this.$modal.show('includeProductByCategoryId', this.includeProductByCategoryIdData)
        }
      })
    },
    categoryIdProductsAll(event){
      let id = event.target.value;
      axios.get("http://127.0.0.1:8000/api/categories/"+id+"/products").then((res) => {
        console.log(res.data)
        if(res.data.success){
          this.categoryIdProductsAllData = res.data.data
          this.$modal.show("categoryIdProductsAll")
        }else{
          alert("Server error")
        }
      })
    },
    categoryIdProductsIncludeChild(event){
      let id = event.target.value;
      console.log(id)
      axios.get("http://127.0.0.1:8000/api/categories/"+id+"/products?includeChildren=1").then((res) => {
        if(res.data.success){
          this.categoryIdProductsIncludeChildData = res.data.data
          this.$modal.show("categoryIdProductsIncludeChild")
        }else{
          alert("Server error")
        }
      })
    },
    updateProductAPI(data) {
      axios.post("http://127.0.0.1:8000/api/products/" + data.id, data).then((res) => {
        if (res.data.success) {
          this.$modal.hide('getProductById-modal')
          this.$modal.show('success-modal')
          this.getProductsAPIGlobal();
        } else {
          alert("Server error")
        }
      })
    },
    getProductsAPI() {
      axios.get("http://127.0.0.1:8000/api/products").then((res) => {
        if (res.data.success) {
          this.getProductsAPIData = res.data.products
          this.$modal.show('get-products')
        }
      })
    },
    getProductsAPIGlobal() {
      axios.get("http://127.0.0.1:8000/api/products").then((res) => {
        if (res.data.success) {
          this.getProductsAPIDataGlobal = res.data.products
        }
      })
    },
    getProductByIdAPI(id) {
      axios.get("http://127.0.0.1:8000/api/products/" + id).then((res) => {
        if (res.data.success === true) {
          this.getProductDataForUpdate = res.data.data
          this.$modal.show('getProductById-modal', this.getProductDataForUpdate)
        } else if (res.data.success === false) {
          this.successFalse = true
        }
      })
    },
    createCategory() {
      if (this.categoryNameForCreateCategory != null) {
        axios.post("http://127.0.0.1:8000/api/categories", {
          'name': this.categoryNameForCreateCategory,
          'parent_id': this.chooseParentCategoryForCreateCategory
        }).then((res) => {
          if (res.data.success) {
            this.getCategoriesAPIGlobal()
            this.$modal.show('create-categories')
          }
        })
      } else {
        alert('Insert category name')
      }
    },
    createProduct() {
      if (this.productNameForCreate != null && this.chooseCategoryForCreateProduct != null && this.productPriceForCreate !== 0) {
        axios.post("http://127.0.0.1:8000/api/products", {
          'name': this.productNameForCreate,
          'price': this.productPriceForCreate,
          'category_id': this.chooseCategoryForCreateProduct
        }).then((res) => {
          if (res.data.success) {
            this.getProductsAPIGlobal()
            this.$modal.show('create-products')
          }
        })
      } else {
        console.log(this.chooseCategoryForCreateProduct, this.productPriceForCreate, this.productNameForCreate)
      }
    },
    deleteProductById(id) {
      if (id != null || id !== 0) {
        axios.delete("http://127.0.0.1:8000/api/products/" + id).then((res) => {
          if (res.data.success) {
            this.$modal.show('delete-products')
            this.getProductsAPIGlobal();
          } else {
            alert("Product not found")
          }
        })
      }
    },
    makePin(){
      axios.get("http://127.0.0.1:8000/api/makePin").then((res) => {
        this.pin = res.data
      })
    },
    generatePassword(){
      axios.get("http://127.0.0.1:8000/api/makePassword/"+this.passwordLength).then((res) => {
        this.password = res.data
      })
    }
  }
}
</script>

<style>

.includeChildsStyle{
  padding: 20px;
  width: 100%;
}
::-webkit-scrollbar {
  width: 10px;
}
::-webkit-scrollbar-track {
  background: #d7d7d7;
}
::-webkit-scrollbar-thumb {
  background: #42b983;
}
.categories-parent {
  width: auto;
  margin: 20px;
  color: black;
}
.mr-2{
  margin-right: 10px;
}
.vm--modal {
  display: flex;
  align-content: center;
  justify-content: center;
}

.update-category .vm--modal {
  padding: 20px !important;
}

input[type="text"] {
  /*background-color: white !important;*/
  border-radius: 0.25rem;
  border: none;
  outline: none;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.dark-input {
  background-color: grey !important;
  color: white !important;

}

.allContent .inputs {
  color: black !important;
}

.allContent .inputs .name {
  margin-bottom: 20px;
}

.allContent .productById > * {
  margin-bottom: 20px;
  color: black;
}

input[type=number] {
  -moz-appearance: textfield;
  border-radius: 0.25rem;
  border: none;
  outline: none;
}

.success-message {
  padding: 40px;
  color: green;
}

.form-select {
  border: none;
  outline: none;
}
.includeByCategoryId{
  background-color: transparent;
  border: 1px solid #ffc107;
  padding: 6px 12px;
  cursor: pointer;
  font-size: 16px;
  color: #ffc107;
  width: fit-content;
  padding-right: 35px;
}
/*.includeByCategoryId:hover{*/
/*  background-color: #ffc107;*/
/*  color: black;*/
/*}*/
.includeByCategoryId option:first-child{
  color: grey;
}
.includeByCategoryId option:not(first-child){
  color: black;
}

</style>
