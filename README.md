<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ASB Software | Management System</title>
    <!-- Google Fonts & Font Awesome -->
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root {
            --primary: #c41e3a; /* Logo Red */
            --dark-grey: #1a1a1a; /* Logo Dark Grey */
            --accent: #ff3131;
            --success: #10b981;
            --warning: #f59e0b;
            --danger: #ef4444;
            --bg: #0f172a; /* Dark Elegant Background */
            --sidebar-bg: #1e293b;
            --card-bg: #1e293b;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --border: rgba(255, 255, 255, 0.1);
            --glass: rgba(30, 41, 59, 0.7);
            --shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
        }

        * {
            margin: 0; padding: 0; box-sizing: border-box;
            font-family: 'Outfit', sans-serif;
            transition: all 0.3s ease;
        }

        body {
            background-color: var(--bg);
            color: var(--text-main);
            overflow-x: hidden;
        }

        /* --- Custom Scrollbar --- */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: var(--bg); }
        ::-webkit-scrollbar-thumb { background: var(--primary); border-radius: 10px; }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* --- Login Screen --- */
        #login-screen {
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background: radial-gradient(circle at center, #1e293b 0%, #0f172a 100%);
        }

        .login-card {
            background: var(--glass);
            backdrop-filter: blur(20px);
            padding: 3.5rem;
            border-radius: 2.5rem;
            width: 100%;
            max-width: 480px;
            box-shadow: var(--shadow);
            border: 1px solid var(--border);
            text-align: center;
            animation: fadeIn 0.8s ease-out;
        }

        .company-logo {
            width: 150px;
            height: auto;
            margin-bottom: 1.5rem;
            filter: drop-shadow(0 0 10px var(--primary));
        }

        .brand-text {
            font-size: 2.5rem;
            font-weight: 800;
            margin-bottom: 0.5rem;
            background: linear-gradient(to right, #ffffff, var(--primary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .input-group { margin-bottom: 1.5rem; text-align: left; }
        .input-group label { display: block; margin-bottom: 0.5rem; color: var(--text-muted); font-size: 0.9rem; }
        .input-group input {
            width: 100%; padding: 1.2rem; border-radius: 1rem;
            background: rgba(15, 23, 42, 0.5);
            border: 1px solid var(--border); color: white; outline: none;
        }
        .input-group input:focus { border-color: var(--primary); box-shadow: 0 0 15px rgba(196, 30, 58, 0.3); }

        .btn {
            width: 100%; padding: 1.2rem; border-radius: 1rem; border: none;
            background: linear-gradient(45deg, var(--primary), var(--accent));
            color: white; font-weight: 700; cursor: pointer;
            display: flex; align-items: center; justify-content: center; gap: 12px;
            text-transform: uppercase; letter-spacing: 1px;
        }

        .btn:hover { transform: scale(1.02); box-shadow: 0 0 20px rgba(196, 30, 58, 0.5); }

        /* --- Dashboard Layout --- */
        #main-app { display: none; min-height: 100vh; }
        
        .sidebar {
            width: 280px; background: var(--sidebar-bg); border-right: 1px solid var(--border);
            height: 100vh; position: fixed; padding: 2.5rem; display: flex; flex-direction: column;
        }

        .main-content { margin-left: 280px; padding: 3rem; width: calc(100% - 280px); }

        .nav-links { list-style: none; margin-top: 3rem; }
        .nav-links a {
            display: flex; align-items: center; gap: 15px; padding: 1.2rem;
            text-decoration: none; color: var(--text-muted); border-radius: 1.2rem; margin-bottom: 0.5rem;
        }
        .nav-links a.active, .nav-links a:hover {
            background: rgba(196, 30, 58, 0.1); color: var(--primary);
            border-left: 4px solid var(--primary);
        }

        /* --- Dashboard UI --- */
        .header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 3rem; }
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 2rem; margin-bottom: 3rem; }
        
        .stat-card {
            background: var(--card-bg); padding: 2.5rem; border-radius: 2rem;
            border: 1px solid var(--border); position: relative; overflow: hidden;
        }
        .stat-card::before {
            content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 4px;
            background: var(--primary);
        }

        .stat-val { font-size: 3rem; font-weight: 800; color: white; line-height: 1; margin-bottom: 5px; }
        .stat-label { color: var(--text-muted); font-size: 0.9rem; font-weight: 600; text-transform: uppercase; }

        .data-table-container {
            background: var(--card-bg); border-radius: 2rem; border: 1px solid var(--border);
            overflow: hidden; box-shadow: var(--shadow);
        }

        table { width: 100%; border-collapse: collapse; }
        th { background: rgba(0,0,0,0.2); padding: 1.5rem; text-align: left; color: var(--text-muted); font-size: 0.85rem; }
        td { padding: 1.5rem; border-bottom: 1px solid var(--border); color: #e2e8f0; }

        .badge { padding: 6px 14px; border-radius: 25px; font-size: 0.75rem; font-weight: 800; text-transform: uppercase; }
        .badge-pending { background: rgba(245, 158, 11, 0.15); color: var(--warning); }
        .badge-approved { background: rgba(16, 185, 129, 0.15); color: var(--success); }
        .badge-rejected { background: rgba(239, 68, 68, 0.15); color: var(--danger); }

        /* --- Modal --- */
        .modal {
            display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.8);
            backdrop-filter: blur(10px); z-index: 1000; align-items: center; justify-content: center;
        }
        .modal-content {
            background: var(--sidebar-bg); padding: 3rem; border-radius: 2.5rem;
            width: 100%; max-width: 650px; border: 1px solid var(--border);
        }

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
            <!-- Logo Section - Apni logo file ka path yahan dein -->
            <img src="logo.png" alt="ASB Software" class="company-logo" onerror="this.src='https://via.placeholder.com/150/c41e3a/ffffff?text=ASB'">
            <div class="brand-text">ASB SOFTWARE</div>
            <p style="color: var(--text-muted); margin-bottom: 2.5rem; font-size: 0.9rem;">Enterprise Resource Management v3.0</p>
            
            <div id="login-error" style="color: var(--danger); background: rgba(239, 68, 68, 0.1); padding: 1rem; border-radius: 1rem; margin-bottom: 1.5rem; display: none;">
                <i class="fas fa-exclamation-circle"></i> Access Denied: Invalid Credentials
            </div>
            
            <div class="input-group">
                <label><i class="fas fa-user-shield"></i> System ID / Username</label>
                <input type="text" id="username" placeholder="Enter credentials">
            </div>
            
            <div class="input-group">
                <label><i class="fas fa-key"></i> Authorization Key</label>
                <input type="password" id="password" placeholder="••••••••">
            </div>
            
            <button class="btn" onclick="handleLogin()">
                Authorize Access <i class="fas fa-shield-alt"></i>
            </button>
            
            <p style="margin-top: 2rem; font-size: 0.75rem; color: var(--text-muted); letter-spacing: 1px;">
                DEVELOPED BY ASAD BAHADUR | SECURE CLOUD DEPLOYMENT
            </p>
        </div>
    </div>

    <!-- MAIN APP -->
    <div id="main-app">
        <div class="sidebar">
            <div style="text-align: center;">
                <img src="logo.png" alt="ASB" style="width: 60px; filter: drop-shadow(0 0 5px var(--primary));" onerror="this.src='https://via.placeholder.com/60/c41e3a/ffffff?text=ASB'">
                <div style="font-weight: 800; font-size: 1.2rem; margin-top: 10px; color: var(--primary);">ASB</div>
            </div>
            
            <ul class="nav-links">
                <li><a href="#" class="active" onclick="showView('dashboard')"><i class="fas fa-chart-line"></i> <span>System Metrics</span></a></li>
                <li><a href="#" onclick="showView('requests')"><i class="fas fa-project-diagram"></i> <span>Service Requests</span></a></li>
                <li id="nav-users" style="display: none;"><a href="#" onclick="showView('users')"><i class="fas fa-user-cog"></i> <span>Team Directory</span></a></li>
                <li><a href="#" onclick="showView('settings')"><i class="fas fa-sliders-h"></i> <span>Control Panel</span></a></li>
            </ul>

            <div style="margin-top: auto;">
                <div style="display: flex; align-items: center; gap: 15px; padding: 1.2rem; background: rgba(0,0,0,0.2); border-radius: 1.5rem; border: 1px solid var(--border);">
                    <div id="user-initials" style="width: 45px; height: 45px; border-radius: 50%; background: linear-gradient(45deg, var(--primary), var(--accent)); color: white; display: flex; align-items: center; justify-content: center; font-weight: 800; border: 2px solid white;">A</div>
                    <div style="overflow: hidden;">
                        <div id="current-user-name" style="font-weight: 700; font-size: 0.95rem;">ASAD</div>
                        <div id="current-user-role" style="font-size: 0.75rem; color: var(--primary); font-weight: 600;">ROOT ADMIN</div>
                    </div>
                </div>
                <button class="btn" style="background: transparent; color: var(--danger); border: 2px solid var(--danger); margin-top: 1rem;" onclick="logout()">
                    <i class="fas fa-power-off"></i> <span>Disconnect</span>
                </button>
            </div>
        </div>

        <div class="main-content">
            <div id="view-dashboard">
                <div class="header">
                    <div>
                        <h1 style="font-size: 2.5rem; font-weight: 800;">System Health</h1>
                        <p style="color: var(--text-muted);">Real-time monitoring across ASB Infrastructure</p>
                    </div>
                    <button class="btn" style="width: auto; padding: 1rem 2rem;" onclick="openModal('requestModal')">
                        <i class="fas fa-plus-circle"></i> Deploy New Request
                    </button>
                </div>

                <div class="stats-grid">
                    <div class="stat-card">
                        <span class="stat-label">Active Tickets</span>
                        <span class="stat-val" id="stat-total">0</span>
                    </div>
                    <div class="stat-card">
                        <span class="stat-label" style="color: var(--warning);">Pending Review</span>
                        <span class="stat-val" id="stat-pending" style="color: var(--warning);">0</span>
                    </div>
                    <div class="stat-card">
                        <span class="stat-label" style="color: var(--success);">Resolved Units</span>
                        <span class="stat-val" id="stat-resolved" style="color: var(--success);">0</span>
                    </div>
                </div>

                <div class="data-table-container">
                    <div style="padding: 2rem; font-weight: 800; border-bottom: 1px solid var(--border); display: flex; justify-content: space-between;">
                        <span><i class="fas fa-history"></i> SERVICE LOGS</span>
                        <span style="color: var(--primary); font-size: 0.8rem;">LIVE AUTO-SYNC</span>
                    </div>
                    <table id="requests-table">
                        <thead>
                            <tr>
                                <th># REFERENCE</th>
                                <th>INFRASTRUCTURE HUB</th>
                                <th>HARDWARE / TASK</th>
                                <th>PRIORITY STATUS</th>
                                <th>TIMESTAMP</th>
                                <th>ACTIONS</th>
                            </tr>
                        </thead>
                        <tbody id="requests-body"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL -->
    <div id="requestModal" class="modal">
        <div class="modal-content">
            <h2 style="font-size: 2rem; margin-bottom: 0.5rem;">New Maintenance Ticket</h2>
            <p style="color: var(--text-muted); margin-bottom: 2.5rem;">Fill details for ASB Engineering Team.</p>
            
            <div class="input-group">
                <label>Select Deployment Hub</label>
                <select id="req-branch" style="width: 100%; padding: 1.2rem; border-radius: 1rem; background: rgba(0,0,0,0.3); border: 1px solid var(--border); color: white;">
                    <option value="ASB Global Tech Hub">ASB Global Tech Hub</option>
                    <option value="North Region Dev Center">North Region Dev Center</option>
                    <option value="ASB Data Ops Center">ASB Data Ops Center</option>
                    <option value="Cloud Infrastructure Wing">Cloud Infrastructure Wing</option>
                    <option value="Security Operations Center">Security Operations Center</option>
                </select>
            </div>
            <div class="input-group">
                <label>Task / Hardware Description</label>
                <textarea id="req-desc" placeholder="Describe the technical requirement..." style="width: 100%; padding: 1.2rem; border-radius: 1rem; background: rgba(0,0,0,0.3); border: 1px solid var(--border); color: white; min-height: 120px;"></textarea>
            </div>
            
            <div style="display: flex; gap: 1.5rem; margin-top: 1rem;">
                <button class="btn" style="background: var(--sidebar-bg); border: 1px solid var(--border);" onclick="closeModal('requestModal')">ABORT</button>
                <button class="btn" onclick="saveRequest()">CONFIRM DEPLOYMENT</button>
            </div>
        </div>
    </div>

    <script>
        // --- DATA ENGINE ---
        const initData = () => {
            if (!localStorage.getItem('asb_users')) {
                const users = [
                    { id: 1, username: 'superadmin', password: 'password', role: 'superadmin', name: 'Asad Bahadur' },
                    { id: 2, username: 'admin', password: 'password', role: 'admin', name: 'Project Manager' },
                    { id: 3, username: 'user', password: 'password', role: 'user', name: 'Field Engineer' }
                ];
                localStorage.setItem('asb_users', JSON.stringify(users));
            }
            if (!localStorage.getItem('asb_requests')) {
                const requests = [
                    { id: 4001, branch: 'ASB Global Tech Hub', desc: 'Main Frame Server Maintenance', status: 'pending', date: '2026-04-24', requestor: 'superadmin' },
                    { id: 4002, branch: 'Cloud Infrastructure Wing', desc: 'Database Optimization', status: 'approved', date: '2026-04-23', requestor: 'admin' }
                ];
                localStorage.setItem('asb_requests', JSON.stringify(requests));
            }
        };

        let currentUser = JSON.parse(sessionStorage.getItem('asb_session')) || null;

        const handleLogin = () => {
            const u = document.getElementById('username').value;
            const p = document.getElementById('password').value;
            const users = JSON.parse(localStorage.getItem('asb_users'));
            const user = users.find(x => x.username === u && x.password === p);

            if (user) {
                sessionStorage.setItem('asb_session', JSON.stringify(user));
                currentUser = user;
                launchApp();
            } else {
                document.getElementById('login-error').style.display = 'block';
            }
        };

        const launchApp = () => {
            document.getElementById('login-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'flex';
            document.getElementById('current-user-name').innerText = currentUser.name.toUpperCase();
            document.getElementById('current-user-role').innerText = currentUser.role === 'superadmin' ? 'ROOT ACCESS' : currentUser.role.toUpperCase();
            document.getElementById('user-initials').innerText = currentUser.name.charAt(0);
            if(currentUser.role === 'superadmin') document.getElementById('nav-users').style.display = 'block';
            refreshDashboard();
        };

        const logout = () => { sessionStorage.removeItem('asb_session'); location.reload(); };

        const refreshDashboard = () => {
            const requests = JSON.parse(localStorage.getItem('asb_requests'));
            const tbody = document.getElementById('requests-body');
            tbody.innerHTML = '';

            const displayData = currentUser.role === 'user' ? requests.filter(r => r.requestor === currentUser.username) : requests;

            displayData.forEach(req => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td style="font-weight:800; color:var(--primary)">#ASB-${req.id}</td>
                    <td><i class="fas fa-server" style="margin-right:8px; opacity:0.5"></i> ${req.branch}</td>
                    <td>${req.desc}</td>
                    <td><span class="badge badge-${req.status}">${req.status}</span></td>
                    <td><i class="far fa-clock"></i> ${req.date}</td>
                    <td>
                        ${(currentUser.role !== 'user' && req.status === 'pending') ? 
                          `<button onclick="updateStatus(${req.id}, 'approved')" title="Approve" style="color:var(--success); border:none; background:none; cursor:pointer; font-size:1.2rem; margin-right:10px;"><i class="fas fa-check-circle"></i></button>
                           <button onclick="updateStatus(${req.id}, 'rejected')" title="Reject" style="color:var(--danger); border:none; background:none; cursor:pointer; font-size:1.2rem;"><i class="fas fa-times-circle"></i></button>` : 
                          '<i class="fas fa-lock" style="opacity:0.3"></i>'}
                    </td>
                `;
                tbody.appendChild(tr);
            });

            document.getElementById('stat-total').innerText = displayData.length;
            document.getElementById('stat-pending').innerText = displayData.filter(x => x.status === 'pending').length;
            document.getElementById('stat-resolved').innerText = displayData.filter(x => x.status === 'approved').length;
        };

        const saveRequest = () => {
            const branch = document.getElementById('req-branch').value;
            const desc = document.getElementById('req-desc').value;
            if(!desc) return alert("Please enter task description");

            const requests = JSON.parse(localStorage.getItem('asb_requests'));
            const newReq = {
                id: Math.floor(4000 + Math.random() * 900),
                branch: branch,
                desc: desc,
                status: 'pending',
                date: new Date().toISOString().split('T')[0],
                requestor: currentUser.username
            };
            requests.unshift(newReq);
            localStorage.setItem('asb_requests', JSON.stringify(requests));
            closeModal('requestModal');
            refreshDashboard();
        };

        const updateStatus = (id, newStatus) => {
            const requests = JSON.parse(localStorage.getItem('asb_requests'));
            const idx = requests.findIndex(r => r.id === id);
            requests[idx].status = newStatus;
            localStorage.setItem('asb_requests', JSON.stringify(requests));
            refreshDashboard();
        };

        const openModal = (id) => document.getElementById(id).style.display = 'flex';
        const closeModal = (id) => document.getElementById(id).style.display = 'none';

        window.onload = () => { initData(); if (currentUser) launchApp(); };
    </script>
</body>
</html>
