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
Open the original model file you're basing off of. Original models usually look like this:

```json
{
  "model": {
    "type": "minecraft:model",
    "model": "minecraft:item/iron_nugget"
  }
}
```

After 1.21.4, custom model data has gotten a lot more flexible, but also requires more things to be added. 

So first, change the `"type": "minecraft:model"` to be ```"type": "minecraft:select"```, and remove the next line. It should look like this now:
'`minecraft:select`' is the setup that allows us to use strings to declare the name of the item, if you want to use numbers that's a different setup, which I don't want to teach here because strings are a lot more straightforward.

```json
{
  "model": {
    "type": "minecraft:select",
  }
}
```

Now, add on the next line - ```"property": "minecraft:custom_model_data",```

```json
{
  "model": {
    "type": "minecraft:select",
    "property": "minecraft:custom_model_data",
  }
}
```

then you add the 'fallback' texture, so basically the texture this used to point to. In this case, iron nugget. Add `"fallback": {}` and then basically the original contents of the file into the brackets

```json
{
  "model": {
    "type": "minecraft:select",
    "property": "minecraft:custom_model_data",
    "fallback": {
      "type": "minecraft:model",
      "model": "minecraft:item/iron_nugget"
    }
  }
}
```

Now for the actual declaration of the custom data, we need to add '`"cases":[{}]`' for the different cases that will be shown. i.e. the different models that will be shown.

```json
{
  "model": {
    "type": "minecraft:select",
    "property": "minecraft:custom_model_data",
    "fallback": {
      "type": "minecraft:model",
      "model": "minecraft:item/iron_nugget"
    },
    "cases": [
      {
      }
    ]
  }
}
```

Now inside the `{}` we're gonna add `"when": <name>` - this will be the name of the custom data, it'll be what's used in the command.

```json
{
  "model": {
    "type": "minecraft:select",
    "property": "minecraft:custom_model_data",
    "fallback": {
      "type": "minecraft:model",
      "model": "minecraft:item/iron_nugget"
    },
    "cases": [
      {
        "when": "uno_reverse"
      }
    ]
  }
}
```

Then we add the model information for where the model is. So it should look like this, after all that.

```json
{
  "model": {
    "type": "minecraft:select",
    "property": "minecraft:custom_model_data",
    "fallback": {
      "type": "minecraft:model",
      "model": "minecraft:item/iron_nugget"
    },
    "cases": [
      {
        "when" : "uno_reverse",
        "model" : {
          "type": "minecraft:model",
          "model": "custom_model_data_guide_assets:item/uno_reverse_model"
        }
      }
    ]
  }
}
```

This code tells the game which model file to look for. The model file should be in the `assets/<declared namespace>/models/<declared path>` folder. 
<br>For example, if you have a model in `models -> weapons -> swords -> basic -> iron -> noob_sword.json`, it would look like `"model": "your_namespace:weapons/swords/basic/iron/noob_sword"`. 
<br>You should also name your custom model data with a unique prefix number that makes sense to you to avoid conflicts with other custom model data packs.

## New Model
Then, create a model file in the folder you declared in the previous step. 
<br>This is the json file that tells the game where to place and render the textures. In this file, you are telling the game which texture file(s) to look for. 
<br>You can create your own folder for this, and this will likely make it easier to find things if you are making a large pack. 
<br>If making your own folder, you will put it under the `assets` directory. For this demonstration I will be creating a directory called `custom_model_data_guide_assets` to put the models and textures into
<br>
<br>This will be the model that you exported from blockbench/another software if you used one - or if you're just using a hand-made texture, you can make your own. I will be making my own for this demonstration - it looks like this:

```json
{
  "parent": "minecraft:item/generated",
  "textures": {
    "layer0": "custom_model_data_guide_assets:item/uno_reverse_texture"
  }
}
```
This must be named and in the location that was declared in the override from earlier. So for this demonstration, this file must go into `custom_model_data_guide_assets/models/item` and then be named `uno_reverse_model.json`

## Textures
The texture file(s) should be in the `assets/<declared namespace>/textures/<declared path>` folder. You can use assets that are already in the game or your own textures.
<br>If you export a blockbench model file, you might need to manually adjust the texture file assignments in the case that it was not getting the files from the same place that you are getting them in the pack.
<br>For this demonstration, the file must be in the `custom_model_data_guide_assets/textures/item` folder, and be named `uno_reverse_texture`

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


