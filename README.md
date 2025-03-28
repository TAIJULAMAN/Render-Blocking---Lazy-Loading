## Render Blocking & Lazy Loading 


#⚡ Optimizing Render-Blocking Resources & Lazy Loading

Improve your website's **performance and user experience** by reducing render-blocking resources and loading content only when needed. This guide breaks down modern best practices used in the js13kPWA example app.

---

## 🚫 What Are Render-Blocking Resources?

When HTML, CSS, and JS are loaded all at once, rendering is delayed — users see a blank page for several seconds. To avoid this:

---

## 🧩 JavaScript Optimization

### ✅ Use `defer` Attribute

```html
<script src="app.js" defer></script>
```

Allows HTML parsing before JS execution.

---

### ✅ Dynamic Import (Load on Interaction)

```js
document.getElementById("open-search").addEventListener("click", async () => {
  const searchModule = await import("/modules/search.js");
  searchModule.loadAutoComplete();
});
```

Only loads JS when needed (e.g., on button click).

---

## 🎨 CSS Optimization

### ✅ Media-Specific Stylesheets

```html
<link rel="stylesheet" href="style.css" />
<link rel="stylesheet" href="print.css" media="print" />
```

Loads print styles only when required.

---

## 🖼️ Image Optimization

### ✅ Use Placeholders + `data-src`

```html
<img src="data/img/placeholder.png" data-src="data/img/SLUG.jpg" alt="NAME" />
```

Placeholder loaded first; real image swapped in via JS.

---

### ✅ JavaScript-Driven Image Loader

```js
let imagesToLoad = document.querySelectorAll("img[data-src]");

const loadImages = (image) => {
  image.setAttribute("src", image.getAttribute("data-src"));
  image.onload = () => image.removeAttribute("data-src");
};

imagesToLoad.forEach(loadImages);
```

---

### ✅ Blur Transition with CSS

```css
article img[data-src] {
  filter: blur(0.2em);
}

article img {
  filter: blur(0);
  transition: filter 0.5s;
}
```

Creates a smooth "unblur" effect on image load.

---

## 🐌 Lazy Loading

### ✅ Native Lazy Loading

```html
<img src="placeholder.png" data-src="real.jpg" loading="lazy" />
```

Automatically delays image loading until it's near the viewport.

---

### ✅ Intersection Observer API

```js
if ("IntersectionObserver" in window) {
  const observer = new IntersectionObserver((items, observer) => {
    items.forEach((item) => {
      if (item.isIntersecting) {
        loadImages(item.target);
        observer.unobserve(item.target);
      }
    });
  });

  imagesToLoad.forEach((img) => observer.observe(img));
} else {
  imagesToLoad.forEach(loadImages);
}
```

More control over when and how images load based on scroll position.

---

## 🚀 Advanced Ideas

- Use `<noscript>` fallbacks for images
- Wrap images in `<a>` for manual load access
- Implement infinite scroll (load items as user scrolls)
- Consider inline critical CSS in `<style>` for ultra-fast rendering

---

## 🧠 Conclusion

Speed up your app by:

- Reducing initial load
- Splitting large files
- Using placeholders
- Deferring and dynamically importing JS
- Lazy loading images on demand

These small changes lead to big improvements in user experience and performance.

---

## 🧪 Explore Further

- [TinyPNG](https://tinypng.com) — Compress images
- [web.dev/measure](https://web.dev/measure) — Test site performance
- [MDN Docs: Intersection Observer](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)

---

## 📝 License

MIT License. Feel free to use, modify, and enhance!
