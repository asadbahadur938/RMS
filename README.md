<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BMS Pro | Branch Management System</title>
    <!-- Google Fonts & Font Awesome -->
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root {
            --primary: #000000;
            --accent: #2563eb;
            --success: #10b981;
            --warning: #f59e0b;
            --danger: #ef4444;
            --bg: #ffffff;
            --sidebar-bg: #f8fafc;
            --card-bg: #ffffff;
            --text-main: #1e293b;
            --text-muted: #64748b;
            --border: #e2e8f0;
            --glass: rgba(255, 255, 255, 0.7);
            --shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Outfit', sans-serif;
            transition: all 0.2s ease;
        }

        body {
            background-color: var(--bg);
            color: var(--text-main);
            overflow-x: hidden;
        }

        /* --- Animations --- */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* --- Login Screen --- */
        #login-screen {
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
        }

        .login-card {
            background: var(--glass);
            backdrop-filter: blur(20px);
            padding: 3rem;
            border-radius: 2rem;
            width: 100%;
            max-width: 450px;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.1);
            border: 1px solid white;
            text-align: center;
            animation: fadeIn 0.8s ease-out;
        }

        .login-logo {
            font-size: 3rem;
            font-weight: 800;
            margin-bottom: 1rem;
            letter-spacing: -2px;
        }

        .input-group {
            margin-bottom: 1.5rem;
            text-align: left;
        }

        .input-group label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
            color: var(--text-muted);
        }

        .input-group input {
            width: 100%;
            padding: 1rem;
            border-radius: 1rem;
            border: 1px solid var(--border);
            outline: none;
        }

        .input-group input:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 4px rgba(0,0,0,0.05);
        }

        .btn {
            width: 100%;
            padding: 1rem;
            border-radius: 1rem;
            border: none;
            background: var(--primary);
            color: white;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.2);
        }

        /* --- Main Layout --- */
        #main-app {
            display: none;
            min-height: 100vh;
        }

        .sidebar {
            width: 280px;
            background: var(--sidebar-bg);
            border-right: 1px solid var(--border);
            height: 100vh;
            position: fixed;
            padding: 2rem;
            display: flex;
            flex-direction: column;
        }

        .main-content {
            margin-left: 280px;
            padding: 2.5rem;
            width: calc(100% - 280px);
        }

        .nav-links {
            list-style: none;
            margin-top: 3rem;
        }

        .nav-links li {
            margin-bottom: 0.5rem;
        }

        .nav-links a {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 1rem;
            text-decoration: none;
            color: var(--text-muted);
            border-radius: 1rem;
            font-weight: 600;
        }

        .nav-links a.active, .nav-links a:hover {
            background: var(--primary);
            color: white;
        }

        /* --- Dashboard UI --- */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 2rem;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
        }

        .stat-card {
            background: var(--card-bg);
            padding: 2rem;
            border-radius: 1.5rem;
            border: 1px solid var(--border);
            box-shadow: var(--shadow);
            position: relative;
            overflow: hidden;
        }

        .stat-card::after {
            content: '';
            position: absolute;
            top: 0; right: 0; width: 5px; height: 100%;
            background: var(--accent);
        }

        .stat-val {
            font-size: 2.5rem;
            font-weight: 800;
            display: block;
        }

        .stat-label {
            color: var(--text-muted);
            font-weight: 600;
            text-transform: uppercase;
            font-size: 0.8rem;
        }

        .data-table-container {
            background: white;
            border-radius: 1.5rem;
            border: 1px solid var(--border);
            box-shadow: var(--shadow);
            overflow: hidden;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        th {
            background: #f8fafc;
            padding: 1.2rem;
            text-align: left;
            font-weight: 600;
            color: var(--text-muted);
            border-bottom: 1px solid var(--border);
        }

        td {
            padding: 1.2rem;
            border-bottom: 1px solid var(--border);
        }

        .badge {
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 0.75rem;
            font-weight: 700;
            text-transform: uppercase;
        }

        .badge-pending { background: #fef3c7; color: #d97706; }
        .badge-approved { background: #dcfce7; color: #16a34a; }
        .badge-rejected { background: #fee2e2; color: #dc2626; }

        /* --- Modals --- */
        .modal {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.5);
            backdrop-filter: blur(5px);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background: white;
            padding: 2.5rem;
            border-radius: 2rem;
            width: 100%;
            max-width: 600px;
            animation: fadeIn 0.3s ease-out;
        }

        /* Responsive */
        @media (max-width: 900px) {
            .sidebar { width: 80px; padding: 1rem; }
            .sidebar span { display: none; }
            .main-content { margin-left: 80px; width: calc(100% - 80px); }
        }
    </style>
</head>
<body>

    <!-- LOGIN SCREEN -->
    <div id="login-screen">
        <div class="login-card">
            <div class="login-logo">BMS<span style="color:var(--accent)">.</span></div>
            <p style="color: var(--text-muted); margin-bottom: 2rem;">Branch Management System v2.0</p>
            
            <div id="login-error" style="color: var(--danger); margin-bottom: 1rem; display: none;">Invalid credentials!</div>
            
            <div class="input-group">
                <label>Username</label>
                <input type="text" id="username" placeholder="Enter username (superadmin/admin/user)">
            </div>
            
            <div class="input-group">
                <label>Password</label>
                <input type="password" id="password" placeholder="Try: password">
            </div>
            
            <button class="btn" onclick="handleLogin()">
                Login Securely <i class="fas fa-arrow-right"></i>
            </button>
            
            <p style="margin-top: 2rem; font-size: 0.8rem; color: var(--text-muted);">
                GitHub Pages Static Edition
            </p>
        </div>
    </div>

    <!-- MAIN APP -->
    <div id="main-app">
        <div class="sidebar">
            <div class="login-logo" style="font-size: 2rem;">BMS<span style="color:var(--accent)">.</span></div>
            
            <ul class="nav-links">
                <li><a href="#" class="active" onclick="showView('dashboard')"><i class="fas fa-th-large"></i> <span>Dashboard</span></a></li>
                <li><a href="#" onclick="showView('requests')"><i class="fas fa-clipboard-list"></i> <span>All Requests</span></a></li>
                <li id="nav-users" style="display: none;"><a href="#" onclick="showView('users')"><i class="fas fa-users"></i> <span>Users</span></a></li>
                <li><a href="#" onclick="showView('settings')"><i class="fas fa-cog"></i> <span>Settings</span></a></li>
            </ul>

            <div style="margin-top: auto;">
                <div style="display: flex; align-items: center; gap: 10px; padding: 1rem; background: #fff; border-radius: 1rem; border: 1px solid var(--border);">
                    <div id="user-initials" style="width: 40px; height: 40px; border-radius: 50%; background: var(--primary); color: white; display: flex; align-items: center; justify-content: center; font-weight: 800;">U</div>
                    <div style="overflow: hidden;">
                        <div id="current-user-name" style="font-weight: 600; font-size: 0.9rem;">User Name</div>
                        <div id="current-user-role" style="font-size: 0.7rem; color: var(--text-muted);">Role</div>
                    </div>
                </div>
                <button class="btn" style="background: transparent; color: var(--danger); border: 1px solid var(--danger); margin-top: 1rem;" onclick="logout()">
                    <i class="fas fa-sign-out-alt"></i> <span>Logout</span>
                </button>
            </div>
        </div>

        <div class="main-content">
            <!-- Dynamic View Container -->
            <div id="view-container">
                <!-- Dashboard View -->
                <div id="view-dashboard" class="view">
                    <div class="header">
                        <h1>Executive Overview</h1>
                        <button class="btn" style="width: auto; padding: 0.7rem 1.5rem;" onclick="openModal('requestModal')">
                            <i class="fas fa-plus"></i> New Request
                        </button>
                    </div>

                    <div class="stats-grid">
                        <div class="stat-card">
                            <span class="stat-label">Total Requests</span>
                            <span class="stat-val" id="stat-total">0</span>
                        </div>
                        <div class="stat-card" style="border-right-color: var(--warning);">
                            <span class="stat-label">Pending Approval</span>
                            <span class="stat-val" id="stat-pending">0</span>
                        </div>
                        <div class="stat-card" style="border-right-color: var(--success);">
                            <span class="stat-label">Resolved</span>
                            <span class="stat-val" id="stat-resolved">0</span>
                        </div>
                    </div>

                    <div class="data-table-container">
                        <div style="padding: 1.5rem; font-weight: 600; border-bottom: 1px solid var(--border);">Recent Activity</div>
                        <table id="requests-table">
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>Branch</th>
                                    <th>Item/Parts</th>
                                    <th>Status</th>
                                    <th>Date</th>
                                    <th>Action</th>
                                </tr>
                            </thead>
                            <tbody id="requests-body">
                                <!-- Data injected by JS -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODALS -->
    <div id="requestModal" class="modal">
        <div class="modal-content">
            <h2>Create New Request</h2>
            <p style="color: var(--text-muted); margin-bottom: 2rem;">Details will be sent to admin for approval.</p>
            
            <div class="input-group">
                <label>Branch Name</label>
                <input type="text" id="req-branch" placeholder="e.g. Restaurant-Do Darya">
            </div>
            <div class="input-group">
                <label>Parts/Description</label>
                <textarea id="req-desc" style="width: 100%; padding: 1rem; border-radius: 1rem; border: 1px solid var(--border); min-height: 100px;"></textarea>
            </div>
            
            <div style="display: flex; gap: 1rem; margin-top: 2rem;">
                <button class="btn" style="background: var(--text-muted);" onclick="closeModal('requestModal')">Cancel</button>
                <button class="btn" onclick="saveRequest()">Submit Request</button>
            </div>
        </div>
    </div>

    <!-- SCRIPTS -->
    <script>
        // --- 1. MOCK DATABASE INITIALIZATION ---
        const initData = () => {
            if (!localStorage.getItem('bms_users')) {
                const users = [
                    { id: 1, username: 'superadmin', password: 'password', role: 'superadmin', name: 'Asad Bahadur' },
                    { id: 2, username: 'admin', password: 'password', role: 'admin', name: 'Branch Admin' },
                    { id: 3, username: 'user', password: 'password', role: 'user', name: 'Front Desk User' }
                ];
                localStorage.setItem('bms_users', JSON.stringify(users));
            }
            if (!localStorage.getItem('bms_requests')) {
                const requests = [
                    { id: 101, branch: 'Restaurtant-Do Darya', desc: 'New UPS Battery required', status: 'pending', date: '2026-04-24', requestor: 'user' },
                    { id: 102, branch: 'Fried-Bahria Town', desc: 'Keyboard replacement', status: 'approved', date: '2026-04-23', requestor: 'user' }
                ];
                localStorage.setItem('bms_requests', JSON.stringify(requests));
            }
        };

        // --- 2. AUTHENTICATION ---
        let currentUser = JSON.parse(sessionStorage.getItem('bms_session')) || null;

        const handleLogin = () => {
            const u = document.getElementById('username').value;
            const p = document.getElementById('password').value;
            const users = JSON.parse(localStorage.getItem('bms_users'));

            const user = users.find(x => x.username === u && x.password === p);

            if (user) {
                sessionStorage.setItem('bms_session', JSON.stringify(user));
                currentUser = user;
                launchApp();
            } else {
                document.getElementById('login-error').style.display = 'block';
            }
        };

        const launchApp = () => {
            document.getElementById('login-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'flex';
            
            // UI Setup
            document.getElementById('current-user-name').innerText = currentUser.name;
            document.getElementById('current-user-role').innerText = currentUser.role.toUpperCase();
            document.getElementById('user-initials').innerText = currentUser.name.charAt(0);
            
            if(currentUser.role === 'superadmin') document.getElementById('nav-users').style.display = 'block';
            
            refreshDashboard();
        };

        const logout = () => {
            sessionStorage.removeItem('bms_session');
            location.reload();
        };

        // --- 3. DASHBOARD LOGIC ---
        const refreshDashboard = () => {
            const requests = JSON.parse(localStorage.getItem('bms_requests'));
            const tbody = document.getElementById('requests-body');
            tbody.innerHTML = '';

            // Filter for normal users
            const displayData = currentUser.role === 'user' ? requests.filter(r => r.requestor === currentUser.username) : requests;

            displayData.forEach(req => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td style="font-weight:800">#${req.id}</td>
                    <td>${req.branch}</td>
                    <td>${req.desc}</td>
                    <td><span class="badge badge-${req.status}">${req.status}</span></td>
                    <td>${req.date}</td>
                    <td>
                        ${(currentUser.role !== 'user' && req.status === 'pending') ? 
                          `<button onclick="updateStatus(${req.id}, 'approved')" style="color:var(--success); border:none; background:none; cursor:pointer;"><i class="fas fa-check-circle"></i></button>
                           <button onclick="updateStatus(${req.id}, 'rejected')" style="color:var(--danger); border:none; background:none; cursor:pointer;"><i class="fas fa-times-circle"></i></button>` : 
                          '--'}
                    </td>
                `;
                tbody.appendChild(tr);
            });

            // Update Stats
            document.getElementById('stat-total').innerText = displayData.length;
            document.getElementById('stat-pending').innerText = displayData.filter(x => x.status === 'pending').length;
            document.getElementById('stat-resolved').innerText = displayData.filter(x => x.status === 'approved').length;
        };

        const saveRequest = () => {
            const branch = document.getElementById('req-branch').value;
            const desc = document.getElementById('req-desc').value;
            
            if(!branch || !desc) return alert("Fill all fields");

            const requests = JSON.parse(localStorage.getItem('bms_requests'));
            const newReq = {
                id: Math.floor(1000 + Math.random() * 9000),
                branch: branch,
                desc: desc,
                status: 'pending',
                date: new Date().toISOString().split('T')[0],
                requestor: currentUser.username
            };

            requests.unshift(newReq);
            localStorage.setItem('bms_requests', JSON.stringify(requests));
            closeModal('requestModal');
            refreshDashboard();
        };

        const updateStatus = (id, newStatus) => {
            const requests = JSON.parse(localStorage.getItem('bms_requests'));
            const idx = requests.findIndex(r => r.id === id);
            requests[idx].status = newStatus;
            localStorage.setItem('bms_requests', JSON.stringify(requests));
            refreshDashboard();
        };

        // --- 4. UTILS ---
        const openModal = (id) => document.getElementById(id).style.display = 'flex';
        const closeModal = (id) => document.getElementById(id).style.display = 'none';

        // Check Session
        window.onload = () => {
            initData();
            if (currentUser) launchApp();
        };
    </script>
</body>
</html>
