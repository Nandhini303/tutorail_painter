# Tutorial 2: Canvas Engine (Konva.js)

### What you'll learn:
- Setting up the Konva Stage and Layers
- Building drawing tools and polygons
- Handling Undo/Redo logic natively

**Prerequisites:** [Tutorial 1: Project Setup & Architecture](./01-fundamentals.md)

---

## 1. Setting up Konva.js
In `canvas-editor.component.ts`, the canvas is initialized directly into an HTML container.

```typescript
// src/app/features/canvas-editor/canvas-editor.component.ts
initStage() {
  const container = this.canvasContainer.nativeElement;
  this.stage = new Konva.Stage({
    container: container,
    width: container.offsetWidth,
    height: container.offsetHeight,
  });

  this.bgLayer = new Konva.Layer();
  this.paintLayer = new Konva.Layer();
  
  this.stage.add(this.bgLayer);
  this.stage.add(this.paintLayer);
}
```

## 2. Drawing Polygons & Vertices
For architectural walls, we use `Konva.Line` to create closed polygons. We also dynamically render anchor points so users can edit the vertices!

```typescript
// src/app/features/canvas-editor/canvas-editor.component.ts
renderPolygonAnchors(polygon: Konva.Line) {
  this.clearPolygonAnchors();
  const points = polygon.points();
  
  for (let i = 0; i < points.length; i += 2) {
    const anchor = new Konva.Circle({
      x: points[i],
      y: points[i + 1],
      radius: 6,
      fill: '#3b82f6',
      draggable: true,
    });
    // ...
```

## 3. Undo & Redo System
We manage state history efficiently by maintaining an array of Konva node states.

```typescript
// src/app/features/canvas-editor/canvas-editor.component.ts
undo(): void {
  if (this.paintLayer && this.paintLayer.getChildren().length > 0) {
    const children = this.paintLayer.getChildren();
    const lastStroke = children[children.length - 1];
    if (lastStroke) {
      this.redoStack.push(lastStroke);
      lastStroke.remove();
      this.paintLayer.batchDraw();
      this.canUndo.set(this.paintLayer.getChildren().length > 0);
      this.canRedo.set(true);
    }
  }
}
```

---

### Try it yourself!
*Exercise:* Add a "Clear All" button to the UI that calls `this.paintLayer.destroyChildren()` and clears the history stack.