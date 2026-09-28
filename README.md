# Ola-Ola-food-canteen-

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ola-Ola Food Canteen - Management System</title>
    <style>
        :root {
            --primary: #113689;
            --primary-dark: #0a235c;
            --accent: #f8c21a;
            --danger: #dc3545;
            --success: #28a745;
            --bg: #f4f6f9;
            --card-bg: #ffffff;
            --text: #333333;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg);
            color: var(--text);
            padding: 20px;
        }

        .header {
            text-align: center;
            background: var(--primary);
            color: white;
            padding: 20px;
            border-radius: 8px 8px 0 0;
        }

        .header h1 {
            color: var(--accent);
            font-size: 28px;
            margin-bottom: 5px;
        }

        .header p {
            font-size: 14px;
            opacity: 0.9;
        }

        .container {
            max-width: 1100px;
            margin: 0 auto;
            background: var(--card-bg);
            border-radius: 8px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            overflow: hidden;
        }

        .content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            padding: 20px;
        }

        @media (max-width: 850px) {
            .content {
                grid-template-columns: 1fr;
            }
        }

        .section-title {
            color: var(--primary);
            border-bottom: 2px solid var(--accent);
            padding-bottom: 8px;
            margin-bottom: 15px;
            font-size: 18px;
        }

        .customer-details {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 20px;
        }

        .form-group {
            display: flex;
            flex-direction: column;
        }

        .form-group label {
            font-size: 13px;
            font-weight: bold;
            margin-bottom: 5px;
        }

        input, select {
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            font-size: 14px;
        }

        input:focus, select:focus {
            outline: none;
            border-color: var(--primary);
        }

        .menu-table-container {
            max-height: 400px;
            overflow-y: auto;
            border: 1px solid #ddd;
            border-radius: 4px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        th, td {
            padding: 10px;
            text-align: left;
            border-bottom: 1px solid #ddd;
            font-size: 14px;
        }

        th {
            background-color: var(--primary);
            color: white;
            position: sticky;
            top: 0;
        }

        .qty-input {
            width: 60px;
            padding: 5px;
            text-align: center;
        }

        .btn {
            padding: 8px 12px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.2s;
        }

        .btn-add {
            background-color: var(--primary);
            color: white;
        }

        .btn-add:hover {
            background-color: var(--primary-dark);
        }

        .btn-danger {
            background-color: var(--danger);
            color: white;
            padding: 4px 8px;
            font-size: 12px;
        }

        .btn-action {
            width: 100%;
            padding: 12px;
            margin-top: 10px;
            font-size: 16px;
        }

        .btn-success {
            background-color: var(--success);
            color: white;
        }

        .btn-clear {
            background-color: #6c757d;
            color: white;
        }

        .receipt-section {
            background: #fafafa;
            border: 1px solid #eee;
            padding: 15px;
            border-radius: 4px;
        }

        .summary-row {
            display: flex;
            justify-content: space-between;
            margin: 10px 0;
            font-size: 16px;
        }

        .total-amount {
            font-size: 22px;
            font-weight: bold;
            color: var(--primary);
        }

        .payment-box {
            background: #eef2fa;
            padding: 15px;
            border-radius: 4px;
            margin-top: 15px;
        }

        /* Printable Receipt Styling */
        @media print {
            body * {
                visibility: hidden;
            }
            #printableReceipt, #printableReceipt * {
                visibility: visible;
            }
            #printableReceipt {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
                padding: 10px;
            }
            .no-print {
                display: none !important;
            }
        }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <h1>OLA-OLA FOOD CANTEEN</h1>
        <p>Indoor & Outdoor Catering Services</p>
        <p>2, Oluaina Street, Off Isolo Road, Mushin, Lagos | Tel: +234 803 9222 123</p>
    </div>

    <div class="content">
        <!-- Left Column: Menu and Items Selection -->
        <div>
            <h2 class="section-title">Food Menu</h2>
            <div class="menu-table-container">
                <table>
                    <thead>
                        <tr>
                            <th>Item</th>
                            <th>Price</th>
                            <th>Qty</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody id="menuTable">
                        <!-- Populated dynamically via JS -->
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Right Column: Customer Info, Cart & Checkout -->
        <div>
            <h2 class="section-title">Customer & Order Details</h2>
            
            <div class="customer-details">
                <div class="form-group">
                    <label for="customerName">Customer Name:</label>
                    <input type="text" id="customerName" placeholder="Enter name">
                </div>
                <div class="form-group">
                    <label for="orderType">Order Type:</label>
                    <select id="orderType">
                        <option value="Dine-in">Dine-in</option>
                        <option value="Takeaway">Takeaway</option>
                        <option value="Outdoor Catering">Outdoor Catering</option>
                    </select>
                </div>
            </div>

            <div class="receipt-section" id="printableReceipt">
                <h3 class="section-title">Current Order</h3>
                <table>
                    <thead>
                        <tr>
                            <th>Item</th>
                            <th>Qty</th>
                            <th>Price</th>
                            <th>Total</th>
                            <th class="no-print">Remove</th>
                        </tr>
                    </thead>
                    <tbody id="orderList">
                        <!-- Order items will appear here -->
                    </tbody>
                </table>

                <hr style="margin: 15px 0;">

                <div class="summary-row total-amount">
                    <span>Total Amount:</span>
                    <span>₦<span id="total">0</span></span>
                </div>

                <div class="payment-box no-print">
                    <div class="form-group" style="margin-bottom: 10px;">
                        <label for="amountPaid">Amount Paid (₦):</label>
                        <input type="number" id="amountPaid" placeholder="0" oninput="calculateChange()">
                    </div>

                    <div class="summary-row" style="font-weight: bold; color: var(--success);">
                        <span>Change / Balance:</span>
                        <span>₦<span id="change">0</span></span>
                    </div>
                </div>

                <div class="no-print" style="margin-top: 15px;">
                    <button class="btn btn-success btn-action" onclick="printReceipt()">Print Receipt</button>
                    <button class="btn btn-clear btn-action" onclick="clearOrder()">Clear Order</button>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    // Food items from Ola-Ola Food Canteen menu
    const menuItems = [
        { id: 1, name: "Amala", price: 200 },
        { id: 2, name: "Pounded Yam", price: 300 },
        { id: 3, name: "Eba", price: 200 },
        { id: 4, name: "Semo", price: 200 },
        { id: 5, name: "Jollof Rice", price: 300 },
        { id: 6, name: "Fried Rice", price: 300 },
        { id: 7, name: "White Rice", price: 200 },
        { id: 8, name: "Plantain", price: 200 },
        { id: 9, name: "Goat Meat (Ogunfe)", price: 800 },
        { id: 10, name: "Assorted Meat", price: 500 },
        { id: 11, name: "Fish", price: 600 },
        { id: 12, name: "Salad", price: 300 },
        { id: 13, name: "Soft Drink", price: 300 },
        { id: 14, name: "Bottled Water", price: 200 }
    ];

    let order = [];
    let grandTotal = 0;

    // Load Menu Into Table
    function renderMenu() {
        const menuTable = document.getElementById("menuTable");
        menuTable.innerHTML = "";

        menuItems.forEach((item) => {
            const row = document.createElement("tr");
            row.innerHTML = `
                <td>${item.name}</td>
                <td>₦${item.price}</td>
                <td><input type="number" id="qty-${item.id}" class="qty-input" min="1" value="1"></td>
                <td><button class="btn btn-add" onclick="addFood(${item.id})">Add</button></td>
            `;
            menuTable.appendChild(row);
        });
    }

    // Add Food to Order
    function addFood(itemId) {
        const item = menuItems.find(m => m.id === itemId);
        const qtyInput = document.getElementById(`qty-${itemId}`);
        const quantity = parseInt(qtyInput.value);

        if (isNaN(quantity) || quantity <= 0) {
            alert("Please enter a valid quantity.");
            return;
        }

        // Check if item exists in order
        const existingIndex = order.findIndex(o => o.id === itemId);
        if (existingIndex > -1) {
            order[existingIndex].quantity += quantity;
            order[existingIndex].subtotal = order[existingIndex].quantity * item.price;
        } else {
            order.push({
                id: item.id,
                name: item.name,
                price: item.price,
                quantity: quantity,
                subtotal: item.price * quantity
            });
        }

        // Reset quantity input to 1
        qtyInput.value = 1;

        updateOrderUI();
    }

    // Remove Item from Order
    function removeItem(index) {
        order.splice(index, 1);
        updateOrderUI();
    }

    // Update Receipt & Totals UI
    function updateOrderUI() {
        const orderList = document.getElementById("orderList");
        orderList.innerHTML = "";
        grandTotal = 0;

        order.forEach((item, index) => {
            grandTotal += item.subtotal;
            const row = document.createElement("tr");
            row.innerHTML = `
                <td>${item.name}</td>
                <td>${item.quantity}</td>
                <td>₦${item.price}</td>
                <td>₦${item.subtotal}</td>
                <td class="no-print"><button class="btn btn-danger" onclick="removeItem(${index})">X</button></td>
            `;
            orderList.appendChild(row);
        });

        document.getElementById("total").innerText = grandTotal.toLocaleString();
        calculateChange();
    }

    // Calculate Change
    function calculateChange() {
        const amountPaid = parseFloat(document.getElementById("amountPaid").value) || 0;
        const changeDisplay = document.getElementById("change");

        if (amountPaid >= grandTotal) {
            const change = amountPaid - grandTotal;
            changeDisplay.innerText = change.toLocaleString();
        } else {
            changeDisplay.innerText = "0";
        }
    }

    // Clear Order
    function clearOrder() {
        order = [];
        grandTotal = 0;
        document.getElementById("customerName").value = "";
        document.getElementById("amountPaid").value = "";
        document.getElementById("change").innerText = "0";
        updateOrderUI();
    }

    // Print Receipt Function
    function printReceipt() {
        const customerName = document.getElementById("customerName").value.trim();

        if (order.length === 0) {
            alert("Please add at least one item to the order before printing.");
            return;
        }

        if (customerName === "") {
            alert("Please enter the customer name.");
            return;
        }

        window.print();
    }

    // Initialize Menu on Load
    window.onload = renderMenu;
</script>

</body>
</html>
