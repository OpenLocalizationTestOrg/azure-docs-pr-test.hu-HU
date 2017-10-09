---
title: "az adatszolgáltatás hello Piactéri ajánlat aaaTesting |} Microsoft Docs"
description: "Ismerje meg, hogyan tootest a adatszolgáltatás ajánlat hello Azure piactéren."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e861bd11-f74d-4d77-b4b5-23fb463644ad
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 1b9c7027d8e0818b9bdee5cfca971bab25dd1959
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a>Az adatszolgáltatás ajánlatot átmeneti tesztelése
> [!IMPORTANT]
> **Jelenleg dolgozunk már nem bevezetési bármely új adatszolgáltatás közzétevők. Új dataservices nem get jóváhagyott listaelem.** Ha rendelkezik a Szolgáltatottszoftver-üzleti alkalmazások toopublish szeretné a AppSource tekinthet meg további információkat [Itt](https://appsource.microsoft.com/partners). Ha egy infrastruktúra-szolgáltatási alkalmazások vagy fejlesztői szolgáltatást, akkor például az Azure piactér toopublish tekinthet meg további információkat [Itt](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Hello két lépést az befejezése után [a Microsoft Developer-fiók létrehozása](marketplace-publishing-accounts-creation-registration.md) és [a szolgáltatás-ajánlatot létre közzétételi portálon](marketplace-publishing-data-service-creation.md) , hogy készen áll az ajánlatot hello elérhetővé tétele Az Azure piactéren. Ez a témakör fog végigvezetik hello először, köztes, hívott "tesztelés" lépés

Átmeneti azt jelenti, hogy az ajánlatot "védőfal", ahol tesztelése és funkciókat ellenőrzése előtt az tooproduction privát telepítése. hello ajánlat tooa felhasználói, akik már telepítették volna átmeneti fog megjelenni.

## <a name="step-1-pushing-your-offer-toostaging"></a>1. lépés Az ajánlat toostaging küldését
Az ajánlat toostaging küldését teszi lehetővé tootest hello ajánlat ahhoz, hogy elérhető toofuture előfizetők.  Láthatja, hogyan ajánlatát fog jelenik meg, és azok tooyour adatok előfizetés működik.  

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. Bejelentkezés a hello [közzétételi Portáljára](https://publish.windowsazure.com)
2. Válassza ki **adatszolgáltatások** hello bal oldali navigációs ablakban
3. Válassza ki a kívánt toopush toostaging ajánlatot. Hello képernyője felett jelenik meg.
4. Kattintson a **tooStaging leküldéses** gombra.  
5. Ha problémák vannak az hello ajánlat, amely szükséges toobe előzetes toopushing toostaging befejeződött, megjelenik egy lista jelenik meg.  Javítsa ki ezeket az elemeket hello lista összes elemére kattintva. Amikor végzett, az összes korrekció kattintson **tooStaging leküldéses** újra gombra.

Ha nincsenek a ajánlat problémák hello előugró ablakban az alábbi jelenik meg.  

Ha még nem jóváhagyott toosurface az Azure portálon ajánlatot tervezési/nem (jelenleg korlátozott kapacitás), majd az imént Bezárás hello előugró ablak.

tootest az adatok szolgáltatás az Azure portálon (a hozzáadása toohello DataMarket portál), szüksége lesz egy Azure-előfizetési Azonosítót tootest rendelkező.  Az előfizetés-azonosító hello fiók, amely azonosítja tootest ajánlatát engedélyezett.  

Kivágás, és illessze be az előfizetés-Azonosítóval, és kattintson a pipa toocontinue hello.

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> Ezek az Azure-előfizetések azonosítói csak tesztelési és átmeneti előkészítő hello a szükséges [Azure felügyeleti portálon](https://manage.windowsazure.com). Nincsenek szükséges tootest Azure piactéren.
> 
> 

hello tovább a képernyőn megjelenő látható, hogy a közzététel lefolyása "folyamatban" hello megjelenítésével ikonja kiemelve az alábbi sárga. Toostaging küldését veszi too15 10 perc között.  Ha hosszabb ideig tart, először frissítse a böngésző (nyomja le az F5 IE-ben).  Bizonyos ritkán előforduló esetekben, ahol az ajánlat még küldését toostaging egy óra elteltével hello kattintson hello lépjen kapcsolatba velünk toolet arról, hogy probléma van a hivatkozásra.

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Hello leküldéses tooStaging befejezéséről hello "folyamatban" ikonra a továbbiakban nem áthelyezése és túl "előkészített" hello állapota frissül.  Ön most már készen áll a tootest ajánlatát vannak.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>2. lépés Az előkészített ajánlatot DataMarket tesztelése
A következő hello szöveg hello hivatkozásra **"tekintse meg a szolgáltatás következő kínálnak..."** amely előfizető hello toodisplay üdvözlő képernyőt fog látni, amikor az ajánlat kerül tooproduction, és DataMarket fog megjelenni.

  ![rajz](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Tesztelje, vagy ellenőrizze hello 12 elemeket jelölte meg fent tooensure összes emblémák, árak/tranzakciók, szöveg, képek, dokumentáció, és hivatkozások helyességét, és működik megfelelően.  Ez az egy időben tooensure teszt értékeket az ajánlat létrehozásakor megadott helyett tényleges értékeket.

1. Az ajánlat embléma
2. Az ajánlat neve
3. Közzétevő neve/link tooyour vállalati webhely
4. Az ajánlat kategóriák keresése
5. Az ajánlat támogatási hivatkozás tooassist előfizetők
6. Környezetfüggő leírást az előfizetéshez
7. Az ajánlat terv végzett ügyfélellenőrzések számlázási részletek
8. Hivatkozás tooimplementation kódot
9. Minta képek, mely ajánlat adatok használata
10. Bemeneti/kimeneti metaadatok hello ajánlat belül minden szolgáltatás
11. Ajánlat használati feltételek
12. Hello ajánlat adatok megtekintése

Végül ellenőrizze hello szolgáltatás "MEGISMERKEDHET a DATASET" hello hivatkozásra kattintva hello Datamarket keresztül fog működni.  Egy új ablakban nyílik meg hello eszközben közvetlen telepítésnek "Szolgáltatás Explorer", a lekérdezés eredményeinek hello megtekintheti a szolgáltatásra.  Ebben az ablakban hello paraméterek szükséges, és tekintse meg a szolgáltatás egy lekérdezés által megjelenített hello eredményeket is megadhatja.   Megjelenik a, hello URL-címet a lekérdezésben.  

> [!NOTE]
> Lehet, hogy tooreview hello szöveges leírása hello szolgáltatás hello tetején jelenik meg.  És ha az ajánlatot több szolgáltatás hívja, hello alsó tooswitch toohello tovább szolgáltatás tooreview hello fülek kattintson, és teszteléséhez.
> 
> 

## <a name="next-step"></a>Következő lépés
Ha problémát tapasztal, és segítségre van szüksége megoldása kérdéseivel forduljon [Azure közzétevő támogatási csoportjához](http://go.microsoft.com/fwlink/?LinkId=272975).

Ha elégedett, és készen áll a toopublish ajánlatát olvassa el az hello [vonatkozó kérés jóváhagyása tooPush tooProduction](marketplace-publishing-push-to-production.md) dokumentációját.

## <a name="see-also"></a>Lásd még:
* [Első lépések: Hogyan toopublish egy ajánlat toohello Azure piactéren](marketplace-publishing-getting-started.md)

