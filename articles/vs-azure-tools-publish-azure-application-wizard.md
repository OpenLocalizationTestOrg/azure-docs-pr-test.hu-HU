---
title: "a Visual Studio Azure alkalmazás közzététele varázsló aaaUsing hello |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure hello hello Visual Studio Azure alkalmazás közzététele varázsló beállítási"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7d8f1ac9-e439-47e0-a183-0642c4ea1920
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 3f0b580d0054cc246372597660d8ae317d9b8686
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-visual-studio-publish-azure-application-wizard"></a>Hello Visual Studio Azure alkalmazás közzététele varázsló használatával
Után a Visual Studio webalkalmazás fejlesztése, közzéteheti hello segítségével adott alkalmazás tooan Azure-felhőszolgáltatásban **Azure-alkalmazás közzététele** varázsló. 

> [!NOTE]
> Ez a témakör tárgya toocloud szolgáltatások, nem tooweb helyek telepítése. Tooweb helyek telepítésével kapcsolatos információkért lásd: [hogyan tooDeploy egy Azure-webhely](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).
> 
> 

## <a name="accessing-hello-publish-azure-application-wizard"></a>Hello Azure-alkalmazás közzététele varázsló használata

Hello Azure-alkalmazás közzététele varázsló rendelkezik a Visual Studio-projekt hello típusától függően két módon érheti el.

**Ha rendelkezik egy Azure-felhőszolgáltatás-projekt:**

1. Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a projekt hello és, hello helyi menüből válassza ki a **közzététel**.

**Ha a webalkalmazás projekt, amely nincs engedélyezve az Azure-bA rendelkezik:**

1. Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.

1. A **Solution Explorer**, kattintson a jobb gombbal a projekt hello és, hello helyi menüből válassza ki a **átalakítása** > **tooAzure Felhőszolgáltatás-projekt átalakítása**. 

1. A **Megoldáskezelőben**, kattintson a jobb gombbal az újonnan létrehozott hello Azure-projekt, és hello helyi menüből válassza ki a **közzététel**.

## <a name="sign-in-page"></a>Bejelentkezési oldal

![Bejelentkezési oldal](./media/vs-azure-tools-publish-azure-application-wizard/sign-in.png)

**Fiók** – válassza ki a fiókot, vagy válasszon **vegyen fel egy fiókot** hello fiókot a legördülő listában.

**Válassza ki az előfizetés** -válassza ki az üzembe helyezéshez hello előfizetés toouse.
   
## <a name="settings-page---common-settings-tab"></a>Beállítások – Általános beállítások lap   

![Általános beállítások](./media/vs-azure-tools-publish-azure-application-wizard/settings-common-settings.png)

**A felhőalapú szolgáltatás** -hello legördülő lista használatával, vagy válasszon egy meglévő felhőalapú szolgáltatás, vagy válassza  **&lt;hozzon létre új >**, és a felhőalapú szolgáltatás létrehozása. hello adatközpont minden felhőszolgáltatás zárójelben jeleníti meg. Javasoljuk, hogy hello adatok center helye a hello felhőszolgáltatás lehet hello azonos helyként hello data center hello storage-fiók (Speciális beállítások).  

**Környezet** -válassza **éles** vagy **átmeneti**. Adja meg az alkalmazás átmeneti környezet, ha azt szeretné, hogy toodeploy hello egy tesztkörnyezetben. 

**Konfiguráció** -válassza **Debug** vagy **kiadás**.

**Szolgáltatáskonfiguráció** -válassza **felhő** vagy **helyi**.
   
**A távoli asztal engedélyezése az összes szerepkör** -ellenőrzés ezt a beállítást, ha azt szeretné, hogy toobe képes tooremotely connect toohello szolgáltatás. Ezt a beállítást elsősorban a hibaelhárításhoz. Ha bejelöli ezt a jelölőnégyzetet, hello **a távoli asztal konfigurálásának** párbeszédpanel jelenik meg. Válassza ki a hello **beállítások** hivatkozás toochange hello konfigurációs.
   
**Lehetővé teszi a Web Deploy minden webes szerepkörök** -ellenőrizze ezt a beállítást, tooenable webes hello szolgáltatás központi telepítését. Ki kell választania a hello **távoli asztal engedélyezése az összes szerepkör** toouse Ez a funkció lehetőséget. További információkért lásd: [ [közzététele a Visual Studio használatával Azure-felhőszolgáltatásban](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). 

## <a name="settings-page---advanced-settings-tab"></a>Beállítások lap - Speciális beállítások lap

![Speciális beállítások](./media/vs-azure-tools-publish-azure-application-wizard/settings-advanced-settings.png)

**Üzemelő példány címkéje** -vagy fogadja el hello alapértelmezett nevet, vagy adjon meg egy nevet a. tooappend hello dátum toohello üzemelő példány címkéje, hagyja hello jelölőnégyzet be van jelölve. 
   
**Tárfiók** – adja meg, ehhez a központi telepítéshez, a tárolási fiók toouse hello **&lt;hozzon létre új > toocreate egy tárfiókot. minden tárfiók zárójelben hello adatközpont jeleníti meg. Javasoljuk, hogy hello adatok center helye a hello tárfiók lehet hello azonos helyként hello data center hello felhőszolgáltatás (általános beállítások).  
   
hello Azure storage-fiók hello alkalmazástelepítés hello csomagot tárolja. Hello alkalmazás telepítése után hello csomag hello tárfiók törlődik.

**Törölje a központi telepítési hiba esetén** -válassza ki a beállítás toohave hello központi telepítés törölve, ha bármilyen hiba közzététele során történik. Ez nem kell bejelölni, ha azt szeretné, toomaintain állandó virtuális IP-címnek a felhőalapú szolgáltatáshoz.

**Az üzemelő példányok frissítése** – válassza ezt a beállítást, ha azt szeretné, csak a frissített összetevőket toodeploy. Lehet, hogy a központi telepítési típus gyorsabb, mint a teljes körű telepítésére. Ez ellenőrzésének, ha azt szeretné, toomaintain állandó virtuális IP-címnek a felhőalapú szolgáltatáshoz. 

**Az üzemelő példányok módosítása - beállítások** -ezen a párbeszédpanelen toofurther határozza meg, hogyan hello szerepkörök toobe frissítése. Ha úgy dönt, **növekményes frissítés**, az alkalmazás minden példánya, a frissített egy másik után, hogy adott hello alkalmazások mindig rendelkezésre. Ha úgy dönt, **egyidejű frissítése**, az alkalmazás összes példányát frissítése is hello ugyanannyi időt vesz igénybe. Egyidejű frissítése gyorsabb, de a szolgáltatás nem áll rendelkezésre hello frissítési folyamat alatt. 

![Központi telepítési beállítások](./media/vs-azure-tools-publish-azure-application-wizard/deployment-settings.png)

**IntelliTrace engedélyezése** -adja meg, ha tooenable IntelliTrace. Intellitrace bejelentkezhet a szerepkör példánya nagy mennyiségű hibakeresési adatok az Azure-ban futtatott. Ha a probléma okát toofind hello van szüksége, használhatja hello IntelliTrace-naplók toostep a kód a Visual Studio eszközből, mintha csak az Azure-ban futó. IntelliTrace használatával kapcsolatos további információkért lásd: [egy közzétett Azure felhőalapú szolgáltatás, a Visual Studio és az IntelliTrace-hibakeresés](./vs-azure-tools-intellitrace-debug-published-cloud-services.md). 

**Adatgyűjtés engedélyezése** -adja meg, ha tooenable teljesítmény adatainak összegyűjtése. hello Visual Studio Profilkészítő lehetővé teszi, hogy a tooget hello számítási aspektusait hogyan a felhőalapú szolgáltatás fut-e részletes elemzését. Hello Visual Studio profiler használatával kapcsolatos további információkért lásd: [Azure-felhőszolgáltatás hello teljesítményének tesztelése](./vs-azure-tools-performance-profiling-cloud-services.md).

**Engedélyezze a távoli hibakereső összes szerepkörök** -adja meg, ha tooenable távoli hibakeresés. A Visual Studio használatával felhőszolgáltatások hibakeresés további információkért lásd: [egy Azure-felhőszolgáltatásban vagy a virtuális gép a Visual Studio hibakeresési](./vs-azure-tools-debug-cloud-services-virtual-machines.md).

## <a name="diagnostics-settings-page"></a>Diagnosztikai beállítások lap

![Diagnosztikai beállítások](./media/vs-azure-tools-publish-azure-application-wizard/diagnostic-settings.png)

Diagnosztika lehetővé teszi az Azure felhőszolgáltatásban tootroubleshoot (vagy Azure virtuális gép). Diagnosztikai információ: [diagnosztika konfigurálása az Azure Cloud Services és a virtuális gépek](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). Application insights szolgáltatással kapcsolatos további információkért lásd: [Mi az Application Insights?](./application-insights/app-insights-overview.md).

## <a name="summary-page"></a>Összefoglaló lap

![Összefoglalás](./media/vs-azure-tools-publish-azure-application-wizard/summary.png)

**Cél profil** -választhat toocreate egy közzétételi profilt választott hello-beállítások. Létrehozhat például egy tesztkörnyezetben egy profilt és egy másikat a termelési. toosave ez profil, válassza ki a hello **mentése** ikon. hello varázsló hello-profil létrehozása, és menti a Visual Studio-projekt hello. toomodify hello-profil neve, nyissa meg hello **céloz profil** listában, és válassza a **< kezelés... >**.
   
   > [!NOTE]
   > a Visual Studio Solution Explorer hello a közzétételi profil jelenik meg, és hello-profil beállításainak írt tooa .azurePubxml kiterjesztésű fájl. XML-címkék attribútumai menti a beállításokat.
   > 
   > 

## <a name="publishing-your-application"></a>Az alkalmazás közzététele

A projekt központi telepítés összes hello beállításainak konfigurálása után válassza a **közzététel** hello hello párbeszédpanel alsó részén. Figyelheti hello folyamat állapotát a hello **kimeneti** ablak a Visual Studióban.

## <a name="next-steps"></a>Következő lépések
- [Telepítse át, és tegye közzé a webes alkalmazás tooan Azure-felhőszolgáltatásban a Visual Studio eszközből](./vs-azure-tools-migrate-publish-web-app-to-cloud-service.md)
- [Ismerje meg, hogyan toouse Visual Studio toopublish az Azure cloud service](./vs-azure-tools-publishing-a-cloud-service.md)
- [A közzétett Azure-felhőszolgáltatásban a Visual Studio és az IntelliTrace-hibakeresés](./vs-azure-tools-intellitrace-debug-published-cloud-services.md)
- [Azure-felhőszolgáltatás hello teljesítményének tesztelése](./vs-azure-tools-performance-profiling-cloud-services.md)
- [Diagnosztika konfigurálása az Azure Felhőszolgáltatások és virtuális gépek](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). 
- [Mi az Application Insights?](./application-insights/app-insights-overview.md)