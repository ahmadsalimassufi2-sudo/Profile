<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>Selamat Tahun Baru Sayang</title>

<style>
body {
  font-family: 'Segoe UI', sans-serif;
  background: linear-gradient(135deg, #ffdee9, #b5fffc);
  text-align: center;
  padding: 40px;
}

h1 {
  color: #d63384;
}

button {
  background: #ff4d6d;
  color: white;
  border: none;
  padding: 15px 30px;
  border-radius: 30px;
  font-size: 18px;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background: #ff1e4d;
}

.hidden {
  display: none;
}

#pesan {
  margin-top: 30px;
  background: white;
  padding: 25px;
  border-radius: 20px;
  animation: fadeIn 1.5s ease;
}

.heart {
  font-size: 40px;
  animation: beat 1s infinite;
}

@keyframes beat {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.2); }
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
</head>

<body>

<h1>✨ Selamat Tahun Baru Sayang ✨</h1>
<p>Ada sesuatu yang ingin aku sampaikan untukmu…</p>

<button onclick="tampilkanPesan()">Tekan di Sini 💖</button>

<audio id="musik" src="lagu-romantis.mp3"></audio>

<div id="pesan" class="hidden">
  <div class="heart">❤️</div>

  <p>
    Sayangku, terima kasih sudah hadir di hidupku.  
    Sampai di tahun yang baru ini, aku ingin terus belajar menjadi pasangan yang lebih baik untukmu.  
    Semoga cinta kita selalu tumbuh, saling menguatkan, dan saling menjaga.
  </p>

  <p><strong>Doa untuk kita:</strong></p>

  <p>
    Ya Allah, satukanlah hati kami dalam kebaikan.  
    Jadikan hubungan kami penuh dengan kasih sayang, kejujuran, dan kesabaran.  
    Jauhkan kami dari hal-hal yang buruk.  
    Jika dia memang yang terbaik untukku, mudahkanlah jalan kami menuju ridho-Mu  
    dan berkahilah cinta kami di dunia hingga akhirat.  
    Aamiin Ya Rabbal 'Alamin.
  </p>

  <p style="margin-top:20px; font-weight:bold;">
    💌 Dari Aufi (Ahmad Salim Assufi)<br>
    untuk Tuan Putri tercinta (Putri Bunga Lestari)
  </p>

  <p style="
    margin-top:15px;
    font-size:14px;
    color:#555;
    font-style:italic;
  ">
    Kalau ingin membalas kata-katanya atau membalas doanya,  
    balas di chat WA aja yaa tuan putrikuuuu 💕  
    soalnya nggak mungkin bisa ditulis juga di web iniii 🥰🥰🥰
  </p>

  <div class="heart">🤍</div>
</div>

<script>
function tampilkanPesan() {
  document.getElementById("pesan").classList.remove("hidden");
  document.getElementById("musik").play();
}
</script>

</body>
</html>
