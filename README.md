# E-commerce Web Application Project - Overview and Key Functionalities

## Project Overview

* This web application is designed to provide a seamless shopping experience, featuring a product listing and detailed views for individual items. Utilizing the power of React, React Router, and Axios, this project is a showcase of modern web development techniques.

## Key Functionalities

1. Product Listing:

~ Home page displaying a list of products fetched from a fake store API.
~ Each product is linked to its detailed view page.

2. Product Details:

~ Individual product details are fetched based on the product ID.
~ Detailed view includes product title and description.

3. Context API for State Management:

~ Efficient state management using React's Context API to handle product data.

4. Routing:

~ Smooth navigation between the home page and product details using React Router.

5. API Integration:

~ Axios is used for API calls to fetch product data from a backend server.

## Technical Stack

- React for building the user interface.
- React Router for handling navigation.
- Axios for API calls.
- Context API for state management.


## CODE
***

| main.jsx | App.jsx | component/Home.jsx | component/Details.jsx | utils/axios.js | utils/Context.jsx

### main.jsx
```

import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";
import { BrowserRouter } from "react-router-dom";
import Context from "./utils/Context.jsx";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <Context>
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </Context>
  </React.StrictMode>,
);

```

### App.jsx
```
import { Route, Routes } from "react-router-dom";
import { Home } from "./component/Home";
import Details from "./component/Details";

function App() {
  return (
    <>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/details/:id" element={<Details />} />
      </Routes>
    </>
  );
}

export default App;


```

### component/Home.jsx
```

import React, { useContext } from "react";
import { Link } from "react-router-dom";
import { ProductContext } from "../utils/Context";

export const Home = () => {
  const [products] = useContext(ProductContext);

  return products ? (
    <>
      <div>
        {products.map((e, idx) => (
          <Link to={`/details/${e.id}`} key={idx}>
            <h1>{e.title}</h1>
          </Link>
        ))}
      </div>
    </>
  ) : (
    <p>Loading...</p>
  );
};

```

### component/Details.jsx

```

import React, { useEffect, useState } from "react";
import axios from "../utils/axios";
import { useParams } from "react-router-dom";

function Details() {
  const { id } = useParams();
  const [product, setProduct] = useState(null);

  const getSingleProduct = async () => {
    try {
      const { data } = await axios.get(`/products/${id}`);
      setProduct(data);
    } catch (error) {
      console.log(error);
    }
  };
  useEffect(() => {
    getSingleProduct();
  }, []);

  return product ? (
    <div>
      <h1 className="text-4xl">{product.title}</h1>
      <h1 className="text-2xl">{product.description}</h1>
    </div>
  ) : (
    <p>Loading...</p>
  );
}

export default Details;

```

### utils/axios.js

```

import axios from "axios";

const instance = axios.create({
  baseURL: "https://fakestoreapi.com/",
});

export default instance;

```

### utils/Context.jsx

```

import axios from "./axios";
import React, { createContext, useEffect, useState } from "react";

export const ProductContext = createContext();

const Context = (props) => {
  const [product, setProduct] = useState(null);

  const getProducts = async () => {
    try {
      const { data } = await axios("/products");
      setProduct(data);
    } catch (error) {
      console.log(error);
    }
  };

  useEffect(() => {
    getProducts();
  }, []);

  return (
    <ProductContext.Provider value={[product, setProduct]}>
      {props.children}
    </ProductContext.Provider>
  );
};

export default Context;

```






This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) uses [Babel](https://babeljs.io/) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh
