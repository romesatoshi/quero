#<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>**QUERO**</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.1.1/css/all.min.css" />
    <style>
        #cart-icon {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 50;
            width: 40px;
            height: 40px;
            background-color: #fff;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
        }
        
        .cart-icon {
            width: 20px;
            height: 20px;
            font-size: 16px;
            color: #666;
            transition: color 0.3s ease;
        }
        
        .cart-icon:hover {
            color: #333;
        }
        
        .cart-count {
            position: absolute;
            top: -8px;
            right: -8px;
            width: 16px;
            height: 16px;
            background-color: #ff0000;
            color: #fff;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 10px;
        }
    </style>
</head>
<body>
    <nav class="bg-gray-100 flex justify-between items-center p-4 md:p-6 lg:p-8">
        <h1 class="text-3xl text-gray-900 font-bold">QUERO</h1>
    </nav>
    <div class="max-w-7xl mx-auto p-4 md:p-6 lg:p-8 bg-gray-100 rounded-lg shadow-md mt-4">
        <div class="flex justify-between items-center mb-4">
            <input 
                type="search" 
                id="search" 
                placeholder="Search for food" 
                class="w-full md:w-1/2 lg:w-1/3 p-2 pl-10 text-sm text-gray-700 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-gray-600" 
                oninput="searchMenuItems()"
            />
            <div class="flex items-center">
                <button class="text-gray-600 hover:text-gray-900 transition duration-300 mr-4">Filter</button>
            </div>
        </div>
        <div class="flex flex-wrap justify-center mb-4">
            <button 
                class="text-sm text-gray-600 hover:text-gray-900 transition duration-300 mr-4 active:bg-gray-200 active:p-2 active:rounded-lg" 
                onclick="filterCategory('Starters')"
            >Starters</button>
            <button 
                class="text-sm text-gray-600 hover:text-gray-900 transition duration-300 mr-4 active:bg-gray-200 active:p-2 active:rounded-lg" 
                onclick="filterCategory('Main')"
            >Main</button>
            <button 
                class="text-sm text-gray-600 hover:text-gray-900 transition duration-300 mr-4 active:bg-gray-200 active:p-2 active:rounded-lg" 
                onclick="filterCategory('Desserts')"
            >Desserts</button>
            <button 
                class="text-sm text-gray-600 hover:text-gray-900 transition duration-300 mr-4 active:bg-gray-200 active:p-2 active:rounded-lg" 
                onclick="filterCategory('Drinks')"
            >Drinks</button>
            <button 
                class="text-sm text-gray-600 hover:text-gray-900 transition duration-300 active:bg-gray-200 active:p-2 active:rounded-lg" 
                onclick="filterCategory('All')"
            >All</button>
        </div>
        <div id="menu" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
            <!-- Menu items will be added here -->
        </div>
        <div id="order-info-modal" class="hidden fixed top-0 left-0 z-50 w-full h-full bg-black bg-opacity-50 flex items-center justify-center">
            <div class="bg-white rounded-lg shadow-md p-4 w-1/2">
                <h2 class="text-lg text-gray-900 mb-2 font-bold">Order Information</h2>
                <ul id="order-info-list">
                    <!-- Order information will be added here -->
                </ul>
                <p id="total" class="text-lg text-gray-900 mb-2 font-bold"></p>
                <button 
                    id="place-order-button" 
                    class="bg-gray-200 p-2 rounded-lg text-sm font-bold text-black hover:text-black transition duration-300" 
                    onclick="placeOrder()"
                >Place Order</button>
                <button 
                    id="cancel-order-info" 
                    class="bg-gray-200 p-2 rounded-lg text-sm text-gray-600 hover:text-gray-900 transition duration-300" 
                    onclick="cancelOrderInfo()"
                >Cancel</button>
            </div>
        </div>
        <div id="confirm-modal" class="hidden fixed top-0 left-0 z-50 w-full h-full bg-black bg-opacity-50 flex items-center justify-center">
            <div class="bg-white rounded-lg shadow-md p-4 w-1/2">
                <h2 class="text-lg text-gray-900 mb-2 font-bold">Confirm Order</h2>
                <label class="text-sm text-gray-600">Please enter your name:</label>
                <input 
                    type="text" 
                    id="customer-name" 
                    placeholder="Customer Name" 
                    required
                    class="w-full p-2 pl-10 text-sm text-gray-700 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-gray-600 mb-4"
                />
                <p id="customer-name-error" class="text-red-500 text-sm hidden">Please enter your name.</p>
                <p class="text-sm text-gray-600 mb-4">Are you sure you want to place this order?</p>
                <button 
                    id="confirm-order" 
                    class="bg-gray-200 p-2 rounded-lg text-sm text-gray-600 hover:text-gray-900 transition duration-300 mr-4" 
                    onclick="sendOrderToServer()"
                >Yes</button>
                <button 
                    id="cancel-confirm-order" 
                    class="bg-gray-200 p-2 rounded-lg text-sm text-gray-600 hover:text-gray-900 transition duration-300" 
                    onclick="cancelConfirmOrder()"
                >No</button>
            </div>
        </div>
        <div id="thank-you-modal" class="hidden fixed top-0 left-0 z-50 w-full h-full bg-black bg-opacity-50 flex items-center justify-center">
            <div class="bg-white rounded-lg shadow-md p-4 w-1/2">
                <h2 class="text-lg text-gray-900 mb-2 font-bold">Thank You</h2>
                <p id="customer-name-message" class="text-sm text-gray-600 mb-4"></p>
                <p id="order-summary" class="text-sm text-gray-600 mb-4"></p>
                <p id="order-confirmation-number" class="text-sm text-red-500 mb-4 font-bold"></p>
                <p id="estimated-delivery-time" class="text-sm text-gray-600 mb-4"></p>
                <p id="order-total" class="text-sm text-gray-600 mb-4 font-bold"></p>
                <button 
                    id="track-order" 
                    class="bg-gray-200 p-2 rounded-lg text-sm text-gray-600 hover:text-gray-900 transition duration-300 mr-4" 
                    onclick="trackOrder()"
                >Track Order</button>
                <button 
                    id="place-new-order" 
                    class="bg-gray-200 p-2 rounded-lg text-sm text-gray-600 hover:text-gray-900 transition duration-300" 
                    onclick="placeNewOrder()"
                >Place New Order</button>
            </div>
        </div>
    </div>
    <div id="cart-icon" class="bg-gray-200 p-2 rounded-lg text-sm text-gray-600 hover:text-gray-900 transition duration-300 absolute bottom-4 right-4 md:bottom-6 md:right-6 lg:bottom-8 lg:right-8 z-50 flex justify-center items-center cursor-pointer" onclick="viewOrderInfo()">
        <i class="fa-solid fa-cart-shopping cart-icon"></i>
        <div id="cart-count" class="cart-count">0</div>
    </div>
    <script>
        let cart = [];
        let menuItems = [
            { id: 1, name: 'Rice', category: 'Main', price: 4999 },
            { id: 2, name: 'Chicken', category: 'Main', price: 7999 },
            { id: 3, name: 'Salad', category: 'Starters', price: 2999 },
            { id: 4, name: 'Shawarma', category: 'Main', price: 5999 },
            { id: 5, name: 'Turkey Salad', category: 'Starters', price: 3999 },
            { id: 6, name: 'Cake', category: 'Desserts', price: 1999 },
            { id: 7, name: 'Samosa', category: 'Starters', price: 499 },
        ];
        
        function filterCategory(category) {
            let menu = document.getElementById('menu');
            menu.innerHTML = '';
            if (category === 'All') {
                menuItems.forEach(item => {
                    let div = document.createElement('div');
                    div.classList.add('bg-white', 'rounded-lg', 'shadow-md', 'p-4', 'm-2');
                    div.innerHTML = `
                        <div class="bg-gray-200 border-2 border-dashed rounded-xl w-32 h-32 mb-4"></div>
                        <h2 class="text-lg text-gray-900 mb-2 font-semibold">${item.name}</h2>
                        <p class="text-sm text-gray-600 mb-4 font-semibold">₦${(item.price).toLocaleString()}</p>
                        <button class="text-sm text-gray-600 hover:text-gray-900 transition duration-300 bg-gray-200 p-2 rounded-lg font-bold" onclick="addToCart(${item.id})"><b>Add to Cart</b></button>
                    `;
                    menu.appendChild(div);
                });
            } else {
                menuItems.forEach(item => {
                    if (item.category === category) {
                        let div = document.createElement('div');
                        div.classList.add('bg-white', 'rounded-lg', 'shadow-md', 'p-4', 'm-2');
                        div.innerHTML = `
                            <div class="bg-gray-200 border-2 border-dashed rounded-xl w-32 h-32 mb-4"></div>
                            <h2 class="text-lg text-gray-900 mb-2 font-semibold">${item.name}</h2>
                            <p class="text-sm text-gray-600 mb-4 font-semibold">₦${(item.price).toLocaleString()}</p>
                            <button class="text-sm text-gray-600 hover:text-gray-900 transition duration-300 bg-gray-200 p-2 rounded-lg font-bold" onclick="addToCart(${item.id})"><b>Add to Cart</b></button>
                        `;
                        menu.appendChild(div);
                    }
                });
            }
        }

        function searchMenuItems() {
            let searchQuery = document.getElementById('search').value.toLowerCase();
            let menu = document.getElementById('menu');
            menu.innerHTML = '';
            menuItems.forEach(item => {
                if (item.name.toLowerCase().includes(searchQuery)) {
                    let div = document.createElement('div');
                    div.classList.add('bg-white', 'rounded-lg', 'shadow-md', 'p-4', 'm-2');
                    div.innerHTML = `
                        <div class="bg-gray-200 border-2 border-dashed rounded-xl w-32 h-32 mb-4"></div>
                        <h2 class="text-lg text-gray-900 mb-2 font-semibold">${item.name}</h2>
                        <p class="text-sm text-gray-600 mb-4 font-semibold">₦${(item.price).toLocaleString()}</p>
                        <button class="text-sm text-gray-600 hover:text-gray-900 transition duration-300 bg-gray-200 p-2 rounded-lg font-bold" onclick="addToCart(${item.id})"><b>Add to Cart</b></button>
                    `;
                    menu.appendChild(div);
                }
            });
        }

        function addToCart(id) {
            let item = menuItems.find(item => item.id === id);
            let existingItem = cart.find(cartItem => cartItem.item.id === id);
            if (existingItem) {
                existingItem.quantity++;
            } else {
                cart.push({ item, quantity: 1 });
            }
            updateCart();
            showCartIcon();
        }

        function updateCart() {
            let cartCount = document.getElementById('cart-count');
            cartCount.textContent = cart.reduce((acc, cartItem) => acc + cartItem.quantity, 0);
        }

        function showCartIcon() {
            let cartIcon = document.getElementById('cart-icon');
            if (cart.length > 0) {
                cartIcon.style.display = 'flex';
            } else {
                cartIcon.style.display = 'flex';
            }
        }

        function viewOrderInfo() {
            document.getElementById('order-info-modal').classList.remove('hidden');
            let orderInfoList = document.getElementById('order-info-list');
            orderInfoList.innerHTML = '';
            let total = 0;
            cart.forEach(cartItem => {
                total += cartItem.item.price * cartItem.quantity;
                let li = document.createElement('li');
                li.classList.add('flex', 'justify-between', 'items-center', 'mb-2');
                li.innerHTML = `
                    <span class="text-sm text-gray-600 font-semibold">${cartItem.item.name}</span>
                    <span class="text-sm text-gray-600">x${cartItem.quantity}</span>
                    <span class="text-sm text-gray-600 font-semibold">₦${(cartItem.item.price).toLocaleString()}</span>
                    <span class="text-sm text-gray-600 font-semibold">₦${(cartItem.item.price * cartItem.quantity).toLocaleString()}</span>
                    <div class="flex items-center">
                        <button class="text-sm text-gray-600 hover:text-gray-900 transition duration-300 mr-2" onclick="decreaseQuantity(${cartItem.item.id})">-</button>
                        <button class="text-sm text-gray-600 hover:text-gray-900 transition duration-300 mr-2" onclick="increaseQuantity(${cartItem.item.id})">+</button>
                        <button class="text-sm text-gray-600 hover:text-gray-900 transition duration-300" onclick="removeFromCart(${cartItem.item.id})">Remove</button>
                    </div>
                `;
                orderInfoList.appendChild(li);
            });
            document.getElementById('total').innerHTML = `<b>Total: ₦${(total).toLocaleString()}</b>`;
        }

        function decreaseQuantity(id) {
            let item = cart.find(cartItem => cartItem.item.id === id);
            if (item.quantity > 1) {
                item.quantity--;
            } else {
                cart = cart.filter(cartItem => cartItem.item.id !== id);
            }
            updateCart();
            showCartIcon();
            viewOrderInfo();
        }

        function increaseQuantity(id) {
            let item = cart.find(cartItem => cartItem.item.id === id);
            item.quantity++;
            updateCart();
            showCartIcon();
            viewOrderInfo();
        }

        function removeFromCart(id) {
            cart = cart.filter(cartItem => cartItem.item.id !== id);
            updateCart();
            showCartIcon();
            viewOrderInfo();
        }

        function cancelOrderInfo() {
            document.getElementById('order-info-modal').classList.add('hidden');
        }

        function placeOrder() {
            if (cart.length === 0) {
                // No alert since we are not allowed to use alert.
            } else {
                document.getElementById('order-info-modal').classList.add('hidden');
                document.getElementById('confirm-modal').classList.remove('hidden');
            }
        }

        function sendOrderToServer() {
            let customerName = document.getElementById('customer-name').value;
            if (customerName === '') {
                // No alert since we are not allowed to use alert.
            } else {
                let total = cart.reduce((acc, cartItem) => acc + cartItem.item.price * cartItem.quantity, 0);
                let orderSummary = cart.map(cartItem => `<b>${cartItem.item.name}</b> x${cartItem.quantity} = ₦${(cartItem.item.price * cartItem.quantity).toLocaleString()}`).join('<br>');
                let orderConfirmationNumber = Math.floor(Math.random() * 1000000);
                document.getElementById('customer-name-message').innerHTML = `Hello, ${customerName}!`;
                document.getElementById('order-summary').innerHTML = `Order Details:<br>${orderSummary}`;
                document.getElementById('order-confirmation-number').innerHTML = `Order Confirmation Number: <b class="text-red-500">${orderConfirmationNumber}</b>`;
                document.getElementById('estimated-delivery-time').textContent = 'Estimated Delivery Time: 2 minutes';
                document.getElementById('order-total').innerHTML = `Order Total: ₦${(total).toLocaleString()}`;
                document.getElementById('confirm-modal').classList.add('hidden');
                document.getElementById('thank-you-modal').classList.remove('hidden');
                // Send order to server logic
            }
        }

        function cancelConfirmOrder() {
            document.getElementById('confirm-modal').classList.add('hidden');
        }

        function trackOrder() {
            console.log('Order tracked');
        }

        function placeNewOrder() {
            document.getElementById('thank-you-modal').classList.add('hidden');
            cart = [];
            updateCart();
            showCartIcon();
        }

        filterCategory('All');
    </script>
</body>
</html> quero
