---
title: "aaaUnity tudják állítani egy golyó oktatóanyag"
description: "Lépéseket toocreate hello Unity klasszikus Roll egy golyó játék, amely előzetesen szükséges összes Mobile Engagement Unity-oktatóanyag"
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
ms.openlocfilehash: 10d923682432961207594886b08e5db60cf60d9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="f164a-103"><a id="unity-roll-a-ball"></a>Hozzon létre Unity Roll golyó játék</span><span class="sxs-lookup"><span data-stu-id="f164a-103"><a id="unity-roll-a-ball"></a>Create Unity Roll a Ball game</span></span>
<span data-ttu-id="f164a-104">Ez az oktatóanyag végigvezeti hello fő lépései kis mértékben módosított [Unity visszaállítani egy golyó oktatóanyag](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial).</span><span class="sxs-lookup"><span data-stu-id="f164a-104">This tutorial walks through hello main steps for a slightly modified [Unity Roll a Ball tutorial](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial).</span></span> <span data-ttu-id="f164a-105">Ez a minta játék áll hello alkalmazás felhasználói által ellenőrzött gömb "player" objektum és hello hello játék célja too'collect "ütközés hello player objektum ezeket az objektumokat a memóriából kiüríthető memóriából kiüríthető objektumok.</span><span class="sxs-lookup"><span data-stu-id="f164a-105">This sample game consists of a spherical 'player' object which is controlled by hello app user and hello objective of hello game is too'collect' collectible objects by colliding hello player object with these collectible objects.</span></span> <span data-ttu-id="f164a-106">Ez azt feltételezi, hogy a Unity-szerkesztő környezet alapszintű ismeretét.</span><span class="sxs-lookup"><span data-stu-id="f164a-106">This assumes basic familiarity with Unity editor environment.</span></span> <span data-ttu-id="f164a-107">Ha probléma merül fel, toohello teljes oktatóanyag kell hivatkoznia.</span><span class="sxs-lookup"><span data-stu-id="f164a-107">If you run into any issues then you should refer toohello full tutorial.</span></span> 

### <a name="setting-up-hello-game"></a><span data-ttu-id="f164a-108">Hello játék beállítása</span><span class="sxs-lookup"><span data-stu-id="f164a-108">Setting up hello game</span></span>
<span data-ttu-id="f164a-109">hello lépéseket tartoznak, amely hello [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)</span><span class="sxs-lookup"><span data-stu-id="f164a-109">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)</span></span>

1. <span data-ttu-id="f164a-110">Nyissa meg **Unity-szerkesztő** kattintson **új**.</span><span class="sxs-lookup"><span data-stu-id="f164a-110">Open **Unity Editor** and click **New**.</span></span> 
   
    ![][51] 
2. <span data-ttu-id="f164a-111">Adjon meg egy **projektnevet** & **hely**, jelölje be **3D** kattintson **Create project**.</span><span class="sxs-lookup"><span data-stu-id="f164a-111">Provide a **Project name** & **Location**, select **3D** and click **Create project**.</span></span>
   
    ![][52]
3. <span data-ttu-id="f164a-112">Mentés hello alapértelmezett helyszín hello új projekt részeként hello nevét az imént létrehozott **MiniGame** belül egy új  **\_színfalak** mappája **eszközök** mappába:</span><span class="sxs-lookup"><span data-stu-id="f164a-112">Save hello default scene just created as part of hello new project as with hello name **MiniGame** within a new **\_Scenes** folder under **Assets** folder:</span></span>
   
    ![][53]
4. <span data-ttu-id="f164a-113">Hozzon létre egy **3D objektum -> Vezérlősík** hello játszik mezőben, és nevezze át ezt az vezérlősík objektumot **Ground**</span><span class="sxs-lookup"><span data-stu-id="f164a-113">Create a **3D Object -> Plane** as hello playing field and rename this plane object as **Ground**</span></span>
   
    ![][1]
5. <span data-ttu-id="f164a-114">Alaphelyzetbe állítása hello átalakító összetevője a **Ground** objektumot, hogy ez ugyanis azonos hello forrása.</span><span class="sxs-lookup"><span data-stu-id="f164a-114">Reset hello transform component for this **Ground** object so that it is at hello Origin.</span></span> 
   
    ![][3]
6. <span data-ttu-id="f164a-115">Törölje a jelet **rácsvonalak megjelenítése** a **Gizmos menü** a hello **Ground** objektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-115">Uncheck **Show Grid** from **Gizmos menu** for hello **Ground** object.</span></span>
   
    ![][4]
7. <span data-ttu-id="f164a-116">Frissítés hello **méretezési** hello a komponens **Ground** object toobe [X = 2, Y = 1, Z = 2].</span><span class="sxs-lookup"><span data-stu-id="f164a-116">Update hello **Scale** component for hello **Ground** object toobe [X = 2,Y = 1, Z = 2].</span></span> 
   
    ![][5]
8. <span data-ttu-id="f164a-117">Adjon hozzá egy új **3D objektum -> kapcsolataikat** toohello projektet, és nevezze át az eddigi mint objektum **Player**.</span><span class="sxs-lookup"><span data-stu-id="f164a-117">Add a new **3D Object -> Sphere** toohello project and rename this sphere object as **Player**.</span></span> 
   
    ![][6]
9. <span data-ttu-id="f164a-118">Jelölje be hello **Player** objektumra, és kattintson a **átalakítás visszaállítása** hasonló toohello Vezérlősík objektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-118">Select hello **Player** object and click **Reset Transform** similar toohello Plane object.</span></span> 
10. <span data-ttu-id="f164a-119">Frissítés **átalakító -> pozíció -> Y koordinátáját** hello Player Y 0,5 az összetevőt.</span><span class="sxs-lookup"><span data-stu-id="f164a-119">Update **Transform -> Position -> Y Coordinate** component for hello Player Y as 0.5.</span></span>  
    
    ![][7]
11. <span data-ttu-id="f164a-120">Hozzon létre egy új nevű **anyagok** hello projektben, ahol hello jelentős toocolor hello player létre fogunk hozni.</span><span class="sxs-lookup"><span data-stu-id="f164a-120">Create a new folder called **Materials** in hello project where we will create hello material toocolor hello player.</span></span> 
12. <span data-ttu-id="f164a-121">Hozzon létre egy új **anyag** nevű **háttér** ebben a mappában.</span><span class="sxs-lookup"><span data-stu-id="f164a-121">Create a new **Material** called **Background** in this folder.</span></span> 
    
    ![][8]
13. <span data-ttu-id="f164a-122">Hello anyagot hello színe frissítése hello frissítésével **Albedo** tulajdonsága azt.</span><span class="sxs-lookup"><span data-stu-id="f164a-122">Update hello color of hello material by updating hello **Albedo** property of it.</span></span> <span data-ttu-id="f164a-123">Kiválaszthatja a [0,32,64] hello RGB-értékeket.</span><span class="sxs-lookup"><span data-stu-id="f164a-123">You can select hello RGB values of [0,32,64].</span></span> 
    
    ![][9]
14. <span data-ttu-id="f164a-124">Húzza az anyag hello helyszín nézet tooapply szín toohello **Ground** objektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-124">Drag this material into hello scene view tooapply color toohello **Ground** object.</span></span> 
    
    ![][10]
15. <span data-ttu-id="f164a-125">Végül frissítse a hello **átalakító -> Elforgatás -> Y** too60 jobb érthetőség kedvéért bizonyos hello irányt világos objektumon.</span><span class="sxs-lookup"><span data-stu-id="f164a-125">Finally update hello **Transform -> Rotation -> Y** too60 on hello Directional Light object for clarity.</span></span> 
    
    ![][12]

### <a name="moving-hello-player"></a><span data-ttu-id="f164a-126">Áthelyezése hello player</span><span class="sxs-lookup"><span data-stu-id="f164a-126">Moving hello player</span></span>
<span data-ttu-id="f164a-127">hello lépéseket tartoznak, amely hello [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)</span><span class="sxs-lookup"><span data-stu-id="f164a-127">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)</span></span>

1. <span data-ttu-id="f164a-128">Adja hozzá a **RigidBody** összetevő toohello **Player** objektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-128">Add a **RigidBody** component toohello **Player** object.</span></span> 
   
    ![][13]
2. <span data-ttu-id="f164a-129">Hozzon létre egy új nevű **parancsfájlok** a projekt hello.</span><span class="sxs-lookup"><span data-stu-id="f164a-129">Create a new folder called **Scripts** in hello Project.</span></span> 
3. <span data-ttu-id="f164a-130">Kattintson a **összetevőt -> Új parancsfájl -> C# parancsfájl**.</span><span class="sxs-lookup"><span data-stu-id="f164a-130">Click **Add Component-> New Script -> C# Script**.</span></span> <span data-ttu-id="f164a-131">Nevezze el **PlayerController**, és kattintson a **létrehozása és hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="f164a-131">Name it **PlayerController**, and click **Create and Add**.</span></span> <span data-ttu-id="f164a-132">Ezzel a hoz létre, és csatolja a parancsfájl toohello Player objektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-132">This will create and attach a script toohello Player object.</span></span>  
   
    ![][14]
4. <span data-ttu-id="f164a-133">Helyezze át a parancsfájlt a hello **parancsfájlok** hello projekt mappájára.</span><span class="sxs-lookup"><span data-stu-id="f164a-133">Move this script under hello **Scripts** folder in hello project.</span></span> 
5. <span data-ttu-id="f164a-134">Nyissa meg szerkesztésre a kedvenc parancsprogram-szerkesztő hello parancsfájl, frissítse a következő kód hello hello parancsfájlkód, és mentse azokat.</span><span class="sxs-lookup"><span data-stu-id="f164a-134">Open hello script for editing in your favorite script editor, update hello script code with hello following code and save it.</span></span> 
   
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
6. <span data-ttu-id="f164a-135">Vegye figyelembe, hogy hello parancsfájl fent használ egy **sebesség** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="f164a-135">Note that hello script above uses a **Speed** property.</span></span> <span data-ttu-id="f164a-136">Hello Unity-szerkesztőben hello sebesség tulajdonság too10 frissítése.</span><span class="sxs-lookup"><span data-stu-id="f164a-136">In hello Unity editor, update hello speed property too10.</span></span>  
   
    ![][15]
7. <span data-ttu-id="f164a-137">Találati **lejátszása** a hello Unity-szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="f164a-137">Hit **Play** in hello Unity Editor.</span></span> <span data-ttu-id="f164a-138">Most már tudja toocontrol hello golyó hello billentyűzet használatával kell lennie, és kell elforgatása, valamint navigálás.</span><span class="sxs-lookup"><span data-stu-id="f164a-138">Now you should be able toocontrol hello ball using hello keyboard and it should rotate and move around.</span></span> 

### <a name="moving-hello-camera"></a><span data-ttu-id="f164a-139">Kamera. hello áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="f164a-139">Moving hello camera</span></span>
<span data-ttu-id="f164a-140">hello lépéseket tartoznak, amely hello [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) és lelassul hello **fő kamera** toohello **Player** objektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-140">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) and will tie hello **Main Camera** toohello **Player** object.</span></span> 

1. <span data-ttu-id="f164a-141">Frissítés hello **Transform.Position** toobe X = 0, Y 10.5, Z-= 10 =.</span><span class="sxs-lookup"><span data-stu-id="f164a-141">Update hello **Transform.Position** toobe X = 0,  Y = 10.5, Z=-10.</span></span>  
2. <span data-ttu-id="f164a-142">Frissítés hello **Transform.Rotation** toobe X = 45, I = 0, Z = 0.</span><span class="sxs-lookup"><span data-stu-id="f164a-142">Update hello **Transform.Rotation** toobe X = 45, Y = 0, Z = 0.</span></span>  
   
    ![][16]
3. <span data-ttu-id="f164a-143">Adja hozzá a nevű új parancsfájl **CameraController** toohello **MainCamera** és hello parancsfájlmappa alá helyezi.</span><span class="sxs-lookup"><span data-stu-id="f164a-143">Add a new script called **CameraController** toohello **MainCamera** and move it under hello Scripts folder.</span></span> 
   
    ![][17]
4. <span data-ttu-id="f164a-144">Nyissa meg szerkesztésre hello parancsfájlt, és adja hozzá a következő kód azt hello:</span><span class="sxs-lookup"><span data-stu-id="f164a-144">Open up hello script for editing and add hello following code in it:</span></span>
   
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
5. <span data-ttu-id="f164a-145">A környezet Unity - húzza hello Player változó hello Player tárhely hello fő kamera objektum, hello két legyenek társítva van egy másik.</span><span class="sxs-lookup"><span data-stu-id="f164a-145">In Unity environment - drag hello Player variable into hello Player slot for hello Main Camera object so that hello two are associated with one another.</span></span> 
   
    ![][18]
6. <span data-ttu-id="f164a-146">Most kattintson a Play hello Unity-szerkesztő és elforgatás hello Player golyó objektum majd látják hello következő hello adatátviteli kamera.</span><span class="sxs-lookup"><span data-stu-id="f164a-146">Now if you hit Play in hello Unity editor and rotate hello Player Ball object then you will see hello Camera following it in hello movement.</span></span>  

### <a name="setting-up-hello-play-area"></a><span data-ttu-id="f164a-147">Hello Play terület beállítása</span><span class="sxs-lookup"><span data-stu-id="f164a-147">Setting up hello Play area</span></span>
<span data-ttu-id="f164a-148">hello lépéseket tartoznak, amely hello [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="f164a-148">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141).</span></span> <span data-ttu-id="f164a-149">Létrehozunk hello egységesen határoló hello Ground, így hello Player golyó objektum nem leadásához hello play terület mozgása.</span><span class="sxs-lookup"><span data-stu-id="f164a-149">We will create hello Walls surrounding hello Ground so that hello Player Ball object doesn't drop off hello play area in its movement.</span></span> 

1. <span data-ttu-id="f164a-150">Kattintson a **létrehozás -> hozzon létre üres játék objektum ->** és adjon neki nevet **falvastagságát**</span><span class="sxs-lookup"><span data-stu-id="f164a-150">Click **Create -> Create Empty -> Game Object** and name it **Walls**</span></span>
   
    ![][19]
2. <span data-ttu-id="f164a-151">Mért falvastagságát objektum - alatt hozzon létre egy új **3D objektum adatkocka ->** "Nyugati fali" néven.</span><span class="sxs-lookup"><span data-stu-id="f164a-151">Under this Walls object - create a new **3D Object -> Cube** and name it "West wall".</span></span> 
   
    ![][20]
3. <span data-ttu-id="f164a-152">Frissítés hello **átalakító pozíció ->** és **átalakítási a méretezési ->** nyugati fali objektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-152">Update hello **Transform -> Position** and **Transform -> Scale** for this West Wall object.</span></span> 
   
    ![][21]
4. <span data-ttu-id="f164a-153">Ismétlődő hello nyugati fali toocreate egy **keleti fali** hello frissíteni az átalakítás elhelyezése és méretezése.</span><span class="sxs-lookup"><span data-stu-id="f164a-153">Duplicate hello West wall toocreate an **East wall** with hello updated transform position and scale.</span></span> 
   
    ![][22]
5. <span data-ttu-id="f164a-154">Ismétlődő hello keleti fali toocreate egy **északi fali** hello frissíteni az átalakítás pozíció & skála.</span><span class="sxs-lookup"><span data-stu-id="f164a-154">Duplicate hello East wall toocreate a **North wall** with hello updated transform position & scale.</span></span> 
   
    ![][23]
6. <span data-ttu-id="f164a-155">Ismétlődő hello északi fali, és hozzon létre egy **Dél fali** hello frissíteni az átalakítás pozíció & skála.</span><span class="sxs-lookup"><span data-stu-id="f164a-155">Duplicate hello North wall and create a **South wall** with hello updated transform position & scale.</span></span> 
   
    ![][24]

### <a name="creating-collectible-objects"></a><span data-ttu-id="f164a-156">A memóriából kiüríthető objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="f164a-156">Creating Collectible objects</span></span>
<span data-ttu-id="f164a-157">hello lépéseket tartoznak, amely hello [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="f164a-157">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141).</span></span> <span data-ttu-id="f164a-158">Létre fogunk hozni néhány vonzó, amely kell too'collect mely hello Player golyó objektumot a memóriából kiüríthető objektumok hello készletét objektumok keresése "való ütközés által.</span><span class="sxs-lookup"><span data-stu-id="f164a-158">We will create some attractive looking objects which will form hello set of collectible objects which hello Player Ball object needs too'collect' by colliding with them.</span></span> 

1. <span data-ttu-id="f164a-159">Hozzon létre egy új **3D adatkocka-objektum** és nevezze el a felvételi.</span><span class="sxs-lookup"><span data-stu-id="f164a-159">Create a new **3D Cube object** and name it Pickup.</span></span> 
2. <span data-ttu-id="f164a-160">Hello beállítása **átalakító -> Elforgatás** & **átalakítási a méretezési ->** hello felvételi objektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-160">Adjust hello **Transform -> Rotation** & **Transform -> Scale** of hello Pickup object.</span></span> 
   
    ![][25]
3. <span data-ttu-id="f164a-161">Hozzon létre, és csatlakoztassa a **új C# parancsfájl** nevű **Rotator** toohello felvételi objektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-161">Create and attach a **new C# Script** called **Rotator** toohello Pickup object.</span></span> <span data-ttu-id="f164a-162">Győződjön meg arról, hogy tooput hello parancsfájl hello parancsfájlok mappában.</span><span class="sxs-lookup"><span data-stu-id="f164a-162">Make sure tooput hello script under hello Scripts folder.</span></span> 
   
    ![][26]
4. <span data-ttu-id="f164a-163">Nyissa meg szerkesztésre a parancsfájlt, és frissítse a következő toobe hello:</span><span class="sxs-lookup"><span data-stu-id="f164a-163">Open this script for editing and update it toobe hello following:</span></span> 
   
        using UnityEngine;
        using System.Collections;
   
        public class Rotator : MonoBehaviour {
   
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }
5. <span data-ttu-id="f164a-164">Most találati hello Play mód hello Unity-szerkesztő és a felvételi objektum megjelenítése kell cserélni az tengelyen.</span><span class="sxs-lookup"><span data-stu-id="f164a-164">Now hit hello Play mode in hello Unity Editor and your Pickup object show be rotating on its axis.</span></span>
6. <span data-ttu-id="f164a-165">Hozzon létre egy új nevű **Prefabs**</span><span class="sxs-lookup"><span data-stu-id="f164a-165">Create a new folder called **Prefabs**</span></span> 
   
    ![][27]
7. <span data-ttu-id="f164a-166">A csomóponthúzási hello **a felvételi** objektumra, és hello Prefabs mappába.</span><span class="sxs-lookup"><span data-stu-id="f164a-166">Drag hello **Pickup** object and put it in hello Prefabs folder.</span></span>
   
    ![][28]
8. <span data-ttu-id="f164a-167">Hozzon létre egy új **üres játék objektum** nevű **Pickups**.</span><span class="sxs-lookup"><span data-stu-id="f164a-167">Create a new **Empty Game object** called **Pickups**.</span></span> <span data-ttu-id="f164a-168">A pozíció tooorigin visszaállítása, és húzza a felvételi objektum hello játék objektum alatt.</span><span class="sxs-lookup"><span data-stu-id="f164a-168">Reset its position tooorigin and then drag hello Pickup object under this game object.</span></span>  
   
    ![][29]
9. <span data-ttu-id="f164a-169">Ismétlődő hello **a felvételi** objektumra, és azt a hello terjednek **Ground** objektum körüli hello **Player** hello frissítésével objektum **Transform.Position tartozó X & Z**  megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="f164a-169">Duplicate hello **Pickup** object and spread it on hello **Ground** object around hello **Player** object by updating hello **Transform.Position's X & Z** values appropriately.</span></span> 
   
    ![][30]
10. <span data-ttu-id="f164a-170">Hozzon létre egy **új anyagokkal** nevű **a felvételi** és frissítheti azt hello frissítésével toobe vörös színnel **Albedo tulajdonság** hasonló toowhat azt adta hello Ground frissítési objektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-170">Create a **new material** called **Pickup** and update it toobe Red in color by updating hello **Albedo property** similar toowhat we did for updating hello Ground object.</span></span> 
    
    ![][31]
11. <span data-ttu-id="f164a-171">Hello jelentős tooall hello 4 felvételi objektumok alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="f164a-171">Apply hello material tooall hello 4 pickup objects.</span></span>
    
    ![][32]

### <a name="collecting-hello-pickup-objects"></a><span data-ttu-id="f164a-172">Hello felvételi objektumok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="f164a-172">Collecting hello Pickup objects</span></span>
<span data-ttu-id="f164a-173">hello lépéseket tartoznak, amely hello [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141).</span><span class="sxs-lookup"><span data-stu-id="f164a-173">hello steps below are from hello [Unity tutorial](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141).</span></span> <span data-ttu-id="f164a-174">Frissítjük a hello Player úgy, hogy az képes too'collect "hello felvételi objektumok által való ütközés.</span><span class="sxs-lookup"><span data-stu-id="f164a-174">We will update hello Player so that it is able too'collect' hello pickup objects by colliding with them.</span></span> 

1. <span data-ttu-id="f164a-175">Nyissa meg hello **PlayerController** csatolt toohello Player objektum szerkesztésre parancsfájl és toohello a következő frissítést:</span><span class="sxs-lookup"><span data-stu-id="f164a-175">Open up hello **PlayerController** script attached toohello Player object for editing and update it toohello following:</span></span>  
   
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
2. <span data-ttu-id="f164a-176">Hozzon létre egy új **címke** nevű **válasszon mentése** (akkor meg kell egyeznie a hello parancsfájl)</span><span class="sxs-lookup"><span data-stu-id="f164a-176">Create a new **Tag** called **Pick Up** (it must match what is in hello script)</span></span>  
   
    ![][33]
   
    ![][34]
3. <span data-ttu-id="f164a-177">Ez alkalmazza **címke** toohello Prefab felvételi objektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-177">Apply this **Tag** toohello Prefab Pickup object.</span></span> 
   
    ![][35]
4. <span data-ttu-id="f164a-178">Engedélyezése **IsTrigger** hello Prefab objektum jelölőnégyzetét.</span><span class="sxs-lookup"><span data-stu-id="f164a-178">Enable **IsTrigger** checkbox for hello Prefab object.</span></span>
   
    ![][36]
5. <span data-ttu-id="f164a-179">Adjon hozzá egy kemény tooPickup Prefab törzsobjektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-179">Add a Rigid body tooPickup Prefab object.</span></span> <span data-ttu-id="f164a-180">A teljesítmény optimalizálása, hogy használtuk tooa dinamikus collider hello statikus collider frissítjük.</span><span class="sxs-lookup"><span data-stu-id="f164a-180">For performance optimization we will update hello static collider that we used tooa Dynamic collider.</span></span> 
   
    ![][37]
6. <span data-ttu-id="f164a-181">Végül ellenőrizze hello **IsKinematic** hello prefab objektum tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="f164a-181">Finally check hello **IsKinematic** property for hello prefab object.</span></span> 
   
    ![][38]
7. <span data-ttu-id="f164a-182">Találati **lejátszása** hello Unity-szerkesztőt, és nem lesz képes tooplay ez **tudják állítani egy golyó** mozgatásával játék hello a billentyűkkel irány bemeneti Player objektum.</span><span class="sxs-lookup"><span data-stu-id="f164a-182">Hit **Play** in hello Unity editor and you will be able tooplay this **Roll a Ball** game by moving hello Player object using your keyboard keys for direction input.</span></span> 

### <a name="updating-hello-game-for-mobile-play"></a><span data-ttu-id="f164a-183">Game mobil Play hello frissítése</span><span class="sxs-lookup"><span data-stu-id="f164a-183">Updating hello game for mobile play</span></span>
<span data-ttu-id="f164a-184">hello szakaszok alapszintű oktatóprogram kötött hello Unity fent.</span><span class="sxs-lookup"><span data-stu-id="f164a-184">hello sections above concluded hello basic tutorial from Unity.</span></span> <span data-ttu-id="f164a-185">Most módosítja a Microsoft hello játék toomake azt mobileszköz rövid.</span><span class="sxs-lookup"><span data-stu-id="f164a-185">Now we will modify hello game toomake it mobile device friendly.</span></span> <span data-ttu-id="f164a-186">Vegye figyelembe, hogy azt használja, amennyiben a teszteléshez játék hello bevitel a billentyűzetről.</span><span class="sxs-lookup"><span data-stu-id="f164a-186">Note that we used keyboard input for hello game so far for testing.</span></span> <span data-ttu-id="f164a-187">Most azt módosítja majd, hogy azt is szabályozhatja a hello player hello mozgásának hello segítségével telefonszám, azaz használatával gyorsulásmérőt hello bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="f164a-187">Now we will modify it so that we can control hello player by using hello motion of hello phone i.e. using Accelerometer as hello input.</span></span> 

<span data-ttu-id="f164a-188">Nyissa meg hello **PlayerController** parancsfájl szerkesztése és a frissítés hello **FixedUpdate** metódus toouse hello mozgásérzékelési hello gyorsulásmérőt toomove hello Player objektumból.</span><span class="sxs-lookup"><span data-stu-id="f164a-188">Open up hello **PlayerController** script for editing and update hello **FixedUpdate** method toouse hello motion from hello accelerometer toomove hello Player object.</span></span> 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

<span data-ttu-id="f164a-189">Ebben az oktatóanyagban egy egyszerű játék létrehozását a Unity megjelenik, és ez a választás tooplay hello játék eszközön telepítheti.</span><span class="sxs-lookup"><span data-stu-id="f164a-189">This tutorial concludes a basic game creation with Unity and you can deploy this on a device of your choice tooplay hello game.</span></span> 

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













