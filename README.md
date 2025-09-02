<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Assets - IT Management System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }
        
        .equipment-card {
            transition: all 0.3s ease;
            border-left: 4px solid #667eea;
        }
        
        .equipment-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
        }
        
        .search-input:focus {
            outline: none;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.3);
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .fade-in {
            animation: fadeIn 0.5s ease forwards;
        }
    </style>
</head>
<body class="min-h-screen py-8 px-4">
    <div class="max-w-6xl mx-auto">
        <!-- Header -->
        <div class="text-center mb-8 fade-in">
            <div class="flex items-center justify-center mb-4">
                <div class="w-16 h-16 bg-white rounded-full flex items-center justify-center shadow-lg mr-4">
                    <i class="fas fa-laptop-code text-3xl text-purple-600"></i>
                </div>
                <h1 class="text-4xl font-bold text-white">Assets Peralatan Kerja</h1>
            </div>
            <p class="text-white/90 text-lg">Manajemen dan tracking peralatan IT untuk seluruh karyawan</p>
        </div>

        <!-- Search Section -->
        <div class="bg-white rounded-2xl shadow-xl p-6 mb-8 fade-in">
            <div class="flex flex-col md:flex-row items-center gap-4">
                <div class="flex-1 relative">
                    <i class="fas fa-search absolute left-4 top-3 text-gray-400"></i>
                    <input 
                        type="text" 
                        id="searchInput" 
                        placeholder="Cari berdasarkan nama karyawan, departemen, atau peralatan..." 
                        class="search-input w-full pl-12 pr-4 py-3 border border-gray-300 rounded-xl focus:border-purple-500"
                    >
                </div>
                <button 
                    onclick="searchEquipment()" 
                    class="bg-purple-600 hover:bg-purple-700 text-white px-8 py-3 rounded-xl font-semibold transition-colors duration-200 w-full md:w-auto"
                >
                    <i class="fas fa-search mr-2"></i>Cari
                </button>
                <button 
                    onclick="resetSearch()" 
                    class="bg-gray-500 hover:bg-gray-600 text-white px-6 py-3 rounded-xl font-semibold transition-colors duration-200 w-full md:w-auto"
                >
                    <i class="fas fa-sync mr-2"></i>Reset
                </button>
            </div>
        </div>

        <!-- Equipment List -->
        <div id="equipmentList" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <!-- Data akan diisi oleh JavaScript -->
        </div>

        <!-- Empty State -->
        <div id="emptyState" class="hidden text-center py-12 bg-white rounded-2xl shadow-xl">
            <i class="fas fa-search text-6xl text-gray-300 mb-4"></i>
            <h3 class="text-xl font-semibold text-gray-600 mb-2">Tidak ada hasil ditemukan</h3>
            <p class="text-gray-500">Coba kata kunci pencarian yang berbeda atau reset pencarian</p>
        </div>
    </div>

    <script>
        // Sample data - in real application this would come from API/database
        const equipmentData = [
            {
                id: 1,
                employee: "Muhammad Fahrur Rozi",
                department: "TM-70",
                position: "DIVISI : IT",
                avatar: "https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/d4bd931d-028c-430a-aadc-93e18b2fe84f.png",
                equipment: [
                    
                    { name: "Monitor : LG", sn: "-", status: "Active" },
                
                    { name: "Desktop PC : Microsoft", sn: "-", status: "Active" },
                 
                    { name: "UPS : ICAcc 600", sn: "-", status: "Active" }
                ]
            },
            {
                id: 2,
                employee: "Faried Ginanjar Rizky",
                department: "TM-175",
                position: "DIVISI : IT",
                avatar: "https://placehold.co/100x100",
                equipment: [
                   
                    { name: "Monitor : LG", sn: "-", status: "Active" },

                    { name: "Desktop PC : Microsoft", sn: "-", status: "Active" },

                    { name: "Keyboard : Logitech", sn: "-", status: "Active" }
                ]
            },
            {
                id: 3,
                employee: "Ivan Leo",
                department: "TM-69",
                position: "DIVISI : IT",
                avatar: "https://placehold.co/100x100",
                equipment: [
                    { name: "Laptop ThinkPad", sn: "THNKP7890", status: "Active" },
                    { name: "Smartphone Samsung", sn: "SGS21U345", status: "Active" }
                ]
            },
            {
                id: 4,
                employee: "Dewi Anggraini",
                department: "HR",
                position: "HR Manager",
                avatar: "https://placehold.co/100x100",
                equipment: [
                    { name: "Laptop HP Elite", sn: "HPELT6789", status: "Active" },
                    { name: "Headset Wireless", sn: "HSWT4567", status: "Active" }
                ]
            },
            {
                id: 5,
                employee: "Rizki Pratama",
                department: "Finance",
                position: "Accountant",
                avatar: "https://placehold.co/100x100",
                equipment: [
                    { name: "Desktop PC", sn: "DTPC78901", status: "Active" },
                    { name: "Calculator", sn: "CALC1234", status: "Active" },
                    { name: "Printer Laser", sn: "PRNT5678", status: "Active" }
                ]
            },
            {
                id: 6,
                employee: "Maya Sari",
                department: "Operations",
                position: "Operations Manager",
                avatar: "https://placehold.co/100x100",
                equipment: [
                    { name: "Laptop Surface", sn: "SURF7890", status: "Maintenance" },
                    { name: "Tablet Android", sn: "TAB4567", status: "Active" }
                ]
            }
        ];

        // Render equipment cards
        function renderEquipment(data) {
            const container = document.getElementById('equipmentList');
            const emptyState = document.getElementById('emptyState');
            
            if (data.length === 0) {
                container.classList.add('hidden');
                emptyState.classList.remove('hidden');
                return;
            }
            
            container.classList.remove('hidden');
            emptyState.classList.add('hidden');
            
            container.innerHTML = data.map(item => `
                <div class="equipment-card bg-white rounded-2xl shadow-lg overflow-hidden hover:shadow-xl">
                    <div class="bg-gradient-to-r from-purple-600 to-blue-600 p-6 text-white">
                        <div class="flex items-center">
                            <img src="${item.avatar}" alt="Profile photo of ${item.employee}" class="w-16 h-16 rounded-full border-4 border-white/20 mr-4" onerror="this.src='https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/3e0dd9c9-de04-4a15-b996-203a1d422cad.png)}'">
                            <div>
                                <h3 class="text-xl font-bold">${item.employee}</h3>
                                <p class="text-white/90">${item.position}</p>
                                <span class="text-white/80 text-sm bg-white/20 px-3 py-1 rounded-full">${item.department}</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="p-6">
                        <h4 class="font-semibold text-gray-700 mb-4 flex items-center">
                            <i class="fas fa-laptop-medical mr-2 text-purple-600"></i>
                            Daftar Peralatan
                        </h4>
                        
                        <div class="space-y-3">
                            ${item.equipment.map(eq => `
                                <div class="border-l-4 ${eq.status === 'Active' ? 'border-green-500' : 'border-yellow-500'} pl-4 py-2">
                                    <div class="flex justify-between items-start">
                                        <div>
                                            <h5 class="font-medium text-gray-800">${eq.name}</h5>
                                            <p class="text-sm text-gray-600">SN: ${eq.sn}</p>
                                        </div>
                                        <span class="text-xs px-2 py-1 rounded-full ${eq.status === 'Active' ? 'bg-green-100 text-green-800' : 'bg-yellow-100 text-yellow-800'}">
                                            ${eq.status}
                                        </span>
                                    </div>
                                </div>
                            `).join('')}
                        </div>
                        
                        <div class="mt-6 pt-4 border-t border-gray-100">
                            <div class="flex justify-between items-center text-sm text-gray-600">
                                <span>Total: ${item.equipment.length} peralatan</span>
                                <span class="flex items-center">
                                    <i class="fas fa-circle text-green-500 text-xs mr-1"></i>
                                    ${item.equipment.filter(eq => eq.status === 'Active').length} aktif
                                </span>
                            </div>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        // Search function
        function searchEquipment() {
            const searchTerm = document.getElementById('searchInput').value.toLowerCase().trim();
            
            if (!searchTerm) {
                renderEquipment(equipmentData);
                return;
            }
            
            const filteredData = equipmentData.filter(item => 
                item.employee.toLowerCase().includes(searchTerm) ||
                item.department.toLowerCase().includes(searchTerm) ||
                item.position.toLowerCase().includes(searchTerm) ||
                item.equipment.some(eq => 
                    eq.name.toLowerCase().includes(searchTerm) ||
                    eq.sn.toLowerCase().includes(searchTerm)
                )
            );
            
            renderEquipment(filteredData);
        }

        // Reset search
        function resetSearch() {
            document.getElementById('searchInput').value = '';
            renderEquipment(equipmentData);
        }

        // Enter key to search
        document.getElementById('searchInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                searchEquipment();
            }
        });

        // Initial render
        document.addEventListener('DOMContentLoaded', function() {
            renderEquipment(equipmentData);
            
            // Add fade-in animation to all cards
            setTimeout(() => {
                document.querySelectorAll('.equipment-card').forEach((card, index) => {
                    card.style.animationDelay = ${index * 0.1}s;
                    card.classList.add('fade-in');
                });
            }, 100);
        });
    </script>
</body>
</html>
