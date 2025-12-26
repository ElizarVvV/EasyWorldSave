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
>
> You can check whether your IDs have been generated by looking at the GuidPreview section.
> 
> <img width="690" height="220" alt="Screenshot 2025-12-12 220319" src="https://github.com/user-attachments/assets/281b5ae7-56ad-43ba-827f-5f0550b6f38b" />
>
> ****Register Save Guild For All Components:**** The adds a SaveGuideline to all components, but it does not generate any IDs.
>
> <img width="301" height="21" alt="image" src="https://github.com/user-attachments/assets/53c83194-c4d6-4f58-a262-6e01c9dee44c" />
>
> ****Un Register Save Guid For All Components:**** The deletes the SaveGuideline of all components in the scene.
> 
> <img width="300" height="19" alt="image" src="https://github.com/user-attachments/assets/e39f957f-66fe-4c91-b51b-4aa19c090150" />
>
> ## FEATURES
>
> ### UActorSaveComponent Features
>
> There are two types of events available inside UActorSaveComponent. You can use these events to handle cases that the system does not support or to extend the functionality of your setup.
> 
> <img width="443" height="85" alt="image" src="https://github.com/user-attachments/assets/b58d0e81-5c2f-462c-98f7-267f5b7fb2b3" />
>
> ****On Save Prop:**** This event is triggered after the actor has finished saving. It returns the *World Save Game Object Reference*, which you can use to store additional global variables inside the instance.
> 
> <img width="223" height="98" alt="image" src="https://github.com/user-attachments/assets/df12d595-7e32-43c8-a2be-bd88bc9b7a77" />
>
> ****On Load Prop:**** This event is triggered after the actor has been loaded. It can be very useful in cases where the system does not provide built-in support. For example, if dynamic materials are not supported, you can use this event to create the dynamic material during loading and reapply its values.
> 
> <img width="223" height="104" alt="image" src="https://github.com/user-attachments/assets/d27e1d89-d395-4f06-9d81-5af923d8eb5f" />
>
> ### You can create a child class of the World Save Manager.
>
> After creating your own custom class and assigning it as the Manager Class in the Project Settings, you can take advantage of many additional features.
> 
> <img width="612" height="217" alt="image" src="https://github.com/user-attachments/assets/83f7df9d-dfdd-4c82-bd90-f0dd62f320ee" />
>
> there are many events you can use. We won’t go over each one individually, but we will provide an example.
> 
> <img width="559" height="264" alt="image" src="https://github.com/user-attachments/assets/30ff3548-2a95-45cd-a176-32e63befa73a" />
>
> For example, I’m using a different UObject-based plugin that also needs to be saved. By using the events, I ensure that both systems are saved and loaded in sequence.
> 
> <img width="634" height="712" alt="image" src="https://github.com/user-attachments/assets/181f4acd-2017-4f9c-9860-7d3f034ff855" />
>
> It contains the slots you want to use by default.
> 
> <img width="381" height="40" alt="image" src="https://github.com/user-attachments/assets/53a70b5f-4cdd-4585-8994-45f8ae163dfc" />
>
> ### World Save Game
>
> By creating a child class from your World Save Game Object, you can define the object that will be used during save and load. This object can contain anything you need at its core, making it useful for storing global values.
>
> <img width="612" height="208" alt="image" src="https://github.com/user-attachments/assets/5b77747a-e1b6-4f08-86c7-ee0b2c7d55cf" />
>
> To use it, it is enough to change the Save Game Class from within the World Save Manager you created.
> 
> <img width="799" height="58" alt="image" src="https://github.com/user-attachments/assets/20954e9e-eaa2-411d-9fd2-630f24b1a3d9" />
>
> ### Player Save
>
> If you want the player to be saved together with the world, make sure that Custom Player Save is disabled under:
>
> Project Settings → Engine → EasyWorldSave → Custom Player Save
>
> When Custom Player Save is disabled, the player is loaded automatically using Get Player Pawn.
>
> However, if you are developing an online/multiplayer game or if you want to save the character independently from the world, this process must be handled manually. In that case, you should enable Custom Player Save and implement your own save/load logic.
>
> <img width="944" height="31" alt="image" src="https://github.com/user-attachments/assets/8551646f-e34f-4a2e-b6c3-af16a2f8e92b" />
>
> You need to add an ActorSaveComponent to the player in order for it to be saved.
> 
> <img width="512" height="24" alt="image" src="https://github.com/user-attachments/assets/4f7e4bae-1692-43f6-87fa-cc2cb1a14099" />
>
> If you are using Full Reconstruct, you must enable the Native option in the component’s settings. This ensures that the character is not deleted during the load process.
> 
> <img width="502" height="30" alt="image" src="https://github.com/user-attachments/assets/4801016d-5b49-4af7-8eb5-ac4e56a3abe3" />
> 
> ## FUNCTIONS
>
> ### Get World Save Manager
>
> This function is used to retrieve the World Save Manager.
> If you have created your own World Save Manager in a custom class, you can obtain it by casting.
> Most of the available functions should be called through this manager, using the returned reference to perform save and load operations.
> 
> <img width="233" height="84" alt="image" src="https://github.com/user-attachments/assets/3809e106-1220-4980-b92f-42dccbcebdbc" />
>
> ### Save / Load World
> 
> This function should be used to save or load the world.
> If you want to work with save slots, that functionality is handled through a separate function.
> 
> <img width="525" height="204" alt="image" src="https://github.com/user-attachments/assets/88f153d3-7a1a-403c-ab22-b771cd642fe7" />
>
> ### Set Save Slot Or Index
>
> With these two functions, you can configure the Save Slot or User Index.
> If no configuration is provided, the default values will be used automatically.
> 
> <img width="563" height="250" alt="image" src="https://github.com/user-attachments/assets/964b65dd-21d4-40e9-8ff9-96ce718e7493" />
>
> ### Get Is First Saved
>
> This function returns true if the world has been previously saved.
> A common use case is during BeginPlay. For example, if you are spawning objects in BeginPlay, they will be spawned every time the game starts. You can prevent this behavior by checking whether a save already exists using this function.
> This function can safely be used in BeginPlay because Pre-Load is executed while the world is being created, before the actual load process begins.
>
> ****Important warning:****
> When this function is used and a saved file already exists, the function will still be executed.
> If this behavior is not desired, you must delete the existing save file.
> 
> <img width="484" height="114" alt="image" src="https://github.com/user-attachments/assets/cc6ec20c-43e0-4b5b-b684-8909a0551ce5" />
>
> ### Does Save Game Exist World Default
>
> This function checks whether a save file exists for the currently selected save slot and user index.
> 
> <img width="568" height="153" alt="image" src="https://github.com/user-attachments/assets/d4e9f48e-7ce7-43dd-b430-e0653ed89887" />
>
> ### Default Slot Delete
>
> This function deletes the world save associated with the currently selected save slot and user index.
> 
> <img width="496" height="171" alt="image" src="https://github.com/user-attachments/assets/819c149f-609c-4021-968c-221018d67f4e" />
>
> ## Manual Property Saving
> 
> This system allows you to manually save and load your UObjects.
>
> ### Save Object To Save Data: 
>
> Here, you must provide the UObject that needs to be saved as the target.
> The function will return the object’s data as Property Data (byte array).
>
> When you assign a variable to the Property Data input, it will be set by reference when the function is executed.
>
> Before performing the save operation, you should create a variable in your World Save Game to store this Property Data.
> This allows you to use the stored property data during the load process to load the UObject again.
> 
> <img width="522" height="236" alt="image" src="https://github.com/user-attachments/assets/577f054b-23e7-4acd-b1a5-fef1d5476e77" />
>
> ### Load Object To Save Data:
> 
> This function loads the provided Property Data into the Target reference during the load process, ensuring that all properties are restored correctly.
> 
> <img width="510" height="246" alt="image" src="https://github.com/user-attachments/assets/1b6cdc81-e0c2-443e-b85c-58a1069f5e2f" />
>
> We also provide a lower-level library related to this system, which allows for more advanced and detailed functionality.
> It can be used in Blueprints as well, but it will not be covered here.

> # Potential Problems
> 
> ### HybridConstruct Map Transitions - Only Editor
> * When using ****HybridConstruct****, save and load operations may not function correctly in the Editor when switching between different maps. This limitation applies only to the Editor; packaged builds are not affected and work correctly. For testing purposes, perform your operations on the main map and avoid changing maps.
> 
> ### BeginPlay
>
> 1- When using BeginPlay, do not overlook the fact that it will be executed again after an actor is loaded and reconstructed. Ignoring this behavior can lead to multiple issues.
>
> For example, if you perform a spawn operation in BeginPlay, that same logic will run again after the actor is loaded, causing the spawn operation to execute once more. This can make it appear as if there is an error in the system, while it is actually expected behavior.
>
> To prevent this issue, you can use Get Is First Saved. By checking whether the actor has been previously saved, you can ensure that the related logic is executed only once and does not run again after loading.
>
> 2- ****Loaded Data Availability During BeginPlay:**** Due to the load execution order, the load process is not completed at the time actors’ BeginPlay functions are called. As a result, loaded data is not yet available during BeginPlay, and attempting to access it at this stage will not work as expected.
>
> To properly access loaded data, you must use OnLoadProp inside the UActorSaveComponent. This callback is executed after the load process has completed, ensuring that all saved properties are correctly restored and accessible.
>
> 3- ****Executing Logic Only Once After Actor Spawn:**** If you need a piece of logic to run only once immediately after an actor is spawned, you must trigger a custom event inside the actor at the moment it is spawned. Due to the execution behavior of BeginPlay and OnLoadProp, neither of these callbacks is suitable for this purpose.
>
> Using Get Is First Saved will also not fully solve this case. Since this check prevents execution when a saved record already exists, the logic will not run again once the actor has been previously saved.
>
> For example, if you rely on Get Is First Saved inside BeginPlay to execute a function and then spawn the actor, this setup will never run as expected. Because the actor already has a saved state, the logic guarded by Get Is First Saved will be skipped entirely.
>
> To avoid this issue, you should execute a dedicated custom event immediately after the actor is spawned, ensuring that the logic runs exactly once at the correct time.
>
> ###  Duplicate Actor
> 
> When using HybridConstruct, if an actor placed in the world does not have a valid Actor ID, it can result in the actor being spawned twice, leading to duplicate instances in the scene.
>
> ### Packaged Build Issues Caused by Non-Transient References
>
> If the system does not function correctly in a packaged build, the issue is most likely caused by references that are not marked as transient.
>
> This may not cause any problems in the Editor, but failing to mark UObject references or related properties as Transient can lead to unexpected behavior or errors in packaged builds.
>
> To avoid this issue, ensure that all runtime-only UObject references are properly marked as Transient.
> 
> <img width="479" height="32" alt="image" src="https://github.com/user-attachments/assets/7a430aee-7a05-4e4a-ac32-0cf962138864" />
>
> <img width="336" height="29" alt="image" src="https://github.com/user-attachments/assets/9c897887-213b-4860-b9a8-cf21594ddd36" />
>
> 








