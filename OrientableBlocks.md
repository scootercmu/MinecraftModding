# Orientable Block Faces
Creating an orientable block will begin by creating a class for your block (i.e. **CustomBlock.java**) as directed by GamePlan. Then we will have to create a couple JSON files defining the blockstates and models of our block. This guide will be building a block called **ArrowBlock** with a registry name of **arrowblock**, but you can use your own block by just changing out the names.

### Files We Will Be Making
* ArrowBlock.java
* **resources**>**blockstates**> arrowblock.json
* **resources**>**models**> arrowblock.json
* (Optional) **resources**>**models**> arrowblock_vertical.json

## ArrowBlock.java
To begin follow the GamePlan until you have variables named **INSTANCE**, **ITEM**, and **properties** and have created a constructor for your class. It should look like this:
```java
public class ArrowBlock extends Block {  
  
 private static Properties properties = Properties.of(Material.STONE).strength(3.0f).requiresCorrectToolForDrops();  
 public static Block INSTANCE = new MossBlock(properties).setRegistryName(BaseMod.MODID, "arrowblock");  
 public static Item ITEM = BlockUtils.createBlockItem(INSTANCE, CreativeModeTab.TAB_MISC);  
  
 public ArrowBlock(Properties properties){  
        super(properties); 
  }
```
Next we will be adding the methods and variables to handle the direction our block is facing and orient it. First we will create a new variable at the top of data type **DirectionProperty** titled **FACING**.
```java
public static final DirectionProperty FACING = DirectionalBlock.FACING;
```
Inside our constructor below the **super( )** call we are going to add a default state for our block setting the direction to North.
```java
this.registerDefaultState(this.stateDefinition.any().setValue(FACING, Direction.NORTH));
```
To polish off our class we will add a couple methods to handle getting our block state during placement and creating our block state definition. So add these two functions below your constructor.

```java
public BlockState getStateForPlacement(BlockPlaceContext p_52669_) {  
    return this.defaultBlockState().setValue(FACING, p_52669_.getNearestLookingDirection().getOpposite());  
}  
  
protected void createBlockStateDefinition(StateDefinition.Builder<Block, BlockState> p_52719_) {  
    p_52719_.add(FACING);  
}
```
And with that our block class is complete and we can move on to defining our block states and models.

## Defining Blockstate and Model JSONs
Now that we have our Java file created we will have to set up the block states to handle each direction of our block and the models texturing them.

### Blockstates JSON

1. To begin lets navigate through our files to **resources>assets.examplemod>blockstates**. Inside the blockstates folder you should see a file titled **castlewall.json**. 
2. Right-click, copy, and then paste the copy inside the **blockstates** folder and name the file **arrowblock.json**.
3. Delete the contents of the file and paste the text below into the file.
```json
{  
	"variants": {  
        "facing=down": {  
	        "model": "examplemod:block/arrowblock"  
	    },  
	    "facing=east": {  
            "model": "examplemod:block/arrowblock",  
		    "y": 90  
        },  
        "facing=north": {  
            "model": "examplemod:block/arrowblock"  
        },  
        "facing=south": {  
            "model": "examplemod:block/arrowblock",  
            "y": 180  
        },  
        "facing=up": {  
	        "model": "examplemod:block/arrowblock"  
	    },  
	    "facing=west": {  
		    "model": "examplemod:block/arrowblock",  
			"y": 270  
		}  
	}  
}
```
This JSON auto *up* and *down* placements to point north, but there will be an option to create a model JSON for a vertical model of the block. If you choose to do so you would switch the ```facing=down``` and ```facing=up``` to the following:
```json
"facing=down": {  
  "model": "examplemod:block/arrowblock_vertical",  
  "x": 180  
},
"facing=up": {  
  "model": "examplemod:block/arrowblock_vertical"  
}
```

### Model JSONs
Normally we haven't had to make these JSONs for blocks thanks to iDtech's scripts that we have been using to generate them. But since we are creating a crop there are factors that the script does not account for.

1. To begin you will want to navigate to the **resources>assets.examplemod>models>block** folder.
2. And just like with the blockstate JSON find the **castlewall.json**, copy it, and  paste it with the name **arrowblock.json**.
3. Now that the file has been created delete the old contents of it and paste the following code.
```json
{  
  "parent": "minecraft:block/orientable",  
  "textures": {  
	  "top": "examplemod:block/arrowblock_top",  
	  "front": "examplemod:block/arrowblock_front",  
	  "side": "examplemod:block/arrowblock",
	  "bottom": "examplemod:block/arrowblock_bottom"
  }  
}
```
These are not the only faces you can use for your model. If you do not want a front you can remove that line and all the sides will be the same. Similarly you can remove the bottom line and the bottom of the block will take the same texture as the top.

The name in ```"examplemod:block/arrowblock"``` must match with the file name of the texture you want. 

**Vertical Model** - If you want to make a vertical model you can use the same code as above using different textures for each value.

## Fin
And with that we have completed our orientable block and you can test it out in game. Feel free to play around with the models to find new possibilities, the [Minecraft Fandom Wiki on Models](https://minecraft.fandom.com/wiki/Model#Block_models) can be useful to explore all the possibilities.
____
> Written with [StackEdit](https://stackedit.io/).
