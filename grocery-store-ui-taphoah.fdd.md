# 1. Overview

Build a static grocery store website (Vietnamese local grocery shop).

Store Name: Ngọc Hoàng

Features:

* Display grocery products (food, drinks, daily items)
* Search products
* Categorized sections
* Add product (protected by admin key)
* Store data in localStorage

---

# 2. UI Layout

```plaintext id="layout_taphoah"
--------------------------------------------------
TOP BAR
  - "Chào mừng bạn đến với cửa hàng tạp hoá Ngọc Hoàng"

HEADER
  - Logo text: "Tạp Hoá Ngọc Hoàng"
  - Search bar (placeholder: "Tìm sản phẩm...")
  - Phone: 0966 997 711
  - Cart button (UI only)

NAVBAR
  - Trang chủ
  - Đồ uống
  - Mì - Đồ ăn liền
  - Bánh kẹo
  - Gia vị
  - Đồ gia dụng

BANNER
  - Big banner: quảng cáo hàng tạp hoá
  - Small banners: khuyến mãi

FEATURE SECTION
  - Giá rẻ mỗi ngày
  - Hàng chính hãng
  - Giao nhanh

PRODUCT SECTION
  - "Sản phẩm bán chạy"
  - "Đồ uống"
  - "Mì ăn liền"

FLOAT BUTTON (+)
--------------------------------------------------
```

---

# 3. Categories

```json id="categories_taphoah"
[
  "ban_chay",
  "do_uong",
  "mi_goi",
  "banh_keo",
  "gia_vi",
  "do_gia_dung"
]
```

---

# 4. Data Model

```json id="product_model_taphoah"
{
  "id": "string",
  "name": "string",
  "price": "number",
  "imageUrl": "string",
  "category": "string",
  "createdAt": "string"
}
```

---

# 5. LocalStorage

```js id="storage_taphoah"
const STORAGE_KEY = "taphoa_products";
const ADMIN_KEY = "123456";
```

---

# 6. Default Data (IMPORTANT)

```json id="default_taphoah"
[
  {
    "id": "1",
    "name": "Mì tôm Hảo Hảo",
    "price": 3500,
    "imageUrl": "https://via.placeholder.com/200",
    "category": "mi_goi",
    "createdAt": "2026-01-01"
  },
  {
    "id": "2",
    "name": "Nước suối Lavie",
    "price": 5000,
    "imageUrl": "https://via.placeholder.com/200",
    "category": "do_uong",
    "createdAt": "2026-01-01"
  },
  {
    "id": "3",
    "name": "Bánh Oreo",
    "price": 12000,
    "imageUrl": "https://via.placeholder.com/200",
    "category": "banh_keo",
    "createdAt": "2026-01-01"
  }
]
```

---

# 7. Components

## 7.1 Header

State:

```json id="header_taphoah"
{
  "searchKeyword": ""
}
```

---

## 7.2 Navbar

* Static menu

---

## 7.3 BannerSection

* Grocery promotion images

---

## 7.4 ProductSection

Props:

```json id="section_taphoah"
{
  "title": "string",
  "category": "string",
  "products": "Product[]"
}
```

---

## 7.5 ProductCard

Props:

```json id="card_taphoah"
{
  "name": "string",
  "price": "number",
  "imageUrl": "string"
}
```

UI:

* Image
* Product name
* Price
* Button: "Thêm vào giỏ"

---

## 7.6 AddProductModal

State:

```json id="modal_taphoah"
{
  "name": "",
  "price": "",
  "imageUrl": "",
  "category": "ban_chay",
  "adminKey": "",
  "error": ""
}
```

---

# 8. Core Logic

## Load Products

```js id="load_taphoah"
function loadProducts() {
  const data = localStorage.getItem(STORAGE_KEY);
  if (!data) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(DEFAULT_PRODUCTS));
    return DEFAULT_PRODUCTS;
  }
  return JSON.parse(data);
}
```

---

## Add Product

```js id="add_taphoah"
function addProduct(products, form) {
  if (form.adminKey !== ADMIN_KEY) {
    throw new Error("INVALID_KEY");
  }

  const newProduct = {
    id: Date.now().toString(),
    name: form.name,
    price: Number(form.price),
    imageUrl: form.imageUrl || "https://via.placeholder.com/200",
    category: form.category,
    createdAt: new Date().toISOString()
  };

  const updated = [newProduct, ...products];
  localStorage.setItem(STORAGE_KEY, JSON.stringify(updated));
  return updated;
}
```

---

## Search

```js id="search_taphoah"
function searchProducts(products, keyword) {
  return products.filter(p =>
    p.name.toLowerCase().includes(keyword.toLowerCase())
  );
}
```

---

## Filter Category

```js id="filter_taphoah"
function filterByCategory(products, category) {
  return products.filter(p => p.category === category);
}
```

---

# 9. UI Rules

## Price format

```js id="price_taphoah"
price.toLocaleString('vi-VN') + ' đ'
```

---

## Grid

* Mobile: 2 columns
* Desktop: 4 columns

---

# 10. Add Product Flow

1. Click "+"
2. Open modal
3. Input:

   * Tên sản phẩm
   * Giá
   * Link ảnh
   * Category
   * Admin key
4. Submit

IF key sai:

* Hiển thị: "Sai key"

IF đúng:

* Lưu localStorage
* Update UI

---

# 11. UX Suggestion (IMPORTANT)

* Highlight sản phẩm bán chạy
* Badge "HOT"
* Hover zoom ảnh
* Button màu xanh lá

---

# 12. Folder Structure

```plaintext id="folder_taphoah"
src/
  components/
    Header.vue
    Navbar.vue
    BannerSection.vue
    ProductSection.vue
    ProductCard.vue
    AddProductModal.vue
  pages/
    Home.vue
  utils/
    storage.js
```

---

# 13. Notes

* Không có backend
* Không cần login
* Key chỉ để hạn chế cơ bản

---
