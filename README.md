<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Tomodachi Matcha — Toko</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-green-50 text-gray-900 min-h-screen flex flex-col">

  <!-- HEADER -->
  <header class="bg-green-600 text-white shadow-md">
    <div class="max-w-4xl mx-auto flex items-center gap-3 p-4">
      <img src="https://i.imgur.com/8Km9tLL.jpeg" alt="logo" class="w-12 h-12 rounded-full border bg-white/20">
      <div>
        <h1 class="text-xl font-bold">Tomodachi Matcha</h1>
        <p class="text-sm opacity-90">Matcha & minuman hijau — pesan cepat lewat WhatsApp</p>
      </div>
      <div class="ml-auto text-sm" id="cart-mini">Keranjang: <span id="cart-count">0</span> item</div>
    </div>
  </header>

  <!-- MAIN -->
  <main class="max-w-4xl mx-auto flex-1 w-full p-4">
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">

      <!-- PRODUCTS -->
      <section>
        <h2 class="text-2xl font-semibold mb-4">Produk</h2>
        <div id="productGrid" class="grid grid-cols-1 sm:grid-cols-2 gap-4"></div>
      </section>

      <!-- CART & CHECKOUT PANEL -->
      <aside class="bg-white rounded-2xl p-4 shadow-md sticky top-6">
        <h3 class="text-lg font-bold mb-2">Keranjang</h3>
        <div id="cartList" class="space-y-3 max-h-64 overflow-auto mb-3">
          <div class="text-gray-500">Keranjang kosong — tambahkan produk.</div>
        </div>

        <div class="border-t pt-3 space-y-2">
          <div class="flex justify-between"><span>Subtotal</span><span id="subtotalBox">Rp 0</span></div>
          <div class="flex justify-between"><span>Diskon</span><span id="discountBox">Rp 0</span></div>
          <div class="flex justify-between items-center">
            <span>Kupon</span>
            <div class="flex gap-2">
              <input id="couponInput" class="border rounded px-2 py-1 text-sm" placeholder="Masukkan kode" />
              <button id="applyCouponBtn" class="bg-yellow-500 text-white px-3 py-1 rounded text-sm">Terapkan</button>
            </div>
          </div>

          <div class="mt-2">
            <label class="block text-sm font-medium mb-1">Kota (pengiriman)</label>
            <select id="citySelect" class="w-full border rounded px-3 py-2 text-sm">
              <option value="">Pilih kota</option>
              <option value="Jakarta">Jakarta</option>
              <option value="Bandung">Bandung</option>
              <option value="Surabaya">Surabaya</option>
              <option value="Medan">Medan</option>
              <option value="Makassar">Makassar</option>
            </select>
          </div>

          <div class="flex justify-between"><span>Ongkir</span><span id="ongkirBox">Rp 0</span></div>

          <div class="flex justify-between text-xl font-bold pt-2 border-t">
            <span>Total</span><span id="totalBox">Rp 0</span>
          </div>

          <div class="mt-3">
            <label class="block text-sm font-medium mb-1">Metode Pembayaran</label>
            <div class="flex gap-2">
              <label class="flex items-center gap-2"><input type="radio" name="payment" value="COD" checked> COD</label>
              <label class="flex items-center gap-2"><input type="radio" name="payment" value="Transfer"> Transfer Bank</label>
            </div>
          </div>

          <button id="openCheckoutBtn" class="mt-4 w-full bg-green-700 text-white py-2 rounded-lg font-semibold">Checkout</button>
          <button id="clearCartBtn" class="mt-2 w-full bg-red-100 text-red-700 py-2 rounded-lg text-sm">Kosongkan Keranjang</button>
        </div>
      </aside>
    </div>

    <!-- CHECKOUT MODAL -->
    <div id="checkoutModal" class="fixed inset-0 bg-black/50 hidden items-center justify-center p-4 z-50">
      <div class="bg-white rounded-lg w-full max-w-md p-5 shadow-lg">
        <h3 class="text-xl font-bold mb-3">Form Checkout</h3>
        <div class="space-y-2">
          <label class="text-sm font-medium">Nama</label>
          <input id="custName" class="w-full border rounded px-3 py-2" placeholder="Nama lengkap">

          <label class="text-sm font-medium">Alamat</label>
          <textarea id="custAddress" class="w-full border rounded px-3 py-2" placeholder="Alamat lengkap"></textarea>

          <label class="text-sm font-medium">No. Telepon</label>
          <input id="custPhone" class="w-full border rounded px-3 py-2" placeholder="08xxxxxxxx">

          <div class="text-sm text-gray-600">Periksa kembali data sebelum melanjutkan.</div>

          <div class="flex gap-2 mt-3">
            <button id="closeModalBtn" class="flex-1 border rounded py-2">Batal</button>
            <button id="confirmCheckoutBtn" class="flex-1 bg-green-700 text-white rounded py-2">Kirim via WhatsApp</button>
          </div>
        </div>
      </div>
    </div>

  </main>

  <!-- FOOTER -->
  <footer class="text-center py-4 text-sm text-gray-600">
    © <span id="year"></span> Tomodachi Matcha — dibuat dengan ❤️
  </footer>

  <!-- SCRIPT -->
  <script>
    // ====== KONFIG ======
    const SHOP_PHONE = "6281234567890"; // <<-- GANTI NOMOR WA tujuan (format internasional, tanpa '+')
    const PRODUCTS = [
      { id: 1, name: "Matcha Premium 100g", price: 85000, img: "https://images.unsplash.com/photo-1521305916504-4a1121188589?w=800&q=80" },
      { id: 2, name: "Matcha Latte Mix 200g", price: 120000, img: "https://images.unsplash.com/photo-1612874742651-d9f91b3d60c8?w=800&q=80" },
      { id: 3, name: "Gift Box — Matcha Set", price: 250000, img: "https://images.unsplash.com/photo-1546820389-44d77e1f3b31?w=800&q=80" },
      { id: 4, name: "Matcha Kit 150g", price: 95000, img: "https://images.unsplash.com/photo-1611080629006-9c8f3dfe3b09?w=800&q=80" },
      { id: 5, name: "Matcha Cookies (pak)", price: 50000, img: "https://images.unsplash.com/photo-1604908177166-5b2c9bd8b0f2?w=800&q=80" }
    ];
    const CITY_SHIPPING = { "Jakarta":10000, "Bandung":12000, "Surabaya":15000, "Medan":18000, "Makassar":20000 };

    // ====== STATE ======
    let cart = []; // each item: {id,name,price,img,qty}
    let activeCoupon = null;

    // ====== UTIL ======
    const currency = (v) => "Rp " + Number(v).toLocaleString("id-ID");

    // ====== DOM REFERENCES ======
    const productGrid = document.getElementById("productGrid");
    const cartList = document.getElementById("cartList");
    const subtotalBox = document.getElementById("subtotalBox");
    const discountBox = document.getElementById("discountBox");
    const ongkirBox = document.getElementById("ongkirBox");
    const totalBox = document.getElementById("totalBox");
    const cartCount = document.getElementById("cart-count");
    const openCheckoutBtn = document.getElementById("openCheckoutBtn");
    const checkoutModal = document.getElementById("checkoutModal");
    const closeModalBtn = document.getElementById("closeModalBtn");
    const confirmCheckoutBtn = document.getElementById("confirmCheckoutBtn");
    const applyCouponBtn = document.getElementById("applyCouponBtn");
    const couponInput = document.getElementById("couponInput");
    const citySelect = document.getElementById("citySelect");
    const clearCartBtn = document.getElementById("clearCartBtn");

    // ====== INIT ======
    document.getElementById("year").textContent = new Date().getFullYear();
    loadCart();
    renderProducts();
    renderCart();

    // ====== RENDER PRODUCTS ======
    function renderProducts(){
      productGrid.innerHTML = PRODUCTS.map(p => `
        <article class="bg-white p-3 rounded-xl shadow flex flex-col">
          <img src="${p.img}" alt="${p.name}" class="rounded-xl object-cover h-40 w-full mb-3">
          <h4 class="font-semibold">${p.name}</h4>
          <div class="text-sm text-gray-600 mb-3">${currency(p.price)}</div>
          <div class="mt-auto">
            <button class="w-full bg-green-700 text-white rounded py-2" onclick="addToCart(${p.id})">Tambah</button>
          </div>
        </article>
      `).join("");
    }

    // ====== CART: CRUD ======
    function addToCart(id){
      const prod = PRODUCTS.find(p => p.id === id);
      if(!prod) return;
      const found = cart.find(i => i.id === id);
      if(found) found.qty += 1;
      else cart.push({ ...prod, qty: 1 });
      saveCart();
      renderCart();
    }

    function changeQty(id, delta){
      const item = cart.find(i => i.id === id);
      if(!item) return;
      item.qty = Math.max(0, item.qty + delta);
      if(item.qty === 0) removeItem(id);
      saveCart();
      renderCart();
    }

    function removeItem(id){
      cart = cart.filter(i => i.id !== id);
      saveCart();
      renderCart();
    }

    function clearCart(){
      if(!confirm("Kosongkan seluruh keranjang?")) return;
      cart = [];
      activeCoupon = null;
      couponInput.value = "";
      saveCart();
      renderCart();
    }

    // ====== CART: RENDER ======
    function renderCart(){
      if(cart.length === 0){
        cartList.innerHTML = "<div class='text-gray-500'>Keranjang kosong — tambahkan produk.</div>";
        subtotalBox.textContent = currency(0);
        discountBox.textContent = currency(0);
        ongkirBox.textContent = currency(0);
        totalBox.textContent = currency(0);
        cartCount.textContent = 0;
        return;
      }

      cartList.innerHTML = cart.map(item => `
        <div class="flex justify-between items-center bg-white p-3 rounded-xl shadow border">
          <div class="flex items-center gap-3">
            <img src="${item.img}" alt="" class="w-12 h-12 rounded object-cover">
            <div>
              <div class="font-semibold">${item.name}</div>
              <div class="text-sm text-gray-600">${currency(item.price)} x ${item.qty}</div>
            </div>
          </div>
          <div class="flex items-center gap-2">
            <button class="px-3 py-1 border rounded-lg" onclick="changeQty(${item.id}, -1)">-</button>
            <div class="w-8 text-center font-semibold">${item.qty}</div>
            <button class="px-3 py-1 border rounded-lg" onclick="changeQty(${item.id}, 1)">+</button>
            <button class="text-red-600 font-semibold ml-3" onclick="removeItem(${item.id})">Hapus</button>
          </div>
        </div>
      `).join("");

      // kalkulasi
      const subtotal = cart.reduce((s, it) => s + it.price * it.qty, 0);
      const city = citySelect.value;
      const ongkir = CITY_SHIPPING[city] || 0;
      let discount = 0;
      if(activeCoupon === "MATCHA10") discount = Math.round(subtotal * 0.10);
      // (bisa tambah tipe kupon lain di sini)

      const total = Math.max(0, subtotal + ongkir - discount);

      subtotalBox.textContent = currency(subtotal);
      discountBox.textContent = currency(discount);
      ongkirBox.textContent = currency(ongkir);
      totalBox.textContent = currency(total);
      cartCount.textContent = cart.reduce((s,i) => s + i.qty, 0);
    }

    // ====== COUPON ======
    applyCouponBtn.addEventListener("click", () => {
      const code = (couponInput.value || "").trim().toUpperCase();
      if(code === "MATCHA10"){
        activeCoupon = code;
        alert("Kupon MATCHA10 diterapkan: diskon 10%!");
      } else if(code === ""){
        activeCoupon = null;
        alert("Masukkan kode kupon.");
      } else {
        activeCoupon = null;
        alert("Kode kupon tidak valid.");
      }
      saveCart();
      renderCart();
    });

    // ====== CITY SELECT CHANGE ======
    citySelect.addEventListener("change", () => { renderCart(); });

    // ====== LOCALSTORAGE ======
    function saveCart(){
      localStorage.setItem("tm_cart_v1", JSON.stringify({ cart, activeCoupon }));
    }
    function loadCart(){
      try {
        const raw = localStorage.getItem("tm_cart_v1");
        if(!raw) return;
        const parsed = JSON.parse(raw);
        if(Array.isArray(parsed.cart)) cart = parsed.cart;
        activeCoupon = parsed.activeCoupon || null;
        if(activeCoupon) couponInput.value = activeCoupon;
      } catch(e){
        console.warn("Gagal load cart:", e);
      }
    }

    // ====== CHECKOUT (WA) ======
    openCheckoutBtn.addEventListener("click", () => {
      if(cart.length === 0) return alert("Keranjang masih kosong!");
      // open modal
      checkoutModal.classList.remove("hidden");
      checkoutModal.classList.add("flex");
    });

    closeModalBtn.addEventListener("click", () => {
      checkoutModal.classList.add("hidden");
      checkoutModal.classList.remove("flex");
    });

    confirmCheckoutBtn.addEventListener("click", () => {
      const name = document.getElementById("custName").value.trim();
      const address = document.getElementById("custAddress").value.trim();
      const phone = document.getElementById("custPhone").value.trim();
      const city = citySelect.value;
      const payment = document.querySelector('input[name="payment"]:checked')?.value || "COD";

      if(!name || !address || !phone){
        return alert("Silakan lengkapi Nama, Alamat, dan Nomor Telepon.");
      }
      if(!city){
        return alert("Silakan pilih kota pengiriman.");
      }

      // recompute totals to ensure up-to-date
      const subtotal = cart.reduce((s, it) => s + it.price * it.qty, 0);
      const ongkir = CITY_SHIPPING[city] || 0;
      const discount = (activeCoupon === "MATCHA10") ? Math.round(subtotal * 0.10) : 0;
      const total = Math.max(0, subtotal + ongkir - discount);

      // build message (url-encoded)
      let lines = [];
      lines.push("Halo Tomodachi Matcha 👋");
      lines.push("Saya ingin memesan:");
      cart.forEach((it, idx) => {
        lines.push(`${idx+1}. ${it.name} — ${it.qty} x ${currency(it.price)} = ${currency(it.qty * it.price)}`);
      });
      lines.push(`Subtotal: ${currency(subtotal)}`);
      if(discount > 0) lines.push(`Diskon (${activeCoupon}): -${currency(discount)}`);
      lines.push(`Ongkir (${city}): ${currency(ongkir)}`);
      lines.push(`Total: ${currency(total)}`);
      lines.push("");
      lines.push("Metode pembayaran: " + payment);
      lines.push("");
      lines.push("Data pemesan:");
      lines.push("Nama: " + name);
      lines.push("Alamat: " + address);
      lines.push("Kota: " + city);
      lines.push("Telepon: " + phone);

      const message = encodeURIComponent(lines.join("\n"));
      const waUrl = `https://wa.me/${SHOP_PHONE}?text=${message}`;
      window.open(waUrl, "_blank");

      // optionally clear cart after checkout
      // cart = []; saveCart(); renderCart();
    });

    // ====== CLEAR CART ======
    clearCartBtn.addEventListener("click", clearCart);

    // expose functions used in markup (for onclick attributes)
    window.addToCart = addToCart;
    window.changeQty = changeQty;
    window.removeItem = removeItem;

  </script>
</body>
</html>
