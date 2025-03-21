<!DOCTYPE html>
<html lang="tr">

<head>
    <meta charset="UTF-8">
    <title>Arkadaşını Davet Et</title>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.9.0/firebase-app.js";
        import { getFirestore, collection, addDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.9.0/firebase-firestore.js";

        const firebaseConfig = {
            apiKey : "AIzaSyDI6f_7_9ew-p5kE5oDAe1FNTD3K9kYg-k" , 
  authDomain : "tur-61.firebaseapp.com" , 
  projeKimliği : "tur-61" , 
  depolamaKovası : "tur-61.firebasestorage.app" , 
  mesajlaşmaGönderenKimliği : "294563420707" , 
  uygulama kimliği : "1:294563420707:web:97f50bb6f7dee679a9ef96"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        window.submitForm = async (event) => {
            event.preventDefault();
            if (!document.getElementById('kvkk').checked) {
                Swal.fire('KVKK kutucuğunu işaretleyiniz.');
                return;
            }

            const formData = {
                adSoyad: document.getElementById('name').value,
                acenta: document.getElementById('agency').value,
                telefon: document.getElementById('phone').value,
                sehir: document.getElementById('city').value,
                ulke: document.getElementById('country').value,
                aciklama: document.getElementById('description').value,
                tarih: new Date()
            };

            await addDoc(collection(db, "davet_edilenler"), formData);
            Swal.fire('Başarıyla Gönderildi!', 'Form başarıyla kaydedildi.', 'success');
            document.getElementById('inviteForm').reset();
        };

        window.sendWhatsapp = function () {
            const mesaj = encodeURIComponent("Merhaba! Seni satıcı olmaya davet ediyorum. Başvurmak için 👉 https://davetformu.github.io/davetsayfasi/#form");
            window.open(`https://wa.me/?text=${mesaj}`, '_blank');
        };
    </script>
    <style>
        body { font-family: Arial; background: #f7f9fc; }
        form { max-width: 500px; margin: auto; background: #fff; padding: 15px; border-radius: 8px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        input, textarea { width: 100%; margin-top: 6px; margin-bottom: 12px; padding: 8px; border-radius: 4px; border: 1px solid #ddd; }
        button { background: #25D366; color: #fff; padding: 10px; border: none; border-radius: 5px; cursor: pointer; }
        button:hover { background-color: #128C7E; }
    </script>
</head>

<body>
    <div style="text-align:center; margin-top:40px;">
        <h2>✨ Arkadaşını Davet Et ✨</h2>
        <button onclick="sendWhatsapp()">WhatsApp ile Davet Et 📲</button>
    </div>

    <div style="max-width:500px; margin:auto;" id="form">
        <form id="inviteForm" onsubmit="submitForm(event)">
            <label>Ad Soyad:</label>
            <input type="text" id="name" required>

            <label>Acenta İsmi</label>
            <input type="text" id="agency" required>

            <label>İletişim</label>
            <input type="text" id="phone" required>

            <label>Şehir</label>
            <input type="text" id="city" required>

            <label>Ülke</label>
            <input type="text" id="country" required>

            <label>Açıklama</label>
            <textarea id="description"></textarea>

            <input type="checkbox" id="kvkk"> KVKK şartlarını kabul ediyorum<br><br>

            <button onclick="submitForm(event)">Gönder 🚀</button>
    </div>
</body>

</html>
