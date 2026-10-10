<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SOLVER v2 - D.E.F Ecosystem</title>
    <script src="https://tailwindcss.com"></script>
    <style>
        .gold-gradient { background: linear-gradient(135deg, #bf953f 0%, #fcf6ba 25%, #b38728 50%, #fbf5b7 75%, #aa771c 100%); }
        .text-gold { background: linear-gradient(135deg, #bf953f 0%, #fcf6ba 50%, #aa771c 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
    </style>
</head>
<body class="bg-slate-950 text-slate-100 font-sans min-h-screen flex flex-col items-center justify-center p-3 pb-16">

    <!-- ⏳ ÉCRAN D'ANIMATION D.E.X -->
    <div id="dex-loader" class="hidden fixed inset-0 bg-slate-950/95 z-50 flex flex-col items-center justify-center">
        <div class="relative w-24 h-24 border-4 border-t-amber-400 border-r-transparent border-b-indigo-500 border-l-transparent rounded-full animate-spin"></div>
        <div class="absolute text-center">
            <h2 class="text-xl font-black tracking-widest text-indigo-400">D.E.X</h2>
            <p class="text-[8px] uppercase tracking-widest text-amber-400 font-bold mt-0.5">D.E.F-X-CHANGE</p>
        </div>
        <p class="text-xs text-slate-400 mt-6 tracking-wide animate-pulse">Traitement sécurisé en cours...</p>
    </div>

    <!-- 🔐 ÉCRAN 1 : CONNEXION / INSCRIPTION -->
    <div id="screen-auth" class="bg-slate-900/80 p-5 rounded-2xl shadow-2xl w-full max-w-md border border-slate-800 backdrop-blur">
        
        <!-- INTEGRATION DU LOGO SOLVER -->
        <div class="flex flex-col items-center mb-4">
            <img src="assets/logo.png" alt="SOLVER Logo" class="w-32 h-auto object-contain mb-2 drop-shadow-[0_0_15px_rgba(34,197,94,0.2)]">
            <h1 class="text-4xl font-black tracking-wider text-gold hidden">SOLVER</h1>
            <p class="text-indigo-400 text-[10px] font-semibold uppercase tracking-widest mt-1">Système de Règlement D.E.F</p>
        </div>

        <div class="flex gap-2 mb-4 bg-slate-950 p-1 rounded-xl border border-slate-850">
            <button onclick="switchTab('login')" id="tab-login" class="flex-1 py-1.5 text-xs font-bold rounded-lg bg-indigo-600 text-white">Connexion</button>
            <button onclick="switchTab('register')" id="tab-register" class="flex-1 py-1.5 text-xs font-bold rounded-lg text-slate-400">S'inscrire</button>
        </div>

        <!-- Form Connexion -->
        <form id="form-login" onsubmit="handleLogin(event)" class="space-y-4">
            <div>
                <label class="block text-[10px] font-bold uppercase text-slate-400 mb-1">ID Utilisateur D.E.F</label>
                <input type="text" id="login-id" required placeholder="Ex: DEF-1111 ou ADMIN" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm focus:outline-none focus:border-amber-500">
            </div>
            <div>
                <label class="block text-[10px] font-bold uppercase text-slate-400 mb-1">Mot de passe</label>
                <input type="password" required placeholder="••••••••" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm focus:outline-none focus:border-amber-500">
            </div>
            <div id="login-error" class="text-rose-500 text-xs font-semibold hidden text-center bg-rose-500/10 p-2 rounded-lg border border-rose-500/20"></div>
            <button type="submit" class="w-full gold-gradient text-slate-950 font-black text-xs uppercase tracking-wider py-3.5 rounded-xl shadow-lg">
                Ouvrir le Portefeuille
            </button>
        </form>

        <!-- Form Inscription -->
        <form id="form-register" onsubmit="handleRegistration(event)" class="space-y-4 hidden">
            <div>
                <label class="block text-[10px] font-bold uppercase text-slate-400 mb-1">Nom Complet</label>
                <input type="text" id="reg-name" required placeholder="Ex: Marc Malik" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm focus:outline-none focus:border-indigo-500">
            </div>
            <div>
                <label class="block text-[10px] font-bold uppercase text-slate-400 mb-1">ID Utilisateur D.E.F (Obligatoire)</label>
                <input type="text" id="reg-id" required placeholder="Ex: DEF-1111" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm focus:outline-none focus:border-indigo-500">
            </div>
            <div>
                <label class="block text-[10px] font-bold uppercase text-slate-400 mb-1">Numéro WhatsApp</label>
                <input type="tel" required placeholder="Ex: 067778899" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm focus:outline-none focus:border-indigo-500">
            </div>
            <div>
                <label class="block text-[10px] font-bold uppercase text-slate-400 mb-1">Créer un Mot de passe SOLVER</label>
                <input type="password" required placeholder="••••••••" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm focus:outline-none focus:border-indigo-500">
            </div>
            <div id="reg-error" class="text-rose-500 text-xs font-semibold hidden text-center bg-rose-500/10 p-2 rounded-lg border border-rose-500/20"></div>
            <button type="submit" class="w-full bg-indigo-600 text-white font-black text-xs uppercase tracking-wider py-3.5 rounded-xl shadow-lg">
                Activer mon SOLVER (À 0 XAF)
            </button>
        </form>
    </div>

    <!-- 📊 ÉCRAN 2 : INTERFACE DROPPUR -->
    <div id="screen-dropper" class="hidden bg-slate-900/90 p-4 rounded-2xl shadow-2xl w-full max-w-md border border-slate-800">
        <div class="flex justify-between items-center mb-4">
            <div>
                <h2 class="text-base font-black text-gold tracking-wide">SOLVER CLIENT</h2>
                <p id="client-id-display" class="text-[10px] text-slate-400 font-mono">ID: DEF-XXXX</p>
            </div>
            <button onclick="logout()" class="text-[9px] font-bold bg-slate-950 border border-slate-850 text-rose-400 px-2.5 py-1.5 rounded-xl">Quitter</button>
        </div>

        <!-- SOLDE CENTRAL AUTO-CONVERTI -->
        <div class="bg-slate-950 p-4 rounded-xl border border-slate-850 text-center mb-4">
            <p class="text-[9px] uppercase font-bold text-slate-500 tracking-wider">Solde Total Garanti</p>
            <p id="client-fcfa" class="text-3xl font-black text-white mt-0.5">0 XAF</p>
            <p id="client-afro" class="text-[10px] font-bold text-amber-400/80 font-mono mt-0.5">~ 0 ΔFRO</p>
        </div>

        <!-- FORMULAIRE DE RETRAIT MOBILE MONEY -->
        <div class="bg-slate-950/50 p-3 rounded-xl border border-slate-850/60 mb-4">
            <h3 class="text-[10px] font-bold text-indigo-400 uppercase tracking-wider mb-2">Demander un Retrait / Échange</h3>
            <div class="space-y-2">
                <input type="number" id="withdraw-amount" placeholder="Montant en FCFA (Ex: 5000)" class="w-full bg-slate-950 border border-slate-800 rounded-lg p-2 text-xs focus:outline-none focus:border-indigo-500">
                <select id="withdraw-method" class="w-full bg-slate-950 border border-slate-800 rounded-lg p-2 text-xs focus:outline-none text-slate-300">
                    <option value="MTN MoMo">MTN MoMo 🟡</option>
                    <option value="Airtel Money">Airtel Money 🔴</option>
                </select>
                <button onclick="submitWithdrawal()" class="w-full gold-gradient text-slate-950 font-black text-[10px] uppercase py-2 rounded-lg">Envoyer la Demande</button>
            </div>
        </div>

        <!-- TRANSACTIONS DU COMPTE -->
        <h3 class="text-[9px] uppercase font-bold text-slate-400 mb-2 px-1">Historique des Transactions</h3>
        <div id="client-tx-list" class="space-y-2 text-xs max-h-32 overflow-y-auto">
            <p class="text-slate-500 text-center py-2 text-[11px]">Aucune transaction pour le moment.</p>
        </div>
    </div>

    <!-- 👑 ÉCRAN 3 : PANNEAU ADMINISTRATEUR PRINCIPAL -->
    <div id="screen-admin" class="hidden bg-slate-900 p-4 rounded-2xl shadow-2xl w-full max-w-xl border border-indigo-950">
        <div class="flex justify-between items-center mb-4 border-b border-slate-800 pb-3">
            <div>
                <h2 class="text-base font-black text-indigo-400 tracking-wider">SOLVER ADMIN PANEL 👑</h2>
                <p class="text-[9px] text-amber-400 font-mono">Fonds Centraux : ∞ XAF (Illimités)</p>
            </div>
            <button onclick="logout()" class="text-[9px] font-bold bg-slate-950 border border-slate-850 text-rose-400 px-2.5 py-1.5 rounded-xl">Quitter</button>
        </div>

        <!-- SECTION 1 : GESTION DE LA LISTE SPÉCIALE (WHITELIST) -->
        <div class="bg-slate-950 p-3 rounded-xl border border-slate-850 mb-4">
            <h3 class="text-[10px] font-black uppercase text-amber-400 mb-2">️ Contrôle des ID Pré-Approuvés (Whitelist)</h3>
            <div class="flex gap-2 mb-2">
                <input type="text" id="new-whitelist-id" placeholder="Ex: DEF-7777" class="flex-1 bg-slate-900 border border-slate-800 rounded-lg p-2 text-xs uppercase focus:outline-none">
                <button onclick="addToWhitelist()" class="bg-indigo-600 hover:bg-indigo-500 text-white font-bold text-[10px] px-4 rounded-lg uppercase">Ajouter</button>
            </div>
            <div id="whitelist-tags" class="flex flex-wrap gap-1.5 max-h-16 overflow-y-auto pt-1"></div>
        </div>

        <!-- ALERTE VISUELLE SIMPLE -->
        <div id="admin-alert-banner" class="hidden bg-amber-500/10 border border-amber-500/30 text-amber-400 text-[11px] p-2.5 rounded-xl mb-4 font-bold flex items-center justify-between animate-pulse">
    <script>
        function switchTab(tab) {
            document.getElementById('form-login').classList.toggle('hidden', tab !== 'login');
            document.getElementById('form-register').classList.toggle('hidden', tab !== 'register');
            document.getElementById('tab-login').className = tab === 'login' ? 'flex-1 py-1.5 text-xs font-bold rounded-lg bg-indigo-600 text-white' : 'flex-1 py-1.5 text-xs font-bold rounded-lg text-slate-400';
            document.getElementById('tab-register').className = tab === 'register' ? 'flex-1 py-1.5 text-xs font-bold rounded-lg bg-indigo-600 text-white' : 'flex-1 py-1.5 text-xs font-bold rounded-lg text-slate-400';
        }
        function handleLogin(e) { 
            e.preventDefault(); 
            const userId = document.getElementById('login-id').value.toUpperCase();
            if(userId === 'ADMIN') {
                document.getElementById('screen-auth').classList.add('hidden');
                document.getElementById('screen-admin').classList.remove('hidden');
            } else {
                document.getElementById('screen-auth').classList.add('hidden');
                document.getElementById('screen-dropper').classList.remove('hidden');
                document.getElementById('client-id-display').innerText = 'ID: ' + userId;
            }
        }
        function handleRegistration(e) { 
            e.preventDefault(); 
            alert('Inscription réussie ! Vous pouvez maintenant vous connecter.'); 
            switchTab('login');
        }
        function logout() {
            document.getElementById('screen-admin').classList.add('hidden');
            document.getElementById('screen-dropper').classList.add('hidden');
            document.getElementById('screen-auth').classList.remove('hidden');
        }
    </script>
</body>
</html>
