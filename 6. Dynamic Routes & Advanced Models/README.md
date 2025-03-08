# 6. Dynamic Routes & Advanced Models

tags: #NodeJS #Backend #Routing #Models

---

## **Adding Product Id to the Path**
Dynamic routes allow passing parameters through the URL:
```js
router.get('/products/:productId', productController.getProductById);
```

---

## **Extracting Dynamic Params**
Extract parameters from the request:
```js
exports.getProductById = (req, res) => {
  const productId = req.params.productId;
  res.render('product-detail', { productId });
};
```

---

## **Loading Product Detail Data**
Retrieve product details from the model:
```js
exports.getProductById = (req, res) => {
  const productId = req.params.productId;
  Product.findById(productId, (product) => {
    res.render('product-detail', { product });
  });
};
```

---

## **Rendering the Product Detail View**
Update the view to display product details:
```html
<h1>{{ product.title }}</h1>
<p>{{ product.description }}</p>
```

---

## **Passing Data with POST Requests**
Handle POST data submission:
```js
router.post('/cart', cartController.addToCart);
```

---

## **Adding a Cart Model**
Structure the cart model:
```js
class Cart {
  static addProduct(productId) {
    // Logic to add product to cart
  }
}
module.exports = Cart;
```

---

## **Using Query Params**
Query parameters can be used to filter data:
```js
router.get('/products', productController.getFilteredProducts);
```

---

## **Linking to the Edit Page**
Generate dynamic edit links:
```html
<a href="/admin/edit-product/{{ product.id }}">Edit</a>
```

---

## **Editing the Product Data**
Handle product updates:
```js
router.post('/edit-product', productController.postEditProduct);
```

---

## **Adding the Product Delete Functionality**
Enable product deletion:
```js
router.post('/delete-product', productController.postDeleteProduct);
```

---

## **Deleting Cart Items**
Remove products from the cart:
```js
Cart.removeProduct(productId);
```

---

## **Displaying Cart Items on the Cart Page**
Retrieve and render cart items:
```js
exports.getCart = (req, res) => {
  Cart.getCart((cart) => {
    res.render('cart', { cart });
  });
};
```

---

## **Deleting Cart Items**
Allow users to remove items from the cart:
```js
router.post('/cart-delete-item', cartController.postCartDeleteItem);
```

---

## **Summary**
- Dynamic routing allows passing parameters via the URL.
- Query parameters help filter data.
- Implementing a cart system enables product management.
- CRUD operations enable users to add, edit, and delete products dynamically.
