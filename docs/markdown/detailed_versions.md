# Detailed versions

>Each version is detailed in this file to give an idea of the main
features every update will cover.

### v0.0
This is the beginning, it marks down the moment I (Grieljis) feel
ready enough to start coding instead of just writing my ideas.

### v0.1
The goal is to setup a window with buttons you can click on to escape
the so window.

### v0.2
With the help of Perlin Noise, the goal is to generate a world
with continents, oceans and islands you can see thanks to a graphical
interface, and where each square (made of severeal pixels) represents a map you
will play on. The world will be 3D, on a sphere so there will be a first implementation
on a 2D surface called alpha, before I try putting in 3D a 2D Perlin
Noise map + coding a 3D graphical interface.

### v0.3
The map will be composed of GroundTile, each containing an OnTile (for
example OnTile:Block which represents air, stone or a wall) and a RoofTile
which is needed for example to hold a GroundTile on top. Before generating
a map, it is needed to create such classes the map will use and that is
the goal of this version.

### v0.4
According to data like temperature, precipitation and elevation from the
selectionned square in the world, a map will be generated in 3D with
GroundTile, OnTile:Block and RoofTile. Theis will be an alpha, before
coding the graphical interface which will work like in Dwarf Fortress : 2D
view with direction arrows to help navigate in 3D. This graphical interface
is what the user will spend most of his time on, as seeing the map (and 
in the future interacting with features on it) is how one plays the game.
