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
> * Runtime Add Or Remove UActorComponent
> * If your components contain values that are not marked with UPROPERTY, the save system will not detect them. UPROPERTY is essential for making data savable. If this tag is missing and the save system cannot capture the related data, you can create your own custom data class in C++ by deriving from USaveBaseData.
>  (For example, since the Static Mesh inside a UStaticMeshComponent cannot be detected directly, we created the USaveStaticMeshData class to make it savable.)

> ## ⚠️ IMPORTANT INFORMATION:
> **Don’t forget to check the sections where you may encounter issues with the plugin. Some problems may only appear after creating a packaged build, so it’s important to test it there as well. I highly recommend reading the entire documentation before using the plugin. If it is used incorrectly, it will not function properly.**

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
>
> ### FullReconstruct (Recommended)
> 
> **How does it work?**
> 
> Actors that exist in the level from the start and actors that are spawned during runtime are two different categories, and it is important to distinguish between them. If an actor has a UActorSaveComponent, the save system collects both types of actors together and treats all of them as spawned actors. Because of this, during the load process, the actors that originally existed in the level are removed and recreated. Even if they were already present in the world, the system handles them as if they were newly spawned.
> 
> **How to use?**
> 
> Adding a UActorSaveComponent to an actor is enough for it to be included in the save system. However, keep in mind that if you add this component to an actor that already exists in the world from the level editor (not through Blueprint or C++), it will not work. If you need this to function properly, you should use HybridConstruct.
> 
> <img width="512" height="174" alt="Screenshot 2025-12-12 210100" src="https://github.com/user-attachments/assets/cfd2a1b1-12d0-4f53-af74-36d4f2d67913" />
>
> ### HybridConstruct (Legacy)
>
> **How does it work?**
> 
> Actors and components that are already placed in the world are treated differently from those created through Actor Blueprints or C++. These two categories are saved, loaded, and collected in different ways. Every actor and component has its own unique ID, and actors that already exist in the level are never deleted during loading. Instead, the system loads data directly onto them. This also ensures that components you added from the level editor are properly saved and restored.
>
> **How to use?**
>
> Actors that are placed in the world and actors that are spawned during runtime are treated as two separate categories. Because of this, there are two steps you need to follow.
>
> **For spawned actors:**
> 
> You need to add a UActorSaveComponent to the actor. This component will automatically generate a unique ID for the actor and all of its components. Because of this, make sure you do not manually add any Save Guideline components to the components inside it.
> 
> <img width="512" height="174" alt="Screenshot 2025-12-12 210100" src="https://github.com/user-attachments/assets/cfd2a1b1-12d0-4f53-af74-36d4f2d67913" />
>
> You need to add a SaveGuideline inside the UActorSaveComponent. You can add it from the Components panel by selecting ActorSave in the Details panel.
> 
> <img width="711" height="56" alt="image" src="https://github.com/user-attachments/assets/8ef237a4-44bc-4418-8f6d-90cebcf21cea" />>
>
> **For actors and components placed in the world:**
>
> ****Information:**** You do not need to add a UActorSaveComponent to actors that are already placed in the world. However, if you don’t add it, the actor itself will not be saved. Even so, any components inside the actor will still be saved if you added a SaveGuideline to them. This works because every component has its own unique ID.
>
> If you want to save the actor's properties, you need to add UActorSaveComponent and also add SaveGuideline.
> 
> <img width="695" height="165" alt="Screenshot 2025-12-12 214349" src="https://github.com/user-attachments/assets/18446db5-f3e8-4d25-9131-9edebb7da3d4" />
>
> <img width="711" height="56" alt="Screenshot 2025-12-12 213512" src="https://github.com/user-attachments/assets/7ab892c7-c309-4b80-9021-292dbd652998" />
>
> If you want to save the components inside the actor, it is enough to add a SaveGuideline to them. If you do not add it, those components will not be saved.
>
> ****You need to create an ID for actors that already exist in the world:****
>
> Open the Tools → Miscellaneous → EasyWorldSave window.
> 
> <img width="405" height="38" alt="image" src="https://github.com/user-attachments/assets/4610aec3-b7cd-4307-8f57-2afe826bee46" />
>
> You will see three buttons. These buttons run asynchronously and use multiple CPU cores, allowing you to manage ID operations in bulk quickly and efficiently.
> 
> <img width="324" height="107" alt="image" src="https://github.com/user-attachments/assets/9462a515-739c-4ea3-89b2-566dcf90e4a3" />
>
> ****Generate Save Guid For Save Guideline:**** It generates IDs for all SaveGuidelines in the scene. If you do not generate these IDs, you will run into issues, so always make sure that none of your SaveGuidelines are missing an ID.
>
> <img width="308" height="24" alt="image" src="https://github.com/user-attachments/assets/45a2354b-9bcb-43a4-bd8e-79acc7d0ead5" />




