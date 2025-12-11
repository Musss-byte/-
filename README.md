<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Tomodachi Matcha</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/heroicons@2.0.18/dist/heroicons.js"></script>
  <style>
    body { background-color: #f6fdf7; }
    #cart-panel { transform: translateX(100%); transition: 0.3s ease; }
    #cart-panel.open { transform: translateX(0); }
  </style>
</head>
<body class="text-green-900">

  <!-- NAVBAR -->
  <nav class="flex items-center justify-between py-4 px-6 bg-white shadow">
    <div class="flex items-center gap-3">
      <img src="logo.png" alt="Logo" class="h-10 w-10 rounded-full">
      <h1 class="text-2xl font-bold text-green-900">Tomodachi Matcha</h1>
    </div>
    <div class="flex items-center gap-6">
      <a href="#produk" class="font-semibold">Produk</a>
      <button id="cart-btn" class="relative">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.8" stroke="currentColor" class="w-7 h-7 text-green-900">
          <path stroke-linecap="round" stroke-linejoin="round" d="M2.25 3h1.386c.51 0 .955.343 1.087.836l.383 1.437M7.5 14.25a3 3 0 1 0 0 6 3 3 0 0 0 0-6Zm9 3a3 3 0 1 1-6 0 m6 0a3 3 0 1 0 6 0 m-6 0H9m12-3.75-1.5-6.75H4.106M20.25 10.5H6.857" />
        </svg>
        <span id="cart-count" class="absolute -top-1 -right-1 bg-red-500 text-white text-xs px-1.5 py-0.5 rounded-full hidden">0</span>
      </button>
    </div>
  </nav>

  <!-- HEADER -->
  <header class="text-center py-10">
    <h2 class="text-3xl font-bold text-green-800">Selamat Datang di Tomodachi Matcha</h2>
    <p class="mt-2 text-green-700">Matcha premium, rasa original Jepang 🍵</p>
  </header>

  <!-- PRODUK -->
  <section id="produk" class="px-6">
    <h3 class="text-2xl font-bold mb-4">Produk Kami</h3>
    <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
      <!-- PRODUK 1 -->
      <div class="bg-white p-4 rounded-xl shadow border">
        <img src="produk1.jpg" class="rounded-lg w-full h-40 object-cover" />
        <h4 class="font-bold mt-2">Matcha Latte</h4>
        <p class="text-green-700">Rp 18.000</p>
        <button onclick="addToCart('Matcha Latte',18000)" class="mt-2 bg-green-600 text-white px-4 py-2 rounded-lg w-full">Tambah</button>
      </div>
      <!-- PRODUK 2 -->
      <div class="bg-white p-4 rounded-xl shadow border">
        <img src="produk2.jpg" class="rounded-lg w-full h-40 object-cover" />
        <h4 class="font-bold mt-2">Matcha Original</h4>
        <p class="text-green-700">Rp 15.000</p>
        <button onclick="addToCart('Matcha Original',15000)" class="mt-2 bg-green-600 text-white px-4 py-2 rounded-lg w-full">Tambah</button>
      </div>
      <!-- PRODUK 3 -->
      <div class="bg-white p-4 rounded-xl shadow border">
        <img src="produk3.jpg" class="rounded-lg w-full h-40 object-cover" />
        <h4 class="font-bold mt-2">Matcha Hazelnut</h4>
        <p class="text-green-700">Rp 20.000</p>
        <button onclick="addToCart('Matcha Hazelnut',20000)" class="mt-2 bg-green-600 text-white px-4 py-2 rounded-lg w-full">Tambah</button>
      </div>
      <!-- PRODUK 4 -->
      <div class="bg-white p-4 rounded-xl shadow border">
        <img src="produk4.jpg" class="rounded-lg w-full h-40 object-cover" />
        <h4 class="font-bold mt-2">Matcha Tiramisu</h4>
        <p class="text-green-700">Rp 22.000</p>
        <button onclick="addToCart('Matcha Tiramisu',22000)" class="mt-2 bg-green-600 text-white px-4 py-2 rounded-lg w-full">Tambah</button>
      </div>
      <!-- PRODUK 5 -->
      <div class="bg-white p-4 rounded-xl shadow border">
        <img src="produk5.jpg" class="rounded-lg w-full h-40 object-cover" />
        <h4 class="font-bold mt-2">Matcha Red Bean</h4>
        <p class="text-green-700">Rp 19.000</p>
        <button onclick="addToCart('Matcha Red Bean',19000)" class="mt-2 bg-green-600 text-white px-4 py-2 rounded-lg w-full">Tambah</button>
      </div>
    </div>
  </section>

  <!-- GPS -->
  <section class="px-6 mt-10">
    <h3 class="text-xl font-bold mb-3">Ambil Lokasi Otomatis (GPS)</h3>
    <button onclick="getLocation()" class="bg-green-700 text-white px-5 py-2 rounded-lg active:scale-95 shadow">Ambil Lokasi Saya 📍</button>
    <p class="mt-2 text-green-700" id="gps-result"></p>
  </section>

  <!-- GOOGLE MAPS -->
  <section class="px-6 mt-10">
    <h3 class="text-xl font-bold">Lokasi Bisnis Kami</h3>
    <p class="text-green-700">Jl Pembangunan RT 02 RW 05 Kedung Halang, Bogor Utara</p>
    <div class="max-w-xl mt-4 rounded-xl overflow-hidden shadow-lg border border-green-200">
      <iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3967.31372696543!2d106.8279876!3d-6.562045199999999!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x2e69c9c81cecc7df%3A0x8a83b2e1fa89bbf0!2sKedung%20Halang%2C%20Bogor%20Utara!5e0!3m2!1sid!2sid!4v1700000000000!5m2!1sid!2sid" width="100%" height="300" allowfullscreen loading="lazy"></iframe>
    </div>
  </section>

  <!-- ABOUT -->
  <section class="px-6 mt-10">
    <h3 class="text-xl font-bold">Tentang Kami</h3>
    <p class="text-green-700 mt-2">
      Tomodachi Matcha menghadirkan minuman matcha premium dengan cita rasa autentik Jepang.
      Dibuat dari bahan alami tanpa pengawet.
    </p>
  </section>

  <!-- CONTACT -->
  <section class="px-6 mt-10">
    <h3 class="text-xl font-bold">Kontak Kami</h3>
    <p class="mt-2">📞 WhatsApp: <a href="https://wa.me/6283879528983" class="text-green-700 underline">Hubungi Kami</a></p>
    <p>📍 Bogor Utara, Kota Bogor</p>
  </section>

  <!-- FOOTER -->
  <footer class="text-center py-10 mt-10 text-green-800">
    <p>© 2025 Tomodachi Matcha. All Rights Reserved.</p>
  </footer>

  <!-- SIDEBAR KERANJANG -->
  <div id="cart-panel" class="fixed top-0 right-0 w-80 h-full bg-white shadow-xl p-5 border-l border-green-200 z-50">
    <h2 class="text-xl font-bold mb-3 flex justify-between items-center">Keranjang 
      <button onclick="closeCart()" class="bg-red-500 text-white px-2 py-1 rounded">X</button>
    </h2>
    <!-- Scrollable cart -->
    <div id="cart-items" class="mb-4 max-h-[60vh] overflow-y-auto"></div>
    <p class="font-bold">Total: Rp <span id="total-price">0</span></p>
    <button onclick="openCheckout()" class="w-full bg-green-600 text-white py-2 rounded-lg mt-4">Checkout via WhatsApp</button>
    <button onclick="closeCart()" class="w-full bg-red-500 text-white py-2 rounded-lg mt-2">Tutup Keranjang</button>
  </div>

  <!-- MODAL CHECKOUT WA -->
  <div id="checkout-modal" class="fixed inset-0 bg-black/40 hidden items-center justify-center z-50">
    <div class="bg-white p-6 rounded-xl w-80 text-center">
      <h3 class="text-xl font-bold mb-3">Checkout</h3>
      <input id="buyer-name" placeholder="Nama lengkap" class="w-full border p-2 rounded mb-3">
      <textarea id="buyer-address" placeholder="Alamat lengkap" class="w-full border p-2 rounded mb-3"></textarea>
      <input id="buyer-wa" placeholder="Nomor WhatsApp" class="w-full border p-2 rounded mb-3">
      <p class="font-bold">Ongkir otomatis: <span id="ongkir-display">Rp 0</span></p>

      <!-- Pilihan pembayaran -->
      <div class="flex flex-col gap-2 mb-3">
        <button onclick="pilihMetode('cod')" class="bg-yellow-500 text-white py-2 rounded-lg">COD (Bayar di Tempat)</button>
        <button onclick="pilihMetode('transfer'); openTransfer()" class="bg-blue-500 text-white py-2 rounded-lg">Transfer Bank</button>
        <button onclick="pilihMetode('qris'); openQRIS()" class="bg-green-700 text-white py-2 rounded-lg">QRIS</button>
      </div>

      <input type="file" id="bukti-file" onchange="uploadBuktiFoto(this)" class="mb-3">
      <button onclick="sendWa()" class="mt-2 bg-green-700 text-white px-4 py-2 rounded-lg w-full">Kirim Pesanan via WhatsApp</button>
      <button onclick="closeCheckout()" class="mt-2 bg-red-500 text-white px-4 py-2 rounded-lg w-full">Tutup</button>
    </div>
  </div>

  <!-- MODAL QRIS -->
  <div id="qris-modal" class="fixed inset-0 bg-black/40 hidden items-center justify-center z-50">
    <div class="bg-white p-6 rounded-xl w-80 text-center">
      <h3 class="text-xl font-bold mb-3">Pembayaran QRIS</h3>
      <img src="qris.png" id="qris-img" class="w-full rounded-lg border shadow" />
      <p class="mt-3 text-green-700">Scan QR untuk membayar</p>
      <button onclick="closeQRIS()" class="mt-4 bg-green-700 text-white px-4 py-2 rounded-lg w-full">Selesai</button>
    </div>
  </div>

  <!-- MODAL TRANSFER -->
  <div id="transfer-modal" class="fixed inset-0 bg-black/40 hidden items-center justify-center z-50">
    <div class="bg-white p-6 rounded-xl w-80 text-center">
      <h3 class="text-xl font-bold mb-3">Pembayaran Transfer</h3>
      <div class="flex flex-col gap-2 text-left">
        <div class="border p-2 rounded cursor-pointer" onclick="pilihBank('BNI','1234567890')"><b>BNI</b> - No Rek: 1234567890</div>
        <div class="border p-2 rounded cursor-pointer" onclick="pilihBank('BCA','0987654321')"><b>BCA</b> - No Rek: 0987654321</div>
        <div class="border p-2 rounded cursor-pointer" onclick="pilihBank('BRI','1122334455')"><b>BRI</b> - No Rek: 1122334455</div>
      </div>
      <button onclick="closeTransfer()" class="mt-4 bg-green-700 text-white px-4 py-2 rounded-lg w-full">Tutup</button>
    </div>
  </div>

  <!-- MODAL BUKTI TRANSAKSI -->
  <div id="bukti-modal" class="fixed inset-0 bg-black/40 hidden items-center justify-center z-50">
    <div class="bg-white p-6 rounded-xl w-80 text-center">
      <h3 class="text-xl font-bold mb-3">Bukti Transaksi</h3>
      <div id="bukti-content" class="text-green-700 text-left"></div>
      <button onclick="closeBukti()" class="mt-4 bg-green-700 text-white px-4 py-2 rounded-lg w-full">Tutup</button>
    </div>
  </div>

  <!-- SCRIPT -->
  <script>
let cart=[]; let ongkirValue=0; const TOKO_LAT=-6.567778, TOKO_LON=106.825135;
let pembayaranMetode="", buktiFoto=null;

function addToCart(n,p){
  const exist = cart.find(i=>i.name===n);
  if(exist){ exist.qty++; } else { cart.push({name:n,price:p,qty:1}); }
  updateCartCount();
  renderCart();
}

function renderCart(){
  let html="", total=0;
  cart.forEach((i,index)=>{
    total += i.price*i.qty;
    html+=`
      <div class="flex justify-between items-center border-b py-2">
        <div>
          <span>${i.name}</span>
          <div class="flex items-center gap-1 mt-1">
            <button onclick="updateQty(${index},-1)" class="bg-gray-300 px-2 rounded">-</button>
            <span>${i.qty}</span>
            <button onclick="updateQty(${index},1)" class="bg-gray-300 px-2 rounded">+</button>
          </div>
        </div>
        <span>Rp ${i.price*i.qty}</span>
      </div>`;
  });
  let subtotal = total + ongkirValue;
  html+=`<div class="flex justify-between border-t font-bold py-2 mt-2"><span>Ongkir</span><span>Rp ${ongkirValue}</span></div>
        <div class="flex justify-between border-t font-bold py-2 mt-1"><span>Subtotal</span><span>Rp ${subtotal}</span></div>`;
  document.getElementById("cart-items").innerHTML = html;
  document.getElementById("total-price").innerText = subtotal;
}

function updateQty(index,change){
  cart[index].qty += change;
  if(cart[index].qty<1){ cart.splice(index,1); } // hapus produk jika qty 0
  updateCartCount();
  renderCart();
}

function updateCartCount(){
  const count = cart.length;
  const el = document.getElementById("cart-count");
  if(count>0){ el.innerText=count; el.classList.remove("hidden"); }
  else{ el.classList.add("hidden"); }
}

document.getElementById("cart-btn").onclick = ()=>{ document.getElementById("cart-panel").classList.add("open"); }
function closeCart(){ document.getElementById("cart-panel").classList.remove("open"); }
function openCheckout(){ document.getElementById("checkout-modal").classList.remove("hidden"); ambilLokasiPembeli(); }
function closeCheckout(){ document.getElementById("checkout-modal").classList.add("hidden"); }

function pilihMetode(m){ pembayaranMetode=m; }
function uploadBuktiFoto(input){ if(input.files && input.files[0]) buktiFoto=input.files[0]; }

function openQRIS(){ if(pembayaranMetode!=='qris'){ alert("Pilih metode pembayaran QRIS."); return; } document.getElementById("qris-modal").classList.remove("hidden"); }
function closeQRIS(){ document.getElementById("qris-modal").classList.add("hidden"); }

function openTransfer(){ if(pembayaranMetode!=='transfer'){ alert("Pilih metode pembayaran Transfer."); return; } document.getElementById("transfer-modal").classList.remove("hidden"); }
function closeTransfer(){ document.getElementById("transfer-modal").classList.add("hidden"); }

function pilihBank(bank,noRek){ alert("Anda memilih "+bank+"\nNo Rek: "+noRek+"\nSilakan transfer dan upload bukti pembayaran."); closeTransfer(); }

function sendWa(){
  const nama=document.getElementById("buyer-name").value;
  const alamat=document.getElementById("buyer-address").value;
  const wa=document.getElementById("buyer-wa").value;
  if(!nama||!alamat||!wa){ alert("Lengkapi data checkout."); return; }
  if((pembayaranMetode==='qris'||pembayaranMetode==='transfer')&&!buktiFoto){ alert("Upload bukti pembayaran terlebih dahulu."); return; }

  let cartText=cart.map(i=>`- ${i.name} x${i.qty} (Rp ${i.price*i.qty})`).join("\n");
  const totalHarga=cart.reduce((t,i)=>t+i.price*i.qty,0);
  const totalAkhir=totalHarga+ongkirValue;

  let buktiHtml=`<p><b>Nama:</b> ${nama}</p><p><b>Alamat:</b> ${alamat}</p><p><b>No WA:</b> ${wa}</p><p><b>Metode:</b> ${pembayaranMetode}</p><p><b>Total:</b> Rp ${totalAkhir}</p>`;
  if(buktiFoto){ buktiHtml+=`<p><b>Bukti Pembayaran:</b> ${buktiFoto.name}</p>`; }
  document.getElementById("bukti-content").innerHTML=buktiHtml;
  document.getElementById("bukti-modal").classList.remove("hidden");

  let message=`Halo Tomodachi Matcha!\nSaya ingin memesan:\n${cartText}\nOngkir: Rp ${ongkirValue}\nTotal: Rp ${totalAkhir}\nNama: ${nama}\nAlamat: ${alamat}\nNo WA: ${wa}\nMetode: ${pembayaranMetode}`;
  if(buktiFoto){ message+="\nBukti: "+buktiFoto.name; }
  window.open(`https://wa.me/6283879528983?text=${encodeURIComponent(message)}`);
}

function ambilLokasiPembeli(){
  if(!navigator.geolocation){ alert("GPS tidak tersedia."); return; }
  navigator.geolocation.getCurrentPosition(pos=>{
    const lat=pos.coords.latitude, lon=pos.coords.longitude;
    const jarak=hitungJarak(lat,lon,TOKO_LAT,TOKO_LON);
    ongkirValue=Math.round(jarak*5000);
    document.getElementById("ongkir-display").innerText="Rp "+ongkirValue;
    renderCart();
  });
}

function hitungJarak(lat1,lon1,lat2,lon2){
  const R=6371;
  let dLat=(lat2-lat1)*Math.PI/180, dLon=(lon2-lon1)*Math.PI/180;
  let a=Math.sin(dLat/2)**2+Math.cos(lat1*Math.PI/180)*Math.cos(lat2*Math.PI/180)*Math.sin(dLon/2)**2;
  let c=2*Math.atan2(Math.sqrt(a),Math.sqrt(1-a));
  return R*c;
}

function getLocation(){
  if(!navigator.geolocation){ document.getElementById("gps-result").innerText="Browser tidak mendukung GPS."; return; }
  navigator.geolocation.getCurrentPosition(pos=>{
    const lat=pos.coords.latitude, lon=pos.coords.longitude;
    document.getElementById("gps-result").innerHTML="Lokasi Anda:<br>Lat: "+lat+"<br>Lon: "+lon;
  },()=>{ document.getElementById("gps-result").innerText="Tidak dapat mengambil lokasi."; });
}

function closeBukti(){ document.getElementById("bukti-modal").classList.add("hidden"); }
  </script>

</body>
</html>
