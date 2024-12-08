---
title: Adding Furniture
parent: Lavender
grand_parent: Guides
has_children: true
---

# Adding Furniture

![Static Badge](https://img.shields.io/badge/Skill_Level-Beginner-blue?style=for-the-badge)  
![Static Badge](https://img.shields.io/badge/Estimated_Time-15_Minutes-blue?style=for-the-badge)

{: .note}
> This guide assumes you've already downloaded Lavender and/or referenced it in your project Dependencies!

## Setting Up Furniture Assets
Lavender offers two options for adding your furniture:

### Option 1: AssetBundles

{: .note}
> If you need a guide on how AssetBundles work, click [here]({% link www/Guides/AssetBundles.md %}) to learn more.

In your AssetBundle, you will need the following three objects:

1. **Sprite:** The image of your object *(used in the Building System or Furniture Shop UIs)*
2. **Preview Prefab:** A furniture object with collision, including components like a kinematic Rigidbody and a trigger-enabled Box Collider.
3. **Prefab:** The furniture object with any additional components you want, such as collision.

### Option 2: FurnitureAssetData.json
```json
{
    "imagePath": "png-or-jpg-path",
    "objPath": "obj-path",
    "texturePaths": [
        "png-or-jpg-texture-path",
        "more-texture-paths-if-needed"
    ]
}
```

- **imagePath:** The path to an image of your object.
- **objPath:** The path to your ``Furniture.obj`` file.
- **texturePaths:** The path (or multiple paths, if needed) to the textures for your ``.obj``.

All paths are relative to this ``.json`` file.

## FurnitureConfig.json
```json
{
	"title": "Name",
	"details":"Description",

	"priceOC": 0,
	"priceRM": 0,

	"assetBundlePath": "assetBundle",
	"imageName":"asset_img",
	"prefabName":"asset_pre",
	"previewPrefabName":"asset_prepre",
	
	"category": "ALL",
	"restrictedAreas": [
	],
	
	"displayStyle": "Default",
        "placeType": "all",
	"displayRotationY": 0
}
```

- **title:** The name of the furniture
- **details:** A short description of the furniture
- **priceOC:** OC price
- **priceRM:** RM price
- **assetBundlePath:** The location of the AssetBundle or FurnitureAssetData.json relative to the .json path
- **imageName:** The name of the sprite in the AssetBundle defined at assetBundlePath *(set its value to ``"imageName": ""`` if using FurnitureAssetData.json)*
- **prefabName:** The name of the GameObject/Prefab in the AssetBundle defined at assetBundlePath *(set its value to ``"prefabName:" ""`` if using FurnitureAssetData.json)*
- **previewPrefabName:** The name of the GameObject/Prefab in the AssetBundle defined at assetBundlePath *(set its value to ``"previewPrefabName": ""`` if using FurnitureAssetData.json)*
- **category:** The category of the Furniture
  - [list of all valid enum values]({% link www/Guides/Lavender/Furniture Config Enums.md %}#furniture-category)
- **restrictedAreas:** Restricted areas to build the furniture -> Allowed to put everywhere if empty
  - [list of all valid enum values]({% link www/Guides/Lavender/Furniture Config Enums.md %}#furniture-building-area)
- **displayStyle:** Should it be positioned like a normal furniture or like a painting or ceiling object?
  - [list of all valid enum values]({% link www/Guides/Lavender/Furniture Config Enums.md %}#furniture-display-style)
- **placeType:** Where (Wall, Floor, etc.) can the furniture be placed?
  - [list of all valid enum values]({% link www/Guides/Lavender/Furniture Config Enums.md %}#furniture-place-type)
- **displayRotationY:** Display rotation on the Y-Achses

## Creating the Furniture Object
After setting up the FurnitureConfig.json and either the AssetBundle or FurnitureAssetData.json, you're ready to dive into your mod code!

```c#
using Lavender.FurnitureLib;

// The path is relativ to Obenseuer.exe
string path = "FurnitureConfig.json";
Furniture? yourFurniture = FurnitureCreator.Create(path);
if(yourFurniture != null) 
{
    // GiveItem() directly gives the Player your Furniture, perfect for testing!
    yourFurniture.GiveItem();
}
```

## Adding Custom Scripts to Your Furniture Object
The Furniture Handler Attribute allows you to add custom scripts to your furniture prefab!

```c#
using Lavender.FurnitureLib;

// This Handler function gets called every time your furniture gets loaded!
[FurnitureHandlerAttribute("YourFurnitureTitel")]
public static Furniture YourFurnitureHandler(Furniture furniture)
{
    // ... -> do something with the fresh loaded furniture like furniture.prefab.AddComponent<T>()

    return furniture;
}
```

Inorder to make the FurnitureHandler and FurnitureShopRestockHandler work, you need to register them:
```c#
using Lavender;

// Put this in your mod Awake() function
Lavender.AddFurnitureHandlers(typeof(the_class_containing_the_FurnitureHandler));
Lavender.AddFurnitureShopRestockHandlers(typeof(the_class_containing_the_FurnitureShopRestockHandler));
```

Speaking of the FurnitureShopRestockHandler...

## Adding Your Furniture to a Furniture Shop
The Furniture Shop Restock Handler Attribute allows you to add your furniture every time a restock event occurs!

```c#
using Lavender.FurnitureLib;

[FurnitureShopRestockHandlerAttribute("AnyID")]
public static List<BuildingSystem.FurnitureInfo> FurShopRestockHandler(FurnitureShopName name)
{
    List<BuildingSystem.FurnitureInfo> restock = new List<BuildingSystem.FurnitureInfo>();

    // The path is relativ to Obenseuer.exe
    string path = "FurnitureConfig.json";
    
    // Idea: Randomize the restock amount to make it more realistic
    int amount = 20;
   
    BuildingSystem.FurnitureInfo? info = FurnitureCreator.CreateShopFurniture(path, amount);
    if(info != null) restock.Add(info);

    return restock;
}
```

**FurnitureShopName Enum Values:**
- None -> Debug enum
- OneStopShop -> One Stop Shop
- MoebelmannFurnitures -> Möbelmann Furnitures
- SamuelJonasson -> Jonasson's Shop