<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grocery Marketplace</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>Welcome to Grocery Marketplace</h1>
        <p>Your one-stop shop for fresh groceries online.</p>
    </header>
    <nav>
        <ul>
            <li><a href="#catalogue">Product Catalogue</a></li>
            <li><a href="#cart">Shopping Cart</a></li>
            <li><a href="#login">Login/Register</a></li>
            <li><a href="#feedback">Feedback</a></li>
            <li><a href="#contact">Contact Us</a></li>
            <li><a href="#about">About Us</a></li>
        </ul>
    </nav>
    <section id="catalogue">
        <h2>Product Catalogue</h2>
        <div>
            <input type="text" id="searchBar" placeholder="Search for groceries...">
            <div id="searchResults"></div>
        </div>
        <div id="filter">
            <button onclick="filterCategory('all')">All</button>
            <button onclick="filterCategory('fruits')">Fruits</button>
            <button onclick="filterCategory('vegetables')">Vegetables</button>
            <button onclick="filterCategory('dairy')">Dairy</button>
        </div>
        <div id="products">
            <!-- List of products will be displayed here -->
        </div>
    </section>
    <section id="cart">
        <h2>Shopping Cart</h2>
        <div id="cartItems">
            <!-- Items added to cart will be displayed here -->
        </div>
        <p>Total Price: <span id="totalPrice">$0.00</span></p>
        <button onclick="checkout()">Proceed to Checkout</button>
    </section>
    <section id="checkout">
        <h2>Checkout</h2>
        <form id="checkoutForm">
            <label for="name">Name:</label>
            <input type="text" id="name" name="name" required>
            <label for="address">Address:</label>
            <input type="text" id="address" name="address" required>
            <label for="payment">Payment Details:</label>
            <input type="text" id="payment" name="payment" required>
            <button type="submit">Confirm Order</button>
        </form>
        <p id="confirmationMessage"></p>
    </section>
    <section id="login">
        <h2>Login/Register</h2>
        <form id="loginForm">
            <label for="username">Username:</label>
            <input type="text" id="username" name="username" required>
            <label for="password">Password:</label>
            <input type="password" id="password" name="password" required>
            <button type="submit">Login</button>
        </form>
        <form id="registerForm">
            <label for="newUsername">Username:</label>
            <input type="text" id="newUsername" name="newUsername" required>
            <label for="newPassword">Password:</label>
            <input type="password" id="newPassword" name="newPassword" required>
            <button type="submit">Register</button>
        </form>
    </section>
    <section id="feedback">
        <h2>Feedback</h2>
        <form id="feedbackForm">
            <label for="feedbackText">Your Feedback:</label>
            <textarea id="feedbackText" name="feedbackText" required></textarea>
            <button type="submit">Submit Feedback</button>
        </form>
    </section>
    <section id="contact">
        <h2>Contact Us</h2>
        <p>Email: support@grocerymarketplace.com</p>
        <p>Phone: 123-456-7890</p>
    </section>
    <section id="about">
        <h2>About Us</h2>
        <p>We are a dedicated team committed to bringing you the freshest groceries right to your doorstep. Our mission is to provide quality products and excellent customer service.</p>
    </section>
    <script src="scripts.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
}

header, nav, section {
    padding: 20px;
    margin: 20px;
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0,0,0,0.1);
}

header h1 {
    margin: 0;
}

nav ul {
    list-style: none;
    padding: 0;
}

nav ul li {
    display: inline;
    margin-right: 10px;
}

nav ul li a {
    text-decoration: none;
    color: #333;
}

#products, #cartItems {
    display: flex;
    flex-wrap: wrap;
}

.product, .cart-item {
    border: 1px solid #ddd;
    margin: 10px;
    padding: 10px;
    border-radius: 5px;
}

#searchResults {
    margin-top: 10px;
}

button {
    padding: 10px;
    background-color: #28a745;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #218838;
}
document.addEventListener("DOMContentLoaded", () => {
    // Sample products data
    const products = [
        { id: 1, name: "Apple", category: "fruits", price: 1.5, description: "Fresh apples." },
        { id: 2, name: "Broccoli", category: "vegetables", price: 2.0, description: "Fresh broccoli." },
        { id: 3, name: "Milk", category: "dairy", price: 3.0, description: "1 liter of milk." },
        // Add more products as needed
    ];

    const cart = [];

    // Functions to display products and cart items
    const displayProducts = (filteredProducts) => {
        const productsDiv = document.getElementById('products');
        productsDiv.innerHTML = '';
        filteredProducts.forEach(product => {
            const productDiv = document.createElement('div');
            productDiv.classList.add('product');
            productDiv.innerHTML = `<h3>${product.name}</h3><p>${product.description}</p><p>Price: $${product.price}</p><button onclick="addToCart(${product.id})">Add to Cart</button>`;
            productsDiv.appendChild(productDiv);
        });
    };

    const displayCart = () => {
        const cartItemsDiv = document.getElementById('cartItems');
        cartItemsDiv.innerHTML = '';
        cart.forEach(item => {
            const cartItemDiv = document.createElement('div');
            cartItemDiv.classList.add('cart-item');
            cartItemDiv.innerHTML = `<h3>${item.name}</h3><p>Price: $${item.price}</p><button onclick="removeFromCart(${item.id})">Remove</button>`;
            cartItemsDiv.appendChild(cartItemDiv);
        });
        document.getElementById('totalPrice').innerText = `$${cart.reduce((total, item) => total + item.price, 0).toFixed(2)}`;
    };

    // Event handlers
    document.getElementById('searchBar').addEventListener('input', (event) => {
        const searchTerm = event.target.value.toLowerCase();
        const searchResults = products.filter(product => product.name.toLowerCase().includes(searchTerm));
        displayProducts(searchResults);
    });

    window.filterCategory = (category) => {
        if (category === 'all') {
            displayProducts(products);
        } else {
            const filteredProducts = products.filter(product => product.category === category);
            displayProducts(filteredProducts);
        }
    };

    window.addToCart = (id) => {
        const product = products.find(product => product.id === id);
        cart.push(product);
        displayCart();
    };

    window.removeFromCart = (id) => {
        const index = cart.findIndex(item => item.id === id);
        cart.splice(index, 1);
        displayCart();
    };

    window.checkout = () => {
        document.getElementById('checkout').style.display = 'block';
    };

    document.getElementById('checkoutForm').addEventListener('submit', (event) => {
        event.preventDefault();
        document.getElementById('confirmationMessage').innerText = "Thank you for your order!";
        document.getElementById('checkoutForm').reset();
        cart.length = 0;
        displayCart();
    });

    // Initial display
    displayProducts(products);
});
