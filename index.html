// --- SUPABASE AYARLARI ---
// ADIM 4'TE ALDIĞINIZ BİLGİLERİ BURAYA YAPIŞTIRIN!
const SUPABASE_URL = 'https://mruogrrmbhehharznuru.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im1ydW9ncnJtYmhlaGhhcnpudXJ1Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTU4MzcwNDUsImV4cCI6MjA3MTQxMzA0NX0.q6s8w-xty1NWiPEDJrnsVV-7I-SNrz0qdENsuPjLhBk';

const supabase = supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

// --- GLOBAL DEĞİŞKENLER ---
const canvas = document.getElementById('main-canvas');
const ctx = canvas.getContext('2d');
const colorPalette = document.getElementById('color-palette');
const timerDisplay = document.getElementById('timer-display');
const usernameDisplay = document.getElementById('username-display');
const usernameModal = document.getElementById('username-modal');
const usernameInput = document.getElementById('username-input');
const saveUsernameBtn = document.getElementById('save-username-btn');

let selectedColor = '#000000';
let username = '';
const COOLDOWN_SECONDS = 60; // 1 dakika

const COLORS = [
    '#FF0000', '#00FF00', '#0000FF', '#FFFF00', '#FF00FF', '#00FFFF', 
    '#000000', '#FFFFFF', '#808080', '#FFA500', '#800080', '#A52A2A'
];

// --- KULLANICI İSMİ YÖNETİMİ ---
function handleUsername() {
    const savedUsername = localStorage.getItem('global_canvas_username');
    if (savedUsername) {
        username = savedUsername;
        usernameDisplay.textContent = username;
        usernameModal.style.display = 'none';
        init(); // Kullanıcı adı varsa uygulamayı başlat
    } else {
        usernameModal.style.display = 'flex';
    }
}

saveUsernameBtn.addEventListener('click', () => {
    const name = usernameInput.value.trim();
    if (name) {
        username = name;
        localStorage.setItem('global_canvas_username', username);
        usernameDisplay.textContent = username;
        usernameModal.style.display = 'none';
        init(); // Kullanıcı adı kaydedilince uygulamayı başlat
    }
});

// --- RENK PALETİ OLUŞTURMA ---
function createColorPalette() {
    COLORS.forEach(color => {
        const colorBox = document.createElement('div');
        colorBox.className = 'color-box';
        colorBox.style.backgroundColor = color;
        colorBox.dataset.color = color;
        colorBox.addEventListener('click', () => {
            selectedColor = color;
            document.querySelector('.color-box.selected')?.classList.remove('selected');
            colorBox.classList.add('selected');
        });
        colorPalette.appendChild(colorBox);
    });
    // İlk rengi seçili yap
    colorPalette.firstChild.classList.add('selected');
}

// --- TUVAL ÇİZİMİ ---
function drawPixel(x, y, color) {
    ctx.fillStyle = color;
    ctx.fillRect(x, y, 1, 1);
}

async function fetchInitialCanvas() {
    console.log("Mevcut pikseller çekiliyor...");
    const { data, error } = await supabase.from('pixels').select('*');
    if (error) {
        console.error("Pikselleri çekerken hata:", error);
    } else {
        data.forEach(pixel => {
            drawPixel(pixel.x, pixel.y, pixel.color);
        });
        console.log(`${data.length} piksel yüklendi.`);
    }
}

// --- ZAMANLAYICI (COOLDOWN) ---
let cooldownInterval;
function startCooldownTimer() {
    let timeLeft = COOLDOWN_SECONDS;
    localStorage.setItem('last_pixel_time', Date.now());
    
    timerDisplay.textContent = `${timeLeft}s`;
    canvas.style.cursor = 'not-allowed';

    cooldownInterval = setInterval(() => {
        timeLeft--;
        timerDisplay.textContent = `${timeLeft}s`;
        if (timeLeft <= 0) {
            clearInterval(cooldownInterval);
            timerDisplay.textContent = 'Hazır!';
            canvas.style.cursor = 'crosshair';
        }
    }, 1000);
}

function checkCooldown() {
    const lastPixelTime = localStorage.getItem('last_pixel_time');
    if (!lastPixelTime) return true; // Daha önce piksel koymamış

    const secondsPassed = (Date.now() - parseInt(lastPixelTime)) / 1000;
    if (secondsPassed >= COOLDOWN_SECONDS) {
        return true;
    } else {
        // Sayfa yenilendiyse zamanlayıcıyı devam ettir
        const remaining = Math.ceil(COOLDOWN_SECONDS - secondsPassed);
        startCooldownTimerFrom(remaining);
        return false;
    }
}

function startCooldownTimerFrom(startTime) {
    clearInterval(cooldownInterval); // Önceki interval'i temizle
    let timeLeft = startTime;
    timerDisplay.textContent = `${timeLeft}s`;
    canvas.style.cursor = 'not-allowed';

    cooldownInterval = setInterval(() => {
        timeLeft--;
        timerDisplay.textContent = `${timeLeft}s`;
        if (timeLeft <= 0) {
            clearInterval(cooldownInterval);
            timerDisplay.textContent = 'Hazır!';
            canvas.style.cursor = 'crosshair';
        }
    }, 1000);
}

// --- OLAY DİNLEYİCİLERİ ---
canvas.addEventListener('click', async (e) => {
    if (!checkCooldown()) {
        alert("Lütfen bir sonraki piksel için bekleyin.");
        return;
    }

    const rect = canvas.getBoundingClientRect();
    const x = Math.floor(e.clientX - rect.left);
    const y = Math.floor(e.clientY - rect.top);

    // 1. Önce yerel olarak çiz (anında geri bildirim için)
    drawPixel(x, y, selectedColor);

    // 2. Supabase'e gönder
    const { error } = await supabase
        .from('pixels')
        .upsert({ x, y, color: selectedColor, username: username }, { onConflict: 'x,y' });

    if (error) {
        console.error("Piksel gönderilemedi:", error);
        alert("Bir hata oluştu, pikseliniz kaydedilemedi.");
        // Hata durumunda tuvali yeniden yüklemek iyi bir fikir olabilir.
    } else {
        // 3. Zamanlayıcıyı başlat
        startCooldownTimer();
    }
});

// --- GERÇEK ZAMANLI GÜNCELLEMELER ---
function subscribeToChanges() {
    supabase.channel('pixels_changes')
        .on(
            'postgres_changes',
            { event: '*', schema: 'public', table: 'pixels' },
            (payload) => {
                console.log('Yeni piksel geldi:', payload.new);
                const { x, y, color } = payload.new;
                drawPixel(x, y, color);
            }
        )
        .subscribe();
}

// --- BAŞLANGIÇ FONKSİYONU ---
function init() {
    createColorPalette();
    fetchInitialCanvas();
    subscribeToChanges();
    checkCooldown();
}

// Uygulamayı başlat
handleUsername();
