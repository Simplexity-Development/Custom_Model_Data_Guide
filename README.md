# Preface
Custom model data is a feature in Minecraft that allows you to create your own models for items. 
# Prep
To use custom model data, you need to make sure that all paths are set properly.
<br>This means that the base model file with the CustomModelData declaration that points to a new model file must be in the same spot it would be in the vanilla datapack. (If you are not sure of where these would be, I suggest looking at [mcassets](https://mcasset.cloud) or [mcmeta](https://github.com/misode/mcmeta))
To create a custom model data, you first need to take the original model file and put it into your resource pack. 
<br>This must go into the vanilla namespace. 
<br>Then, create a new folder for your own assets within the assets directory. (for this example I'm going to name it `custom_model_data_guide_assets`.) 
<br>Within this folder, create the initial folders `models`, and `textures`, and an `item` folder within both of those.

## Base Model
Open the original model file you're basing off of (in this demo we're using an iron nugget) and add the following code:

```json
"overrides" : [
{ "predicate": { "custom_model_data": 1 }, "model": "custom_model_data_guide_assets:item/purpur_item" }
]
```
This code tells the game which model file to look for. The model file should be in the `assets/<declared namespace>/models/<declared path>` folder. 
<br>For example, if you have a model in `models -> weapons -> swords -> basic -> iron -> noob_sword.json`, it would look like `"model": "your_namespace:weapons/swords/basic/iron/noob_sword"`. 
<br>You should also name your custom model data with a unique prefix number that makes sense to you to avoid conflicts with other custom model data packs.

## New Model
Then, create a model file in the folder you declared in the previous step. 
<br>This is the json file that tells the game where to place and render the textures. In this file, you are telling the game which texture file(s) to look for. 
<br>The texture file(s) should be in the `assets/<declared namespace>/textures/<declared path>` folder. You can use assets that are already in the game or your own textures.
<br>If you export a blockbench model file, you might need to manually adjust the texture file assignments in the case that it was not getting the files from the same place that you are getting them in the pack.

### Final
You need an mcmeta file or your pack won't work. [misode.github.io](https://misode.github.io/pack-mcmeta/) has a generator, as well as many other very helpful resources for data/resource packs. 
<br>The mcmeta must simply be named `pack.mcmeta`
<br>If you want to include a pack image, it must be named `pack.png`
### Other Notes
It's important to note that if you have multiple custom model data declarations on one item, you must declare them in numerical order or it will break.

### It's not working!
Use your debug option on your launcher, it'll help a ton. It'll tell you exactly what's wrong.

* It's not showing up at all, it just looks normal!
  * Your model data is either declared wrong, or your pack is not loading. 

* My item is just a huge broken box!
  * You declared the model data, but it's pointing to a broken model

* My item looks right, but it's just the wrong/broken texture!
  * Check the texture declaration in your model. Make sure all paths are correct.

* It's showing up with the wrong item!
  * Make sure you didn't either paste the wrong path, or use the wrong number, or accidentally add a zero/extra number somewhere

* This item is supposed to look like x but it's not the right size!
  * Use blockbench to rescale/try using a different parent 

* I made a bow and it only changed the first texture!
  * You will need overrides for all the textures on the bow. I suggest getting someone more qualified to help with complex texture questions


