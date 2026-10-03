# Code understanding

>This file is divided in two parts. The Macro gives a global
understanding of how everything should work. The Micro details
the purpose of each class, before explaining the purpose of
each of their methods, who calls it and for what reason(s).

## Macro

### Vocabulary
- Attributes will be written as the following : *"<name_of_variable> : <type_of_variable>"*.
- When describing methods of a class, the word *this* relates to the object the methods
calls on, as in "obj.f()", *this* relates to *obj*.
- The game will run with a loop in the *Engine* class. Each step of the loop will be
called a *"tick"*. Some actions, in order to optimize the game, do not need to be run
every tick, they will be every *"step"*. A step may clock 1 to 4 times a second while 
tick will be around 20/s. FPS still stands for *"frame per second"*, therefore, it is
not linked to how fast the *Engine* loops, but on how fast the *Display* does, it can
be up to 60/s for example.
- Some methods may need verifications first before being called. Indeed, on every process,
there will be first a test method, then a *"<name_of_the_method>TY"* that will
run without checking for issues. TY stands for "Trusts You (for calling the
method)". You, are the developper. The idea of splitting these methods in two is
that you may need many checks before commiting to the action. Some character may
need to check if there is enough space in a storage before walking to it, then check
again just before putting the item in.

### Heat Management
- Each object influenced by temperature needs to interact with the world around
to get its new temperature every step.  
- Because every object can be made up of several materials, each one will have
a *material_composition*, and a *HeatManager* attribute.  
- The goal of the *HeatManager* is **first**, when the object **is created**, to compute
the average heat capacity, thermal conductivity and albedo of the object. The link
between material and heat parameters is the *HeatParameters* class linked with the enum
*Material* by the map in *HeatData* in *Factory*. **Then**, on **each tick**,
the *HeatManager* will compute based on the world around the object, the change
in temperature.  
- Objects influenced by temperature are : *Stackable (Item)*, *Container (Unique Item*,
*Meal (Unique Item)*, *OnTile* and *abstract Entity*
- *Entity* are subjected to heat, but have protection like *Apparel*, the same goes on with
*OnTile* being protected by *RoofTile* from sun exposition. While *Apparel* and *RoofTile*
do not have temperature, their existence will be taken into account as *Entity* and 
*OnTile* will inform *HeatManager* of a factor to multiply by the flow which 
directly depends on the protection used.

## Micro

### Coding style
- There will be no *shared_ptr* nor *unique_ptr* as many classes are daughter of
abstracts/classes. Instead, I will use classic *pointers* using the keyword *new*
and storing every objects in the *Factory*.
- There will be very few array, replacing them by *vector* which will have a "fixed"
capacity by using the keyword *reserve* just after creating a *vector*.

### Storage
>This class is not linked to anything physical. Its principal attributes are :  
>*list* : *vector<Item\*>* representing what is stored, the current volume occupied and maximum
volume, a pointer to its owner. Many classes/abstracts like *Entity*, *GroundTile*,
*Container (Unique Item)* or *Furniture* may have a *Storage*.  

>Its purpose is to manage the *Item* flow by transfering them from a *Storage* to another,
and merging them if they are *Stackable*.

- *bool canTake(Unique\* item)*  
Returns *true* if *this* has enough space to take *item*.
- *int canTake(Stackable\* item, int nb)*  
Returns the number of item *this* can take before being full. Returns *nb* if
everything can be transferred and 0 if none.
- *void putInTY(Unique\* item)*  
Puts in *this* the *item*, raises an AssertionError if the current volume exceeds
the maximum.
- ***private** Stackable\* searchForMerge(Stackable\* item)*  
Returns the pointer of an already existing *Stackable* item stored in *this* that
corresponds to *item* so *item* can be merged. Creates a *Stackable* item with 
*number=0* and return its pointer if no *Stackable* has been found.
- *void putInTY(Stackable\* item, int nb)*  
Puts in *this nb item*. It will merge to an already existing *Stackable* in *this* thanks
to the *searchForMerge* method.

