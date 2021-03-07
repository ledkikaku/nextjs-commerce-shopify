**[Next.js Commerce](https://github.com/vercel/commerce) now supports Shopify. This package is not required if you're looking for Shopify integration for your Next.js Commerce application.** 

## Table of Contents

- [Getting Started](#getting-started)
  - [Installation](#installation)
  - [CommerceProvider](#commerceprovider)
  - [useCommerce](#usecommerce)
- [Hooks](#hooks)
  - [usePrice](#useprice)
  - [useAddItem](#useadditem)
  - [useRemoveItem](#useremoveitem)
  - [useUpdateItem](#useupdateitem)
- [APIs](#apis)
  - [getProduct](#getproduct)
  - [getAllProducts](#getallproducts)
  - [getAllCollections](#getallcollections)
  - [getAllPages](#getallpages)

# nextjs-commerce-shopify

Collection of hooks and data fetching functions to integrate Shopify in a React application. Designed to work with [Next.js Commerce](https://demo.vercel.store/) and can be used for other React commerce applications that need Shopify integration.

**This package requires modifications to your Next.js Commerce application. For a smoother integration, use their own Shopify support.**

## Getting Started

### Installation

```
yarn install nextjs-commerce-shopify
```

### CommerceProvider

Provider component that creates the commerce context for children.

```js
import { CommerceProvider } from 'nextjs-commerce-shopify';

const App = ({ children }) => {
  return (
    <CommerceProvider
      config={{
        domain: 'myshop.shopify.com',
        token: 'XXXXXX',
        currencyCode: 'SGD'
      }}
    >
      {children}
    </CommerceProvider>
  );
};

export default App;
```

The `config` takes:

- `domain`: Shopify domain. This is **required**.
- `token`: Shopify Storefront API Access Token. This is **required**.
- `currencyCode`: Currency code to use in store. Defaults to your Shopify default currency.
- `locale`: Used for currency format and if your Shopify supports translated content. Defaults to `en-US`.

### useCommerce

Returns the configs that are defined in the nearest `CommerceProvider`. Also provides access to Shopify's `checkout` and `shop`.

```js
import { useCommerce } from 'nextjs-commerce-shopify';

const { checkout, shop } = useCommerce();
```

- `checkout`: The information required to checkout items and pay ([Documentation](https://shopify.dev/docs/storefront-api/reference/checkouts/checkout)).
- `shop`: Represents a collection of the general settings and information about the shop ([Documentation](https://shopify.dev/docs/storefront-api/reference/online-store/shop/index)).

## Hooks

### usePrice

Display the product variant price according to currency and locale.

```js
import { usePrice } from 'nextjs-commerce-shopify';

const { price } = usePrice({
  amount
});
```

Takes in either `amount` or `variant`:

- `amount`: A price value for a particular item if the amount is known.
- `variant`: A shopify product variant. Price will be extracted from the variant.

### useAddItem

```js
import { useAddItem } from 'nextjs-commerce-shopify';

const AddToCartButton = ({ variantId, quantity }) => {
  const addItem = useAddItem();

  const addToCart = async () => {
    await addItem({
      variantId,
      quantity
    });
  };

  return <button onClick={addToCart}>Add To Cart</button>;
};
```

### useRemoveItem

```js
import { useRemoveItem } from 'nextjs-commerce-shopify';

const RemoveButton = ({ item }) => {
  const removeItem = useRemoveItem();

  const handleRemove = async () => {
    await removeItem({ id: item.id });
  };

  return <button onClick={handleRemove}>Remove</button>;
};
```

### useUpdateItem

```js
import { useUpdateItem } from 'nextjs-commerce-shopify';

const CartItem = ({ item }) => {
  const [quantity, setQuantity] = useState(item.quantity);
  const updateItem = useUpdateItem(item);

  const updateQuantity = async (e) => {
    const val = e.target.value;
    await updateItem({ quantity: val });
  };

  return (
    <input
      type="number"
      max={99}
      min={0}
      value={quantity}
      onChange={updateQuantity}
    />
  );
};
```

## APIs

Collections of APIs to fetch data from a Shopify store.

The data is fetched using the [Shopify JavaScript Buy SDK](https://github.com/Shopify/js-buy-sdk#readme). Read the [Shopify Storefront API reference](https://shopify.dev/docs/storefront-api/reference) for more information.

### getProduct

Get a single product by its `handle`.

```js
import { getProduct } from 'nextjs-commerce-shopify';

const product = await getProduct({
  domain,
  token,
  handle
});
```

### getAllProducts

```js
import { getAllProducts } from 'nextjs-commerce-shopify';

const products = await getAllProducts({
  domain,
  token
});
```

### getAllCollections

```js
import { getAllCollections } from 'nextjs-commerce-shopify';

const collections = await getAllCollections({
  domain,
  token
});
```

### getAllPages

```js
import { getAllPages } from 'nextjs-commerce-shopify';

const pages = await getAllPages({
  domain,
  token
});
```
