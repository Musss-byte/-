<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Tomodachi Matcha</title>

<!-- Tailwind CDN -->
<script src="https://cdn.tailwindcss.com"></script>

<style>
  body {
    font-family: 'Inter', sans-serif;
    background: linear-gradient(135deg, #cfe8cf, #b5ddb5, #9fce9f);
  }
  .glass {
    background: rgba(255,255,255,0.78);
    backdrop-filter: blur(18px);
    border: 1px solid rgba(255,255,255,0.35);
  }
</style>

</head>
<body class="min-h-screen">

<!-- NAVBAR -->
<nav class="glass fixed top-0 left-0 w-full px-6 py-4 flex justify-between items-center shadow-lg z-50">
  <h1 class="text-2xl font-bold text-green-800">🍃 Tomodachi Matcha</h1>

  <button onclick="toggleCart()"
    class="relative px-4 py-2 bg-green-700 text-white rounded-xl hover:bg-green-800 transition">
    Keranjang 🛒
    <span id="cart-count" class="absolute -top-2 -right-2 bg-red-600 text-white text-xs px-2 py-0.5 rounded-full">0</span>
  </button>
</nav>

<!-- CONTENT -->
<section class="pt-28 pb-16 px-6 max-w-6xl mx-auto">
  <h2 class="text-3xl font-bold text-green-900 mb-6">Produk Matcha Premium</h2>

  <div id="product-list" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6"></div>
</section>

<!-- CART SIDEBAR -->
<div id="cart-overlay" class="hidden fixed inset-0 bg-black/40 z-40" onclick="toggleCart()"></div>

<div id="cart-panel"
  class="hidden fixed right-0 top-0 h-full w-80 glass p-5 shadow-2xl z-50 flex flex-col">

  <div class="flex justify-between items-center mb-4">
    <h2 class="text-xl font-bold text-green-800">Keranjang</h2>
    <button onclick="toggleCart()" class="text-xl font-bold text-green-900">✖</button>
  </div>

  <div id="cart-items" class="flex-1 overflow-y-auto"></div>

  <div class="mt-4 border-t pt-4">
    <div class="flex justify-between mb-2">
      <span>Total:</span>
      <span id="total-price" class="font-bold">Rp0</span>
    </div>
    <button onclick="checkoutWA()"
      class="w-full bg-green-700 text-white py-2 rounded-xl hover:bg-green-800 transition">
      Checkout via WhatsApp
    </button>
  </div>
</div>

<!-- SCRIPT -->
<script>
  const products = [
    { id: 1, name: "Matcha Premium 100g", price: 85000, img: "https://i.imgur.com/4ZQZ4Zy.png" },
    { id: 2, name: "Matcha Ceremonial 50g", price: 120000, img: "https://i.imgur.com/4ZQZ4Zy.png" },
    { id: 3, name: "Matcha Latte Blend", price: 75000, img: "https://i.imgur.com/4ZQZ4Zy.png" },
    { id: 4, name: "Matcha Cookies", price: 45000, img: "https://i.imgur.com/4ZQZ4Zy.png" },
    { id: 5, name: "Matcha Tea Bag", price: 30000, img: "https://i.imgur.com/4ZQZ4Zy.png" }
  ];

  let cart = JSON.parse(localStorage.getItem("cart") || "[]");

  const productList = document.getElementById("product-list");
  const cartItems = document.getElementById("cart-items");

  // RENDER PRODUK
  function renderProducts() {
    productList.innerHTML = "";
    products.forEach(p => {
      productList.innerHTML += `
        <div class="glass p-4 rounded-2xl shadow hover:scale-[1.02] transition">
          <img src="${p.img}" class="w-full h-40 object-cover rounded-xl mb-3">
          <h3 class="font-bold text-lg">${p.name}</h3>
          <p class="text-green-800 font-semibold mb-3">Rp${p.price.toLocaleString()}</p>

          <button onclick="addToCart(${p.id})"
            class="w-full bg-green-700 text-white py-2 rounded-xl hover:bg-green-800 transition">
            Tambah ke Keranjang
          </button>
        </div>
      `;
    });
  }

  // TAMBAH KE KERANJANG
  function addToCart(id) {
    const item = cart.find(i => i.id === id);
    if (item) {
      item.qty++;
    } else {
      const p = products.find(x => x.id === id);
      cart.push({ ...p, qty: 1 });
    }
    saveCart();
    renderCart();
  }

  // HAPUS ITEM
  function removeItem(id) {
    cart = cart.filter(i => i.id !== id);
    saveCart();
    renderCart();
  }

  // RENDER KERANJANG
  function renderCart() {
    cartItems.innerHTML = "";
    let total = 0;

    cart.forEach(i => {
      const subtotal = i.price * i.qty;
      total += subtotal;

      cartItems.innerHTML += `
        <div class="flex gap-3 mb-4">
          <img src="${i.img}" class="w-16 h-16 rounded-lg object-cover">
          <div class="flex-1">
            <h4 class="font-semibold">${i.name}</h4>
            <p class="text-sm text-green-700">Rp${i.price.toLocaleString()}</p>

            <div class="flex items-center gap-2 mt-1">
              <button onclick="changeQty(${i.id}, -1)" class="px-2 bg-green-700 text-white rounded">-</button>
              <span>${i.qty}</span>
              <button onclick="changeQty(${i.id}, 1)" class="px-2 bg-green-700 text-white rounded">+</button>
            </div>

            <p class="text-sm mt-1 font-semibold">Subtotal: Rp${subtotal.toLocaleString()}</p>
          </div>

          <button onclick="removeItem(${i.id})" class="text-red-600 font-bold">✖</button>
        </div>
      `;
    });

    document.getElementById("total-price").innerText = "Rp" + total.toLocaleString();
    document.getElementById("cart-count").innerText = cart.length;
  }

  // UBAH QTY
  function changeQty(id, delta) {
    const item = cart.find(i => i.id === id);
    if (!item) return;
    item.qty += delta;
    if (item.qty <= 0) removeItem(id);
    saveCart();
    renderCart();
  }

  // SAVE STORAGE
  function saveCart() {
    localStorage.setItem("cart", JSON.stringify(cart));
  }

  // CHECKOUT WHATSAPP
  function checkoutWA() {
    let text = "Halo, saya ingin melakukan pemesanan:%0A%0A";
    cart.forEach(i => {
      text += `• ${i.name} x${i.qty} = Rp${(i.price * i.qty).toLocaleString()}%0A`;
    });
    const total = cart.reduce((a, b) => a + b.price * b.qty, 0);
    text += `%0ATotal: Rp${total.toLocaleString()}`;
    window.open("https://wa.me/6280000000000?text=" + text, "_blank");
  }

  // OPEN / CLOSE CART
  function toggleCart() {
    const panel = document.getElementById("cart-panel");
    const overlay = document.getElementById("cart-overlay");

    panel.classList.toggle("hidden");
    overlay.classList.toggle("hidden");
  }

  // INIT
  renderProducts();
  renderCart();
</script>

</body>
</html>
