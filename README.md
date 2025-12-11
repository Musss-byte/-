<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Tomodachi Matcha — Single File</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-green-50 text-gray-900">
  <header class="p-4 bg-green-600 text-white shadow flex items-center gap-3">
    <img src="https://i.imgur.com/8Km9tLL.jpeg" class="w-10 h-10 rounded-full border" />
    <h1 class="text-2xl font-bold">Tomodachi Matcha</h1>
  </header>

  <main class="p-4 max-w-4xl mx-auto">
    <h2 class="text-xl font-semibold mb-4">Produk</h2>
    <div id="productList" class="grid grid-cols-2 gap-4"></div>

    <h2 class="text-xl font-semibold mt-8 mb-4">Keranjang</h2>
    <div id="cartList" class="space-y-2"></div>
    <button id="openCheckout" class="mt-4 w-full bg-green-600 text-white p-3 rounded-lg">Checkout</button>
  </main>

  <!-- MODAL CHECKOUT -->
  <div id="checkoutModal" class="fixed inset-0 bg-black/50 hidden items-center justify-center p-4">
    <div class="bg-white p-6 rounded-lg w-full max-w-md shadow">
      <h2 class="text-xl font-bold mb-4">Checkout</h2>

      <label class="block mb-2 text-sm font-semibold">Nama</label>
      <input id="nama" class="w-full p-2 border rounded mb-3" placeholder="Masukkan nama" />

      <label class="block mb-2 text-sm font-semibold">Alamat</label>
      <textarea id="alamat" class="w-full p-2 border rounded mb-3" placeholder="Masukkan alamat"></textarea>

      <label class="block mb-2 text-sm font-semibold">Nomor Telepon</label>
      <input id="telepon" class="w-full p-2 border rounded mb-3" placeholder="08xxxxxxxx" />

      <div class="flex gap-2 mt-4">
        <button id="closeModal" class="w-1/2 bg-gray-300 p-2 rounded">Batal</button>
        <button id="checkoutBtn" class="w-1/2 bg-green-600 text-white p-2 rounded">Pesan via WhatsApp</button>
      </div>
    </div>
  </div>

  <script>
    const products = [
      { id: 1, name: "Matcha Premium", price: 45000, img: "https://i.imgur.com/8Km9tLL.jpeg" },
      { id: 2, name: "Matcha Latte", price: 35000, img: "https://i.imgur.com/8Km9tLL.jpeg" },
      { id: 3, name: "Matcha Cookies", price: 30000, img: "https://i.imgur.com/8Km9tLL.jpeg" },
      { id: 4, name: "Matcha Milk", price: 38000, img: "https://i.imgur.com/8Km9tLL.jpeg" },
      { id: 5, name: "Matcha Ice Cream", price: 25000, img: "https://i.imgur.com/8Km9tLL.jpeg" }
    ];

    const cart = [];

    function renderProducts() {
      const list = document.getElementById("productList");
      list.innerHTML = products.map(p => `
        <div class='bg-white p-3 rounded shadow'>
          <img src='${p.img}' class='w-full rounded mb-2'/>
          <div class='font-semibold'>${p.name}</div>
          <div class='mb-2'>Rp ${p.price.toLocaleString()}</div>
          <button onclick='addToCart(${p.id})' class='bg-green-600 text-white px-3 py-2 rounded w-full'>Tambah</button>
        </div>`).join("");
    }

    function renderCart() {
      const list = document.getElementById("cartList");
      if (cart.length === 0) {
        list.innerHTML = "<div class='text-gray-500'>Keranjang kosong</div>";
        document.getElementById("subtotalBox").innerHTML = "Rp 0";
        document.getElementById("totalBox").innerHTML = "Rp 0";
        return;
      }

      let html = "";
      cart.forEach(item => {
        html += `
        <div class='flex justify-between items-center bg-white p-3 rounded-xl shadow border'>
          <div class='flex flex-col'>
            <span class='font-semibold'>${item.name}</span>
            <span class='text-sm text-gray-600'>Rp ${item.price.toLocaleString()} x ${item.qty}</span>
          </div>
          <div class='flex items-center gap-2'>
            <button onclick='changeQty(${item.id}, -1)' class='px-3 py-1 border rounded-lg'>-</button>
            <span class='font-semibold w-6 text-center'>${item.qty}</span>
            <button onclick='changeQty(${item.id}, 1)' class='px-3 py-1 border rounded-lg'>+</button>
            <button onclick='removeItem(${item.id})' class='text-red-600 font-semibold ml-2'>Hapus</button>
          </div>
        </div>`;
      });

      list.innerHTML = html;

      const subtotal = cart.reduce((a, b) => a + b.price * b.qty, 0);
      const ongkir = 10000;
      const discount = activeCoupon === "MATCHA10" ? subtotal * 0.10 : 0;
      const total = subtotal + ongkir - discount;

      document.getElementById("subtotalBox").innerHTML = `Rp ${subtotal.toLocaleString()}`;
      document.getElementById("discountBox").innerHTML = `Rp ${discount.toLocaleString()}`;
      document.getElementById("ongkirBox").innerHTML = `Rp ${ongkir.toLocaleString()}`;
      document.getElementById("totalBox").innerHTML = `Rp ${total.toLocaleString()}`;
    }

    function changeQty(id, delta) {
      const item = cart.find(i => i.id === id);
      if (!item) return;
      item.qty += delta;
      if (item.qty <= 0) removeItem(id);
      renderCart();
    }

    function removeItem(id) {
      const index = cart.findIndex(i => i.id === id);
      if (index !== -1) cart.splice(index, 1);
      renderCart();
    }

    let activeCoupon = null;
    function applyCoupon() {
      const code = document.getElementById("couponInput").value.trim();
      if (code === "MATCHA10") {
        activeCoupon = code;
        alert("Kupon berhasil! Diskon 10% diterapkan.");
      } else {
        activeCoupon = null;
        alert("Kupon tidak valid.");
      }
      renderCart();
    }

    function changeQty(id, delta) {
      const item = cart.find(i => i.id === id);
      if (!item) return;
      item.qty += delta;
      if (item.qty <= 0) {
        const index = cart.indexOf(item);
        cart.splice(index, 1);
      }
      renderCart();
    }
      list.innerHTML = cart.map(item => `
        <div class='flex justify-between bg-white p-3 rounded shadow'>
          <span>${item.name}</span>
          <span>Rp ${item.price.toLocaleString()}</span>
        </div>`).join("");
    }

    function addToCart(id) {
      const product = products.find(p => p.id === id);
      cart.push(product);
      renderCart();
    }

    document.getElementById("openCheckout").onclick = () => {
      if (cart.length === 0) return alert("Keranjang masih kosong!");
      document.getElementById("checkoutModal").classList.remove("hidden");
    };

    document.getElementById("closeModal").onclick = () => {
      document.getElementById("checkoutModal").classList.add("hidden");
    };

    document.getElementById("checkoutBtn").onclick = () => {
      const nama = document.getElementById("nama").value.trim();
      const alamat = document.getElementById("alamat").value.trim();
      const telepon = document.getElementById("telepon").value.trim();

      if (!nama || !alamat || !telepon)
        return alert("Semua data wajib diisi!");

      const text = cart.map(c => `- ${c.name} (Rp ${c.price.toLocaleString()})`).join("%0A");
      const total = cart.reduce((a, b) => a + b.price, 0);

      const url = `https://wa.me/6281234567890?text=Halo,%20saya%20${nama}%0AAlamat:%20${alamat}%0ATelepon:%20${telepon}%0A%0ASaya%20ingin%20pesan:%0A${text}%0ATotal:%20Rp${total.toLocaleString()}`;

      window.open(url, "_blank");
    };

    renderProducts();
    renderCart();
  </script>
<script>
// --- LocalStorage Load ---
let saved = localStorage.getItem('cart');
if(saved){ cart = JSON.parse(saved); renderCart(); }

// --- Save to LocalStorage on Update ---
function saveCart(){ localStorage.setItem('cart', JSON.stringify(cart)); }

// Modify existing updateCart calls
function updateQty(i,delta){ cart[i].qty=Math.max(1,cart[i].qty+delta); saveCart(); renderCart(); }
function removeItem(i){ cart.splice(i,1); saveCart(); renderCart(); }

// --- Shipping Options ---
const shippingOptions = {
  'jne': 10000,
  'jnt': 12000,
  'sicepat': 9000
};

// Update total calculation
document.getElementById('shippingSelect').addEventListener('change', renderCart);

// --- WhatsApp Checkout ---
function checkoutWA(){
  const name=document.getElementById('name').value;
  const addr=document.getElementById('address').value;
  const phone=document.getElementById('phone').value;
  const shipping=document.getElementById('shippingSelect').value;
  let text=`Halo, saya ingin memesan:%0A`;
  cart.forEach(i=>{ text+=`- ${i.name} x${i.qty} = Rp${i.price*i.qty}%0A`; });
  text+=`%0AOngkir: ${shipping}%0A`;
  text+=`Nama: ${name}%0AAlamat: ${addr}%0ANo HP: ${phone}`;
  window.open(`https://wa.me/6280000000000?text=${text}`);
}
</script>
<script>
// --- Payment Methods ---
function getPaymentMethod(){
  return document.querySelector('input[name="paymentMethod"]:checked')?.value || 'COD';
}

// --- City-based Shipping Cost ---
const cityShipping = {
  "Jakarta": 10000,
  "Bandung": 12000,
  "Surabaya": 15000,
  "Medan": 18000,
  "Makassar": 20000
};

document.getElementById('citySelect').addEventListener('change', renderCart);

// Modify total calculation to include city shipping
function getCityOngkir(){
  const city = document.getElementById('citySelect').value;
  return cityShipping[city] || 0;
}

// --- WhatsApp Checkout Updated ---
function checkoutWA(){
  const name=document.getElementById('name').value;
  const addr=document.getElementById('address').value;
  const phone=document.getElementById('phone').value;
  const city=document.getElementById('citySelect').value;
  const payment=getPaymentMethod();
  let text=`Halo, saya ingin memesan:%0A`;
  cart.forEach(i=>{ text+=`- ${i.name} x${i.qty} = Rp${i.price*i.qty}%0A`; });
  text+=`%0AOngkir Kota (${city}): Rp${getCityOngkir()}%0A`;
  text+=`Metode Pembayaran: ${payment}%0A`;
  text+=`%0AData Pemesan:%0A`;
  text+=`Nama: ${name}%0AAlamat: ${addr}%0ANo HP: ${phone}`;
  window.open(`https://wa.me/6280000000000?text=${text}`);
}
</script>
</body>
</html>
