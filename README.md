<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SISTEMA V2.0 - PREMIUM</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
    <style>
        :root {
            --neon-pink: #ff007b;
            --neon-glow: rgba(255, 0, 123, 0.6);
            --bg-dark: #050505;
            --card-bg: rgba(15, 15, 18, 0.98);
            --success-green: #25d366;
        }

        * { box-sizing: border-box; user-select: none; font-family: 'Inter', sans-serif; -webkit-tap-highlight-color: transparent; }

        body {
            background-color: var(--bg-dark);
            color: #fff; margin: 0;
            display: flex; flex-direction: column; align-items: center;
            min-height: 100vh; overflow-x: hidden;
        }

        .particles {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: radial-gradient(circle, #ff007b 0.6px, transparent 0.6px);
            background-size: 30px 30px; opacity: 0.1; z-index: -1;
        }

        /* TELA DE LOGIN */
        #loginPage {
            width: 90%; max-width: 380px; padding: 35px 25px; margin-top: 70px;
            background: var(--card-bg); border: 1px solid rgba(255, 0, 123, 0.2);
            border-radius: 24px; text-align: center; box-shadow: 0 10px 40px rgba(0,0,0,0.9);
        }

        .error-msg {
            display: none; background: rgba(255, 0, 0, 0.1);
            border: 1px solid rgba(255, 68, 68, 0.4); color: #ff6666;
            padding: 14px; border-radius: 12px; font-size: 11px;
            margin-bottom: 20px; line-height: 1.4;
        }

        .input-box { position: relative; margin-bottom: 15px; }
        .input-box i { position: absolute; left: 15px; top: 16px; color: var(--neon-pink); opacity: 0.8; }
        .input-box input {
            width: 100%; background: #0c0c0e; border: 1px solid #1a1a1c;
            padding: 16px 15px 16px 45px; border-radius: 12px; color: #fff;
            outline: none; font-size: 14px; transition: 0.3s;
        }

        .btn-login {
            width: 100%; background: var(--neon-pink); color: #fff;
            border: none; padding: 18px; border-radius: 12px;
            font-weight: 800; cursor: pointer; text-transform: uppercase;
            box-shadow: 0 5px 20px var(--neon-glow); margin-bottom: 15px;
        }

        /* PAINEL PRINCIPAL */
        #mainApp { display: none; width: 95%; max-width: 420px; padding: 20px; padding-bottom: 140px; }

        .header { text-align: center; margin-bottom: 30px; }
        .header h1 { font-size: 30px; color: var(--neon-pink); margin: 0; font-weight: 900; text-shadow: 0 0 15px var(--neon-glow); }
        .header p { font-size: 10px; letter-spacing: 4px; color: #555; margin-top: 5px; font-weight: bold; }

        .tabs { display: flex; gap: 10px; margin-bottom: 25px; }
        .tab-btn {
            flex: 1; padding: 15px; border-radius: 14px; border: none;
            background: #111114; color: #666; font-weight: 800; font-size: 12px; cursor: pointer;
            display: flex; align-items: center; justify-content: center; gap: 8px;
        }
        .tab-btn.active { background: var(--neon-pink); color: #fff; box-shadow: 0 0 20px var(--neon-glow); }

        .content-section { display: none; }
        .content-section.active { display: block; animation: slideUp 0.4s ease; }

        .card {
            background: linear-gradient(135deg, rgba(255, 0, 128, 0.05) 0%, #080808 100%);
            border: 1px solid #1a1a1c; border-radius: 20px;
            padding: 20px; margin-bottom: 15px;
        }
        .card.active { border-color: var(--neon-pink); box-shadow: 0 0 25px rgba(255, 0, 123, 0.15); }

        .card-main { display: flex; align-items: center; justify-content: space-between; }
        .card-icon { 
            width: 48px; height: 48px; background: rgba(255, 0, 123, 0.1); 
            border-radius: 12px; display: flex; align-items: center; justify-content: center; 
            color: var(--neon-pink); font-size: 20px; 
        }
        
        .card-text { flex: 1; margin-left: 15px; }
        .card-text h3 { margin: 0; font-size: 17px; color: #fff; font-weight: 700; }
        .card-text p { margin: 5px 0 0; font-size: 11px; color: #666; line-height: 1.4; }

        .status-dot { font-size: 9px; margin-top: 15px; display: flex; align-items: center; gap: 6px; color: #333; font-weight: 800; text-transform: uppercase; }
        .active .status-dot { color: var(--neon-pink); }

        .footer { position: fixed; bottom: 0; left: 0; width: 100%; padding: 25px; background: linear-gradient(transparent, #000 50%); text-align: center; }
        .btn-inject {
            width: 100%; max-width: 380px; padding: 20px; border-radius: 16px; border: none;
            background: #151518; color: #333; font-weight: 900; font-size: 15px;
        }
        .btn-inject.ready { background: var(--neon-pink); color: #fff; box-shadow: 0 0 30px var(--neon-glow); cursor: pointer; }

        .switch { position: relative; width: 42px; height: 24px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #222; transition: .4s; border-radius: 34px; }
        .slider:before { position: absolute; content: ""; height: 16px; width: 16px; left: 4px; bottom: 4px; background-color: #fff; transition: .4s; border-radius: 50%; }
        input:checked + .slider { background-color: var(--neon-pink); }
        input:checked + .slider:before { transform: translateX(18px); }

        @keyframes slideUp { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div class="particles"></div>

    <div id="loginPage">
        <h2 style="margin: 0; font-size: 22px;">Acesso Restrito</h2>
        <p style="color:#444; font-size:10px; margin-top:5px; margin-bottom:30px;">VALIDE SUA CHAVE</p>
        
        <div id="errHWID" class="error-msg">Dispositivo não autorizado para esta KEY.</div>
        <div id="errKey" class="error-msg">Chave inválida ou expirada.</div>

        <div class="input-box">
            <i class="fas fa-key"></i>
            <input type="text" id="keyField" placeholder="KEY-XXXXX-XXXXX">
        </div>

        <button class="btn-login" id="btnLogin">VALIDAR ACESSO</button>

        <a href="https://wa.me/5527999999999" target="_blank" style="text-decoration:none; color:#25d366; font-size:12px; font-weight:bold;">
            <i class="fab fa-whatsapp"></i> Precisa de ajuda? Fale conosco
        </a>
    </div>

    <div id="mainApp">
        <div class="header">
            <h1>PAINEL V2.0</h1>
            <p>SISTEMA ATIVO</p>
        </div>

        <div class="tabs">
            <button class="tab-btn active" onclick="setTab(0)">FUNÇÕES</button>
            <button class="tab-btn" onclick="setTab(1)">INFO</button>
        </div>

        <div id="tab0" class="content-section active">
            <div class="card" id="c1">
                <div class="card-main">
                    <div class="card-icon"><i class="fas fa-crosshair"></i></div>
                    <div class="card-text"><h3>AimBot VIP</h3><p>Otimização de precisão</p></div>
                    <label class="switch"><input type="checkbox" onchange="toggle('c1')"><span class="slider"></span></label>
                </div>
                <div class="status-dot">● INATIVO</div>
            </div>

            <div class="card" id="c2">
                <div class="card-main">
                    <div class="card-icon"><i class="fas fa-microchip"></i></div>
                    <div class="card-text"><h3>RegEdit</h3><p>Estabilização de sistema</p></div>
                    <label class="switch"><input type="checkbox" onchange="toggle('c2')"><span class="slider"></span></label>
                </div>
                <div class="status-dot">● INATIVO</div>
            </div>

            <div class="card" id="c3">
                <div class="card-main">
                    <div class="card-icon"><i class="fas fa-bolt"></i></div>
                    <div class="card-text"><h3>Otimização</h3><p>Melhoria de performance</p></div>
                    <label class="switch"><input type="checkbox" onchange="toggle('c3')"><span class="slider"></span></label>
                </div>
                <div class="status-dot">● INATIVO</div>
            </div>
        </div>

        <div id="tab1" class="content-section">
            <div style="background: #0a0a0c; border: 1px solid #1a1a1c; border-radius: 18px; padding: 20px;">
                <h3 style="color:var(--neon-pink); margin-top:0;">Informações</h3>
                <p style="font-size:12px; color:#888;">Sistema operando em tempo real. Se sua chave for removida ou resetada pelo administrador, você será desconectado imediatamente.</p>
            </div>
        </div>

        <div class="footer">
            <button id="btnInject" class="btn-inject">INJETAR</button>
            <div style="font-size: 9px; color: #444; margin-top: 12px;">© 2026 TELESZZQY</div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.15.0/firebase-app.js";
        import { getDatabase, ref, get, update, onValue, off } from "https://www.gstatic.com/firebasejs/9.15.0/firebase-database.js";

        const firebaseConfig = { databaseURL: "https://painelff-9cb80-default-rtdb.firebaseio.com/" };
        const app = initializeApp(firebaseConfig);
        const db = getDatabase(app);

        const userHWID = localStorage.getItem('user_hwid_v2') || 'ID-' + Math.random().toString(36).substr(2, 9).toUpperCase();
        localStorage.setItem('user_hwid_v2', userHWID);

        let activeMonitor = null;

        document.getElementById('btnLogin').onclick = async () => {
            const key = document.getElementById('keyField').value.trim().toUpperCase();
            if(!key) return;

            const keyRef = ref(db, 'keys/' + key);
            const snap = await get(keyRef);

            if(snap.exists()){
                const d = snap.val();
                
                // Verifica se a key já está em outro HWID
                if(d.hwid && d.hwid !== userHWID) {
                    document.getElementById('errHWID').style.display = 'block';
                    document.getElementById('errKey').style.display = 'none';
                    return;
                }

                // Vincula o HWID se estiver vazio ou confirma o atual
                await update(keyRef, { hwid: userHWID, status: "ON" });
                
                document.getElementById('loginPage').style.display = 'none';
                document.getElementById('mainApp').style.display = 'block';

                // MONITORAMENTO EM TEMPO REAL
                if (activeMonitor) off(activeMonitor);
                
                activeMonitor = keyRef;
                onValue(keyRef, (snapshot) => {
                    const data = snapshot.val();

                    // Caso 1: A Key foi deletada do Firebase
                    if (!snapshot.exists()) {
                        alert("Sua KEY foi excluída do servidor!");
                        window.location.reload();
                        return;
                    }

                    // Caso 2: O HWID foi resetado (ficou vazio no banco)
                    if (!data.hwid || data.hwid === "") {
                        alert("Sua KEY foi resetada! Por favor, faça login novamente.");
                        window.location.reload();
                        return;
                    }

                    // Caso 3: O HWID no banco mudou e não é mais o deste aparelho
                    if (data.hwid !== userHWID) {
                        alert("Esta KEY está sendo usada em outro dispositivo.");
                        window.location.reload();
                        return;
                    }
                });

            } else {
                document.getElementById('errKey').style.display = 'block';
                document.getElementById('errHWID').style.display = 'none';
            }
        };

        window.setTab = (n) => {
            document.querySelectorAll('.tab-btn').forEach((b,i) => b.classList.toggle('active', i===n));
            document.querySelectorAll('.content-section').forEach((t,i) => t.classList.toggle('active', i===n));
        };

        window.toggle = (id) => {
            const card = document.getElementById(id);
            const active = card.querySelector('input').checked;
            card.classList.toggle('active', active);
            card.querySelector('.status-dot').innerText = active ? "● ATIVO" : "● INATIVO";
            const any = Array.from(document.querySelectorAll('input[type="checkbox"]')).some(c => c.checked);
            document.getElementById('btnInject').classList.toggle('ready', any);
        };

        document.getElementById('btnInject').onclick = function() {
            if(!this.classList.contains('ready')) return;
            this.innerHTML = '<i class="fas fa-spinner fa-spin"></i> PROCESSANDO...';
            this.style.pointerEvents = "none";
            setTimeout(() => {
                this.innerHTML = '<i class="fas fa-check"></i> SUCESSO!';
                alert("Processo concluído com sucesso.");
                this.style.pointerEvents = "auto";
            }, 3000);
        };
    </script>
</body>
</html>
