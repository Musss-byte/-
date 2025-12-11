<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tomodachi Matcha — Developer Mode</title>
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://accounts.google.com/gsi/client" async defer></script>
</head>
<body class="min-h-screen bg-gradient-to-b from-green-50 via-green-100 to-green-50 text-gray-900">

<!-- Header -->
<header class="bg-green-700 text-white px-6 py-4 shadow-md sticky top-0 z-10">
  <div class="max-w-6xl mx-auto flex items-center justify-between">
    <div class="flex items-center gap-3">
      <div class="w-12 h-12 rounded-full bg-white/20 flex items-center justify-center text-2xl font-bold">🍵</div>
      <div>
        <h1 class="text-xl font-extrabold">Tomodachi Matcha</h1>
        <p class="text-sm opacity-90">Matcha & minuman hijau — langsung ke WhatsApp</p>
      </div>
    </div>
    <nav class="flex items-center gap-4">
      <button onclick="scrollToProduk()" class="text-white/90 hover:text-white transition">Produk</button>
      <button onclick="toggleCart()" class="bg-white text-green-700 px-4 py-2 rounded-md font-semibold shadow hover:scale-[1.01] transition" id="cart-count">Keranjang (0)</button>
      <!-- Tombol developer toggle & logout muncul hanya saat login developer -->
      <div id="developer-buttons" class="flex gap-2"></div>
    </nav>
  </div>
</header>

<!-- Google Sign-In -->
<div class="max-w-6xl mx-auto px-6 py-4 flex items-center gap-2">
  <div id="g_id_onload"
       data-client_id="YOUR_GOOGLE_CLIENT_ID"
       data-login_uri=""
       data-auto_prompt="false"
       data-callback="handleCredentialResponse">
  </div>
  <div class="g_id_signin" data-type="standard"></div>
</div>

<main class="max-w-6xl mx-auto px-6 py-10">

  <!-- Produk & Keranjang -->
  <section id="produk" class="grid grid-cols-1 md:grid-cols-3 gap-8">
    <div class="md:col-span-2">
      <h2 class="text-2xl font-bold mb-4">Produk</h2>
      <div class="grid grid-cols-1 sm:grid-cols-2 gap-6" id="product-list"></div>
    </div>

    <aside class="bg-white p-4 rounded-2xl shadow-md hidden md:block" id="cart-sidebar">
      <h3 class="font-bold text-lg">Ringkasan Keranjang</h3>
      <div class="mt-3 space-y-3" id="cart-items">
        <p class="text-sm text-gray-600">Keranjang kosong — tambahkan produk.</p>
      </div>
      <div class="mt-4 border-t pt-4">
        <div class="flex justify-between font-semibold">Subtotal <span id="cart-subtotal">Rp0</span></div>
        <div class="mt-3 flex flex-wrap gap-2">
          <button onclick="openCheckoutModal()" class="flex-1 bg-green-700 text-white py-2 rounded-lg font-semibold">Checkout</button>
        </div>
      </div>
    </aside>
  </section>

  <!-- Tentang -->
  <section class="mt-12 bg-white rounded-2xl p-6 shadow">
    <h2 class="text-xl font-bold mb-2">Tentang Tomodachi Matcha</h2>
    <p class="text-gray-700">Tomodachi Matcha menyediakan matcha berkualitas dengan kemasan rapi. Pesan cepat lewat WhatsApp — kami responsif dan siap bantu.</p>
  </section>
</main>

<!-- Footer -->
<footer class="text-center py-6 text-sm text-gray-700">© <span id="year"></span> Tomodachi Matcha — dibuat dengan ❤️</footer>

<!-- Checkout Modal -->
<div id="checkout-modal" class="fixed inset-0 bg-black/40 hidden items-center justify-center z-50">
  <div class="bg-white w-full max-w-2xl rounded-2xl p-6">
    <div class="flex items-start justify-between">
      <div>
        <h3 class="text-2xl font-bold">Checkout</h3>
        <p class="text-sm text-gray-600">Isi data pengiriman dan pilih metode pembayaran.</p>
      </div>
      <button onclick="closeCheckoutModal()" class="text-gray-500">Tutup</button>
    </div>

    <div class="mt-4 grid grid-cols-1 md:grid-cols-2 gap-4">
      <div>
        <label class="block text-sm font-medium">Nama</label>
        <input type="text" id="customer-name" class="mt-1 block w-full rounded-md border px-3 py-2" placeholder="Nama lengkap">
      </div>
      <div>
        <label class="block text-sm font-medium">Telepon</label>
        <input type="text" id="customer-phone" class="mt-1 block w-full rounded-md border px-3 py-2" placeholder="08xxxxxxxx">
      </div>
      <div class="md:col-span-2">
        <label class="block text-sm font-medium">Alamat</label>
        <textarea id="customer-address" class="mt-1 block w-full rounded-md border px-3 py-2" placeholder="Alamat lengkap untuk estimasi ongkir"></textarea>
      </div>
    </div>

    <div class="mt-6">
      <div class="flex gap-2 mb-4">
        <button onclick="selectTab('cod')" id="tab-cod" class="flex-1 border rounded px-3 py-2">COD</button>
        <button onclick="selectTab('transfer')" id="tab-transfer" class="flex-1 border rounded px-3 py-2">Transfer Bank</button>
        <button onclick="selectTab('midtrans')" id="tab-midtrans" class="flex-1 border rounded px-3 py-2">Midtrans</button>
      </div>
      <div id="tab-content">
        <p>Pilih metode pembayaran di atas.</p>
      </div>
    </div>

    <div class="mt-6 flex items-center justify-between flex-wrap gap-2">
      <div>
        <div class="font-medium">Subtotal</div>
        <div class="text-2xl font-bold" id="checkout-subtotal">Rp0</div>
      </div>
    </div>
  </div>
</div>

<script>
// --- KONSTANTA & STATE ---
const SHOP_PHONE="6281234567890";
let cart=[];
let activeTab=null;
let isDeveloper = false;
const allowedDevelopers = ["developer@example.com"]; // ganti dengan akun Google developer
let currentDeveloperEmail = null;

// --- PRODUK ---
const DEFAULT_PRODUCTS=[
{id:"m1",title:"Matcha Premium 100g",price:85000,img:"https://images.unsplash.com/photo-1521305916504-4a1121188589?w=800",desc:"Powder matcha grade premium — aroma kaya dan warna hijau cerah."},
{id:"m2",title:"Matcha Latte Mix 200g",price:120000,img:"https://images.unsplash.com/photo-1612874742651-d9f91b3d60c8?w=800",desc:"Mudah dibuat, rasa lembut dan creamy — cocok untuk kafe di rumah."},
{id:"m3",title:"Gift Box — Matcha Set",price:250000,img:"https://images.unsplash.com/photo-1546820389-44d77e1f3b31?w=800",desc:"Paket kado elegan untuk anniversary atau ulang tahun."},
{id:"m4",title:"Matcha Kit 150g",price:95000,img:"https://images.unsplash.com/photo-1611080629006-9c8f3dfe3b09?w=800",desc:"Kit matcha praktis untuk mulai membuat minuman hijau."},
{id:"m5",title:"Matcha Cookies",price:50000,img:"https://images.unsplash.com/photo-1612874742651-d9f91b3d60c8?w=800",desc:"Cookies renyah rasa matcha, cocok untuk camilan."}
];

let PRODUCTS = JSON.parse(localStorage.getItem('PRODUCTS')) || DEFAULT_PRODUCTS;

// --- UTIL ---
function currencyIDR(n){return new Intl.NumberFormat("id-ID",{style:"currency",currency:"IDR"}).format(n);}
function scrollToProduk(){document.getElementById('produk').scrollIntoView({behavior:'smooth'});}
function toggleCart(){document.getElementById('cart-sidebar').classList.toggle('hidden');}

// --- Keranjang ---
function updateCartUI(){
  const el=document.getElementById('cart-items'); el.innerHTML='';
  if(cart.length===0){ el.innerHTML='<p class="text-sm text-gray-600">Keranjang kosong — tambahkan produk.</p>'; }
  else{
    cart.forEach(it=>{
      const div=document.createElement('div'); div.className='flex items-center justify-between gap-3';
      div.innerHTML=`<div class="flex items-center gap-3">
        <img src="${it.img}" class="w-14 h-14 rounded-md object-cover">
        <div><div class="font-medium">${it.title}</div><div class="text-sm text-gray-600">${currencyIDR(it.price)}</div></div>
      </div>
      <div class="flex items-center gap-2">
        <button onclick="changeQty('${it.id}',-1)" class="px-2 py-1 rounded border">-</button>
        <div class="w-6 text-center">${it.qty}</div>
        <button onclick="changeQty('${it.id}',1)" class="px-2 py-1 rounded border">+</button>
        <button onclick="removeItem('${it.id}')" class="ml-2 text-sm text-red-500">Hapus</button>
      </div>`;
      el.appendChild(div);
    });
  }
  document.getElementById('cart-subtotal').textContent=currencyIDR(cart.reduce((s,it)=>s+it.price*it.qty,0));
}

// --- CRUD Keranjang ---
function addToCart(p){ const found=cart.find(it=>it.id===p.id); if(found)found.qty+=1; else cart.push({...p,qty:1}); updateCartUI();}
function changeQty(id,delta){cart=cart.map(it=>it.id===id?{...it,qty:Math.max(1,it.qty+delta)}:it).filter(it=>it.qty>0); updateCartUI();}
function removeItem(id){cart=cart.filter(it=>it.id!==id); updateCartUI();}

// --- Checkout ---
function openCheckoutModal(){
  if(cart.length===0) return alert("Keranjang kosong");
  document.getElementById('checkout-modal').classList.remove('hidden');
  document.getElementById('checkout-subtotal').textContent=currencyIDR(cart.reduce((s,it)=>s+it.price*it.qty,0));
  selectTab('cod');
}
function closeCheckoutModal(){document.getElementById('checkout-modal').classList.add('hidden');}

function selectTab(tab){
  activeTab=tab;
  document.getElementById('tab-content').innerHTML='';
  if(tab==='cod'){
    document.getElementById('tab-content').innerHTML='<p>Metode Cash on Delivery (bayar di tempat).</p><button onclick="checkout()" class="mt-2 px-4 py-2 bg-green-700 text-white rounded-md font-semibold">Pesan via WA</button>';
  } else if(tab==='transfer'){
    document.getElementById('tab-content').innerHTML='<p>Silakan transfer ke Bank BCA/Mandiri, lalu konfirmasi via WA.</p><button onclick="checkout()" class="mt-2 px-4 py-2 bg-green-700 text-white rounded-md font-semibold">Konfirmasi via WA</button>';
  } else if(tab==='midtrans'){
    document.getElementById('tab-content').innerHTML='<p>Pembayaran via Midtrans (sandbox).</p><button onclick="alert(`Simulasi Midtrans`)" class="mt-2 px-4 py-2 bg-green-800 text-white rounded-md font-semibold">Bayar Midtrans</button>';
  }
}

function checkout(){
  const name=document.getElementById('customer-name').value;
  const phone=document.getElementById('customer-phone').value;
  const address=document.getElementById('customer-address').value;
  if(cart.length===0) return alert("Keranjang kosong");
  let lines=["Halo Tomodachi Matcha 👋","Saya ingin memesan:"];
  cart.forEach((it,i)=>lines.push(`${i+1}. ${it.title} — ${it.qty} x ${currencyIDR(it.price)} = ${currencyIDR(it.qty*it.price)}`));
  lines.push(`Subtotal: ${currencyIDR(cart.reduce((s,it)=>s+it.qty*it.price,0))}`);
  if(activeTab==='cod') lines.push('Metode pembayaran: COD');
  if(activeTab==='transfer') lines.push('Metode pembayaran: Transfer Bank');
  if(name) lines.push(`Nama: ${name}`);
  if(phone) lines.push(`Telepon: ${phone}`);
  if(address) lines.push(`Alamat: ${address}`);
  const msg=encodeURIComponent(lines.join("\n"));
  window.open(`https://wa.me/${SHOP_PHONE}?text=${msg}`,'_blank');
  closeCheckoutModal();
}

// --- Google Login Callback ---
function handleCredentialResponse(response) {
  const data = JSON.parse(atob(response.credential.split(".")[1]));
  const email = data.email;
  if(allowedDevelopers.includes(email)){
    currentDeveloperEmail = email;
    showDeveloperButtons(true);
    alert(`Halo ${email}, Anda dapat mengaktifkan mode developer.`);
  } else {
    alert(`Akun ${email} tidak memiliki akses developer.`);
  }
}

// --- Developer Buttons ---
function showDeveloperButtons(show){
  const container = document.getElementById('developer-buttons');
  container.innerHTML = '';
  if(show){
    const toggleBtn = document.createElement('button');
    toggleBtn.id = 'dev-toggle-btn';
    toggleBtn.className = 'bg-green-700 text-white px-3 py-2 rounded';
    toggleBtn.textContent = isDeveloper ? 'Matikan Mode Developer' : 'Aktifkan Mode Developer';
    toggleBtn.addEventListener('click', toggleDeveloperMode);
    container.appendChild(toggleBtn);

    const logoutBtn = document.createElement('button');
    logoutBtn.className = 'bg-red-600 text-white px-3 py-2 rounded';
    logoutBtn.textContent = 'Logout Developer';
    logoutBtn.addEventListener('click', logoutDeveloper);
    container.appendChild(logoutBtn);
  }
}

// --- Toggle Developer ---
function toggleDeveloperMode(){
  isDeveloper = !isDeveloper;
  localStorage.setItem('IS_DEV', JSON.stringify(isDeveloper));
  const btn = document.getElementById('dev-toggle-btn');
  btn.textContent = isDeveloper ? 'Matikan Mode Developer' : 'Aktifkan Mode Developer';
  renderProducts();
}

// --- Logout Developer ---
function logoutDeveloper(){
  isDeveloper = false;
  currentDeveloperEmail = null;
  localStorage.setItem('IS_DEV', JSON.stringify(false));
  showDeveloperButtons(false);
  renderProducts();
}

// --- Upload Foto Produk ---
function updateProductImage(event, productId){
  const file = event.target.files[0];
  if(!file) return;
  const reader = new FileReader();
  reader.onload = function(e){
    const product = PRODUCTS.find(p=>p.id===productId);
    if(product){
      product.img = e.target.result;
      localStorage.setItem('PRODUCTS', JSON.stringify(PRODUCTS));
      renderProducts();
    }
  }
  reader.readAsDataURL(file);
}

// --- Render Produk ---
function renderProducts(){
  const productListEl = document.getElementById('product-list');
  productListEl.innerHTML = '';
  PRODUCTS.forEach(p=>{
    const article = document.createElement('article');
    article.className="bg-white rounded-2xl shadow p-4 flex flex-col";
    article.innerHTML = `
      <img src="${p.img}" alt="${p.title}" class="rounded-xl object-cover h-40 w-full mb-3">
      <h3 class="font-semibold text-lg">${p.title}</h3>
      <p class="text-sm text-gray-600 mb-3">${p.desc}</p>
      <div class="mt-auto flex items-center justify-between">
        <div class="text-lg font-bold">${currencyIDR(p.price)}</div>
        <div class="flex gap-2">
          <button onclick='addToCart(${JSON.stringify(p)})' class="bg-green-700 text-white px-3 py-2 rounded-lg shadow hover:opacity-95">Tambah</button>
        </div>
      </div>
      ${isDeveloper ? `<input type="file" onchange="updateProductImage(event,'${p.id}')" accept="image/*" class="mt-2">` : ''}
    `;
    productListEl.appendChild(article);
  });
}

// --- Init ---
renderProducts();
document.getElementById('year').textContent = new Date().getFullYear();
</script>
</body>
</html>
