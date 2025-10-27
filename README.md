<!DOCTYPE html>
<html>
<head>
    <title>Admin Panel - Science Notes</title>
    <style>
        body { 
            font-family: Arial, sans-serif; 
            margin: 0; 
            padding: 20px; 
            background: #f0f2f5;
        }
        .container { 
            max-width: 1200px; 
            margin: 0 auto; 
            background: white; 
            padding: 20px; 
            border-radius: 10px; 
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .login-box { 
            max-width: 400px; 
            margin: 100px auto; 
            padding: 40px; 
            background: white; 
            border-radius: 10px; 
            box-shadow: 0 2px 10px rgba(0,0,0,0.1); 
            text-align: center;
        }
        input { 
            width: 100%; 
            padding: 12px; 
            margin: 10px 0; 
            border: 1px solid #ddd; 
            border-radius: 5px; 
            box-sizing: border-box;
        }
        button { 
            width: 100%; 
            padding: 12px; 
            background: #007bff; 
            color: white; 
            border: none; 
            border-radius: 5px; 
            cursor: pointer; 
            margin: 10px 0;
        }
        button:hover { background: #0056b3; }
        table { 
            width: 100%; 
            border-collapse: collapse; 
            margin: 20px 0;
        }
        th, td { 
            padding: 12px; 
            text-align: left; 
            border-bottom: 1px solid #ddd;
        }
        th { 
            background: #f8f9fa;
        }
        .nav { 
            background: #007bff; 
            padding: 15px; 
            border-radius: 5px; 
            margin-bottom: 20px;
        }
        .nav button { 
            width: auto; 
            margin: 0 10px; 
            background: white; 
            color: #007bff;
        }
        .badge { 
            padding: 4px 8px; 
            border-radius: 10px; 
            font-size: 12px; 
            font-weight: bold;
        }
        .premium { background: #28a745; color: white; }
        .basic { background: #6c757d; color: white; }
        .pending { background: #ffc107; color: black; }
        .approved { background: #28a745; color: white; }
    </style>
</head>
<body>
    <!-- LOGIN SCREEN -->
    <div id="loginScreen" class="login-box">
        <h2>🔐 Admin Login</h2>
        <p>Science Notes & Quiz Admin Panel</p>
        <input type="text" id="username" placeholder="Username" value="admin">
        <input type="password" id="password" placeholder="Password" value="admin123">
        <button onclick="login()">Login</button>
        <p><small>Use: admin / admin123</small></p>
    </div>

    <!-- ADMIN PANEL -->
    <div id="adminPanel" class="container" style="display:none">
        <h1>🏠 Science Notes Admin Panel</h1>
        
        <div class="nav">
            <button onclick="showTab('dashboard')">📊 Dashboard</button>
            <button onclick="showTab('users')">👥 Users</button>
            <button onclick="showTab('requests')">💰 Premium Requests</button>
        </div>

        <!-- DASHBOARD -->
        <div id="dashboard" class="tab">
            <h2>Dashboard Overview</h2>
            <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 20px; margin: 20px 0;">
                <div style="background: #e3f2fd; padding: 20px; border-radius: 10px; text-align: center;">
                    <h3 id="totalUsers">0</h3>
                    <p>Total Users</p>
                </div>
                <div style="background: #e8f5e8; padding: 20px; border-radius: 10px; text-align: center;">
                    <h3 id="premiumUsers">0</h3>
                    <p>Premium Users</p>
                </div>
                <div style="background: #fff3cd; padding: 20px; border-radius: 10px; text-align: center;">
                    <h3 id="pendingRequests">0</h3>
                    <p>Pending Requests</p>
                </div>
                <div style="background: #f8d7da; padding: 20px; border-radius: 10px; text-align: center;">
                    <h3 id="totalRevenue">₨0</h3>
                    <p>Total Revenue</p>
                </div>
            </div>

            <h3>Recent Users</h3>
            <table id="recentUsersTable">
                <thead>
                    <tr>
                        <th>Name</th>
                        <th>Email</th>
                        <th>Type</th>
                        <th>Joined</th>
                    </tr>
                </thead>
                <tbody></tbody>
            </table>
        </div>

        <!-- USERS -->
        <div id="users" class="tab" style="display:none">
            <h2>👥 All Users</h2>
            <input type="text" id="searchUsers" placeholder="🔍 Search users..." onkeyup="searchUsers()" style="width: 300px; margin-bottom: 20px;">
            <table id="usersTable">
                <thead>
                    <tr>
                        <th>Name</th>
                        <th>Email</th>
                        <th>Phone</th>
                        <th>Type</th>
                        <th>Joined</th>
                        <th>Progress</th>
                    </tr>
                </thead>
                <tbody></tbody>
            </table>
        </div>

        <!-- REQUESTS -->
        <div id="requests" class="tab" style="display:none">
            <h2>💰 Premium Requests</h2>
            <div>
                <button onclick="filterRequests('all')">All</button>
                <button onclick="filterRequests('pending')">Pending</button>
                <button onclick="filterRequests('approved')">Approved</button>
                <button onclick="filterRequests('rejected')">Rejected</button>
            </div>
            <table id="requestsTable">
                <thead>
                    <tr>
                        <th>User</th>
                        <th>Plan</th>
                        <th>Amount</th>
                        <th>Date</th>
                        <th>Status</th>
                        <th>Actions</th>
                    </tr>
                </thead>
                <tbody></tbody>
            </table>
        </div>
    </div>

    <script>
        // SIMPLE ADMIN PANEL - GUARANTEED TO WORK
        let currentTab = 'dashboard';

        function login() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            
            if (username === 'admin' && password === 'admin123') {
                localStorage.setItem('adminLogin', 'true');
                showAdminPanel();
                alert('✅ Login successful!');
            } else {
                alert('❌ Use: admin / admin123');
            }
        }

        function showAdminPanel() {
            document.getElementById('loginScreen').style.display = 'none';
            document.getElementById('adminPanel').style.display = 'block';
            loadDashboard();
        }

        function showTab(tabName) {
            currentTab = tabName;
            document.querySelectorAll('.tab').forEach(tab => tab.style.display = 'none');
            document.getElementById(tabName).style.display = 'block';
            
            if (tabName === 'dashboard') loadDashboard();
            if (tabName === 'users') loadUsers();
            if (tabName === 'requests') loadRequests();
        }

        function loadDashboard() {
            const users = getUsers();
            const requests = getRequests();
            
            // Update stats
            document.getElementById('totalUsers').textContent = Object.keys(users).length;
            document.getElementById('premiumUsers').textContent = Object.values(users).filter(u => u.premium?.active).length;
            document.getElementById('pendingRequests').textContent = Object.values(requests).filter(r => r.status === 'pending').length;
            
            const revenue = Object.values(requests)
                .filter(r => r.status === 'approved' || r.status === 'verified')
                .reduce((sum, r) => sum + (r.amount || 0), 0);
            document.getElementById('totalRevenue').textContent = '₨' + revenue;
            
            // Show recent users
            const recentUsers = Object.values(users).slice(-5);
            const tbody = document.querySelector('#recentUsersTable tbody');
            tbody.innerHTML = '';
            
            recentUsers.forEach(user => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${user.name}</td>
                    <td>${user.email}</td>
                    <td><span class="badge ${user.premium?.active ? 'premium' : 'basic'}">${user.premium?.active ? 'Premium' : 'Basic'}</span></td>
                    <td>${new Date(user.createdAt).toLocaleDateString()}</td>
                `;
                tbody.appendChild(row);
            });
        }

        function loadUsers() {
            const users = getUsers();
            const tbody = document.querySelector('#usersTable tbody');
            tbody.innerHTML = '';
            
            Object.values(users).forEach(user => {
                const progress = user.progress || {};
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${user.name}</td>
                    <td>${user.email}</td>
                    <td>${user.phone || 'N/A'}</td>
                    <td><span class="badge ${user.premium?.active ? 'premium' : 'basic'}">${user.premium?.active ? 'Premium' : 'Basic'}</span></td>
                    <td>${new Date(user.createdAt).toLocaleDateString()}</td>
                    <td>${progress.quizzesTaken || 0} quizzes, ${progress.averageScore || 0}% avg</td>
                `;
                tbody.appendChild(row);
            });
        }

        function loadRequests() {
            const requests = getRequests();
            const tbody = document.querySelector('#requestsTable tbody');
            tbody.innerHTML = '';
            
            Object.values(requests).forEach(request => {
                const users = getUsers();
                const user = users[request.userEmail];
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${user ? user.name : 'Unknown'}</td>
                    <td>${request.planType || 'N/A'}</td>
                    <td>₨${request.amount || '0'}</td>
                    <td>${new Date(request.timestamp).toLocaleDateString()}</td>
                    <td><span class="badge ${request.status || 'pending'}">${request.status || 'Pending'}</span></td>
                    <td>
                        ${request.status === 'pending' ? `
                            <button onclick="approveRequest('${request.transactionId}')" style="background: #28a745; margin: 2px;">Approve</button>
                            <button onclick="rejectRequest('${request.transactionId}')" style="background: #dc3545; margin: 2px;">Reject</button>
                        ` : 'No actions'}
                    </td>
                `;
                tbody.appendChild(row);
            });
        }

        function getUsers() {
            try {
                return JSON.parse(localStorage.getItem('scienceNotesUsers') || '{}');
            } catch {
                return {};
            }
        }

        function getRequests() {
            try {
                const pending = JSON.parse(localStorage.getItem('pendingTransactions') || '{}');
                const verified = JSON.parse(localStorage.getItem('verifiedTransactions') || '{}');
                return {...pending, ...verified};
            } catch {
                return {};
            }
        }

        function approveRequest(transactionId) {
            if (confirm('Approve this premium request?')) {
                const requests = getRequests();
                const request = requests[transactionId];
                
                if (request) {
                    request.status = 'approved';
                    
                    // Remove from pending
                    const pending = JSON.parse(localStorage.getItem('pendingTransactions') || '{}');
                    delete pending[transactionId];
                    localStorage.setItem('pendingTransactions', JSON.stringify(pending));
                    
                    // Add to verified
                    const verified = JSON.parse(localStorage.getItem('verifiedTransactions') || '{}');
                    verified[transactionId] = request;
                    localStorage.setItem('verifiedTransactions', JSON.stringify(verified));
                    
                    // Activate premium
                    const users = getUsers();
                    const user = users[request.userEmail];
                    if (user) {
                        user.premium = {
                            active: true,
                            plan: request.planType,
                            startDate: new Date().toISOString(),
                            expiryDate: new Date(Date.now() + 30 * 24 * 60 * 60 * 1000).toISOString(),
                            transactionId: transactionId
                        };
                        localStorage.setItem('scienceNotesUsers', JSON.stringify(users));
                    }
                    
                    alert('✅ Request approved!');
                    loadRequests();
                    loadDashboard();
                }
            }
        }

        function rejectRequest(transactionId) {
            if (confirm('Reject this premium request?')) {
                const requests = getRequests();
                const request = requests[transactionId];
                
                if (request) {
                    request.status = 'rejected';
                    
                    const pending = JSON.parse(localStorage.getItem('pendingTransactions') || '{}');
                    delete pending[transactionId];
                    localStorage.setItem('pendingTransactions', JSON.stringify(pending));
                    
                    const verified = JSON.parse(localStorage.getItem('verifiedTransactions') || '{}');
                    verified[transactionId] = request;
                    localStorage.setItem('verifiedTransactions', JSON.stringify(verified));
                    
                    alert('❌ Request rejected!');
                    loadRequests();
                }
            }
        }

        function searchUsers() {
            const search = document.getElementById('searchUsers').value.toLowerCase();
            const users = getUsers();
            const filtered = Object.values(users).filter(user => 
                user.name.toLowerCase().includes(search) || 
                user.email.toLowerCase().includes(search)
            );
            
            const tbody = document.querySelector('#usersTable tbody');
            tbody.innerHTML = '';
            
            filtered.forEach(user => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${user.name}</td>
                    <td>${user.email}</td>
                    <td>${user.phone || 'N/A'}</td>
                    <td><span class="badge ${user.premium?.active ? 'premium' : 'basic'}">${user.premium?.active ? 'Premium' : 'Basic'}</span></td>
                    <td>${new Date(user.createdAt).toLocaleDateString()}</td>
                    <td>${user.progress?.quizzesTaken || 0} quizzes</td>
                `;
                tbody.appendChild(row);
            });
        }

        function filterRequests(status) {
            const requests = getRequests();
            const filtered = status === 'all' ? Object.values(requests) : 
                            Object.values(requests).filter(req => req.status === status);
            
            const tbody = document.querySelector('#requestsTable tbody');
            tbody.innerHTML = '';
            
            filtered.forEach(request => {
                const users = getUsers();
                const user = users[request.userEmail];
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${user ? user.name : 'Unknown'}</td>
                    <td>${request.planType || 'N/A'}</td>
                    <td>₨${request.amount || '0'}</td>
                    <td>${new Date(request.timestamp).toLocaleDateString()}</td>
                    <td><span class="badge ${request.status || 'pending'}">${request.status || 'Pending'}</span></td>
                    <td>
                        ${request.status === 'pending' ? `
                            <button onclick="approveRequest('${request.transactionId}')" style="background: #28a745; margin: 2px;">Approve</button>
                            <button onclick="rejectRequest('${request.transactionId}')" style="background: #dc3545; margin: 2px;">Reject</button>
                        ` : 'No actions'}
                    </td>
                `;
                tbody.appendChild(row);
            });
        }

        // Check if already logged in
        if (localStorage.getItem('adminLogin') === 'true') {
            showAdminPanel();
        }
    </script>
</body>
</html>
