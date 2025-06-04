<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام إدارة الحسابات</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f5f5f5;
            margin: 0;
            padding: 20px;
            color: #333;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h1, h2 {
            color: #2c3e50;
            text-align: center;
        }
        .section {
            margin-bottom: 30px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        input, select {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        button {
            background-color: #3498db;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
        }
        button:hover {
            background-color: #2980b9;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
        }
        th {
            background-color: #f2f2f2;
        }
        tr:nth-child(even) {
            background-color: #f9f9f9;
        }
        .password-section {
            background-color: #fff8e1;
            padding: 10px;
            border-radius: 5px;
            margin-bottom: 15px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>نظام إدارة الحسابات</h1>
        
        <div class="section">
            <h2>إضافة حساب جديد</h2>
            <div class="password-section">
                <div class="form-group">
                    <label for="addPassword">كلمة المرور:</label>
                    <input type="password" id="addPassword" placeholder="أدخل كلمة المرور لإضافة حساب">
                </div>
            </div>
            <div class="form-group">
                <label for="name">الاسم:</label>
                <input type="text" id="name" placeholder="أدخل الاسم">
            </div>
            <div class="form-group">
                <label for="balance">الرصيد:</label>
                <input type="number" id="balance" placeholder="أدخل الرصيد">
            </div>
            <button onclick="addAccount()">إضافة حساب</button>
        </div>
        
        <div class="section">
            <h2>تعديل رصيد حساب</h2>
            <div class="password-section">
                <div class="form-group">
                    <label for="editPassword">كلمة المرور:</label>
                    <input type="password" id="editPassword" placeholder="أدخل كلمة المرور للتعديل">
                </div>
            </div>
            <div class="form-group">
                <label for="editName">اسم الحساب:</label>
                <select id="editName"></select>
            </div>
            <div class="form-group">
                <label for="newBalance">الرصيد الجديد:</label>
                <input type="number" id="newBalance" placeholder="أدخل الرصيد الجديد">
            </div>
            <button onclick="editAccount()">تعديل الرصيد</button>
        </div>
        
        <div class="section">
            <h2>الحسابات المسجلة</h2>
            <table id="accountsTable">
                <thead>
                    <tr>
                        <th>الاسم</th>
                        <th>الرصيد</th>
                    </tr>
                </thead>
                <tbody></tbody>
            </table>
        </div>
    </div>

    <script>
        // تخزين الحسابات
        let accounts = [];
        const PASSWORD = "lucifer 3mk";

        // عند تحميل الصفحة
        document.addEventListener('DOMContentLoaded', function() {
            loadAccounts();
            updateAccountsList();
        });

        // تحميل الحسابات من localStorage
        function loadAccounts() {
            const savedAccounts = localStorage.getItem('accounts');
            if (savedAccounts) {
                accounts = JSON.parse(savedAccounts);
            }
        }

        // حفظ الحسابات في localStorage
        function saveAccounts() {
            localStorage.setItem('accounts', JSON.stringify(accounts));
        }

        // تحديث قائمة الحسابات في الواجهة
        function updateAccountsList() {
            const tableBody = document.querySelector('#accountsTable tbody');
            const editSelect = document.querySelector('#editName');
            
            // تفريغ الجدول والقائمة المنسدلة
            tableBody.innerHTML = '';
            editSelect.innerHTML = '';
            
            // إضافة الحسابات إلى الجدول
            accounts.forEach(account => {
                const row = document.createElement('tr');
                
                const nameCell = document.createElement('td');
                nameCell.textContent = account.name;
                
                const balanceCell = document.createElement('td');
                balanceCell.textContent = account.balance;
                
                row.appendChild(nameCell);
                row.appendChild(balanceCell);
                tableBody.appendChild(row);
            });
            
            // إضافة الحسابات إلى القائمة المنسدلة للتعديل
            accounts.forEach(account => {
                const option = document.createElement('option');
                option.value = account.name;
                option.textContent = account.name;
                editSelect.appendChild(option);
            });
        }

        // إضافة حساب جديد
        function addAccount() {
            const passwordInput = document.querySelector('#addPassword');
            const nameInput = document.querySelector('#name');
            const balanceInput = document.querySelector('#balance');
            
            const password = passwordInput.value.trim();
            const name = nameInput.value.trim();
            const balance = balanceInput.value.trim();
            
            // التحقق من كلمة المرور
            if (password !== PASSWORD) {
                alert('كلمة المرور غير صحيحة! لا يمكن إضافة حساب.');
                return;
            }
            
            if (!name || !balance) {
                alert('الرجاء إدخال الاسم والرصيد');
                return;
            }
            
            // التحقق من عدم وجود حساب بنفس الاسم
            if (accounts.some(account => account.name === name)) {
                alert('هذا الحساب موجود بالفعل!');
                return;
            }
            
            // إضافة الحساب الجديد
            accounts.push({
                name: name,
                balance: balance
            });
            
            // حفظ وتحديث الواجهة
            saveAccounts();
            updateAccountsList();
            
            // تفريغ حقول الإدخال
            nameInput.value = '';
            balanceInput.value = '';
            passwordInput.value = '';
            
            alert('تمت إضافة الحساب بنجاح!');
        }

        // تعديل حساب موجود
        function editAccount() {
            const passwordInput = document.querySelector('#editPassword');
            const editSelect = document.querySelector('#editName');
            const newBalanceInput = document.querySelector('#newBalance');
            
            const password = passwordInput.value.trim();
            const selectedName = editSelect.value;
            const newBalance = newBalanceInput.value.trim();
            
            // التحقق من كلمة المرور
            if (password !== PASSWORD) {
                alert('كلمة المرور غير صحيحة! لا يمكن التعديل.');
                return;
            }
            
            if (!selectedName || !newBalance) {
                alert('الرجاء اختيار حساب وإدخال الرصيد الجديد');
                return;
            }
            
            // البحث عن الحساب وتعديله
            const accountIndex = accounts.findIndex(account => account.name === selectedName);
            if (accountIndex !== -1) {
                accounts[accountIndex].balance = newBalance;
                saveAccounts();
                updateAccountsList();
                
                // تفريغ حقول الإدخال
                newBalanceInput.value = '';
                passwordInput.value = '';
                
                alert('تم تعديل الرصيد بنجاح!');
            }
        }
    </script>
</body>
</html>
