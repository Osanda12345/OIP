# OIP
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OIP - Your Products</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    <style>
        /* Global Styles */
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Roboto', sans-serif; }
        body { background-color: #f0f4f8; }

        /* Header */
        header { 
            background: linear-gradient(90deg, #4facfe, #00f2fe);
            color: white; text-align: center; padding: 30px 0; box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }
        header h1 { font-size: 2.5rem; }

        /* Container */
        .container { max-width: 1000px; margin: 40px auto; padding: 0 20px; }

        /* Form */
        form { 
            background: white; padding: 25px; border-radius: 12px; box-shadow: 0 8px 20px rgba(0,0,0,0.05); 
            display: grid; grid-template-columns: 1fr 1fr; gap: 20px;
        }
        form input, form textarea, form button {
            width: 100%; padding: 12px 15px; border-radius: 8px; border: 1px solid #ddd;
            font-size: 1rem; outline: none;
        }
        form button {
            grid-column: span 2; background: #4facfe; color: white; font-weight: bold; border: none; cursor: pointer;
            transition: 0.3s;
        }
        form button:hover { background: #00f2fe; }

        /* Products Grid */
        .products { display: grid; grid-template-columns: repeat(auto-fill,minmax(250px,1fr)); gap: 25px; margin-top: 40px; }
        .product-card { background: white; border-radius: 12px; box-shadow: 0 6px 20px rgba(0,0,0,0.08); overflow: hidden; transition: transform 0.3s, box-shadow 0.3s; }
        .product-card:hover { transform: translateY(-5px); box-shadow: 0 12px 25px rgba(0,0,0,0.15); }
        .product-card img { width: 100%; height: 200px; object-fit: cover; }
        .product-card .content { padding: 15px; }
        .product-card h3 { font-size: 1.2rem; margin-bottom: 8px; color: #333; }
        .product-card p { font-size: 0.95rem; color: #555; margin-bottom: 10px; }
        .product-card button {
            background: #ff5f6d; color: white; border: none; padding: 8px 12px; border-radius: 8px;
            cursor: pointer; font-weight: bold; transition: 0.3s;
        }
        .product-card button:hover { background: #ff848f; }
    </style>
</head>
<body>

<header>
    <h1>OIP - Your Products</h1>
</header>

<div class="container">
    <form id="productForm">
        <input type="text" id="productName" placeholder="Product Name" required>
        <input type="text" id="productPrice" placeholder="Product Price" required>
        <input type="text" id="productImage" placeholder="Product Image URL">
        <textarea id="productDesc" placeholder="Product Description"></textarea>
        <button type="submit">Add Product</button>
    </form>

    <div class="products" id="productsContainer"></div>
</div>

<script>
    const form = document.getElementById('productForm');
    const productsContainer = document.getElementById('productsContainer');
    let products = JSON.parse(localStorage.getItem('products')) || [];

    function displayProducts() {
        productsContainer.innerHTML = '';
        products.forEach((product, index) => {
            const div = document.createElement('div');
            div.classList.add('product-card');
            div.innerHTML = `
                ${product.image ? `<img src="${product.image}" alt="${product.name}">` : `<div style="height:200px;background:#ddd;"></div>`}
                <div class="content">
                    <h3>${product.name}</h3>
                    <p>Price: $${product.price}</p>
                    <p>${product.desc}</p>
                    <button onclick="deleteProduct(${index})">Delete</button>
                </div>
            `;
            productsContainer.appendChild(div);
        });
    }

    function deleteProduct(index) {
        products.splice(index, 1);
        localStorage.setItem('products', JSON.stringify(products));
        displayProducts();
    }

    form.addEventListener('submit', function(e){
        e.preventDefault();
        const name = document.getElementById('productName').value;
        const price = document.getElementById('productPrice').value;
        const image = document.getElementById('productImage').value;
        const desc = document.getElementById('productDesc').value;

        products.push({name, price, image, desc});
        localStorage.setItem('products', JSON.stringify(products));
        displayProducts();
        form.reset();
    });

    displayProducts();
</script>

</body>
</html>
