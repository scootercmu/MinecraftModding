# Mob Loot Table
You may find that after you have completed your Mob following the GamePlan that its drops don't properly work. To fix this we will need to add some code to your **ZomboEntity.java** file. 

Inside your **ZomboEntity.java** we will place this line of code above your **TYPE** and **EGG** variable declarations.
```java
public static final ResourceLocation LOOT_TABLE = new ResourceLocation(BaseMod.MODID, "entity/zombo");
```
This will create a reference to the loot table for your mob. And next we will create a method for access this variable titled **getDefaultLootTable( )** that will simply return your variable **LOOT_TABLE**.
```java
public ResourceLocation getDefaultLootTable(){  
    return LOOT_TABLE;  
}
```
With these lines of code our entity will provide its corresponding loot table to Minecraft and drop our items properly.

----
> Written with [StackEdit](https://stackedit.io/).
