---
title: "egy szolgáltatás a piactér hello aaaGuide toocreating |} Microsoft Docs"
description: "Hogyan toocreate, hitelesíthet és az adatok szolgáltatás telepítése részletes utasításokat vásárolja meg a hello Azure piactéren."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 96194198-6991-43b4-918e-ee337e250339
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 0220d357ae0ec89e7d4f6399605850e57c646f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-service-publishing-guide-for-hello-azure-marketplace"></a>Adatok szolgáltatás közzétételi útmutatója hello Azure piactéren
> [!IMPORTANT]
> **Jelenleg dolgozunk már nem bevezetési bármely új adatszolgáltatás közzétevők. Új dataservices nem get jóváhagyott listaelem.** Ha rendelkezik a Szolgáltatottszoftver-üzleti alkalmazások toopublish szeretné a AppSource tekinthet meg további információkat [Itt](https://appsource.microsoft.com/partners). Ha egy infrastruktúra-szolgáltatási alkalmazások vagy fejlesztői szolgáltatást, akkor például az Azure piactér toopublish tekinthet meg további információkat [Itt](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

1. lépésben hello befejezése után [fiók létrehozása és regisztrálása](marketplace-publishing-accounts-creation-registration.md), azt végigvezeti meg hello [általános jellegű](marketplace-publishing-pre-requisites.md) és [műszaki követelményeiben](marketplace-publishing-data-service-creation-prerequisites.md) adatok szolgáltatás ajánlat Azure piactéren. Most végigvezetjük meg olyan adatszolgáltatás ajánlat létrehozásakor hello hello lépései [közzétételi Portáljára] [ link-pubportal] a hello Azure piactéren.

## <a name="1----login-toohello-publishing-portal"></a>1.    Bejelentkezési toohello közzétételi Portáljára.
Nyissa meg túl[https://publish.windowsazure.com](https://publish.windowsazure.com.)

**Az első alkalommal bejelentkezési tooPublishing Portal használja ugyanazt a fiókot, amellyel a vállalati értékesítő profil sikeresen regisztrálva lett fejlesztői központ hello.**  (Később is hozzáadhat a vállalat bármely alkalmazott hello közzétételi Portáljára lévő co-rendszergazdaként).

Kattintson a hello **közzététele egy Data Services** Ha ez az első bejelentkezés hello hello közzétételi portált csempéje.

## <a name="2----choose-data-services-in-hello-navigation-menu-on-hello-left-side"></a>2.    Válasszon **adatszolgáltatások** hello hello bal oldali navigációs menüjében található.
  ![rajz](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3----create-a-new-data-service"></a>3.    Egy új szolgáltatás létrehozása
Töltse ki az új adatok szolgáltatás kínálnak a hello cím, és kattintson a "+" a megfelelő hello.

  ![rajz](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4----review-hello-sub-menu-under-hello-newly-created-data-service-in-hello-navigation-menu"></a>4.    Felülvizsgálati hello almenü hello az újonnan létrehozott adatszolgáltatás hello navigációs menüjében.
Kattintson a hello **forgatókönyv** lapra, és tekintse át az összes szükséges lépéseket szükséges toopublish megfelelően hello szolgáltatás a hello Azure piactéren.

> [!TIP]
> Mindig hello hivatkozások hello "Forgatókönyv" lapon kattintson vagy hello adatszolgáltatás ajánlat almenü bal oldalán hello a lapok használatának.
> 
> 

## <a name="5----create-a-new-plan"></a>5.    Hozzon létre egy új tervet.
### <a name="offers-plans-transactions"></a>Ajánlatokat, tervek, tranzakciók.
Minden egyes ajánlat rendelkezhet több terveket, de rendelkeznie kell legalább egy (1) terv. Amikor a végfelhasználók előfizetés tooyour ajánlat előfizet egy hello ajánlat terv. Minden csomag határozza meg, hogyan történik a végfelhasználók képes toouse lehet a szolgáltatáshoz.

Jelenleg Azure piactéren csak a havi előfizetési tranzakció alapú modell támogatása adatszolgáltatások, azaz végfelhasználók hello meghatározott csomag azokat az előfizetett tooand toohello ára szerint havi díj fizet lesz képes tooconsume minden hónap száma a tranzakció hello terv által definiált.

Minden tranzakció általában definiálva, a szolgáltatás ad vissza rekordok száma a szolgáltatás toohello küldött hello lekérdezés alapján. hello alapértelmezett érték 100. A tranzakciók számának visszaadott tooeach megpróbálkozik a bejegyzések száma osztva 100 és toohello legközelebbi egész számra kerekítve.

Azure piactér szolgáltatás réteg felelősségi toomonitor (mérő) tranzakciók száma minden egyes lekérdezés által felhasznált.

> [!IMPORTANT]
> Végfelhasználók számára, amely elérte hello tranzakció hello hónap során le lesz tiltva, a havi előfizetési ciklus végéig toouse hello szolgáltatás folytatása.
> 
> hello terv vagy hello csomagok valamelyikének is (de nem kell) korlátlan számú tranzakciók közé tartoznak.
> 
> 

### <a name="create-a-plan"></a>Hozzon létre egy csomagot.
1. Kattintson a **"+"** következő toohello "hozzáadása egy új tervet".
2. Hello lehetőségek közül választhat: **korlátlan** vagy **korlátozott** a terv használata.  Ha korlátozott adja meg a tranzakció hello terv hello száma a hónap tooconsume lehetővé teszi.
   
    ![rajz](media/marketplace-publishing-data-service-creation/step-5.1.png)  
   
    Portál közzétételi is javaslatot tesz "Terv azonosítója", amely lesz használt toocommunicate toohello végfelhasználók hello hello terv a felhasználói felület hello neve és a hello piactéren szolgáltatás tooidentify hello terv által is használt. Ha azt szeretné, hello "Terv azonosítója" módosíthatja.
   
   > [!NOTE]
   > hello "Terv azonosítója" minden ajánlat hello hatókörén belül egyedinek kell lennie. Az alkalmazott sok egyéb azonosítókhoz hello közzétételi Portal megtervezése után hello első közzétételi tooproduction, és nem lesz képes toochange azonosítója lesz zárolva. Ez az azonosító.
   > 
   > 
3. Kattintson a tooaccept választását.
4. Majd kérni fogja néhány további kérdést az újonnan létrehozott terv kapcsolatban.
   
    ![rajz](media/marketplace-publishing-data-service-creation/step-5.2.png)

| Kérdés | Többszörösére. |
| --- | --- |
| **Ezt a tervet az ingyenes és rendelkezésre álló világszerte?** |Egy teljesen ingyenes-az-kell fizetni tervet is létrehozhat. Ha az hello csak tervezése – Ez az ajánlat azt jelenti, hogy közzétételekor "Szabad kínálnak" hello piactér a. Ha csak az egyik (kevés) terv, hello biztosít egy beállítás toooffer végfelhasználók toolearn további információk a szolgáltatást, amely viszonylag kis számú havonta tranzakciók.  Ha hello válasz "Yes", akkor nincs további kérdéseket kell adnia. |

> [!NOTE]
> A végfelhasználók mindig frissítheti a fizetős csomagokban toohello.
> 
> 

| Kérdés | Többszörösére. |
| --- | --- |
| **Ingyenes próbaverzió érhető el?** |"Nem próbaverzió" minden választhat, vagy adjon egy beállítás toouse a terv "Hónapig". Közzétevők például toouse Ez a beállítás tooprovide végfelhasználók hello lehetőségét toounderstand hello előnyei hello szabad hónapig kínálnak. |

> [!IMPORTANT]
> Végfelhasználók csak akkor tudja toopurchase egy ingyenes próbaverziót, ha az általuk létrehozott fizetési eszközt pl. hitelkártya, nagyvállalati szerződésben.
> 
> Az ingyenes próbaverzió hello egy hónap után Azure piactér indul el díjszabási ügyfelek hello ár hello dátum hello előfizetés, kivéve, ha hello ügyfél által kezdeményezett hello előfizetés törlése. Nincsenek különleges értesítési toohello végfelhasználók biztosítja.
> 
> 

| Kérdés | Többszörösére. |
| --- | --- |
| **Ez a séma szükséges egy promóciós kódot toopurchase?** |Közzétevők egy beállítás toolimit hozzáférés tootheir Service-csomagok révén egy különleges kódot, "A Promocode" toospecific ügyfelek nevű rendelkezik. Csak azok a végfelhasználók a Promocode amelyekről tudja toosubscribe toohello terv lesz. Ha úgy dönt, hogy a "nem", akkor beleegyezik abba, hogy rendelkezésre áll-e mindenki hello régióban, ahol hello kínálnak (lásd: [piactér Marketing Content útmutató](marketplace-publishing-push-to-staging.md) további részletekért) képes toosubscribe toothis terv lesz. Nincsenek további kérdések meg kell adnia. |
| **A terv a bárki, aki nem rendelkezik érvényes promóciós kódot is elrejthetik?** |Ha hello válasz toohello előző kérdéssel "Yes" hello Publisher rendelkezik egy beállítás toocompletely, távolítsa el a terv jelennek meg hello hello piactér felhasználói Felületét. Azt jelenti, ügyfelek nem fogják látni a terv hello ajánlat részletei lapon. Végfelhasználók számára, amely megkapja a promocode toopurchase, lesz képes toosubscribe tooit a promocode használatával. |

## <a name="6----create-your-marketplace-marketing-content"></a>6.    A tartalom marketing piactér létrehozása
Hogyan tooprovide információ szükséges a **Marketing, árazás, támogatást és kategóriák** lapok látogasson el a [piactér Marketing Content útmutató](marketplace-publishing-push-to-staging.md) Ez az hello közzétett közös tooall összetevők Az Azure piactéren.  

## <a name="7----connect-your-offer-tooyour-service-sql-azure-based-or-web-service-based"></a>7.    Csatlakoztassa az ajánlat tooyour (SQL Azure-alapú vagy webszolgáltatás-alapú) szolgáltatás.
Kattintson a hello **adatszolgáltatások** almenü.

A hello lap felső részén hello meg kell adnia tooprovide hello ajánlat **Namespace**.  

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.png)

alább kérdés hello meghatározzák, hogyan hello Publisher állapotra vált, az újonnan létrehozott tooexpose ajánlat tooAzure piactér. (További információ: hello [Data Services műszaki előfeltétel útmutató](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Adatbázis alapú közzétételi hello szolgáltatás**

Kattintson a **adatbázis**. a következő lap hello jelenik meg:

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.3.png)

toocreate hello Dataset CSDL társítás hello SQL Azure Database alapján:

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.4.png)

Majd minden táblához

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.6.png)

Ha a webszolgáltatás

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [!IMPORTANT]
> Olvasási [leképezése egy meglévő webes szolgáltatás tooOData keresztül CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) részletes útmutatásért és példákért CSDL webes szolgáltatás létrehozásához.
> 
> 

## <a name="next-steps"></a>Következő lépések
Most, hogy létrehozta az adatszolgáltatás ajánlatot, győződjön meg arról, hogy elvégezte-e hello hello utasításait [piactér Marketing Content útmutató](marketplace-publishing-push-to-staging.md) áthelyezése előtt előre túl[a szolgáltatás átmenetitesztelése](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Lásd még:
* [Első lépések: Hogyan toopublish egy ajánlat toohello Azure piactéren](marketplace-publishing-getting-started.md)
* A cikkből megtudhatja, hogyan készítheti ismertetése a hello általános OData-megfeleltetési folyamat és a cél, [szolgáltatás OData megfeleltetése](marketplace-publishing-data-service-creation-odata-mapping.md) tooreview definíciókat, és a utasításokat.
* Ha érdekli tanulási és ismertetése hello adott csomópontok és a paraméterek, olvassa el ebben a cikkben [szolgáltatás OData leképezési Adatcsomópontokat](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definíciók és magyarázatokat, példák és a nagybetűk használatát a környezetben.
* Ha érdekli példák megtekintésével, olvassa el ebben a cikkben [adatok szolgáltatás OData leképezési példák](marketplace-publishing-data-service-creation-odata-mapping-examples.md) toosee példakód és kód szintaxis és a környezetben.

[link-pubportal]:https://publish.windowsazure.com
