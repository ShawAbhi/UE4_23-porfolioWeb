# UE4_23-porfolioWeb

# 🎉 Final Guide: Fullscreen Canvas in Unreal Engine 4.23 HTML5 Export (Without Fullscreen Mode)

This guide explains how to make Unreal Engine 4.23 HTML5 builds **use the entire browser window** dynamically, **without forcing fullscreen mode**, and **remove all unnecessary UI clutter**.

---

## ✨ Step-by-Step Instructions

### 1. ✅ Edit `BaseTP.html`

Replace the canvas section to contain **only** the canvas element:

```html
<canvas tabindex="0" id="canvas" oncontextmenu="event.preventDefault()"></canvas>
```

**Remove** the wrapper `<div class="wrapper" id="mainarea">...</div>`.

---

### 2. ✅ Update `BaseTP.css`

Add the following styles:

```css
html, body {
	margin: 0;
	padding: 0;
	width: 100vw;
	height: 100vh;
	overflow: hidden;
	background-color: black; /* Optional */
}

canvas#canvas {
	margin: 0;
	padding: 0;
	display: block;
	width: 100vw !important;
	height: 100vh !important;
	border: none;
}

#buttonrow, #logwindow {
	display: none !important;
	pointer-events: none;
}
```

---

### 3. ✅ Add a Custom Canvas Resizer in `BaseTP.UE4.js`

Right after this line:

```js
Module['canvas'] = document.getElementById('canvas');
```

Add:

```js
function forceFullscreenCanvas() {
	const canvas = Module['canvas'];
	canvas.style.width = "100vw";
	canvas.style.height = "100vh";
	canvas.width = window.innerWidth;
	canvas.height = window.innerHeight;

	if (typeof Module.setCanvasSize === 'function') {
		Module.setCanvasSize(window.innerWidth, window.innerHeight);
	}
	if (typeof Module.UE4_resizeCanvas === 'function') {
		Module.UE4_resizeCanvas(true);
	}

	console.log("Canvas forcibly resized to:", canvas.width, canvas.height);
}

window.addEventListener('load', forceFullscreenCanvas);
window.addEventListener('resize', () => {
	requestAnimationFrame(forceFullscreenCanvas);
});
```

---

### 4. ✅ Fix Unreal's `resizeCanvas()` Function

Find this function in `BaseTP.UE4.js`:

```js
function resizeCanvas(aboutToEnterFullscreen) { ... }
```

Inside it:

- Replace:

```js
var mainArea = document.getElementById('mainarea');
var canvasRect = mainArea.getBoundingClientRect();
```

**With:**

```js
var mainArea = document.getElementById('mainarea');
var canvasRect = { left: 0, right: window.innerWidth, top: 0, bottom: window.innerHeight };
```

- Then **comment out** this line inside the same function:

```js
// Module['canvas'].style.height = mainArea.style.height = cssHeight + 'px';
```

---

## ✅ Final Result

- Canvas stretches and covers the entire browser window
- Resolution updates dynamically when resizing
- No black bars
- Button UI and log windows are hidden

---

# 🔧 Clean, Professional HTML5 Packaging Done Right!

Now you can easily export clean and professional UE4.23 HTML5 builds without fullscreen mode, and fully use the browser screenspace.

---

Happy developing! 🚀

