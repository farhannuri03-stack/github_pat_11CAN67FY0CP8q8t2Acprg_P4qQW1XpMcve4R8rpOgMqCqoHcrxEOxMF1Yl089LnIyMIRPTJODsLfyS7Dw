<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard Perumahan Anugrah - Fixed System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .login-bg { background: linear-gradient(135deg, #0f172a 0%, #1e3a8a 100%); }
        .sidebar-active { background-color: #2563eb; color: white; border-left: 4px solid #fbbf24; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .card-shadow { box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1); }
        tr:nth-child(even) { background-color: #f8fafc; }
    </style>
</head>
<body class="bg-slate-50 font-sans text-slate-900">

    <!-- Login Section -->
    <div id="loginSection" class="fixed inset-0 z-[100] flex items-center justify-center login-bg p-4">
        <div class="bg-white p-8 rounded-2xl shadow-2xl w-full max-w-md border border-slate-200">
            <div class="text-center mb-8">
                <div class="inline-block p-3 bg-blue-100 rounded-full mb-4">
                    <span class="text-3xl">🏠</span>
                </div>
                <h1 class="text-2xl font-black text-slate-800 uppercase tracking-tight">Anugrah Group</h1>
                <p class="text-slate-500 text-sm font-medium">Sistem Dashboard Perumahan</p>
            </div>
            <div class="space-y-4">
                <div>
                    <label class="block text-xs font-bold uppercase text-slate-500 mb-1">ID Pengguna</label>
                    <input type="text" id="username" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none transition" placeholder="AdminID">
                </div>
                <div>
                    <label class="block text-xs font-bold uppercase text-slate-500 mb-1">Password</label>
                    <input type="password" id="password" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none transition" placeholder="••••••••">
                </div>
                <button onclick="handleLogin()" class="w-full bg-blue-600 text-white py-3 rounded-xl font-bold hover:bg-blue-700 transform transition active:scale-95 shadow-lg">LOGIN</button>
                <p id="loginError" class="text-red-500 text-xs text-center font-semibold hidden italic">ID atau Password salah!</p>
            </div>
        </div>
    </div>

    <!-- Main Dashboard Section -->
    <div id="mainDashboard" class="hidden min-h-screen flex flex-col md:flex-row">
        
        <!-- Sidebar -->
        <aside class="w-full md:w-72 bg-slate-900 text-slate-300 flex-shrink-0 flex flex-col shadow-2xl z-40">
            <div class="p-6 border-b border-slate-800">
                <h2 class="text-lg font-black text-white flex items-center gap-2">
                    <span class="bg-blue-600 px-2 py-0.5 rounded">AG</span> ANUGRAH
                </h2>
            </div>
            <nav class="flex-1 mt-4 overflow-y-auto no-scrollbar pb-6">
                <p class="px-6 text-[10px] font-bold text-slate-500 uppercase tracking-widest mb-2">Pilih Lokasi</p>
                <button onclick="changeLoc('ceon')" id="nav-ceon" class="sidebar-active flex items-center w-full px-6 py-4 hover:bg-slate-800 transition text-sm font-medium">Anugrah Regency Ceon</button>
                <button onclick="changeLoc('regency3')" id="nav-regency3" class="flex items-center w-full px-6 py-4 hover:bg-slate-800 transition text-sm font-medium">Anugrah Regency 3</button>
                <button onclick="changeLoc('regency6')" id="nav-regency6" class="flex items-center w-full px-6 py-4 hover:bg-slate-800 transition text-sm font-medium">Anugrah Regency 6</button>
                <button onclick="changeLoc('barabai')" id="nav-barabai" class="flex items-center w-full px-6 py-4 hover:bg-slate-800 transition text-sm font-medium">Anugrah Regency Barabai</button>
                <button onclick="changeLoc('tapin')" id="nav-tapin" class="flex items-center w-full px-6 py-4 hover:bg-slate-800 transition text-sm font-medium">Anugrah Regency Tapin</button>
            </nav>
            <div class="p-4 border-t border-slate-800 space-y-2">
                <div id="adminButtons" class="space-y-2">
                    <button onclick="exportData()" class="flex items-center justify-center gap-2 w-full py-2 text-sm font-bold text-blue-400 hover:bg-blue-950/30 rounded-xl transition border border-blue-900/30">
                        📤 EXPORT DATA
                    </button>
                    <button onclick="importData()" class="flex items-center justify-center gap-2 w-full py-2 text-sm font-bold text-green-400 hover:bg-green-950/30 rounded-xl transition border border-green-900/30">
                        📥 IMPORT DATA
                    </button>
                </div>
                <input type="file" id="importFile" accept=".json" style="display: none;" onchange="handleImport(event)">
                <button onclick="confirmLogout()" class="flex items-center justify-center gap-2 w-full py-3 text-sm font-bold text-red-400 hover:bg-red-950/30 rounded-xl transition border border-red-900/30">
                    🚪 LOGOUT
                </button>
            </div>
        </aside>

        <!-- Main Content -->
        <main class="flex-1 h-screen overflow-y-auto bg-slate-50 p-4 md:p-8">
            <div class="max-w-7xl mx-auto">
                <header class="mb-8 flex flex-col md:flex-row md:items-center justify-between gap-4">
                    <div>
                        <h1 id="currentLocationTitle" class="text-3xl font-black text-slate-800 tracking-tight">Anugrah Regency Ceon</h1>
                        <p class="text-slate-500 font-medium">Manajemen Proyek & Pemberkasan</p>
                    </div>
                    <button id="addDataButton" onclick="openModal()" class="bg-blue-600 text-white px-6 py-3.5 rounded-2xl font-bold hover:bg-blue-700 shadow-xl shadow-blue-200 flex items-center justify-center gap-2 transition active:scale-95">
                        <span>➕ Tambah Data</span>
                    </button>
                </header>

                <!-- Navigation Tabs -->
                <div class="flex space-x-1 mb-6 bg-slate-200 p-1 rounded-xl w-fit overflow-x-auto no-scrollbar">
                    <button onclick="switchTab('nasabah')" id="tab-nasabah" class="px-6 py-2.5 rounded-lg text-sm font-bold transition-all bg-white text-blue-600 shadow-sm">Nasabah</button>
                    <button onclick="switchTab('booking')" id="tab-booking" class="px-6 py-2.5 rounded-lg text-sm font-bold transition-all text-slate-600 hover:bg-slate-300">Booking</button>
                    <button onclick="switchTab('pemberkasan')" id="tab-pemberkasan" class="px-6 py-2.5 rounded-lg text-sm font-bold transition-all text-slate-600 hover:bg-slate-300">Pemberkasan</button>
                    <button onclick="switchTab('pembangunan')" id="tab-pembangunan" class="px-6 py-2.5 rounded-lg text-sm font-bold transition-all text-slate-600 hover:bg-slate-300">Pembangunan</button>
                </div>

                <!-- Search -->
                <div class="relative mb-6">
                    <input type="text" id="searchInput" onkeyup="filterData()" placeholder="Cari nama nasabah atau nomor blok..." class="w-full pl-12 pr-4 py-3.5 bg-white border border-slate-200 rounded-2xl shadow-sm focus:ring-2 focus:ring-blue-500 outline-none transition">
                    <span class="absolute left-4 top-4 text-slate-400">🔍</span>
                </div>

                <!-- Table Container -->
                <div class="bg-white rounded-2xl border border-slate-200 shadow-xl overflow-hidden">
                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse">
                            <thead id="tableHead" class="bg-slate-50 border-b border-slate-200 text-slate-500 text-[10px] font-black uppercase tracking-widest">
                                <!-- Dynamic Head -->
                            </thead>
                            <tbody id="tableBody" class="divide-y divide-slate-100 text-sm">
                                <!-- Dynamic Data -->
                            </tbody>
                        </table>
                    </div>
                    <div id="emptyState" class="hidden py-20 text-center">
                        <p class="text-slate-400 font-bold">Data tidak ditemukan.</p>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- Modal Form -->
    <div id="formModal" class="fixed inset-0 z-[60] hidden bg-slate-900/60 backdrop-blur-sm flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl w-full max-w-xl p-8 max-h-[90vh] overflow-y-auto shadow-2xl">
            <div class="flex items-center justify-between mb-6">
                <h2 id="modalTitle" class="text-2xl font-black text-slate-800 uppercase">Tambah Data</h2>
                <button onclick="closeModal()" class="text-slate-400 hover:text-slate-600 text-2xl">✕</button>
            </div>
            
            <form id="mainForm" class="space-y-4">
                <input type="hidden" id="editIndex">
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-bold uppercase text-slate-500 mb-1.5 tracking-wider">Nama Lengkap</label>
                        <input type="text" id="f_nama" required class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl outline-none focus:ring-2 focus:ring-blue-500" placeholder="Nama Nasabah">
                    </div>
                    <div>
                        <label class="block text-xs font-bold uppercase text-slate-500 mb-1.5 tracking-wider">Blok / Nomor</label>
                        <input type="text" id="f_blok" required class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl outline-none focus:ring-2 focus:ring-blue-500" placeholder="Misal: K1">
                    </div>
                </div>

                <!-- Nasabah Specific -->
                <div id="sec_nasabah" class="grid grid-cols-1 md:grid-cols-2 gap-4 hidden">
                    <div>
                        <label class="block text-xs font-bold uppercase text-slate-500 mb-1.5 tracking-wider">Nomor WA</label>
                        <input type="text" id="f_wa" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl outline-none focus:ring-2 focus:ring-blue-500" placeholder="628xxx">
                    </div>
                    <div>
                        <label class="block text-xs font-bold uppercase text-slate-500 mb-1.5 tracking-wider">Pekerjaan</label>
                        <input type="text" id="f_pekerjaan" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl outline-none focus:ring-2 focus:ring-blue-500" placeholder="Pekerjaan">
                    </div>
                </div>

                <!-- Pemberkasan Specific -->
                <div id="sec_pemberkasan" class="space-y-4 hidden">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs font-bold uppercase text-slate-500 mb-1.5 tracking-wider">BI Checking</label>
                            <select id="f_bi" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl outline-none focus:ring-2 focus:ring-blue-500">
                                <option value="Nihil">Nihil</option>
                                <option value="Tidak Nihil">Tidak Nihil</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-bold uppercase text-slate-500 mb-1.5 tracking-wider">Pilih Bank</label>
                            <select id="f_bank" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl outline-none focus:ring-2 focus:ring-blue-500">
                                <option value="Bank Kalsel Syariah">Bank Kalsel Syariah</option>
                                <option value="Bank Kalsel Konvensional">Bank Kalsel Konvensional</option>
                                <option value="Bank BRI">Bank BRI</option>
                                <option value="Bank BNI">Bank BNI</option>
                                <option value="Bank BCA">Bank BCA</option>
                                <option value="Bank BTN">Bank BTN</option>
                                <option value="Cash">Cash</option>
                            </select>
                        </div>
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs font-bold uppercase text-slate-500 mb-1.5 tracking-wider">Status Tahapan</label>
                            <select id="f_tahap" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl outline-none focus:ring-2 focus:ring-blue-500">
                                <option value="Pemberkasan">Pemberkasan</option>
                                <option value="Wawancara">Wawancara</option>
                                <option value="SP3K">SP3K</option>
                                <option value="Akad">Akad</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-bold uppercase text-slate-500 mb-1.5 tracking-wider">Tgl Pemberkasan</label>
                            <input type="date" id="f_tgl_berkas" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl outline-none focus:ring-2 focus:ring-blue-500">
                        </div>
                    </div>
                    <div>
                        <label class="block text-xs font-bold uppercase text-slate-500 mb-1.5 tracking-wider">Tgl Disetujui (ACC)</label>
                        <input type="date" id="f_tgl_acc" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-xl outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                </div>

                <!-- Pembangunan Specific -->
                <div id="sec_pembangunan" class="hidden">
                    <label class="block text-xs font-bold uppercase text-slate-500 mb-1.5 tracking-wider">Progres Pembangunan (%)</label>
                    <input type="range" id="f_progress" min="0" max="100" step="1" class="w-full h-2 bg-slate-200 rounded-lg appearance-none cursor-pointer accent-blue-600" oninput="document.getElementById('prog_label').innerText = this.value + '%'">
                    <p class="text-right text-blue-600 font-black text-sm mt-1"><span id="prog_label">0</span>% Tahap Pengerjaan</p>
                </div>

                <!-- Global Photo -->
                <div>
                    <label class="block text-xs font-bold uppercase text-slate-500 mb-1.5 tracking-wider">Unggah Foto / Resi</label>
                    <div class="border-2 border-dashed border-slate-200 rounded-2xl p-4 text-center cursor-pointer hover:border-blue-500 transition" onclick="document.getElementById('f_foto').click()">
                        <input type="file" id="f_foto" accept="image/*" class="hidden" onchange="handleImagePreview(event)">
                        <div id="up_placeholder">
                            <p class="text-2xl mb-1">📷</p>
                            <p class="text-xs text-slate-400 font-bold uppercase tracking-widest">Klik untuk unggah</p>
                        </div>
                        <img id="f_preview" class="hidden max-h-32 mx-auto rounded-xl shadow-md border border-slate-100 mt-2">
                    </div>
                </div>

                <div class="flex justify-end gap-3 pt-6">
                    <button type="button" onclick="closeModal()" class="px-6 py-3 text-sm font-black text-slate-400 hover:text-slate-600 transition">BATAL</button>
                    <button type="submit" class="bg-blue-600 text-white px-8 py-3 rounded-2xl font-black text-sm shadow-xl shadow-blue-100 hover:bg-blue-700 transition">SIMPAN DATA</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Viewer Modal -->
    <div id="photoViewer" class="fixed inset-0 z-[70] hidden bg-black/95 backdrop-blur-sm flex flex-col items-center justify-center p-4">
        <button onclick="closeViewer()" class="absolute top-6 right-6 text-white text-4xl hover:scale-110 transition">✕</button>
        <img id="view_img" class="max-w-full max-h-[80vh] rounded-2xl shadow-2xl border border-slate-800">
        <p id="view_cap" class="text-white mt-6 font-black text-xl tracking-tight"></p>
    </div>

    <script>
        // State Management
        let currentLoc = 'ceon';
        let currentTab = 'nasabah';
        let tempPhoto = null;
        let db = {}; // Will be loaded from IndexedDB
        let currentRole = 'admin'; // 'admin' or 'karyawan'

        // IndexedDB Setup
        let dbRequest = indexedDB.open('anugrah_db', 1);
        let idb;

        dbRequest.onupgradeneeded = function(event) {
            idb = event.target.result;
            if (!idb.objectStoreNames.contains('data')) {
                idb.createObjectStore('data', { keyPath: 'key' });
            }
        };

        dbRequest.onsuccess = function(event) {
            idb = event.target.result;
            loadAllData().then(() => {
                renderTable();
            });
        };

        dbRequest.onerror = function(event) {
            console.error('IndexedDB error:', event.target.error);
        };

        async function loadAllData() {
            const transaction = idb.transaction(['data'], 'readonly');
            const store = transaction.objectStore('data');
            const keys = ['ceon_nasabah', 'ceon_booking', 'ceon_pemberkasan', 'ceon_pembangunan',
                          'regency3_nasabah', 'regency3_booking', 'regency3_pemberkasan', 'regency3_pembangunan',
                          'regency6_nasabah', 'regency6_booking', 'regency6_pemberkasan', 'regency6_pembangunan',
                          'barabai_nasabah', 'barabai_booking', 'barabai_pemberkasan', 'barabai_pembangunan',
                          'tapin_nasabah', 'tapin_booking', 'tapin_pemberkasan', 'tapin_pembangunan'];

            db = {
                ceon: { nasabah: [], booking: [], pemberkasan: [], pembangunan: [] },
                regency3: { nasabah: [], booking: [], pemberkasan: [], pembangunan: [] },
                regency6: { nasabah: [], booking: [], pemberkasan: [], pembangunan: [] },
                barabai: { nasabah: [], booking: [], pemberkasan: [], pembangunan: [] },
                tapin: { nasabah: [], booking: [], pemberkasan: [], pembangunan: [] }
            };

            for (let key of keys) {
                try {
                    const request = store.get(key);
                    const result = await new Promise((resolve, reject) => {
                        request.onsuccess = () => resolve(request.result);
                        request.onerror = () => reject(request.error);
                    });
                    if (result) {
                        const [loc, tab] = key.split('_');
                        db[loc][tab] = result.data;
                    }
                } catch (e) {
                    console.log('No data for', key);
                }
            }
        }

        async function saveData(loc, tab, data) {
            const transaction = idb.transaction(['data'], 'readwrite');
            const store = transaction.objectStore('data');
            const key = `${loc}_${tab}`;
            await new Promise((resolve, reject) => {
                const request = store.put({ key: key, data: data });
                request.onsuccess = () => resolve();
                request.onerror = () => reject(request.error);
            });
        }

        // --- AUTH FUNCTIONS (FIXED) ---
        function handleLogin() {
            const u = document.getElementById('username').value;
            const p = document.getElementById('password').value;
            // Gunakan kredensial yang diminta atau default admin
            if ((u === 'admin' && p === 'anugrah123') || (u === 'karyawan' && p === 'karyawan123')) {
                currentRole = u === 'karyawan' ? 'karyawan' : 'admin';
                document.getElementById('loginSection').classList.add('hidden');
                document.getElementById('mainDashboard').classList.remove('hidden');
                updateUIBasedOnRole();
                renderTable();
            } else {
                const err = document.getElementById('loginError');
                err.classList.remove('hidden');
                setTimeout(() => err.classList.add('hidden'), 3000);
            }
        }

        function updateUIBasedOnRole() {
            const adminButtons = document.getElementById('adminButtons');
            const addDataButton = document.getElementById('addDataButton');
            if (currentRole === 'karyawan') {
                adminButtons.style.display = 'none';
                addDataButton.style.display = 'none';
            } else {
                adminButtons.style.display = 'block';
                addDataButton.style.display = 'inline-block';
            }
        }

        function confirmLogout() {
            if (confirm("Apakah Anda yakin ingin keluar dari sistem?")) {
                currentRole = 'admin'; // Reset to default
                document.getElementById('loginSection').classList.remove('hidden');
                document.getElementById('mainDashboard').classList.add('hidden');
                document.getElementById('username').value = '';
                document.getElementById('password').value = '';
            }
        }

        // --- NAVIGATION FUNCTIONS ---
        function changeLoc(loc) {
            currentLoc = loc;
            const locIds = ['ceon', 'regency3', 'regency6', 'barabai', 'tapin'];
            locIds.forEach(id => {
                document.getElementById(`nav-${id}`).className = "flex items-center w-full px-6 py-4 hover:bg-slate-800 transition text-sm font-medium";
            });
            document.getElementById(`nav-${loc}`).className = "sidebar-active flex items-center w-full px-6 py-4 transition text-sm font-medium";
            
            const map = {ceon:'Regency Ceon', regency3:'Regency 3', regency6:'Regency 6', barabai:'Regency Barabai', tapin:'Regency Tapin'};
            document.getElementById('currentLocationTitle').innerText = 'Anugrah ' + map[loc];
            renderTable();
        }

        function switchTab(tab) {
            currentTab = tab;
            ['nasabah', 'booking', 'pemberkasan', 'pembangunan'].forEach(t => {
                const btn = document.getElementById(`tab-${t}`);
                if (t === tab) {
                    btn.className = "px-6 py-2.5 rounded-lg text-sm font-bold transition-all bg-white text-blue-600 shadow-sm";
                } else {
                    btn.className = "px-6 py-2.5 rounded-lg text-sm font-bold transition-all text-slate-600 hover:bg-slate-300";
                }
            });
            renderTable();
        }

        // --- TABLE RENDERING ---
        function renderTable(filterQuery = '') {
            const head = document.getElementById('tableHead');
            const body = document.getElementById('tableBody');
            const empty = document.getElementById('emptyState');
            
            head.innerHTML = '';
            body.innerHTML = '';

            let rawData = db[currentLoc][currentTab] || [];
            
            // Filter
            if (filterQuery) {
                rawData = rawData.filter(item => 
                    item.nama.toLowerCase().includes(filterQuery.toLowerCase()) || 
                    item.blok.toLowerCase().includes(filterQuery.toLowerCase())
                );
            }

            // Sort Blok (Numeric)
            rawData.sort((a, b) => a.blok.localeCompare(b.blok, undefined, {numeric: true, sensitivity: 'base'}));

            if (rawData.length === 0) empty.classList.remove('hidden');
            else empty.classList.add('hidden');

            if (currentTab === 'nasabah') {
                head.innerHTML = `<tr><th class="p-4">Data Nasabah</th><th class="p-4">Blok</th><th class="p-4">Pekerjaan</th><th class="p-4 text-center">Foto</th><th class="p-4 text-right">Aksi</th></tr>`;
                rawData.forEach((item, idx) => {
                    body.innerHTML += `
                        <tr class="hover:bg-blue-50/50 transition">
                            <td class="p-4">
                                <p class="font-black text-slate-800">${item.nama}</p>
                                <p class="text-xs text-blue-500 font-bold">${item.wa || 'No WA Kosong'}</p>
                            </td>
                            <td class="p-4"><span class="bg-slate-200 px-2 py-1 rounded-md text-[10px] font-black">${item.blok}</span></td>
                            <td class="p-4 text-slate-500 font-medium">${item.pekerjaan || '-'}</td>
                            <td class="p-4 text-center">${item.foto ? `<button onclick="openViewer('${item.foto}', '${item.nama}')" class="text-xl hover:scale-125 transition">🖼️</button>` : '-'}</td>
                            <td class="p-4 text-right space-x-2">
                                <button onclick="editData(${idx})" class="text-blue-500 hover:underline font-black">Edit</button>
                                ${currentRole === 'admin' ? `<button onclick="handleDelete(${idx})" class="text-red-500 hover:underline font-black">Hapus</button>` : ''}
                            </td>
                        </tr>`;
                });
            } else if (currentTab === 'booking') {
                head.innerHTML = `<tr><th class="p-4">Nama Nasabah</th><th class="p-4">Blok</th><th class="p-4 text-center">Bukti Bayar</th><th class="p-4 text-right">Aksi</th></tr>`;
                rawData.forEach((item, idx) => {
                    body.innerHTML += `
                        <tr class="hover:bg-blue-50/50 transition">
                            <td class="p-4 font-black text-slate-800">${item.nama}</td>
                            <td class="p-4 font-black text-slate-500">${item.blok}</td>
                            <td class="p-4 text-center">${item.foto ? `<button onclick="openViewer('${item.foto}', 'Resi: ${item.nama}')" class="text-xl hover:scale-125 transition">🧾</button>` : '-'}</td>
                            <td class="p-4 text-right space-x-2">
                                <button onclick="editData(${idx})" class="text-blue-500 hover:underline font-black">Edit</button>
                                ${currentRole === 'admin' ? `<button onclick="handleDelete(${idx})" class="text-red-500 hover:underline font-black">Hapus</button>` : ''}
                            </td>
                        </tr>`;
                });
            } else if (currentTab === 'pemberkasan') {
                head.innerHTML = `<tr><th class="p-4">Informasi</th><th class="p-4">Bank & BI</th><th class="p-4">Status</th><th class="p-4">Tanggal</th><th class="p-4 text-center">File</th><th class="p-4 text-right">Aksi</th></tr>`;
                rawData.forEach((item, idx) => {
                    body.innerHTML += `
                        <tr class="hover:bg-blue-50/50 transition">
                            <td class="p-4">
                                <p class="font-black text-slate-800">${item.nama}</p>
                                <p class="text-[10px] font-bold text-slate-400 uppercase">Blok ${item.blok}</p>
                            </td>
                            <td class="p-4 text-xs">
                                <p><b>BI:</b> ${item.bi}</p>
                                <p><b>Bank:</b> ${item.bank}</p>
                            </td>
                            <td class="p-4">
                                <span class="px-2 py-1 rounded-full text-[10px] font-black ${item.tahap==='Akad'?'bg-green-100 text-green-600':'bg-amber-100 text-amber-600'}">${item.tahap}</span>
                            </td>
                            <td class="p-4 text-[10px] font-medium text-slate-500">
                                <p>📝: ${item.tgl_berkas || '-'}</p>
                                <p>✅: ${item.tgl_acc || '-'}</p>
                            </td>
                            <td class="p-4 text-center">${item.foto ? `<button onclick="openViewer('${item.foto}', 'Berkas: ${item.nama}')" class="text-xl">📂</button>` : '-'}</td>
                            <td class="p-4 text-right space-x-2">
                                <button onclick="editData(${idx})" class="text-blue-500 font-black">Edit</button>
                                ${currentRole === 'admin' ? `<button onclick="handleDelete(${idx})" class="text-red-500 font-black">Hapus</button>` : ''}
                            </td>
                        </tr>`;
                });
            } else if (currentTab === 'pembangunan') {
                head.innerHTML = `<tr><th class="p-4">Nasabah & Lokasi</th><th class="p-4">Progres Pengerjaan</th><th class="p-4 text-center">Foto Unit</th><th class="p-4 text-right">Aksi</th></tr>`;
                rawData.forEach((item, idx) => {
                    body.innerHTML += `
                        <tr class="hover:bg-blue-50/50 transition">
                            <td class="p-4">
                                <p class="font-black text-slate-800">${item.nama}</p>
                                <p class="text-[10px] font-black text-slate-400">Blok ${item.blok}</p>
                            </td>
                            <td class="p-4">
                                <div class="flex items-center gap-3">
                                    <div class="flex-1 bg-slate-200 h-2 rounded-full overflow-hidden">
                                        <div class="bg-blue-600 h-full" style="width: ${item.progress}%"></div>
                                    </div>
                                    <span class="text-xs font-black text-slate-700">${item.progress}%</span>
                                </div>
                            </td>
                            <td class="p-4 text-center">${item.foto ? `<button onclick="openViewer('${item.foto}', 'Progres: ${item.nama}')" class="text-xl">🏗️</button>` : '-'}</td>
                            <td class="p-4 text-right space-x-2">
                                <button onclick="editData(${idx})" class="text-blue-500 font-black">Edit</button>
                                ${currentRole === 'admin' ? `<button onclick="handleDelete(${idx})" class="text-red-500 font-black">Hapus</button>` : ''}
                            </td>
                        </tr>`;
                });
            }
        }

        // --- SEARCH ---
        function filterData() {
            renderTable(document.getElementById('searchInput').value);
        }

        // --- DELETE FUNCTION (FIXED) ---
        async function handleDelete(idx) {
            if (confirm("Apakah Anda benar-benar ingin menghapus data ini secara permanen?")) {
                db[currentLoc][currentTab].splice(idx, 1);
                await saveData(currentLoc, currentTab, db[currentLoc][currentTab]);
                renderTable();
            }
        }

        // --- FORM MODAL LOGIC ---
        function openModal() {
            document.getElementById('mainForm').reset();
            document.getElementById('editIndex').value = '';
            document.getElementById('modalTitle').innerText = 'Tambah Data ' + currentTab;
            document.getElementById('f_preview').classList.add('hidden');
            document.getElementById('up_placeholder').classList.remove('hidden');
            tempPhoto = null;

            // Show/Hide fields
            document.getElementById('sec_nasabah').classList.add('hidden');
            document.getElementById('sec_pemberkasan').classList.add('hidden');
            document.getElementById('sec_pembangunan').classList.add('hidden');

            if (currentTab === 'nasabah') document.getElementById('sec_nasabah').classList.remove('hidden');
            if (currentTab === 'pemberkasan') document.getElementById('sec_pemberkasan').classList.remove('hidden');
            if (currentTab === 'pembangunan') {
                document.getElementById('sec_pembangunan').classList.remove('hidden');
                document.getElementById('prog_label').innerText = '0%';
            }

            document.getElementById('formModal').classList.remove('hidden');
        }

        function closeModal() {
            document.getElementById('formModal').classList.add('hidden');
        }

        function handleImagePreview(e) {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (ev) => {
                    tempPhoto = ev.target.result;
                    document.getElementById('f_preview').src = tempPhoto;
                    document.getElementById('f_preview').classList.remove('hidden');
                    document.getElementById('up_placeholder').classList.add('hidden');
                };
                reader.readAsDataURL(file);
            }
        }

        document.getElementById('mainForm').onsubmit = async function(e) {
            e.preventDefault();
            const idx = document.getElementById('editIndex').value;
            
            let item = {
                nama: document.getElementById('f_nama').value,
                blok: document.getElementById('f_blok').value.toUpperCase(),
                foto: tempPhoto
            };

            if (currentTab === 'nasabah') {
                item.wa = document.getElementById('f_wa').value;
                item.pekerjaan = document.getElementById('f_pekerjaan').value;
            } else if (currentTab === 'pemberkasan') {
                item.bi = document.getElementById('f_bi').value;
                item.bank = document.getElementById('f_bank').value;
                item.tahap = document.getElementById('f_tahap').value;
                item.tgl_berkas = document.getElementById('f_tgl_berkas').value;
                item.tgl_acc = document.getElementById('f_tgl_acc').value;
            } else if (currentTab === 'pembangunan') {
                item.progress = document.getElementById('f_progress').value;
            }

            if (idx === '') {
                db[currentLoc][currentTab].push(item);
            } else {
                db[currentLoc][currentTab][idx] = item;
            }

            await saveData(currentLoc, currentTab, db[currentLoc][currentTab]);
            renderTable();
            closeModal();
        };

        function editData(idx) {
            const item = db[currentLoc][currentTab][idx];
            openModal();
            document.getElementById('modalTitle').innerText = 'Edit Data ' + currentTab;
            document.getElementById('editIndex').value = idx;
            document.getElementById('f_nama').value = item.nama;
            document.getElementById('f_blok').value = item.blok;

            if (currentTab === 'nasabah') {
                document.getElementById('f_wa').value = item.wa || '';
                document.getElementById('f_pekerjaan').value = item.pekerjaan || '';
            } else if (currentTab === 'pemberkasan') {
                document.getElementById('f_bi').value = item.bi;
                document.getElementById('f_bank').value = item.bank;
                document.getElementById('f_tahap').value = item.tahap;
                document.getElementById('f_tgl_berkas').value = item.tgl_berkas || '';
                document.getElementById('f_tgl_acc').value = item.tgl_acc || '';
            } else if (currentTab === 'pembangunan') {
                document.getElementById('f_progress').value = item.progress || 0;
                document.getElementById('prog_label').innerText = (item.progress || 0) + '%';
            }

            if (item.foto) {
                tempPhoto = item.foto;
                document.getElementById('f_preview').src = item.foto;
                document.getElementById('f_preview').classList.remove('hidden');
                document.getElementById('up_placeholder').classList.add('hidden');
            }
        }

        // --- VIEWER ---
        function openViewer(src, cap) {
            document.getElementById('view_img').src = src;
            document.getElementById('view_cap').innerText = cap;
            document.getElementById('photoViewer').classList.remove('hidden');
        }
        function closeViewer() {
            document.getElementById('photoViewer').classList.add('hidden');
        }

        // --- SYNC FUNCTIONS ---
        function exportData() {
            const dataStr = JSON.stringify(db, null, 2);
            const dataBlob = new Blob([dataStr], {type: 'application/json'});
            const url = URL.createObjectURL(dataBlob);
            const link = document.createElement('a');
            link.href = url;
            link.download = 'anugrah_data_backup.json';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            URL.revokeObjectURL(url);
            alert('Data berhasil diekspor! File backup telah diunduh.');
        }

        function importData() {
            document.getElementById('importFile').click();
        }

        async function handleImport(event) {
            const file = event.target.files[0];
            if (!file) return;
            
            if (!confirm('Apakah Anda yakin ingin mengimpor data? Data saat ini akan diganti dengan data dari file backup.')) {
                return;
            }

            const reader = new FileReader();
            reader.onload = async function(e) {
                try {
                    const importedData = JSON.parse(e.target.result);
                    
                    // Validate structure
                    const requiredLocs = ['ceon', 'regency3', 'regency6', 'barabai', 'tapin'];
                    const requiredTabs = ['nasabah', 'booking', 'pemberkasan', 'pembangunan'];
                    
                    for (let loc of requiredLocs) {
                        if (!importedData[loc]) throw new Error(`Lokasi ${loc} tidak ditemukan dalam file backup.`);
                        for (let tab of requiredTabs) {
                            if (!Array.isArray(importedData[loc][tab])) throw new Error(`Data ${tab} di ${loc} tidak valid.`);
                        }
                    }
                    
                    // Save to IndexedDB
                    for (let loc of requiredLocs) {
                        for (let tab of requiredTabs) {
                            await saveData(loc, tab, importedData[loc][tab]);
                        }
                    }
                    
                    // Reload data
                    await loadAllData();
                    renderTable();
                    alert('Data berhasil diimpor! Halaman akan dimuat ulang.');
                    
                } catch (error) {
                    alert('Error mengimpor data: ' + error.message);
                }
            };
            reader.readAsText(file);
            
            // Reset input
            event.target.value = '';
        }
    </script>
</body>
</html>
