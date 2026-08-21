# Pathfinder

**A\* pathfinding on your own map — upload an image, and watch the shortest route get found in real time.**

Upload any map image (a screenshot, a scanned floor plan, a PDF), sample the color of the roads/paths with a click, and Pathfinder automatically extracts the road network and runs an animated A\* search between any two points you pick.
# webpage-link
https://haisam10.github.io/Pathfinder/

![Pathfinder demo](https://raw.githubusercontent.com/haisam10/Pathfinder/refs/heads/main/Pathfinder.gif)

---

## Features

- **Upload your own map** — PNG, JPG, or PDF (first page is rendered automatically)
- **Color-based road extraction** — click a few road pixels to sample their color, adjust the tolerance slider, and preview the generated mask live
- **Automatic road-network detection** — Zhang–Suen skeletonization thins the road mask down to single-pixel-wide centerlines, which are then converted into a weighted graph of intersections and segments
- **Interactive A\* search** — click a start point and an end point on the map; the shortest path is computed with A\* over the extracted graph
- **Animated visualization**
  - A heatmap "wavefront" sweeps outward from the start node as the search explores the graph, colored by distance from the start
  - Every road segment the search actually checks stays persistently highlighted, so you can see exactly what was explored
  - The final shortest route is traced and highlighted in gold once found
- **Live stats** — distance, nodes explored, and compute time
- **Pan & zoom** on the map, drag to pan, scroll to zoom

## How it works

1. **Upload** — the map image (or PDF page) is loaded and downscaled to a working resolution for performance, with care taken to preserve crisp road colors during the resize.
2. **Sample** — you click a few pixels on visible roads; Pathfinder builds a color mask of every pixel within a tolerance of your samples.
3. **Extract** — the mask is thinned to a 1-pixel skeleton (Zhang–Suen algorithm), then walked to build a graph: junctions and dead-ends become nodes, and the corridors between them become weighted edges.
4. **Search** — click a start and end point; A\* runs over the graph using Euclidean distance as its heuristic, and the exploration + resulting path are animated on the canvas.

## Usage

1. Open `pathfinder.html` in a browser (or visit the hosted version, if deployed).
2. Drop in a map image or PDF.
3. Click a few road pixels to sample their color, and tune the tolerance slider until the mask preview cleanly covers the roads.
4. Click **Extract roads**.
5. Click a start point, then an end point, and watch the search run.

## Tech stack

- Vanilla JavaScript + HTML5 Canvas — no frameworks, no build step
- [pdf.js](https://mozilla.github.io/pdf.js/) for PDF rendering
- Custom implementations of:
  - Zhang–Suen thinning for skeleton extraction
  - Graph construction from a raster skeleton
  - A\* search with a binary min-heap

## Notes

- Larger/higher-resolution maps take longer to process (thinning and graph-building run client-side), but produce more accurate road networks.
- The color-sampling approach works best on maps with distinct, consistent road coloring (e.g. schematic/vector-style maps). Photorealistic satellite imagery may need a higher tolerance or extra samples.

