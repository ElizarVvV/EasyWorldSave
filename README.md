# EASY WORLD SAVE DOCUMENT
> ## ⚠️ IMPORTANT: 
> There can be a lot of things in your world that need to be saved. Attach structures, material changes, meshes, properties, runtime-generated objects, dozens of interconnected components… Since all of these are constantly changing in the scene, deciding what to save and how to save it can become quite challenging. Wrong references, missing data, or actors loading back  in the wrong position are just some of the common problems.
 If you’re working with Blueprints, things can get even more complicated. Node graphs get larger, performance drops, and you end up having to save everything manually. In the end, building a solid save system takes far more time than expected in most projects.
> This plugin was created specifically to solve these issues. It automatically saves and loads complex world structures, objects, and their relationships for you. However, there is one important thing to remember: simply enabling the plugin is not enough. A small integration step is required. But aside from that, most of the work is already handled for you.

> ## 🛠️ WHAT DOES THIS PLUGIN OFFER YOU?:
> ### 💾 What does it save?:
> * It saves all properties (variables), and you don’t need to do anything.
> * Mesh
> * Materials
> * Attach, Socket
> * Modular Usability(This makes it very easy to save even things that cannot be save)
> * Identity System
> * Component , Actor
> * Animation Support
> * More Feature...
> ### 🔍 What Doesn’t Get Saved?:
> * It does not save references within variables. Therefore, if you create a UObject variable, this variable is not saved. However, it makes it easier to save them when necessary. 
> * If set transient in variable, it is not saved.
> * Dynamic Material
> * Animation Blueprint
> * Behavior Tree
> * If your components contain values that are not marked with UPROPERTY, the save system will not detect them. UPROPERTY is essential for making data savable. If this tag is missing and the save system cannot capture the related data, you can create your own custom data class in C++ by deriving from USaveBaseData.
>  (For example, since the Static Mesh inside a UStaticMeshComponent cannot be detected directly, we created the USaveStaticMeshData class to make it savable.)

> ## ⚠️ IMPORTANT INFORMATION:
> ### Don’t forget to check the sections where you may encounter issues with the plugin. Some problems may only appear after creating a packaged build, so it’s important to test it there as well. I highly recommend reading the entire documentation before using the plugin. If it is used incorrectly, it will not function properly.

# Click here to learn how to use it.

# Click here to learn about the issues you may encounter.

