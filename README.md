# Different Ways To Do Custom Textures In Game

In Minecraft 1.21.4, new ways of handling custom textures were introduced. This has made it far simpler to add custom
textures to the game.

If this is your first time adding custom textures, the **Item Model** method will likely be enough. If you need specific
textures based on specific cases, **Custom Model Data** will be necessary

If you have previously done custom model data, you no longer need to declare a new item texture as 'connected' to any
specific item in the game at all. You can just add the custom item model and texture and then you can apply that to any
item in the game. "Custom Model Data" now is very useful for changing texture based on specific contexts around a
specific item (such as if it's enchanted with a specific thing, named a certain way, etc) but is no longer necessary for
just changing the texture and model of an item.

# Custom Item

You are going to need 3 things for a custom item:

- Item Declaration
- Model file
- Texture

For the structure of your resource pack, you will have two folders - `<namespace>` is your name you choose for your
pack. It should not have any spaces or capital letters. If you are new to making stuff with paths and namespaces, check
out [Resource Locations In Minecraft](https://minecraft.wiki/w/Resource_location)

- `/assets/<namespace>/items/`
- `/assets/<namespace>/models/item/`
- `/assets/<namespace>/textures/item/`

If you used something like [Block Bench](https://www.blockbench.net/) to make your item, export as a model and export
your texture.

### Item Declaration

With the new system, you will be making an item declaration. This is extremely easy, it just tells the game what type of
model the item is going to be using. For basic item models, it will just look like this:

```json
{
  "model": {
    "type": "minecraft:model",
    "model": "<namespace>:item/<model_name>"
  }
}
```

So for our item declaration, we will have:

```json
{
  "model": {
    "type": "minecraft:model",
    "model": "item_model_guide:item/uno_reverse"
  }
}
```

This file will go into the `assets/<namespace>/items/` directory.

### Model

If you are making a basic item model, it will look something like this:

```json  
{
  "parent": "<parent>",
  "textures": {
    "layer0": "<namespace>:item/<texture>"
  }
}  
```  

`"parent"` refers to the 'parent' (lol) file that will be used as a reference for things like scale, angle, way it's
held and placed in relation to the character, that the model will be rendered in the game.

- `"minecraft:item/generated"` - most items in the game (sticks, potatoes, whatever)
- `"minecraft:item/handheld"` - items that are held by the player and used i.e. tools, weapons
- `"minecraft:item/template_bed"` - beds
  If I missed any item parent models please let me know. There are many block parent models but I will not be covering
  those as there is currently no natively-supported way to add custom blocks into the game.

so for our item we want to put the parent as `"minecraft:item/generated"` - since it will just be an item that is held
normally, no special properties or anything.

```json
{
  "parent": "minecraft:item/generated"
}
```

Then for the texture, we have one texture that is referenced for this item. We will make the `"textures"` group

```json
{
  "parent": "minecraft:item/generated",
  "textures": {
    "layer0": ""
  }
}
```

Then we will add the reference for the texture as `"layer0"` - since I am putting the texture into our namespace, I will
also need to specify that. If you are referencing a default Minecraft texture, you would use the `minecraft` namespace
in this declaration. So if you are using oak planks as a texture in your item's model, you would do
`"minecraft:block/oak_planks"`

```json
{
  "parent": "minecraft:item/generated",
  "textures": {
    "layer0": "item_model_guide:item/uno_reverse"
  }
}
```

Next, make sure you have placed your textures into `assets/<namespace>/textures/item/`
They must be named the way they have been referenced in the model file (outside of the file end i.e. png)

So for this, I must have placed a texture image called `uno_reverse.png` into the folder
`assets/item_model_guide/textures/item/`

If you do not need to change textures based on specific fancy contexts, you can skip to [Final Touches](#final-touches)

# Custom Model Data

If you are wanting to adjust textures based on certain circumstances, you'll wanna use custom model data.
If you are *only* going to be using custom model data, you do not need the item declaration, but having it will allow you to use the item model in other areas.

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

So first, change the `"type": "minecraft:model"` to be ```"type": "minecraft:select"```, and remove the next line. It
should look like this now:  
'`minecraft:select`' is the setup that allows us to choose different textures based on different things in game.

```json  
{
  "model": {
    "type": "minecraft:select"
  }
}  
```  

Now, add on the next line - ```"property": "minecraft:custom_model_data",```

```json  
{
  "model": {
    "type": "minecraft:select",
    "property": "minecraft:custom_model_data"
  }
}  
```  

then you add the 'fallback' texture, so basically the texture this used to point to. In this case, iron nugget. Add
`"fallback": {}` and then basically the original contents of the file into the brackets

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

Now for the actual declaration of the custom data, we need to add '`"cases":[{}]`' for the different cases that will be
shown. i.e. the different models that will be shown.

```json  
{
  "model": {
    "type": "minecraft:select",
    "property": "minecraft:custom_model_data",
    "cases": [
      {}
    ],
    "fallback": {
      "type": "minecraft:model",
      "model": "minecraft:item/iron_nugget"
    }
  }
}  
```  

Now inside the `{}` we're gonna add `"when": <name>` - this will be the name of the custom data, this will be used in
any commands, recipes, etc to tell the game 'Hey **this** item is special and will need to be altered based on special
things' - and prevents the original game items from being broken.

```json  
{
  "model": {
    "type": "minecraft:select",
    "property": "minecraft:custom_model_data",
    "cases": [
      {
        "when": "uno_reverse"
      }
    ],
    "fallback": {
      "type": "minecraft:model",
      "model": "minecraft:item/iron_nugget"
    }
  }
}  
```  

Then we're gonna add the information for what situations we want this texture to change in. This goes under the`"model"`
section after the `"when"`. The type will be `"minecraft:condition"`.

Next will be `"property"`. You can get some ideas on what is
available [here](https://minecraft.wiki/w/Items_model_definition#condition). For the purposes of this tutorial, I am
going to choose `"selected"` - which just means 'it is highlighted in the hotbar'

```json  
{
  "model": {
    "type": "minecraft:select",
    "property": "minecraft:custom_model_data",
    "cases": [
      {
        "when": "uno_reverse",
        "model": {
          "type": "minecraft:condition",
          "property": "minecraft:selected"
        }
      }
    ],
    "fallback": {
      "type": "minecraft:model",
      "model": "minecraft:item/iron_nugget"
    }
  }
}
```  

Next, will be `"on_true"` which is where we'll put the path to the custom model. This code tells the game which model
file to look for. The model file should be in the `assets/<namespace>/models/item` folder.

```json
{
  "model": {
    "type": "minecraft:select",
    "property": "minecraft:custom_model_data",
    "cases": [
      {
        "when": "uno_reverse",
        "model": {
          "type": "minecraft:condition",
          "property": "minecraft:selected",
          "on_true": {
            "type": "minecraft:model",
            "model": "item_model_guide:item/uno_reverse"
          }
        }
      }
    ],
    "fallback": {
      "type": "minecraft:model",
      "model": "minecraft:item/iron_nugget"
    }
  }
}
```

I am also going to need an `"on_false"` section, which in this example will just show the fallback model.

```json
{
  "model": {
    "type": "minecraft:select",
    "property": "minecraft:custom_model_data",
    "cases": [
      {
        "when": "uno_reverse",
        "model": {
          "type": "minecraft:condition",
          "property": "minecraft:selected",
          "on_true": {
            "type": "minecraft:model",
            "model": "item_model_guide:item/uno_reverse"
            },
          "on_false": {
            "type": "minecraft:model",
            "model": "minecraft:item/iron_nugget"
          }
        }
      }
    ],
    "fallback": {
      "type": "minecraft:model",
      "model": "minecraft:item/iron_nugget"
    }
  }
}
```

### How to use `minecraft:composite`

The property `minecraft:composite` allows you to layer multiple models on top of each other. 
If we wanted to do this in our example, we would set up a list named `models` under the `composite` property:

```json
{
  "type": "minecraft:composite",
  "models": []
}
```
We put the two models into that list. I'm going to use red stained glass pane as the second layer because it's easy to see overlap with the texture we're using.

```json
{
  "type": "minecraft:composite",
  "models": [
    {
      "type": "minecraft:model",
      "model": "item_model_guide:item/uno_reverse"
    },
    {
      "type": "minecraft:model",
      "model": "minecraft:item/red_stained_glass_pane"
    }
  ]
}
```

Then we can put this into the item file like this

```json
{
  "model": {
    "type": "minecraft:select",
    "property": "minecraft:custom_model_data",
    "cases": [
      {
        "when": "uno_reverse_layered",
        "model": {
          "type": "minecraft:composite",
          "models": [
            {
              "type": "minecraft:model",
              "model": "item_model_guide:item/uno_reverse"
            },
            {
              "type": "minecraft:model",
              "model": "minecraft:item/red_stained_glass_pane"
            }
          ]
        }
      }
    ],
    "fallback": {
      "type": "minecraft:model",
      "model": "minecraft:item/iron_nugget"
    }
  }
}
```

# Final Touches

You need an mcmeta file or your pack won't work. [misode.github.io](https://misode.github.io/pack-mcmeta/) has a
generator, as well as many other very helpful resources for data/resource packs.

The mcmeta must simply be named `pack.mcmeta`  
If you want to include a pack image, it must be named `pack.png`  
These will be in the base folder, so before the assets folder.
You can now copy and paste the base folder into your game's resource pack folder and try loading it.

# How to get the item in-game

## Command

#### Item Model

If you just did item model, the command will look like this:
`/give @p <item>[minecraft:item_model="<namespace>:<item_name>"]`

The 'item name' will be the file name you used in step one. You can use *any* item for the item you're giving yourself.
So you can apply this model to any item you want. So in our case:

`/give @p peony[minecraft:item_model="item_model_guide:uno_reverse"]`

#### Custom Model Data

For items where you're using custom model data, you'll be using a slightly different command:
`/give @p <item>[minecraft:custom_model_data={strings:["<name>"]}]`

The name in this case will be the name you used in the "when" section of the custom model data declaration.

In our case this would be:
`/give @p minecraft:iron_nugget[minecraft:custom_model_data={strings:["uno_reverse"]}]`

## Recipes

You can use these in a recipe result as well.

#### Item Model

Example of how you can use item model in a recipe:

```json
{
  "type": "minecraft:crafting_shaped",
  "category": "misc",
  "pattern": [
    " X ",
    "X X",
    " X "
  ],
  "key": {
    "X": "minecraft:paper"
  },
  "result": {
    "id": "minecraft:iron_nugget",
    "components": {
      "minecraft:item_model": "custom_model_data_guide_assets:uno_reverse"
    }
  }
}
```

#### Custom Model Data

Making a recipe with custom model data is very similar:

```json
{
  "type": "minecraft:crafting_shaped",
  "category": "misc",
  "pattern": [
    " X ",
    "X X",
    " X "
  ],
  "key": {
    "X": "minecraft:paper"
  },
  "result": {
    "id": "minecraft:iron_nugget",
    "components": {
      "minecraft:custom_model_data": {
        "strings": [
          "uno_reverse"
        ]
      }
    }
  }
}
```

### It's not working!

- Use your debug option on your launcher, it'll help a ton. It'll tell you exactly what's wrong.
- Make sure all your paths are named correctly. A lot of the directories have changed from being plural to being
  singular. This will be the main reason things are messed up, 99% of the time.
- Make sure you have all your declarations in. Item, model, texture.

## References

- [mcmeta](https://github.com/misode/mcmeta)
- [Misode's Stuff](https://misode.github.io/)
- [Minecraft Wiki - Item Model Definition](https://minecraft.wiki/w/Items_model_definition)
- [Minecraft Wiki - Data Components](https://minecraft.wiki/w/Data_component_format)
- [McAssetExtractor](https://github.com/rmheuer/McAssetExtractor)
- [JSON Validator](https://jsonlint.com/)


## Thanks to:
- @AJman14 - 'How to use' section suggestion
- @zbirow - 'minecraft:composite' section suggestion