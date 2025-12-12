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

> ## HOW TO USE?:
> ### Save Method:
> There are two different saving methods: ***FullReconstruct*** and ***HybridConstruct***.
> Both work in different ways and each has its own advantages and disadvantages. FullReconstruct is the newer and recommended method, but HybridConstruct has been tested more extensively.
> 
> <img width="172" height="47" alt="image" src="https://github.com/user-attachments/assets/d0d1b53c-ae95-4696-805c-b835e5c29f5a" />
>
> To choose the method you want, simply go to ***Project Settings → Engine → EasyWorldSave → Manager Class*** and change it.
> <img width="1166" height="184" alt="image" src="https://github.com/user-attachments/assets/8ad3c1ef-fe78-4eee-9166-d835bd5f6cf0" />
>
> By setting a Persist Level, you can choose in which maps the system should be created. If you leave it empty, it will automatically be created in all maps, and you don’t need to do anything else.
> <img width="1128" height="91" alt="image" src="https://github.com/user-attachments/assets/70263c37-2fea-4993-a616-79ceaa671b80" />

> ### FullReconstruct 
