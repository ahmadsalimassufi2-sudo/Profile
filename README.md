<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Generator Nama Global</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      text-align: center;
      padding: 20px;
      transition: background-image 1s ease-in-out;
    }
    h1 {
      margin-bottom: 20px;
    }
    select, input, button {
      margin: 8px;
      padding: 10px;
      border-radius: 6px;
      border: 1px solid #ccc;
    }
    button {
      cursor: pointer;
      background: #ff9800;
      color: white;
      border: none;
      font-weight: bold;
    }
    button:hover {
      background: #e68900;
    }
    #output {
      margin-top: 20px;
      padding: 15px;
      background: white;
      border-radius: 8px;
      min-height: 100px;
      text-align: left;
      white-space: pre-wrap;
    }
    .name-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 4px;
      padding: 2px 4px;
      border-bottom: 1px solid #eee;
      transition: background 0.3s;
    }
    .name-text {
      flex: 1;
      text-align: left;
    }
    .copy-single {
      margin-left: 10px;
      padding: 2px 6px;
      font-size: 12px;
      background: #4caf50;
      border: none;
      color: white;
      border-radius: 4px;
      cursor: pointer;
    }
    .copy-single:hover {
      background: #43a047;
    }
    .copied {
      color: gray;
      font-weight: bold;
    }
    #copyMsg {
      margin-top: 10px;
      color: green;
      font-weight: bold;
      display: none;
    }
  </style>
</head>
<body>
  <h1>🌍 Generator Nama Global</h1>

  <label for="country">Pilih Negara:</label>
  <select id="country">
    <option>Semua Negara</option>
  </select>

  <label for="gender">Jenis Kelamin:</label>
  <select id="gender">
    <option>Laki-laki</option>
    <option>Perempuan</option>
  </select>

  <label for="jumlah">Jumlah Nama:</label>
  <input type="number" id="jumlah" value="5" min="1" max="1000000">

  <br>
  <button onclick="generateNames()">Generate</button>
  <button onclick="copyNames()">Salin Semua</button>

  <div id="output"></div>
  <div id="copyMsg">✅ Nama berhasil disalin!</div>

  <script>
    const backgrounds = [
      'url("https://images.unsplash.com/photo-1506748686210-1b8b3a1e6e0a")',
      'url("https://images.unsplash.com/photo-1518709268802-8b1b1b1b1b1b")'
    ];
    let currentBackground = 0;
    setInterval(() => {
      currentBackground = (currentBackground + 1) % backgrounds.length;
      document.body.style.backgroundImage = backgrounds[currentBackground];
    }, 2000);

    const allCountries = ["Indonesia", "Jepang", "Amerika Serikat", "Jerman"];

    const sampleNames = {
      "Indonesia": {
        "Laki-laki": { "depan": ["Budi","Andi","Agus","Rizki"], "belakang": ["Santoso","Pratama","Setiawan","Wijaya"] },
        "Perempuan": { "depan": ["Siti","Dewi","Ayu","Rina"], "belakang": ["Lestari","Putri","Wulandari","Halimah"] }
      },
      "Jepang": {
        "Laki-laki": { "depan": ["Haruto","Kaito","Ren"], "belakang": ["Sato","Takahashi","Yamamoto"] },
        "Perempuan": { "depan": ["Sakura","Yui","Hina"], "belakang": ["Tanaka","Kobayashi","Nakamura"] }
      },
      "Amerika Serikat": {
        "Laki-laki": { "depan": ["James","Michael","David"], "belakang": ["Smith","Johnson","Brown"] },
        "Perempuan": { "depan": ["Jennifer","Ashley","Sarah"], "belakang": ["Williams","Jones","Miller"] }
      },
      "Jerman": {
        "Laki-laki": { "depan": ["Hans","Klaus","Wolfgang"], "belakang": ["Müller","Schmidt","Schneider"] },
        "Perempuan": { "depan": ["Greta","Heidi","Lena"], "belakang": ["Fischer","Weber","Meyer"] }
      }
    };

    const select = document.getElementById("country");
    allCountries.forEach(c => {
      const opt = document.createElement("option");
      opt.textContent = c;
      select.appendChild(opt);
    });

    function generateNames() {
      const country = document.getElementById("country").value;
      const gender = document.getElementById("gender").value;
      let jumlah = parseInt(document.getElementById("jumlah").value);
      const output = document.getElementById("output");
      output.innerHTML = '';

      let poolDepan = [];
      let poolBelakang = [];

      if (country === "Semua Negara") {
        for (let c in sampleNames) {
          poolDepan = poolDepan.concat(sampleNames[c][gender].depan || []);
          poolBelakang = poolBelakang.concat(sampleNames[c][gender].belakang || []);
        }
      } else {
        if (sampleNames[country]) {
          poolDepan = [...sampleNames[country][gender].depan];
          poolBelakang = [...sampleNames[country][gender].belakang];
        } else {
          for (let c in sampleNames) {
            poolDepan = poolDepan.concat(sampleNames[c][gender].depan || []);
            poolBelakang = poolBelakang.concat(sampleNames[c][gender].belakang || []);
          }
        }
      }

      if (poolDepan.length === 0) poolDepan = ["Anonim"];
      if (poolBelakang.length === 0) poolBelakang = ["Anonim"];

      let result = new Set();
      while (result.size < jumlah) {
        const depan = poolDepan[Math.floor(Math.random() * poolDepan.length)];
        const belakang = poolBelakang[Math.floor(Math.random() * poolBelakang.length)];
        const number = Math.floor(100000 + Math.random() * 900000);
        result.add(depan + "." + belakang + "." + number);
      }

      Array.from(result).forEach(name => {
        const row = document.createElement('div');
        row.className = 'name-row';
        const span = document.createElement('span');
        span.className = 'name-text';
        span.textContent = name;
        const btn = document.createElement('button');
        btn.className = 'copy-single';
        btn.textContent = 'Salin';
        btn.onclick = () => {
          navigator.clipboard.writeText(name);
          span.classList.add('copied');
          span.textContent = name + " ✅"; // menandai nama yang sudah disalin
          const msg = document.getElementById("copyMsg");
          msg.style.display = "block";
          setTimeout(() => { msg.style.display = "none"; }, 1500);
        };
        row.appendChild(span);
        row.appendChild(btn);
        output.appendChild(row);
      });

      document.getElementById("copyMsg").style.display = false;
    }

    function copyNames() {
      const output = Array.from(document.querySelectorAll('.name-text')).map(e => e.textContent.replace(" ✅","")).join("\n");
      if (!output.trim()) {
        alert("Belum ada nama untuk disalin!");
        return;
      }
      navigator.clipboard.writeText(output).then(() => {
        const msg = document.getElementById("copyMsg");
        msg.style.display = "block";
        setTimeout(() => { msg.style.display = "none"; }, 2000);
      });
    }
  </script>
</body>
</html>
