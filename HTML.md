## 🧱 HTML & CSS Basics

---

### 📄 HTML5 Boilerplate

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Page Title</title>
  </head>
  <body>
    <!-- Content goes here -->
  </body>
</html>
```

---

### 🧩 Elements and Attributes

- HTML elements can have **attributes**.
- Example of an anchor (`<a>`) with an attribute:
  ```html
  <a href="page.html">Click here</a>
  ```
- Example of an image:
  ```html
  <img src="image.jpg" alt="Description">
  ```
- 🔹 `href` → used in links  
- 🔹 `src` → used in images (source)  
- 🔹 `alt` → describes the image  
- ⚠️ `<img>` is self-closing

---

### 📚 HTML Lists

- Ordered list → `<ol>`
- Unordered list → `<ul>`
- List item → `<li>`

```html
<ol>
  <li>First</li>
  <li>Second</li>
</ol>
```

Used for:
- Bullet/numbered content
- Navigation
- Organizing content

---

### 🎨 CSS Basics

- CSS is written in **property-value pairs**
- Always use `:` between property and value, and end with `;`

```css
color: red;
font-size: 21px;
font-weight: bold;
```

#### Inline CSS
```html
<p style="color: steelblue; font-size: 21px;">Text</p>
```

---

### ✨ Common CSS Properties

- `color`
- `font-size`
- `font-weight`
- `background-color`
- `text-align`

**Colors** can be defined using:
- Keywords: `red`, `steelblue`, `hotpink`
- Hex codes: `#ff0000`
- RGB: `rgb(255, 0, 0)`
- HSL: `hsl(0, 100%, 50%)`

---

### 🧭 CSS Selectors

#### Element Selector
```css
p {
  color: black;
}
```

#### Class Selector
```html
<p class="intro">Hello</p>
```

```css
.intro {
  font-size: 18px;
}
```

#### ID Selector
```html
<p id="unique">Only me</p>
```

```css
#unique {
  font-weight: bold;
}
```

- 🔸 Use `.` for class selectors  
- 🔸 Use `#` for ID selectors  
- 🔹 IDs should be unique  
- 🔹 Classes can be reused

---

### 🧷 External CSS

Link it in the `<head>`:
```html
<link rel="stylesheet" href="styles.css">
```

- Keeps your HTML clean  
- Viewer won’t see the CSS directly

---

### 💬 Comments

- **HTML**:
  ```html
  <!-- This is a comment -->
  ```

- **CSS**:
  ```css
  /* This is a comment */
  ```

---

### 📦 HTML Structure Tags

- `<header>` – Top intro section  
- `<nav>` – Navigation links  
- `<main>` – Main page content  
- `<section>` – Thematic grouping  
- `<footer>` – Bottom info  
- `<div>` – Generic container

---

### 📐 Block-Level Elements

- Default width: `100%`  
- Default height: `0`, grows with content  
- Stack vertically

---

### 📏 Box Model Overview

#### Margin
Space **outside** the element

```css
margin: 10px 20px 30px 40px;
/* top right bottom left */
```

Shortcuts:
```css
margin: 10px;           /* all sides */
margin: 10px 20px;       /* top & bottom | left & right */
margin: 10px 20px 30px;  /* top | left & right | bottom */
```

#### Padding
Space **inside** the element (between content and border)

```css
padding: 10px;
```

---

### 🔲 Borders

```css
border: 1px solid black;
```

- `1px` → thickness  
- `solid` → style (solid, dashed, dotted...)  
- `black` → color

---

### 🎯 Centering with Margin

```css
margin: 0 auto;
width: 50%;
```

- Set `margin: auto` to center  
- Must set a **width** for proper centering

---