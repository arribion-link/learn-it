simple and elegant way to create an **infinite scrolling carousel animation using only CSS** — no JavaScript required.

---

### 🎯 Goal
Create a horizontal carousel that scrolls infinitely, looping through items seamlessly.

---

### 🧩 HTML Structure

```html
<div class="carousel">
  <div class="carousel-track">
    <div class="carousel-item">Item 1</div>
    <div class="carousel-item">Item 2</div>
    <div class="carousel-item">Item 3</div>
    <div class="carousel-item">Item 4</div>
    <!-- Repeat items to create seamless loop -->
    <div class="carousel-item">Item 1</div>
    <div class="carousel-item">Item 2</div>
    <div class="carousel-item">Item 3</div>
    <div class="carousel-item">Item 4</div>
  </div>
</div>
```

---

### 🎨 CSS Styling and Animation

```css
.carousel {
  overflow: hidden;
  width: 100%;
  background: #f0f0f0;
}

.carousel-track {
  display: flex;
  width: calc(200%); /* Double width for looping */
  animation: scroll 20s linear infinite;
}

.carousel-item {
  flex: 0 0 25%; /* 4 items per view */
  box-sizing: border-box;
  padding: 20px;
  background: #fff;
  border: 1px solid #ccc;
  text-align: center;
  font-size: 1.2em;
}

/* Keyframes for infinite scroll */
@keyframes scroll {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-50%);
  }
}
```

---

### 🧠 How It Works

- The `.carousel-track` is twice the width of the visible area and contains two sets of items.
- The `@keyframes scroll` animation moves the track leftward by 50%, creating a seamless loop.
- Items are repeated to ensure continuity when the animation resets.

---

### ⚠️ Common Glitch Causes & Fixes

#### 1. **Container width mismatch**
If `.stack-container` is animating but its width isn't wide enough to accommodate all images twice, the scroll will stutter or snap.

**Fix:**
```css
.stack-container {
  display: flex;
  width: max-content; /* Ensures full width of all images */
  animation: scroll 30s linear infinite;
}
```

#### 2. **Missing overflow wrapper**
If you don’t wrap the animated container in a parent with `overflow: hidden`, the animation may show unwanted edges or cause layout shifts.

**Fix:**
```css
.stack-wrapper {
  overflow: hidden;
  width: 100%;
}
```

#### 3. **Uneven image sizes**
If your images have inconsistent widths or margins, the scroll may appear jerky.

**Fix:**
```css
.stack-images {
  height: 60px;
  width: auto;
  margin: 0 1em;
  flex-shrink: 0;
}
```

#### 4. **Incorrect animation distance**
If you're using `translateX(-50%)` but the duplicated content isn't exactly half the width, the loop will glitch.

**Fix:**
Use `width: max-content` and duplicate the exact set of items once. Then animate to `translateX(-50%)`.

---

### ✅ Recommended Setup Summary

```html
<div class="stack-wrapper">
  <div class="stack-container">
    <!-- 8 images duplicated once -->
    <img src="..." class="stack-images" />
    ...
    <img src="..." class="stack-images" />
  </div>
</div>
```

```css
.stack-wrapper {
  overflow: hidden;
  width: 100%;
}

.stack-container {
  display: flex;
  width: max-content;
  animation: scroll 30s linear infinite;
}

.stack-images {
  height: 60px;
  margin: 0 1em;
  flex-shrink: 0;
}

@keyframes scroll {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-50%);
  }
}
```

---