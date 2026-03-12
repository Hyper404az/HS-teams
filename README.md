<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>HS Financeiro - Master Pro Mobile</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        body { 
            background-color: #050508; 
            font-family: 'Inter', sans-serif; 
            color: #e5e7eb; 
            overflow-x: hidden; 
            width: 100vw;
        }
        .card-pro { 
            background: rgba(17, 24, 39, 0.7); 
            backdrop-filter: blur(12px); 
            border: 1px solid rgba(255,255,255,0.08); 
            border-radius: 24px; 
        }
        .custom-scroll::-webkit-scrollbar { width: 0px; }
        
        /* Resolve o bug do zoom e redimensionamento no mobile */
        input, select { font-size: 16px !important; } 
        
        input[type="number"]::-webkit-inner-spin-button { display: none; }
        .btn-card-active { background: #8b5cf6 !important; color: white !important; box-shadow: 0 0 10px rgba(139, 92, 246, 0.4); }
        
        /* Ajuste para evitar o bounce vertical no mobile */
        html, body { height: 100%; position: relative; }
    </style>
</head>
<body class="pb-10">

    <div id="auth-screen" class="min-h-screen flex items-center justify-center p-6">
        <div class="card-pro p-8 w-full max-w-sm text-center shadow-2xl">
            <div class="inline-flex p-5 bg-orange-500 rounded-[2rem] mb-6 text-4xl text-black font-black">HS</div>
            
            <div id="login-box" class="space-y-4">
                <h2 class="text-xl font-black uppercase tracking-widest mb-4">Entrar</h2>
                <input id="l-user" type="text" placeholder="Usuário" class="w-full bg-gray-900/50 border border-white/5 rounded-2xl p-4 outline-none focus:ring-2 focus:ring-orange-500">
                <input id="l-pass" type="password" placeholder="Senha" class="w-full bg-gray-900/50 border border-white/5 rounded-2xl p-4 outline-none focus:ring-2 focus:ring-orange-500">
                <button onclick="login()" class="w-full bg-orange-500 text-black py-4 rounded-2xl font-black uppercase text-xs tracking-widest">Acessar</button>
                <p class="text-[10px] text-gray-500">Novo aqui? <button onclick="toggleAuth(true)" class="text-orange-500 font-bold">Criar Conta</button></p>
            </div>

            <div id="register-box" class="hidden space-y-4">
                <h2 class="text-xl font-black uppercase tracking-widest mb-4 text-blue-400">Cadastro</h2>
                <input id="r-user" type="text" placeholder="Usuário" class="w-full bg-gray-900/50 border border-white/5 rounded-2xl p-4 outline-none focus:ring-2 focus:ring-blue-500">
                <input id="r-pass" type="password" placeholder="Senha" class="w-full bg-gray-900/50 border border-white/5 rounded-2xl p-4 outline-none focus:ring-2 focus:ring-blue-500">
                <button onclick="cadastrar()" class="w-full bg-blue-600 text-white py-4 rounded-2xl font-black uppercase text-xs tracking-widest">Cadastrar</button>
                <button onclick="toggleAuth(false)" class="text-[10px] text-gray-500 underline">Voltar</button>
            </div>
        </div>
    </div>

    <div id="dashboard" class="hidden">
        <header class="p-4 border-b border-white/5 sticky top-0 bg-[#050508]/90 backdrop-blur-xl z-50">
            <div class="max-w-6xl mx-auto flex justify-between items-center">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 bg-orange-500 rounded-xl flex items-center justify-center text-black font-black">HS</div>
                    <span id="display-user" class="font-black text-sm"></span>
                </div>
                <button onclick="logout()" class="w-10 h-10 flex items-center justify-center text-red-400 bg-red-400/10 rounded-xl"><i class="fa-solid fa-power-off"></i></button>
            </div>
        </header>

        <main class="max-w-6xl mx-auto p-4 space-y-4">
            
            <div class="grid grid-cols-2 lg:grid-cols-4 gap-3">
                <div class="card-pro p-4 border-b-2 border-emerald-500/50">
                    <p class="text-[9px] font-black text-gray-500 uppercase mb-1">Receita</p>
                    <h3 id="res-receita" class="text-base md:text-lg font-black truncate text-emerald-400">R$ 0</h3>
                </div>
                <div class="card-pro p-4 border-b-2 border-red-500/50">
                    <p class="text-[9px] font-black text-gray-500 uppercase mb-1">Total Gastos</p>
                    <h3 id="res-gastos" class="text-base md:text-lg font-black text-red-400 truncate">R$ 0</h3>
                </div>
                <div class="card-pro p-4 border-b-2 border-purple-500/50">
                    <p class="text-[9px] font-black text-gray-500 uppercase mb-1">Fatura Cartão</p>
                    <h3 id="res-cartao" class="text-base md:text-lg font-black text-purple-400 truncate">R$ 0</h3>
                </div>
                <div class="card-pro p-4 border-b-2 border-blue-500/50">
                    <p class="text-[9px] font-black text-gray-500 uppercase mb-1">Saldo Final</p>
                    <h3 id="res-saldo" class="text-base md:text-lg font-black text-white truncate">R$ 0</h3>
                </div>
            </div>

            <div id="ia-insights" class="grid grid-cols-1 gap-3"></div>

            <div class="grid grid-cols-1 lg:grid-cols-3 gap-4">
                
                <div class="space-y-4 order-2 lg:order-1">
                    <section class="card-pro p-5">
                        <h4 class="text-[11px] font-black text-orange-500 uppercase mb-4 tracking-widest">💰 Ajustes</h4>
                        <div class="space-y-4">
                            <div>
                                <label class="text-[10px] text-gray-500 font-black uppercase">Receita Mensal</label>
                                <input id="in-receita" type="number" oninput="salvarBase()" class="w-full bg-white/5 rounded-xl p-3 mt-1 outline-none border border-white/5 font-black text-emerald-400">
                            </div>
                            <div>
                                <label class="text-[10px] text-gray-500 font-black uppercase">Teto Comida</label>
                                <input id="in-limite-alim" type="number" oninput="salvarBase()" class="w-full bg-white/5 rounded-xl p-3 mt-1 outline-none border border-white/5 font-black text-red-400">
                            </div>
                        </div>
                    </section>

                    <section class="card-pro p-5">
                        <h4 class="text-[11px] font-black text-blue-400 uppercase mb-4 tracking-widest">🎯 Meta</h4>
                        <div class="space-y-3">
                            <input id="meta-nome" type="text" oninput="salvarBase()" placeholder="Ex: Viagem" class="w-full bg-white/5 rounded-xl p-3 text-xs outline-none border border-white/5">
                            <input id="meta-valor" type="number" oninput="salvarBase()" placeholder="Valor R$" class="w-full bg-white/5 rounded-xl p-3 text-xs outline-none border border-white/5 font-bold">
                            <div class="bg-black/20 p-3 rounded-2xl">
                                <div class="flex justify-between text-[9px] mb-2 font-black uppercase">
                                    <span id="meta-label-display">Progresso</span>
                                    <span id="meta-perc-display">0%</span>
                                </div>
                                <div class="w-full bg-gray-800 rounded-full h-1.5">
                                    <div id="meta-bar" class="bg-blue-500 h-1.5 rounded-full" style="width: 0%"></div>
                                </div>
                            </div>
                        </div>
                    </section>
                </div>

                <div class="lg:col-span-2 space-y-4 order-1 lg:order-2">
                    <section class="card-pro p-5">
                        <div class="relative w-full mb-4">
                            <i class="fa-solid fa-search absolute left-4 top-1/2 -translate-y-1/2 text-gray-500 text-xs"></i>
                            <input type="text" id="search-input" onkeyup="renderGastos()" placeholder="Buscar..." class="w-full bg-white/5 border border-white/5 rounded-2xl py-3 pl-10 pr-4 text-xs outline-none">
                        </div>

                        <div class="p-4 bg-white/5 rounded-[1.5rem] border border-white/5 mb-6">
                            <div class="flex gap-2 mb-2">
                                <input id="add-nome" type="text" placeholder="Descrição" class="flex-1 bg-transparent p-2 text-sm outline-none font-bold">
                                <button onclick="adicionarGasto()" class="w-10 h-10 bg-white text-black rounded-xl flex items-center justify-center"><i class="fa-solid fa-plus"></i></button>
                            </div>
                            <div class="grid grid-cols-2 gap-2">
                                <select id="add-cat" class="bg-gray-900 rounded-xl p-2 text-[10px] font-black uppercase text-gray-400">
                                    <option value="Fixo">🏠 Contas Fixas</option>
                                    <option value="Alimentação">🛒 Alimentação</option>
                                    <option value="Transporte">🚗 Transporte</option>
                                    <option value="Lazer">🍿 Lazer</option>
                                    <option value="Outros">⚙️ Outros</option>
                                </select>
                                <div class="flex items-center justify-center bg-gray-900 rounded-xl text-[10px] font-black uppercase text-purple-400 gap-2">
                                    <i class="fa-solid fa-credit-card"></i> Cartão?
                                    <input type="checkbox" id="add-is-card" class="w-4 h-4 accent-purple-500">
                                </div>
                            </div>
                        </div>

                        <div id="lista-gastos" class="space-y-3 max-h-[400px] overflow-y-auto custom-scroll"></div>
                    </section>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div class="card-pro p-5 h-44"><canvas id="chart-evo"></canvas></div>
                        <div class="card-pro p-5 h-44"><canvas id="chart-rank"></canvas></div>
                    </div>
                </div>

            </div>
        </main>
    </div>

    <script>
        let users = JSON.parse(localStorage.getItem('HS_MOBILE_FIX_V1')) || [];
        let curUser = null;
        let cEvo = null, cRank = null;

        function toggleAuth(isReg) {
            document.getElementById('login-box').classList.toggle('hidden', isReg);
            document.getElementById('register-box').classList.toggle('hidden', !isReg);
        }

        function cadastrar() {
            const u = document.getElementById('r-user').value.trim();
            const p = document.getElementById('r-pass').value.trim();
            if(!u || !p) return;
            if(users.find(x => x.u === u)) return alert("Já existe!");
            users.push({ u, p, receita: 0, limiteAlim: 800, metaNome: "Reserva", metaValor: 1000, gastos: [] });
            localStorage.setItem('HS_MOBILE_FIX_V1', JSON.stringify(users));
            alert("Cadastrado!"); toggleAuth(false);
        }

        function login() {
            const u = document.getElementById('l-user').value.trim();
            const p = document.getElementById('l-pass').value.trim();
            const user = users.find(x => x.u === u && x.p === p);
            if(!user) return alert("Erro!");
            curUser = user;
            sessionStorage.setItem('HS_USER', u);
            initApp();
        }

        function logout() { sessionStorage.clear(); location.reload(); }

        function initApp() {
            document.getElementById('auth-screen').classList.add('hidden');
            document.getElementById('dashboard').classList.remove('hidden');
            document.getElementById('display-user').textContent = curUser.u;
            document.getElementById('in-receita').value = curUser.receita;
            document.getElementById('in-limite-alim').value = curUser.limiteAlim;
            document.getElementById('meta-nome').value = curUser.metaNome;
            document.getElementById('meta-valor').value = curUser.metaValor;
            renderGastos();
            atualizarTudo();
        }

        function atualizarTudo() {
            const totalG = curUser.gastos.reduce((a, b) => a + (Number(b.v) || 0), 0);
            const totalC = curUser.gastos.filter(g => g.cartao).reduce((a, b) => a + (Number(b.v) || 0), 0);
            const saldo = (curUser.receita || 0) - totalG;

            document.getElementById('res-receita').textContent = format(curUser.receita);
            document.getElementById('res-gastos').textContent = format(totalG);
            document.getElementById('res-cartao').textContent = format(totalC);
            document.getElementById('res-saldo').textContent = format(saldo);

            processarLogica(totalG, totalC, saldo);
            atualizarGraficos();
            localStorage.setItem('HS_MOBILE_FIX_V1', JSON.stringify(users));
        }

        function salvarBase() {
            curUser.receita = Number(document.getElementById('in-receita').value) || 0;
            curUser.limiteAlim = Number(document.getElementById('in-limite-alim').value) || 0;
            curUser.metaNome = document.getElementById('meta-nome').value;
            curUser.metaValor = Number(document.getElementById('meta-valor').value) || 0;
            atualizarTudo();
        }

        function adicionarGasto() {
            const n = document.getElementById('add-nome').value;
            const c = document.getElementById('add-cat').value;
            const isCard = document.getElementById('add-is-card').checked;
            if(!n) return;
            curUser.gastos.unshift({ id: Date.now(), n, c, v: 0, cartao: isCard });
            document.getElementById('add-nome').value = "";
            document.getElementById('add-is-card').checked = false;
            renderGastos();
            atualizarTudo();
        }

        function editarGasto(id, val) {
            const g = curUser.gastos.find(x => x.id === id);
            if(g) g.v = Number(val) || 0;
            atualizarTudo();
        }

        function toggleGastoCartao(id) {
            const g = curUser.gastos.find(x => x.id === id);
            if(g) g.cartao = !g.cartao;
            renderGastos();
            atualizarTudo();
        }

        function deletarGasto(id) {
            curUser.gastos = curUser.gastos.filter(x => x.id !== id);
            renderGastos();
            atualizarTudo();
        }

        function renderGastos() {
            const busca = document.getElementById('search-input').value.toLowerCase();
            const container = document.getElementById('lista-gastos');
            const filtrados = curUser.gastos.filter(g => g.n.toLowerCase().includes(busca));

            container.innerHTML = filtrados.map(g => `
                <div class="flex items-center gap-2 bg-white/5 p-4 rounded-2xl border border-white/5">
                    <button onclick="deletarGasto(${g.id})" class="w-8 text-gray-600"><i class="fa-solid fa-trash-can text-[10px]"></i></button>
                    <div class="flex-1 min-w-0">
                        <p class="text-[7px] font-black text-orange-500 uppercase">${g.c}</p>
                        <p class="text-[11px] font-bold uppercase truncate text-white">${g.n}</p>
                    </div>
                    <button onclick="toggleGastoCartao(${g.id})" class="w-7 h-7 rounded-lg text-[9px] ${g.cartao ? 'btn-card-active' : 'bg-gray-800 text-gray-500'}">
                        <i class="fa-solid fa-credit-card"></i>
                    </button>
                    <div class="flex items-center bg-black/40 rounded-xl px-2 border border-white/10">
                        <input type="number" value="${g.v || ''}" oninput="editarGasto(${g.id}, this.value)" 
                            class="w-14 bg-transparent py-2 text-right outline-none text-red-400 font-black text-xs" placeholder="0">
                    </div>
                </div>
            `).join('');
        }

        function processarLogica(totalG, totalC, saldo) {
            const insBox = document.getElementById('ia-insights');
            const gastoAlim = curUser.gastos.filter(g => g.c === 'Alimentação').reduce((a, b) => a + (Number(b.v) || 0), 0);
            let html = '';

            const metaTotal = curUser.metaValor || 1;
            const percMeta = Math.min(100, (Math.max(0, saldo) / metaTotal) * 100).toFixed(0);
            document.getElementById('meta-perc-display').textContent = percMeta + '%';
            document.getElementById('meta-bar').style.width = percMeta + '%';

            if(totalC > (curUser.receita * 0.4)) html += alertCard('Cartão Alto', '40% da renda comprometida.', 'purple', 'fa-credit-card');
            if(gastoAlim > curUser.limiteAlim) html += alertCard('Comida', 'Teto excedido!', 'red', 'fa-utensils');
            
            insBox.innerHTML = html;
        }

        function alertCard(title, msg, color, icon) {
            const bg = color === 'purple' ? 'border-purple-500 text-purple-400' : 'border-red-500 text-red-400';
            return `<div class="p-3 rounded-2xl bg-gray-900 border ${bg} flex gap-3 items-center">
                <i class="fa-solid ${icon} text-xs"></i>
                <div class="text-[10px] uppercase font-black"><span>${title}:</span> <span class="text-gray-400 font-normal">${msg}</span></div>
            </div>`;
        }

        function atualizarGraficos() {
            const ctxE = document.getElementById('chart-evo').getContext('2d');
            const ctxR = document.getElementById('chart-rank').getContext('2d');
            if(cEvo) cEvo.destroy();
            if(cRank) cRank.destroy();

            cEvo = new Chart(ctxE, {
                type: 'line',
                data: {
                    labels: ['Jan', 'Fev', 'Mar', 'Agora'],
                    datasets: [{
                        data: [1500, 2400, 2100, ((curUser.receita||0) - curUser.gastos.reduce((a,b)=>a+Number(b.v),0))],
                        borderColor: '#f97316', tension: 0.5, pointRadius: 0, borderWidth: 3
                    }]
                },
                options: { maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { y: { display: false }, x: { grid: { display: false }, ticks: { color: '#444', font: { size: 8 } } } } }
            });

            const cats = {};
            curUser.gastos.forEach(g => cats[g.c] = (cats[g.c] || 0) + Number(g.v));
            cRank = new Chart(ctxR, {
                type: 'bar',
                data: {
                    labels: Object.keys(cats),
                    datasets: [{ data: Object.values(cats), backgroundColor: '#8b5cf6', borderRadius: 5 }]
                },
                options: { maintainAspectRatio: false, indexAxis: 'y', plugins: { legend: { display: false } }, scales: { y: { grid: { display: false }, ticks: { color: '#777', font: { size: 9 } } }, x: { display: false } } }
            });
        }

        function format(v) { return (v || 0).toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' }); }

        window.onload = () => {
            const logged = sessionStorage.getItem('HS_USER');
            if(logged) { curUser = users.find(x => x.u === logged); if(curUser) initApp(); }
        };
    </script>
</body>
</html>
