<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Software Company in Nepal. IT Technology related work. IT Company, Web App Devlopment Company, Desktop Add Development Company, Nepal, Lunar I.T Solution, Lunar it solution." />
    <title>Lunar I.T. Solution - Dashboard</title>
  
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Rubik:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap" rel="stylesheet" />
  
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-9ndCyUaIbzAi2FUVXJi0CjmCapSmO7SnpJef0486qhLnuZ2cdeRhO02iuK6FUUVM" crossorigin="anonymous">
  
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" integrity="sha512-iecdLmaskl7CVkqkXNQ/ZH/XLlvWZOJyj7Yy7tcenmpD1ypASozpmT/E0iPtmFIB46ZmdtAc9eNBvH0H/ZpiBw==" crossorigin="anonymous" referrerpolicy="no-referrer" />
  
    <!-- Chart.js for analytics -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="icon" type="image/png" href="~/img/website-logo.png" />
    <script>
        // Global AJAX error handler — prevents console spam when offline
        $(document).ajaxError(function (event, xhr, settings) {
            if (xhr.status === 0 || xhr.statusText === "error") {
                console.warn("Offline or server not reachable:", settings.url);
                // Optionally show a nice toast once
                if (!window.offlineWarningShown) {
                    toastr.warning("You are offline or server is down. Some features are disabled.");
                    window.offlineWarningShown = true;
                }
            }
        });

        // Reset warning when back online
        $(window).on('online', function () {
            window.offlineWarningShown = false;
            toastr.info("Back online!");
        });
    </script>
  
    <style>
        :root {
            --primary: #4361ee;
            --secondary: #3f37c9;
            --success: #4cc9f0;
            --info: #4895ef;
            --warning: #f72585;
            --danger: #e63946;
            --light: #f8f9fa;
            --dark: #212529;
            --sidebar-width: 250px;
            --sidebar-collapsed-width: 70px;
            --header-height: 70px;
            --border-radius: 12px;
            --box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
            --transition: all 0.3s ease;
        }
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Rubik', sans-serif;
            background-color: #f5f7fb;
            color: #495057;
            overflow-x: hidden;
        }
        /* Layout */
        .dashboard-container {
            display: flex;
            min-height: 100vh;
        }
        /* Sidebar */
        .sidebar {
            width: var(--sidebar-width);
            background: linear-gradient(180deg, var(--primary) 0%, var(--secondary) 100%);
            height: 100vh;
            position: fixed;
            left: 0;
            top: 0;
            z-index: 1000;
            transition: var(--transition);
            box-shadow: var(--box-shadow);
        }
        .sidebar.collapsed {
            width: var(--sidebar-collapsed-width);
        }
        .sidebar-header {
            padding: 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            height: var(--header-height);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        .logo {
            color: white;
            font-size: 22px;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .logo i {
            font-size: 28px;
        }
        .logo-text {
            transition: opacity 0.3s ease;
        }
        .sidebar.collapsed .logo-text {
            opacity: 0;
            width: 0;
            height: 0;
            overflow: hidden;
        }
        .toggle-btn {
            color: white;
            background: rgba(255, 255, 255, 0.1);
            border: none;
            width: 36px;
            height: 36px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: var(--transition);
        }
        .toggle-btn:hover {
            background: rgba(255, 255, 255, 0.2);
        }
        .sidebar-menu {
            padding: 20px 0;
            height: calc(100vh - var(--header-height));
            overflow-y: auto;
        }
        .sidebar-menu::-webkit-scrollbar {
            width: 5px;
        }
        .sidebar-menu::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 10px;
        }
        .nav-title {
            padding: 10px 20px;
            font-size: 12px;
            text-transform: uppercase;
            color: rgba(255, 255, 255, 0.6);
            letter-spacing: 1px;
        }
        .nav-item {
            margin: 5px 15px;
            border-radius: var(--border-radius);
            overflow: hidden;
        }
        .nav-link {
            display: flex;
            align-items: center;
            padding: 12px 15px;
            color: rgba(255, 255, 255, 0.8);
            text-decoration: none;
            transition: var(--transition);
            border-radius: var(--border-radius);
        }
        .nav-link:hover, .nav-link.active {
            background: rgba(255, 255, 255, 0.1);
            color: white;
        }
        .nav-link i {
            margin-right: 15px;
            font-size: 18px;
            width: 20px;
            text-align: center;
        }
        .nav-text {
            transition: opacity 0.3s ease;
        }
        .sidebar.collapsed .nav-text {
            opacity: 0;
            width: 0;
            height: 0;
            overflow: hidden;
        }
        .sidebar.collapsed .nav-title {
            opacity: 0;
            width: 0;
            height: 0;
            overflow: hidden;
        }
        /* Main Content */
        .main-content {
            flex: 1;
            margin-left: var(--sidebar-width);
            transition: var(--transition);
        }
        .main-content.expanded {
            margin-left: var(--sidebar-collapsed-width);
        }
        /* Header */
        .header {
            height: var(--header-height);
            background: white;
            box-shadow: var(--box-shadow);
            padding: 0 25px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            position: sticky;
            top: 0;
            z-index: 999;
        }
        .menu-icon {
            font-size: 24px;
            cursor: pointer;
            color: var(--dark);
            display: none;
        }
        .search-box {
            display: flex;
            align-items: center;
            background: #f5f7fb;
            border-radius: 20px;
            padding: 8px 15px;
            width: 300px;
        }
        .search-box input {
            border: none;
            background: transparent;
            outline: none;
            padding: 0 10px;
            width: 100%;
        }
        .search-box i {
            color: #6c757d;
        }
        .header-right {
            display: flex;
            align-items: center;
            gap: 20px;
        }
        .notification-btn, .message-btn {
            position: relative;
            background: #f5f7fb;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: #6c757d;
            transition: var(--transition);
        }
        .notification-btn:hover, .message-btn:hover {
            background: var(--primary);
            color: white;
        }
        .badge {
            position: absolute;
            top: -5px;
            right: -5px;
            background: var(--danger);
            color: white;
            width: 18px;
            height: 18px;
            border-radius: 50%;
            font-size: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .user-profile {
            display: flex;
            align-items: center;
            cursor: pointer;
        }
        .user-img {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            object-fit: cover;
            margin-right: 10px;
            border: 2px solid var(--primary);
        }
        .user-info {
            display: flex;
            flex-direction: column;
        }
        .user-name {
            font-weight: 600;
            font-size: 14px;
        }
        .user-role {
            font-size: 12px;
            color: #6c757d;
        }
        /* Content Area */
        .content {
            padding: 25px;
        }
        .page-title {
            font-size: 24px;
            font-weight: 600;
            margin-bottom: 20px;
            color: var(--dark);
        }
        /* Dashboard Cards */
        .stats-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
            gap: 20px;
            margin-bottom: 25px;
        }
        .stat-card {
            background: white;
            border-radius: var(--border-radius);
            padding: 20px;
            box-shadow: var(--box-shadow);
            display: flex;
            align-items: center;
            transition: var(--transition);
        }
        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
        }
        .stat-icon {
            width: 60px;
            height: 60px;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            margin-right: 15px;
        }
        .icon-primary {
            background: rgba(67, 97, 238, 0.1);
            color: var(--primary);
        }
        .icon-success {
            background: rgba(76, 201, 240, 0.1);
            color: var(--success);
        }
        .icon-warning {
            background: rgba(247, 37, 133, 0.1);
            color: var(--warning);
        }
        .icon-danger {
            background: rgba(230, 57, 70, 0.1);
            color: var(--danger);
        }
        .stat-info h4 {
            font-size: 24px;
            font-weight: 700;
            margin-bottom: 5px;
        }
        .stat-info p {
            color: #6c757d;
            font-size: 14px;
            margin: 0;
        }
        /* Charts Section */
        .charts-row {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 20px;
            margin-bottom: 25px;
        }
        .chart-card {
            background: white;
            border-radius: var(--border-radius);
            padding: 20px;
            box-shadow: var(--box-shadow);
        }
        .chart-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        .chart-title {
            font-size: 18px;
            font-weight: 600;
        }
        /* Recent Activity */
        .activity-card {
            background: white;
            border-radius: var(--border-radius);
            padding: 20px;
            box-shadow: var(--box-shadow);
            margin-bottom: 25px;
        }
        .activity-list {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        .activity-item {
            display: flex;
            padding: 15px 0;
            border-bottom: 1px solid #f1f1f1;
        }
        .activity-item:last-child {
            border-bottom: none;
        }
        .activity-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 15px;
            font-size: 16px;
        }
        .activity-content {
            flex: 1;
        }
        .activity-content h5 {
            font-size: 14px;
            margin-bottom: 5px;
            font-weight: 600;
        }
        .activity-content p {
            font-size: 13px;
            color: #6c757d;
            margin: 0;
        }
        .activity-time {
            font-size: 12px;
            color: #6c757d;
        }
        /* Responsive Styles */
        @@media(max-width: 992px) {
            .sidebar {
                left: calc(var(--sidebar-width) * -1);
            }
            .sidebar.mobile-show {
                left: 0;
            }
          
            .main-content {
                margin-left: 0;
            }
          
            .main-content.expanded {
                margin-left: 0;
            }
          
            .menu-icon {
                display: block;
            }
          
            .charts-row {
                grid-template-columns: 1fr;
            }
        }
        @@media(max-width: 768px) {
            .search-box {
                width: 200px;
            }
          
            .user-info {
                display: none;
            }
          
            .stats-cards {
                grid-template-columns: 1fr;
            }
        }
        @@media(max-width: 576px) {
            .header {
                padding: 0 15px;
            }
          
            .search-box {
                display: none;
            }
          
            .content {
                padding: 15px;
            }
        }
        /* Custom utilities */
        .card {
            border: none;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
        }
        .btn-primary {
            background-color: var(--primary);
            border-color: var(--primary);
        }
        .btn-primary:hover {
            background-color: var(--secondary);
            border-color: var(--secondary);
        }
        .text-primary {
            color: var(--primary) !important;
        }
        .bg-primary {
            background-color: var(--primary) !important;
        }
    </style>
</head>
<body>
    @if (User.Identity != null && User.Identity.IsAuthenticated)
    {
        <div class="dashboard-container">
            <!-- Sidebar -->
            <aside class="sidebar">
                <div class="sidebar-header">
                    <div class="logo">
                        <i class="fas fa-moon"></i>
                        <h6 class="logo-text" style="margin-top:8px;">Lunar I.T Solution</h6>
                    </div>
                    <button class="toggle-btn" id="sidebar-toggle">
                        <i class="fas fa-chevron-left"></i>
                    </button>
                </div>
                <div class="sidebar-menu">
                    <div class="nav-title">Main Navigation</div>
                    <div class="nav-item">
                        <a class="nav-link active" asp-action="Index" asp-controller="UserLists">
                            <i class="fas fa-home"></i>
                            <span class="nav-text">Home</span>
                        </a>
                    </div>
                    <div class="nav-item">
                        <a class="nav-link" asp-action="Index" asp-controller="Teams">
                            <i class="fas fa-users"></i>
                            <span class="nav-text">Team</span>
                        </a>
                    </div>
                    <div class="nav-item">
                        <a class="nav-link" asp-action="Index" asp-controller="ContactForms">
                            <i class="fas fa-envelope"></i>
                            <span class="nav-text">Contact Messages</span>
                        </a>
                    </div>
                    <div class="nav-item">
                        <a class="nav-link" asp-action="Index" asp-controller="Projects">
                            <i class="fas fa-briefcase"></i>
                            <span class="nav-text">Projects Complete</span>
                        </a>
                    </div>
                    <div class="nav-item">
                        <a class="nav-link" asp-action="Index" asp-controller="Products">
                            <i class="fas fa-box"></i>
                            <span class="nav-text">Products</span>
                        </a>
                    </div>
                    <div class="nav-item">
                        <a class="nav-link" asp-action="Index" asp-controller="ProductBuyRequests">
                            <i class="fas fa-shopping-cart"></i>
                            <span class="nav-text">Purchase Request</span>
                        </a>
                    </div>
                    <div class="nav-item">
                        <a class="nav-link" asp-action="Index" asp-controller="Clients">
                            <i class="fas fa-user-tie"></i>
                            <span class="nav-text">Clients</span>
                        </a>
                    </div>
                    <!-- FIXED: Testimonials menu item -->
                    <div class="nav-item">
                        <a class="nav-link" asp-controller="Testimonials" asp-action="Index">
                            <i class="fas fa-quote-left"></i>
                            <span class="nav-text">Testimonials</span>
                        </a>
                    </div>
                    <div class="nav-item">
                        <a class="nav-link" asp-controller="Galleries" asp-action="Index">
                            <i class="fas fa-images"></i>
                            <span class="nav-text">Gallery</span>
                        </a>
                    </div>
                    <div class="nav-item">
                        <a class="nav-link" asp-controller="CustomerProjects" asp-action="Index">
                            <i class="fas fa-project-diagram"></i>
                            <span class="nav-text">Customer Projects</span>
                        </a>
                    </div>
                    <div class="nav-title">Account</div>
                    <div class="nav-item">
                        <a class="nav-link" asp-action="ChangePassword" asp-controller="UserLists">
                            <i class="fas fa-key"></i>
                            <span class="nav-text">Change Password</span>
                        </a>
                    </div>
                    <div class="nav-item">
                        <a class="nav-link" asp-action="Logout" asp-controller="Account">
                            <i class="fas fa-sign-out-alt"></i>
                            <span class="nav-text">Logout</span>
                        </a>
                    </div>
                </div>
            </aside>
            <!-- Main Content -->
            <div class="main-content">
                <!-- Header -->
                <!-- Header -->
                <header class="header d-flex align-items-center px-3">
                    <!-- Mobile menu icon -->
                    <div class="menu-icon" id="mobile-menu-toggle">
                        <i class="fas fa-bars"></i>
                    </div>

                    <!-- Right side items -->
                    <div class="header-right d-flex align-items-center ms-auto gap-3">

                        <!-- ---------- ENVELOPE DROPDOWN ---------- -->
                        <div class="dropdown position-relative">
                            <button class="notification-btn message-btn position-relative" id="msgDropdownBtn"
                                    data-bs-toggle="dropdown" aria-expanded="false">
                                <i class="far fa-envelope"></i>
                                <span class="badge bg-danger position-absolute top-0 start-100 translate-middle p-1"
                                      id="unreadBadge">0</span>
                            </button>

                            <!-- Dropdown menu (Bootstrap 5) -->
                            <ul class="dropdown-menu dropdown-menu-end p-0 shadow-lg"
                                aria-labelledby="msgDropdownBtn"
                                style="width:320px; max-height:70vh; overflow-y:auto;">
                                <li class="dropdown-header bg-primary text-white p-2">
                                    <i class="fas fa-envelope me-1"></i> Unread Messages
                                </li>

                                <!-- Wrapper for dynamic items -->
                                <li id="unreadListWrapper">
                                    <ul class="list-unstyled mb-0" id="unreadList">
                                        <!-- JS fills <li> elements here -->
                                    </ul>
                                </li>

                                <li class="dropdown-footer text-center p-2 bg-light">
                                    <a asp-action="Index" asp-controller="ContactForms"
                                       class="text-decoration-none small text-primary">
                                        View All Messages to
                                    </a>
                                </li>
                            </ul>
                        </div>
                        <!-- ---------- END ENVELOPE ---------- -->
                        <!-- User profile (keep your existing code) -->
                        <div class="user-profile dropdown d-flex align-items-center">
                            <img src="https://ui-avatars.com/api/?name=@(User.FindFirst("UserName")?.Value)&background=4361ee&color=fff"
                                 alt="User" class="rounded-circle me-2" style="width:38px; height:38px;">
                            <div class="user-info d-flex flex-column">
                                <span class="user-name fw-semibold">@(User.FindFirst("UserName")?.Value)</span>
                                <span class="user-role text-muted small">Administrator</span>
                            </div>
                        </div>
                    </div>
                </header>

                <!-- Content Area -->
                <div class="content">
                    @RenderBody()
                </div>
            </div>
        </div>
        <!-- Modal for Create Testimonial -->
        <div class="modal fade" id="Create-Modal-testi" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg" id="modal-create-testi-1">
            </div>
        </div>
    }

    <!-- 1. jQuery FIRST -->
    <script src="https://code.jquery.com/jquery-3.7.1.min.js"
            integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo="
            crossorigin="anonymous"></script>

    <!-- 2. Bootstrap Bundle (includes Popper) -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>

    <!-- 3. Unobtrusive Ajax (for data-ajax="true") -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-ajax-unobtrusive/3.2.6/jquery.unobtrusive-ajax.min.js"></script>

    <!-- 4. Your global admin scripts (now $ is defined) -->
    <script src="~/js/contact-messages.js" defer></script>

  

    <script>
        $(document).ready(function () {
            loadUserList();
        });
        function loadUserList() {
            $('#spin').show();
            $.ajax({
                url: '@Url.Action("UserList", "UserLists")',
                type: 'GET',
                success: function (data) {
                    $('#userList').html(data);
                    $('#spin').hide();
                    if ($.fn.DataTable.isDataTable('#data')) {
                        $('#data').DataTable().destroy();
                    }
                    $('#data').DataTable({
                        pageLength: 10,
                        responsive: true,
                        language: { emptyTable: "No users found" },
                        columnDefs: [ { orderable: false, targets: -1 } ]
                    });
                },
                error: function () {
                    $('#spin').hide();
                    $('#userList').html('<div class="alert alert-danger">Failed to load user list. Please try again.</div>');
                }
            });
        }
        function refreshUserList() { loadUserList(); }
        function EditPage(xhr) {
            document.getElementById('modal-edit').innerHTML = xhr.responseText;
        }
        function EditPagePostData(xhr) {
            if (xhr.responseText === "success") {
                alert("Updated Successfully");
                refreshUserList();
                $('.btn-close').click();
            } else {
                document.getElementById('modal-edit').innerHTML = xhr.responseText;
            }
        }
        function updateDStatus(xhr) {
            alert(xhr.responseText === "Success" ? "User De-Activated Successfully" : "User De-Activation Failed");
            if (xhr.responseText === "Success") refreshUserList();
        }
        function updateAStatus(xhr) {
            alert(xhr.responseText === "Success" ? "User Activated Successfully" : "User Activation Failed");
            if (xhr.responseText === "Success") refreshUserList();
        }
        function PostUserData(xhr) {
            if (xhr.responseText === "success") {
                alert("User Added Successfully");
                refreshUserList();
                $('.myinput').val('');
            } else if (xhr.responseText === "Unique") {
                $('#error').html('<span class="text-danger">Choose a unique username. This name is already taken, e.g., User12</span>');
            } else {
                $('#error').html('<span class="text-danger">Failed to add user</span>');
            }
        }
    </script>
    <script src="~/js/contact-messages.js" defer></script>
    <!-- THIS IS THE KEY FIX -->
    @RenderSection("Scripts", required: false)
</body>
</html>@model LunarItSolution.Models.Product
<form asp-action="Create" enctype="multipart/form-data" data-ajax="true" data-ajax-method="post" data-ajax-success="onProductSuccess">
    <div class="modal-content">
        <div class="modal-header">
            <h5 class="modal-title">Add New Product</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
        </div>
        <div class="modal-body">
            <!-- your existing inputs -->
            <div class="mb-3"><label>Product Name</label><input asp-for="ProductName" class="form-control" required /></div>
            <div class="mb-3"><label>Information</label><textarea asp-for="ProductInformation" class="form-control" rows="4"></textarea></div>
            <div class="mb-3"><label>Image</label><input asp-for="FileUpload" type="file" class="form-control" accept="image/*" required /></div>
            <div class="mb-3"><label>Current Price</label><input asp-for="PcurrentPrice" type="number" step="0.01" class="form-control" /></div>
            <div class="mb-3"><label>Original Price</label><input asp-for="PoriginalPrice" type="number" step="0.01" class="form-control" /></div>
            <div class="mb-3"><label>YouTube URL</label><input asp-for="ProductVideo" class="form-control" placeholder="https://youtube.com/watch?v=..." /></div>
        </div>
        <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            <button type="submit" class="btn btn-primary">Save Product</button>
        </div>
    </div>
</form>@model IEnumerable<LunarItSolution.Models.Product>

@if (!Model.Any())
{
    <div class="text-center py-5">
        <img src="https://via.placeholder.com/150?text=No+Products" class="mb-3 opacity-50">
        <h5 class="text-muted">No products yet</h5>
        <p class="text-muted">Click "Add New Product" to get started!</p>
    </div>
}
else
{
    <div class="table-responsive">
        <table class="table table-hover mb-0">
            <thead class="table-light">
                <tr>
                    <th>#</th>
                    <th>Image</th>
                    <th>Product Name</th>
                    <th>Price</th>
                    <th>Features</th>
                    <th width="120" class="text-center">Actions</th>
                </tr>
            </thead>
            <tbody>
                @{
                    int i = 1;
                }
                @foreach (var p in Model)
                {
                    <tr>
                        <td>@i</td>
                        <td>
                            @if (!string.IsNullOrEmpty(p.ProductImage))
                            {
                                <img src="~/ProductImage/@p.ProductImage" class="rounded" width="60" height="60" style="object-fit:cover;" />
                            }
                            else
                            {
                                <div class="bg-light rounded d-flex align-items-center justify-content-center" style="width:60px;height:60px;">
                                    <i class="fas fa-image text-muted"></i>
                                </div>
                            }
                        </td>
                        <td>
                            <strong>@p.ProductName</strong>
                            <small class="text-muted d-block">@(p.ProductInformation?.Length > 60 ? p.ProductInformation.Substring(0, 60) + "..." : p.ProductInformation)</small>
                        </td>
                        <td>
                            <strong class="text-success">Rs. @p.PcurrentPrice</strong>
                            @if (p.PoriginalPrice > p.PcurrentPrice)
                            {
                                <del class="text-muted small">Rs. @p.PoriginalPrice</del>
                            }
                        </td>
                        <td>
                            <button type="button" class="btn btn-sm btn-outline-success add-feature mb-1"
                                    data-url="@Url.Action("Create", "ProductFeatures", new { productId = p.Pid })">
                                Add Feature
                            </button>
                            @if (p.ProductFeatures?.Any() == true)
                            {
                                <div class="mt-2 small">
                                    @foreach (var f in p.ProductFeatures.Take(3))
                                    {
                                        <span class="badge bg-light text-dark me-1">
                                            @f.Features
                                            <button type="button" class="btn btn-xs btn-link text-decoration-none edit-feature"
                                                    data-url="@Url.Action("Edit", "ProductFeatures", new { id = f.FeatureId })">
                                                Edit
                                            </button>
                                        </span>
                                    }
                                    @if (p.ProductFeatures.Count > 3)
                                    {
                                        <span class="text-muted">+@(p.ProductFeatures.Count - 3) more</span>
                                    }
                                </div>
                            }
                        </td>
                        <td class="text-center">
                            <button type="button" title="Edit" class="btn btn-warning btn-sm edit-product"
                                    data-url="@Url.Action("Edit", "Products", new { id = p.Pid })">
                                Edit
                            </button>
                            <button type="button" title="Delete" class="btn btn-danger btn-sm delete-product"
                                    data-url="@Url.Action("Delete", "Products", new { id = p.Pid })">
                                Delete
                            </button>
                        </td>
                    </tr>
                    i++;
                }
            </tbody>
        </table>
    </div>
}why create not wokring
