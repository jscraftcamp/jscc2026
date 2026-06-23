# Game Map Rendered from OpenStreetMap Data

> hosted by Faisal and Vadim

## Summary

- showed off little demo project: https://github.com/FaisalBinAhmed/pixijs-isotoon
- data pulled from openstreetmap, saved statically as JSON
- PixiJS used for rendering on canvas
- caveat: 2D only library
- "fake" 3D by applying isometric perspective
- switched between Canvas2D and WebGL rendering (PixiJS supports both). Discussed the performance advantages of WebGL due to hardware acceleration.
- WebGL allows for fun filters that can be applied to the map. You can play around with that in our demo. PixiJS offers many of those s.t. you don't necessarily have to write WebGL shader code.

## Discussion

- real 3D library will likely be more performant since perspective is calculated on CPU. in 3D library like babylon.js and three.js you could write a vertex shader that does that transformation for you.
- played around with Chrome settings: disabled hardware acceleration and watched the framerate plummet.
- question: does PixiJS support glTF?
  - no, since 2D only. But there are libraries that might help: https://github.com/my2iu/pixi-omber-gltf2-vector
- PixiJS seamlessly integrates with 3d libraries like three.js and can be rendered to the same canvas even: https://pixijs.com/8.x/guides/third-party/mixing-three-and-pixi
- Rendering text in canvas is troublesome. Also no layout natively available there. But [HTML in Canvas](https://html-in-canvas.dev/) is coming and will hopefully see broader adoption!
- for more serious projects if you want to render map data to : use [MapLibre](https://maplibre.org/) (talk on the same day in the conference)
