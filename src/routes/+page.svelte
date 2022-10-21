<script>
  // Import the store etc.
  import  {products, categories, getAllProducts} from '../stores/productStore.js';

  // filtered is subscribed to the $products store
  // initially filtered is a copy of products (from the store)
  $: filtered = $products;

  // This function will filter products by cat_id
  const filterByCat = async (cat_id = 0) => {

    // Refresh the store to ensure it is up to date (optional)
    await getAllProducts();

    // Filter if we have a cat_id value > 0
    // See https://javascript.info/array-methods#filter
    // Note this filters the existing $product store and does not replace it like getProductsByCat()
    if (cat_id > 0) {
      filtered = $products.filter(p => {return p.category_id == cat_id});
    }
  }

 </script>
 
 <!-- Main Content - Products etc. -->
    <div class="container">
      <div class="row">
        <!-- Page Header -->
        <h2 class="mt-5">Products List</h2>
      </div>
      <div class="row">
        <div class="col-sm-2">
          <!-- Page Body Left Column (menu) -->
          <div id="categoryList" class="list-group">
            <!-- placeholders - replace with real category links -->
            <button on:click= {() => filterByCat()}
                class="category-link list-group-item list-group-item-action">
                All Products
            </button>
            {#each $categories as cat }
            <button on:click= {() => filterByCat(cat.id)}
                class="category-link list-group-item list-group-item-action">
                {cat.category_name}
            </button>
            {/each}
          </div>
        </div> <!-- End category col -->
        <div class="col-sm-10">
          <!-- Page Body Right Side (Content goes here) -->
          <div id="products">
            <table class="table table-striped table-bordered table-hover">
              <thead>
                <tr>
                  <th>id</th>
                  <th>name</th>
                  <th>description</th>
                  <th>stock</th>
                  <th>price</th>
                </tr>
              </thead>
              <tbody id="productRows">
                <!-- Product Rows to be inserted here -->
                {#each filtered as product }
                <tr>
                    <td>{product.id}</td>
                    <td>{product.product_name}</td>
                    <td>{product.product_description}</td>
                    <td>{product.product_stock}</td>
                    <td class="price">&euro;{Number(product.product_price).toFixed(2)}</td>
                </tr>
                {/each}
              </tbody>
            </table>
          </div>
        </div> <!-- End Product col -->
      </div> <!-- End Row -->
    </div> <!-- End Main Content-->