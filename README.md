# LigaInterna-CDC-2026
Ranking oficial Liga Interna CDC 2026
[ranking_de_la_liga_de_debate (1).html](https://github.com/user-attachments/files/33396643/ranking_de_la_liga_de_debate.1.html)
<!DOCTYPE html>
<html lang="es" class="h-full bg-slate-50">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ranking Liga de Debate</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0f7ff',
                            100: '#e0effe',
                            500: '#1d4ed8',
                            600: '#1e40af',
                            700: '#1d3557',
                            800: '#0f172a'
                        }
                    }
                }
            }
        }
    </script>
    <!-- FontAwesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Font Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
    </style>
</head>
<body class="h-full flex flex-col text-slate-800 antialiased selection:bg-blue-200 selection:text-blue-900">

    <header class="bg-slate-900 text-white shadow-md border-b border-slate-800 sticky top-0 z-30">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex flex-col sm:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-3">
                <div class="p-2.5 bg-blue-600 rounded-xl text-white shadow-lg shadow-blue-500/30">
                    <i class="fa-solid fa-trophy text-xl"></i>
                </div>
                <div>
                    <h1 class="text-xl sm:text-2xl font-bold tracking-tight text-white">Liga de Debate</h1>
                    <p class="text-xs text-slate-400 font-medium">Clasificación Oficial y Puntuación de Oradores</p>
                </div>
            </div>

            <div class="flex items-center gap-2">
                <div id="adminBadge" class="hidden text-xs font-semibold bg-amber-500/20 text-amber-300 border border-amber-500/30 px-3 py-1.5 rounded-full flex items-center gap-1.5">
                    <i class="fa-solid fa-user-shield"></i> Modo Administrador
                </div>
                <button id="adminToggleBtn" onclick="toggleAdminMode()" class="px-4 py-2 text-sm font-semibold rounded-lg bg-blue-600 hover:bg-blue-700 text-white transition-all shadow-sm flex items-center gap-2">
                    <i class="fa-solid fa-lock" id="adminBtnIcon"></i>
                    <span id="adminBtnText">Panel Administrador</span>
                </button>
            </div>
        </div>
    </header>

    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8 space-y-6">

        <!-- Controls & Filters Bar -->
        <div class="bg-white rounded-2xl p-4 sm:p-5 shadow-sm border border-slate-200/80 flex flex-col md:flex-row gap-4 items-stretch md:items-center justify-between">
            <div class="relative flex-1">
                <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-1/2 -translate-y-1/2 text-slate-400 text-sm"></i>
                <input type="text" id="searchInput" oninput="filterData()" placeholder="Buscar equipo, institución u orador..." class="w-full pl-10 pr-4 py-2.5 text-sm bg-slate-50 border border-slate-300 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-600 focus:border-transparent transition-all">
            </div>

            <div class="flex flex-wrap items-center gap-2">
                <button onclick="exportJSON()" class="px-3.5 py-2 text-xs font-semibold rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 border border-slate-300/70 transition-colors flex items-center gap-1.5">
                    <i class="fa-solid fa-download text-slate-500"></i> Exportar JSON
                </button>
                <button onclick="exportCSV()" class="px-3.5 py-2 text-xs font-semibold rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 border border-slate-300/70 transition-colors flex items-center gap-1.5">
                    <i class="fa-solid fa-file-csv text-slate-500"></i> Exportar CSV
                </button>

                <!-- Admin Action Controls -->
                <div id="adminActions" class="hidden flex items-center gap-2 pl-2 border-l border-slate-200">
                    <label class="px-3.5 py-2 text-xs font-semibold rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 border border-slate-300/70 transition-colors cursor-pointer flex items-center gap-1.5">
                        <i class="fa-solid fa-upload text-slate-500"></i> Importar
                        <input type="file" id="importInput" onchange="importJSON(event)" accept=".json" class="hidden">
                    </label>
                    <button onclick="openAddModal()" class="px-4 py-2 text-xs font-bold rounded-lg bg-emerald-600 hover:bg-emerald-700 text-white transition-colors flex items-center gap-1.5 shadow-sm">
                        <i class="fa-solid fa-plus"></i> Añadir Equipo
                    </button>
                </div>
            </div>
        </div>

        <!-- Teams Leaderboard Table -->
        <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
            <div class="px-6 py-4 bg-slate-900 text-white flex justify-between items-center">
                <div>
                    <h2 class="font-bold text-lg flex items-center gap-2">
                        <i class="fa-solid fa-award text-amber-400"></i> Ranking General de Equipos
                    </h2>
                    <p class="text-xs text-slate-300">Ordenado por victorias y puntos totales acumulados</p>
                </div>
                <span id="teamCountBadge" class="text-xs bg-slate-800 text-slate-200 px-3 py-1 rounded-full font-semibold border border-slate-700">0 Equipos</span>
            </div>

            <div class="overflow-x-auto">
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="bg-slate-800 text-slate-200 text-xs font-bold uppercase tracking-wider border-b border-slate-700">
                            <th class="py-3.5 px-4 text-center w-16">Pos</th>
                            <th class="py-3.5 px-6">Equipo / Institución</th>
                            <th class="py-3.5 px-4 text-center">Debates</th>
                            <th class="py-3.5 px-4 text-center">Ganados</th>
                            <th class="py-3.5 px-4 text-center">Perdidos</th>
                            <th class="py-3.5 px-4 text-center bg-slate-700/50">Pts. Orador</th>
                            <th class="py-3.5 px-4 text-center text-amber-300 bg-slate-700">Puntos Totales</th>
                            <th class="admin-only py-3.5 px-4 text-center w-28 hidden">Acciones</th>
                        </tr>
                    </thead>
                    <tbody id="teamsTableBody" class="divide-y divide-slate-200 text-sm font-medium text-slate-700">
                        <!-- Dynamic rows generated by JS -->
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Speakers Top Ranking -->
        <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
            <div class="px-6 py-4 bg-slate-900 text-white flex justify-between items-center">
                <div>
                    <h2 class="font-bold text-lg flex items-center gap-2">
                        <i class="fa-solid fa-microphone text-blue-400"></i> Mejores Oradores
                    </h2>
                    <p class="text-xs text-slate-300">Puntuaciones individuales destacadas</p>
                </div>
            </div>

            <div class="overflow-x-auto">
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="bg-slate-800 text-slate-200 text-xs font-bold uppercase tracking-wider border-b border-slate-700">
                            <th class="py-3.5 px-4 text-center w-16">Pos</th>
                            <th class="py-3.5 px-6">Nombre del Orador</th>
                            <th class="py-3.5 px-6">Equipo / Institución</th>
                            <th class="py-3.5 px-4 text-center text-blue-300 bg-slate-700">Puntuación Individual</th>
                        </tr>
                    </thead>
                    <tbody id="speakersTableBody" class="divide-y divide-slate-200 text-sm font-medium text-slate-700">
                        <!-- Dynamic rows generated by JS -->
                    </tbody>
                </table>
            </div>
        </div>

    </main>

    <!-- Add/Edit Team Modal -->
    <div id="teamModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4">
        <div class="bg-white rounded-2xl max-w-lg w-full p-6 shadow-2xl border border-slate-200 relative animate-in fade-in zoom-in-95 duration-200">
            <div class="flex justify-between items-center pb-4 border-b border-slate-100">
                <h3 id="modalTitle" class="text-lg font-bold text-slate-900">Añadir Nuevo Equipo</h3>
                <button onclick="closeModal()" class="text-slate-400 hover:text-slate-600 transition-colors p-1">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>

            <form id="teamForm" onsubmit="handleFormSubmit(event)" class="space-y-4 mt-4">
                <input type="hidden" id="editingTeamId">

                <div>
                    <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Nombre del Equipo</label>
                    <input type="text" id="teamName" required class="w-full px-3.5 py-2 text-sm bg-slate-50 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-600">
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Institución / Universidad</label>
                    <input type="text" id="institution" required class="w-full px-3.5 py-2 text-sm bg-slate-50 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-600">
                </div>

                <div class="grid grid-cols-3 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Debates</label>
                        <input type="number" id="debatesCount" min="0" required class="w-full px-3.5 py-2 text-sm bg-slate-50 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-600">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Ganados</label>
                        <input type="number" id="winsCount" min="0" required class="w-full px-3.5 py-2 text-sm bg-slate-50 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-600">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Perdidos</label>
                        <input type="number" id="lossesCount" min="0" required class="w-full px-3.5 py-2 text-sm bg-slate-50 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-600">
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Pts. Orador</label>
                        <input type="number" step="0.1" id="speakerPoints" min="0" required class="w-full px-3.5 py-2 text-sm bg-slate-50 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-600">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Puntos Totales</label>
                        <input type="number" step="0.1" id="totalPoints" min="0" required class="w-full px-3.5 py-2 text-sm bg-slate-50 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-600">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Orador Principal / Destacado</label>
                    <input type="text" id="bestSpeaker" placeholder="Opcional" class="w-full px-3.5 py-2 text-sm bg-slate-50 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-600">
                </div>

                <div class="pt-4 flex justify-end gap-2 border-t border-slate-100">
                    <button type="button" onclick="closeModal()" class="px-4 py-2 text-xs font-semibold rounded-lg bg-slate-100 text-slate-700 hover:bg-slate-200">Cancelar</button>
                    <button type="submit" class="px-4 py-2 text-xs font-semibold rounded-lg bg-blue-600 text-white hover:bg-blue-700 shadow-sm">Guardar Equipo</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Password Modal Prompt -->
    <div id="passwordModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4">
        <div class="bg-white rounded-2xl max-w-sm w-full p-6 shadow-2xl border border-slate-200 text-center space-y-4">
            <div class="w-12 h-12 bg-blue-100 text-blue-600 rounded-full flex items-center justify-center mx-auto">
                <i class="fa-solid fa-shield-halved text-xl"></i>
            </div>
            <div>
                <h3 class="text-base font-bold text-slate-900">Acceso Administrador</h3>
                <p class="text-xs text-slate-500 mt-1">Por favor, introduce la clave de acceso para gestionar el ranking.</p>
            </div>
            <div>
                <input type="password" id="adminPasswordInput" placeholder="Contraseña de administrador" class="w-full text-center px-3.5 py-2 text-sm bg-slate-50 border border-slate-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-600">
                <p id="passwordError" class="text-xs text-red-600 font-medium mt-1 hidden">Contraseña incorrecta. Inténtalo de nuevo.</p>
            </div>
            <div class="flex gap-2">
                <button onclick="closePasswordModal()" class="flex-1 py-2 text-xs font-semibold rounded-lg bg-slate-100 text-slate-700 hover:bg-slate-200">Cancelar</button>
                <button onclick="verifyPassword()" class="flex-1 py-2 text-xs font-semibold rounded-lg bg-blue-600 text-white hover:bg-blue-700">Entrar</button>
            </div>
        </div>
    </div>

    <script>
        // Initial Mock Data
        const initialTeams = [
            { id: '1', name: "Ateneo Político", institution: "Univ. Central", debates: 5, wins: 4, losses: 1, speakerPoints: 382.5, totalPoints: 12, bestSpeaker: "Sofía Martínez" },
            { id: '2', name: "Dialéctica Magna", institution: "Univ. de las Artes", debates: 5, wins: 4, losses: 1, speakerPoints: 379.0, totalPoints: 11, bestSpeaker: "Carlos Mendoza" },
            { id: '3', name: "Oradores del Norte", institution: "Inst. Tecnológico", debates: 5, wins: 3, losses: 2, speakerPoints: 365.0, totalPoints: 9, bestSpeaker: "Elena Rostova" },
            { id: '4', name: "Agora Abierta", institution: "Univ. Metropolitana", debates: 5, wins: 2, losses: 3, speakerPoints: 350.5, totalPoints: 6, bestSpeaker: "Javier Ríos" },
            { id: '5', name: "Retórica Viva", institution: "Campus Sur", debates: 5, wins: 1, losses: 4, speakerPoints: 341.0, totalPoints: 3, bestSpeaker: "Lucía Fernández" }
        ];

        let teams = [];
        let isAdmin = false;
        const DEFAULT_PASS = "1234";

        // Load initialization
        window.onload = function() {
            const savedTeams = localStorage.getItem('debate_league_teams');
            if (savedTeams) {
                try {
                    teams = JSON.parse(savedTeams);
                } catch (e) {
                    teams = [...initialTeams];
                }
            } else {
                teams = [...initialTeams];
                saveToLocalStorage();
            }
            renderTables();
        };

        function saveToLocalStorage() {
            localStorage.setItem('debate_league_teams', JSON.stringify(teams));
        }

        function toggleAdminMode() {
            if (isAdmin) {
                isAdmin = false;
                updateAdminUI();
            } else {
                document.getElementById('adminPasswordInput').value = '';
                document.getElementById('passwordError').classList.add('hidden');
                document.getElementById('passwordModal').classList.remove('hidden');
                document.getElementById('adminPasswordInput').focus();
            }
        }

        function closePasswordModal() {
            document.getElementById('passwordModal').classList.add('hidden');
        }

        function verifyPassword() {
            const input = document.getElementById('adminPasswordInput').value;
            if (input === DEFAULT_PASS) {
                isAdmin = true;
                closePasswordModal();
                updateAdminUI();
            } else {
                document.getElementById('passwordError').classList.remove('hidden');
            }
        }

        function updateAdminUI() {
            const badge = document.getElementById('adminBadge');
            const adminActions = document.getElementById('adminActions');
            const btnText = document.getElementById('adminBtnText');
            const btnIcon = document.getElementById('adminBtnIcon');
            const adminOnlyElements = document.querySelectorAll('.admin-only');

            if (isAdmin) {
                badge.classList.remove('hidden');
                adminActions.classList.remove('hidden');
                btnText.textContent = "Cerrar Sesión Admin";
                btnIcon.className = "fa-solid fa-lock-open";
                adminOnlyElements.forEach(el => el.classList.remove('hidden'));
            } else {
                badge.classList.add('hidden');
                adminActions.classList.add('hidden');
                btnText.textContent = "Panel Administrador";
                btnIcon.className = "fa-solid fa-lock";
                adminOnlyElements.forEach(el => el.classList.add('hidden'));
            }
            renderTables();
        }

        function renderTables() {
            const searchTerm = document.getElementById('searchInput').value.toLowerCase();
            const teamsTable = document.getElementById('teamsTableBody');
            const speakersTable = document.getElementById('speakersTableBody');

            // Sort teams: Wins desc, then Total Points desc, then Speaker Points desc
            const sortedTeams = [...teams].sort((a, b) => {
                if (b.wins !== a.wins) return b.wins - a.wins;
                if (b.totalPoints !== a.totalPoints) return b.totalPoints - a.totalPoints;
                return b.speakerPoints - a.speakerPoints;
            });

            const filteredTeams = sortedTeams.filter(t => 
                t.name.toLowerCase().includes(searchTerm) || 
                t.institution.toLowerCase().includes(searchTerm) ||
                (t.bestSpeaker && t.bestSpeaker.toLowerCase().includes(searchTerm))
            );

            document.getElementById('teamCountBadge').textContent = `${filteredTeams.length} Equipos`;

            // Render Teams Table with readable alternating rows
            teamsTable.innerHTML = '';
            filteredTeams.forEach((team, index) => {
                const tr = document.createElement('tr');
                tr.className = index % 2 === 0 ? 'bg-white hover:bg-blue-50/40 transition-colors' : 'bg-slate-50/80 hover:bg-blue-50/40 transition-colors';
                
                let rankBadge = `<span class="font-bold text-slate-500">${index + 1}</span>`;
                if (index === 0) rankBadge = `<span class="inline-flex items-center justify-center w-7 h-7 bg-amber-100 text-amber-700 font-bold rounded-full border border-amber-300 text-xs shadow-sm"><i class="fa-solid fa-crown text-amber-500 mr-0.5"></i>1</span>`;
                else if (index === 1) rankBadge = `<span class="inline-flex items-center justify-center w-7 h-7 bg-slate-200 text-slate-700 font-bold rounded-full border border-slate-300 text-xs">2</span>`;
                else if (index === 2) rankBadge = `<span class="inline-flex items-center justify-center w-7 h-7 bg-amber-700/10 text-amber-800 font-bold rounded-full border border-amber-600/30 text-xs">3</span>`;

                tr.innerHTML = `
                    <td class="py-3.5 px-4 text-center">${rankBadge}</td>
                    <td class="py-3.5 px-6">
                        <div class="font-bold text-slate-900">${escapeHtml(team.name)}</div>
                        <div class="text-xs text-slate-500">${escapeHtml(team.institution)}</div>
                    </td>
                    <td class="py-3.5 px-4 text-center font-medium">${team.debates}</td>
                    <td class="py-3.5 px-4 text-center font-semibold text-emerald-700">${team.wins}</td>
                    <td class="py-3.5 px-4 text-center font-semibold text-rose-600">${team.losses}</td>
                    <td class="py-3.5 px-4 text-center font-medium text-slate-700 bg-slate-100/50">${team.speakerPoints.toFixed(1)}</td>
                    <td class="py-3.5 px-4 text-center font-extrabold text-blue-900 bg-blue-50/80">${team.totalPoints.toFixed(1)}</td>
                    ${isAdmin ? `
                    <td class="py-3.5 px-4 text-center space-x-1">
                        <button onclick="editTeam('${team.id}')" class="p-1.5 text-blue-600 hover:text-blue-800 hover:bg-blue-100 rounded transition-colors" title="Editar"><i class="fa-solid fa-pen-to-square"></i></button>
                        <button onclick="deleteTeam('${team.id}')" class="p-1.5 text-rose-600 hover:text-rose-800 hover:bg-rose-100 rounded transition-colors" title="Eliminar"><i class="fa-solid fa-trash"></i></button>
                    </td>
                    ` : ''}
                `;
                teamsTable.appendChild(tr);
            });

            // Speakers Data
            const speakers = filteredTeams
                .filter(t => t.bestSpeaker && t.bestSpeaker.trim() !== '')
                .map(t => ({
                    name: t.bestSpeaker,
                    team: t.name,
                    institution: t.institution,
                    points: t.speakerPoints
                }))
                .sort((a, b) => b.points - a.points);

            speakersTable.innerHTML = '';
            speakers.forEach((sp, idx) => {
                const tr = document.createElement('tr');
                tr.className = idx % 2 === 0 ? 'bg-white hover:bg-blue-50/40 transition-colors' : 'bg-slate-50/80 hover:bg-blue-50/40 transition-colors';
                tr.innerHTML = `
                    <td class="py-3.5 px-4 text-center font-bold text-slate-500">${idx + 1}</td>
                    <td class="py-3.5 px-6 font-semibold text-slate-900">${escapeHtml(sp.name)}</td>
                    <td class="py-3.5 px-6 text-xs text-slate-600">${escapeHtml(sp.team)} <span class="text-slate-400">(${escapeHtml(sp.institution)})</span></td>
                    <td class="py-3.5 px-4 text-center font-bold text-blue-800 bg-blue-50/50">${sp.points.toFixed(1)}</td>
                `;
                speakersTable.appendChild(tr);
            });
        }

        function filterData() {
            renderTables();
        }

        function openAddModal() {
            document.getElementById('modalTitle').textContent = 'Añadir Nuevo Equipo';
            document.getElementById('editingTeamId').value = '';
            document.getElementById('teamForm').reset();
            document.getElementById('teamModal').classList.remove('hidden');
        }

        function closeModal() {
            document.getElementById('teamModal').classList.add('hidden');
        }

        function editTeam(id) {
            const team = teams.find(t => t.id === id);
            if (!team) return;

            document.getElementById('modalTitle').textContent = 'Editar Equipo';
            document.getElementById('editingTeamId').value = team.id;
            document.getElementById('teamName').value = team.name;
            document.getElementById('institution').value = team.institution;
            document.getElementById('debatesCount').value = team.debates;
            document.getElementById('winsCount').value = team.wins;
            document.getElementById('lossesCount').value = team.losses;
            document.getElementById('speakerPoints').value = team.speakerPoints;
            document.getElementById('totalPoints').value = team.totalPoints;
            document.getElementById('bestSpeaker').value = team.bestSpeaker || '';

            document.getElementById('teamModal').classList.remove('hidden');
        }

        function deleteTeam(id) {
            if (confirm('¿Estás seguro de que deseas eliminar este equipo?')) {
                teams = teams.filter(t => t.id !== id);
                saveToLocalStorage();
                renderTables();
            }
        }

        function handleFormSubmit(e) {
            e.preventDefault();

            const editingId = document.getElementById('editingTeamId').value;
            const newTeamData = {
                id: editingId || Date.now().toString(),
                name: document.getElementById('teamName').value.trim(),
                institution: document.getElementById('institution').value.trim(),
                debates: parseInt(document.getElementById('debatesCount').value) || 0,
                wins: parseInt(document.getElementById('winsCount').value) || 0,
                losses: parseInt(document.getElementById('lossesCount').value) || 0,
                speakerPoints: parseFloat(document.getElementById('speakerPoints').value) || 0,
                totalPoints: parseFloat(document.getElementById('totalPoints').value) || 0,
                bestSpeaker: document.getElementById('bestSpeaker').value.trim()
            };

            if (editingId) {
                teams = teams.map(t => t.id === editingId ? newTeamData : t);
            } else {
                teams.push(newTeamData);
            }

            saveToLocalStorage();
            renderTables();
            closeModal();
        }

        function exportJSON() {
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(teams, null, 2));
            const downloadAnchor = document.createElement('a');
            downloadAnchor.setAttribute("href", dataStr);
            downloadAnchor.setAttribute("download", "ranking_liga_debate.json");
            document.body.appendChild(downloadAnchor);
            downloadAnchor.click();
            downloadAnchor.remove();
        }

        function exportCSV() {
            let csvContent = "data:text/csv;charset=utf-8,";
            csvContent += "Nombre,Institucion,Debates,Ganados,Perdidos,PuntosOrador,PuntosTotales,OradorDestacado\n";

            teams.forEach(t => {
                csvContent += `"${t.name}","${t.institution}",${t.debates},${t.wins},${t.losses},${t.speakerPoints},${t.totalPoints},"${t.bestSpeaker || ''}"\n`;
            });

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", "ranking_liga_debate.csv");
            document.body.appendChild(link);
            link.click();
            link.remove();
        }

        function importJSON(event) {
            const file = event.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    const imported = JSON.parse(e.target.result);
                    if (Array.isArray(imported)) {
                        teams = imported;
                        saveToLocalStorage();
                        renderTables();
                        alert('Datos importados con éxito.');
                    } else {
                        alert('Formato de archivo inválido.');
                    }
                } catch (err) {
                    alert('Error al leer el archivo JSON.');
                }
            };
            reader.readAsText(file);
        }

        function escapeHtml(text) {
            if (!text) return '';
            return text
                .replace(/&/g, "&amp;")
                .replace(/</g, "&lt;")
                .replace(/>/g, "&gt;")
                .replace(/"/g, "&quot;")
                .replace(/'/g, "&#039;");
        }
    </script>
</body>
</html>
