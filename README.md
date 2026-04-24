<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ASB Software | Secure Login</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root {
            --primary: #c41e3a; --dark-grey: #1a1a1a; --accent: #ff3131;
            --success: #10b981; --warning: #f59e0b; --danger: #ef4444;
            --bg: #0f172a; --sidebar-bg: #1e293b; --card-bg: #1e293b;
            --text-main: #f8fafc; --text-muted: #94a3b8; --border: rgba(255, 255, 255, 0.1);
            --glass: rgba(30, 41, 59, 0.7); --shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; transition: all 0.3s ease; }
        body { background-color: var(--bg); color: var(--text-main); overflow-x: hidden; }

        #login-screen { height: 100vh; display: flex; align-items: center; justify-content: center; background: radial-gradient(circle at center, #1e293b 0%, #0f172a 100%); }
        .login-card { background: var(--glass); backdrop-filter: blur(20px); padding: 3rem; border-radius: 2.5rem; width: 100%; max-width: 450px; box-shadow: var(--shadow); border: 1px solid var(--border); text-align: center; }
        .company-logo { width: 120px; margin-bottom: 1rem; filter: drop-shadow(0 0 10px var(--primary)); }
        .brand-text { font-size: 2.2rem; font-weight: 800; margin-bottom: 0.5rem; background: linear-gradient(to right, #ffffff, var(--primary)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .input-group { margin-bottom: 1.2rem; text-align: left; }
        .input-group label { display: block; margin-bottom: 0.5rem; color: var(--text-muted); font-size: 0.85rem; font-weight: 600; }
        .input-group input { width: 100%; padding: 1.1rem; border-radius: 1rem; background: rgba(15, 23, 42, 0.6); border: 1px solid var(--border); color: white; outline: none; font-size: 1rem; }
        .input-group input:focus { border-color: var(--primary); box-shadow: 0 0 15px rgba(196, 30, 58, 0.3); }
        
        .btn { width: 100%; padding: 1.1rem; border-radius: 1rem; border: none; background: linear-gradient(45deg, var(--primary), var(--accent)); color: white; font-weight: 700; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 10px; text-transform: uppercase; letter-spacing: 1px; font-size: 0.9rem; }
        .btn:hover { transform: scale(1.02); box-shadow: 0 0 20px rgba(196, 30, 58, 0.5); }
        
        #main-app { display: none; min-height: 100vh; }
        .sidebar { width: 260px; background: var(--sidebar-bg); border-right: 1px solid var(--border); height: 100vh; position: fixed; padding: 2rem; display: flex; flex-direction: column; z-index: 100; }
        .main-content { margin-left: 260px; padding: 2.5rem; width: calc(100% - 260px); }
        .nav-links { list-style: none; margin-top: 2.5rem; }
        .nav-links a { display: flex; align-items: center; gap: 12px; padding: 1rem; text-decoration: none; color: var(--text-muted); border-radius: 1rem; margin-bottom: 0.5rem; font-weight: 600; font-size: 0.95rem; }
        .nav-links a.active, .nav-links a:hover { background: rgba(196, 30, 58, 0.1); color: var(--primary); border-left: 4px solid var(--primary); }
        
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 1.5rem; margin: 2rem 0; }
        .stat-card { background: var(--card-bg); padding: 2rem; border-radius: 1.5rem; border: 1px solid var(--border); border-top: 4px solid var(--primary); box-shadow: var(--shadow); }
        .stat-val { font-size: 2.5rem; font-weight: 800; color: white; display: block; }
        .stat-label { color: var(--text-muted); font-size: 0.8rem; font-weight: 600; text-transform: uppercase; letter-spacing: 1px; }

        .data-table-container { background: var(--card-bg); border-radius: 1.5rem; border: 1px solid var(--border); overflow-x: auto; box-shadow: var(--shadow); }
        table { width: 100%; border-collapse: collapse; min-width: 800px; }
        th { background: rgba(0,0,0,0.2); padding: 1.2rem; text-align: left; color: var(--text-muted); font-size: 0.8rem; border-bottom: 1px solid var(--border); }
        td { padding: 1.2rem; border-bottom: 1px solid var(--border); color: #e2e8f0; font-size: 0.9rem; }
        .badge { padding: 5px 12px; border-radius: 20px; font-size: 0.7rem; font-weight: 800; text-transform: uppercase; }
        .badge-pending { background: rgba(245, 158, 11, 0.2); color: var(--warning); }
        .badge-approved { background: rgba(16, 185, 129, 0.2); color: var(--success); }
    </style>
</head>
<body>

    <!-- LOGIN SCREEN -->
    <div id="login-screen">
        <div class="login-card">
            <img src="logo.png" alt="ASB Logo" class="company-logo" onerror="this.src='https://via.placeholder.com/100/c41e3a/ffffff?text=ASB'">
            <div class="brand-text">ASB SOFTWARE</div>
            <p style="color: var(--text-muted); margin-bottom: 2rem; font-size: 0.85rem;">Secure Infrastructure Management</p>
            
            <div id="login-error" style="color: var(--danger); background: rgba(239, 68, 68, 0.1); padding: 0.8rem; border-radius: 0.8rem; margin-bottom: 1.2rem; display: none; font-size: 0.85rem;">
                <i class="fas fa-lock"></i> Invalid System ID or Key
            </div>
            
            <div class="input-group">
                <label>System Username</label>
                <input type="text" id="username" placeholder="Try: superadmin" autocomplete="off">
            </div>
            
            <div class="input-group">
                <label>Access Key</label>
                <input type="password" id="password" placeholder="Try: password" autocomplete="off">
            </div>
            
            <button class="btn" onclick="handleLogin()">
                Authorize Access <i class="fas fa-shield-alt"></i>
            </button>

            <button onclick="resetSystem()" style="background:none; border:none; color:var(--text-muted); font-size:0.7rem; margin-top:1.5rem; cursor:pointer; text-decoration:underline;">
                Reset System Data (Fix Login Issues)
            </button>
        </div>
    </div>

    <!-- MAIN APP -->
    <div id="main-app">
        <div class="sidebar">
            <div style="text-align: center; margin-bottom: 2rem;">
                <div style="font-weight: 800; font-size: 1.5rem; color: var(--primary);">ASB</div>
                <div style="font-size: 0.6rem; color: var(--text-muted);">ENTERPRISE SOLUTION</div>
            </div>
            
            <ul class="nav-links">
                <li><a href="#" class="active"><i class="fas fa-home"></i> <span>Dashboard</span></a></li>
                <li><a href="#"><i class="fas fa-tasks"></i> <span>Service Requests</span></a></li>
                <li><a href="#"><i class="fas fa-users"></i> <span>Team Directory</span></a></li>
                <li><a href="#" onclick="logout()"><i class="fas fa-power-off"></i> <span>Logout</span></a></li>
            </ul>
        </div>

        <div class="main-content">
            <h1 id="welcome-msg" style="margin-bottom: 1.5rem;">System Health</h1>
            <div class="stats-grid">
                <div class="stat-card">
                    <span class="stat-label">Total Tickets</span>
                    <span id="stat-total" class="stat-val">0</span>
                </div>
                <div class="stat-card" style="border-top-color: var(--warning);">
                    <span class="stat-label">Pending Review</span>
                    <span id="stat-pending" class="stat-val">0</span>
                </div>
                <div class="stat-card" style="border-top-color: var(--success);">
                    <span class="stat-label">Units Resolved</span>
                    <span id="stat-resolved" class="stat-val">0</span>
                </div>
            </div>

            <div class="data-table-container">
                <table>
                    <thead>
                        <tr>
                            <th>REF ID</th>
                            <th>LOCATION HUB</th>
                            <th>DESCRIPTION</th>
                            <th>STATUS</th>
                            <th>TIMESTAMP</th>
                        </tr>
                    </thead>
                    <tbody id="requests-body"></tbody>
                </table>
            </div>
        </div>
    </div>

    <script>
        // --- DATA ENGINE ---
        const initData = () => {
            // Force clear old BMS data if exists to avoid conflicts
            if (localStorage.getItem('bms_users')) {
                localStorage.removeItem('bms_users');
                localStorage.removeItem('bms_requests');
            }

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
                    { id: 401, branch: 'ASB Global Hub', desc: 'Main Frame Checkup', status: 'pending', date: '2026-04-24' },
                    { id: 402, branch: 'North Tech Center', desc: 'Cloud Sync Fix', status: 'approved', date: '2026-04-23' }
                ];
                localStorage.setItem('asb_requests', JSON.stringify(requests));
            }
        };

        const handleLogin = () => {
            const uInput = document.getElementById('username').value.trim().toLowerCase();
            const pInput = document.getElementById('password').value.trim();
            
            if (!uInput || !pInput) {
                showError();
                return;
            }

            const users = JSON.parse(localStorage.getItem('asb_users') || '[]');
            const user = users.find(x => x.username.toLowerCase() === uInput && x.password === pInput);

            if (user) {
                sessionStorage.setItem('asb_session', JSON.stringify(user));
                launchApp(user);
            } else {
                showError();
            }
        };

        const launchApp = (user) => {
            document.getElementById('login-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'flex';
            document.getElementById('welcome-msg').innerText = `Welcome, ${user.name.split(' ')[0]}`;
            renderData();
        };

        const renderData = () => {
            const requests = JSON.parse(localStorage.getItem('asb_requests') || '[]');
            const tbody = document.getElementById('requests-body');
            tbody.innerHTML = requests.map(req => `
                <tr>
                    <td style="color:var(--primary); font-weight:800;">#${req.id}</td>
                    <td>${req.branch}</td>
                    <td>${req.desc}</td>
                    <td><span class="badge badge-${req.status}">${req.status}</span></td>
                    <td>${req.date}</td>
                </tr>
            `).join('');
            
            document.getElementById('stat-total').innerText = requests.length;
            document.getElementById('stat-pending').innerText = requests.filter(x => x.status === 'pending').length;
            document.getElementById('stat-resolved').innerText = requests.filter(x => x.status === 'approved').length;
        };

        const showError = () => {
            const err = document.getElementById('login-error');
            err.style.display = 'block';
            setTimeout(() => { err.style.display = 'none'; }, 3000);
        };

        const logout = () => { sessionStorage.removeItem('asb_session'); location.reload(); };
        
        const resetSystem = () => {
            if(confirm("Yeh system ka tamam data reset kar dega aur login issues fix ho jayenge. Continue?")) {
                localStorage.clear();
                sessionStorage.clear();
                location.reload();
            }
        };

        window.onload = () => {
            initData();
            const session = sessionStorage.getItem('asb_session');
            if (session) launchApp(JSON.parse(session));
        };
    </script>
</body>
</html>
