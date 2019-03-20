# [Phosphor](https://phos.ph) UE4 Style Guide {

[![Analytics](https://ga-beacon.appspot.com/UA-80567399-1/repo?useReferrer)](#) ![#](https://img.shields.io/badge/lint-partial_support-yellow.svg)

> 1. [Directory Structure](#structure)

<a name="2"></a>
<a name="structure"></a>
## 2. Content Directory Structure ![#](https://img.shields.io/badge/lint-partial_support-yellow.svg)

Equally important as asset names, the directory structure style of a project should be considered law. Asset naming conventions and content directory structure go hand in hand, and a violation of either causes unneeded chaos.

There are multiple ways to lay out the content of a UE4 project. In this style, we will be using a structure that relies more on filtering and search abilities of the Content Browser for those working with assets to find assets of a specific type instead of another common structure that groups asset types with folders.

> Naming folders that contain assets of similar types such as `Meshes`, `Textures`, and `Materials` is a redundant practice as asset types are already both sorted by prefix as well as able to be filtered in the content browser.

<a name="2e1"><a>
### 2e1 Example Project Content Structure
<pre>
|-- Content
    |-- <a href="#2.2">GenericShooter</a>
        |-- Art
        |   |-- Industrial
        |   |   |-- Ambient
        |   |   |-- Machinery
        |   |   |-- Pipes
        |   |-- Nature
        |   |   |-- Ambient
        |   |   |-- Foliage
        |   |   |-- Rocks
        |   |   |-- Trees
        |   |-- Office
        |-- Characters
        |   |-- Bob
        |   |-- Common
        |   |   |-- <a href="#2.7">Animations</a>
        |   |   |-- Audio
        |   |-- Jack
        |   |-- Steve
        |   |-- <a href="#2.1.3">Zoe</a>
        |-- <a href="#2.5">Core</a>
        |   |-- Characters
        |   |-- Engine
        |   |-- <a href="#2.1.2">GameModes</a>
        |   |-- Interactables
        |   |-- Pickups
        |   |-- Weapons
        |-- Effects
        |   |-- Electrical
        |   |-- Fire
        |   |-- Weather
        |-- <a href="#2.4">Maps</a>
        |   |-- Campaign1
        |   |-- Campaign2
        |-- <a href="#2.8">MaterialLibrary</a>
        |   |-- Debug
        |   |-- Metal
        |   |-- Paint
        |   |-- Utility
        |   |-- Weathering
        |-- Placeables
        |   |-- Pickups
        |-- Weapons
            |-- Common
            |-- Pistols
            |   |-- DesertEagle
            |   |-- RocketPistol
            |-- Rifles
</pre>

The reasons for this structure are listed in the following sub-sections.

### Sections

> 2.1 [Folder Names](#structure-folder-names)

> 2.2 [Top-Level Folders](#structure-top-level)

> 2.3 [Developer Folders](#structure-developers)

> 2.4 [Maps](#structure-maps)

> 2.5 [Core](#structure-core)

> 2.6 [`Assets` and `AssetTypes`](#structure-assettypes)

> 2.7 [Large Sets](#structure-large-sets)

> 2.8 [Material Library](#structure-material-library)


<a name="2.1"></a>
<a name="structure-folder-names"><a>
### 2.1 Folder Names ![#](https://img.shields.io/badge/lint-partial_support-yellow.svg)

These are common rules for naming any folder in the content structure.

<a name="2.1.1"></a>
#### 2.1.1 Always Use PascalCase[<sup>*</sup>](#terms-cases) ![#](https://img.shields.io/badge/lint-supported-green.svg)

PascalCase refers to starting a name with a capital letter and then instead of using spaces, every following word also starts with a capital letter. For example, `DesertEagle`, `RocketPistol`, and `ASeriesOfWords`.

See [Cases](#terms-cases).

<a name="2.1.2"></a>
#### 2.1.2 Never Use Spaces ![#](https://img.shields.io/badge/lint-supported-green.svg)

Re-enforcing [2.1.1](#2.1.1), never use spaces. Spaces can cause various engineering tools and batch processes to fail. Ideally your project's root also contains no spaces and is located somewhere such as `D:\Project` instead of `C:\Users\My Name\My Documents\Unreal Projects`.

<a name="2.1.3"></a>
#### 2.1.3 Never Use Unicode Characters And Other Symbols ![#](https://img.shields.io/badge/lint-supported-green.svg)

If one of your game characters is named 'Zoë', its folder name should be `Zoe`. Unicode characters can be worse than [Spaces](#2.1.2) for engineering tool and some parts of UE4 don't support Unicode characters in paths either.

Related to this, if your project has [unexplained issues](https://answers.unrealengine.com/questions/101207/undefined.html) and your computer's user name has a Unicode character (i.e. your name is `Zoë`), any project located in your `My Documents` folder will suffer from this issue. Often simply moving your project to something like `D:\Project` will fix these mysterious issues.

Using other characters outside `a-z`, `A-Z`, and `0-9` such as `@`, `-`, `_`, `,`, `*`, and `#` can also lead to unexpected and hard to track issues on other platforms, source control, and weaker engineering tools. 

<a name="2.2"></a>
<a name="structure-top-level"><a>
### 2.2 Use A Top Level Folder For Project Specific Assets ![#](https://img.shields.io/badge/lint-supported-green.svg)

All of a project's assets should exist in a folder named after the project. For example, if your project is named 'Generic Shooter', _all_ of it's content should exist in `Content/GenericShooter`.

> The `Developers` folder is not for assets that your project relies on and therefore is not project specific. See [Developer Folders](#2.3) for details about this.

There are multiple reasons for this approach.

<a name="2.2.1"></a>
#### 2.2.1 No Global Assets

Often in code style guides it is written that you should not pollute the global namespace and this follows the same principle. When assets are allowed to exist outside of a project folder it often becomes much harder to enforce a strict structure layout as assets not in a folder encourages the bad behavior of not having to organize assets.

Every asset should have a purpose, otherwise it does not belong in a project. If an asset is an experimental test and shouldn't be used by the project it should be put in a [`Developer`](#2.3) folder.

<a name="2.2.2"></a>
#### 2.2.2 Reduce Migration Conflicts

When working on multiple projects it is common for a team to copy assets from one project to another if they have made something useful for both. When this occurs, the easiest way to perform the copy is to use the Content Browser's Migrate functionality as it will copy over not just the selected asset but all of its dependencies.

These dependencies are what can easily get you into trouble. If two project's assets do not have a top level folder and they happen to have similarly named or already previously migrated assets, a new migration can accidentally wipe any changes to the existing assets.

This is also the primary reason why Epic's Marketplace staff enforces the same policy for submitted assets.

After a migration, safe merging of assets can be done using the 'Replace References' tool in the content browser with the added clarity of assets not belonging to a project's top level folder are clearly pending a merge. Once assets are merged and fully migrated, there shouldn't be another top level folder in your Content tree. This method is _100%_ guaranteed to make any migrations that occur completely safe.

<a name="2.2.2e1"></a>
##### 2.2.2e1 Master Material Example

For example, say you created a master material in one project that you would like to use in another project so you migrated that asset over. If this asset is not in a top level folder, it may have a name like `Content/MaterialLibrary/M_Master`. If the target project doesn't have a master material already, this should work without issue.

As work on one or both projects progress their respective master materials may change to be tailored for their specific projects due to the course of normal development.

The issue comes when, for example, an artist for one project created a nice generic modular set of static meshes and someone wants to include that set of static meshes in the second project. If the artist who created the assets used material instances based on `Content/MaterialLibrary/M_Master` as they're instructed to, when a migration is performed there is a great chance of conflict for the previously migrated `Content/MaterialLibrary/M_Master` asset.

This issue can be hard to predict and hard to account for. The person migrating the static meshes may not be the same person who is familiar with the development of both project's master material, and they may not be even aware that the static meshes in question rely on material instances which then rely on the master material. The Migrate tool requires the entire chain of dependencies to work however, and so it will be forced to grab `Content/MaterialLibrary/M_Master` when it copies these assets to the other project and it will overwrite the existing asset.

It is at this point where if the master materials for both projects are incompatible in _any way_, you risk breaking possibly the entire material library for a project as well as any other dependencies that may have already been migrated, simply because assets were not stored in a top level folder. The simple migration of static meshes now becomes a very ugly task.

<a name="2.2.3"></a>
#### 2.2.3 Samples, Templates, and Marketplace Content Are Risk-Free

An extension to [2.2.2](#2.2.2), if a team member decides to add sample content, template files, or assets they bought from the marketplace, it is guaranteed that these new assets will not interfere with the project in any way unless your project's top level folder is not uniquely named.

You can not trust marketplace content to fully conform to the [top level folder rule](#2.2). There exist many assets that have the majority of their content in a top level folder but also have possibly modified Epic sample content as well as level files polluting the global `Content` folder.

When adhering to [2.2](#2.2), the worst marketplace conflict you can have is if two marketplace assets both have the same Epic sample content. If all your assets are in a project specific folder, including sample content you may have moved into your folder, your project will never break.

#### 2.2.4 DLC, Sub-Projects, and Patches Are Easily Maintained

If your project plans to release DLC or has multiple sub-projects associated with it that may either be migrated out or simply not cooked in a build, assets relating to these projects should have their own separate top level content folder. This make cooking DLC separate from main project content far easier. Sub-projects can also be migrated in and out with minimal effort. If you need to change a material of an asset or add some very specific asset override behavior in a patch, you can easily put these changes in a patch folder and work safely without the chance of breaking the core project.

<a name="2.3"></a>
<a name="structure-developers"></a>
### 2.3 Use Developers Folder For Local Testing ![#](https://img.shields.io/badge/lint-unsupported-red.svg)

During a project's development, it is very common for team members to have a sort of 'sandbox' where they can experiment freely without risking the core project. Because this work may be ongoing, these team members may wish to put their assets on a project's source control server. Not all teams require use of Developer folders, but ones that do use them often run into a common problem with assets submitted to source control.

It is very easy for a team member to accidentally use assets that are not ready for use which will cause issues once those assets are removed. For example, an artist may be iterating on a modular set of static meshes and still working on getting their sizing and grid snapping correct. If a world builder sees these assets in the main project folder, they might use them all over a level not knowing they could be subject to incredible change and/or removal. This causes massive amounts of re-working by everyone on the team to resolve.

If these modular assets were placed in a Developer folder, the world builder should never of had a reason to use them and the whole issue would never happen. The Content Browser has specific View Options that will hide Developer folders (they are hidden by default) making it impossible to accidentally use Developer assets under normal use.

Once the assets are ready for use, an artist simply has to move the assets into the project specific folder and fix up redirectors. This is essentially 'promoting' the assets from experimental to production.

<a name="2.4"></a>
<a name="structure-maps"></a>
### 2.4 All Map[<sup>*</sup>](#terms-level-map) Files Belong In A Folder Called Maps ![#](https://img.shields.io/badge/lint-supported-green.svg)

Map files are incredibly special and it is common for every project to have its own map naming system, especially if they work with sub-levels or streaming levels. No matter what system of map organization is in place for the specific project, all levels should belong in `/Content/Project/Maps`.

Being able to tell someone to open a specific map without having to explain where it is is a great time saver and general 'quality of life' improvement. It is common for levels to be within sub-folders of `Maps`, such as `Maps/Campaign1/` or `Maps/Arenas`, but the most important thing here is that they all exist within `/Content/Project/Maps`.

This also simplifies the job of cooking for engineers. Wrangling levels for a build process can be extremely frustrating if they have to dig through arbitrary folders for them. If a team's maps are all in one place, it is much harder to accidentally not cook a map in a build. It also simplifies lighting build scripts as well QA processes.

<a name="2.5"></a>
<a name="structure-core"></a>
### 2.5 Use A `Core` Folder For Critical Blueprints And Other Assets ![#](https://img.shields.io/badge/lint-unsupported-red.svg)

Use `/Content/Project/Core` folder for assets that are absolutely fundamental to a project's workings. For example, base `GameMode`, `Character`, `PlayerController`, `GameState`, `PlayerState`, and related Blueprints should live here.

This creates a very clear "don't touch these" message for other team members. Non-engineers should have very little reason to enter the `Core` folder. Following good code structure style, designers should be making their gameplay tweaks in child classes that expose functionality. World builders should be using prefab Blueprints in designated folders instead of potentially abusing base classes.

For example if your project requires pickups that can be placed in a level, there should exist a base Pickup class in `Core/Pickups` that defines base behavior for a pickup. Specific pickups such as a Health or Ammo should exist in a folder such as `/Content/Project/Placeables/Pickups/`. Game designers can define and tweak pickups in this folder however they please, but they should not touch `Core/Pickups` as they may unintentionally break pickups project-wide.

<a name="2.6"></a>
<a name="structure-assettypes"></a>
### 2.6 Do Not Create Folders Called `Assets` or `AssetTypes` ![#](https://img.shields.io/badge/lint-supported-green.svg)

<a name="2.6.1"></a>
#### 2.6.1 Creating a folder named `Assets` is redundant. ![#](https://img.shields.io/badge/lint-supported-green.svg)

All assets are assets.

<a name="2.6.2"></a>
#### 2.6.2 Creating a folder named `Meshes`, `Textures`, or `Materials` is redundant. ![#](https://img.shields.io/badge/lint-supported-green.svg)

All asset names are named with their asset type in mind. These folders offer only redundant information and the use of these folders can easily be replaced with the robust and easy to use filtering system the Content Browser provides.

Want to view only static mesh in `Environment/Rocks/`? Simply turn on the Static Mesh filter. If all assets are named correctly, they will also be sorted in alphabetical order regardless of prefixes. Want to view both static meshes and skeletal meshes? Simply turn on both filters. this eliminates the need to potentially have to `Control-Click` select two folders in the Content Browser's tree view.

> This also extends the full path name of an asset for very little benefit. The `S_` prefix for a static mesh is only two characters, whereas `Meshes/` is seven characters.

Not doing this also prevents the inevitability of someone putting a static mesh or a texture in a `Materials` folder.

<a name="2.7"></a>
<a name="structure-large-sets"></a>
### 2.7 Very Large Asset Sets Get Their Own Folder Layout ![#](https://img.shields.io/badge/lint-unsupported-red.svg)

This can be seen as a pseudo-exception to [2.6](#2.6).

There are certain asset types that have a huge volume of related files where each asset has a unique purpose. The two most common are Animation and Audio assets. If you find yourself having 15+ of these assets that belong together, they should be together.

For example, animations that are shared across multiple characters should lay in `Characters/Common/Animations` and may have sub-folders such as `Locomotion` or `Cinematic`.

> This does not apply to assets like textures and materials. It is common for a `Rocks` folder to have a large amount of textures if there are a large amount of rocks, however these textures are generally only related to a few specific rocks and should be named appropriately. Even if these textures are part of a [Material Library](#2.8).

<a name="2.8"></a>
<a name="structure-material-library"></a>
### 2.8 `MaterialLibrary` ![#](https://img.shields.io/badge/lint-unsupported-red.svg)

If your project makes use of master materials, layered materials, or any form of reusable materials or textures that do not belong to any subset of assets, these assets should be located in `Content/Project/MaterialLibrary`.

This way all 'global' materials have a place to live and are easily located.

> This also makes it incredibly easy to enforce a 'use material instances only' policy within a project. If all artists and assets should be using material instances, then the only regular material assets that should exist are within this folder. You can easily verify this by searching for base materials in any folder that isn't the `MaterialLibrary`.

The `MaterialLibrary` doesn't have to consist of purely materials. Shared utility textures, material functions, and other things of this nature should be stored here as well within folders that designate their intended purpose. For example, generic noise textures should be located in `MaterialLibrary/Utility`.

Any testing or debug materials should be within `MaterialLibrary/Debug`. This allows debug materials to be easily stripped from a project before shipping and makes it incredibly apparent if production assets are using them if reference errors are shown.

<a name="2.9"></a>
<a name="structure-no-empty-folders"></a>
### 2.9 No Empty Folders ![#](https://img.shields.io/badge/lint-unsupported-red.svg)

There simply shouldn't be any empty folders. They clutter the content browser.

If you find that the content browser has an empty folder you can't delete, you should perform the following:
1. Be sure you're using source control.
1. Immediately run Fix Up Redirectors on your project.
1. Navigate to the folder on-disk and delete the assets inside.
1. Close the editor.
1. Make sure your source control state is in sync (i.e. if using Perforce, run a Reconcile Offline Work on your content directory)
1. Open the editor. Confirm everything still works as expected. If it doesn't, revert, figure out what went wrong, and try again.
1. Ensure the folder is now gone.
1. Submit changes to source control.

**[⬆ Back to Top](#table-of-contents)**
