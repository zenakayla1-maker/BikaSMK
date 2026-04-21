# BikaSMK
website aplikasi BIKA (Bisa SMK) untuk kerja,magang,dan konsultasi siswa
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BIKA - Bisa SMK</title>
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- Firebase -->
  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js"></script>
</head>
<body class="bg-gradient-to-br from-green-100 to-white font-sans">

  <!-- Navbar -->
  <nav class="bg-white shadow p-4 flex justify-between items-center">
    <h1 class="text-xl font-bold text-green-600">BIKA</h1>
    <div class="space-x-4 text-gray-700">
      <a href="#masa-depan">Masa Depan</a>
      <a href="#tutorial">Tutorial</a>
      <a href="#konsultasi">Konsultasi</a>
    </div>
  </nav>

  <!-- Hero -->
  <section class="text-center py-16">
    <h2 class="text-4xl font-bold text-gray-800 mb-4">Bangun Masa Depanmu 🚀</h2>
    <p class="text-gray-600">Kerja • Magang • Usaha dalam satu platform</p>
  </section>

  <!-- Masa Depan (Lowongan Real) -->
  <section id="masa-depan" class="p-8">
    <h2 class="text-2xl font-bold mb-6">Lowongan Terbaru</h2>
    <div id="jobList" class="grid md:grid-cols-3 gap-6"></div>
  </section>

  <!-- Tutorial -->
  <section id="tutorial" class="p-8 bg-white">
    <h2 class="text-2xl font-bold mb-6">Tutorial</h2>
    <div class="grid md:grid-cols-2 gap-6">
      <div class="bg-gray-100 p-4 rounded-xl shadow">
        <iframe class="w-full h-48 rounded-lg" src="https://www.youtube.com/embed/dQw4w9WgXcQ" allowfullscreen></iframe>
        <p class="mt-2">Cara bikin CV</p>
      </div>
      <div class="bg-gray-100 p-4 rounded-xl shadow">
        <iframe class="w-full h-48 rounded-lg" src="https://www.youtube.com/embed/dQw4w9WgXcQ" allowfullscreen></iframe>
        <p class="mt-2">Tips interview</p>
      </div>
    </div>
  </section>

  <!-- Konsultasi Real-time -->
  <section id="konsultasi" class="p-8">
    <h2 class="text-2xl font-bold mb-6">Konsultasi</h2>
    <div class="bg-white p-6 rounded-2xl shadow max-w-xl mx-auto">
      <div id="chatBox" class="h-60 overflow-y-auto border p-3 mb-4 rounded"></div>
      <div class="flex">
        <input id="chatInput" class="flex-1 border p-2 rounded-l" placeholder="Tanya ke guru...">
        <button onclick="sendMessage()" class="bg-green-600 text-white px-4 rounded-r">Kirim</button>
      </div>
    </div>
  </section>

  <script>
    // CONFIG FIREBASE (ISI PUNYA KAMU)
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT.firebaseapp.com",
      projectId: "YOUR_PROJECT_ID",
    };

    const app = firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();

    // LOAD LOWONGAN (REAL DATABASE)
    const jobList = document.getElementById('jobList');

    db.collection("jobs").onSnapshot(snapshot => {
      jobList.innerHTML = '';
      snapshot.forEach(doc => {
        const job = doc.data();
        jobList.innerHTML += `
          <div class="bg-white p-4 rounded-xl shadow hover:scale-105 transition">
            <h3 class="font-bold text-lg">${job.title}</h3>
            <p class="text-sm text-gray-500">${job.company}</p>
            <p class="mt-2">${job.description}</p>
          </div>
        `;
      });
    });

    // CHAT REAL-TIME
    const chatBox = document.getElementById('chatBox');

    db.collection("chat").orderBy("time").onSnapshot(snapshot => {
      chatBox.innerHTML = '';
      snapshot.forEach(doc => {
        const msg = doc.data();
        chatBox.innerHTML += `<p><b>${msg.user}:</b> ${msg.text}</p>`;
      });
      chatBox.scrollTop = chatBox.scrollHeight;
    });

    function sendMessage() {
      const input = document.getElementById('chatInput');
      if(input.value.trim() === '') return;

      db.collection("chat").add({
        user: "Siswa",
        text: input.value,
        time: Date.now()
      });

      input.value = '';
    }
  </script>

</body>
</html>
