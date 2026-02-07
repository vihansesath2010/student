<!DOCTYPE html>
<html lang="si">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ProLMS Cloud | Professional Edition</title>
    
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        :root {
            --primary: #3b82f6; --success: #10b981; --danger: #ef4444;
            --bg: #0f172a; --card: #1e293b; --text-main: #f8fafc;
            --text-sub: #94a3b8; --border: #334155;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Inter', sans-serif; }
        body { background-color: var(--bg); color: var(--text-main); }

        /* --- Login Section --- */
        #login-section { 
            height: 100vh; display: flex; justify-content: center; align-items: center; 
            background: radial-gradient(circle at top right, #1e3a8a, #0f172a); 
        }
        .login-box { 
            background: rgba(30, 41, 59, 0.7); backdrop-filter: blur(15px); padding: 50px 40px; 
            border-radius: 30px; width: 400px; text-align: center; border: 1px solid var(--border); 
            box-shadow: 0 25px 50px rgba(0,0,0,0.5); 
        }
        .login-box input { 
            width: 100%; padding: 18px; margin-bottom: 20px; background: rgba(15,23,42,0.6); 
            border: 1px solid var(--border); border-radius: 15px; color: white; outline: none; font-size: 16px; 
        }

        /* --- Dashboard UI --- */
        #dashboard-section { display: none; }
        .navbar { 
            background: var(--card); padding: 15px 40px; display: flex; justify-content: space-between; 
            border-bottom: 1px solid var(--border); position: sticky; top: 0; z-index: 100; 
        }
        .container { padding: 40px; max-width: 1400px; margin: auto; }
        
        /* Grid & Cards */
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 25px; }
        .card { 
            background: var(--card); padding: 35px 25px; border-radius: 28px; text-align: center; 
            border: 1px solid var(--border); cursor: pointer; position: relative; transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1); 
        }
        .card:hover { transform: translateY(-10px); border-color: var(--primary); box-shadow: 0 20px 40px rgba(0,0,0,0.4); }
        .avatar { width: 70px; height: 70px; background: #1e3a8a; border-radius: 20px; display: flex; align-items: center; justify-content: center; margin: 0 auto 15px; font-size: 32px; color: #60a5fa; }
        .delete-btn { position: absolute; top: 20px; right: 20px; color: #334155; font-size: 22px; cursor: pointer; transition: 0.2s; }
        .delete-btn:hover { color: var(--danger); }

        /* --- Modal (Large Input Rules) --- */
        #modal-overlay { 
            display: none; position: fixed; inset: 0; background: rgba(15, 23, 42, 0.95); 
            backdrop-filter: blur(12px); justify-content: center; align-items: center; z-index: 1000; 
        }
        .modal { 
            background: #1e293b; padding: 45px; border-radius: 35px; width: 480px; 
            border: 1px solid rgba(255,255,255,0.1); box-shadow: 0 30px 60px rgba(0,0,0,0.6); 
        }
        .modal-group { margin-bottom: 25px; }
        .modal-label { display: block; color: var(--text-sub); font-size: 13px; font-weight: 700; margin-bottom: 10px; margin-left: 10px; text-transform: uppercase; letter-spacing: 1px; }
        .modal input, .modal select { 
            width: 100%; padding: 20px 25px; background: rgba(15, 23, 42, 0.8); 
            border: 2px solid var(--border); border-radius: 20px; color: white; font-size: 17px; outline: none; transition: 0.3s; 
        }
        .modal input:focus, .modal select:focus { border-color: var(--primary); background: #0f172a; box-shadow: 0 0 0 5px rgba(59, 130, 246, 0.2); }

        /* --- Buttons --- */
        .btn-big { 
            width: 100%; padding: 20px; background: linear-gradient(135deg, #3b82f6, #1d4ed8); 
            color: white; border: none; border-radius: 18px; font-size: 18px; font-weight: 800; 
            cursor: pointer; transition: 0.3s; box-shadow: 0 10px 20px rgba(37, 99, 235, 0.3); 
        }
        .btn-big:hover { transform: translateY(-3px); box-shadow: 0 15px 30px rgba(37, 99, 235, 0.5); }
        .btn-save { background: linear-gradient(135deg, #10b981, #059669); box-shadow: 0 10px 20px rgba(16, 185, 129, 0.2); }

        /* Tables */
        .record-card { background: var(--card); border-radius: 24px; padding: 30px; border: 1px solid var(--border); overflow-x: auto; }
        .table-lms { width: 100%; border-collapse: collapse; min-width: 800px; }
        .table-lms th { text-align: left; padding: 18px; color: var(--text-sub); border-bottom: 2px solid var(--border); font-size: 14px; }
        .table-lms td { padding: 18px; border-bottom: 1px solid var(--border); }
        .pay-btn { padding: 10px 20px; border-radius: 12px; border: none; font-weight: 800; cursor: pointer; background: #0f172a; color: var(--text-sub); transition: 0.3s; }
        .pay-btn.paid { background: #064e3b; color: #34d399; }

        .week-grid { display: flex; gap: 8px; }
        .week-item { width: 35px; height: 35px; border-radius: 10px; border: 2px solid var(--border); display: flex; align-items: center; justify-content: center; font-size: 12px; font-weight: 800; cursor: pointer; transition: 0.2s; }
        .week-item.present { background: var(--success); color: white; border-color: var(--success); }

        .fab { position: fixed; bottom: 40px; right: 40px; width: 75px; height: 75px; background: var(--primary); color: white; border-radius: 50%; border: none; font-size: 40px; cursor: pointer; box-shadow: 0 15px 30px rgba(0,0,0,0.4); display: flex; align-items: center; justify-content: center; transition: 0.3s; z-index: 500; }
        .fab:hover { transform: scale(1.1) rotate(90deg); }
    </style>
</head>
<body>

    <section id="login-section">
        <div class="login-box">
            <h1 style="color:white; font-size: 36px; font-weight: 800; margin-bottom: 10px;">ProLMS</h1>
            <p style="color:var(--text-sub); margin-bottom:40px;">Cloud System Active ✅</p>
            <input type="text" id="username" placeholder="පරිශීලක නාමය">
            <input type="password" id="password" placeholder="මුරපදය">
            <button class="btn-big" onclick="checkLogin()">පද්ධතියට පිවිසෙන්න</button>
        </div>
    </section>

    <section id="dashboard-section">
        <nav class="navbar">
            <div onclick="showView('main-content')" style="font-size:26px; font-weight:800; color:var(--primary); cursor:pointer;">ProLMS<span style="color:white;">.</span></div>
            <div style="display:flex; gap:25px; align-items:center;">
                <button onclick="showView('settings-view')" style="background:none; border:none; color:var(--text-sub); cursor:pointer; font-weight:700; font-size:16px;">⚙️ Settings</button>
                <button onclick="logout()" style="color:var(--danger); background:none; border:none; font-weight:800; cursor:pointer; font-size:16px;">Logout</button>
            </div>
        </nav>

        <div class="container">
            <div id="main-content">
                <div class="grid" id="studentGrid"></div>
            </div>

            <div id="student-view" style="display:none;">
                <div onclick="showView('main-content')" style="color:var(--primary); cursor:pointer; font-weight:800; margin-bottom:30px; display:inline-block; font-size:18px;">← ආපසු යන්න</div>
                <h1 id="vName" style="font-size:45px; letter-spacing:-1px; margin-bottom:5px;">Name</h1>
                <p id="vInfo" style="color:var(--text-sub); margin-bottom:40px; font-size:18px;"></p>
                <div class="record-card">
                    <table class="table-lms">
                        <thead><tr><th>මාසය</th><th>ගෙවීම් (Payment)</th><th>පැමිණීම (සති 1-10)</th></tr></thead>
                        <tbody id="recordBody"></tbody>
                    </table>
                </div>
            </div>

            <div id="settings-view" style="display:none;">
                <div style="max-width:550px; margin:auto; background:var(--card); padding:50px; border-radius:35px; border:1px solid var(--border);">
                    <h2 style="margin-bottom:30px; color:var(--primary); font-size:28px;">පද්ධති සැකසුම්</h2>
                    <div class="modal-group">
                        <label class="modal-label">ගෙවීම් තහවුරු කිරීමේ මුරපදය</label>
                        <input type="text" id="payPwInput" placeholder="නව මුරපදය මෙතන ලියන්න">
                    </div>
                    <button class="btn-big" onclick="updatePayPassword()">මුරපදය යාවත්කාලීන කරන්න</button>
                </div>
            </div>
        </div>
        <button id="addFab" class="fab" onclick="showModal()">+</button>
    </section>

    <div id="modal-overlay">
        <div class="modal">
            <h2 style="color:white; margin-bottom:35px; text-align:center; font-size: 30px; font-weight: 800;">නව ශිෂ්‍ය තොරතුරු</h2>
            
            <div class="modal-group">
                <label class="modal-label">ශිෂ්‍යයාගේ සම්පූර්ණ නම</label>
                <input type="text" id="inName" placeholder="උදා: කසුන් පෙරේරා">
            </div>

            <div class="modal-group">
                <label class="modal-label">ශ්‍රේණිය (Grade)</label>
                <select id="inGrade">
                    <option value="" disabled selected>පන්තිය තෝරන්න</option>
                    <option value="6">6 ශ්‍රේණිය</option><option value="7">7 ශ්‍රේණිය</option><option value="8">8 ශ්‍රේණිය</option><option value="9">9 ශ්‍රේණිය</option><option value="10">10 ශ්‍රේණිය</option><option value="11">11 ශ්‍රේණිය</option>
                </select>
            </div>

            <div class="modal-group">
                <label class="modal-label">දුරකථන අංකය</label>
                <input type="text" id="inPhone" placeholder="07x xxxxxxx">
            </div>

            <button class="btn-big btn-save" onclick="addStudent()">✅ ශිෂ්‍යයා පද්ධතියට එක් කරන්න</button>
            <button onclick="closeModal()" style="width:100%; margin-top:25px; background:none; border:none; color:var(--text-sub); cursor:pointer; font-weight: 700;">අවලංගු කරන්න</button>
        </div>
    </div>

    <script>
        // --- ඔබේ FIREBASE CONFIG එක ---
        const firebaseConfig = {
          apiKey: "AIzaSyC2pP0uNFeOoY0EW_wiAvC5b91Hte-7Pbg",
          authDomain: "student-lms-teacher.firebaseapp.com",
          projectId: "student-lms-teacher",
          storageBucket: "student-lms-teacher.firebasestorage.app",
          messagingSenderId: "798959070441",
          appId: "1:798959070441:web:fb5a9df4a5d4e20b8d5478",
          databaseURL: "https://student-lms-teacher-default-rtdb.firebaseio.com/"
        };

        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();

        let db = {};
        let payPassword = "34";
        const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];

        // Cloud දත්ත Sync කිරීම
        database.ref('students').on('value', (snapshot) => {
            db = snapshot.val() || {};
            renderStudents();
        });

        database.ref('settings/payPassword').on('value', (snapshot) => {
            if(snapshot.val()) payPassword = snapshot.val();
        });

        // Authentication පරීක්ෂාව
        window.onload = () => {
            if(sessionStorage.getItem('lmsLogged') === 'true') {
                document.getElementById('login-section').style.display = 'none';
                document.getElementById('dashboard-section').style.display = 'block';
                showView('main-content');
            }
        };

        function checkLogin() {
            if(document.getElementById('username').value === "teacher.lms" && document.getElementById('password').value === "1234") {
                sessionStorage.setItem('lmsLogged', 'true');
                location.reload();
            } else { alert("ලොග් වීමේ විස්තර වැරදියි!"); }
        }

        function logout() { sessionStorage.removeItem('lmsLogged'); location.reload(); }

        function saveToCloud() { database.ref('students').set(db); }

        function showView(viewId) {
            document.getElementById('main-content').style.display = 'none';
            document.getElementById('student-view').style.display = 'none';
            document.getElementById('settings-view').style.display = 'none';
            document.getElementById(viewId).style.display = 'block';
            document.getElementById('addFab').style.display = (viewId === 'main-content') ? 'flex' : 'none';
        }

        function updatePayPassword() {
            const newPw = document.getElementById('payPwInput').value;
            if(!newPw) return alert("මුරපදයක් ඇතුළත් කරන්න!");
            database.ref('settings/payPassword').set(newPw).then(() => {
                alert("මුරපදය සාර්ථකව Cloud එකේ සුරැකුණා!");
                showView('main-content');
            });
        }

        function showModal() { document.getElementById('modal-overlay').style.display = 'flex'; }
        function closeModal() { document.getElementById('modal-overlay').style.display = 'none'; }

        function addStudent() {
            const name = document.getElementById('inName').value;
            const grade = document.getElementById('inGrade').value;
            if(!name || !grade) return alert("කරුණාකර නම සහ ශ්‍රේණිය ඇතුළත් කරන්න!");
            const id = "S" + Date.now();
            db[id] = { name, grade, phone: document.getElementById('inPhone').value || "N/A", pay: {}, att: {} };
            months.forEach(m => { db[id].pay[m] = false; db[id].att[m] = new Array(10).fill(false); });
            saveToCloud(); closeModal();
            document.getElementById('inName').value = '';
        }

        function renderStudents() {
            const grid = document.getElementById('studentGrid');
            grid.innerHTML = '';
            Object.keys(db).forEach(id => {
                const card = document.createElement('div');
                card.className = 'card';
                card.onclick = () => openStudent(id);
                card.innerHTML = `<span class="delete-btn" onclick="event.stopPropagation(); deleteStu('${id}')">×</span>
                                  <div class="avatar">👤</div><h3>${db[id].name}</h3><p style="color:var(--text-sub);">Grade ${db[id].grade}</p>`;
                grid.appendChild(card);
            });
        }

        function openStudent(id) {
            showView('student-view');
            const s = db[id];
            document.getElementById('vName').innerText = s.name;
            document.getElementById('vInfo').innerText = `Grade ${s.grade} | Contact: ${s.phone}`;
            const body = document.getElementById('recordBody');
            body.innerHTML = '';
            months.forEach(m => {
                const tr = document.createElement('tr');
                let weeks = `<div class="week-grid">`;
                for(let i=0; i<10; i++) weeks += `<div class="week-item ${s.att[m][i]?'present':''}" onclick="toggleAtt('${id}','${m}',${i})">${i+1}</div>`;
                tr.innerHTML = `<td>${m}</td><td><button class="pay-btn ${s.pay[m]?'paid':''}" onclick="securePay('${id}','${m}')">${s.pay[m]?'Paid ✅':'Pay Now'}</button></td><td>${weeks}</td>`;
                body.appendChild(tr);
            });
        }

        function securePay(id, m) {
            const pw = prompt("ගෙවීම තහවුරු කිරීමට මුරපදය ඇතුළත් කරන්න:");
            if(pw === payPassword) { db[id].pay[m] = !db[id].pay[m]; saveToCloud(); openStudent(id); }
            else if(pw !== null) alert("මුරපදය වැරදියි!");
        }

        function toggleAtt(id, m, i) { db[id].att[m][i] = !db[id].att[m][i]; saveToCloud(); openStudent(id); }
        function deleteStu(id) { if(confirm("මෙම ශිෂ්‍යයා පද්ධතියෙන් ඉවත් කරන්නද?")) { delete db[id]; saveToCloud(); } }
    </script>
</body>
</html>
