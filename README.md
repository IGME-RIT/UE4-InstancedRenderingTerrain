<h1> Instance Rendering of Tile-Based Terrain in UE4 </h1>

<h2>Overview</h2>
<p>Whether you're making a tactical turn-based rpg, an RTS, or a farming simulator - you'll probably come across a situation in which you want to create a game map out of tiles or other tile-like objects.</p>

<p>Now the most basic implementation would be simply to spawn a tile prefab/blueprint in a grid, with each tile being a separate object. This implementation is easy to understand, and allows us to access and change any of the tiles during run time, but it's also EXPENSIVE. 

<p>In the included example, just a 20x20 grid of simple tiles alone starts to make a noticable dent in the framerate - and that's just displaying the terrain; we haven't even begun to implement all the other odds and ends that make the game. Obviously we're going to need a much more efficient solution if we want to have a map larger than someone's backyard</p>

<p>That's where instanced rendering comes in. We can essentially instantiate only one instance of a mesh and "copy and paste" it to create the rest of the terrain. Using this method you could easily turn just a few terrain types (like river, dirt, grass, mountian, etc) into a map consisting of thousands of tiles with essentially zero impact to your performance. For this example we'll keep things simple and just work with a map of grass. In the included unreal project you can hit 'O' on the keyboard in order to spawn the map of indavidual tiles, or 'P' for the instanced tiles (you will need to stop and restart the scene each time).</p>

<h2>Implementation Specifics: Explicit Instantiation Option</h2>
<p>As noted above, the most direct option is to simply make a grid of separate actors. This implementation is easy to code, easy to understand, easy to edit at runtime, and will destroy your FPS.</p>
<p>First thing we need to do after downloading and opening the project is to enable the FPS display. In your viewport hit the dropdown in the top left then go stat -> engine and check FPS and Unit. This will allow us to better see performance changes</p>
<p>If you open up the level blueprint (Blueprints -> Open Level Blueprint) you can see two sections of script - the top one is what we'll be looking at in this section. The variables 'rowCount' and 'columnCount' can be edited to change the size of the grid to spawn. I reccomend not going too far over 100 each unless you have some kind of NASA supercomputer, the default is 20x20. Feel free to play around with the numbers and see their performance impact.</p>
<p>As far as implementation goes, a nested for loop adds a static mesh for each index in the 2D grid given by the size constraints, that's really about it</p>

<h2>Implementation Specifics: Instanced Rendering Option</h2>
<p>If we want a decent sized map that doesn't tank our performance, one of our best options is instanced rendering
<p>If you haven't already enabled the FPS display, in your viewport hit the dropdown in the top left then go stat -> engine and check FPS and Unit. This way we can see just how little of an impact this option will have.
<p>This implementation begins by making a root instanced mesh component prior to the for loops. The for loops than add an instance of the mesh component where the previous implementation would have made an entirely new and spearate mesh. The size of the grid here can be changes with 'rowCountInstance' and 'columnCountInstance'. The default is 200x200 but you can go up to just shy of 500x500 before a crash will occur. The crash occurs above this number as Unreal flags the loops as un-ending due to the sheer number of passes required to make all of those instances. Note how even at huge grid sizes there's little to no impact on performance (although large grids may have a small lag time before popping in initially).</p>
