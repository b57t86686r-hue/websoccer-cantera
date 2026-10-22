# websoccer<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>WebSoccer Manager - Cantera Pro</title>
    <style>
        :root {
            --bg: #0f172a;
            --card: #1e293b;
            --border: #334155;
            --accent: #10b981;
            --blue: #3b82f6;
            --amber: #f59e0b;
            --red: #ef4444;
            --text: #f8fafc;
            --muted: #94a3b8;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: var(--bg); color: var(--text); padding-bottom: 75px; user-select: none; }

        /* Header */
        header { background: var(--card); padding: 14px 16px; border-bottom: 1px solid var(--border); position: sticky; top: 0; z-index: 50; display: flex; flex-direction: column; gap: 10px; }
        .top-bar { display: flex; justify-content: space-between; align-items: center; }
        .title { font-size: 1.1rem; font-weight: 800; color: #fff; display: flex; align-items: center; gap: 6px; }
        .badge-budget { background: rgba(16, 185, 129, 0.15); color: var(--accent); padding: 4px 10px; border-radius: 20px; font-weight: 700; font-size: 0.85rem; border: 1px solid var(--accent); }
        .btn-advance { background: linear-gradient(135deg, #10b981, #059669); color: white; border: none; padding: 10px; border-radius: 10px; font-weight: 700; font-size: 0.95rem; width: 100%; cursor: pointer; box-shadow: 0 4px 12px rgba(16, 185, 129, 0.25); }
        .btn-advance:active { transform: scale(0.98); }

        /* Container */
        .container { padding: 14px; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }

        /* Cards & Grids */
        .card { background: var(--card); border-radius: 14px; padding: 14px; margin-bottom: 14px; border: 1px solid var(--border); }
        .card-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; }
        .card-title { font-size: 1rem; font-weight: 700; color: #fff; }

        .grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin-bottom: 14px; }
        .stat-card { background: rgba(15, 23, 42, 0.6); padding: 10px; border-radius: 10px; text-align: center; border: 1px solid var(--border); }
        .stat-num { font-size: 1.2rem; font-weight: 800; color: var(--accent); }
        .stat-lbl { font-size: 0.68rem; color: var(--muted); margin-top: 2px; }

        /* Players List */
        .player-card { display: flex; justify-content: space-between; align-items: center; padding: 10px 0; border-bottom: 1px solid var(--border); }
        .player-card:last-child { border-bottom: none; }
        .p-name { font-weight: 700; font-size: 0.9rem; display: flex; align-items: center; gap: 6px; }
        .p-meta { font-size: 0.75rem; color: var(--muted); margin-top: 2px; }
        .pos-badge { font-size: 0.65rem; padding: 2px 5px; border-radius: 4px; font-weight: 800; background: #334155; color: #fff; }

        /* Bars */
        .bar-container { display: flex; align-items: center; gap: 6px; margin-top: 3px; }
        .bar-bg { width: 70px; height: 5px; background: #334155; border-radius: 3px; overflow: hidden; }
        .bar-fill { height: 100%; background: var(--blue); border-radius: 3px; }
        .bar-prog { background: var(--amber); }

        /* Buttons */
        .btn { background: #334155; color: white; border: none; padding: 6px 10px; border-radius: 8px; font-size: 0.75rem; font-weight: 600; cursor: pointer; }
        .btn-sm { padding: 4px 8px; font-size: 0.7rem; }
        .btn-green { background: var(--accent); color: #000; }
        .btn-red { background: var(--red); color: #fff; }

        /* League Table */
        .table { width: 100%; border-collapse: collapse; font-size: 0.78rem; text-align: left; }
        .table th { color: var(--muted); padding: 6px 4px; border-bottom: 1px solid var(--border); }
        .table td { padding: 8px 4px; border-bottom: 1px solid var(--border); }
        .match-row { display: flex; justify-content: space-between; padding: 8px 0; border-bottom: 1px solid var(--border); font-size: 0.82rem; }

        /* Bottom Nav */
        nav { position: fixed; bottom: 0; left: 0; right: 0; background: var(--card); border-top: 1px solid var(--border); display: flex; height: 60px; z-index: 100; }
        .nav-item { flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center; background: none; border: none; color: var(--muted); font-size: 0.68rem; gap: 3px; }
        .nav-item.active { color: var(--accent); font-weight: 700; }
        .nav-icon { font-size: 1.2rem; }

        .log-box { background: #090d16; padding: 10px; border-radius: 8px; font-family: monospace; font-size: 0.72rem; color: #10b981; max-height: 100px; overflow-y: auto; border: 1px solid var(--border); }
    </style>
</head>
<body>

    <header>
        <div class="top-bar">
            <span class="title">⚽ Cantera Manager</span>
            <span class="badge-budget" id="budget-txt">100,000 €</span>
        </div>
        <button class="btn-advance" onclick="advanceDay()">Avanzar Día ➡️ (<span id="day-txt">Día 1</span>)</button>
    </header>

    <div class="container">
        
        <!-- PESTAÑA CANTERA -->
        <div id="tab-youth" class="tab-content active">
            <div class="grid-3">
                <div class="stat-card"><div class="stat-num" id="st-total">0</div><div class="stat-lbl">Canteranos</div></div>
                <div class="stat-card"><div class="stat-num" id="st-pot">0</div><div class="stat-lbl">Pot. Medio</div></div>
                <div class="stat-card"><div class="stat-num" id="st-ready">0</div><div class="stat-lbl">Listos Subir</div></div>
            </div>

            <div class="card">
                <div class="card-header">
                    <span class="card-title">Plantilla Juvenil U18</span>
                    <button class="btn btn-green" onclick="generateIntake()">Nuevo Intake</button>
                </div>
                <div id="youth-list"></div>
            </div>

            <div class="card">
                <div class="card-title" style="margin-bottom:8px;">Feed de Eventos</div>
                <div class="log-box" id="log-box">Gestor inicializado correctamente.</div>
            </div>
        </div>

        <!-- PESTAÑA LIGA U18 -->
        <div id="tab-league" class="tab-content">
            <div class="card">
                <div class="card-header">
                    <span class="card-title">Liga Juvenil U18 (Jornada <span id="jornada-txt">1</span>)</span>
                    <button class="btn btn-green" onclick="simulateMatchday()">Simular Jornada</button>
                </div>
                <table class="table">
                    <thead>
                        <tr><th>#</th><th>Equipo</th><th>PJ</th><th>DG</th><th>Pts</th></tr>
                    </thead>
                    <tbody id="league-body"></tbody>
                </table>
            </div>

            <div class="card">
                <div class="card-title" style="margin-bottom:8px;">Últimos Resultados</div>
                <div id="matches-list"></div>
            </div>
        </div>

        <!-- PESTAÑA PRIMER EQUIPO -->
        <div id="tab-squad" class="tab-content">
            <div class="card">
                <div class="card-title" style="margin-bottom:10px;">Primer Equipo (<span id="squad-count">0</span>/25)</div>
                <div id="squad-list"></div>
            </div>
        </div>

        <!-- PESTAÑA MERCADO & SCOUTING -->
        <div id="tab-market" class="tab-content">
            <div class="card">
                <div class="card-title" style="margin-bottom:10px;">Red de Ojeo</div>
                <div style="display:flex; gap:8px; margin-bottom:10px;">
                    <select id="scout-region" style="flex:1; padding:8px; background:#0f172a; color:#fff; border:1px solid var(--border); border-radius:8px; font-size:0.8rem;">
                        <option value="ESP">España</option>
                        <option value="ARG">Argentina</option>
                        <option value="BRA">Brasil</option>
                        <option value="FRA">Francia</option>
                    </select>
                    <button class="btn btn-green" onclick="hireScout()">Ojeador (15k €)</button>
                </div>
                <div id="scouts-list"></div>
            </div>

            <div class="card">
                <div class="card-title" style="margin-bottom:10px;">Informes de Scouting</div>
                <div id="reports-list"></div>
            </div>

            <div class="card">
                <div class="card-title" style="margin-bottom:10px;">Ofertas Recibidas</div>
                <div id="offers-list"></div>
            </div>
        </div>

    </div>

    <!-- NAVEGACIÓN INFERIOR -->
    <nav>
        <button class="nav-item active" onclick="switchTab('youth', this)"><span class="nav-icon">🌱</span>Cantera</button>
        <button class="nav-item" onclick="switchTab('league', this)"><span class="nav-icon">🏆</span>Liga U18</button>
        <button class="nav-item" onclick="switchTab('squad', this)"><span class="nav-icon">👕</span>1er Equipo</button>
        <button class="nav-item" onclick="switchTab('market', this)"><span class="nav-icon">💼</span>Mercado</button>
    </nav>

    <script>
        let game = {
            day: 1,
            jornada: 1,
            budget: 100000,
            youth: [],
            squad: [],
            scouts: [],
            reports: [],
            offers: [],
            logs: [],
            standings: [
                { id: 1, name: 'Mi Club U18', played: 0, gf: 0, ga: 0, pts: 0, strength: 65 },
                { id: 2, name: 'Real Madrid U18', played: 0, gf: 0, ga: 0, pts: 0, strength: 75 },
                { id: 3, name: 'Barcelona U18', played: 0, gf: 0, ga: 0, pts: 0, strength: 74 },
                { id: 4, name: 'Atlético U18', played: 0, gf: 0, ga: 0, pts: 0, strength: 70 },
                { id: 5, name: 'Valencia U18', played: 0, gf: 0, ga: 0, pts: 0, strength: 68 },
                { id: 6, name: 'Sevilla U18', played: 0, gf: 0, ga: 0, pts: 0, strength: 67 }
            ],
            lastMatches: []
        };

        const dbNames = {
            ESP: { f: ['Lucas','Mateo','Hugo','Pablo','Álvaro','Pedri','Gavi','Lamine','Fermín'], l: ['García','Martínez','López','Sánchez','Pérez','Gómez','Torres'] },
            ARG: { f: ['Julián','Lautaro','Enzo','Alexis','Thiago','Facundo'], l: ['Álvarez','Martínez','Fernández','Mac Allister','Almada'] },
            BRA: { f: ['Vinícius','Rodrygo','Endrick','Vitor','Gabriel'], l: ['Júnior','Silva','Roque','Jesus','Lima'] },
            FRA: { f: ['Kylian','Eduardo','Aurélien','Bradley','Mathys'], l: ['Mbappé','Camavinga','Tchouaméni','Barcola','Tel'] }
        };

        const posList = ['POR', 'DEF', 'MED', 'DEL'];

        function init() {
            const saved = localStorage.getItem('websoccer_v2');
            if (saved) game = JSON.parse(saved);
            else generateIntake();
            render();
        }

        function save() { localStorage.setItem('websoccer_v2', JSON.stringify(game)); }

        function log(msg) {
            game.logs.unshift(`[Día ${game.day}] ${msg}`);
            if (game.logs.length > 15) game.logs.pop();
        }

        function getIdentity(reg = 'ESP') {
            const pool = dbNames[reg] || dbNames['ESP'];
            return {
                name: `${pool.f[Math.floor(Math.random()*pool.f.length)]} ${pool.l[Math.floor(Math.random()*pool.l.length)]}`,
                nat: reg
            };
        }

        function advanceDay() {
            game.day++;

            // Progresión Cantera
            game.youth.forEach(p => {
                p.progression = Math.min(1.0, p.progression + 0.01 + Math.random() * 0.015);
            });

            // Scouting
            game.scouts.forEach(s => {
                if (Math.random() < 0.35) {
                    const iden = getIdentity(s.region);
                    const pot = Math.floor(65 + Math.random() * 30);
                    const price = Math.floor((pot - 50) * 1200 + Math.random() * 4000);
                    game.reports.push({
                        id: Date.now() + Math.random(),
                        name: iden.name,
                        nat: iden.nat,
                        pos: posList[Math.floor(Math.random()*posList.length)],
                        age: Math.floor(15 + Math.random() * 3),
                        potential: pot,
                        price: price
                    });
                    log(`Informe de ${s.name}: ${iden.name} (${pot} Pot).`);
                }
            });

            // Ofertas IA
            if (Math.random() < 0.25 && game.youth.length > 0) {
                const target = game.youth[Math.floor(Math.random() * game.youth.length)];
                if (target.potential >= 75) {
                    const offerVal = Math.floor((target.potential - 50) * 2500 + Math.random() * 8000);
                    game.offers.push({ id: Date.now(), player: target, amount: offerVal, club: 'Club Rival' });
                    log(`Oferta de ${offerVal.toLocaleString()} € por ${target.name}.`);
                }
            }

            save();
            render();
        }

        function generateIntake() {
            for (let i = 0; i < 3; i++) {
                const iden = getIdentity();
                game.youth.push({
                    id: Date.now() + i,
                    name: iden.name,
                    nat: iden.nat,
                    pos: posList[Math.floor(Math.random() * posList.length)],
                    age: Math.floor(15 + Math.random() * 3),
                    strength: Math.floor(45 + Math.random() * 18),
                    potential: Math.floor(60 + Math.random() * 35),
                    progression: 0.05
                });
            }
            log("Nuevos canteranos han llegado al club.");
            save(); render();
        }

        function trainPlayer(id) {
            const p = game.youth.find(x => x.id === id);
            if (p) {
                p.progression = Math.min(1.0, p.progression + 0.12);
                p.strength = Math.min(p.potential, p.strength + (Math.random() < 0.4 ? 1 : 0));
                save(); render();
            }
        }

        function promotePlayer(id) {
            const idx = game.youth.findIndex(x => x.id === id);
            if (idx !== -1) {
                if (game.squad.length >= 25) return alert("Plantilla llena (máx 25).");
                const p = game.youth[idx];
                p.strength += 4;
                game.squad.push(p);
                game.youth.splice(idx, 1);
                log(`¡${p.name} subió al primer equipo!`);
                save(); render();
            }
        }

        function simulateMatchday() {
            game.lastMatches = [];
            // Fuerza media del equipo basándonos en la cantera
            const avgYouthStr = game.youth.length ? game.youth.reduce((a,b)=>a+b.strength,0)/game.youth.length : 50;
            game.standings.find(t => t.id === 1).strength = Math.round(avgYouthStr + 15);

            for (let i = 0; i < game.standings.length; i += 2) {
                const t1 = game.standings[i];
                const t2 = game.standings[i+1];
                if (!t2) break;

                const g1 = Math.floor(Math.random() * (t1.strength / 20));
                const g2 = Math.floor(Math.random() * (t2.strength / 20));

                t1.played++; t2.played++;
                t1.gf += g1; t1.ga += g2;
                t2.gf += g2; t2.ga += g1;

                if (g1 > g2) t1.pts += 3;
                else if (g2 > g1) t2.pts += 3;
                else { t1.pts += 1; t2.pts += 1; }

                game.lastMatches.push(`${t1.name} ${g1} - ${g2} ${t2.name}`);
            }

            game.standings.sort((a,b) => b.pts - a.pts || (b.gf - b.ga) - (a.gf - a.ga));
            game.jornada++;
            log(`Jornada ${game.jornada - 1} simulada.`);
            save(); render();
        }

        function hireScout() {
            if (game.budget < 15000) return alert("Faltan fondos (15,000 €).");
            if (game.scouts.length >= 3) return alert("Máximo 3 ojeadores.");
            const reg = document.getElementById('scout-region').value;
            const iden = getIdentity(reg);
            game.budget -= 15000;
            game.scouts.push({ id: Date.now(), name: iden.name, region: reg });
            log(`Ojeador ${iden.name} contratado.`);
            save(); render();
        }

        function signProspect(id) {
            const idx = game.reports.findIndex(r => r.id === id);
            if (idx !== -1) {
                const r = game.reports[idx];
                if (game.budget < r.price) return alert("Sin dinero suficiente.");
                game.budget -= r.price;
                game.youth.push({ id: Date.now(), name: r.name, nat: r.nat, pos: r.pos, age: r.age, strength: 50, potential: r.potential, progression: 0.1 });
                game.reports.splice(idx, 1);
                log(`Fichado ${r.name} por ${r.price.toLocaleString()} €.`);
                save(); render();
            }
        }

        function acceptOffer(id) {
            const idx = game.offers.findIndex(o => o.id === id);
            if (idx !== -1) {
                const o = game.offers[idx];
                game.budget += o.amount;
                game.youth = game.youth.filter(p => p.id !== o.player.id);
                game.offers.splice(idx, 1);
                log(`Vendido ${o.player.name} por ${o.amount.toLocaleString()} €.`);
                save(); render();
            }
        }

        function rejectOffer(id) { game.offers = game.offers.filter(o => o.id !== id); save(); render(); }

        function render() {
            document.getElementById('budget-txt').innerText = `${game.budget.toLocaleString()} €`;
            document.getElementById('day-txt').innerText = `Día ${game.day}`;
            document.getElementById('jornada-txt').innerText = game.jornada;

            document.getElementById('st-total').innerText = game.youth.length;
            document.getElementById('st-pot').innerText = game.youth.length ? Math.round(game.youth.reduce((a,b)=>a+b.potential,0)/game.youth.length) : 0;
            document.getElementById('st-ready').innerText = game.youth.filter(p => p.progression >= 0.8).length;

            // Lista Cantera
            document.getElementById('youth-list').innerHTML = game.youth.map(p => `
                <div class="player-card">
                    <div>
                        <div class="p-name">${p.name} <span class="pos-badge">${p.pos}</span></div>
                        <div class="p-meta">Edad: ${p.age} | Str: ${p.strength} | Nac: ${p.nat}</div>
                        <div class="bar-container">
                            <span class="p-meta">Pot: ${p.potential}</span>
                            <div class="bar-bg"><div class="bar-fill" style="width:${p.potential}%"></div></div>
                            <span class="p-meta">Prg: ${Math.round(p.progression*100)}%</span>
                            <div class="bar-bg"><div class="bar-fill bar-prog" style="width:${p.progression*100}%"></div></div>
                        </div>
                    </div>
                    <div style="display:flex; gap:4px;">
                        <button class="btn btn-sm" onclick="trainPlayer(${p.id})">Entrenar</button>
                        ${p.progression >= 0.8 ? `<button class="btn btn-sm btn-green" onclick="promotePlayer(${p.id})">Subir</button>` : ''}
                    </div>
                </div>
            `).join('') || '<p style="color:var(--muted); font-size:0.8rem;">Sin canteranos.</p>';

            // Tabla Liga
            document.getElementById('league-body').innerHTML = game.standings.map((t, i) => `
                <tr>
                    <td>${i+1}</td>
                    <td><strong>${t.name}</strong></td>
                    <td>${t.played}</td>
                    <td>${t.gf - t.ga}</td>
                    <td><strong>${t.pts}</strong></td>
                </tr>
            `).join('');

            document.getElementById('matches-list').innerHTML = game.lastMatches.map(m => `<div class="match-row"><span>${m}</span></div>`).join('') || '<p style="color:var(--muted); font-size:0.75rem;">Aún no hay partidos.</p>';

            // Primer Equipo
            document.getElementById('squad-count').innerText = game.squad.length;
            document.getElementById('squad-list').innerHTML = game.squad.map(p => `
                <div class="player-card">
                    <div>
                        <div class="p-name">${p.name} <span class="pos-badge">${p.pos}</span></div>
                        <div class="p-meta">Edad: ${p.age} | Fuerza: ${p.strength} (Cantera)</div>
                    </div>
                </div>
            `).join('') || '<p style="color:var(--muted); font-size:0.8rem;">Sin promociones en el primer equipo.</p>';

            // Ojeadores & Ofertas
            document.getElementById('scouts-list').innerHTML = game.scouts.map(s => `<div class="player-card"><div><div class="p-name">${s.name}</div><div class="p-meta">Región: ${s.region}</div></div></div>`).join('') || '<p style="color:var(--muted); font-size:0.8rem;">Sin ojeadores.</p>';

            document.getElementById('reports-list').innerHTML = game.reports.map(r => `
                <div class="player-card">
                    <div>
                        <div class="p-name">${r.name} (${r.nat}) <span class="pos-badge">${r.pos}</span></div>
                        <div class="p-meta">Edad: ${r.age} | Pot: ${r.potential} | Coste: ${r.price.toLocaleString()} €</div>
                    </div>
                    <button class="btn btn-sm btn-green" onclick="signProspect(${r.id})">Fichar</button>
                </div>
            `).join('') || '<p style="color:var(--muted); font-size:0.8rem;">Sin informes de scouting.</p>';

            document.getElementById('offers-list').innerHTML = game.offers.map(o => `
                <div class="player-card">
                    <div>
                        <div class="p-name">${o.player.name}</div>
                        <div class="p-meta">Oferta: ${o.amount.toLocaleString()} € de ${o.club}</div>
                    </div>
                    <div style="display:flex; gap:4px;">
                        <button class="btn btn-sm btn-green" onclick="acceptOffer(${o.id})">Sí</button>
                        <button class="btn btn-sm btn-red" onclick="rejectOffer(${o.id})">No</button>
                    </div>
                </div>
            `).join('') || '<p style="color:var(--muted); font-size:0.8rem;">Sin ofertas pendientes.</p>';

            document.getElementById('log-box').innerHTML = game.logs.join('<br>');
        }

        function switchTab(id, btn) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.nav-item').forEach(b => b.classList.remove('active'));
            document.getElementById(`tab-${id}`).classList.add('active');
            btn.classList.add('active');
        }

        window.onload = init;
    </script>
</body>
</html>
