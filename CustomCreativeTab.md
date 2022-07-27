# Custom Creative Mode Tab

This is a quick tutorial on how to create your own creative mode tab to hold your mods. Overall it is a very brief process to setup.

### Files We Will Be Making
* ModTab.java

## Creating ModTab.java
To begin navigate to the **java>com.idtech** folder and create a file titled **ModTab.java**.

This file ``` extends CreativeModeTab``` which will need to be put after the class declaration ```public class ModTab```.

Now lets create our class constructor titled **ModTab( )** and put it into the class.

```java
public ModTab(String label){  
    super(label);  
}
```

Our next step is to create an Instance of our class. We will put this above our constructor and feel free to change the name inside the quotations from *"examplemod"* to anything with registry name convention.

```java
public static CreativeModeTab INSTANCE = new ModTab("examplemod");
```

Finally, to wrap up our **ModTab.java** we will be creating the **makeIcon( )** method for our class using the code below.

```java
@Override  
public ItemStack makeIcon() {  
    return new ItemStack(ItemMod.STRUCTURE_GEL);  
}
```

The code right now is using the provided Structure Gel item as the icon. You can go ahead an use any of your items or blocks for this by calling the instance of that class from either your **ItemMod.java** or specific class (ex. TeleportRodItem.INSTANCE, GelPickaxeItem.INSTANCE)

With that our **ModTab.java** file is complete.

## Changing Tab Properties
Now that we have our creative mode tab defined we need to put some items in it. This has to do with changing the properties of the item.

Inside **ItemMod.java** doing this will look like below when you create the new item.
```java
public static final Item STRUCTURE_GEL = ItemUtils.buildBasicItem("structuregel", ModTab.INSTANCE);
```

In a custom item class this will be done in the line where you define the item or block's properties.
```java
private static Properties properties = new Properties().tab(ModTab.INSTANCE).food(foodproperties);
```

## Changing the Tab Name
Last but not least we will change the name of the tab. Navigate to the **resources>assets>examplemod>lang** folder and open your **en_us.json** file and add the line below.

```json
"itemGroup.examplemod": "Example Mod"
```

The *examplemod* will need to correspond to the name inside your ModTab Instance.

## Fin
And with that your custom creative mode tab is complete and will show up in game.

> Written with [StackEdit](https://stackedit.io/).
