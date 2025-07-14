<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>E-commerce Product Page</title>
</head>
<body>
  <div id="root"></div>
</body>
</html>


import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './App.css';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);


import React, { useState } from 'react';
import ProductCard from './components/ProductCard';
import Cart from './components/Cart';
import productsData from './products';

function App() {
  const [cart, setCart] = useState([]);

  const addToCart = (product) => {
    setCart(prev => {
      const item = prev.find(p => p.id === product.id);
      if (item) {
        return prev.map(p => p.id === product.id ? { ...p, quantity: p.quantity + 1 } : p);
      } else {
        return [...prev, { ...product, quantity: 1 }];
      }
    });
  };

  const removeFromCart = (id) => {
    setCart(cart.filter(p => p.id !== id));
  };

  return (
    <div className="container">
      <h1>🛒 E-commerce Store</h1>
      <Cart cart={cart} removeFromCart={removeFromCart} />
      <div className="product-grid">
        {productsData.map(product => (
          <ProductCard key={product.id} product={product} addToCart={addToCart} />
        ))}
      </div>
    </div>
  );
}

export default App;



const products = [
  { id: 1, name: "T-Shirt", price: 25, image: "https://via.placeholder.com/150" },
  { id: 2, name: "Sneakers", price: 80, image: "https://via.placeholder.com/150" },
  { id: 3, name: "Backpack", price: 50, image: "https://via.placeholder.com/150" },
  { id: 4, name: "Watch", price: 120, image: "https://via.placeholder.com/150" },
];

export default products;



import React from 'react';

function ProductCard({ product, addToCart }) {
  return (
    <div className="product-card">
      <img src={product.image} alt={product.name}/>
      <h3>{product.name}</h3>
      <p>${product.price}</p>
      <button onClick={() => addToCart(product)}>Add to Cart</button>
    </div>
  );
}

export default ProductCard;


import React from 'react';

function Cart({ cart, removeFromCart }) {
  const total = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);

  return (
    <div className="cart">
      <h2>🧺 Cart</h2>
      {cart.length === 0 ? <p>Cart is empty</p> : (
        <ul>
          {cart.map(item => (
            <li key={item.id}>
              {item.name} x{item.quantity} — ${item.price * item.quantity}
              <button onClick={() => removeFromCart(item.id)}>❌</button>
            </li>
          ))}
        </ul>
      )}
      <h3>Total: ${total}</h3>
    </div>
  );
}

export default Cart;



body {
  font-family: Arial, sans-serif;
  background: #f8f8f8;
  margin: 0;
  padding: 0;
}

.container {
  padding: 20px;
}

h1 {
  text-align: center;
  color: #333;
}

.product-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}

.product-card {
  background: white;
  padding: 15px;
  border: 1px solid #ddd;
  width: 200px;
  text-align: center;
  border-radius: 8px;
}

.product-card img {
  width: 100%;
}

.product-card button {
  margin-top: 10px;
  background: #007bff;
  color: white;
  border: none;
  padding: 8px;
  cursor: pointer;
  border-radius: 5px;
}

.cart {
  margin: 20px 0;
  padding: 10px;
  background: #fff;
  border: 1px solid #ccc;
}

.cart ul {
  list-style: none;
  padding: 0;
}

.cart li {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

.cart button {
  background: red;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 4px;
}



