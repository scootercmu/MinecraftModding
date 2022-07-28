# Ore Generation
Ore generation is a relatively simple process to create. Its going to require us to create a new directory and a new java class. Afterwards you'll be able to see your own custom ores generate throughout your world.

### Files We Will Be Making
* WorldMod.java

## WorldMod.java
First we're going to have to create a new directory in **java>com>idtech** folder named **world**. And now inside of this folder create a new Java class titled **WorldMod.java**; your file should look like this:
```java
package com.idtech.world;
public class WorldMod {
}
```
Inside of this we will now define a ConfiguredFeature variable and a PlacedFeature variable which will be integral in our generation placing the blocks.
*ConfiguredFeature*
```java
public static ConfiguredFeature<?, ?> GEL_ORE_FEATURE = new ConfiguredFeature(  
        Feature.ORE, new OreConfiguration(  
        OreFeatures.STONE_ORE_REPLACEABLES,  
		BlockMod.GEL_ORE.defaultBlockState(),  
        12));
```
*PlacedFeature*
```java
public static PlacedFeature GEL_ORE_PLACED_FEATURE = GEL_ORE_FEATURE.placed(  
        List.of(  
                CountPlacement.of(25),  
			    InSquarePlacement.spread(),  
		        HeightRangePlacement.uniform(VerticalAnchor.absolute(-80), VerticalAnchor.absolute(80)),  
			    BiomeFilter.biome()  
        ));
```
Now we will move on to creating the lone function of our **WorldMod.java**, the **addFeatures( )** method. Go ahead and place the code below into your **WorldMod** class.
```java
@SubscribeEvent(priority = EventPriority.HIGHEST)  
public static void addFeatures(BiomeLoadingEvent event) {  
    Biome.BiomeCategory biomeCategory = event.getCategory();  
    BiomeGenerationSettingsBuilder biomeGen = event.getGeneration();  
    MobSpawnSettingsBuilder builder = event.getSpawns();    
  
    FeatureUtils.register("gelorefeature", GEL_ORE_FEATURE);  
    PlacementUtils.register("gelorefeature", GEL_ORE_PLACED_FEATURE);  
  
    biomeGen.addFeature(GenerationStep.Decoration.UNDERGROUND_ORES, GEL_ORE_PLACED_FEATURE);  
}
```
This will creat a variable of the biome category, the biome generator, and mob spawning builder in order to read the biome and know what to generate. We will expand upon this at a later time. For now we will just generate our ore in every biome with this part...
```java
	FeatureUtils.register("gelorefeature", GEL_ORE_FEATURE);  
    PlacementUtils.register("gelorefeature", GEL_ORE_PLACED_FEATURE);  
  
    biomeGen.addFeature(GenerationStep.Decoration.UNDERGROUND_ORES, GEL_ORE_PLACED_FEATURE); 
```
...of the code above. Here we register our ore features, and then using our biome generator we add our feature in the biome, generating our ore throughout.
### Subscribe Events
But we are not fully done yet because Minecraft does not know to run our class. To do so we need to add two lines to our code.
First, we need to place the following line above our class header like so:
```java
@Mod.EventBusSubscriber(modid = BaseMod.MODID)
public class WorldMod {
```
This creates an Event Bus linked to our BaseMod file which runs on bootup.
Then above out **addFeatures( )** method we need to add a similar line.
```java
@SubscribeEvent(priority = EventPriority.HIGHEST)  
public static void addFeatures(BiomeLoadingEvent event) {
```
This gives our addFeatures the highest priority when generating biomes and ensures it runs our method.

## Fin
Now we are done and you should be able to boot up a new world and find your ore generated all over your world. If you want to fiddle around with the generation rate simply change the number in ```CountPlacement.of(25)``` within our PlacedFeature variable. And you can change the vein size by changing the number at the end of your ConfiguredFeature variable.
____
> Written with [StackEdit](https://stackedit.io/).
