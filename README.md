<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RIUHONG ®</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
    <style>
        * { margin:0; padding:0; box-sizing:border-box; }
        body {
            font-family: 'Helvetica Neue', Arial, sans-serif;
            background:#000; color:#fff;
            overflow-x:hidden;
        }
        header {
            position:fixed; top:0; width:100%; z-index:1000;
            padding:20px 40px; background:rgba(0,0,0,0.8);
            backdrop-filter:blur(10px); display:flex;
            justify-content:space-between; align-items:center;
        }
        header h1 { font-size:28px; letter-spacing:10px; font-weight:300; }
        nav a { color:#fff; margin-left:35px; text-decoration:none; font-weight:300; }
        nav a:hover { opacity:0.7; }

        /* HERO */
        .hero {
            height:100vh;
            background:url('https://i.imgur.com/3jK9k8E.jpeg') center/cover no-repeat;
            display:flex; align-items:center; justify-content:center;
            position:relative; filter:grayscale(90%);
        }
        .hero::before {
            content:"RIUHONG";
            position:absolute; font-size:120px; font-weight:100;
            letter-spacing:25px; color:rgba(255,255,255,0.9);
            text-shadow:0 0 40px #000; z-index:2;
        }

        /* PRODUCTS */
        .products { padding:100px 40px; text-align:center; }
        .products h2 { font-size:50px; letter-spacing:12px; font-weight:100; margin-bottom:80px; }
        .grid {
            display:grid; grid-template-columns:repeat(auto-fit, minmax(320px,1fr));
            gap:30px; max-width:1600px; margin:0 auto;
        }
        .item { cursor:pointer; overflow:hidden; background:#111; }
        .item img {
            width:100%; height:500px; object-fit:cover;
            filter:grayscale(100%); transition:all 0.6s ease;
        }
        .item:hover img { filter:grayscale(0%); transform:scale(1.08); }
        .item p { padding:30px; font-size:22px; letter-spacing:4px; font-weight:300; }

        /* GIỎ HÀNG NỔI */
        .cart-icon {
            position:fixed; bottom:30px; right:30px; width:75px; height:75px;
            background:#fff; color:#000; border-radius:50%;
            display:flex; flex-direction:column; align-items:center; justify-content:center;
            font-size:13px; font-weight:bold; letter-spacing:1px;
            box-shadow:0 15px 35px rgba(0,0,0,0.6); z-index:999; cursor:pointer;
            transition:0.3s;
        }
        .cart-icon:hover { transform:scale(1.15); }
        .cart-icon i { font-size:28px; margin-bottom:4px; }
        .cart-count {
            position:absolute; top:-10px; right:-10px; background:#e74c3c;
            color:#fff; width:35px; height:35px; border-radius:50%;
            font-size:18px; font-weight:bold; display:flex;
            align-items:center; justify-content:center;
        }

        /* PANEL GIỎ HÀNG */
        .cart-panel {
            position:fixed; top:0; right:-450px; width:450px; height:100vh;
            background:#111; z-index:2000; transition:right 0.5s ease;
            padding:100px 30px 30px; overflow-y:auto;
            box-shadow:-10px 0 40px rgba(0,0,0,0.9);
        }
        .cart-panel.open { right:0; }
        .cart-header {
            position:absolute; top:0; left:0; right:0; padding:25px 30px;
            background:#000; display:flex; justify-content:space-between;
            align-items:center; font-size:24px; letter-spacing:4px;
        }
        .close-cart { cursor:pointer; font-size:35px; }

        .cart-item { display:flex; margin-bottom:25px; padding-bottom:20px; border-bottom:1px solid #333; }
        .cart-item img { width:100px; height:130px; object-fit:cover; margin-right:15px; filter:grayscale(70%); }
        .cart-item-info { flex:1; }
        .cart-item-name { font-size:16px; margin-bottom:8px; }
        .cart-item-price { color:#aaa; }
        .remove-item { background:none; border:none; color:#e74c3c; cursor:pointer; font-size:20px; }

        .cart-total { margin:30px 0; padding:20px 0; border-top:2px solid #333; font-size:24px; text-align:center; }
        .checkout-btn {
            width:100%; padding:18px; background:#fff; color:#000;
            border:none; font-size:18px; letter-spacing:3px; cursor:pointer;
        }

        /* FORM THANH TOÁN */
        .checkout-form { display:none; margin-top:20px; }
        .checkout-form input, .checkout-form textarea {
            width:100%; padding:15px; margin:12px 0;
            background:#222; border:none; color:#fff; font-size:16px;
        }
        .order-btn {
            width:100%; margin-top:20px; padding:18px;
            background:#00ff00; color:#000; border:none;
            font-size:20px; font-weight:bold; letter-spacing:3px; cursor:pointer;
        }

        /* HIỆU ỨNG THÀNH CÔNG */
        .success-overlay {
            position:fixed; top:0; left:0; right:0; bottom:0;
            background:rgba(0,0,0,0.95); display:none;
            justify-content:center; align-items:center; z-index:3000;
            flex-direction:column; text-align:center; font-size:50px;
        }
        .success-text { animation:successAnim 2s ease; }
        @keyframes successAnim {
            0% { transform:scale(0); opacity:0; }
            50% { transform:scale(1.2); }
            100% { transform:scale(1); opacity:1; }
        }
        .firework {
            position:absolute; width:10px; height:10px; background:#fff;
            border-radius:50%; animation:firework 1.5s ease-out forwards;
        }
        @keyframes firework {
            0% { transform:translate(var(--x),var(--y)); opacity:1; }
            100% { transform:translate(var(--x2),var(--y2)); opacity:0; }
        }

        /* POPUP SẢN PHẨM */
        .overlay { position:fixed; top:0; left:0; right:0; bottom:0;
            background:rgba(0,0,0,0.97); display:none;
            justify-content:center; align-items:center; z-index:1500;
        }
        .popup { background:#111; width:90%; max-width:1100px; max-height:90vh;
            display:flex; flex-direction:column; color:#fff;
        }
        .popup-header { padding:25px 40px; background:#000;
            display:flex; justify-content:space-between; align-items:center;
            font-size:28px; letter-spacing:5px;
        }
        .close-btn { font-size:50px; cursor:pointer; }
        .popup-body { display:flex; flex:1; }
        .popup-img { width:50%; object-fit:cover; }
        .popup-info { width:50%; padding:60px 50px; display:flex; flex-direction:column; justify-content:center; }
        .popup-info h3 { font-size:40px; margin-bottom:20px; font-weight:300; }
        .price { font-size:34px; margin:25px 0; }
        .buy-btn {
            background:#fff; color:#000; border:none;
            padding:20px 60px; font-size:18px; letter-spacing:4px;
            cursor:pointer; margin-top:30px; align-self:flex-start;
        }

        footer { padding:120px 40px; text-align:center; background:#000; }

        @media (max-width:768px) {
            .cart-panel { width:100%; right:-100%; }
            .hero::before { font-size:60px; letter-spacing:10px; }
            .popup-body { flex-direction:column; }
            .popup-img,.popup-info { width:100%; }
        }
    </style>
</head>
<body>

    <header>
        <h1>RIUHONG</h1>
        <nav>
            <a href="#hero">HOME</a>
            <a href="#products">COLLECTION</a>
            <a href="#contact">CONTACT</a>
        </nav>
    </header>

    <section class="hero" id="hero"></section>

    <section class="products" id="products">
        <h2>NEW DROP</h2>
        <div class="grid">
            <div class="item" onclick="openPopup(0)">
                <img src="https://i.imgur.com/3jK9k8E.jpeg" alt="Hoodie">
                <p>ESSENTIAL HOODIE</p>
            </div>
            <div class="item" onclick="openPopup(1)">
                <img src="https://i.imgur.com/3jK9k8E.jpeg" alt="Tee">
                <p>OVERSIZED TEE</p>
            </div>
            <div class="item" onclick="openPopup(2)">
                <img src="https://i.imgur.com/3jK9k8E.jpeg" alt="Cargo">
                <p>CARGO PANTS</p>
            </div>
            <div class="item" onclick="openPopup(3)">
                <img src="https://i.imgur.com/3jK9k8E.jpeg" alt="Jacket">
                <p>PUFFER JACKET</p>
            </div>
        </div>
    </section>

    <!-- GIỎ HÀNG NỔI -->
    <div class="cart-icon" onclick="toggleCart()">
        <i class="fas fa-shopping-bag"></i>
        CART
        <div class="cart-count" id="cart-count">0</div>
    </div>

    <!-- PANEL GIỎ HÀNG -->
    <div class="cart-panel" id="cart-panel">
        <div class="cart-header">
            <span>GIỎ HÀNG</span>
            <span class="close-cart" onclick="toggleCart()">×</span>
        </div>
        <div id="cart-items"></div>
        <div class="cart-total">Tổng tiền: <strong id="cart-total">0đ</strong></div>
        <button class="checkout-btn" onclick="showCheckoutForm()">THANH TOÁN</button>

        <!-- FORM THANH TOÁN -->
        <div class="checkout-form" id="checkout-form">
            <input type="text" id="customer-name" placeholder="Họ & tên" required>
            <input type="text" id="customer-phone" placeholder="Số điện thoại" required>
            <textarea id="customer-address" rows="3" placeholder="Địa chỉ giao hàng chi tiết" required></textarea>
            <input type="text" id="voucher-code" placeholder="Mã giảm giá (nếu có)">
            <button class="order-btn" onclick="placeOrder()">ĐẶT HÀNG NGAY</button>
        </div>
    </div>

    <!-- POPUP CHI TIẾT SẢN PHẨM -->
    <div class="overlay" id="overlay" onclick="closePopup()">
        <div class="popup" onclick="event.stopPropagation()">
            <div class="popup-header">
                <span id="popup-title">ESSENTIAL HOODIE</span>
                <span class="close-btn" onclick="closePopup()">×</span>
             </div>
            <div class="popup-body">
                <img id="popup-img" class="popup-img" src="https://i.imgur.com/3jK9k8E.jpeg">
                <div class="popup-info">
                    <h3 id="popup-name">ESSENTIAL HOODIE</h3>
                    <div class="price" id="popup-price">850.000đ</div>
                    <p>Heavyweight 400GSM • Oversized fit<br>Made in Vietnam • Limited drop</p>
                    <button class="buy-btn" id="add-to-cart-btn">ADD TO CART</button>
                </div>
            </div>
        </div>
    </div>

    <!-- HIỆU ỨNG THÀNH CÔNG -->
    <div class="success-overlay" id="success-overlay">
        <div class="success-text">THÀNH CÔNG!</div>
        <div style="margin-top:30px;font-size:24px;">Mã đơn hàng: <strong id="order-id"></strong></div>
        <div style="margin-top:20px;font-size:18px;">Cảm ơn bạn đã ủng hộ RIUHONG</div>
    </div>

    <footer id="contact">
        <p>pipomusic0306@gmail.com • 0964457535 • KHÂM THIÊN, HÀ NỘI</p>
    </footer>

    <script>
        const products = [
            { name: "ESSENTIAL HOODIE", price: 850000, img: "https://i.imgur.com/3jK9k8E.jpeg" },
            { name: "OVERSIZED TEE", price: 450000, img: "https://i.imgur.com/3jK9k8E.jpeg" },
            { name: "CARGO PANTS", price: 950000, img: "https://i.imgur.com/3jK9k8E.jpeg" },
            { name: "PUFFER JACKET", price: 1850000, img: "https://i.imgur.com/3jK9k8E.jpeg" }
        ];

        let cart = [];
        let currentProduct = 0;

        function openPopup(i) {
            currentProduct = i;
            const p = products[i];
            document.getElementById('popup-title').textContent = p.name;
            document.getElementById('popup-name').textContent = p.name;
            document.getElementById('popup-price').textContent = p.price.toLocaleString('vi-VN') + 'đ';
            document.getElementById('popup-img').src = p.img;
            document.getElementById('overlay').style.display = 'flex';
            document.body.style.overflow = 'hidden';

            // Cập nhật nút ADD TO CART
            document.getElementById('add-to-cart-btn').onclick = () => addToCart(i);
        }

        function closePopup() {
            document.getElementById('overlay').style.display = 'none';
            document.body.style.overflow = 'auto';
        }

        function addToCart(i) {
            cart.push(products[i]);
            updateCart();
            alert(products[i].name + " đã được thêm vào giỏ hàng!");
            closePopup();
        }

        function removeFromCart(index) {
            cart.splice(index, 1);
            updateCart();
        }

        function updateCart() {
            const itemsDiv = document.getElementById('cart-items');
            const totalEl = document.getElementById('cart-total');
            const countEl = document.getElementById('cart-count');

            if (cart.length === 0) {
                itemsDiv.innerHTML = "<p style='text-align:center;padding:60px;color:#666'>Giỏ hàng trống</p>";
                totalEl.textContent = "0đ";
                countEl.textContent = "0";
                return;
            }

            itemsDiv.innerHTML = "";
            let total = 0;
            cart.forEach((item, idx) => {
                total += item.price;
                itemsDiv.innerHTML += `
                    <div class="cart-item">
                        <img src="${item.img}" alt="${item.name}">
                        <div class="cart-item-info">
                            <div class="cart-item-name">${item.name}</div>
                            <div class="cart-item-price">${item.price.toLocaleString('vi-VN')}đ</div>
                            <button class="remove-item" onclick="removeFromCart(${idx})">Xóa</button>
                        </div>
                    </div>`;
            });
            totalEl.textContent = total.toLocaleString('vi-VN') + "đ";
            countEl.textContent = cart.length;
        }

        function toggleCart() {
            document.getElementById('cart-panel').classList.toggle('open');
        }

        function showCheckoutForm() {
            document.querySelector('.checkout-btn').style.display = 'none';
            document.getElementById('checkout-form').style.display = 'block';
        }

        function placeOrder() {
            const name = document.getElementById('customer-name').value.trim();
            const phone = document.getElementById('customer-phone').value.trim();
            const address = document.getElementById('customer-address').value.trim();

            if (!name || !phone || !address) {
                alert("Vui lòng điền đầy đủ thông tin!");
                return;
            }

            const orderId = "RH" + Date.now().toString().slice(-8);
            const overlay = document.getElementById('success-overlay');
            document.getElementById('order-id').textContent = orderId;
            overlay.style.display = 'flex';

            // Pháo hoa
            for(let i=0; i<30; i++){
                const fw = document.createElement('div');
                fw.className = 'firework';
                fw.style.setProperty('--x', Math.random()*innerWidth + 'px');
                fw.style.setProperty('--y', Math.random()*innerHeight + 'px');
                fw.style.setProperty('--x2', (Math.random()-0.5)*500 + 'px');
                fw.style.setProperty('--y2', (Math.random()-0.5)*500 + 'px');
                fw.style.background = ['#ff0000','#00ff00','#0000ff','#ffff00','#ff00ff'][Math.floor(Math.random()*5)];
                overlay.appendChild(fw);
                setTimeout(()=>fw.remove(), 1500);
            }

            setTimeout(() => {
                cart = [];
                updateCart();
                toggleCart();
                overlay.style.display = 'none';
                document.querySelector('.checkout-btn').style.display = 'block';
                document.getElementById('checkout-form').style.display = 'none';
                document.getElementById('checkout-form').reset();
            }, 4000);
        }

        // Khởi tạo giỏ hàng
        updateCart();
    </script>
</body>
</html>
