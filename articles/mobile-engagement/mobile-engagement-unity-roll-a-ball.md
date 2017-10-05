---
title: "Unity összegző golyó oktatóanyag"
description: "Lépések végrehajtásával hozza létre a Unity klasszikus Roll egy golyó játék, amely előzetesen szükséges összes Mobile Engagement Unity-oktatóanyag"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0afd0eca-f74a-43aa-bba8-436a0265c312
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 6392d1f780b1bc2348fee5947550b05e86ea4de2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="ee7f8-103"><a id="unity-roll-a-ball"></a>Hozzon létre Unity Roll golyó játék</span><span class="sxs-lookup"><span data-stu-id="ee7f8-103"><a id="unity-roll-a-ball"></a>Create Unity Roll a Ball game</span></span>
<span data-ttu-id="ee7f8-104">Ez az oktatóanyag végigvezeti egy kis mértékben módosított fő lépései [Unity visszaállítani egy golyó oktatóanyag](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial).</span><span class="sxs-lookup"><span data-stu-id="ee7f8-104">This tutorial walks through the main steps for a slightly modified [Unity Roll a Ball tutorial](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial).</span></span> <span data-ttu-id="ee7f8-105">Ez a minta játék egy gömb "player" objektumon, amelynek az alkalmazás felhasználói által szabályozott áll, és a játék célja, hogy a "collect" memóriából kiüríthető objektumok által a memóriából kiüríthető objektumok player objektum ütközés.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-105">This sample game consists of a spherical 'player' object which is controlled by the app user and the objective of the game is to 'collect' collectible objects by colliding the player object with these collectible objects.</span></span> <span data-ttu-id="ee7f8-106">Ez azt feltételezi, hogy a Unity-szerkesztő környezet alapszintű ismeretét.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-106">This assumes basic familiarity with Unity editor environment.</span></span> <span data-ttu-id="ee7f8-107">Ha probléma merül fel, majd tekintse át a teljes útmutató.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-107">If you run into any issues then you should refer to the full tutorial.</span></span> 

### <a name="setting-up-the-game"></a><span data-ttu-id="ee7f8-108">A game beállítása</span><span class="sxs-lookup"><span data-stu-id="ee7f8-108">Setting up the game</span></span>
<span data-ttu-id="ee7f8-109">A rendszer az alábbi lépéseket a [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)</span><span class="sxs-lookup"><span data-stu-id="ee7f8-109">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)</span></span>

1. <span data-ttu-id="ee7f8-110">Nyissa meg **Unity-szerkesztő** kattintson **új**.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-110">Open **Unity Editor** and click **New**.</span></span> 
   
    ![][51] 
2. <span data-ttu-id="ee7f8-111">Adjon meg egy **projektnevet** & **hely**, jelölje be **3D** kattintson **Create project**.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-111">Provide a **Project name** & **Location**, select **3D** and click **Create project**.</span></span>
   
    ![][52]
3. <span data-ttu-id="ee7f8-112">Mentse a részeként az új projekt nevét az imént létrehozott alapértelmezett helyszín **MiniGame** belül egy új  **\_színfalak** mappát **eszközök** mappába:</span><span class="sxs-lookup"><span data-stu-id="ee7f8-112">Save the default scene just created as part of the new project as with the name **MiniGame** within a new **\_Scenes** folder under **Assets** folder:</span></span>
   
    ![][53]
4. <span data-ttu-id="ee7f8-113">Hozzon létre egy **3D objektum -> Vezérlősík** a játszik mezőben, és nevezze át ezt az vezérlősík objektumot **Ground**</span><span class="sxs-lookup"><span data-stu-id="ee7f8-113">Create a **3D Object -> Plane** as the playing field and rename this plane object as **Ground**</span></span>
   
    ![][1]
5. <span data-ttu-id="ee7f8-114">Az átalakítási összetevő alaphelyzetbe a **Ground** objektumot, hogy a forráson van.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-114">Reset the transform component for this **Ground** object so that it is at the Origin.</span></span> 
   
    ![][3]
6. <span data-ttu-id="ee7f8-115">Törölje a jelet **rácsvonalak megjelenítése** a **Gizmos menü** a a **Ground** objektum.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-115">Uncheck **Show Grid** from **Gizmos menu** for the **Ground** object.</span></span>
   
    ![][4]
7. <span data-ttu-id="ee7f8-116">Frissítés a **méretezési** az összetevőt a **Ground** objektumtípus [X = 2, Y = 1, Z = 2].</span><span class="sxs-lookup"><span data-stu-id="ee7f8-116">Update the **Scale** component for the **Ground** object to be [X = 2,Y = 1, Z = 2].</span></span> 
   
    ![][5]
8. <span data-ttu-id="ee7f8-117">Adjon hozzá egy új **3D objektum -> kapcsolataikat** a projekt és az eddigi objektum, nevezze át **Player**.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-117">Add a new **3D Object -> Sphere** to the project and rename this sphere object as **Player**.</span></span> 
   
    ![][6]
9. <span data-ttu-id="ee7f8-118">Válassza ki a **Player** objektumra, és kattintson a **átalakítás visszaállítása** hasonló, és a Vezérlősík objektum.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-118">Select the **Player** object and click **Reset Transform** similar to the Plane object.</span></span> 
10. <span data-ttu-id="ee7f8-119">Frissítés **átalakító -> pozíció -> Y koordinátáját** 0,5 Player Y az összetevőt.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-119">Update **Transform -> Position -> Y Coordinate** component for the Player Y as 0.5.</span></span>  
    
    ![][7]
11. <span data-ttu-id="ee7f8-120">Hozzon létre egy új nevű **anyagok** a projektben, ahol az anyag a Windows Media player színes létre fogunk hozni.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-120">Create a new folder called **Materials** in the project where we will create the material to color the player.</span></span> 
12. <span data-ttu-id="ee7f8-121">Hozzon létre egy új **anyag** nevű **háttér** ebben a mappában.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-121">Create a new **Material** called **Background** in this folder.</span></span> 
    
    ![][8]
13. <span data-ttu-id="ee7f8-122">Az anyag színének frissítése frissítésével a **Albedo** tulajdonsága azt.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-122">Update the color of the material by updating the **Albedo** property of it.</span></span> <span data-ttu-id="ee7f8-123">Kiválaszthatja a [0,32,64] RGB-értékeket.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-123">You can select the RGB values of [0,32,64].</span></span> 
    
    ![][9]
14. <span data-ttu-id="ee7f8-124">Húzza a leképezni kívánt jelenetben nézet színnel anyagot a **Ground** objektum.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-124">Drag this material into the scene view to apply color to the **Ground** object.</span></span> 
    
    ![][10]
15. <span data-ttu-id="ee7f8-125">Végül frissítse a **átalakító -> Elforgatás -> Y** 60 jobb érthetőség kedvéért bizonyos irányt világos objektumon.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-125">Finally update the **Transform -> Rotation -> Y** to 60 on the Directional Light object for clarity.</span></span> 
    
    ![][12]

### <a name="moving-the-player"></a><span data-ttu-id="ee7f8-126">A Windows Media player áthelyezése</span><span class="sxs-lookup"><span data-stu-id="ee7f8-126">Moving the player</span></span>
<span data-ttu-id="ee7f8-127">A rendszer az alábbi lépéseket a [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)</span><span class="sxs-lookup"><span data-stu-id="ee7f8-127">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)</span></span>

1. <span data-ttu-id="ee7f8-128">Adja hozzá a **RigidBody** összetevőt, hogy a **Player** objektum.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-128">Add a **RigidBody** component to the **Player** object.</span></span> 
   
    ![][13]
2. <span data-ttu-id="ee7f8-129">Hozzon létre egy új nevű **parancsfájlok** a projektben.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-129">Create a new folder called **Scripts** in the Project.</span></span> 
3. <span data-ttu-id="ee7f8-130">Kattintson a **összetevőt -> Új parancsfájl -> C# parancsfájl**.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-130">Click **Add Component-> New Script -> C# Script**.</span></span> <span data-ttu-id="ee7f8-131">Nevezze el **PlayerController**, és kattintson a **létrehozása és hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-131">Name it **PlayerController**, and click **Create and Add**.</span></span> <span data-ttu-id="ee7f8-132">Ezzel a hoz létre, és egy parancsfájl csatolása a Player objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-132">This will create and attach a script to the Player object.</span></span>  
   
    ![][14]
4. <span data-ttu-id="ee7f8-133">Ezt a parancsfájlt a helyezze át a **parancsfájlok** a projekt mappájára.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-133">Move this script under the **Scripts** folder in the project.</span></span> 
5. <span data-ttu-id="ee7f8-134">Nyissa meg szerkesztésre a kedvenc parancsprogram-szerkesztő, a script kód frissítése a következő kódra, és mentse azokat.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-134">Open the script for editing in your favorite script editor, update the script code with the following code and save it.</span></span> 
   
        using UnityEngine;
        using System.Collections;
   
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
6. <span data-ttu-id="ee7f8-135">Vegye figyelembe, hogy a fenti parancsfájl használ egy **sebesség** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-135">Note that the script above uses a **Speed** property.</span></span> <span data-ttu-id="ee7f8-136">A Unity-szerkesztőben frissítse a sebesség tulajdonság 10.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-136">In the Unity editor, update the speed property to 10.</span></span>  
   
    ![][15]
7. <span data-ttu-id="ee7f8-137">Találati **lejátszása** a Unity-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-137">Hit **Play** in the Unity Editor.</span></span> <span data-ttu-id="ee7f8-138">Most kell vezérelhetik a billentyűzettel golyó, és azt kell elforgatás és navigálás.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-138">Now you should be able to control the ball using the keyboard and it should rotate and move around.</span></span> 

### <a name="moving-the-camera"></a><span data-ttu-id="ee7f8-139">A kamera áthelyezése</span><span class="sxs-lookup"><span data-stu-id="ee7f8-139">Moving the camera</span></span>
<span data-ttu-id="ee7f8-140">A rendszer az alábbi lépéseket a [Unity-oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) és lelassul a **fő kamera** számára a **Player** objektum.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-140">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) and will tie the **Main Camera** to the **Player** object.</span></span> 

1. <span data-ttu-id="ee7f8-141">Frissítés a **Transform.Position** X kell = 0, Y 10.5, Z-= 10 =.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-141">Update the **Transform.Position** to be X = 0,  Y = 10.5, Z=-10.</span></span>  
2. <span data-ttu-id="ee7f8-142">Frissítés a **Transform.Rotation** X kell = 45, I = 0, Z = 0.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-142">Update the **Transform.Rotation** to be X = 45, Y = 0, Z = 0.</span></span>  
   
    ![][16]
3. <span data-ttu-id="ee7f8-143">Nevű új parancsfájl hozzáadása **CameraController** számára a **MainCamera** , és a parancsfájlok mappa alá helyezi.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-143">Add a new script called **CameraController** to the **MainCamera** and move it under the Scripts folder.</span></span> 
   
    ![][17]
4. <span data-ttu-id="ee7f8-144">Nyissa meg szerkesztésre a parancsfájlt, és adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="ee7f8-144">Open up the script for editing and add the following code in it:</span></span>
   
        using UnityEngine;
        using System.Collections;
   
        public class CameraController : MonoBehaviour {
   
            public GameObject player;
   
            private Vector3 offset;
   
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
   
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
5. <span data-ttu-id="ee7f8-145">Környezetben Unity - húzza a Player változó a fő kamera objektum a Player tárhely, hogy a két társítva egy másik.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-145">In Unity environment - drag the Player variable into the Player slot for the Main Camera object so that the two are associated with one another.</span></span> 
   
    ![][18]
6. <span data-ttu-id="ee7f8-146">Most már a Unity-szerkesztőben kattintson a Play és a Player golyó objektum elforgatása majd látják a kamera következő mozgása.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-146">Now if you hit Play in the Unity editor and rotate the Player Ball object then you will see the Camera following it in the movement.</span></span>  

### <a name="setting-up-the-play-area"></a><span data-ttu-id="ee7f8-147">A Play terület beállítása</span><span class="sxs-lookup"><span data-stu-id="ee7f8-147">Setting up the Play area</span></span>
<span data-ttu-id="ee7f8-148">A rendszer az alábbi lépéseket a [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="ee7f8-148">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141).</span></span> <span data-ttu-id="ee7f8-149">A körülvevő az alapoktól, így a Player golyó objektum nem leadásához mozgása play területén falakat létre fogunk hozni.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-149">We will create the Walls surrounding the Ground so that the Player Ball object doesn't drop off the play area in its movement.</span></span> 

1. <span data-ttu-id="ee7f8-150">Kattintson a **létrehozás -> hozzon létre üres játék objektum ->** és adjon neki nevet **falvastagságát**</span><span class="sxs-lookup"><span data-stu-id="ee7f8-150">Click **Create -> Create Empty -> Game Object** and name it **Walls**</span></span>
   
    ![][19]
2. <span data-ttu-id="ee7f8-151">Mért falvastagságát objektum - alatt hozzon létre egy új **3D objektum adatkocka ->** "Nyugati fali" néven.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-151">Under this Walls object - create a new **3D Object -> Cube** and name it "West wall".</span></span> 
   
    ![][20]
3. <span data-ttu-id="ee7f8-152">Frissítés a **átalakító pozíció ->** és **átalakítási a méretezési ->** nyugati fali objektum.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-152">Update the **Transform -> Position** and **Transform -> Scale** for this West Wall object.</span></span> 
   
    ![][21]
4. <span data-ttu-id="ee7f8-153">Ismétlődő a nyugati fali létrehozásához egy **keleti fali** a módosított rendelkező átalakítási elhelyezése és méretezése.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-153">Duplicate the West wall to create an **East wall** with the updated transform position and scale.</span></span> 
   
    ![][22]
5. <span data-ttu-id="ee7f8-154">Ismétlődő a keleti fali létrehozásához egy **északi fali** a módosított rendelkező átalakítási pozíció & skála.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-154">Duplicate the East wall to create a **North wall** with the updated transform position & scale.</span></span> 
   
    ![][23]
6. <span data-ttu-id="ee7f8-155">A északi fali duplikált, és hozzon létre egy **Dél fali** a módosított rendelkező átalakítási pozíció & skála.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-155">Duplicate the North wall and create a **South wall** with the updated transform position & scale.</span></span> 
   
    ![][24]

### <a name="creating-collectible-objects"></a><span data-ttu-id="ee7f8-156">A memóriából kiüríthető objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee7f8-156">Creating Collectible objects</span></span>
<span data-ttu-id="ee7f8-157">A rendszer az alábbi lépéseket a [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="ee7f8-157">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141).</span></span> <span data-ttu-id="ee7f8-158">Néhány vonzó kinézetű objektumokat, amely a memóriából kiüríthető objektumokat, amely a Player golyó objektumhoz kell a "collect" való ütközés által készletét alkotó létre fogunk hozni.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-158">We will create some attractive looking objects which will form the set of collectible objects which the Player Ball object needs to 'collect' by colliding with them.</span></span> 

1. <span data-ttu-id="ee7f8-159">Hozzon létre egy új **3D adatkocka-objektum** és nevezze el a felvételi.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-159">Create a new **3D Cube object** and name it Pickup.</span></span> 
2. <span data-ttu-id="ee7f8-160">Módosítsa a **átalakító -> Elforgatás** & **átalakítási a méretezési ->** a felvételi objektum.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-160">Adjust the **Transform -> Rotation** & **Transform -> Scale** of the Pickup object.</span></span> 
   
    ![][25]
3. <span data-ttu-id="ee7f8-161">Hozzon létre, és csatlakoztassa a **új C# parancsfájl** nevű **Rotator** felvételi objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-161">Create and attach a **new C# Script** called **Rotator** to the Pickup object.</span></span> <span data-ttu-id="ee7f8-162">Ügyeljen arra, hogy a parancsfájlt helyezze a parancsfájlok mappában.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-162">Make sure to put the script under the Scripts folder.</span></span> 
   
    ![][26]
4. <span data-ttu-id="ee7f8-163">Nyissa meg szerkesztésre a parancsfájlt, és frissítse úgy, hogy a következő:</span><span class="sxs-lookup"><span data-stu-id="ee7f8-163">Open this script for editing and update it to be the following:</span></span> 
   
        using UnityEngine;
        using System.Collections;
   
        public class Rotator : MonoBehaviour {
   
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }
5. <span data-ttu-id="ee7f8-164">A tengely elforgatása most elérte a Play mód a Unity-szerkesztő és a felvételi objektum megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-164">Now hit the Play mode in the Unity Editor and your Pickup object show be rotating on its axis.</span></span>
6. <span data-ttu-id="ee7f8-165">Hozzon létre egy új nevű **Prefabs**</span><span class="sxs-lookup"><span data-stu-id="ee7f8-165">Create a new folder called **Prefabs**</span></span> 
   
    ![][27]
7. <span data-ttu-id="ee7f8-166">Húzza a **a felvételi** objektumra, és a Prefabs mappába.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-166">Drag the **Pickup** object and put it in the Prefabs folder.</span></span>
   
    ![][28]
8. <span data-ttu-id="ee7f8-167">Hozzon létre egy új **üres játék objektum** nevű **Pickups**.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-167">Create a new **Empty Game object** called **Pickups**.</span></span> <span data-ttu-id="ee7f8-168">Pozícióját visszaáll az eredeti, és húzza a felvételi objektum játék objektum alatt.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-168">Reset its position to origin and then drag the Pickup object under this game object.</span></span>  
   
    ![][29]
9. <span data-ttu-id="ee7f8-169">Ismétlődő a **a felvételi** objektumra, és azt a terjednek a **Ground** körül objektum a **Player** objektum frissítésével a **Transform.Position tartozó X & Z** megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-169">Duplicate the **Pickup** object and spread it on the **Ground** object around the **Player** object by updating the **Transform.Position's X & Z** values appropriately.</span></span> 
   
    ![][30]
10. <span data-ttu-id="ee7f8-170">Hozzon létre egy **új anyagokkal** nevű **a felvételi** és frissítése, hogy piros szín frissítésével a **Albedo tulajdonság** hasonló mi azt az, ami az alapoktól objektum frissítésére.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-170">Create a **new material** called **Pickup** and update it to be Red in color by updating the **Albedo property** similar to what we did for updating the Ground object.</span></span> 
    
    ![][31]
11. <span data-ttu-id="ee7f8-171">Az anyag alkalmazása 4 felvételi objektumait.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-171">Apply the material to all the 4 pickup objects.</span></span>
    
    ![][32]

### <a name="collecting-the-pickup-objects"></a><span data-ttu-id="ee7f8-172">A felvételi objektumok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="ee7f8-172">Collecting the Pickup objects</span></span>
<span data-ttu-id="ee7f8-173">A rendszer az alábbi lépéseket a [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="ee7f8-173">The steps below are from the [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141).</span></span> <span data-ttu-id="ee7f8-174">A Windows Media Player frissítjük, hogy tud-e a "collect" a felvételi objektumok által való ütközés.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-174">We will update the Player so that it is able to 'collect' the pickup objects by colliding with them.</span></span> 

1. <span data-ttu-id="ee7f8-175">Nyissa meg a **PlayerController** parancsfájl a Player objektum szerkesztésre csatolja, és frissíti a következőket:</span><span class="sxs-lookup"><span data-stu-id="ee7f8-175">Open up the **PlayerController** script attached to the Player object for editing and update it to the following:</span></span>  
   
        using UnityEngine;
        using System.Collections;
   
        public class PlayerController : MonoBehaviour {
   
            public float speed;
   
            private Rigidbody rb;
   
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
   
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
   
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
   
                rb.AddForce (movement * speed);
            }
   
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }
2. <span data-ttu-id="ee7f8-176">Hozzon létre egy új **címke** nevű **válasszon mentése** (akkor meg kell egyeznie a parancsfájl a)</span><span class="sxs-lookup"><span data-stu-id="ee7f8-176">Create a new **Tag** called **Pick Up** (it must match what is in the script)</span></span>  
   
    ![][33]
   
    ![][34]
3. <span data-ttu-id="ee7f8-177">Ez alkalmazza **címke** Prefab felvételi objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-177">Apply this **Tag** to the Prefab Pickup object.</span></span> 
   
    ![][35]
4. <span data-ttu-id="ee7f8-178">Engedélyezése **IsTrigger** a Prefab objektum jelölőnégyzetét.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-178">Enable **IsTrigger** checkbox for the Prefab object.</span></span>
   
    ![][36]
5. <span data-ttu-id="ee7f8-179">Kemény törzs hozzáadni a felvételi Prefab objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-179">Add a Rigid body to Pickup Prefab object.</span></span> <span data-ttu-id="ee7f8-180">A teljesítmény optimalizálása frissítjük a statikus collider használt dinamikus collider számára.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-180">For performance optimization we will update the static collider that we used to a Dynamic collider.</span></span> 
   
    ![][37]
6. <span data-ttu-id="ee7f8-181">Végül ellenőrizze a **IsKinematic** a prefab vonatkozó tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-181">Finally check the **IsKinematic** property for the prefab object.</span></span> 
   
    ![][38]
7. <span data-ttu-id="ee7f8-182">Találati **lejátszása** a Unity a szerkesztőt, és meg fogja tudni ezt **tudják állítani egy golyó** játék azáltal, hogy az a billentyűkkel irány bemeneti Player objektum.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-182">Hit **Play** in the Unity editor and you will be able to play this **Roll a Ball** game by moving the Player object using your keyboard keys for direction input.</span></span> 

### <a name="updating-the-game-for-mobile-play"></a><span data-ttu-id="ee7f8-183">A game mobil Play frissítése</span><span class="sxs-lookup"><span data-stu-id="ee7f8-183">Updating the game for mobile play</span></span>
<span data-ttu-id="ee7f8-184">A fenti szakaszokban a Unity alapvető oktatóprogram kötött.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-184">The sections above concluded the basic tutorial from Unity.</span></span> <span data-ttu-id="ee7f8-185">Most már a játék abba, hogy mobileszköz rövid módosítja azt.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-185">Now we will modify the game to make it mobile device friendly.</span></span> <span data-ttu-id="ee7f8-186">Vegye figyelembe, hogy azt használja, amennyiben a teszteléshez játék bevitel a billentyűzetről.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-186">Note that we used keyboard input for the game so far for testing.</span></span> <span data-ttu-id="ee7f8-187">Most azt módosítja majd, hogy azt a telefonszámot, mozgásérzékelési azaz segítségével szabályozhatják a Windows Media player</span><span class="sxs-lookup"><span data-stu-id="ee7f8-187">Now we will modify it so that we can control the player by using the motion of the phone i.e.</span></span> <span data-ttu-id="ee7f8-188">a bemeneti gyorsulásmérőt használata.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-188">using Accelerometer as the input.</span></span> 

<span data-ttu-id="ee7f8-189">Nyissa meg a **PlayerController** szerkesztésre parancsfájlt, és frissítse a **FixedUpdate** a Player objektum áthelyezése a gyorsulásmérő a mozgásérzékelési használandó módszert.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-189">Open up the **PlayerController** script for editing and update the **FixedUpdate** method to use the motion from the accelerometer to move the Player object.</span></span> 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

<span data-ttu-id="ee7f8-190">Ebben az oktatóanyagban egy egyszerű játék létrehozását a Unity megjelenik, és ez a játék tetszőleges eszközön telepítheti.</span><span class="sxs-lookup"><span data-stu-id="ee7f8-190">This tutorial concludes a basic game creation with Unity and you can deploy this on a device of your choice to play the game.</span></span> 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png    
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png    
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png













