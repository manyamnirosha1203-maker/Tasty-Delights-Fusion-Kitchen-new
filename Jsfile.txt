

let cart = [];
let total = 0;

function addToCart(item, price) {
  cart.push({ item, price });
  total += price;
  renderCart();
}

function renderCart() {
  const cartList = document.getElementById("cart-items");
  cartList.innerHTML = "";
  cart.forEach((c, index) => {
    cartList.innerHTML += `<li>${c.item} - ₹${c.price} 
      <button onclick="removeFromCart(${index})">Remove</button></li>`;
  });
  document.getElementById("total").innerText = `Total: ₹${total}`;
}

function removeFromCart(index) {
  total -= cart[index].price;
  cart.splice(index, 1);
  renderCart();
}

function checkout() {
  alert("Thank you for your order! Your food will be delivered soon.");
  cart = [];
  total = 0;
  renderCart();
}

document.querySelectorAll('.add-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    const name = btn.dataset.name;
    const price = parseInt(btn.dataset.price);
    const parent = btn.parentElement;

    if (!cart[name]) {
      cart[name] = { price, qty: 1 };
      total += price;
      cartTotal.textContent = total;

      // Replace Add button with quantity controls (namespaced)
      parent.innerHTML = `
        <div class="order-inline-controls">
          <button class="order-btn-minus">−</button>
          <span class="order-qty-count">1</span>
          <button class="order-btn-plus">+</button>
        </div>
      `;

      // Add item to cart
      const li = document.createElement('li');
      li.setAttribute('data-name', name);
      li.innerHTML = `${name} — ₹${price} × <span class="order-qty-count">1</span>`;
      cartItems.appendChild(li);

      const updateQty = () => {
        const qty = cart[name].qty;
        parent.querySelector('.order-qty-count').textContent = qty;
        li.querySelector('.order-qty-count').textContent = qty;
        cartTotal.textContent = total;
      };

      parent.querySelector('.order-btn-plus').addEventListener('click', () => {
        cart[name].qty++;
        total += price;
        updateQty();
      });

      parent.querySelector('.order-btn-minus').addEventListener('click', () => {
        cart[name].qty--;
        total -= price;
        if (cart[name].qty === 0) {
          delete cart[name];
          parent.innerHTML = `<button class="add-btn" data-name="${name}" data-price="${price}">Add</button>`;
          li.remove();
          cartTotal.textContent = total;
          parent.querySelector('.add-btn').addEventListener('click', btn.click);
        } else {
          updateQty();
        }
      });
    }
  });
});
