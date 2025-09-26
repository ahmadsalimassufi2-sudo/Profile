<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Global Name Generator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 40px;
      color: white;
      transition: background-image 1s ease-in-out;
      background-size: cover;
      background-position: center;
      background-attachment: fixed;
    }
    h1 {
      margin-bottom: 20px;
      text-shadow: 2px 2px 5px black;
    }
    input, select, button {
      padding: 10px 15px;
      margin: 10px;
      border: none;
      border-radius: 8px;
      font-size: 16px;
    }
    input#count {
      width: 100px;
      text-align: center;
      background-color: pink; /* merah muda */
    }
    button {
      background: #ff5722;
      color: white;
      cursor: pointer;
    }
    button:hover {
      background: #e64a19;
    }
    #result {
      margin-top: 20px;
      padding: 15px;
      background: rgba(0, 0, 0, 0.6);
      border-radius: 8px;
      min-height: 150px;
      text-align: left;
      font-family: monospace;
      color: white;
      overflow-y: auto;
      max-height: 400px;
    }
    .name-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 5px;
      border-bottom: 1px solid rgba(255,255,255,0.2);
    }
    .copy-btn {
      background: #007bff;
      color: white;
      border: none;
      border-radius: 6px;
      padding: 3px 8px;
      font-size: 14px;
      cursor: pointer;
    }
    .copy-btn:hover {
      background: #0056b3;
    }
  </style>
</head>
<body>
  <h1>🌍 Global Name Generator</h1>

  <label for="count">Jumlah nama: </label>
  <input type="number" id="count" min="1" max="1000" value="20">

  <label for="gender">Jenis Kelamin: </label>
  <select id="gender">
    <option value="female">Perempuan</option>
    <option value="male">Laki-laki</option>
    <option value="mixed" selected>Campur</option>
  </select>

  <label for="country">Negara: </label>
  <select id="country">
    <option value="indonesia">Indonesia</option>
    <option value="greece">Yunani</option>
    <option value="japan">Jepang</option>
    <option value="italy">Italia</option>
    <option value="france">Prancis</option>
    <option value="usa">Amerika</option>
    <option value="arab">Arab</option>
    <option value="uk">Inggris</option>
    <option value="finland">Finlandia</option>
    <option value="netherlands">Belanda</option>
    <option value="russia">Rusia</option>
    <option value="germany">Jerman</option>
    <option value="korea">Korea</option>
    <option value="china">Cina</option>
    <option value="india">India</option>
    <option value="turkey">Turki</option>
    <option value="spain">Spanyol</option>
    <option value="brazil">Brasil</option>
    <option value="egypt">Mesir</option>
    <option value="mexico">Meksiko</option>
  </select>

  <br>
  <button onclick="generateNames()">Generate Names</button>
  <button onclick="copyNames()">Salin Semua</button>
  <button onclick="refreshNames()">Refresh</button>

  <div id="result"></div>

  <script>
    // 50 Foto HD Unsplash perempuan luar negeri
    const backgrounds = [];
    for(let i=1;i<=50;i++){
      backgrounds.push(`https://source.unsplash.com/1600x900/?woman,portrait&sig=${i}`);
    }

    let preloadedImages = [];
    let loadedCount = 0;

    // preload semua gambar
    backgrounds.forEach((src) => {
      const img = new Image();
      img.src = src;
      img.onload = () => {
        loadedCount++;
        if (loadedCount === backgrounds.length) startSlideshow();
      }
      preloadedImages.push(img);
    });

    let bgIndex = 0;
    function startSlideshow() {
      document.body.style.backgroundImage = `url('${backgrounds[bgIndex]}')`;
      setInterval(() => {
        bgIndex = (bgIndex + 1) % backgrounds.length;
        document.body.style.backgroundImage = `url('${backgrounds[bgIndex]}')`;
      }, 2000);
    }

    // Data nama semua negara
    const namesData = {
      indonesia: {male:["Budi","Agus","Joko","Hendra","Rizky"],female:["Siti","Dewi","Ayu","Putri","Lestari"],last:["Santoso","Wijaya","Saputra","Pratama","Kusuma"]},
      greece: {male:["Nikos","Giorgos","Dimitris","Kostas","Christos"],female:["Maria","Eleni","Katerina","Sofia","Despina"],last:["Papadopoulos","Nikolaidis","Georgiou","Christakis","Stefanidis"]},
      japan: {male:["Haruto","Ren","Takumi","Riku","Souta"],female:["Yui","Sakura","Hina","Aoi","Miyu"],last:["Tanaka","Yamamoto","Suzuki","Takahashi","Kobayashi"]},
      italy: {male:["Luca","Marco","Giovanni","Francesco","Matteo"],female:["Giulia","Sofia","Aurora","Martina","Chiara"],last:["Rossi","Russo","Ferrari","Esposito","Bianchi"]},
      france: {male:["Jean","Pierre","Louis","Nicolas","Antoine"],female:["Marie","Camille","Sophie","Chloe","Juliette"],last:["Dubois","Lefevre","Moreau","Laurent","Simon"]},
      usa: {male:["James","Robert","John","Michael","William"],female:["Mary","Patricia","Jennifer","Linda","Elizabeth"],last:["Smith","Johnson","Williams","Brown","Jones"]},
      arab: {male:["Ahmed","Mohammed","Ali","Omar","Hassan"],female:["Fatima","Aisha","Zainab","Layla","Maryam"],last:["Al-Farsi","Ibn-Said","Al-Mansoor","Al-Hakim","Al-Khaled"]},
      uk: {male:["Oliver","George","Harry","Jack","Jacob"],female:["Olivia","Amelia","Isla","Emily","Sophia"],last:["Smith","Taylor","Wilson","Davies","Evans"]},
      finland: {male:["Mikael","Juha","Antti","Kari","Jari"],female:["Aino","Emilia","Sofia","Laura","Katja"],last:["Korhonen","Virtanen","Mäkinen","Nieminen","Heikkinen"]},
      netherlands: {male:["Jan","Peter","Johan","Daan","Lars"],female:["Anna","Emma","Sanne","Lisa","Sofie"],last:["deJong","Jansen","Bakker","Visser","Smit"]},
      russia: {male:["Ivan","Dmitri","Sergei","Nikolai","Alexei"],female:["Anastasia","Olga","Natalia","Ekaterina","Maria"],last:["Ivanov","Petrov","Sidorov","Volkov","Smirnov"]},
      germany: {male:["Hans","Peter","Karl","Stefan","Lukas"],female:["Anna","Mia","Lea","Lena","Sophie"],last:["Müller","Schmidt","Fischer","Weber","Meyer"]},
      korea: {male:["Min-Jun","Ji-Hoon","Hyun-Woo","Sung-Min","Jae-Won"],female:["Seo-Yeon","Ji-Eun","Ha-Young","Soo-Min","Ye-Jin"],last:["Kim","Lee","Park","Choi","Jung"]},
      china: {male:["Wei","Hao","Jun","Peng","Ming"],female:["Li","Mei","Xiu","Hua","Yan"],last:["Wang","Li","Zhang","Chen","Liu"]},
      india: {male:["Arjun","Ravi","Amit","Rajesh","Vikram"],female:["Priya","Anita","Kavita","Sunita","Lakshmi"],last:["Sharma","Patel","Gupta","Mehta","Reddy"]},
      turkey: {male:["Mehmet","Ahmet","Mustafa","Ali","Yusuf"],female:["Ayşe","Fatma","Zeynep","Elif","Hatice"],last:["Yılmaz","Demir","Şahin","Çelik","Arslan"]},
      spain: {male:["Jose","Antonio","Manuel","Francisco","Carlos"],female:["Maria","Carmen","Josefa","Isabel","Ana"],last:["Garcia","Martinez","Lopez","Sanchez","Gonzalez"]},
      brazil: {male:["Joao","Jose","Antonio","Francisco","Carlos"],female:["Maria","Ana","Francisca","Antonia","Adriana"],last:["Silva","Santos","Oliveira","Souza","Rodrigues"]},
      egypt: {male:["Mohamed","Ahmed","Mahmoud","Mostafa","Omar"],female:["Fatma","Aya","Nour","Sara","Mona"],last:["Hassan","Mahmoud","Ibrahim","Youssef","Ali"]},
      mexico: {male:["Jose","Juan","Luis","Carlos","Jorge"],female:["Maria","Guadalupe","Juana","Rosa","Carmen"],last:["Hernandez","Garcia","Martinez","Lopez","Gonzalez"]}
    };

    function generateNames() {
      let count = parseInt(document.getElementById("count").value);
      let gender = document.getElementById("gender").value;
      let country = document.getElementById("country").value;

      if (isNaN(count) || count <= 0) {
        alert("Masukkan jumlah nama yang valid!");
        return;
      }

      let generated = new Set();
      let pool = namesData[country];

      while (generated.size < count) {
        let first;
        if (gender === "female") {
          first = pool.female[Math.floor(Math.random() * pool.female.length)];
        } else if (gender === "male") {
          first = pool.male[Math.floor(Math.random() * pool.male.length)];
        } else {
          let g = Math.random() < 0.5 ? pool.female : pool.male;
          first = g[Math.floor(Math.random() * g.length)];
        }

        let last = pool.last[Math.floor(Math.random() * pool.last.length)];
        let randomNum = Math.floor(100 + Math.random() * 900);
        let fullName = first + "." + last + randomNum;

        generated.add(fullName);
      }

      let container = document.getElementById("result");
      container.innerHTML = "";

      Array.from(generated).forEach(name => {
        let div = document.createElement("div");
        div.className = "name-item";
        div.innerHTML = `<span>${name}</span> 
                         <button class="copy-btn" onclick="copyOne('${name}')">Salin</button>`;
        container.appendChild(div);
      });
    }

    function copyNames() {
      let names = Array.from(document.querySelectorAll(".name-item span"))
                       .map(el => el.innerText).join("\n");
      if (names.trim() === "") {
        alert("Belum ada nama yang dihasilkan!");
        return;
      }
      navigator.clipboard.writeText(names).then(() => {
        alert("Semua nama berhasil disalin!");
      });
    }

    function copyOne(name) {
      navigator.clipboard.writeText(name).then(() => {
        alert(`Nama "${name}" berhasil disalin!`);
      });
    }

    function refreshNames() {
      document.getElementById("result").innerHTML = "";
    }
  </script>
</body>
</html>
