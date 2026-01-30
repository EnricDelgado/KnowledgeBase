# Shader Learning Roadmap & Resource Guide

You mentioned encountering advanced technical art concepts like **9-sliceable flow maps**, **UI space splines**, **pivot caching**, and **vector baking**. These sound intimidating, but they are essentially creative uses of fundamental shader principles: manipulating UV coordinates, storing custom data in mesh attributes (like vertex colors), and packing mathematical data into textures.

This guide organizes your existing resources (from `_SORT_THIS.md` and the `Unity` folder) into a structured learning path to help you master these concepts.

## 1. The Concepts Demystified
Before diving into tutorials, here is what those terms actually mean in the context of shaders:

*   **9-Sliceable Flow Maps**:
    *   *What*: Combining "9-slice scaling" (keeping corners scaling correctly for UI/sprites) with "flow maps" (textures that tell a customized UV distortion direction).
    *   *Goal*: To have a UI element or sprite that stretches dynamically but still has a cool flowing liquid/energy effect that doesn't get distorted at the borders.
*   **UI Space Splines**:
    *   *What*: Drawing smooth curves (splines) directly in the UI canvas, often using Signed Distance Fields (SDFs) in a pixel shader rather than heavy mesh geometry.
    *   *Goal*: Crisp, infinite-resolution curves for UI connections, health bars, or indicators.
*   **Pivot Caching using Vertex Colors**:
    *   *What*: Baking the "pivot point" position of sub-objects (like leaves on a tree or debris) into the vertex color channel of the mesh.
    *   *Goal*: Allows the shader to rotate each leaf individually knowing where its "center" is, even though they are all combined into one single static mesh.
*   **Vector Baking (Low Fidelity Textures)**:
    *   *What*: Storing non-color data (like velocity vectors, flow directions, or positions) into small textures.
    *   *Goal*: Using "ugly" low-res textures to drive complex math in the shader because you only need the *data*, not high-frequency visual detail.

---

## 2. Resource Classification
I have organized the links and files from your KnowledgeBase into logical learning tiers.

### Tier 1: The Foundations (Start Here)
*Essential for understanding how the GPU processes pixels and vertices.*

*   **The Book of Shaders**: [thebookofshaders.com/00/](https://thebookofshaders.com/00/) - *The bible of fragment shaders. Read this first.*
*   **Catlike Coding**: [catlikecoding.com/unity/tutorials/](https://catlikecoding.com/unity/tutorials/) - *Excellent step-by-step Unity tutorials covering basics to advanced rendering.*
*   **Daniel Ilett**: [danielilett.com](https://danielilett.com/) - *Great intro to shader basics and geometry.*
    *   *Specific basic*: [Shader Basics](https://danielilett.com/2019-04-27-tut1-0-smo-shader-basics/)
*   **Shader Development**: [shaderdev.com](https://shaderdev.com/)

### Tier 2: Texture & Data Optimization (Crucial for "Baking")
*To understand "Vector Baking" and "Flow Maps", you must understand texture formats and compression.*

*   **Official Docs**: [Unity Texture Compression Formats](https://docs.unity3d.com/Manual/texture-compression-formats.html)
*   **Tech Art Hub**:
    *   [Unity Texture Compression Cheat Sheet](https://techarthub.com/unity-texture-compression-cheat-sheet/)
    *   [Intro to Texture Compression](https://techarthub.com/an-introduction-to-texture-compression-in-unity/)
*   **Deep Dives**:
    *   [PVRTC Compression](https://blog.imaginationtech.com/pvrtc-the-most-efficient-texture-compression-standard-for-the-mobile-graphics-world/)
    *   [Texture Formats for 2D Games](https://joostdevblog.blogspot.com/2015/11/texture-formats-for-2d-games-part-3-dxt.html)

### Tier 3: Shader Graph & Visual Effects
*Practical implementation of "Flow" and "Splines".*

*   **Ben Cloward (YouTube)**: *Incredible technical artist explaining the "Why" and "How".*
    *   [Shader Graph Technical Overview](https://www.classcentral.com/classroom/youtube-shader-graph-technical-overview-with-atbencloward-450814)
    *   [Rain Effects (Deep Dive)](https://www.classcentral.com/classroom/youtube-rain-effects-shader-graph-deep-dive-ft-atbencloward-459764) - *Likely covers flow mapping.*
    *   [Advanced 2D Shader Effects](https://www.classcentral.com/classroom/youtube-20-advanced-2d-shader-effects-part-1-305655)
*   **Unity Manual**: [Visual Effects](https://docs.unity3d.com/6000.3/Documentation/Manual/visual-effects.html)

### Tier 4: Math & Geometry (For Splines & Vertex Colors)
*Where the "Pivot Caching" and "SDF" magic happens.*

*   **Your Books**:
    *   `_BOOKS/Visualising Equations 1` & `2` (PDFs covers procedural shapes and math).
*   **Advanced Topics**:
    *   [Vertex Buffer Layouts](https://www.classcentral.com/classroom/youtube-vertex-buffer-layouts-game-engine-series-140870) - *Directly relevant to understanding how to pass custom data (like pivot points) to vertices.*
    *   [Ray Tracing](https://www.classcentral.com/classroom/youtube-coding-adventure-ray-tracing-154768)

---

## 3. The Roadmap to Success

Follow this path to bridge the gap from your current knowledge to the concepts you heard in the meeting.

### Phase 1: Vertex & Fragment Basics
**Goal**: Understand the pipeline.
1.  Complete the first few chapters of **The Book of Shaders**.
2.  Follow **Daniel Ilett’s** "Shader Basics" tutorial.
3.  **Concept Check**: Can you write a shader that colors a mesh based on its Normal vector? Can you scroll a texture using `Time`?

### Phase 2: Data in Textures (The "Vector Baking" Concept)
**Goal**: Treat textures as data containers, not just pictures.
1.  Read the **Texture Compression Cheat Sheet**.
2.  Understand **Channels**: A texture is just R, G, B, A numbers. You can store X velocity in R, Y velocity in G.
3.  **Exercise**: create a small texture in Photoshop where Red = X direction and Green = Y direction. Read it in a shader to move pixels. You just did "Vector Baking".

### Phase 3: Mesh Data (The "Pivot Caching" Concept)
**Goal**: Use Vertex Colors for logic, not color.
1.  Watch the **Vertex Buffer Layouts** video.
2.  **Exercise**: Make a mesh of 4 cubes in Blender. detailed logic:
    *   In Blender/Maya, paint the vertices of Cube 1 Red=(1,0,0), Cube 2 Red=(0,1,0), etc.
    *   Import to Unity.
    *   Write a shader that moves the vertex UP only if `VertexColor.R > 0.5`.
    *   *Result*: You now understand how to use vertex data to control sub-objects independently (the core of "Pivot caching").

### Phase 4: UV Math (The "9-Slice Flow Map" Concept)
**Goal**: Manipulate coordinates.
1.  Watch **Ben Cloward's** "Rain Effects" or "Flow" tutorials.
2.  Learn **9-Slice** in Unity UI.
3.  **Combine**: Write a shader that takes UV coordinates, applies 9-slice logic (math that preserves corners), and *then* adds a scrolling distortion (flow) to the middle section UVS.

### Phase 5: Procedural Shapes (The "UI Space Splines" Concept)
**Goal**: Draw math, don't sample textures.
1.  Read your `Visualising Equations` PDFs.
2.  Search **Inigo Quilez** (SDFs) online (The Book of Shaders touches on this).
3.  **Exercise**: Write a pixel shader that draws a circle using `distance(uv, center)`. Then try to draw a line between two points. You are now rendering "UI Space Splines".

## Summary
You have a great collection of resources effectively hidden in that `_SORT_THIS.md` list. The key is to stop thinking of "Shaders" as one big topic, and start thinking of them as:
1.  **Math on Coordinates** (Flow maps, Splines)
2.  **Data Storage** (Vertex colors, Texture baking)

Start with **Daniel Ilett** for quick wins, then move to **Catlike Coding** for depth. Use **Ben Cloward** when you start working in Shader Graph.
