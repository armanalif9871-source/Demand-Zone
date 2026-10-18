<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Demand Zone</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <!-- Login Section -->
    <div id="login-screen" class="card">
        <h2>Demand Zone</h2>
        <p>Admin Login</p>
        <input type="password" id="password" placeholder="পাসওয়ার্ড লিখুন">
        <button onclick="login()">Login</button>
        <p id="error-msg" style="color:red; display:none; margin-top:10px;">ভুল পাসওয়ার্ড!</p>
    </div>

    <!-- Dashboard Section -->
    <div id="dashboard" class="dashboard" style="display:none;">
        <header class="no-print">
            <h1>Demand Zone</h1>
            <button class="logout-btn" onclick="logout()">Logout</button>
        </header>

        <!-- Form to Add Dress -->
        <div class="card no-print">
            <h3>নতুন ড্রেস যোগ করুন</h3>
            <form id="add-dress-form" onsubmit="addDress(event)">
                <input type="text" id="dress-name" placeholder="পোশাকের নাম" required>
                <input type="text" id="dress-code" placeholder="কোড" required>
                <input type="number" id="dress-price" placeholder="মূল্য (৳)" required>
                <input type="number" id="dress-qty" placeholder="পরিমাণ (টি)" required>
                <input type="url" id="dress-img" placeholder="ছবির ইউআরএল (Image URL)" required>
                <button type="submit">যোগ করুন</button>
            </form>
        </div>

        <!-- Stock Table -->
        <div class="card">
            <div class="table-header">
                <h3>স্টক তালিকা</h3>
                <button class="print-btn no-print" onclick="window.print()">Print</button>
            </div>
            <table>
                <thead>
                    <tr>
                        <th>ছবি</th>
                        <th>পোশাকের নাম</th>
                        <th>কোড</th>
                        <th>মূল্য</th>
                        <th>পরিমাণ</th>
                        <th>স্ট্যাটাস</th>
                        <th class="no-print">অ্যাকশন</th>
                    </tr>
                </thead>
                <tbody id="stock-list">
                    <!-- Dynamic Data -->
                </tbody>
            </table>
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>
* {
    box-sizing: border-box;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    margin: 0;
    padding: 0;
}

body {
    background-color: #f4f6f9;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    padding: 20px;
}

.card {
    background: #ffffff;
    padding: 25px;
    border-radius: 10px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    width: 100%;
    max-width: 800px;
    margin-bottom: 20px;
}

#login-screen {
    max-width: 350px;
    text-align: center;
}

#login-screen h2 {
    color: #4a154b;
    margin-bottom: 5px;
}

input {
    width: 100%;
    padding: 10px;
    margin: 8px 0;
    border: 1px solid #ccc;
    border-radius: 5px;
}

button {
    width: 100%;
    padding: 10px;
    background-color: #4a154b;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
}

button:hover {
    background-color: #360e37;
}

.dashboard {
    width: 100%;
    max-width: 900px;
}

header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #4a154b;
    color: white;
    padding: 15px 20px;
    border-radius: 10px;
    margin-bottom: 20px;
}

.logout-btn {
    width: auto;
    background-color: #e74c3c;
    padding: 8px 15px;
}

.table-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 15px;
}

.print-btn {
    width: auto;
    background-color: #27ae60;
    padding: 8px 20px;
}

table {
    width: 100%;
    border-collapse: collapse;
}

th, td {
    border: 1px solid #ddd;
    padding: 10px;
    text-align: center;
}

th {
    background-color: #f8f9fa;
}

.dress-thumb {
    width: 50px;
    height: 50px;
    object-fit: cover;
    border-radius: 5px;
}

.delete-btn {
    background-color: #e74c3c;
    padding: 5px 10px;
    width: auto;
}

/* Print Specific Styles */
@media print {
    .no-print {
        display: none !important;
    }
    body {
        background-color: white;
    }
    .card {
        box-shadow: none;
        padding: 0;
    }
}
const CORRECT_PASSWORD = "admin123";

// Initial Sample Data
let stockData = [
    { name: "Floral Dress", code: "D001", price: 1200, qty: 10, img: "https://via.placeholder.com/50" },
    { name: "Black Dress", code: "D002", price: 1500, qty: 5, img: "https://via.placeholder.com/50" }
];

function login() {
    const inputPass = document.getElementById("password").value;
    if (inputPass === CORRECT_PASSWORD) {
        document.getElementById("login-screen").style.display = "none";
        document.getElementById("dashboard").style.display = "block";
        renderTable();
    } else {
        document.getElementById("error-msg").style.display = "block";
    }
}

function logout() {
    document.getElementById("login-screen").style.display = "block";
    document.getElementById("dashboard").style.display = "none";
    document.getElementById("password").value = "";
}

function renderTable() {
    const tableBody = document.getElementById("stock-list");
    tableBody.innerHTML = "";

    stockData.forEach((item, index) => {
        const row = `
            <tr>
                <td><img src="${item.img}" class="dress-thumb" alt="Dress"></td>
                <td>${item.name}</td>
                <td>${item.code}</td>
                <td>৳${item.price}</td>
                <td>${item.qty}</td>
                <td><span style="color: ${item.qty > 0 ? 'green' : 'red'};">${item.qty > 0 ? 'In Stock' : 'Out of Stock'}</span></td>
                <td class="no-print"><button class="delete-btn" onclick="deleteDress(${index})">মুছুন</button></td>
            </tr>
        `;
        tableBody.innerHTML += row;
    });
}

function addDress(event) {
    event.preventDefault();
    
    const name = document.getElementById("dress-name").value;
    const code = document.getElementById("dress-code").value;
    const price = document.getElementById("dress-price").value;
    const qty = document.getElementById("dress-qty").value;
    const img = document.getElementById("dress-img").value;

    stockData.push({ name, code, price, qty, img });
    renderTable();

    document.getElementById("add-dress-form").reset();
}

function deleteDress(index) {
    stockData.splice(index, 1);
    renderTable();
}
