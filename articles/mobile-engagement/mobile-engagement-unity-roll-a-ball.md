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
# <a id="unity-roll-a-ball"></a>Hozzon létre Unity Roll golyó játék
Ez az oktatóanyag végigvezeti hello fő lépései kis mértékben módosított [Unity visszaállítani egy golyó oktatóanyag](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). Ez a minta játék áll hello alkalmazás felhasználói által ellenőrzött gömb "player" objektum és hello hello játék célja too'collect "ütközés hello player objektum ezeket az objektumokat a memóriából kiüríthető memóriából kiüríthető objektumok. Ez azt feltételezi, hogy a Unity-szerkesztő környezet alapszintű ismeretét. Ha probléma merül fel, toohello teljes oktatóanyag kell hivatkoznia. 

### <a name="setting-up-hello-game"></a>Hello játék beállítása
hello lépéseket tartoznak, amely hello [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Nyissa meg **Unity-szerkesztő** kattintson **új**. 
   
    ![][51] 
2. Adjon meg egy **projektnevet** & **hely**, jelölje be **3D** kattintson **Create project**.
   
    ![][52]
3. Mentés hello alapértelmezett helyszín hello új projekt részeként hello nevét az imént létrehozott **MiniGame** belül egy új  **\_színfalak** mappája **eszközök** mappába:
   
    ![][53]
4. Hozzon létre egy **3D objektum -> Vezérlősík** hello játszik mezőben, és nevezze át ezt az vezérlősík objektumot **Ground**
   
    ![][1]
5. Alaphelyzetbe állítása hello átalakító összetevője a **Ground** objektumot, hogy ez ugyanis azonos hello forrása. 
   
    ![][3]
6. Törölje a jelet **rácsvonalak megjelenítése** a **Gizmos menü** a hello **Ground** objektum.
   
    ![][4]
7. Frissítés hello **méretezési** hello a komponens **Ground** object toobe [X = 2, Y = 1, Z = 2]. 
   
    ![][5]
8. Adjon hozzá egy új **3D objektum -> kapcsolataikat** toohello projektet, és nevezze át az eddigi mint objektum **Player**. 
   
    ![][6]
9. Jelölje be hello **Player** objektumra, és kattintson a **átalakítás visszaállítása** hasonló toohello Vezérlősík objektum. 
10. Frissítés **átalakító -> pozíció -> Y koordinátáját** hello Player Y 0,5 az összetevőt.  
    
    ![][7]
11. Hozzon létre egy új nevű **anyagok** hello projektben, ahol hello jelentős toocolor hello player létre fogunk hozni. 
12. Hozzon létre egy új **anyag** nevű **háttér** ebben a mappában. 
    
    ![][8]
13. Hello anyagot hello színe frissítése hello frissítésével **Albedo** tulajdonsága azt. Kiválaszthatja a [0,32,64] hello RGB-értékeket. 
    
    ![][9]
14. Húzza az anyag hello helyszín nézet tooapply szín toohello **Ground** objektum. 
    
    ![][10]
15. Végül frissítse a hello **átalakító -> Elforgatás -> Y** too60 jobb érthetőség kedvéért bizonyos hello irányt világos objektumon. 
    
    ![][12]

### <a name="moving-hello-player"></a>Áthelyezése hello player
hello lépéseket tartoznak, amely hello [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Adja hozzá a **RigidBody** összetevő toohello **Player** objektum. 
   
    ![][13]
2. Hozzon létre egy új nevű **parancsfájlok** a projekt hello. 
3. Kattintson a **összetevőt -> Új parancsfájl -> C# parancsfájl**. Nevezze el **PlayerController**, és kattintson a **létrehozása és hozzáadása**. Ezzel a hoz létre, és csatolja a parancsfájl toohello Player objektum.  
   
    ![][14]
4. Helyezze át a parancsfájlt a hello **parancsfájlok** hello projekt mappájára. 
5. Nyissa meg szerkesztésre a kedvenc parancsprogram-szerkesztő hello parancsfájl, frissítse a következő kód hello hello parancsfájlkód, és mentse azokat. 
   
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
6. Vegye figyelembe, hogy hello parancsfájl fent használ egy **sebesség** tulajdonság. Hello Unity-szerkesztőben hello sebesség tulajdonság too10 frissítése.  
   
    ![][15]
7. Találati **lejátszása** a hello Unity-szerkesztőt. Most már tudja toocontrol hello golyó hello billentyűzet használatával kell lennie, és kell elforgatása, valamint navigálás. 

### <a name="moving-hello-camera"></a>Kamera. hello áthelyezése.
hello lépéseket tartoznak, amely hello [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) és lelassul hello **fő kamera** toohello **Player** objektum. 

1. Frissítés hello **Transform.Position** toobe X = 0, Y 10.5, Z-= 10 =.  
2. Frissítés hello **Transform.Rotation** toobe X = 45, I = 0, Z = 0.  
   
    ![][16]
3. Adja hozzá a nevű új parancsfájl **CameraController** toohello **MainCamera** és hello parancsfájlmappa alá helyezi. 
   
    ![][17]
4. Nyissa meg szerkesztésre hello parancsfájlt, és adja hozzá a következő kód azt hello:
   
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
5. A környezet Unity - húzza hello Player változó hello Player tárhely hello fő kamera objektum, hello két legyenek társítva van egy másik. 
   
    ![][18]
6. Most kattintson a Play hello Unity-szerkesztő és elforgatás hello Player golyó objektum majd látják hello következő hello adatátviteli kamera.  

### <a name="setting-up-hello-play-area"></a>Hello Play terület beállítása
hello lépéseket tartoznak, amely hello [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). Létrehozunk hello egységesen határoló hello Ground, így hello Player golyó objektum nem leadásához hello play terület mozgása. 

1. Kattintson a **létrehozás -> hozzon létre üres játék objektum ->** és adjon neki nevet **falvastagságát**
   
    ![][19]
2. Mért falvastagságát objektum - alatt hozzon létre egy új **3D objektum adatkocka ->** "Nyugati fali" néven. 
   
    ![][20]
3. Frissítés hello **átalakító pozíció ->** és **átalakítási a méretezési ->** nyugati fali objektum. 
   
    ![][21]
4. Ismétlődő hello nyugati fali toocreate egy **keleti fali** hello frissíteni az átalakítás elhelyezése és méretezése. 
   
    ![][22]
5. Ismétlődő hello keleti fali toocreate egy **északi fali** hello frissíteni az átalakítás pozíció & skála. 
   
    ![][23]
6. Ismétlődő hello északi fali, és hozzon létre egy **Dél fali** hello frissíteni az átalakítás pozíció & skála. 
   
    ![][24]

### <a name="creating-collectible-objects"></a>A memóriából kiüríthető objektumok létrehozása
hello lépéseket tartoznak, amely hello [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). Létre fogunk hozni néhány vonzó, amely kell too'collect mely hello Player golyó objektumot a memóriából kiüríthető objektumok hello készletét objektumok keresése "való ütközés által. 

1. Hozzon létre egy új **3D adatkocka-objektum** és nevezze el a felvételi. 
2. Hello beállítása **átalakító -> Elforgatás** & **átalakítási a méretezési ->** hello felvételi objektum. 
   
    ![][25]
3. Hozzon létre, és csatlakoztassa a **új C# parancsfájl** nevű **Rotator** toohello felvételi objektum. Győződjön meg arról, hogy tooput hello parancsfájl hello parancsfájlok mappában. 
   
    ![][26]
4. Nyissa meg szerkesztésre a parancsfájlt, és frissítse a következő toobe hello: 
   
        using UnityEngine;
        using System.Collections;
   
        public class Rotator : MonoBehaviour {
   
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }
5. Most találati hello Play mód hello Unity-szerkesztő és a felvételi objektum megjelenítése kell cserélni az tengelyen.
6. Hozzon létre egy új nevű **Prefabs** 
   
    ![][27]
7. A csomóponthúzási hello **a felvételi** objektumra, és hello Prefabs mappába.
   
    ![][28]
8. Hozzon létre egy új **üres játék objektum** nevű **Pickups**. A pozíció tooorigin visszaállítása, és húzza a felvételi objektum hello játék objektum alatt.  
   
    ![][29]
9. Ismétlődő hello **a felvételi** objektumra, és azt a hello terjednek **Ground** objektum körüli hello **Player** hello frissítésével objektum **Transform.Position tartozó X & Z**  megfelelő értékeket. 
   
    ![][30]
10. Hozzon létre egy **új anyagokkal** nevű **a felvételi** és frissítheti azt hello frissítésével toobe vörös színnel **Albedo tulajdonság** hasonló toowhat azt adta hello Ground frissítési objektum. 
    
    ![][31]
11. Hello jelentős tooall hello 4 felvételi objektumok alkalmazni.
    
    ![][32]

### <a name="collecting-hello-pickup-objects"></a>Hello felvételi objektumok gyűjtése
hello lépéseket tartoznak, amely hello [Unity oktatóanyag](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). Frissítjük a hello Player úgy, hogy az képes too'collect "hello felvételi objektumok által való ütközés. 

1. Nyissa meg hello **PlayerController** csatolt toohello Player objektum szerkesztésre parancsfájl és toohello a következő frissítést:  
   
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
2. Hozzon létre egy új **címke** nevű **válasszon mentése** (akkor meg kell egyeznie a hello parancsfájl)  
   
    ![][33]
   
    ![][34]
3. Ez alkalmazza **címke** toohello Prefab felvételi objektum. 
   
    ![][35]
4. Engedélyezése **IsTrigger** hello Prefab objektum jelölőnégyzetét.
   
    ![][36]
5. Adjon hozzá egy kemény tooPickup Prefab törzsobjektum. A teljesítmény optimalizálása, hogy használtuk tooa dinamikus collider hello statikus collider frissítjük. 
   
    ![][37]
6. Végül ellenőrizze hello **IsKinematic** hello prefab objektum tulajdonsága. 
   
    ![][38]
7. Találati **lejátszása** hello Unity-szerkesztőt, és nem lesz képes tooplay ez **tudják állítani egy golyó** mozgatásával játék hello a billentyűkkel irány bemeneti Player objektum. 

### <a name="updating-hello-game-for-mobile-play"></a>Game mobil Play hello frissítése
hello szakaszok alapszintű oktatóprogram kötött hello Unity fent. Most módosítja a Microsoft hello játék toomake azt mobileszköz rövid. Vegye figyelembe, hogy azt használja, amennyiben a teszteléshez játék hello bevitel a billentyűzetről. Most azt módosítja majd, hogy azt is szabályozhatja a hello player hello mozgásának hello segítségével telefonszám, azaz használatával gyorsulásmérőt hello bemeneti adatként. 

Nyissa meg hello **PlayerController** parancsfájl szerkesztése és a frissítés hello **FixedUpdate** metódus toouse hello mozgásérzékelési hello gyorsulásmérőt toomove hello Player objektumból. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Ebben az oktatóanyagban egy egyszerű játék létrehozását a Unity megjelenik, és ez a választás tooplay hello játék eszközön telepítheti. 

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













