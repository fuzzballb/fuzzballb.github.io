SUPER SKIDDERS — MAKE YOUR OWN CAR
==================================

Each custom car is a folder inside resources/cars/ containing three files:

    resources/cars/mycar/car.obj    the 3D body (Wavefront OBJ)
    resources/cars/mycar/car.mtl    the colors (referenced by the .obj)
    resources/cars/mycar/car.txt    name, stats and wheel settings

Then add the folder name to resources/cars/cars.txt (one name per line):

    example
    mycar

The "example" folder is a complete working template — copy it and start
editing. Cars that fail to load are skipped with a message in the browser
console (F12); the game always keeps running with the built-in cars.


THE MODEL (car.obj + car.mtl)
-----------------------------
* Model the BODY ONLY — no wheels. The game attaches its own animated
  wheels at the positions you set in car.txt.
* Orientation: nose toward -Z, +Y up (Blender's default OBJ export).
* Size doesn't matter: the model is automatically centered, dropped to
  the ground and scaled to standard car length. Use "scale:" in car.txt
  to make it a bit bigger or smaller.
* KEEP IT LOW-POLY. Hard limit: 500 triangles (the car is rejected
  above that). The built-in cars are ~35 faces — aim for under 100 to
  match the game's chunky look and keep it fast.
* Colors come from materials: assign a material per area and set its
  base/diffuse color. Only the color is used — no textures, no
  transparency, no shininess. The game shades each face with its own
  lighting. Faces with no material come out gray.

BLENDER EXPORT
  1. Model the car facing -Y in Blender (so the default export faces -Z).
  2. To stay low-poly: enable the Statistics overlay (viewport overlays
     menu) to watch the triangle count, and use the Decimate modifier to
     reduce a too-dense mesh.
  3. File > Export > Wavefront (.obj) with the default axes
     (Forward = -Z, Up = Y) and check:
        [x] Triangulate Faces   (optional, the game also accepts quads)
        [x] Export Materials
  4. Rename the exported files to car.obj and car.mtl.

BLOCKBENCH (easier alternative)
  Blockbench is a free low-poly editor that fits this art style well:
  https://www.blockbench.net — build with cubes, paint with flat colors,
  then File > Export > Export OBJ Model. Rename to car.obj / car.mtl.


THE PROPERTIES (car.txt)
------------------------
Simple "key: value" lines, # starts a comment. All keys are optional;
out-of-range values are clamped. See example/car.txt for a documented
template.

    name: ROADRUNNER    shown in the car-select screen (max 14 chars)
    desc: Fast but slippery
    cylinders: 6        engine sound: 2, 4, 6 or 8

    maxSpeed: 26        20 - 29
    accel: 19           13 - 26
    brake: 30           20 - 40
    turnSpeed: 2.5      1.8 - 3.2
    grip: 8             5 - 15

    scale: 1.0          extra size multiplier (0.4 - 2.5)
    wheelRadius: 6      3 - 10
    wheelFront: 0.65    front axle position (0 = center, ~1 = nose)
    wheelBack: -0.6     rear axle position (negative = behind center)
    wheelSide: 0.45     sideways wheel offset
    ride: 0.45          body height above the wheels


TESTING
-------
Easiest way: DRAG & DROP your car files onto the game window (the
car-select screen has a hint for this). The car is added straight to
the carousel and remembered by the browser for next time. Dropping
the same car again replaces it, so you can iterate on your model.
This works no matter how the game was opened — with one catch: if you
opened index.html straight from disk (file:// in the address bar),
Chrome cannot look inside a dropped FOLDER, so open the folder and
drop the 3 files themselves. On a server (http) dropping the whole
folder works too.

The cars.txt manifest only loads over http — it will NOT load if you
open index.html straight from disk (file://), because browsers block
fetch() there. For manifest testing, run a small local server:

    cd <game folder>
    python3 -m http.server 8000

then open http://localhost:8000/ — your car appears at the end of the
car-select carousel. Check the browser console (F12) for load errors.
