# Cloud-Paradise
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cloud Paradise</title>
  <style>
    /* Allgemeine Stile */
    body {
      margin: 0;
      font-family: 'Montserrat', sans-serif;
      background: #0f0f0f;
      color: white;
      overflow-x: hidden;
    }
    header {
      height: 100vh;
      background: url('vape-background.jpg') no-repeat center center/cover;
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;
      text-align: center;
      color: white;
    }
    header::after {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0, 0, 0, 0.6);
    }
    .header-content {
      position: relative;
      z-index: 2;
      animation: fadeInDown 1.5s ease;
    }
    @keyframes fadeInDown {
      0% {
        opacity: 0;
        transform: translateY(-20px);
      }
      100% {
        opacity: 1;
        transform: translateY(0);
      }
    }
    .header-content h1 {
      font-size: 3.5rem;
      margin: 0;
    }
    .header-content p {
      font-size: 1.5rem;
      margin: 20px 0;
    }
    .buttons a {
      text-decoration: none;
      background: #1e88e5;
      padding: 15px 30px;
      border-radius: 5px;
      color: white;
      font-weight: bold;
      margin: 10px;
      transition: transform 0.3s ease, background 0.3s ease;
    }
    .buttons a:hover {
      background: #1565c0;
      transform: scale(1.1);
    }
    /* Warenkorb */
    .cart {
      position: fixed;
      top: 20px;
      right: 20px;
      background: #282828;
      padding: 10px 20px;
      border-radius: 5px;
      display: flex;
      align-items: center;
      cursor: pointer;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.5);
      transition: transform 0.3s ease;
    }
    .cart:hover {
      transform: scale(1.1);
    }
    .cart span {
      margin-left: 10px;
      font-size: 1.2rem;
      color: #ffffff;
    }
    /* Flavors Section */
    section {
      padding: 40px;
      background: #1a1a1a;
    }
    h2 {
      text-align: center;
      margin-bottom: 20px;
      font-size: 2.5rem;
      animation: fadeIn 1s ease;
    }
    @keyframes fadeIn {
      0% {
        opacity: 0;
      }
      100% {
        opacity: 1;
      }
    }
    .categories {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 20px;
    }
    .category {
      background: #282828;
      padding: 20px;
      border-radius: 8px;
      cursor: pointer;
      text-align: center;
      width: 200px;
      transition: transform 0.3s ease, background 0.3s ease;
      animation: slideIn 1s ease;
    }
    .category:hover {
      transform: scale(1.1);
      background: #383838;
    }
    @keyframes slideIn {
      0% {
        transform: translateY(20px);
        opacity: 0;
      }
      100% {
        transform: translateY(0);
        opacity: 1;
      }
    }
    .vape-list {
      margin-top: 30px;
    }
    .vape-item {
      padding: 10px;
      background: #282828;
      margin: 5px 0;
      border-radius: 5px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .vape-item button {
      background: #1e88e5;
      color: white;
      border: none;
      padding: 5px 10px;
      border-radius: 5px;
      cursor: pointer;
      transition: background 0.3s ease;
    }
    .vape-item button:hover {
      background: #1565c0;
    }
    /* Warenkorb-Seite */
    #cart-page {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.9);
      padding: 20px;
      color: white;
      overflow-y: auto;
      z-index: 10;
    }
    #cart-page h2 {
      text-align: center;
    }
    #cart-page .cart-items {
      margin: 20px 0;
    }
    #cart-page .cart-item {
      padding: 10px;
      background: #282828;
      margin: 10px 0;
      border-radius: 5px;
      display: flex;
      justify-content: space-between;
    }
    #close-cart {
      background: #e53935;
      padding: 10px 20px;
      border: none;
      color: white;
      font-weight: bold;
      border-radius: 5px;
      cursor: pointer;
      margin: 10px auto;
      display: block;
    }
    #close-cart:hover {
      background: #c62828;
    }
  </style>
</head>
<body>
  <!-- Header -->
  <header>
    <div class="cart" onclick="openCartPage()">
      🛒 <span id="cart-count">0</span>
    </div>
    <div class="header-content">
      <h1>Welcome to Vape Paradise</h1>
      <p>Discover your next favorite vape flavor!</p>
      <div class="buttons">
        <a href="#flavors">Discover Flavors</a>
      </div>
    </div>
  </header>

  <!-- Flavors Section -->
  <section id="flavors">
    <h2>Discover Flavors</h2>
    <div class="categories">
      <div class="category" onclick="showVapes('fruity')">Fruchtige Sorten</div>
      <div class="category" onclick="showVapes('refreshing')">Erfrischende Kombinationen</div>
    </div>
    <div class="vape-list" id="vape-list"></div>
  </section>

  <!-- Cart Page -->
  <div id="cart-page">
    <h2>Ihr Warenkorb</h2>
    <div class="cart-items" id="cart-items"></div>
    <button id="close-cart" onclick="closeCartPage()">Schließen</button>
  </div>

  <script>
    const vapes = {
      fruity: [{ name: "Wassermelone Ice", price: 12.99 }],
      refreshing: [{ name: "Cool Mint", price: 12.99 }]
    };

    let cart = [];

    function showVapes(category) {
      const vapeList = document.getElementById("vape-list");
      vapeList.innerHTML = "";
      if (vapes[category]) {
        vapes[category].forEach(vape => {
          const vapeItem = document.createElement("div");
          vapeItem.classList.add("vape-item");
          vapeItem.innerHTML = `<span>${vape.name} - €${vape.price.toFixed(2)}</span><button onclick="addToCart('${vape.name}', ${vape.price})">In den Warenkorb</button>`;
          vapeList.appendChild(vapeItem);
        });
      }
    }

    function addToCart(name, price) {
      cart.push({ name, price });
      document.getElementById("cart-count").textContent = cart.length;
    }

    function openCartPage() {
      const cartPage = document.getElementById("cart-page");
      const cartItems = document.getElementById("cart-items");
      cartItems.innerHTML = "";
      cart.forEach(item => {
        const div = document.createElement("div");
        div.classList.add("cart-item");
        div.textContent = `${item.name} - €${item.price.toFixed(2)}`;
        cartItems.appendChild(div);
      });
      cartPage.style.display = "block";
    }

    function closeCartPage() {
      document.getElementById("cart-page").style.display = "none";
    }
  </script>
</body>
</html>
