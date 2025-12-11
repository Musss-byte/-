<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Tomodachi Matcha — Premium Store</title>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">

  <style>
    body {
      font-family: 'Inter', sans-serif;
      background: linear-gradient(135deg, #cfe8cf, #b5ddb5, #9fce9f);
    }
    .glass {
      background: rgba(255,255,255,0.7);
      backdrop-filter: blur(16px);
      border: 1px solid rgba(255,255,255,0.4);
      box-shadow: 0 4px 20px rgba(0,0,0,0.1);
    }
  </style>
</head>

<body class="min-h-screen">

  <!-- NAVBAR -->
  <nav class="glass fixed top-0 left-0 w-full z-50 p-4 flex justify-between items-center">
    <h1 class="text-xl font-bold text-green-900">🍵 Tomodachi Matcha</h1>
    <button onclick="toggleCart()" class="px-4 py-2 bg-green-700 text-white rounded-lg">
      Keranjang <span id="cart-count">(0)</span>
    </button>
  </nav>

  <!-- CONTENT WRAPPER -->
  <div class="pt-24 pb-20 px-4 max-w-5xl mx-auto">

    <!-- PRODUK GRID -->
    <h2 class="text-2xl font-bold text-green-900 mb-4">Produk Kami</h2>

    <div id="product-list" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6"></div>
  </div>

  <!-- KERANJANG SLIDE PANEL -->
  <div id="cart-panel"
       class="fixed top-0 right-0 w-80 h-full glass transform translate-x-full transition-all duration-300 z-50 p-4 overflow-y-auto">
    
    <div class="flex justify-between items-center mb-4">
      <h2 class="text-xl font-bold">Keranjang</h2>
      <button onclick="toggleCart()" class="text-red-500 text-xl">✖</button>
    </div>

    <div id="cart-items" class="space-y-3"></div>

    <hr class="my-4">

    <!-- KUPON -->
    <div>
      <label class="font-semibold">Kupon Diskon:</label>
      <input id="coupon" type="text" placeholder="Masukkan kupon"
             class="w-full mt-1 px-3 py-2 border rounded">
      <button onclick="applyCoupon()"
              class="w-full mt-2 bg-green-700 text-white py-2 rounded">Gunakan Kupon</button>
      <p id="coupon-info" class="text-sm text-green-700 mt-1"></p>
    </div>

    <hr class="my-4">

    <!-- ALAMAT -->
    <h3 class="font-bold mb-2">Alamat Pengiriman</h3>

    <input id="nama" type="text" placeholder="Nama Lengkap"
           class="w-full mb-2 px-3 py-2 border rounded">
    <input id="telepon" type="text" placeholder="Nomor Telepon"
           class="w-full mb-2 px-3 py-2 border rounded">
    <textarea id="alamat" placeholder="Alamat Lengkap"
              class="w-full mb-2 px-3 py-2 border rounded"></textarea>

    <input id="jarak" type="number" placeholder="Jarak (KM)"
           class="w-full mb-2 px-3 py-2 border rounded">

    <!-- AMBIL LOKASI OTOMATIS -->
    <button onclick="ambilLokasi()" class="w-full bg-green-800 text-white py-2 rounded mb-4">
      📍 Ambil Lokasi Otomatis (GPS)
    </button>
    <p id="lokasi-info" class="text-sm text-green-800 mb-4"></p>

    <!-- METODE PEMBAYARAN -->
    <label class="font-semibold">Metode Pembayaran:</label>
    <select id="payment" class="w-full px-3 py-2 border rounded mt-2">
      <option value="COD">COD (Bayar di Tempat)</option>
      <option value="TRANSFER">Transfer Bank</option>
    </select>

    <hr class="my-4">

    <!-- TOTAL -->
    <div id="summary" class="space-y-1 text-green-900 font-medium"></div>

    <button onclick="checkout()"
            class="w-full mt-4 bg-green-700 text-white py-3 rounded-lg text-lg font-bold">
      Checkout via WhatsApp
    </button>
  </div>

  <!-- FOOTER -->
  <footer class="mt-16 glass text-center py-6">
    <h3 class="font-bold text-green-900 text-lg">Tentang Kami</h3>
    <p class="max-w-xl mx-auto text-green-800 mt-2 px-6">
      Tomodachi Matcha adalah brand matcha premium dengan kualitas terbaik, 
      menghadirkan cita rasa Jepang autentik serta manfaat kesehatan.
    </p>

    <h3 class="font-bold text-green-900 text-lg mt-6">Contact</h3>
    <p class="text-green-800">WhatsApp: 0812-3456-7890</p>
    <p class="text-green-800">Email: hello@tomodachimatcha.com</p>

    <!-- GOOGLE MAPS (Lokasi Bisnis Kamu) -->
    <h3 class="font-bold text-green-900 text-lg mt-10">Lokasi Kami</h3>

    <div class="max-w-xl mx-auto mt-4 rounded-xl overflow-hidden shadow-lg border border-green-200">
      <iframe
        src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3962.597037968024!2d106.812854!3d-6.563240!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x2e69c9eba0f32ef7%3A0x7c8e3f3bb4a7ed2d!2sJl.%20Pembangunan%2C%20Kedung%20Halang%2C%20Bogor%20Utara%2C%20Kota%20Bogor!5e0!3m2!1sid!2sid!4v1700000000001!"
        width="100%"
        height="300"
        style="border:0;"
        allowfullscreen=""
        loading="lazy">
      </iframe>
    </div>

    <p class="text-green-900 mt-6 text-sm">© 2025 Tomodachi Matcha. All Rights Reserved.</p>
  </footer>


  <!-- SCRIPT -->
  <script>
    /* ============================================
       PRODUK
    ============================================ */
    const products = [
      { id: 1, name: "Matcha Premium 100gr", price: 55000, img: "https://i.imgur.com/e0S1ZfP.jpeg" },
      { id: 2, name: "Matcha Latte Mix", price: 45000, img: "https://i.imgur.com/5ZQqR7a.jpeg" },
      { id: 3, name: "Matcha Ceremonial", price: 120000, img: "https://i.imgur.com/nXPQm6C.jpeg" },
      { id: 4, name: "Hojicha Powder", price: 38000, img: "https://i.imgur.com/5ZQeZr0.jpeg" },
      { id: 5, name: "Matcha Cookies", price: 30000, img: "https://i.imgur.com/UlgL6c3.jpeg" }
    ];

    let cart = JSON.parse(localStorage.getItem("cart")) || [];

    function renderProducts() {
      const container = document.getElementById("product-list");
      container.innerHTML = products.map(p => `
        <div class="glass rounded-xl p-4">
          <img src="${p.img}" class="w-full h-40 object-cover rounded-lg">
          <h3 class="text-lg font-bold mt-2 text-green-900">${p.name}</h3>
          <p class="text-green-800 font-semibold mb-2">Rp ${p.price.toLocaleString()}</p>
          <button onclick="addToCart(${p.id})"
                  class="w-full bg-green-700 text-white py-2 rounded">Tambah</button>
        </div>
      `).join("");
    }

    /* ============================================
       KERANJANG
    ============================================ */
    function toggleCart() {
      document.getElementById("cart-panel").classList.toggle("translate-x-full");
      renderCart();
    }

    function addToCart(id) {
      const item = cart.find(i => i.id === id);
      if (item) item.qty++;
      else cart.push({ ...products.find(p => p.id === id), qty: 1 });

      saveCart();
      renderCart();
    }

    function updateQty(id, delta) {
      const item = cart.find(i => i.id === id);
      if (!item) return;
      
      item.qty += delta;
      if (item.qty <= 0) cart = cart.filter(i => i.id !== id);

      saveCart();
      renderCart();
    }

    function saveCart() {
      localStorage.setItem("cart", JSON.stringify(cart));
    }

    /* ============================================
       KUPON
    ============================================ */
    const coupons = {
      MATCHA100: { type: "percent", value: 10, min: 100000 },
      MATCHA20: { type: "percent", value: 20, min: 150000 },
      HEMAT5: { type: "flat", value: 5000, min: 0 }
    };

    let appliedCoupon = null;

    function applyCoupon() {
      const code = document.getElementById("coupon").value.toUpperCase();
      if (!coupons[code]) {
        document.getElementById("coupon-info").innerText = "Kupon tidak valid.";
        appliedCoupon = null;
      } else {
        appliedCoupon = coupons[code];
        document.getElementById("coupon-info").innerText = "Kupon diterapkan!";
      }
      renderCart();
    }

    /* ============================================
       GPS → Hitung Jarak ke Lokasi Bisnis
    ============================================ */

    // Koordinat Toko
    const toko = {
      lat: -6.563240, 
      lng: 106.812854 
    };

    // Fungsi hitung jarak (Haversine)
    function hitungJarak(lat1, lon1, lat2, lon2) {
      const R = 6371;
      const dLat = (lat2 - lat1) * Math.PI / 180;
      const dLon = (lon2 - lon1) * Math.PI / 180;

      const a =
        Math.sin(dLat/2) * Math.sin(dLat/2) +
        Math.cos(lat1 * Math.PI / 180) *
        Math.cos(lat2 * Math.PI / 180) *
        Math.sin(dLon/2) * Math.sin(dLon/2);

      const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
      return R * c;
    }

    function ambilLokasi() {
      if (!navigator.geolocation) {
        alert("Browser Anda tidak mendukung GPS.");
        return;
      }

      navigator.geolocation.getCurrentPosition(
        (pos) => {
          const lat = pos.coords.latitude;
          const lng = pos.coords.longitude;

          const jarak = hitungJarak(lat, lng, toko.lat, toko.lng).toFixed(2);

          document.getElementById("jarak").value = jarak;
          document.getElementById("lokasi-info").innerText =
            `Lokasi ditemukan! Estimasi jarak: ${jarak} KM`;

          renderCart();
        },
        () => alert("Gagal mengambil lokasi. Izinkan akses GPS.")
      );
    }

    /* ============================================
       RINGKASAN TOTAL
    ============================================ */
    function renderCart() {
      const cartDiv = document.getElementById("cart-items");
      const summary = document.getElementById("summary");

      if (cart.length === 0) {
        cartDiv.innerHTML = `<p class="text-gray-600">Keranjang kosong.</p>`;
        summary.innerHTML = "";
        document.getElementById("cart-count").innerText = "(0)";
        return;
      }

      cartDiv.innerHTML = cart.map(i => `
        <div class="flex justify-between items-center">
          <div>
            <p class="font-semibold text-green-900">${i.name}</p>
            <p class="text-sm">Rp ${i.price.toLocaleString()}</p>
            <p class="text-sm">Subtotal: Rp ${(i.price * i.qty).toLocaleString()}</p>
          </div>

          <div class="flex items-center space-x-2">
            <button onclick="updateQty(${i.id}, -1)" class="px-2 bg-red-500 text-white rounded">-</button>
            <span>${i.qty}</span>
            <button onclick="updateQty(${i.id}, 1)" class="px-2 bg-green-700 text-white rounded">+</button>
          </div>
        </div>
      `).join("");

      const subtotal = cart.reduce((a, b) => a + b.price * b.qty, 0);

      // Ongkir per km (Rp 2.000/km)
      const jarak = parseFloat(document.getElementById("jarak").value || 0);
      const ongkir = jarak * 2000;

      // Diskon
      let diskon = 0;
      if (appliedCoupon && subtotal >= appliedCoupon.min) {
        diskon = appliedCoupon.type === "percent"
          ? subtotal * (appliedCoupon.value / 100)
          : appliedCoupon.value;
      }

      const total = subtotal + ongkir - diskon;

      summary.innerHTML = `
        <p>Subtotal: <span class="float-right">Rp ${subtotal.toLocaleString()}</span></p>
        <p>Ongkir: <span class="float-right">Rp ${ongkir.toLocaleString()}</span></p>
        <p>Diskon: <span class="float-right">- Rp ${diskon.toLocaleString()}</span></p>
        <hr class="my-2">
        <p class="font-bold">Total: <span class="float-right">Rp ${total.toLocaleString()}</span></p>
      `;

      document.getElementById("cart-count").innerText = `(${cart.length})`;
    }

    /* ============================================
       CHECKOUT
    ============================================ */
    function checkout() {
      const nama = document.getElementById("nama").value;
      const tel = document.getElementById("telepon").value;
      const alamat = document.getElementById("alamat").value;
      const jarak = document.getElementById("jarak").value;
      const payment = document.getElementById("payment").value;

      if (!nama || !tel || !alamat) {
        alert("Lengkapi data pengiriman.");
        return;
      }

      const subtotal = cart.reduce((a, b) => a + b.price * b.qty, 0);
      const ongkir = jarak * 2000;
      let diskon = 0;

      if (appliedCoupon && subtotal >= appliedCoupon.min)
        diskon = appliedCoupon.type === "percent"
          ? subtotal * (appliedCoupon.value / 100)
          : appliedCoupon.value;

      const total = subtotal + ongkir - diskon;

      const pesan = `
*Checkout Tomodachi Matcha*
Nama: ${nama}
Telepon: ${tel}
Alamat: ${alamat}

Pesanan:
${cart.map(i => `- ${i.name} x${i.qty}`).join("\n")}

Subtotal: Rp ${subtotal.toLocaleString()}
Ongkir: Rp ${ongkir.toLocaleString()}
Diskon: Rp ${diskon.toLocaleString()}
Total: *Rp ${total.toLocaleString()}*

Metode Pembayaran: ${payment}
      `;

      window.open("https://wa.me/6281234567890?text=" + encodeURIComponent(pesan));
    }

    renderProducts();
    renderCart();
  </script>

</body>
</html>
