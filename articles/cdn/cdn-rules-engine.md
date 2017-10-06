---
title: "hello Azure CDN szabálymotor aaaOverride HTTP viselkedésének |} Microsoft Docs"
description: "hello szabálymotor lehetővé teszi a HTTP-kérések kezelésének módja Azure CDN által például blokkolja a tartalom bizonyos típusú hello kézbesítési toocustomize, a gyorsítótárazási házirend határozza meg és módosíthatja a HTTP-fejlécek."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: dd7194be9dbda43180c64568d3e1f52c5c513a7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="override-http-behavior-using-hello-azure-cdn-rules-engine"></a>Bírálja felül a HTTP viselkedés hello Azure CDN szabályok motor használata
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Áttekintés
hello szabálymotor lehetővé teszi a HTTP-kérések kezelésének módja, például tartalom bizonyos típusú hello kézbesítési blokkolja, a gyorsítótárazási házirend meghatározása és a HTTP-fejlécek módosítása toocustomize.  Ez az oktatóanyag mutatni szabályt hoz létre a CDN eszközök gyorsítótárazásának hello változik.  Nincs is videotartalom hello érhető el "[lásd még:](#see-also)" szakasz.

   > [!TIP] 
   > Egy hivatkozási toohello szintaxis részletesen, lásd: [szabályok motor hivatkozás](cdn-rules-engine-reference.md).
   > 


## <a name="tutorial"></a>Oktatóanyag
1. A CDN-profil panelje hello, kattintson a hello **kezelése** gombra.
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    Megnyílik a hello CDN felügyeleti portálon.
2. Kattintson a hello **HTTP nagy** fülre, majd **szabálymotor**.
   
    Új szabály beállításait jelennek meg.
   
    ![Új CDN-szabály beállítások](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > a több szabály listán hello rendelés van hatással, kezelésének módját. Egy későbbi szabály felülbírálhatják az előző szabályok által megadott hello műveletek.
   > 
   > 
3. Adjon meg egy nevet a hello **neve / leírása** szövegmező.
4. Azonosítsa a hello típusú kérelmek hello szabály vonatkozik.  Alapértelmezés szerint hello **mindig** egyezés feltétel meg van jelölve.  Fogjuk **mindig** ehhez az oktatóanyaghoz, így hagyja kiválasztott.
   
   ![CDN-egyeztetés feltétel](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > Nincsenek megfelelő számos különböző feltételek hello legördülő érhetők el.  Kék hello információs ikon toohello kattintva balra hello egyezés feltétel alapján meghatározható, jelenleg kijelölt hello feltétel részletesen.
   > 
   >  Hello teljes listája a feltételes kifejezések részletesen [szabályok motor feltételes kifejezések](cdn-rules-engine-reference-match-conditions.md).
   >  
   > Hello teljes listája megtalálható egyezés feltételek részletesen, [szabályok motor megfelelő feltételek](cdn-rules-engine-reference-match-conditions.md).
   > 
   > 
5. Kattintson a hello  **+**  gomb melletti túl**szolgáltatások** tooadd új szolgáltatása.  Hello legördülő hello bal oldali menüben válassza ki **kényszerített belső maximális életkora**.  Hello szövegmezőben, amely akkor jelenik meg, írja be a **300**.  Alapértelmezett értékek fennmaradó hello hagyja.
   
   ![CDN-szolgáltatás](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Mivel feltételek egyeznek, a kék hello információs ikon toohello kattintva balra hello az új szolgáltatás jelenik meg részleteket ezzel a funkcióval kapcsolatban.  Hello esetében **kényszerített belső maximális életkora**, azt mérvadóak hello eszköz **Cache-Control** és **Expires** fejlécek toocontrol hello CDN élcsomópont hello fog frissítése után eszköz hello a forrásból.  300 másodperc példában azt jelenti, hogy hello CDN élcsomópont gyorsítótárazhatják az hello eszköz 5 perccel az eredetéül hello eszköz frissítése előtt.
   > 
   > A szolgáltatások részletes hello teljes listáját, lásd: [szabályok motor a szolgáltatás részletei](cdn-rules-engine-reference-features.md).
   > 
   > 
6. Kattintson a hello **Hozzáadás** gomb toosave hello új szabályt.  Új szabály hello most jóváhagyására vár. Amennyiben jóváhagyták, hello állapota változik **függőben lévő XML** túl**aktív XML**.
   
   > [!IMPORTANT]
   > Szabályok módosítások too90 perc toopropagate hello CDN keresztül is tarthat.
   > 
   > 

## <a name="see-also"></a>Lásd még:
* [Az Azure CDN áttekintése](cdn-overview.md)
* [Szabályok motor referencia](cdn-rules-engine-reference.md)
* [Szabályok motor egyezés feltételek](cdn-rules-engine-reference-match-conditions.md)
* [Szabályok motor feltételes kifejezések](cdn-rules-engine-reference-conditional-expressions.md)
* [Szabályok adatbázismotor-szolgáltatások](cdn-rules-engine-reference-features.md)
* [Alapértelmezett HTTP működés használata hello szabályok felülbírálása](cdn-rules-engine.md)
* [Az Azure péntekenként: Az Azure CDN új Premium szolgáltatással](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (videó)