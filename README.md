<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Link Shortener</title>
  <style>
    body {
      margin: 0;
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(135deg, #ff6a00, #ee0979, #2193b0, #6dd5ed);
      background-size: 400% 400%;
      animation: gradientBG 15s ease infinite;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }

    @keyframes gradientBG {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    .container {
      background: rgba(255,255,255,0.9);
      padding: 30px;
      border-radius: 16px;
      box-shadow: 0px 8px 20px rgba(0,0,0,0.2);
      width: 480px;
      text-align: center;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }
    .container:hover {
      transform: translateY(-5px);
      box-shadow: 0px 12px 24px rgba(0,0,0,0.3);
    }

    .owner {
      font-size: 28px;
      font-weight: 800;
      margin-bottom: 10px;
      color: #0f172a;
      text-transform: uppercase;
      letter-spacing: 3px;
      text-shadow: 2px 2px 6px rgba(0,0,0,0.25);
      transition: color 0.4s ease;
    }
    .owner:hover {
      color: #1d4ed8;
    }

    .brand {
      font-size: 20px;
      font-weight: 700;
      margin-bottom: 20px;
      color: #e11d48;
      letter-spacing: 2px;
      text-shadow: 1px 1px 3px rgba(0,0,0,0.2);
      transition: color 0.3s ease;
    }
    .brand:hover {
      color: #9d174d;
    }

    h1 {
      margin-bottom: 5px;
      color: #1d4ed8;
    }

    textarea {
      width: 100%;
      height: 80px;
      padding: 10px;
      margin-bottom: 10px;
      border-radius: 8px;
      border: 1px solid #ccc;
      resize: none;
      font-size: 14px;
    }

    button {
      padding: 10px 20px;
      border: none;
      margin: 5px;
      background: #2563eb;
      color: white;
      border-radius: 8px;
      cursor: pointer;
      font-size: 14px;
      transition: background 0.3s ease, transform 0.2s ease;
    }
    button:hover {
      background: #1d4ed8;
      transform: scale(1.05);
    }

    #results {
      margin-top: 20px;
      text-align: left;
      max-height: 200px;
      overflow-y: auto;
    }

    .link-box {
      background: #f3f4f6;
      padding: 8px;
      border-radius: 8px;
      margin-bottom: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 14px;
    }

    .link-box a {
      color: #2563eb;
      text-decoration: none;
      word-break: break-all;
    }

    .copy-btn {
      background: #10b981;
    }
    .copy-btn:hover {
      background: #059669;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="owner">(by: SUTRA_LAUT04)</div>
    <div class="brand">✨ IMOTIZEIT ✨</div>
    <h1>Pendekkan Link Anda</h1>
    <textarea id="urlInput" placeholder="Masukkan link panjang (bisa banyak, pisahkan dengan enter)"></textarea>
    <br>
    <button onclick="shorten()">Pendekkan</button>
    <button onclick="refreshPage()">Refresh</button>
    <button onclick="copyAll()">Copy Semua</button>
    <div id="results"></div>
  </div>

  <script>
    async function shorten() {
      const input = document.getElementById("urlInput").value.trim();
      if (!input) {
        alert("Masukkan minimal 1 link!");
        return;
      }
      const urls = input.split("\n").map(u => u.trim()).filter(u => u);

      const resultsDiv = document.getElementById("results");
      resultsDiv.innerHTML = "";

      for (let url of urls) {
        try {
          const res = await fetch(`https://tinyurl.com/api-create.php?url=${encodeURIComponent(url)}`);
          const shortUrl = await res.text();

          const div = document.createElement("div");
          div.className = "link-box";
          div.innerHTML = `
            <a href="${shortUrl}" target="_blank">${shortUrl}</a>
            <button class="copy-btn" onclick="copyToClipboard('${shortUrl}')">Copy</button>
          `;
          resultsDiv.appendChild(div);
        } catch (e) {
          alert("Gagal memendekkan: " + url);
        }
      }
    }

    function refreshPage() {
      document.getElementById("urlInput").value = "";
      document.getElementById("results").innerHTML = "";
    }

    function copyToClipboard(text) {
      navigator.clipboard.writeText(text).then(() => {
        alert("Link disalin: " + text);
      });
    }

    function copyAll() {
      const links = Array.from(document.querySelectorAll("#results a")).map(a => a.href).join("\n");
      if (!links) {
        alert("Belum ada link untuk disalin!");
        return;
      }
      navigator.clipboard.writeText(links).then(() => {
        alert("Semua link berhasil disalin!");
      });
    }
  </script>
</body>
</html>
