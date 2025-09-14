<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ShopEasy - Simple Online Store</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
    </style>
</head>
<body class="bg-gray-50">
    
    <!-- Header -->
    <header class="bg-white shadow-sm border-b">
        <div class="max-w-6xl mx-auto px-4 py-4">
            <div class="flex justify-between items-center">
                <div class="flex items-center space-x-8">
                    <h1 class="text-2xl font-bold text-blue-600">🛒 ShopEasy</h1>
                    <nav class="hidden md:flex space-x-6">
                        <button id="homeBtn" class="text-gray-600 hover:text-blue-600">Home</button>
                        <button id="productsBtn" class="text-gray-600 hover:text-blue-600">Products</button>
                    </nav>
                </div>
                
                <div class="flex items-center space-x-4">
                    <input type="text" id="searchBox" class="px-4 py-2 border rounded-lg w-64" placeholder="Search products...">
                    <button id="cartBtn" class="relative bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700">
                        Cart (<span id="cartCount">0</span>)
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-6xl mx-auto px-4 py-8">
        
        <!-- Home Page -->
        <div id="homePage">
            <!-- Hero Section -->
            <section class="bg-blue-600 text-white rounded-lg p-12 mb-12 text-center">
                <h2 class="text-4xl font-bold mb-4">Welcome to ShopEasy</h2>
                <p class="text-xl mb-6">Find amazing products at great prices</p>
                <button id="shopNowBtn" class="bg-white text-blue-600 px-8 py-3 rounded-lg font-semibold hover:bg-gray-100">
                    Shop Now
                </button>
            </section>

            <!-- Categories -->
            <section class="mb-12">
                <h3 class="text-2xl font-bold mb-6">Shop by Category</h3>
                <div class="grid grid-cols-2 md:grid-cols-4 gap-6">
                    <div class="category-card bg-white p-6 rounded-lg shadow text-center cursor-pointer hover:shadow-lg" onclick="filterCategory('electronics')">
                        <div class="text-4xl mb-3">📱</div>
                        <h4 class="font-semibold">Electronics</h4>
                    </div>
                    <div class="category-card bg-white p-6 rounded-lg shadow text-center cursor-pointer hover:shadow-lg" onclick="filterCategory('fashion')">
                        <div class="text-4xl mb-3">👕</div>
                        <h4 class="font-semibold">Fashion</h4>
                    </div>
                    <div class="category-card bg-white p-6 rounded-lg shadow text-center cursor-pointer hover:shadow-lg" onclick="filterCategory('home')">
                        <div class="text-4xl mb-3">🏠</div>
                        <h4 class="font-semibold">Home</h4>
                    </div>
                    <div class="category-card bg-white p-6 rounded-lg shadow text-center cursor-pointer hover:shadow-lg" onclick="filterCategory('books')">
                        <div class="text-4xl mb-3">📚</div>
                        <h4 class="font-semibold">Books</h4>
                    </div>
                </div>
            </section>

            <!-- Featured Products -->
            <section>
                <h3 class="text-2xl font-bold mb-6">Featured Products</h3>
                <div id="featuredGrid" class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-6"></div>
            </section>
        </div>

        <!-- Products Page -->
        <div id="productsPage" class="hidden">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-3xl font-bold">All Products</h2>
                <select id="sortSelect" class="px-4 py-2 border rounded-lg">
                    <option value="name">Sort by Name</option>
                    <option value="price-low">Price: Low to High</option>
                    <option value="price-high">Price: High to Low</option>
                </select>
            </div>
            <div id="productsGrid" class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-6"></div>
        </div>
    </main>

    <!-- Product Modal -->
    <div id="productModal" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
        <div class="bg-white rounded-lg p-8 max-w-md w-full mx-4">
            <div class="flex justify-between items-start mb-4">
                <h3 id="modalTitle" class="text-2xl font-bold"></h3>
                <button id="closeModal" class="text-gray-500 hover:text-gray-700 text-2xl">&times;</button>
            </div>
            <div id="modalImage" class="text-6xl text-center mb-4"></div>
            <p id="modalDescription" class="text-gray-600 mb-4"></p>
            <div class="flex items-center justify-between mb-6">
                <span id="modalPrice" class="text-2xl font-bold text-blue-600"></span>
                <div class="flex items-center space-x-2">
                    <button id="decreaseQty" class="bg-gray-200 px-3 py-1 rounded">-</button>
                    <span id="quantity">1</span>
                    <button id="increaseQty" class="bg-gray-200 px-3 py-1 rounded">+</button>
                </div>
            </div>
            <button id="addToCartModal" class="w-full bg-blue-600 text-white py-3 rounded-lg hover:bg-blue-700">
                Add to Cart
            </button>
        </div>
    </div>

    <!-- Cart Sidebar -->
    <div id="cartSidebar" class="hidden fixed right-0 top-0 h-full w-80 bg-white shadow-lg z-50">
        <div class="p-6 border-b">
            <div class="flex justify-between items-center">
                <h3 class="text-xl font-bold">Shopping Cart</h3>
                <button id="closeCart" class="text-gray-500 hover:text-gray-700">&times;</button>
            </div>
        </div>
        <div class="p-6 flex-1 overflow-y-auto">
            <div id="cartItems"></div>
        </div>
        <div class="p-6 border-t">
            <div class="flex justify-between items-center mb-4">
                <span class="text-lg font-semibold">Total: </span>
                <span id="cartTotal" class="text-xl font-bold text-blue-600">₹0</span>
            </div>
            <button id="checkoutBtn" class="w-full bg-green-600 text-white py-3 rounded-lg hover:bg-green-700">
                Checkout
            </button>
        </div>
    </div>

    <!-- Checkout Modal -->
    <div id="checkoutModal" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
        <div class="bg-white rounded-lg p-8 max-w-md w-full mx-4">
            <h3 class="text-2xl font-bold mb-6">Checkout</h3>
            <form id="checkoutForm">
                <div class="space-y-4">
                    <input type="text" id="customerName" class="w-full px-4 py-2 border rounded-lg" placeholder="Full Name" required>
                    <input type="email" id="customerEmail" class="w-full px-4 py-2 border rounded-lg" placeholder="Email" required>
                    <input type="tel" id="customerPhone" class="w-full px-4 py-2 border rounded-lg" placeholder="Phone Number" required>
                    <textarea id="customerAddress" class="w-full px-4 py-2 border rounded-lg" rows="3" placeholder="Address" required></textarea>
                </div>
                <div class="mt-6 p-4 bg-gray-50 rounded-lg">
                    <div class="flex justify-between mb-2">
                        <span>Total Amount:</span>
                        <span id="checkoutTotal" class="font-bold">₹0</span>
                    </div>
                </div>
                <div class="flex space-x-4 mt-6">
                    <button type="button" id="cancelCheckout" class="flex-1 bg-gray-500 text-white py-3 rounded-lg hover:bg-gray-600">
                        Cancel
                    </button>
                    <button type="submit" class="flex-1 bg-green-600 text-white py-3 rounded-lg hover:bg-green-700">
                        Place Order
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Success Modal -->
    <div id="successModal" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
        <div class="bg-white rounded-lg p-8 max-w-md w-full mx-4 text-center">
            <div class="text-6xl mb-4">✅</div>
            <h3 class="text-2xl font-bold mb-4">Order Placed Successfully!</h3>
            <p class="text-gray-600 mb-6">Thank you for shopping with us. Your order will be delivered soon.</p>
            <button id="closeSuccess" class="bg-blue-600 text-white px-6 py-3 rounded-lg hover:bg-blue-700">
                Continue Shopping
            </button>
        </div>
    </div>

    <script>
        // Simple product data with Indian Rupee prices
        const products = [
            { id: 1, name: "iPhone 15", price: 79900, category: "electronics", image: "📱", description: "Latest iPhone with advanced features" },
            { id: 2, name: "Samsung Galaxy S24", price: 74999, category: "electronics", image: "📱", description: "Premium Android smartphone" },
            { id: 3, name: "MacBook Air", price: 114900, category: "electronics", image: "💻", description: "Lightweight laptop for professionals" },
            { id: 4, name: "Cotton T-Shirt", price: 999, category: "fashion", image: "👕", description: "Comfortable cotton t-shirt" },
            { id: 5, name: "Jeans", price: 2499, category: "fashion", image: "👖", description: "Classic blue jeans" },
            { id: 6, name: "Running Shoes", price: 4999, category: "fashion", image: "👟", description: "Comfortable running shoes" },
            { id: 7, name: "Coffee Maker", price: 8999, category: "home", image: "☕", description: "Automatic coffee maker" },
            { id: 8, name: "Table Lamp", price: 1999, category: "home", image: "💡", description: "Modern LED table lamp" },
            { id: 9, name: "Cooking Book", price: 599, category: "books", image: "📖", description: "Learn to cook delicious meals" },
            { id: 10, name: "Novel", price: 399, category: "books", image: "📚", description: "Bestselling fiction novel" }
        ];

        let cart = [];
        let currentProduct = null;
        let currentQuantity = 1;

        // Initialize app
        document.addEventListener('DOMContentLoaded', function() {
            displayFeaturedProducts();
            setupEventListeners();
        });

        function setupEventListeners() {
            // Navigation
            document.getElementById('homeBtn').addEventListener('click', showHome);
            document.getElementById('productsBtn').addEventListener('click', showProducts);
            document.getElementById('shopNowBtn').addEventListener('click', showProducts);
            
            // Search
            document.getElementById('searchBox').addEventListener('input', handleSearch);
            document.getElementById('sortSelect').addEventListener('change', sortProducts);
            
            // Cart
            document.getElementById('cartBtn').addEventListener('click', toggleCart);
            document.getElementById('closeCart').addEventListener('click', closeCart);
            
            // Modal
            document.getElementById('closeModal').addEventListener('click', closeModal);
            document.getElementById('decreaseQty').addEventListener('click', () => changeQuantity(-1));
            document.getElementById('increaseQty').addEventListener('click', () => changeQuantity(1));
            document.getElementById('addToCartModal').addEventListener('click', addToCartFromModal);
            
            // Checkout
            document.getElementById('checkoutBtn').addEventListener('click', showCheckout);
            document.getElementById('cancelCheckout').addEventListener('click', closeCheckout);
            document.getElementById('checkoutForm').addEventListener('submit', placeOrder);
            document.getElementById('closeSuccess').addEventListener('click', closeSuccess);
        }

        function displayFeaturedProducts() {
            const grid = document.getElementById('featuredGrid');
            const featured = products.slice(0, 8);
            grid.innerHTML = featured.map(product => createProductCard(product)).join('');
        }

        function displayAllProducts(productsToShow = products) {
            const grid = document.getElementById('productsGrid');
            grid.innerHTML = productsToShow.map(product => createProductCard(product)).join('');
        }

        function createProductCard(product) {
            return `
                <div class="bg-white rounded-lg shadow hover:shadow-lg transition-shadow cursor-pointer" onclick="showProductModal(${product.id})">
                    <div class="p-6 text-center">
                        <div class="text-5xl mb-4">${product.image}</div>
                        <h4 class="font-semibold mb-2">${product.name}</h4>
                        <p class="text-2xl font-bold text-blue-600 mb-4">₹${product.price.toLocaleString()}</p>
                        <button class="w-full bg-blue-600 text-white py-2 rounded hover:bg-blue-700" onclick="event.stopPropagation(); quickAddToCart(${product.id})">
                            Add to Cart
                        </button>
                    </div>
                </div>
            `;
        }

        function showProductModal(productId) {
            currentProduct = products.find(p => p.id === productId);
            currentQuantity = 1;
            
            document.getElementById('modalTitle').textContent = currentProduct.name;
            document.getElementById('modalImage').textContent = currentProduct.image;
            document.getElementById('modalDescription').textContent = currentProduct.description;
            document.getElementById('modalPrice').textContent = `₹${currentProduct.price.toLocaleString()}`;
            document.getElementById('quantity').textContent = currentQuantity;
            
            document.getElementById('productModal').classList.remove('hidden');
        }

        function closeModal() {
            document.getElementById('productModal').classList.add('hidden');
        }

        function changeQuantity(change) {
            currentQuantity = Math.max(1, currentQuantity + change);
            document.getElementById('quantity').textContent = currentQuantity;
        }

        function addToCartFromModal() {
            addToCart(currentProduct, currentQuantity);
            closeModal();
        }

        function quickAddToCart(productId) {
            const product = products.find(p => p.id === productId);
            addToCart(product, 1);
        }

        function addToCart(product, quantity) {
            const existingItem = cart.find(item => item.id === product.id);
            
            if (existingItem) {
                existingItem.quantity += quantity;
            } else {
                cart.push({ ...product, quantity });
            }
            
            updateCartDisplay();
            showMessage(`${product.name} added to cart!`);
        }

        function updateCartDisplay() {
            const cartCount = cart.reduce((sum, item) => sum + item.quantity, 0);
            document.getElementById('cartCount').textContent = cartCount;
            
            const cartItems = document.getElementById('cartItems');
            const cartTotal = document.getElementById('cartTotal');
            
            if (cart.length === 0) {
                cartItems.innerHTML = '<p class="text-gray-500 text-center">Your cart is empty</p>';
                cartTotal.textContent = '₹0';
                return;
            }
            
            cartItems.innerHTML = cart.map(item => `
                <div class="flex justify-between items-center py-3 border-b">
                    <div>
                        <div class="font-semibold">${item.name}</div>
                        <div class="text-sm text-gray-600">₹${item.price.toLocaleString()} × ${item.quantity}</div>
                    </div>
                    <div class="flex items-center space-x-2">
                        <button onclick="updateCartQuantity(${item.id}, -1)" class="bg-gray-200 px-2 py-1 rounded text-sm">-</button>
                        <span>${item.quantity}</span>
                        <button onclick="updateCartQuantity(${item.id}, 1)" class="bg-gray-200 px-2 py-1 rounded text-sm">+</button>
                        <button onclick="removeFromCart(${item.id})" class="text-red-500 ml-2">🗑️</button>
                    </div>
                </div>
            `).join('');
            
            const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            cartTotal.textContent = `₹${total.toLocaleString()}`;
        }

        function updateCartQuantity(productId, change) {
            const item = cart.find(item => item.id === productId);
            if (item) {
                item.quantity += change;
                if (item.quantity <= 0) {
                    removeFromCart(productId);
                } else {
                    updateCartDisplay();
                }
            }
        }

        function removeFromCart(productId) {
            cart = cart.filter(item => item.id !== productId);
            updateCartDisplay();
        }

        function toggleCart() {
            const sidebar = document.getElementById('cartSidebar');
            sidebar.classList.toggle('hidden');
            updateCartDisplay();
        }

        function closeCart() {
            document.getElementById('cartSidebar').classList.add('hidden');
        }

        function showCheckout() {
            if (cart.length === 0) {
                showMessage('Your cart is empty!');
                return;
            }
            
            const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            document.getElementById('checkoutTotal').textContent = `₹${total.toLocaleString()}`;
            document.getElementById('checkoutModal').classList.remove('hidden');
            closeCart();
        }

        function closeCheckout() {
            document.getElementById('checkoutModal').classList.add('hidden');
        }

        function placeOrder(e) {
            e.preventDefault();
            
            // Simple form validation
            const form = e.target;
            if (!form.checkValidity()) {
                return;
            }
            
            // Clear cart and show success
            cart = [];
            updateCartDisplay();
            
            document.getElementById('checkoutModal').classList.add('hidden');
            document.getElementById('successModal').classList.remove('hidden');
        }

        function closeSuccess() {
            document.getElementById('successModal').classList.add('hidden');
            showHome();
        }

        function showHome() {
            document.getElementById('homePage').classList.remove('hidden');
            document.getElementById('productsPage').classList.add('hidden');
        }

        function showProducts() {
            document.getElementById('homePage').classList.add('hidden');
            document.getElementById('productsPage').classList.remove('hidden');
            displayAllProducts();
        }

        function filterCategory(category) {
            showProducts();
            const filtered = products.filter(p => p.category === category);
            displayAllProducts(filtered);
        }

        function handleSearch() {
            const searchTerm = document.getElementById('searchBox').value.toLowerCase();
            if (searchTerm.length > 0) {
                const filtered = products.filter(p => 
                    p.name.toLowerCase().includes(searchTerm) || 
                    p.description.toLowerCase().includes(searchTerm)
                );
                showProducts();
                displayAllProducts(filtered);
            }
        }

        function sortProducts() {
            const sortBy = document.getElementById('sortSelect').value;
            let sorted = [...products];
            
            switch(sortBy) {
                case 'name':
                    sorted.sort((a, b) => a.name.localeCompare(b.name));
                    break;
                case 'price-low':
                    sorted.sort((a, b) => a.price - b.price);
                    break;
                case 'price-high':
                    sorted.sort((a, b) => b.price - a.price);
                    break;
            }
            
            displayAllProducts(sorted);
        }

        function showMessage(message) {
            const messageDiv = document.createElement('div');
            messageDiv.className = 'fixed top-4 right-4 bg-green-500 text-white px-4 py-2 rounded-lg z-50';
            messageDiv.textContent = message;
            document.body.appendChild(messageDiv);
            
            setTimeout(() => {
                messageDiv.remove();
            }, 2000);
        }
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'97d66703f74d59f0',t:'MTc1NzU4NTcxMS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
