<template>
    <div>
        <!-- App Bar -->
    <v-app-bar>
    <v-app-bar-title>E-Shop</v-app-bar-title>

    <v-spacer/>

     <v-btn variant="tonal" color="primary">Add Product</v-btn>

    </v-app-bar>

    <h1>
        Products
    </h1>
    <!-- Product Collection -->
    <v-row>
        <v-col
        v-for="product in products"
        :key="product.id"
        sm="6"
        md="4"
        >
        <!-- Product Card -->
        <v-card>
            <v-img :src="getProductImage(product)" cover height="300" @click=""openImage(product)></v-img>
        </v-card>

        <v-card-title>
        {{ product.name }}
        </v-card-title>

        <v-card-title>
        {{ product.price }}
        </v-card-title>

        <v-card-title>
        {{ product.unit }}
        </v-card-title>

        <v-card-title>
        {{ product.description }}
        </v-card-title>

        <div>
            <v-text-field v-model="product.qty" label="Quantity" variant="outlined" type="number">
            </v-text-field>
        </div>

        <div>
            <strong>Total: P {{ computeTotalPrice(product) }}</strong>
        </div>

        <v-card-actions>
            <v-btn block color="success" variant="outlined">Order Item</v-btn>
        </v-card-actions>
        </v-col>
    </v-row>

    <!-- Image Preview -->
     <v-dialog v-model="imageDialog" max-width="700">
        <v-card>
            <v-card-actions>
                <v-btn color="red" @click="imageDialog = false">Close
                </v-btn>
            </v-card-actions>

            <v-img :src="selectedImage" height="600" contain></v-img>
        </v-card>
     </v-dialog>
    </div>
</template>

<script setup>
const products = ref([])
const selectedImage = ref(null)
const imageDialog = ref(false)

async function fetchProducts() {
    const payload = await $fetch("http://localhost:1337/api/products?populate=*")

    products.value = payload.data.map((item) =>({
        ...item,
        qty: 1,
        }))
    }
    //Fetch Image product
    function getProductImage(product){
        if(product.image?.url){
            return `http://localhost:1337${product.image.url}`
        }
    }
    //Compute total price
    function computeTotalPrice(product){
        return Number(product.qty) * (product.price)
    }

    function openImage(product){
        selectedImage.value = `http://localhost:1337${product.image.url}`
        imageDialog.value = true
    }
onMounted(()=>{
    fetchProducts()
})
</script>