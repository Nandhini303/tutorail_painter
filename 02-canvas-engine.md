# 🎨 Tutorial 2 – Canvas Engine (Easy Tanglish)

## 🔥 Intha Tutorial-la Enna Learn Pannuvom?

Canvas Editor eppadi work aguthu nu full-a purinjukuvom.

Learn pannuvom

- 🎨 Konva.js Introduction
- 🖼 Stage & Layers
- ✏ Drawing Tools
- 🔷 Polygon Drawing
- 🔵 Vertex Editing
- ↩ Undo / Redo Logic

---

# 📘 Konva.js-na Enna?

Normally HTML Canvas use panna romba kashtam.

Konva.js use pannina

- Drag & Drop
- Resize
- Rotate
- Layers
- Selection
- Events

ellame easy.

Namma Canvas Editor full-a Konva use pannuthu.

---

# 🏗 Canvas Hierarchy

```
Konva Stage

│

├── Background Layer

│

├── Paint Layer

│

├── Polygon Layer

│

└── Anchor Layer
```

Simple Flow

```
Stage

↓

Layers

↓

Shapes

↓

User Interaction
```

---

# 🎨 Stage-na Enna?

Stage-na main canvas.

Ellaa drawing-um Stage-kulla than irukum.

Example

```
Browser

↓

Canvas

↓

Konva Stage
```

---

# 🗂 Layer-na Enna?

Layer-na Photoshop layer madhiri.

Example

```
Background Layer

↓

Wall Paint Layer

↓

Polygon Layer

↓

Selection Layer

↓

Guides Layer
```

Each layer separate.

One layer edit pannina matha layer affect aagadhu.

---

# 🚀 Stage Initialize

First Stage create pannuvom.

```ts
const stage = new Konva.Stage({
    container: container,
    width: 1200,
    height: 700
});
```

Meaning

```
Container

↓

Create Stage

↓

Ready for Drawing
```

---

# 🎨 Paint Layer

Next

Paint Layer create pannuvom.

```ts
const paintLayer = new Konva.Layer();

stage.add(paintLayer);
```

Flow

```
Stage

↓

Paint Layer

↓

Brush Drawing
```

---

# ✏ Drawing Process

User mouse press pannrar.

```
Mouse Down

↓

Create Shape

↓

Mouse Move

↓

Update Shape

↓

Mouse Up

↓

Save Shape
```

---

# 🔷 Polygon Tool

Polygon use panni wall select pannuvom.

Example

```
Click

↓

Point 1

↓

Click

↓

Point 2

↓

Click

↓

Point 3

↓

Double Click

↓

Polygon Complete
```

---

# 🔵 Vertex Handles

Polygon create aana apram

Every point-ku

Small blue circle varum.

```
●──────●

│      │

│      │

●──────●
```

Ivanga

Anchor Handles.

---

# 🖱 Vertex Drag

Blue Handle drag pannina

Polygon update agum.

```
Old Shape

↓

Move Vertex

↓

New Shape
```

Real-time update.

---

# 🎨 Wall Painting

Polygon complete aana

Selected area-ku

Color apply pannuvom.

```
Polygon

↓

Selected Area

↓

Fill Color

↓

Render
```

---

# ↩ Undo

Undo-na

Last Action remove pannum.

Example

```
Brush 1

↓

Brush 2

↓

Brush 3

↓

Undo

↓

Brush 3 Remove
```

Brush1 & Brush2 irukum.

Canvas clear aagadhu.

---

# ↪ Redo

Undo pannadhu

Thirumba restore pannum.

```
Undo

↓

Redo

↓

Last Stroke Back
```

---

# 🗃 Undo Stack

Internally

```
Undo Stack

↓

Stroke1

↓

Stroke2

↓

Stroke3
```

Undo

↓

Stroke3 remove.

Redo Stack-ku move agum.

---

# 🗃 Redo Stack

```
Redo Stack

↓

Stroke3
```

Redo

↓

Paint Layer-ku thirumba add pannuvom.

---

# 🖼 Image Layer

Room image separate layer.

```
Room Image

↓

Background Layer
```

Brush remove pannina

Image remove aagadhu.

---

# 🎨 Paint Flow

```
Upload Image

↓

Background Layer

↓

Select Brush

↓

Choose Color

↓

Draw

↓

Paint Layer

↓

Autosave
```

---

# 🖌 Brush Tool

Brush

```
Mouse Down

↓

Start Line

↓

Mouse Move

↓

Add Points

↓

Mouse Up

↓

Save Stroke
```

---

# 🪣 Fill Tool

Fill Tool

```
Click Wall

↓

Selected Polygon

↓

Apply Color

↓

Update Canvas
```

---

# 🔍 Zoom

Mouse Wheel

↓

Zoom In

↓

Zoom Out

Konva Stage scale change pannum.

---

# ✋ Pan Tool

Hold Mouse

↓

Move Canvas

↓

Release

Canvas position mattum change agum.

Objects move aagadhu.

---

# 🧪 Practice

## Goal

Ellipse Tool add pannunga.

### Step 1

Signal update

```ts
activeTool = signal('ellipse');
```

---

### Step 2

Toolbar button add pannunga.

```html
Ellipse
```

---

### Step 3

Mouse Down

```ts
new Konva.Ellipse({
    x:100,
    y:100
});
```

---

# ✅ Check Yourself

✔ Ellipse Tool select panna mudiyudha?

✔ Mouse drag panna ellipse create agudha?

✔ Undo work agudha?

✔ Redo work agudha?

✔ Image disappear aagala?

---

# 🧠 Easy Memory Trick

```
Stage

↓

Layer

↓

Shape

↓

User Draw

↓

Undo Stack

↓

Redo Stack
```

---

# 📌 Konva Components

| Component | Purpose |
|------------|----------|
| Stage | Main Canvas |
| Layer | Photoshop Layer |
| Line | Brush Stroke |
| Polygon | Wall Selection |
| Circle | Vertex Handle |
| Image | Room Image |
| Transformer | Resize / Rotate |
| Group | Multiple Objects |

---

# 🎯 Complete Canvas Flow

```
Upload Room

↓

Background Layer

↓

Select Tool

↓

Draw Polygon

↓

Vertex Editing

↓

Apply Paint

↓

Undo / Redo

↓

Autosave

↓

MongoDB

↓

Realtime Sync
```

---

## ✅ End of Tutorial 2

If you understand **Stage → Layer → Shape → Events → Undo/Redo**, you'll understand almost **90% of how the Smart Wall Paint Visualizer Canvas Engine works**.
