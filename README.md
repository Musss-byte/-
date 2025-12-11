<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Tomodachi Matcha Store</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  body { font-family: 'Inter', sans-serif; }
  .glass { backdrop-filter: blur(14px); background: rgba(255,255,255,0.7); }
</style>
</head>
<body class="bg-gray-100 text-gray-800">

<!-- NAVBAR -->
<nav class="w-full fixed top-0 left-0 z-50 shadow bg-white/90 backdrop-blur-lg">
  <div class="max-w-6xl mx-auto flex items-center justify-between p-4">
    <div class="flex items-center gap-3">
      <img src="logo.png" class="w-10 h-10 rounded-full" />
      <h1 class="text-xl font-semibold">Tomodachi Matcha</h1>
    </div>
    <button id="cartBtn" class="relative p-2 bg-green-600 text-white rounded-full">
      🛒
      <span id="cartCount" class="absolute -top-2 -right-2 bg-red-500 text-white text-xs w-5 h-5 flex items-center justify-center rounded-full">0</span>
    </button>
  </div>
</nav>

<div class="pt-24 max-w-6xl mx-auto p-4">

  <!-- PRODUK GRID -->
  <h2 class="text-2xl font-bold mb-4">Produk Matcha</h2>

  <div id="productGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-5"></div>

</div>

<div id="cartOverlay"
     class="fixed inset-0 bg-black/40 hidden z-[55]"></div>
     
<!-- CART SIDEBAR -->
<div id="cartSidebar" class="fixed top-0 right-0 w-80 h-full bg-white shadow-2xl p-5 transform translate-x-full transition-all duration-300 z-[60] overflow-y-auto">
  <div class="flex items-center justify-between mb-3">
  <h2 class="text-2xl font-semibold text-[#244b2f]">Keranjang</h2>
  <button id="closeCart" class="text-xl px-2 text-[#244b2f] hover:text-red-500 transition">
    ✖
  </button>
</div>
const cartOverlay = document.getElementById('cartOverlay');
const closeCartBtn = document.getElementById('closeCart');

// Buka cart
document.getElementById('cartBtn').onclick = () => {
  cartSidebar.classList.remove('translate-x-full');
  cartOverlay.classList.remove('hidden');
};

// Tombol ✖
closeCartBtn.onclick = () => {
  cartSidebar.classList.add('translate-x-full');
  cartOverlay.classList.add('hidden');
};

// Klik area luar
cartOverlay.onclick = () => {
  cartSidebar.classList.add('translate-x-full');
  cartOverlay.classList.add('hidden');
};

  <div id="cartList" class="flex flex-col gap-3"></div>

  <div class="border-t mt-4 pt-4">
    <label class="block text-sm">Pilih Kota (Ongkir)</label>
    <select id="citySelect" class="w-full border p-2 rounded mt-1">
      <option value="Jakarta">Jakarta</option>
      <option value="Cilebut">Cilebut</option>
      <option value="Jalan Baru">Jalan Baru</option>
      <option value="Bogor Kota">Bogor Kota</option>
      <option value="Ciawi">Ciawi</option>
    </select>
  </div>

  <div class="mt-4">
    <label class="block text-sm">Kode Kupon</label>
    <input id="couponInput" class="w-full border p-2 rounded mt-1" placeholder="DISKON10" />
  </div>

  <div class="mt-4 text-sm space-y-1">
    <p>Subtotal: <span id="subtotalBox" class="font-semibold"></span></p>
    <p>Diskon: <span id="discountBox" class="font-semibold text-green-600"></span></p>
    <p>Ongkir: <span id="ongkirBox" class="font-semibold"></span></p>
    <p class="text-lg font-bold mt-2">TOTAL: <span id="totalBox"></span></p>
  </div>

  <button id="checkoutBtn" class="w-full bg-green-600 text-white p-3 rounded-lg mt-4">Checkout</button>
</div>

<!-- CHECKOUT MODAL -->
<div id="checkoutModal" class="fixed inset-0 bg-black/50 hidden items-center justify-center z-[70]">
  <div class="bg-white p-6 rounded-xl max-w-md w-full">
    <h2 class="text-xl font-bold mb-4">Checkout</h2>

    <input id="name" class="w-full border p-2 rounded mb-3" placeholder="Nama" />
    <input id="address" class="w-full border p-2 rounded mb-3" placeholder="Alamat Lengkap" />
    <input id="phone" class="w-full border p-2 rounded mb-3" placeholder="Nomor Telepon" />

    <label class="text-sm font-medium">Metode Pembayaran</label>
    <div class="mt-1 mb-4 flex flex-col gap-1">
      <label><input type="radio" name="paymentMethod" value="COD" checked /> COD</label>
      <label><input type="radio" name="paymentMethod" value="Transfer Bank" /> Transfer Bank</label>
    </div>

    <div class="flex gap-2">
      <button id="sendWA" class="flex-1 bg-green-600 text-white p-2 rounded">Kirim WhatsApp</button>
      <button id="closeModal" class="flex-1 bg-gray-300 p-2 rounded">Batal</button>
    </div>
  </div>
</div>

<script>

// =============== DATA PRODUK ===============
const products = [
  { id: 1, name: "Matcha Latte", price: 15000, img: "p1.jpg" },
  { id: 2, name: "Matcha Premium", price: 30000, img: "p2.jpg" },
  { id: 3, name: "Matcha Soft Cream", price: 20000, img: "p3.jpg" },
  { id: 4, name: "Matcha Cookies", price: 15000, img: "p4.jpg" },
  { id: 5, name: "Matcha Powder", price: 40000, img: "p5.jpg" },
];

let cart = JSON.parse(localStorage.getItem("cart") || "[]");

// =============== RENDER PRODUK ===============
const grid = document.getElementById('productGrid');
grid.innerHTML = products.map(p => `
  <div class='bg-white rounded-xl shadow p-4 hover:shadow-lg transition'>
    <img src="${p.img}" class="w-full h-40 object-cover rounded-lg mb-3" />
    <h3 class="font-semibold text-lg">${p.name}</h3>
    <p class="text-green-600 font-bold">Rp ${p.price.toLocaleString()}</p>
    <button onclick="addToCart(${p.id})" class="mt-3 w-full bg-green-600 text-white p-2 rounded-lg">Tambah</button>
  </div>`).join("");

// =============== CART LOGIC ===============
function saveCart(){ localStorage.setItem("cart", JSON.stringify(cart)); }

function addToCart(id){
  let item = cart.find(i => i.id === id);
  if(item){ item.qty++; }
  else {
    const p = products.find(x => x.id === id);
    cart.push({ ...p, qty: 1 });
  }
  saveCart();
  renderCart();
}

function changeQty(id, delta){
  let item = cart.find(i => i.id === id);
  if(!item) return;
  item.qty = Math.max(1, item.qty + delta);
  saveCart(); renderCart();
}

function removeItem(id){
  cart = cart.filter(i => i.id !== id);
  saveCart(); renderCart();
}

const cityShipping = {
  "Jakarta": 20000,
  "Cilebut": 8000,
  "Jalan Baru": 5000,
  "Bogor Kota": 10000,
  "Ciawi": 180000
};

function renderCart(){
  document.getElementById('cartCount').innerText = cart.reduce((a,b)=>a+b.qty,0);

  const list = document.getElementById('cartList');

  if(cart.length === 0){ list.innerHTML = `<p class='text-gray-500'>Keranjang kosong.</p>`; return; }

  list.innerHTML = cart.map(i => `
    <div class='flex items-center gap-3 bg-gray-100 p-3 rounded-lg'>
      <img src="${i.img}" class='w-14 h-14 rounded-lg object-cover' />
      <div class='flex-1'>
        <p class='font-medium'>${i.name}</p>
        <p class='text-sm text-green-600'>Rp ${i.price.toLocaleString()}</p>
        <div class='flex items-center gap-2 mt-1'>
          <button onclick='changeQty(${i.id}, -1)' class='px-2 bg-gray-300 rounded'>-</button>
          <span>${i.qty}</span>
          <button onclick='changeQty(${i.id}, 1)' class='px-2 bg-gray-300 rounded'>+</button>
        </div>
      </div>
      <div class='text-right'>
        <p class='font-semibold'>Rp ${(i.price*i.qty).toLocaleString()}</p>
        <button onclick='removeItem(${i.id})' class='text-red-500 text-sm mt-1'>Hapus</button>
      </div>
    </div>`).join("");

  let subtotal = cart.reduce((a,b)=> a + b.price*b.qty, 0);
  let coupon = document.getElementById('couponInput').value;
  let discount = coupon === "DISKON10" ? subtotal * 0.10 : 0;
  let ongkir = cityShipping[document.getElementById('citySelect').value] || 0;
  let total = subtotal - discount + ongkir;

  document.getElementById('subtotalBox').innerText = `Rp ${subtotal.toLocaleString()}`;
  document.getElementById('discountBox').innerText = `Rp ${discount.toLocaleString()}`;
  document.getElementById('ongkirBox').innerText = `Rp ${ongkir.toLocaleString()}`;
  document.getElementById('totalBox').innerText = `Rp ${total.toLocaleString()}`;
}

renderCart();

document.getElementById('couponInput').addEventListener('input', renderCart);
document.getElementById('citySelect').addEventListener('change', renderCart);

// =============== SIDEBAR ===============
const cartSidebar = document.getElementById('cartSidebar');
document.getElementById('cartBtn').onclick = () => {
  cartSidebar.classList.toggle('translate-x-full');
};

// =============== CHECKOUT MODAL ===============
document.getElementById('checkoutBtn').onclick = () => {
  if(cart.length===0) return alert("Keranjang kosong!");
  document.getElementById('checkoutModal').classList.remove('hidden');
  document.getElementById('checkoutModal').classList.add('flex');
};

document.getElementById('closeModal').onclick = () => {
  document.getElementById('checkoutModal').classList.add('hidden');
};

document.getElementById('sendWA').onclick = () => {
  const name = document.getElementById('name').value;
  const addr = document.getElementById('address').value;
  const phone = document.getElementById('phone').value;
  const pay = document.querySelector("input[name='paymentMethod']:checked").value;
  const city = document.getElementById('citySelect').value;

  let text = `Halo, saya ingin memesan:%0A`;
  cart.forEach(i=>{
    text += `- ${i.name} x${i.qty} = Rp ${(i.price*i.qty).toLocaleString()}%0A`;
  });

  text += `%0AOngkir: ${city} = Rp ${cityShipping[city].toLocaleString()}%0A`;
  text += `Pembayaran: ${pay}%0A%0A`;
  text += `Nama: ${name}%0AAlamat: ${addr}%0ANo HP: ${phone}`;

  window.open(`https://wa.me/62838795289830?text=${text}`);
};

</script>
</body>
</html>
