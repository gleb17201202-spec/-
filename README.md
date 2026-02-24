<!DOCTYPE html>
<html lang="uk">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Надійні Двері | Продаж дверей</title>

<style>
body{margin:0;font-family:Arial, sans-serif;background:#f4f4f4;}
header{background:#111;color:white;padding:20px;text-align:center;}
.container{max-width:1200px;margin:auto;padding:20px;}
.products{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
gap:20px;margin-top:20px;
}
.card{
background:white;border-radius:10px;overflow:hidden;
box-shadow:0 5px 15px rgba(0,0,0,0.1);
}
.card img{width:100%;height:220px;object-fit:cover;}
.card-content{padding:15px;}
.price{font-weight:bold;margin:10px 0;}
button{
padding:8px 10px;border:none;border-radius:5px;
cursor:pointer;margin-top:5px;
}
.add{background:#28a745;color:white;width:100%;}
.password-box{margin-top:30px;}
input,textarea{
width:100%;padding:8px;margin:6px 0;
border-radius:5px;border:1px solid #ccc;
}
.admin{
background:white;padding:20px;border-radius:10px;
margin-top:20px;box-shadow:0 5px 15px rgba(0,0,0,0.1);
}
.hidden{display:none;}
footer{
background:#111;color:white;text-align:center;
padding:20px;margin-top:40px;
}
</style>
</head>

<body>

<header>
<h1>Надійні Двері</h1>
<p>Продаж вхідних та міжкімнатних дверей</p>
</header>

<div class="container">

<h2>Каталог</h2>
<div class="products" id="productList"></div>

<!-- 🔐 Вхід в адмінку -->
<div class="password-box">
<input type="password" id="adminPassword" placeholder="Пароль адміністратора">
<button onclick="checkPassword()">Увійти</button>
</div>

<!-- 🔧 Адмін-панель -->
<div class="admin hidden" id="adminPanel">
<h3>Додати товар</h3>

<input type="text" id="name" placeholder="Назва">
<input type="number" id="price" placeholder="Ціна (грн)">
<input type="text" id="image" placeholder="Посилання на фото">
<textarea id="description" placeholder="Опис"></textarea>

<button class="add" onclick="addProduct()">Додати товар</button>
</div>

</div>

<footer>
<p>📞 Телефон: +380000000000</p>
<p>© 2026 Надійні Двері</p>
</footer>

<!-- Firebase SDK -->
<script type="module">

// 🔐 ЗМІНИ ПАРОЛЬ
const ADMIN_PASSWORD = "12345";

/* 🔥 ВСТАВ СЮДИ СВІЙ firebaseConfig */
const firebaseConfig = {
apiKey: "PASTE_HERE",
authDomain: "PASTE_HERE",
projectId: "PASTE_HERE",
storageBucket: "PASTE_HERE",
messagingSenderId: "PASTE_HERE",
appId: "PASTE_HERE"
};

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import {
getFirestore,
collection,
addDoc,
getDocs
} from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

// 📦 Завантаження товарів
async function renderProducts(){
const querySnapshot = await getDocs(collection(db, "products"));
const list = document.getElementById("productList");
list.innerHTML = "";

querySnapshot.forEach((doc)=>{
  const product = doc.data();
  list.innerHTML += `
    <div class="card">
      <img src="${product.image}" alt="${product.name}">
      <div class="card-content">
        <h3>${product.name}</h3>
        <p>${product.description || ""}</p>
        <div class="price">${product.price} грн</div>
      </div>
    </div>
  `;
});
}

// ➕ Додати товар
window.addProduct = async function(){
const name = document.getElementById("name").value;
const price = document.getElementById("price").value;
const image = document.getElementById("image").value;
const description = document.getElementById("description").value;

if(!name || !price || !image){
  alert("Заповни всі обов'язкові поля!");
  return;
}

await addDoc(collection(db,"products"),{
  name,
  price,
  image,
  description
});

document.getElementById("name").value="";
document.getElementById("price").value="";
document.getElementById("image").value="";
document.getElementById("description").value="";

renderProducts();
}

// 🔐 Перевірка пароля
window.checkPassword = function(){
const input = document.getElementById("adminPassword").value;
if(input === ADMIN_PASSWORD){
  document.getElementById("adminPanel").classList.remove("hidden");
  alert("Доступ дозволено ✅");
} else {
  alert("Невірний пароль ❌");
}
}

// Запуск
renderProducts();

</script>

</body>
</html>
