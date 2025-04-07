# VoxelShapes for Beta 1.7.3
Fully adds the ability to have multiple outline/collision shapes in one block! Completely compatible with server and client, and modifies Vanilla blocks.

# Base Feature
Fixes stairs and fences collision shape to go to individual smaller boxes rather than the whole block taking up the space.
![Demo Image](https://i.imgur.com/yCJNseu.png)

# API

You can utilize this mod in your mod by giving your blocks multiple boxes!  
`HasVoxelShape` will automatically affect collision, if you want a different collision also implement `HasCollisionVoxelShape`.  
You can also implement `HasCollisionVoxelShape` by itself, to only give collision multiple boxes.  

Include in your build.gradle
```
dependencies {
    implementation("com.github.matthewperiut:voxelshapes:${project.voxelshapes_version}") { transitive false }
}
```

```java
// Demo of a block made of two vertical half slabs
class ExampleBlock extends Block implements HasVoxelShape, HasCollisionVoxelShape {
    // Not required but more efficient to
    // store this here than create it
    // each time getVoxelShape is called.
    // preCache() is also optional but recommended when possible.
    private static final VoxelData VOXEL_DATA = new VoxelData(
            Box.create(0, 0, 0, 0.5, 1.0, 1.0), 
            Box.create(0.5, 0, 0, 1.0, 1.0, 1.0)
    ).preCache();
    
    public ExampleBlock(int id, Material material) {
        super(id, material);
    }

    @Override
    public @Nullable VoxelShape getVoxelShape(World world, int x, int y, int z) {
        // You could check the blockstate/meta here with world to change based off the block
        return voxelData.withOffset(x, y, z);
    }

    public @Nullable VoxelShape getCollisionVoxelShape(World world, int x, int y, int z) {
        // Example of giving this block no collision
        return null;
    }
}
```
