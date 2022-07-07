# Custom Crops

As we create a custom crop we will define several new files supporting the block, the items, and the growth stages of the crop. All usages of CustomFood in file names and such can be exchanged for your plant name (i.e. CucumberItem.java or CornPlantBlock.java).


### Files We Will Be Making
* CustomFood.java
* CustomFoodPlantBlock.java
* CustomFoodSeedItem.java
* **resources**>**blockstates**> customfoodplant.json
* **resources**>**models**>**block**> customfood_stage0.json    (Will have one for each stage)
* **resources**>**data**>**examplemod**>**loot_tables.blocks**> customfoodplant.json

## Creating CustomFood.java
To begin navigate to the **java>com.idtech>item** folder and create a file titled **CustomFood.java**.

This file will ``` extend Item ``` which will need to be put after the class declaration ```public class CustomFood```.

Now lets create our constructor and put it into our class.
```java
public CustomFood(Properties properties){  
    super(properties);  
}
```

Next we will create the properties of our food item, this requires us to create a FoodProperties object and a Properties object. Place these lines above the constructor.
```java
public static FoodProperties foodproperties = (new FoodProperties.Builder().nutrition(5).saturationMod(1.4f).effect(new MobEffectInstance(MobEffects.JUMP, 500, 1), 1.0f).alwaysEat().build());  
private static Properties properties = new Properties().tab(CreativeModeTab.TAB_MISC).food(foodproperties);
```
Feel free to change around the values inside the parentheses following *nutrition, saturationMod, or MobEffectInstance* to change what your food does. There are also other FoodProperties you can look into using.

Wrapping up inside **CustomFood.java** we will create an Instance of our food Item by placing the following below our properties and above our constructor.
```java
public static Item INSTANCE = new TeleportApple(properties).setRegistryName("teleportapple");
```

#### Challenge: **Custom Effects**
With your food created as its own class we can add extra effects like summoning a mob, placing a block, or teleporting the player into the sky. For simplicities sake we will be doing the last one, but the sky is the limit when it comes to what you can code into here.

We will start by defining a new method below our constructor titled **finishUsingItem( )**. Define it like so:
```java
public ItemStack finishUsingItem(ItemStack itemStack, Level level, LivingEntity livingEntity) {

}
```
Now to fill in the inside of our method with code to teleport our player.
1. To start we will have to get our players current location from livingEntity, which we will store in a BlockPos variable. 
```java
BlockPos blockpos = livingEntity.getOnPos();
```
2. Next we will make our call to teleport the player getting the X, Y, and Z coordinates from our BlockPos variable. We will add 50 to the player's Y coordinate to send them 50 blocks up.
```java
livingEntity.teleportTo(blockpos.getX(), blockpos.getY()+50, blockpos.getZ());
```
3. Finally we will have our return statement and actually eat our food item.
```java
return livingEntity.eat(level, itemStack);
```

And with these four lines of code we have added our own custom effect to our food. To truly make some truly creative uses of this method you will need a decent understanding of coding and Java, but even without it you can still make plenty of effects.

## Creating CustomFoodPlantBlock.java
To begin go ahead and create a new file in the **java>com.idtech>block** folder titled **CustomFoodPlantBlock.java**.

This file will ```extend CropBlock``` so add that after the ```public class CustomFoodPlantBlock```.

Next we will put in the constructor into the file, so place it inside the curly brackets.
```java
public  CustomFoodPlantBlock(Properties  properties){ // CustomFoodPlantBlock Constructor
	super(properties);
}
```
Above the constructor is where our various variables will go.
1. First variables we will place have to do with managing the plant age. The value of MAX_AGE and AGE can be changed to reflect how many stages of growth you want (Potatoes and Beetroot are 3, Wheat is 5)
```java
public  static  final  int  MAX_AGE = 3;
public  static  final  IntegerProperty  AGE = BlockStateProperties.AGE_3;
private  static  final  VoxelShape[] SHAPE_BY_AGE = new  VoxelShape[]{Block.box(0.0D, 0.0D, 0.0D, 16.0D, 2.0D, 16.0D), Block.box(0.0D, 0.0D, 0.0D, 16.0D, 4.0D, 16.0D), Block.box(0.0D, 0.0D, 0.0D, 16.0D, 6.0D, 16.0D), Block.box(0.0D, 0.0D, 0.0D, 16.0D, 8.0D, 16.0D)};
```
2. Below the age variables but above the constructor, create a Properties object. The line below uses the properties of Beetroots other options are available as well.
```java
private  static  Properties  properties = BlockBehaviour.Properties.copy(Blocks.BEETROOTS).noCollission();
```
3. Last we will create an Instance of our Block below the properties.
```java
public  static  Block  INSTANCE = new  CustomFoodPlantBlock(properties).setRegistryName(BaseMod.MODID, "customfoodplant");
```

Now with the variables made we will put our methods into the file. First we will create our **getBaseSeedId( )** which will be what ties our plant to its respective seed item. 

Below the constructor go ahead and place the **getBaseSeedId( )** method.
```java
protected  ItemLike  getBaseSeedId(){
	return  CustomFoodSeedsItem.INSTANCE;
}
```

Last but not least we will place the rest of the methods into the file below the **getBaseSeedId( )**. 
```java
public  IntegerProperty  getAgeProperty() {
	return  AGE;
}

public  int  getMaxAge() {
	return  MAX_AGE;
}
public  void  randomTick(BlockState  p_49667_, ServerLevel  p_49668_, BlockPos  p_49669_, Random  p_49670_) {
	if (p_49670_.nextInt(3) != 0) {
		super.randomTick(p_49667_, p_49668_, p_49669_, p_49670_);
	}
}
 
protected  int  getBonemealAgeIncrease(Level  p_49663_) {
	return  super.getBonemealAgeIncrease(p_49663_) / 3;
}

protected  void  createBlockStateDefinition(StateDefinition.Builder<Block, BlockState> p_49665_) {
	p_49665_.add(AGE);
}

public  VoxelShape  getShape(BlockState  p_49672_, BlockGetter  p_49673_, BlockPos  p_49674_, CollisionContext  p_49675_) {
	return  SHAPE_BY_AGE[p_49672_.getValue(this.getAgeProperty())];
}
```
Finally we will register our block in the **registerBlocks( )** method in our BlockMod.java file using the line below.
```java
event.getRegistry().register(CustomFoodPlantBlock.INSTANCE);
```

## Creating CustomFoodSeedsItem.java

Now we will move over to our **java>com.idtech>item** folder and create a java class titled **CustomFoodSeedsItem.java**. 

This file will ```extend CropBlock``` so add that after the ```public class CustomFoodPlantBlock```

Next we will put in the constructor into the file, so place it inside the curly brackets.
```java
public CustomFoodSeedsItem(Properties properties){  
    super(CustomFoodPlantBlock.INSTANCE, properties);  
}
```
Above the constructor lets add a Properties object followed by an instance of our item.
```java
private static Properties properties = new Properties().tab(CreativeModeTab.TAB_MISC);
```
```java
public static Item INSTANCE = new CustomFoodSeedsItem(properties).setRegistryName(BaseMod.MODID, "customfoodseeds");
```

Finally, we will register our seeds item in the ItemMod.java file in the **registerItems( )** method with the following line.
```java
event.getRegistry().register(CustomFoodSeedsItem.INSTANCE);
```

## Defining Blockstate, Model, and Loot Table JSONs
Now that we have our main Java files created we have to set up the stages of growth for our plant along with the models and textures. We also need to make it so our plant drops our Custom Food when broken at its max age. So to start we are going to define the block states.

### Blockstates JSON
The blockstate JSON will be what informs the game of different stages of our custom crop.

1. To begin lets navigate through our files to **resources>assets.examplemod>blockstates**. Inside the blockstates folder you should see a file titled **castlewall.json**. 
2. Right-click, copy, and then paste the copy inside the **blockstates** folder and name the file **customfoodplant.json**.
3. Delete the contents of the file and paste the text below into the file.
```json
{  
   "variants": {  
      "age=0": {"model": "examplemod:block/customfood_stage0"},  
	  "age=1": {"model": "examplemod:block/customfood_stage1"},  
	  "age=2": {"model": "examplemod:block/customfood_stage2"},  
	  "age=3": {"model": "examplemod:block/cusutomfood_stage3"}  
   }  
}
```
The number of lines you need in this file will depend on the MAX_AGE you set in CustomFoodPlantBlock.java. The values above are for a plant with a max age of 3.

### Model JSONs
Normally we haven't had to make these JSONs for blocks thanks to iDtech's scripts that we have been using to generate them. But since we are creating a crop there are factors that the script does not account for.

1. To begin you will want to navigate to the **resources>assets.examplemod>models>block** folder.
2. And just like with the blockstate JSON find the **castlewall.json**, copy it, and  paste it with the name **customfood_stage0.json**.
3. Now that the file has been created delete the old contents of it and paste the following code.
```json
{  
	"parent": "minecraft:block/crop",  
	"textures": {  
		"crop": "examplemod:block/customfood_stage0"  
	}  
}
```
4. Repeat steps two and three making copies of your **customfood_stage0.json** for each of the stages of your plant. 

**Note**: Your texture files for each stage will need to be named in the convention of```customfood_stage0``` for each one.


## Loot Table JSON

Lastly we need to define the drops for destroying our crop at different stages.
1. You will want to navigate to **resources>data>examplemod>loot_tables.blocks** folder.
2. Again you will make a copy of **castlewall.json** and this one will be named **customfoodplant.json**.
3. Lastly replace the contents of the JSON file with the following code.
```json
{ 
  "type": "minecraft:block",  
  "pools": [  
    {  
	    "rolls": 1.0,  
		"bonus_rolls": 0.0,  
	    "entries": [  
          {  
	          "type": "minecraft:alternatives",  
			  "children": [  
	            {  
	              "type": "minecraft:item",  
				  "conditions": [  
	                {  
	                  "condition": "minecraft:block_state_property",  
					  "block": "examplemod:customfoodplant",  
					  "properties": {  
	                    "age": "3"  
					  }  
	                }  
	              ],  
				  "name": "examplemod:customfood"  
				  },  
				  {  
		              "type": "minecraft:item",  
					  "name": "examplemod:customfoodseeds"  
				  }  
	          ]  
	        }  
	      ]  
	    },  
	  {  
	      "rolls": 1.0,  
		  "bonus_rolls": 0.0,  
		  "entries": [  
	        {  
	          "type": "minecraft:item",  
			  "functions": [  
	            {  
	              "function": "minecraft:apply_bonus",  
				  "enchantment": "minecraft:fortune",  
				  "formula": "minecraft:binomial_with_bonus_count",  
				  "parameters": {  
	                "extra": 3,  
					"probability": 0.5714286  
				  }  
	            }  
	          ],  
			  "name": "examplemod:customfoodseeds"  
		    }  
	      ],  
		  "conditions": [  
	        {  
	          "condition": "minecraft:block_state_property",  
			  "block": "examplemod:customfoodplant",  
			  "properties": {  
	            "age": "3"  
			  }  
	        }  
	      ]  
	    }  
	  ],  
	  "functions": [  
	    {  
	      "function": "minecraft:explosion_decay"  
		}  
	  ]  
	}
```
Now all of the files we have to create are made! You can go ahead and runClient to check out your crop, but you might notice something is off...

The crop texture is a solid color! To fix this we will need to inform Minecraft that we want to render transparent blocks which we will need to do in **java>com.idtech>BaseMod.java**.

### Render Transparent Textures
Informing Minecraft of our render only takes a few lines of code, the first we will be putting in our constructor.
```java
FMLJavaModLoadingContext.get().getModEventBus().addListener(this::clientSetup);
```
We will place this after the similar lines that should be around line 60 in **BaseMod.java**

And finally we will add a method titled **clientSetup( )** which we will place below the method **setup( )** and above the method **enqueueIMC( )**.
```java
private void clientSetup (final FMLClientSetupEvent event) {  
	
}
```
Lastly, we will have to set the render layers for our CustomFoodPlantBlock by putting this line inside of the **clientSetup( )** method.
```java
ItemBlockRenderTypes.setRenderLayer(CustomFoodPlantBlock.INSTANCE, RenderType.cutout());
```
If you have multiple different block that need to be transparent you will just include more of these calls but replacing the *CustomFoodPlantBlock.INSTANCE* with the other block names.

### Fin
And with that your custom crop is complete and should be rendered properly!
