Here is an HTML document that creates a turn-based battle game where you control Ratu against an enemy, complete with attack, heal, and restart buttons.
```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Ratu Pelindung - Pertarungan Melawan Musuh Jahat</title>
    <style>
        * {
            box-sizing: border-box;
            user-select: none;
        }

        body {
            background: linear-gradient(145deg, #1a2a3a 0%, #0f1a24 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Segoe UI', 'Poppins', 'Courier New', monospace;
            margin: 0;
            padding: 20px;
        }

        /* GAME CARD UTAMA */
        .game-container {
            max-width: 1100px;
            width: 100%;
            background: rgba(30, 20, 35, 0.8);
            backdrop-filter: blur(2px);
            border-radius: 64px 64px 48px 48px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5), inset 0 1px 2px rgba(255, 255, 255, 0.1);
            padding: 1.8rem;
            border: 1px solid rgba(255, 215, 150, 0.4);
            transition: all 0.2s;
        }

        h1 {
            text-align: center;
            font-size: 2.2rem;
            margin: 0 0 0.5rem 0;
            font-weight: bold;
            background: linear-gradient(135deg, #FFD966, #E6B422, #FFB347);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 2px 4px rgba(0,0,0,0.3);
            letter-spacing: 2px;
            word-break: keep-all;
        }

        .sub {
            text-align: center;
            color: #cddde9;
            font-style: italic;
            margin-bottom: 32px;
            font-size: 0.9rem;
            border-bottom: 1px dashed #ffd96655;
            display: inline-block;
            width: auto;
            margin-left: auto;
            margin-right: auto;
            padding-bottom: 8px;
        }

        /* ARENA KARAKTER */
        .characters {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 2rem;
            margin: 2rem 0;
        }

        .character {
            flex: 1;
            min-width: 240px;
            background: #2c1e2fcc;
            backdrop-filter: blur(8px);
            border-radius: 48px;
            padding: 1.8rem 1.2rem;
            text-align: center;
            box-shadow: 0 15px 25px rgba(0,0,0,0.3);
            transition: transform 0.2s ease;
            border: 1px solid rgba(255, 200, 130, 0.5);
        }

        .character:hover {
            transform: translateY(-5px);
        }

        .character h2 {
            font-size: 2rem;
            margin: 0 0 0.75rem;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .ratu-card h2 {
            color: #f9c270;
            text-shadow: 0 2px 0 #7a2e2e;
        }

        .enemy-card h2 {
            color: #e06c6c;
            text-shadow: 0 2px 0 #3a1a1a;
        }

        /* HEALTH BAR */
        .hp-bar-container {
            background-color: #3a2a2a;
            border-radius: 30px;
            height: 26px;
            width: 100%;
            margin: 12px 0;
            overflow: hidden;
            box-shadow: inset 0 1px 4px #00000055;
        }

        .hp-bar {
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, #54c46b, #2f9e44);
            border-radius: 30px;
            transition: width 0.25s cubic-bezier(0.2, 0.9, 0.4, 1.1);
        }

        .enemy-card .hp-bar {
            background: linear-gradient(90deg, #dd5544, #b12a1a);
        }

        .hp-text {
            font-weight: bold;
            font-size: 1.3rem;
            background: #1e131eaa;
            display: inline-block;
            padding: 4px 16px;
            border-radius: 60px;
            margin-top: 6px;
            color: #f7f3e0;
        }

        /* TOMBOL AKSI */
        .actions {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 20px;
            margin: 30px 0 20px;
        }

        button {
            font-size: 1.4rem;
            font-weight: bold;
            padding: 12px 28px;
            border: none;
            border-radius: 60px;
            background: #efe3c9;
            color: #2c1e2e;
            cursor: pointer;
            transition: 0.1s linear;
            box-shadow: 0 6px 0 #806e5a;
            font-family: inherit;
            display: inline-flex;
            align-items: center;
            gap: 10px;
        }

        button:active {
            transform: translateY(3px);
            box-shadow: 0 3px 0 #806e5a;
        }

        button:disabled {
            opacity: 0.5;
            transform: translateY(0);
            cursor: not-allowed;
            filter: grayscale(0.1);
            box-shadow: 0 6px 0 #806e5a;
        }

        #attack-btn {
            background: #dc7f5c;
            color: white;
            box-shadow: 0 6px 0 #854c30;
        }

        #heal-btn {
            background: #5da38e;
            color: white;
            box-shadow: 0 6px 0 #2f6250;
        }

        #restart-btn {
            background: #4f5f6e;
            color: #f9efcf;
            box-shadow: 0 6px 0 #2c3a44;
        }

        /* LOG DAN STATUS */
        .log-status {
            background: #191919aa;
            border-radius: 32px;
            margin-top: 24px;
            padding: 16px 20px;
            backdrop-filter: blur(4px);
            border: 1px solid #bbaa77;
        }

        #game-status {
            background: #00000066;
            font-size: 1.3rem;
            font-weight: bold;
            text-align: center;
            padding: 10px;
            border-radius: 48px;
            margin-bottom: 16px;
            color: #ffecb3;
            letter-spacing: 1px;
        }

        .battle-log h3 {
            margin: 0 0 8px 0;
            font-size: 1rem;
            color: #e2c28b;
            border-left: 6px solid #f3b34c;
            padding-left: 14px;
        }

        #log-list {
            background: #1e1a1acc;
            border-radius: 28px;
            padding: 12px 16px;
            min-height: 130px;
            max-height: 160px;
            overflow-y: auto;
            font-size: 0.85rem;
            font-family: monospace;
            color: #f0e5c4;
        }

        #log-list div {
            padding: 6px 0;
            border-bottom: 1px dotted #6a5a3e;
            font-weight: 500;
        }

        /* icon karakter animasi kecil */
        .icon-big {
            font-size: 3rem;
            display: block;
            filter: drop-shadow(2px 4px 6px black);
        }

        footer {
            text-align: center;
            font-size: 0.7rem;
            margin-top: 18px;
            color: #ae9a71;
        }

        @media (max-width: 650px) {
            .game-container {
                padding: 1rem;
            }
            button {
                padding: 8px 20px;
                font-size: 1.1rem;
            }
            .character h2 {
                font-size: 1.6rem;
            }
        }
    </style>
</head>
<body>

<div class="game-container">
    <h1>👑 RATU PELINDUNG 👑</h1>
    <div style="text-align: center;"><span class="sub">⚔️ Adu Takhta melawan Kegelapan ⚔️</span></div>

    <div class="characters">
        <!-- Karakter Ratu -->
        <div class="character ratu-card">
            <div class="icon-big">👸✨</div>
            <h2>RATU <span style="font-size: 1.2rem;">Sang Perwira</span></h2>
            <div class="hp-bar-container">
                <div id="ratu-hp-bar" class="hp-bar" style="width: 100%;"></div>
            </div>
            <div class="hp-text">❤️ <span id="ratu-hp-text">100</span> / <span id="ratu-max-hp">100</span></div>
        </div>

        <!-- Musuh Jahat -->
        <div class="character enemy-card">
            <div class="icon-big">👾🐉</div>
            <h2>MUSUH <span style="font-size: 1rem;">Ratu Kegelapan</span></h2>
            <div class="hp-bar-container">
                <div id="enemy-hp-bar" class="hp-bar" style="width: 100%;"></div>
            </div>
            <div class="hp-text">💀 <span id="enemy-hp-text">100</span> / <span id="enemy-max-hp">100</span></div>
        </div>
    </div>

    <!-- Tombol-Tombolt (Ada tombolnya!!) -->
    <div class="actions">
        <button id="attack-btn">⚔️ SERANG</button>
        <button id="heal-btn">💚 SEMBUH</button>
        <button id="restart-btn">🔄 MULAI ULANG</button>
    </div>

    <div class="log-status">
        <div id="game-status">🏰 Perang dimulai! Lawan musuh dengan berani.</div>
        <div class="battle-log">
            <h3>📜 Catatan Pertempuran</h3>
            <div id="log-list">
                <div>✨ Ratu memasuki medan laga, bersiaplah!</div>
                <div>🐉 Musuh jahat mengamuk, kalahkan dia!</div>
            </div>
        </div>
    </div>
    <footer>🎮 Klik Serang atau Sembuh — musuh akan membalas setelah giliranmu!</footer>
</div>

<script>
    // ----- DATA GAME -----
    // Karakter Ratu (Queen)
    let ratuMaxHp = 100;
    let ratuCurrentHp = 100;
    // Musuh (Enemy)
    let enemyMaxHp = 100;
    let enemyCurrentHp = 100;

    let gameActive = true;      // apakah pertarungan berlangsung
    let processingTurn = false; // mencegah spam klik saat animasi/running

    // DOM elements
    const ratuHpText = document.getElementById('ratu-hp-text');
    const ratuHpBar = document.getElementById('ratu-hp-bar');
    const enemyHpText = document.getElementById('enemy-hp-text');
    const enemyHpBar = document.getElementById('enemy-hp-bar');
    const ratuMaxSpan = document.getElementById('ratu-max-hp');
    const enemyMaxSpan = document.getElementById('enemy-max-hp');

    const attackBtn = document.getElementById('attack-btn');
    const healBtn = document.getElementById('heal-btn');
    const restartBtn = document.getElementById('restart-btn');
    const gameStatusDiv = document.getElementById('game-status');
    const logList = document.getElementById('log-list');

    // Helper log dengan limit (max 12 pesan biar tetap rapi)
    function addLogMessage(msg) {
        const logEntry = document.createElement('div');
        logEntry.innerHTML = `⚡ ${new Date().toLocaleTimeString().slice(0,5)} &nbsp; ${msg}`;
        logList.prepend(logEntry); // pesan terbaru di atas
        if (logList.children.length > 12) {
            logList.removeChild(logList.lastChild);
        }
        // Scroll ke atas biar liat pesan terbaru
        logList.scrollTop = 0;
    }

    // update UI (health bars, text, dan status tombol)
    function updateUI() {
        // update angka dan bar ratu
        ratuHpText.innerText = Math.max(0, ratuCurrentHp);
        const ratuPercent = (ratuCurrentHp / ratuMaxHp) * 100;
        ratuHpBar.style.width = `${Math.max(0, ratuPercent)}%`;
        if (ratuPercent < 25) ratuHpBar.style.background = "#e35f4e";
        else if (ratuPercent < 55) ratuHpBar.style.background = "#f4a261";
        else ratuHpBar.style.background = "linear-gradient(90deg, #54c46b, #2f9e44)";

        // update musuh
        enemyHpText.innerText = Math.max(0, enemyCurrentHp);
        const enemyPercent = (enemyCurrentHp / enemyMaxHp) * 100;
        enemyHpBar.style.width = `${Math.max(0, enemyPercent)}%`;
        if (enemyPercent < 25) enemyHpBar.style.background = "#b12a1a";
        else if (enemyPercent < 55) enemyHpBar.style.background = "#dd6b4a";
        else enemyHpBar.style.background = "linear-gradient(90deg, #dd5544, #b12a1a)";

        // update status tombol berdasarkan gameActive & processing
        if (!gameActive) {
            attackBtn.disabled = true;
            healBtn.disabled = true;
        } else {
            attackBtn.disabled = processingTurn;
            healBtn.disabled = processingTurn;
        }
    }

    // atur status kemenangan / kekalahan
    function checkGameOver() {
        if (!gameActive) return true;

        if (ratuCurrentHp <= 0) {
            gameActive = false;
            ratuCurrentHp = 0;
            updateUI();
            gameStatusDiv.innerHTML = "💔 Ratu gugur... Kekalahan! Gunakan 'Mulai Ulang' untuk bertempur lagi 💔";
            addLogMessage("😭 Ratu jatuh! Musuh berhasil mengalahkan Sang Pelindung...");
            updateUI();
            return true;
        }
        else if (enemyCurrentHp <= 0) {
            gameActive = false;
            enemyCurrentHp = 0;
            updateUI();
            gameStatusDiv.innerHTML = "🏆✨ KEMENANGAN! Ratu berhasil mengusir musuh! Alam kembali damai ✨🏆";
            addLogMessage("🎉🎉 SELAMAT! Musuh dikalahkan oleh Ratu! Kemenangan gemilang! 🎉🎉");
            updateUI();
            return true;
        }
        return false;
    }

    // Serangan musuh otomatis (setelah giliran pemain)
    function enemyAttackTurn() {
        if (!gameActive) return false;
        // damage musuh antara 9 - 19
        const dmg = Math.floor(Math.random() * 12) + 9; // 9-20
        const finalDamage = Math.min(dmg, ratuCurrentHp);
        ratuCurrentHp = Math.max(0, ratuCurrentHp - finalDamage);
        addLogMessage(`🐉 MUSUH menyerang balik! Ratu menerima ${finalDamage} damage!`);
        updateUI();

        const isDead = checkGameOver();
        if (isDead) return true; // game over karena ratu mati

        if (ratuCurrentHp <= 0) {
            checkGameOver();
            return true;
        }
        return false;
    }

    // Aksi SERANG dari Ratu
    function ratuAttack() {
        // damage ratu antara 13 - 24 (cukup kuat, layaknya ratu)
        const damage = Math.floor(Math.random() * 14) + 12; // 12 - 25
        const realDamage = Math.min(damage, enemyCurrentHp);
        enemyCurrentHp = Math.max(0, enemyCurrentHp - realDamage);
        addLogMessage(`👸 RATU menghantam dengan pedang sakti! ${realDamage} damage kepada musuh!`);
        updateUI();

        // cek apakah musuh mati setelah serangan
        const enemyDead = checkGameOver();
        if (enemyDead) {
            // musuh mati, tidak ada serangan balik
            return true; // mengakhiri giliran, game over (menang)
        }
        return false; // musuh masih hidup
    }

    // Aksi SEMBUH (Heal)
    function ratuHeal() {
        let healAmount = Math.floor(Math.random() * 17) + 12; // 12 - 28
        let oldHp = ratuCurrentHp;
        ratuCurrentHp = Math.min(ratuMaxHp, ratuCurrentHp + healAmount);
        let healedReal = ratuCurrentHp - oldHp;
        addLogMessage(`💖 Ratu membaca doa pemulihan! +${healedReal} HP (kini ${ratuCurrentHp}/${ratuMaxHp})`);
        updateUI();
        // tidak ada efek langsung ke musuh, tapi musuh akan membalas setelah ini
        return false;
    }

    // Fungsi utama untuk menjalankan giliran pemain (attack / heal) lalu mendapat serangan balik
    async function executePlayerAction(actionType) {
        if (processingTurn) {
            addLogMessage("❄️ Tunggu, pertarungan sedang berlangsung...");
            return;
        }
        if (!gameActive) {
            addLogMessage("⚰️ Pertarungan sudah usai. Tekan 'Mulai Ulang' untuk bertempur kembali.");
            return;
        }

        processingTurn = true;
        updateUI(); // disable tombol segera

        let isGameFinished = false;

        // 1. Aksi Pemain
        if (actionType === 'attack') {
            isGameFinished = ratuAttack();
        } else if (actionType === 'heal') {
            isGameFinished = ratuHeal();
        }

        // Jika setelah aksi game selesai (musuh mati), jangan lanjut serangan balik
        if (isGameFinished) {
            processingTurn = false;
            updateUI();
            return;
        }

        // 2. Serangan balik musuh (jika game masih aktif dan musuh masih hidup serta ratu masih hidup)
        if (gameActive && enemyCurrentHp > 0 && ratuCurrentHp > 0) {
            const afterEnemy = enemyAttackTurn();
            if (afterEnemy) {
                // jika setelah serangan balik ratu mati, game over sudah dihandle di dalam enemyAttackTurn
                processingTurn = false;
                updateUI();
                return;
            }
        }

        // Pastikan game masih aktif, jika tidak maka jangan lanjut
        if (!gameActive) {
            processingTurn = false;
            updateUI();
            return;
        }

        // 3. Setelah serangan balik, cek kembali status game (mungkin ratu mati atau musuh mati karena efek counter? enggak, tapi aman)
        checkGameOver();

        processingTurn = false;
        updateUI();

        // update status pesan jika permainan masih berlangsung 
        if (gameActive) {
            gameStatusDiv.innerHTML = "👸 Ratu siap bertempur! Pilih aksi selanjutnya.";
        } else {
            if (ratuCurrentHp <= 0) gameStatusDiv.innerHTML = "💀 Ratu dikalahkan... Mulai Ulang untuk membalas dendam!";
            else if (enemyCurrentHp <= 0) gameStatusDiv.innerHTML = "🏆 Kemenangan Ratu! Kerajaan selamat! 🏆";
        }
    }

    // ----- RESET GAME (Mulai Ulang) -----
    function resetGame() {
        // mencegah konflik saat reset di tengah processing
        processingTurn = true; // hentikan semua aksi sementara reset
        // reset stat
        ratuCurrentHp = ratuMaxHp;
        enemyCurrentHp = enemyMaxHp;
        gameActive = true;
        
        updateUI();
        
        // reset log tapi dengan nuansa baru
        logList.innerHTML = "";
        addLogMessage("✨ Ratu bangkit kembali dengan semangat baru!");
        addLogMessage("⚔️ Musuh dipanggil lagi dari kegelapan. Pertarungan dimulai ulang!");
        addLogMessage("🛡️ Gunakan Serang atau Sembuh untuk mengalahkan musuh!");
        
        gameStatusDiv.innerHTML = "🏹 Pertempuran dimulai ulang! Kalahkan musuh, Ratu!";
        processingTurn = false;
        updateUI();
        
        // pastikan tombol aktif kembali
        attackBtn.disabled = false;
        healBtn.disabled = false;
        addLogMessage("❤️‍🔥 Kesehatan Ratu dan Musuh pulih penuh!");
    }

    // Event binding
    attackBtn.addEventListener('click', () => {
        if (!gameActive) {
            addLogMessage("Permainan sudah selesai. Tekan Mulai Ulang untuk melanjutkan.");
            return;
        }
        if (processingTurn) return;
        executePlayerAction('attack');
    });

    healBtn.addEventListener('click', () => {
        if (!gameActive) {
            addLogMessage("Permainan sudah selesai. Tekan Mulai Ulang untuk melanjutkan.");
            return;
        }
        if (processingTurn) return;
        executePlayerAction('heal');
    });

    restartBtn.addEventListener('click', () => {
        // reset bahkan jika game sedang processing, aman karena resetGame memaksa state ulang
        resetGame();
    });

    // inisialisasi tampilan & state
    function initGame() {
        ratuMaxHp = 100;
        ratuCurrentHp = 100;
        enemyMaxHp = 100;
        enemyCurrentHp = 100;
        gameActive = true;
        processingTurn = false;
        ratuMaxSpan.innerText = ratuMaxHp;
        enemyMaxSpan.innerText = enemyMaxHp;
        updateUI();
        gameStatusDiv.innerHTML = "🏰 Perang dimulai! Lawan musuh dengan berani.";
        logList.innerHTML = "";
        addLogMessage("✨ Ratu memasuki medan laga, bersiaplah!");
        addLogMessage("🐉 Musuh jahat mengamuk, kalahkan dia!");
        addLogMessage("💡 Tips: Serang untuk mengurangi HP musuh, sembuh memulihkan HP, tapi musuh selalu membalas!");
    }

    initGame();
</script>
</body>
</html>
```
