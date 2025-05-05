<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>E-Commerce Website</title>
</head>
<body>
  <script>
    let products = [
      {
        productid: 1,
        name: "iPhone 16 Pro Max",
        description: "latest model",
        price: 50000,
        stock: 5,
        category: "iPhone",
        tags: ["latest", "smartphone", "apple"],
        discount: null,
      },
      {
        productid: 2,
        name: "Samsung S10",
        description: "latest model",
        price: 40000,
        stock: 4,
        category: "Samsung",
        tags: ["latest", "smartphone", "android"],
        discount: null,
      },
      {
        productid: 3,
        name: "Nokia 3310",
        description: "latest model",
        price: 30000,
        stock: 3,
        category: "Nokia",
        tags: ["latest", "smartphone", "android"],
        discount: { type: "percentage", value: 10 },
      },
      {
        productid: 4,
        name: "iPhone 15 Pro Max",
        description: "latest model",
        price: 100000,
        stock: 2,
        category: "iPhone",
        tags: ["latest", "smartphone", "apple"],
        discount: null,
      },
      {
        productid: 5,
        name: "Samsung S20",
        description: "latest model",
        price: 80000,
        stock: 1,
        category: "Samsung",
        tags: ["latest", "smartphone", "android"],
        discount: { type: "fixed", value: 5000 },
      },
    ];

    const displayProductDetails = (product) => {
      console.log("Product Details:");
      for (const key in product) {
        if (key === "tags") {
          console.log(`tags: ${product[key].join(", ")}`);
        } else if (key === "discount") {
          console.log(`discount: ${product[key] ? JSON.stringify(product[key]) : "null"}`);
        } else {
          console.log(`${key}: ${product[key]}`);
        }
      }
    };

    const filterProductsByCategory = (category) => {
      return products.filter((product) => product.category === category);
    };

    const findProductById = (productid) => {
      return products.find((product) => product.productid === productid);
    };

    const discountModule = (() => {
      const applyDiscount = (product, discount) => {
        if (discount && discount.type === "percentage") {
          product.price -= product.price * (discount.value / 100);
        } else if (discount && discount.type === "fixed") {
          product.price -= discount.value;
        }
      };
      return { applyDiscount };
    })();

    const updateStock = (productid, quantity) => {
      const product = findProductById(productid);
      if (product) {
        product.stock = quantity;
        console.log(`Stock for product ${productid} updated to ${quantity}`);
      } else {
        console.error("Product not found");
      }
    };

    const addTagToProduct = (productid, tag) => {
      const product = findProductById(productid);
      if (product) {
        if (!product.tags.includes(tag)) {
          product.tags.push(tag);
          console.log(`Tag "${tag}" added to product ${productid}`);
        } else {
          console.log(`Tag "${tag}" already exists on product ${productid}`);
        }
      } else {
        console.error("Product not found");
      }
    };

    const removeProduct = (productid) => {
      const index = products.findIndex(p => p.productid === productid);
      if (index !== -1) {
        products.splice(index, 1);
        console.log(`Product with ID ${productid} removed.`);
      } else {
        console.error("Product not found.");
      }
    };

    const calculateTotalValue = () => {
      let total = 0;
      for (const product of products) {
        total += product.price * product.stock;
      }
      console.log(`Total inventory value: ₹${total}`);
    };

    // ----- Actions -----
    console.log("--- Filtered Products (iPhone) ---");
    const iPhoneProducts = filterProductsByCategory("iPhone");
    iPhoneProducts.forEach(product => displayProductDetails(product));

    console.log("\n--- Find Product by ID (2) ---");
    const productById = findProductById(2);
    if (productById) {
      displayProductDetails(productById);
    } else {
      console.error("Product not found");
    }

    console.log("\n--- Apply Discount to Product (3) ---");
    const productToDiscount = findProductById(3);
    if (productToDiscount) {
      discountModule.applyDiscount(productToDiscount, productToDiscount.discount);
      displayProductDetails(productToDiscount);
    }

    console.log("\n--- Update Stock for Product (2) ---");
    updateStock(2, 10);
    displayProductDetails(findProductById(2));

    console.log("\n--- Add Tag to Product (1) ---");
    addTagToProduct(1, "bestseller");
    displayProductDetails(findProductById(1));

    console.log("\n--- Remove Product (5) ---");
    removeProduct(5);
    products.forEach(product => displayProductDetails(product));

    console.log("\n--- Calculate Total Inventory Value ---");
    calculateTotalValue();
  </script>
</body>
</html>
