<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>SIMS | نظام إدارة المخزون الذكي</title>
    
    <!-- Bootstrap 5 RTL -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.rtl.min.css" rel="stylesheet">
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700&display=swap" rel="stylesheet">
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Cairo', sans-serif;
        }

        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }

        /* شاشة التحميل */
        .loading-screen {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 9999;
            color: white;
        }
        .spinner {
            width: 50px;
            height: 50px;
            border: 4px solid rgba(255,255,255,0.3);
            border-top-color: white;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* تسجيل الدخول */
        .login-container {
            max-width: 500px;
            margin: 50px auto;
            background: white;
            padding: 30px 20px;
            border-radius: 20px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.1);
        }

        /* الهيدر */
        .mobile-header {
            background: white;
            padding: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            position: sticky;
            top: 0;
            z-index: 100;
        }
        .user-info {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .user-avatar {
            width: 45px;
            height: 45px;
            border-radius: 50%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
        }
        .btn-icon {
            background: none;
            border: none;
            font-size: 1.5rem;
            color: #667eea;
            cursor: pointer;
        }

        /* شاشة الأيقونات */
        .icons-screen {
            padding: 20px;
            min-height: calc(100vh - 70px);
            background: #f5f5f5;
        }
        .icon-card {
            background: white;
            border-radius: 15px;
            padding: 20px 10px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
        }
        .icon-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 20px rgba(102,126,234,0.2);
        }
        .icon-card i {
            font-size: 2rem;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 10px;
            display: block;
        }
        .icon-card span {
            font-size: 0.85rem;
            font-weight: 600;
            color: #333;
        }

        /* الصفحات الداخلية */
        .inner-page {
            display: none;
            padding: 20px;
            background: #f5f5f5;
            min-height: calc(100vh - 70px);
        }
        .inner-page.active-page {
            display: block;
        }
        .back-button {
            background: white;
            padding: 12px 20px;
            cursor: pointer;
            font-weight: 600;
            color: #667eea;
            border-bottom: 1px solid #eee;
            position: sticky;
            top: 70px;
            z-index: 99;
        }

        /* البطاقات */
        .stats-card {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 15px;
            border-radius: 15px;
            text-align: center;
        }
        .card {
            border-radius: 15px;
            border: none;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
            margin-bottom: 20px;
            background: white;
        }
        .card-header {
            background: white;
            border-bottom: 2px solid #f0f0f0;
            font-weight: 600;
            padding: 15px 20px;
        }

        /* الجداول */
        .table th {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            font-weight: 600;
            font-size: 0.85rem;
            padding: 10px;
        }
        .table td {
            padding: 10px;
            vertical-align: middle;
        }

        /* حالات المخزون */
        .status-critical { background-color: #ff4444; color: white; padding: 3px 10px; border-radius: 20px; font-size: 0.7rem; display: inline-block; }
        .status-medium { background-color: #ffbb33; color: white; padding: 3px 10px; border-radius: 20px; font-size: 0.7rem; display: inline-block; }
        .status-good { background-color: #00C851; color: white; padding: 3px 10px; border-radius: 20px; font-size: 0.7rem; display: inline-block; }

        /* أزرار الدفع */
        .payment-buttons { display: flex; gap: 10px; margin-bottom: 20px; }
        .payment-btn {
            flex: 1; padding: 10px; border: 2px solid #e0e0e0;
            background: white; border-radius: 12px; cursor: pointer;
            transition: all 0.3s; font-weight: 600;
        }
        .payment-btn.active {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-color: transparent; color: white;
        }
        .payment-fields {
            background: #f8f9fa; padding: 15px; border-radius: 12px;
            margin-bottom: 15px;
        }

        /* الإشعارات */
        #notificationArea {
            position: fixed; top: 80px; left: 20px; right: 20px;
            z-index: 9999; pointer-events: none;
        }
        .notification {
            background: white; padding: 12px 20px; border-radius: 10px;
            box-shadow: 0 5px 20px rgba(0,0,0,0.15); margin-bottom: 10px;
            animation: slideIn 0.3s ease; border-right: 4px solid;
            pointer-events: auto; font-size: 0.85rem;
        }
        .notification.success { border-right-color: #00C851; }
        .notification.error { border-right-color: #ff4444; }
        .notification.info { border-right-color: #667eea; }
        @keyframes slideIn {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        /* مودال */
        .modal {
            position: fixed; top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0,0,0,0.5); display: flex;
            align-items: center; justify-content: center; z-index: 10000;
        }
        .modal-content {
            background: white; border-radius: 20px; width: 90%;
            max-width: 500px; max-height: 80vh; overflow-y: auto;
        }
        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border: none; padding: 12px; font-weight: 600; border-radius: 10px;
        }
        .form-control, .form-select {
            border: 2px solid #e0e0e0; border-radius: 10px; padding: 10px 12px;
        }
        .form-control:focus, .form-select:focus {
            border-color: #667eea;
            box-shadow: 0 0 0 0.2rem rgba(102,126,234,0.25);
        }
        .product-image-preview {
            width: 100%; height: 150px; background: #f8f9fa;
            border: 2px dashed #ddd; border-radius: 15px;
            display: flex; align-items: center; justify-content: center;
            color: #999; margin-bottom: 15px;
        }
        .product-image-preview img {
            width: 100%; height: 100%; object-fit: cover; border-radius: 15px;
        }

        @media (max-width: 768px) {
            .icons-screen .row .col-6 { margin-bottom: 12px; }
            .icon-card { padding: 15px 8px; }
            .icon-card i { font-size: 1.5rem; }
            .icon-card span { font-size: 0.7rem; }
        }
    </style>
</head>
<body>

<!-- شاشة التحميل -->
<div id="loadingScreen" class="loading-screen">
    <div class="spinner"></div>
    <p class="mt-3">جاري تحميل النظام...</p>
</div>

<!-- ==================== صفحة تسجيل الدخول ==================== -->
<div id="loginPage" class="page" style="display: block;">
    <div class="container">
        <div class="login-container">
            <div class="text-center mb-4">
                <i class="fas fa-boxes fa-3x" style="color: #667eea;"></i>
                <h2 class="mt-3">SIMS</h2>
                <p class="text-muted">نظام إدارة المخزون الذكي</p>
            </div>
            
            <ul class="nav nav-tabs mb-4" id="loginTab" role="tablist">
                <li class="nav-item"><button class="nav-link active" data-bs-toggle="tab" data-bs-target="#emailLogin"><i class="fas fa-envelope"></i> بريد إلكتروني</button></li>
                <li class="nav-item"><button class="nav-link" data-bs-toggle="tab" data-bs-target="#phoneLogin"><i class="fas fa-phone"></i> رقم هاتف</button></li>
            </ul>
            
            <div class="tab-content">
                <div class="tab-pane fade show active" id="emailLogin">
                    <form id="loginEmailForm">
                        <div class="mb-3"><label>البريد الإلكتروني</label><input type="email" class="form-control" id="loginEmail" required></div>
                        <div class="mb-3"><label>كلمة المرور</label><input type="password" class="form-control" id="loginPassword" required></div>
                        <button type="submit" class="btn btn-primary w-100">تسجيل الدخول</button>
                    </form>
                </div>
                <div class="tab-pane fade" id="phoneLogin">
                    <form id="loginPhoneForm">
                        <div class="mb-3"><label>رقم الهاتف</label><input type="tel" class="form-control" id="loginPhone" required></div>
                        <div class="mb-3"><label>كلمة المرور</label><input type="password" class="form-control" id="loginPhonePassword" required></div>
                        <button type="submit" class="btn btn-primary w-100">تسجيل الدخول</button>
                    </form>
                </div>
            </div>
            
            <div class="text-center mt-3">
                <a href="#" onclick="showRegister()">تسجيل جديد</a> | 
                <a href="#" onclick="showForgotPassword()">نسيت كلمة السر؟</a>
            </div>
        </div>
    </div>
</div>

<!-- ==================== صفحة التسجيل ==================== -->
<div id="registerPage" class="page" style="display: none;">
    <div class="container">
        <div class="login-container">
            <h3 class="text-center mb-4">إنشاء حساب جديد</h3>
            <div id="registerError" class="alert alert-danger" style="display: none;"></div>
            <form id="registerForm">
                <div class="mb-3"><label>الاسم الكامل</label><input type="text" class="form-control" id="regName" required></div>
                <div class="mb-3"><label>رقم الهاتف</label><input type="tel" class="form-control" id="regPhone" required></div>
                <div class="mb-3"><label>البريد الإلكتروني</label><input type="email" class="form-control" id="regEmail" required></div>
                <div class="mb-3"><label>كلمة المرور</label><input type="password" class="form-control" id="regPassword" required></div>
                <div class="mb-3"><label>تأكيد كلمة المرور</label><input type="password" class="form-control" id="regConfirmPassword" required></div>
                <div class="mb-3">
                    <label>القطاع</label>
                    <select class="form-control" id="sectorSelect" required>
                        <option value="">اختر القطاع</option>
                        <option value="صناعي">صناعي</option><option value="زراعي">زراعي</option><option value="تجاري">تجاري</option>
                        <option value="صيدليات">صيدليات</option><option value="مصانع">مصانع</option><option value="سوبرماركت">سوبرماركت</option>
                        <option value="مكتبات">مكتبات</option><option value="ملابس">ملابس</option><option value="الكترونيات">الكترونيات</option>
                        <option value="أغذية">أغذية</option><option value="مشروبات">مشروبات</option><option value="أثاث">أثاث</option>
                    </select>
                </div>
                <div class="mb-3">
                    <label>الفرع</label>
                    <select class="form-control" id="branchSelect" required>
                        <option value="">اختر الفرع</option>
                    </select>
                </div>
                <div class="form-check mb-3">
                    <input class="form-check-input" type="checkbox" id="termsCheck" required>
                    <label>أوافق على الشروط والأحكام</label>
                </div>
                <button type="submit" class="btn btn-primary w-100">إنشاء حساب</button>
            </form>
            <div class="text-center mt-3"><a href="#" onclick="showLogin()">لديك حساب؟ تسجيل الدخول</a></div>
        </div>
    </div>
</div>

<!-- ==================== التطبيق الرئيسي ==================== -->
<div id="appPage" class="page" style="display: none;">
    <!-- الهيدر -->
    <div class="mobile-header">
        <div class="user-info">
            <div class="user-avatar" id="userAvatar"><i class="fas fa-user"></i></div>
            <div><h6 id="userNameDisplay">مرحباً بك</h6><small id="userInfoDisplay"></small></div>
        </div>
        <button class="btn-icon" onclick="logout()"><i class="fas fa-sign-out-alt"></i></button>
    </div>
    
    <!-- شاشة الأيقونات -->
    <div id="iconsScreen" class="icons-screen">
        <div class="container">
            <div class="row g-3">
                <div class="col-6 col-md-4 col-lg-3"><div class="icon-card" onclick="showPage('dashboard')"><i class="fas fa-tachometer-alt"></i><span>لوحة المعلومات</span></div></div>
                <div class="col-6 col-md-4 col-lg-3"><div class="icon-card" onclick="showPage('addProduct')"><i class="fas fa-plus-circle"></i><span>إضافة منتج</span></div></div>
                <div class="col-6 col-md-4 col-lg-3"><div class="icon-card" onclick="showPage('sellProduct')"><i class="fas fa-shopping-cart"></i><span>بيع منتجات</span></div></div>
                <div class="col-6 col-md-4 col-lg-3"><div class="icon-card" onclick="showPage('forecast')"><i class="fas fa-chart-line"></i><span>التنبؤ بالمشتريات</span></div></div>
                <div class="col-6 col-md-4 col-lg-3"><div class="icon-card" onclick="showPage('economicOrder')"><i class="fas fa-calculator"></i><span>الكمية الاقتصادية</span></div></div>
                <div class="col-6 col-md-4 col-lg-3"><div class="icon-card" onclick="showPage('inventory')"><i class="fas fa-clipboard-list"></i><span>جرد حساب</span></div></div>
                <div class="col-6 col-md-4 col-lg-3"><div class="icon-card" onclick="showPage('transactions')"><i class="fas fa-history"></i><span>تسجيل المعاملات</span></div></div>
                <div class="col-6 col-md-4 col-lg-3"><div class="icon-card" onclick="showPage('debts')"><i class="fas fa-money-bill-wave"></i><span>الديون</span></div></div>
                <div class="col-6 col-md-4 col-lg-3"><div class="icon-card" onclick="showPage('invoices')"><i class="fas fa-file-invoice"></i><span>الفواتير</span></div></div>
                <div class="col-6 col-md-4 col-lg-3"><div class="icon-card" onclick="showPage('settings')"><i class="fas fa-cog"></i><span>الضبط</span></div></div>
            </div>
        </div>
    </div>
    
    <!-- الصفحات الداخلية -->
    <div id="pagesContainer">
        <div id="backButton" class="back-button" onclick="goBackToIcons()" style="display: none;"><i class="fas fa-arrow-right"></i> العودة للرئيسية</div>
        
        <!-- لوحة المعلومات -->
        <div id="dashboardPage" class="inner-page">
            <h4 class="mb-3"><i class="fas fa-tachometer-alt"></i> لوحة المعلومات</h4>
            <div class="row mb-4">
                <div class="col-6 col-md-3 mb-3"><div class="stats-card"><h6>المنتجات</h6><h3 id="totalProducts">0</h3></div></div>
                <div class="col-6 col-md-3 mb-3"><div class="stats-card"><h6>منخفضة</h6><h3 id="lowStockProducts">0</h3></div></div>
                <div class="col-6 col-md-3 mb-3"><div class="stats-card"><h6>مبيعات اليوم</h6><h3 id="todaySales">0</h3></div></div>
                <div class="col-6 col-md-3 mb-3"><div class="stats-card"><h6>الديون</h6><h3 id="totalDebts">0</h3></div></div>
            </div>
            <div class="card"><div class="card-header">قائمة المنتجات</div><div class="card-body"><div class="table-responsive"><table class="table"><thead><tr><th>المنتج</th><th>الكمية</th><th>السعر</th><th>الحالة</th></tr></thead><tbody id="productsTableBody"></tbody></table></div></div></div>
        </div>
        
        <!-- إضافة منتج -->
        <div id="addProductPage" class="inner-page">
            <h4 class="mb-3"><i class="fas fa-plus-circle"></i> إضافة منتج</h4>
            <div class="card"><div class="card-body">
                <div id="productImagePreview" class="product-image-preview"><i class="fas fa-image fa-3x"></i></div>
                <form id="addProductForm">
                    <div class="mb-3"><label>اسم المنتج</label><input type="text" class="form-control" id="productName" required oninput="updateProductImage()"></div>
                    <div class="mb-3"><label>الكمية</label><input type="number" class="form-control" id="productQuantity" required></div>
                    <div class="mb-3"><label>سعر الشراء</label><input type="number" class="form-control" id="purchasePrice" required></div>
                    <div class="mb-3"><label>سعر البيع</label><input type="number" class="form-control" id="sellingPrice" required></div>
                    <div class="mb-3"><label>الحد الأدنى</label><input type="number" class="form-control" id="minQuantity" required></div>
                    <div class="mb-3"><label>القطاع</label><select class="form-control" id="productSector" required onchange="updateProductBranches()"><option value="">اختر القطاع</option></select></div>
                    <div class="mb-3"><label>الفرع</label><select class="form-control" id="productBranch" required><option value="">اختر القطاع أولاً</option></select></div>
                    <button type="submit" class="btn btn-primary w-100">حفظ المنتج</button>
                </form>
            </div></div>
        </div>
        
        <!-- بيع منتجات -->
        <div id="sellProductPage" class="inner-page">
            <h4 class="mb-3"><i class="fas fa-shopping-cart"></i> بيع منتجات</h4>
            <div class="card"><div class="card-body">
                <form id="sellForm">
                    <div class="mb-3"><label>اختر المنتج</label><select class="form-control" id="productSelect" required onchange="loadSellProductDetails()"><option value="">اختر منتج...</option></select></div>
                    <div class="mb-3"><label>الكمية</label><input type="number" class="form-control" id="sellQuantity" required oninput="calculateRemainingStock()"></div>
                    
                    <div class="payment-buttons">
                        <button type="button" class="payment-btn active" data-type="cash" onclick="selectPaymentType('cash')"><i class="fas fa-money-bill-wave"></i> كاش</button>
                        <button type="button" class="payment-btn" data-type="bank" onclick="selectPaymentType('bank')"><i class="fas fa-university"></i> تحويل بنكي</button>
                        <button type="button" class="payment-btn" data-type="debt" onclick="selectPaymentType('debt')"><i class="fas fa-hand-holding-usd"></i> دين</button>
                    </div>
                    
                    <div id="cashFields" class="payment-fields"><div class="mb-2"><label>المبلغ المدفوع</label><input type="number" class="form-control" id="cashAmount"></div><div class="mb-2"><label>اسم المشتري</label><input type="text" class="form-control" id="buyerName"></div></div>
                    <div id="bankFields" class="payment-fields" style="display:none;"><div class="mb-2"><label>رقم العملية البنكية</label><input type="text" class="form-control" id="bankTransactionId"></div><div class="mb-2"><label>اسم المشتري</label><input type="text" class="form-control" id="bankBuyerName"></div></div>
                    <div id="debtFields" class="payment-fields" style="display:none;"><div class="mb-2"><label>اسم الدائن</label><input type="text" class="form-control" id="debtorName"></div><div class="mb-2"><label>تاريخ الارجاع</label><input type="date" class="form-control" id="returnDate"></div></div>
                    
                    <div class="row"><div class="col-6"><label>سعر البيع</label><input type="number" class="form-control" id="sellPrice" readonly></div><div class="col-6"><label>المتبقي</label><input type="number" class="form-control" id="remainingQuantity" readonly></div></div>
                    <button type="submit" class="btn btn-primary w-100 mt-3">إتمام البيع</button>
                </form>
            </div></div>
        </div>
        
        <!-- التنبؤ بالمشتريات -->
        <div id="forecastPage" class="inner-page"><h4><i class="fas fa-chart-line"></i> التنبؤ بالمشتريات</h4><div class="card"><div class="card-header">المنتجات المطلوب شراؤها</div><div class="card-body"><div class="table-responsive"><table class="table"><thead><tr><th>المنتج</th><th>الحالة</th><th>الكمية</th><th>الحد الأدنى</th></tr></thead><tbody id="forecastTableBody"></tbody></table></div></div></div></div>
        
        <!-- الكمية الاقتصادية -->
        <div id="economicOrderPage" class="inner-page"><h4><i class="fas fa-calculator"></i> الكمية الاقتصادية</h4><div class="row g-3"><div class="col-4"><div class="card"><div class="card-body text-center"><h6>الحد المسموح</h6><h3 id="maxLimitDisplay">0</h3></div></div></div><div class="col-4"><div class="card"><div class="card-body text-center"><h6>الحد الأدنى</h6><h3 id="minLimitDisplay">0</h3></div></div></div><div class="col-4"><div class="card"><div class="card-body text-center"><h6>ROP</h6><h3 id="ropDisplay">0</h3></div></div></div></div></div>
        
        <!-- جرد حساب -->
        <div id="inventoryPage" class="inner-page"><h4><i class="fas fa-clipboard-list"></i> جرد حساب</h4><div class="card"><div class="card-body"><div class="btn-group w-100 mb-3"><button class="btn btn-primary" onclick="showInventory('all')">كل القطاعات</button><button class="btn btn-outline-primary" onclick="showInventory('single')">سلعة محددة</button><button class="btn btn-outline-primary" onclick="showInventory('sector')">قطاع محدد</button></div><div id="inventoryDisplay"></div></div></div></div>
        
        <!-- تسجيل المعاملات -->
        <div id="transactionsPage" class="inner-page"><h4><i class="fas fa-history"></i> تسجيل المعاملات</h4><div class="card"><div class="card-body"><form id="transactionForm"><div class="mb-3"><label>نوع المعاملة</label><select class="form-control" id="transactionType"><option value="sale">بيع</option><option value="purchase">شراء</option></select></div><div class="mb-3"><label>المنتج</label><select class="form-control" id="transactionProduct"></select></div><div class="mb-3"><label>الكمية</label><input type="number" class="form-control" id="transactionQuantity"></div><div class="mb-3"><label>ملاحظات</label><textarea class="form-control" id="transactionNotes" rows="2"></textarea></div><button type="submit" class="btn btn-primary w-100">تسجيل</button></form></div></div><div class="card mt-3"><div class="card-header">سجل المعاملات</div><div class="card-body"><div class="table-responsive"><table class="table"><thead><tr><th>التاريخ</th><th>النوع</th><th>المنتج</th><th>الكمية</th></tr></thead><tbody id="transactionsTableBody"></tbody></table></div></div></div></div>
        
        <!-- الديون -->
        <div id="debtsPage" class="inner-page"><h4><i class="fas fa-money-bill-wave"></i> الديون</h4><div class="card"><div class="card-body"><div class="table-responsive"><table class="table"><thead><tr><th>التاريخ</th><th>الدائن</th><th>المنتج</th><th>المبلغ</th><th>تاريخ الارجاع</th><th>الحالة</th></tr></thead><tbody id="debtsTableBody"></tbody></table></div></div></div></div>
        
        <!-- الفواتير -->
        <div id="invoicesPage" class="inner-page"><h4><i class="fas fa-file-invoice"></i> الفواتير</h4><div class="card"><div class="card-header"><div class="btn-group w-100"><button class="btn btn-sm btn-primary" onclick="loadInvoices('daily')">يومي</button><button class="btn btn-sm btn-primary" onclick="loadInvoices('weekly')">أسبوعي</button><button class="btn btn-sm btn-primary" onclick="loadInvoices('monthly')">شهري</button></div></div><div class="card-body"><div class="table-responsive"><table class="table"><thead><tr><th>رقم الفاتورة</th><th>التاريخ</th><th>النوع</th><th>المنتج</th><th>الكمية</th><th>الإجمالي</th></tr></thead><tbody id="invoicesTableBody"></tbody></table></div></div></div></div>
        
        <!-- الضبط -->
        <div id="settingsPage" class="inner-page"><h4><i class="fas fa-cog"></i> الضبط</h4><div class="card"><div class="card-body"><form id="changePasswordForm"><h6>تغيير كلمة المرور</h6><div class="mb-3"><label>كلمة المرور الحالية</label><input type="password" class="form-control" id="currentPassword" required></div><div class="mb-3"><label>كلمة المرور الجديدة</label><input type="password" class="form-control" id="newPassword" required></div><div class="mb-3"><label>تأكيد كلمة المرور</label><input type="password" class="form-control" id="confirmNewPassword" required></div><button type="submit" class="btn btn-primary w-100">تغيير كلمة المرور</button></form><hr><h6>معلومات الحساب</h6><div id="userInfoSettings"></div><hr><button class="btn btn-outline-danger w-100 mt-3" onclick="clearAllData()"><i class="fas fa-trash"></i> مسح جميع البيانات</button></div></div></div>
    </div>
</div>

<!-- مودال الإيصال -->
<div id="receiptModal" class="modal" style="display:none;"><div class="modal-content"><div class="modal-header"><h5>إيصال البيع</h5><button onclick="closeReceipt()" style="background:none; border:none; font-size:1.5rem;">&times;</button></div><div id="receiptContent" class="modal-body"></div><div class="modal-footer"><button class="btn btn-primary" onclick="printReceipt()">طباعة</button><button class="btn btn-secondary" onclick="closeReceipt()">إغلاق</button></div></div></div>

<div id="notificationArea"></div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
<script>
// ==================== البيانات والثوابت ====================
let users = [];
let currentUser = null;
let products = [];
let transactions = [];
let debts = [];
let invoices = [];
let currentPaymentType = 'cash';
let lastSaleData = null;

const sectorsList = {
    'صناعي': { branches: ['مصنع الرياض', 'مصنع جدة', 'مصنع الدمام'] },
    'زراعي': { branches: ['مزرعة القصيم', 'مزرعة الأحساء', 'مزرعة تبوك'] },
    'تجاري': { branches: ['مركز الرياض', 'مركز جدة', 'مركز الدمام'] },
    'صيدليات': { branches: ['صيدلية الرياض', 'صيدلية جدة', 'صيدلية الدمام'] },
    'مصانع': { branches: ['مصنع الشمال', 'مصنع الجنوب', 'مصنع الشرق'] },
    'سوبرماركت': { branches: ['سوبرماركت العليا', 'سوبرماركت النهضة', 'سوبرماركت المروج'] },
    'مكتبات': { branches: ['مكتبة الجامعة', 'مكتبة الثقافة', 'مكتبة المعرفة'] },
    'ملابس': { branches: ['ملابس الرجال', 'ملابس النساء', 'ملابس الأطفال'] },
    'الكترونيات': { branches: ['الكترونيات الرياض', 'الكترونيات جدة', 'الكترونيات الدمام'] },
    'أغذية': { branches: ['مواد غذائية الشرق', 'مواد غذائية الغرب', 'مواد غذائية الشمال'] },
    'مشروبات': { branches: ['مشروبات ساخنة', 'مشروبات باردة', 'عصائر طازجة'] },
    'أثاث': { branches: ['أثاث منزلي', 'أثاث مكتبي', 'أثاث حدائق'] }
};

// ==================== التهيئة ====================
function init() {
    loadFromLocalStorage();
    if (users.length === 0) {
        users = [{ id: 1, name: 'مدير النظام', email: 'admin@sims.com', phone: '0500000000', password: 'Admin@123', sector: 'تجاري', branch: 'مركز الرياض', avatar: 'https://ui-avatars.com/api/?name=مدير&background=667eea&color=fff' }];
        saveToLocalStorage();
    }
    if (products.length === 0) {
        products = [
            { id: 1, name: 'لابتوب', quantity: 50, purchasePrice: 2000, sellingPrice: 2500, minQuantity: 10, sector: 'الكترونيات', branch: 'الكترونيات الرياض', image: 'https://ui-avatars.com/api/?name=لابتوب&background=764ba2&color=fff' },
            { id: 2, name: 'قميص', quantity: 100, purchasePrice: 50, sellingPrice: 80, minQuantity: 20, sector: 'ملابس', branch: 'ملابس الرجال', image: 'https://ui-avatars.com/api/?name=قميص&background=764ba2&color=fff' },
            { id: 3, name: 'هاتف', quantity: 8, purchasePrice: 800, sellingPrice: 1200, minQuantity: 5, sector: 'الكترونيات', branch: 'الكترونيات الرياض', image: 'https://ui-avatars.com/api/?name=هاتف&background=764ba2&color=fff' }
        ];
        saveToLocalStorage();
    }
    loadSectorsToSelect();
    document.getElementById('loadingScreen').style.display = 'none';
    const savedUser = localStorage.getItem('currentUser');
    if (savedUser) { currentUser = JSON.parse(savedUser); showApp(); }
}

function loadSectorsToSelect() {
    const sectorSelect = document.getElementById('sectorSelect');
    const productSector = document.getElementById('productSector');
    if (sectorSelect) {
        sectorSelect.innerHTML = '<option value="">اختر القطاع</option>';
        Object.keys(sectorsList).forEach(s => sectorSelect.innerHTML += `<option value="${s}">${s}</option>`);
    }
    if (productSector) {
        productSector.innerHTML = '<option value="">اختر القطاع</option>';
        Object.keys(sectorsList).forEach(s => productSector.innerHTML += `<option value="${s}">${s}</option>`);
    }
}

function updateBranches(sector, targetId) {
    const branchSelect = document.getElementById(targetId);
    if (branchSelect && sector && sectorsList[sector]) {
        branchSelect.innerHTML = '<option value="">اختر الفرع</option>';
        sectorsList[sector].branches.forEach(b => branchSelect.innerHTML += `<option value="${b}">${b}</option>`);
        branchSelect.disabled = false;
    } else if (branchSelect) {
        branchSelect.innerHTML = '<option value="">اختر القطاع أولاً</option>';
        branchSelect.disabled = true;
    }
}

// ==================== تسجيل الدخول ====================
document.getElementById('loginEmailForm')?.addEventListener('submit', function(e) {
    e.preventDefault();
    const user = users.find(u => u.email === document.getElementById('loginEmail').value && u.password === document.getElementById('loginPassword').value);
    if (user) { currentUser = user; localStorage.setItem('currentUser', JSON.stringify(user)); showApp(); showNotification('مرحباً بك', 'success'); }
    else showNotification('بيانات غير صحيحة', 'error');
});
document.getElementById('loginPhoneForm')?.addEventListener('submit', function(e) {
    e.preventDefault();
    const user = users.find(u => u.phone === document.getElementById('loginPhone').value && u.password === document.getElementById('loginPhonePassword').value);
    if (user) { currentUser = user; localStorage.setItem('currentUser', JSON.stringify(user)); showApp(); showNotification('مرحباً بك', 'success'); }
    else showNotification('بيانات غير صحيحة', 'error');
});

// ==================== التسجيل ====================
document.getElementById('sectorSelect')?.addEventListener('change', function(e) { updateBranches(e.target.value, 'branchSelect'); });
document.getElementById('productSector')?.addEventListener('change', function(e) { updateBranches(e.target.value, 'productBranch'); });

document.getElementById('registerForm')?.addEventListener('submit', function(e) {
    e.preventDefault();
    const email = document.getElementById('regEmail').value, phone = document.getElementById('regPhone').value, password = document.getElementById('regPassword').value, confirm = document.getElementById('regConfirmPassword').value, sector = document.getElementById('sectorSelect').value, branch = document.getElementById('branchSelect').value;
    if (users.find(u => u.email === email)) { showNotification('البريد مسجل', 'error'); return; }
    if (users.find(u => u.phone === phone)) { showNotification('رقم الهاتف مسجل', 'error'); return; }
    if (password !== confirm) { showNotification('كلمة المرور غير متطابقة', 'error'); return; }
    if (password.length < 8 || !/[A-Z]/.test(password) || !/[a-z]/.test(password) || !/[0-9]/.test(password)) { showNotification('كلمة المرور يجب أن تحتوي على 8 أحرف، حرف كبير، حرف صغير ورقم', 'error'); return; }
    if (!sector || !branch) { showNotification('اختر القطاع والفرع', 'error'); return; }
    users.push({ id: users.length + 1, name: document.getElementById('regName').value, email, phone, password, sector, branch, avatar: `https://ui-avatars.com/api/?name=${encodeURIComponent(document.getElementById('regName').value)}&background=667eea&color=fff` });
    saveToLocalStorage();
    showNotification('تم إنشاء الحساب بنجاح', 'success');
    showLogin();
});

// ==================== إدارة المنتجات ====================
function updateProductImage() {
    const name = document.getElementById('productName').value;
    const preview = document.getElementById('productImagePreview');
    if (name) preview.innerHTML = `<img src="https://ui-avatars.com/api/?name=${encodeURIComponent(name)}&background=764ba2&color=fff&size=150">`;
    else preview.innerHTML = '<i class="fas fa-image fa-3x"></i>';
}
function updateProductBranches() { updateBranches(document.getElementById('productSector').value, 'productBranch'); }

document.getElementById('addProductForm')?.addEventListener('submit', function(e) {
    e.preventDefault();
    products.push({
        id: Date.now(), name: document.getElementById('productName').value, quantity: parseInt(document.getElementById('productQuantity').value),
        purchasePrice: parseFloat(document.getElementById('purchasePrice').value), sellingPrice: parseFloat(document.getElementById('sellingPrice').value),
        minQuantity: parseInt(document.getElementById('minQuantity').value), sector: document.getElementById('productSector').value,
        branch: document.getElementById('productBranch').value, image: `https://ui-avatars.com/api/?name=${encodeURIComponent(document.getElementById('productName').value)}&background=764ba2&color=fff`
    });
    saveToLocalStorage();
    loadProductsTable();
    loadProductSelect();
    updateStats();
    showNotification('تم إضافة المنتج', 'success');
    this.reset();
    document.getElementById('productImagePreview').innerHTML = '<i class="fas fa-image fa-3x"></i>';
});

// ==================== البيع ====================
function selectPaymentType(type) {
    currentPaymentType = type;
    document.querySelectorAll('.payment-btn').forEach(btn => { btn.classList.remove('active'); if (btn.getAttribute('data-type') === type) btn.classList.add('active'); });
    document.getElementById('cashFields').style.display = type === 'cash' ? 'block' : 'none';
    document.getElementById('bankFields').style.display = type === 'bank' ? 'block' : 'none';
    document.getElementById('debtFields').style.display = type === 'debt' ? 'block' : 'none';
}
function loadProductSelect() {
    const select = document.getElementById('productSelect');
    if (select) { select.innerHTML = '<option value="">اختر منتج...</option>'; products.forEach(p => select.innerHTML += `<option value="${p.id}">${p.name} (${p.quantity})</option>`); }
}
function loadSellProductDetails() {
    const id = document.getElementById('productSelect').value;
    const p = products.find(p => p.id == id);
    if (p) { document.getElementById('sellPrice').value = p.sellingPrice; document.getElementById('remainingQuantity').value = p.quantity; }
}
function calculateRemainingStock() {
    const id = document.getElementById('productSelect').value;
    const qty = parseInt(document.getElementById('sellQuantity').value) || 0;
    const p = products.find(p => p.id == id);
    if (p) document.getElementById('remainingQuantity').value = Math.max(0, p.quantity - qty);
}

document.getElementById('sellForm')?.addEventListener('submit', function(e) {
    e.preventDefault();
    const productId = document.getElementById('productSelect').value;
    const quantity = parseInt(document.getElementById('sellQuantity').value);
    const product = products.find(p => p.id == productId);
    if (!product) { showNotification('اختر منتجاً', 'error'); return; }
    if (quantity > product.quantity) { showNotification('الكمية غير متوفرة', 'error'); return; }
    
    const saleData = { id: Date.now(), productId: product.id, productName: product.name, quantity, price: product.sellingPrice, total: quantity * product.sellingPrice, type: currentPaymentType, date: new Date().toISOString(), buyerName: '' };
    
    if (currentPaymentType === 'cash') {
        const paid = parseFloat(document.getElementById('cashAmount').value);
        if (!paid) { showNotification('أدخل المبلغ المدفوع', 'error'); return; }
        saleData.paid = paid; saleData.buyerName = document.getElementById('buyerName').value || 'عميل';
        if (paid < saleData.total) debts.push({ id: Date.now(), debtorName: saleData.buyerName, productName: product.name, amount: saleData.total - paid, returnDate: null, createdAt: saleData.date });
    } else if (currentPaymentType === 'bank') {
        const tid = document.getElementById('bankTransactionId').value;
        if (!tid) { showNotification('أدخل رقم العملية البنكية', 'error'); return; }
        saleData.transactionId = tid; saleData.buyerName = document.getElementById('bankBuyerName').value || 'عميل';
    } else if (currentPaymentType === 'debt') {
        const debtor = document.getElementById('debtorName').value, retDate = document.getElementById('returnDate').value;
        if (!debtor) { showNotification('أدخل اسم الدائن', 'error'); return; }
        if (!retDate) { showNotification('أدخل تاريخ الارجاع', 'error'); return; }
        saleData.debtorName = debtor; saleData.returnDate = retDate;
        debts.push({ id: Date.now(), debtorName: debtor, productName: product.name, amount: saleData.total, returnDate: retDate, createdAt: saleData.date });
    }
    
    product.quantity -= quantity;
    transactions.push({ id: saleData.id, type: 'sale', productId: product.id, productName: product.name, quantity, total: saleData.total, date: saleData.date });
    invoices.push(saleData);
    saveToLocalStorage();
    loadProductsTable(); loadProductSelect(); updateStats(); loadDebts();
    showReceipt(saleData);
    showNotification('تمت عملية البيع', 'success');
    document.getElementById('sellForm').reset();
    selectPaymentType('cash');
});

function showReceipt(data) {
    lastSaleData = data;
    const now = new Date(data.date);
    let html = `<div style="text-align:center;"><h4>SIMS</h4><hr><p><strong>رقم الفاتورة:</strong> INV-${data.id}</p><p><strong>التاريخ:</strong> ${now.toLocaleDateString('ar-EG')}</p><p><strong>الوقت:</strong> ${now.toLocaleTimeString('ar-EG')}</p><p><strong>نوع الدفع:</strong> ${data.type === 'cash' ? 'كاش' : data.type === 'bank' ? 'تحويل بنكي' : 'دين'}</p><hr><p><strong>المنتج:</strong> ${data.productName}</p><p><strong>الكمية:</strong> ${data.quantity}</p><p><strong>السعر:</strong> ${data.price} ريال</p><p><strong>الإجمالي:</strong> ${data.total} ريال</p>`;
    if (data.type === 'cash') html += `<p><strong>المدفوع:</strong> ${data.paid} ريال</p><p><strong>الباقي:</strong> ${data.paid - data.total} ريال</p>`;
    if (data.type === 'bank') html += `<p><strong>رقم العملية:</strong> ${data.transactionId}</p>`;
    if (data.type === 'debt') html += `<p><strong>الدائن:</strong> ${data.debtorName}</p><p><strong>تاريخ الارجاع:</strong> ${new Date(data.returnDate).toLocaleDateString('ar-EG')}</p>`;
    html += `<hr><p><strong>المشتري:</strong> ${data.buyerName || data.debtorName || 'عميل'}</p><p>${'*'.repeat(30)}</p><p>شكراً لثقتكم</p><p>توقيع المستلم: _________________</p></div>`;
    document.getElementById('receiptContent').innerHTML = html;
    document.getElementById('receiptModal').style.display = 'flex';
}
function closeReceipt() { document.getElementById('receiptModal').style.display = 'none'; }
function printReceipt() {
    const w = window.open('', '_blank');
    w.document.write(`<html dir="rtl"><head><title>إيصال البيع</title><style>body{font-family:'Cairo';padding:20px;text-align:center;}</style></head><body>${document.getElementById('receiptContent').innerHTML}</body></html>`);
    w.document.close(); w.print();
}

// ==================== وظائف العرض ====================
function loadProductsTable() {
    const tbody = document.getElementById('productsTableBody');
    if (tbody) { tbody.innerHTML = ''; products.forEach(p => { const status = p.quantity <= p.minQuantity ? 'منخفض' : 'جيد'; tbody.innerHTML += `<tr><td>${p.name}</td><td>${p.quantity}</td><td>${p.sellingPrice}</td><td><span class="status-${p.quantity <= p.minQuantity ? 'critical' : 'good'}">${status}</span></td></tr>`; }); }
}
function loadForecast() {
    const tbody = document.getElementById('forecastTableBody');
    if (tbody) { tbody.innerHTML = ''; products.filter(p => p.quantity <= p.minQuantity).forEach(p => tbody.innerHTML += `<tr><td>${p.name}</td><td><span class="status-critical">منخفض</span></td><td>${p.quantity}</td><td>${p.minQuantity}</td></tr>`); if (tbody.innerHTML === '') tbody.innerHTML = '<tr><td colspan="4" class="text-center">لا توجد منتجات منخفضة</td></tr>'; }
}
function loadDebts() {
    const tbody = document.getElementById('debtsTableBody');
    if (tbody) { tbody.innerHTML = ''; debts.forEach(d => { const overdue = d.returnDate && new Date(d.returnDate) < new Date(); tbody.innerHTML += `<tr><td>${new Date(d.createdAt).toLocaleDateString()}</td><td>${d.debtorName}</td><td>${d.productName}</td><td>${d.amount} ريال</td><td>${d.returnDate ? new Date(d.returnDate).toLocaleDateString() : '-'}</td><td><span class="status-${overdue ? 'critical' : 'medium'}">${overdue ? 'منتهي' : 'قيد الانتظار'}</span></td></tr>`; }); if (tbody.innerHTML === '') tbody.innerHTML = '<tr><td colspan="6" class="text-center">لا توجد ديون</td></tr>'; }
}
function loadTransactions() {
    const tbody = document.getElementById('transactionsTableBody');
    if (tbody) { tbody.innerHTML = ''; [...transactions].reverse().forEach(t => tbody.innerHTML += `<tr><td>${new Date(t.date).toLocaleString()}</td><td>${t.type === 'sale' ? 'بيع' : 'شراء'}</td><td>${t.productName}</td><td>${t.quantity}</td></tr>`); }
}
function loadInvoices(period) {
    const tbody = document.getElementById('invoicesTableBody');
    if (tbody) { const now = new Date(); let filtered = invoices; if (period === 'daily') filtered = invoices.filter(t => new Date(t.date).toDateString() === now.toDateString()); if (period === 'weekly') filtered = invoices.filter(t => new Date(t.date) >= new Date(now.setDate(now.getDate() - 7))); if (period === 'monthly') filtered = invoices.filter(t => new Date(t.date) >= new Date(now.setMonth(now.getMonth() - 1))); tbody.innerHTML = ''; filtered.forEach(inv => tbody.innerHTML += `<tr><td>INV-${inv.id}</td><td>${new Date(inv.date).toLocaleDateString()}</td><td>${inv.type === 'cash' ? 'كاش' : inv.type === 'bank' ? 'بنكي' : 'دين'}</td><td>${inv.productName}</td><td>${inv.quantity}</td><td>${inv.total} ريال</td></tr>`); if (tbody.innerHTML === '') tbody.innerHTML = '<tr><td colspan="6" class="text-center">لا توجد فواتير</td></tr>'; }
}
function showInventory(type) {
    const display = document.getElementById('inventoryDisplay');
    if (type === 'all') { let html = '<table class="table"><thead><tr><th>المنتج</th><th>الكمية</th><th>السعر</th><th>الإجمالي</th></tr></thead><tbody>'; products.forEach(p => html += `<tr><td>${p.name}</td><td>${p.quantity}</td><td>${p.sellingPrice}</td><td>${p.quantity * p.sellingPrice}</td></tr>`); html += '</tbody></table>'; display.innerHTML = html; }
    else if (type === 'single') { const name = prompt('أدخل اسم المنتج:'); const p = products.find(p => p.name === name); display.innerHTML = p ? `<div class="alert alert-info">${p.name}: الكمية ${p.quantity}, السعر ${p.sellingPrice}</div>` : '<div class="alert alert-warning">منتج غير موجود</div>'; }
    else if (type === 'sector') { const sector = prompt(`القطاعات: ${[...new Set(products.map(p => p.sector))].join(', ')}`); const filtered = products.filter(p => p.sector === sector); if (filtered.length) { let html = '<table class="table"><thead><tr><th>المنتج</th><th>الكمية</th><th>الإجمالي</th></tr></thead><tbody>'; filtered.forEach(p => html += `<tr><td>${p.name}</td><td>${p.quantity}</td><td>${p.quantity * p.sellingPrice}</td></tr>`); html += '</tbody></table>'; display.innerHTML = html; } else display.innerHTML = '<div class="alert alert-warning">لا توجد منتجات</div>'; }
}
function updateStats() {
    document.getElementById('totalProducts').textContent = products.length;
    document.getElementById('lowStockProducts').textContent = products.filter(p => p.quantity <= p.minQuantity).length;
    const today = new Date().toDateString();
    document.getElementById('todaySales').textContent = transactions.filter(t => t.type === 'sale' && new Date(t.date).toDateString() === today).reduce((s, t) => s + t.total, 0);
    document.getElementById('totalDebts').textContent = debts.reduce((s, d) => s + d.amount, 0);
    const totalMin = products.reduce((s, p) => s + p.minQuantity, 0);
    const totalMax = products.reduce((s, p) => s + p.maxQuantity, 0);
    document.getElementById('minLimitDisplay').textContent = totalMin;
    document.getElementById('maxLimitDisplay').textContent = totalMax;
    document.getElementById('ropDisplay').textContent = Math.round(products.reduce((s, p) => s + (p.minQuantity * 0.5), 0) / products.length) || 0;
}

// ==================== التنقل ====================
function showPage(pageName) {
    document.getElementById('iconsScreen').style.display = 'none';
    document.querySelectorAll('.inner-page').forEach(p => p.classList.remove('active-page'));
    document.getElementById(pageName + 'Page').classList.add('active-page');
    document.getElementById('backButton').style.display = 'flex';
    if (pageName === 'dashboard') { loadProductsTable(); updateStats(); }
    if (pageName === 'sellProduct') loadProductSelect();
    if (pageName === 'forecast') loadForecast();
    if (pageName === 'transactions') loadTransactions();
    if (pageName === 'debts') loadDebts();
    if (pageName === 'invoices') loadInvoices('daily');
    if (pageName === 'economicOrder') updateStats();
}
function goBackToIcons() {
    document.getElementById('iconsScreen').style.display = 'block';
    document.querySelectorAll('.inner-page').forEach(p => p.classList.remove('active-page'));
    document.getElementById('backButton').style.display = 'none';
}
function showLogin() { document.getElementById('loginPage').style.display = 'block'; document.getElementById('registerPage').style.display = 'none'; document.getElementById('appPage').style.display = 'none'; }
function showRegister() { document.getElementById('loginPage').style.display = 'none'; document.getElementById('registerPage').style.display = 'block'; document.getElementById('appPage').style.display = 'none'; }
function showForgotPassword() { showNotification('سيتم إرسال رابط الاستعادة إلى بريدك', 'info'); }
function showApp() {
    document.getElementById('loginPage').style.display = 'none';
    document.getElementById('registerPage').style.display = 'none';
    document.getElementById('appPage').style.display = 'block';
    if (currentUser) {
        document.getElementById('userNameDisplay').textContent = currentUser.name;
        document.getElementById('userInfoDisplay').textContent = `${currentUser.sector} | ${currentUser.branch}`;
        document.getElementById('userInfoSettings').innerHTML = `<p><strong>الاسم:</strong> ${currentUser.name}</p><p><strong>البريد:</strong> ${currentUser.email}</p><p><strong>الهاتف:</strong> ${currentUser.phone}</p><p><strong>القطاع:</strong> ${currentUser.sector}</p><p><strong>الفرع:</strong> ${currentUser.branch}</p>`;
        document.getElementById('userAvatar').innerHTML = currentUser.avatar ? `<img src="${currentUser.avatar}" style="width:100%;height:100%;border-radius:50%;">` : '<i class="fas fa-user"></i>';
    }
    loadProductsTable(); loadProductSelect(); updateStats();
}
function logout() { currentUser = null; localStorage.removeItem('currentUser'); showLogin(); showNotification('تم تسجيل الخروج', 'info'); }
function clearAllData() { if (confirm('مسح جميع البيانات؟')) { localStorage.clear(); location.reload(); } }

// ==================== حفظ وتحميل ====================
function saveToLocalStorage() {
    localStorage.setItem('sims_users', JSON.stringify(users));
    localStorage.setItem('sims_products', JSON.stringify(products));
    localStorage.setItem('sims_transactions', JSON.stringify(transactions));
    localStorage.setItem('sims_debts', JSON.stringify(debts));
    localStorage.setItem('sims_invoices', JSON.stringify(invoices));
}
function loadFromLocalStorage() {
    users = JSON.parse(localStorage.getItem('sims_users') || '[]');
    products = JSON.parse(localStorage.getItem('sims_products') || '[]');
    transactions = JSON.parse(localStorage.getItem('sims_transactions') || '[]');
    debts = JSON.parse(localStorage.getItem('sims_debts') || '[]');
    invoices = JSON.parse(localStorage.getItem('sims_invoices') || '[]');
}
function showNotification(msg, type) {
    const area = document.getElementById('notificationArea');
    const n = document.createElement('div');
    n.className = `notification ${type}`;
    n.innerHTML = `<i class="fas ${type === 'success' ? 'fa-check-circle' : 'fa-exclamation-circle'}"></i> ${msg}`;
    area.appendChild(n);
    setTimeout(() => n.remove(), 3000);
}

document.getElementById('changePasswordForm')?.addEventListener('submit', function(e) {
    e.preventDefault();
    const current = document.getElementById('currentPassword').value;
    const newPass = document.getElementById('newPassword').value;
    const confirm = document.getElementById('confirmNewPassword').value;
    if (currentUser.password !== current) { showNotification('كلمة المرور الحالية غير صحيحة', 'error'); return; }
    if (newPass.length < 8 || !/[A-Z]/.test(newPass) || !/[a-z]/.test(newPass) || !/[0-9]/.test(newPass)) { showNotification('كلمة المرور الجديدة يجب أن تحتوي على 8 أحرف، حرف كبير، حرف صغير ورقم', 'error'); return; }
    if (newPass !== confirm) { showNotification('كلمة المرور غير متطابقة', 'error'); return; }
    currentUser.password = newPass;
    const userIndex = users.findIndex(u => u.id === currentUser.id);
    if (userIndex !== -1) users[userIndex].password = newPass;
    saveToLocalStorage();
    localStorage.setItem('currentUser', JSON.stringify(currentUser));
    showNotification('تم تغيير كلمة المرور', 'success');
    this.reset();
});

document.getElementById('transactionForm')?.addEventListener('submit', function(e) {
    e.preventDefault();
    const productId = document.getElementById('transactionProduct').value;
    const product = products.find(p => p.id == productId);
    if (!product) { showNotification('اختر منتجاً', 'error'); return; }
    transactions.push({ id: Date.now(), type: document.getElementById('transactionType').value, productId: product.id, productName: product.name, quantity: parseInt(document.getElementById('transactionQuantity').value), notes: document.getElementById('transactionNotes').value, date: new Date().toISOString() });
    saveToLocalStorage();
    loadTransactions();
    showNotification('تم تسجيل المعاملة', 'success');
    this.reset();
    const select = document.getElementById('transactionProduct');
    if (select) { select.innerHTML = '<option value="">اختر منتج...</option>'; products.forEach(p => select.innerHTML += `<option value="${p.id}">${p.name}</option>`); }
});

init();
</script>
</body>
</html>
