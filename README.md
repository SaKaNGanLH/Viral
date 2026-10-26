
<!DOCTYPE html>
<html lang="id">
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Berbagi Lokasi</title>
<body style="font-family:Arial;text-align:center;padding:24px">
<h2>Berbagi Lokasi</h2>
<p>Tekan tombol dan izinkan browser mengakses lokasi Anda.</p>
<button onclick="lokasi()">Bagikan lokasi saya</button>
<p id="hasil"></p>
<script>
function lokasi() {
  const hasil = document.getElementById("hasil");
  if (!navigator.geolocation) {
    hasil.textContent = "Browser tidak mendukung lokasi.";
    return;
  }
  hasil.textContent = "Menunggu izin lokasi...";
  navigator.geolocation.getCurrentPosition(
    p => {
      const url = "https://maps.google.com/?q="
        + p.coords.latitude + "," + p.coords.longitude;
      hasil.replaceChildren();
      const a = document.createElement("a");
      a.href = url;
      a.textContent = "Buka lokasi saya di Google Maps";
      a.target = "_blank";
      a.rel = "noopener";
      hasil.appendChild(a);
      const salin = document.createElement("button");
      salin.textContent = "Salin link lokasi";
      salin.onclick = async () => {
        try {
          await navigator.clipboard.writeText(url);
          alert("Link disalin. Kirim lewat WhatsApp.");
        } catch {
          prompt("Salin link ini:", url);
        }
      };
      hasil.appendChild(document.createElement("br"));
      hasil.appendChild(salin);
    },
    e => {
      hasil.textContent =
        "Lokasi gagal diakses. Periksa izin browser dan GPS.";
    },
    {enableHighAccuracy:true,timeout:15000}
  );
}
</script>
</body>
</html>
