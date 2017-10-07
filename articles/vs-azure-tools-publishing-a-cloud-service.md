---
title: "egy felhőalapú szolgáltatás hello Azure eszközökkel aaaPublishing |} Microsoft Docs"
description: "További információk a hogyan toopublish Azure felhő alapú szolgáltatás projektek Visual Studio használatával."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1a07b6e4-3678-4cbf-b37e-4520b402a3d9
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/14/2017
ms.author: kraigb
ms.openlocfilehash: 31ede8308146de2bb128b768f23f64eb85bc7548
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publishing-a-cloud-service-using-hello-azure-tools"></a>Egy felhőalapú szolgáltatás hello Azure eszközökkel közzététele
Hello Azure eszközök a Microsoft Visual Studio használatával, a Visual Studio-ről Azure alkalmazást is közzétehet. A Visual Studio támogatja az integrált tooeither közzétételi átmeneti hello vagy hello felhőalapú szolgáltatások éles környezetben.

Az Azure-alkalmazások közzététele előtt Azure-előfizetéssel kell rendelkeznie. Kell állítania egy felhőalapú szolgáltatás és a tárolási fiók toobe az alkalmazás által használt fel. Beállíthatja a következő hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!IMPORTANT]
> Ha közzéteszi, hello környezet hajthatók végre a felhőalapú szolgáltatáshoz. Is választania kell egy tárfiókot, amely használt toostore hello a telepítéshez alkalmazáscsomagot. A központi telepítést követően hello alkalmazáscsomag hello tárfiók törlődik.
> 
> 

Fejlesztés és tesztelés az Azure-alkalmazások, ha használható a Web Deploy toopublish módosítások Növekményesen a webes szerepkörök. Miutá közzéteszi az alkalmazás tooa környezet, a Web Deploy segítségével telepíthet módosítások közvetlen toohello virtuális gépet futtató hello webes szerepkör. Nem rendelkezik toopackage, és tegye közzé a teljes Azure alkalmazást minden egyes alkalommal, amikor szeretné tooupdate a webes szerepkör tootest hello módosításokat. Ezt a módszert lehet hello felhő nélkül várakozási toohave tesztelési a alkalmazás közzétett tooa telepítési környezet a webes szerepkör módosítások érhető el.

A következő eljárások toopublish hello használata az Azure alkalmazás és a webes szerepkör tooupdate Web Deploy használatával:

* Közzététel vagy a csomag a Visual Studio Azure-alkalmazásfejlesztő
* A webes szerepkör hello fejlesztési és tesztelési ciklus részeként frissítése

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Közzététel vagy a csomag az Azure-alkalmazások Visual studióból
Az Azure-alkalmazásában közzétételekor hello a következő tevékenységek egyikét teheti:

* Service-csomag létrehozása: használhatja a csomag- és hello szolgáltatás konfigurációs fájl toopublish a alkalmazás tooa telepítési környezetben hello [klasszikus Azure portál](http://go.microsoft.com/fwlink/?LinkID=213885).
* Az Azure a Visual Studio-projekt közzététele: toopublish az alkalmazás közvetlen tooAzure, használhat hello közzététele varázsló. További információ: [Azure alkalmazás közzététele varázsló](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="toocreate-a-service-package-from-visual-studio"></a>a Visual Studio szolgáltatás csomag toocreate
1. Amikor készen áll a toopublish az alkalmazásról, nyissa meg a Megoldáskezelőben, nyissa meg hello helyi menüje hello Azure-projekt, amely tartalmazza a szerepkörök, és válassza a Publish.
2. a service-csomag toocreate csak, kövesse az alábbi lépéseket:  
   
   1. A helyi menü hello hello Azure projektre, válassza a **csomag**.
   2. A hello **Azure alkalmazás becsomagolása** párbeszédpanelen válassza ki a kívánt toocreate hello szolgáltatás konfigurációs csomagot, és válassza a hello build konfigurációját.
   3. a távoli asztal (nem kötelező) tooturn hello felhőszolgáltatás közzététel után, jelölje be hello **összes szerepkör távoli asztal engedélyezése** jelölje be a jelölőnégyzetet, majd válassza ki **beállítások** tooconfigure távoli asztal. Ha toodebug a felhőalapú szolgáltatás közzététel után, akkor kapcsolja be a távoli hibakeresés kiválasztásával **távoli hibakereső engedélyezése az összes szerepkör**.
      
      További információkért lásd: [a távoli asztal Azure szerepkörök](vs-azure-tools-remote-desktop-roles.md).
   4. toocreate hello csomag, válassza ki a hello **csomag** hivatkozásra.
      
      A Fájlkezelőben hello fájl helye az újonnan létrehozott csomagot hello jeleníti meg. Erre a helyre másolhatja, hogy használhassa a hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).
   5. toopublish a csomag tooa környezet kell használnia ezen a helyen hello csomaghely felhőalapú szolgáltatás létrehozása és központi telepítésekor a csomag tooan környezet hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).
3. (Választható) toocancel hello a központi telepítési folyamat hello sor elem hello műveletnaplóban, hello helyi menüjében válassza a **megszakítás és Eltávolítás**. Ezzel a hello telepítési folyamat leáll, és törli a hello környezet az Azure-ból.
   
   > [!NOTE]
   > azt követően a központi telepítési környezet tooremove le lett telepítve, hello kell használnia [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).
   > 
   > 
4. (Választható) Miután elindította a szerepkörpéldányok, a Visual Studio automatikusan megjeleníti hello környezet a hello **Felhőszolgáltatások** a Server Explorer csomópont. Itt láthatja a hello hello egyéni szerepkörpéldányok állapotát. Lásd: [erőforrások kezelése Azure Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md).hello a következő ábra mutatja hello szerepkörpéldányokat során továbbra is hello inicializálás állapota:
   
    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-hello-development-and-testing-cycle"></a>A webes szerepkör ciklus tesztelése és hello fejlesztési részét képező frissítése
Ha az alkalmazás háttér-infrastruktúra stabil, de hello webes szerepkörök ki kell gyakoribb, frissítése, a Web Deploy tooupdate csak a webes szerepkör a projekt is használhatja. Ez az hasznos, ha nem szeretné, hogy toorebuild és hello háttér feldolgozói szerepkörök telepítse újra, vagy ha még több webes szerepkör, és azt szeretné, hogy csak az egyik hello webes szerepkörök tooupdate.

### <a name="requirements"></a>Követelmények
Az alábbiakban hello követelmények toouse Web Deploy tooupdate a webes szerepkör:

* **A fejlesztéshez és teszteléshez csak jellegű:** hello módosításai közvetlenül toohello virtuális gép hello webes szerepkört futtató. Ha a virtuális gép toobe felhasználását, hello módosítások elvesznek mert hello eredeti csomag közzétett használt toorecreate hello virtuális gép hello szerepkörhöz. Újra tegye közzé az alkalmazást tooget hello legutóbbi változtatásokat hello webes szerepkör.
* **Csak a webes szerepkörök frissíthető:** feldolgozói szerepkörök nem lehet frissíteni. Emellett a webes role.cs RoleEntryPoint hello nem frissíthető.
* **Csak egyet támogat a webes szerepkör egyetlen példányát:** bármely webes szerepkör több példánya a központi telepítési környezetben nem lehet. Azonban több webes szerepkör minden csak egy példány támogatott.
* **Engedélyeznie kell a távoli asztali kapcsolatok:** Erre azért szükség, így a Web Deploy hello felhasználói és a jelszó tooconnect toohello virtuális gép toodeploy hello módosítások toohello futtató kiszolgálón az Internet Information Services (IIS) használhatja. Ezenkívül szükség lehet tooconnect toohello virtuális gép tooadd a megbízható tanúsítványok tooIIS a virtuális gépen. (Ez biztosítja, hogy az IIS által a Web Deploy használt hello a távoli kapcsolat biztonságos.)

hello következő eljárás feltételezi, hogy a hello **Azure-alkalmazás közzététele** varázsló.

### <a name="tooenable-web-deploy-when-you-publish-your-application"></a>tooEnable központi telepítése során Ön közzététele a webalkalmazás
1. tooenable hello **engedélyezéséhez a Web Deploy** az összes webes szerepkörök jelölőnégyzetet, először konfigurálnia kell távoli asztali kapcsolatokhoz. Válassza ki **távoli asztal engedélyezése** minden szerepkörök és majd ellátási hello hitelesítő adatait, amely távolról a hello használt tooconnect lesz **a távoli asztal konfigurálásának** meg. Lásd: [a távoli asztal Azure szerepkörök](vs-azure-tools-remote-desktop-roles.md) további információt.
2. a Web Deploy tooenable összes hello az alkalmazás webes szerepkörök válasszon **engedélyezéséhez a Web Deploy minden webes szerepkörök**.
   
    Figyelmeztetést jelző sárga háromszög jelenik meg. A Web Deploy alapértelmezés szerint nem ajánlott a bizalmas adatok feltöltése egy nem megbízható, önaláírt tanúsítványt használ. Ha toosecure ezt a folyamatot a bizalmas adatokhoz, a Web Deploy kapcsolatokhoz használt SSL-tanúsítvány toobe is hozzáadhat. Ez a tanúsítvány megbízható tanúsítvány toobe kell. További információ toodo, hello című **webes telepítése biztonságos tooMake** a témakör későbbi részében.
3. Válasszon **tovább** tooshow hello **összegzés** képernyőn, és válassza a **közzététel** toodeploy hello felhőalapú szolgáltatás.
   
    hello felhőszolgáltatás közzé van téve. hello virtuális gép, amely jön létre a webes szerepkörök rendelkezik távoli kapcsolatok engedélyezése az IIS-hez, így a Web Deploy lehet használt tooupdate nélkül újbóli közzétételét őket.
   
   > [!NOTE]
   > Ha a konfigurált webes szerepkör több példánya van, egy figyelmeztető üzenet jelenik meg, figyelmezteti a felhasználókat arra, hogy minden webes szerepkör lesz-e a korlátozott tooone példánya csak a hello csomag által létrehozott toopublish az alkalmazás. Válassza ki **OK** toocontinue. Hello követelményeket ismertető részben leírtaknak lehet több mint egy webes szerepkör, de minden szerepkörnek csak egy példánya.
   > 
   > 

### <a name="tooupdate-your-web-role-by-using-web-deploy"></a>a webes szerepkör által a Web Deploy használatával tooUpdate
1. a Web Deploy, toouse kód módosítások toohello projekt a webes szerepkörök bármelyikéhez győződjön meg, hogy szeretné, hogy toopublish, majd kattintson a jobb gombbal a projektcsomópontra a megoldásban, és mutasson túl a Visual Studio**közzététel**. Hello **webhely közzététele** párbeszédpanel jelenik meg.
2. (Választható) Ha egy megbízható SSL tanúsítvány toouse távoli kapcsolatok az IIS-hez hozzáadott, törölheti hello **engedélyezése nem megbízható tanúsítvány** jelölőnégyzetet. Hogyan tooadd egy a Web Deploy tanúsítvány toomake biztonságos kapcsolatos információkért lásd: hello szakasz **webes telepítése biztonságos tooMake** a témakör későbbi részében.
3. a Web Deploy toouse, hello közzététele mechanizmus hello felhasználónevet és jelszót, amely állít be a távoli asztali kapcsolat hello csomag először közzé kell.
   
   1. A **felhasználónév**, adjon meg hello felhasználónevet.
   2. A **jelszó**, hello jelszót adjon meg.
   3. (Választható) Ha ezt a jelszót a profil szeretné toosave, **jelszó mentése**.
4. toopublish hello módosítások tooyour webes szerepkör, válassza a **közzététel**.
   
    hello állapotsor megjelenítése **lépések közzététel**. Hello közzététel befejezése után **sikeres közzététel** jelenik meg. hello módosításainak most már megtörtént a telepített toohello webes szerepkör a virtuális gépen. Most elindíthatja az Azure-alkalmazásában a hello Azure környezetben tootest a módosításokat.

### <a name="toomake-web-deploy-secure"></a>Webalkalmazás telepítése biztonságos tooMake
1. A Web Deploy alapértelmezés szerint nem ajánlott a bizalmas adatok feltöltése egy nem megbízható, önaláírt tanúsítványt használ. Ha toosecure ezt a folyamatot a bizalmas adatokhoz, a Web Deploy kapcsolatokhoz használt SSL-tanúsítvány toobe is hozzáadhat. Ez a tanúsítvány megbízható tanúsítvány, a hitelesítésszolgáltatótól (CA) kapott toobe kell.
   
    a Web Deploy a webes szerepkörök minden egyes virtuális gép biztonságos toomake, fel kell töltenie hello megbízható tanúsítvány, amelyet toouse a web Deploy keretrendszert toohello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885). Ezzel biztosíthatja, hogy hello tanúsítvány szerepel-e toohello virtuális gépet, amely hello webes szerepkör jön létre, amikor az alkalmazás közzétételére.
2. a távoli kapcsolatokhoz, megbízható az SSL-tanúsítvány tooIIS toouse tooadd kövesse az alábbi lépéseket:
   
   1. tooconnect toohello virtuális gép hello webes szerepkör, hello webes szerepkör a select hello példányát futtató **Cloud Explorer** vagy **Server Explorer**, majd válassza a hello **protokoll használatával kapcsolódó levelezőprogramokkal A távoli asztal** parancsot. A részletes lépéseket tooconnect toohello virtuális gép, lásd: [a távoli asztal Azure szerepkörök](vs-azure-tools-remote-desktop-roles.md).
      
      A böngésző felszólítja toodownload egy. RDP-fájlt.
   2. tooadd egy SSL-tanúsítványt, nyissa meg hello szolgáltatás az IIS-kezelőben. Az IIS-kezelőben SSL engedélyezéséhez megnyitása hello **kötések** hello hivatkozásra **művelet** ablaktáblán. Hello **hely kötésének hozzáadása** párbeszédpanel jelenik meg. Válasszon **Hozzáadás**, majd válassza a hello HTTPS **típus** legördülő listából. A hello **SSL-tanúsítvány** menüben válassza ki, hogy meg kellett egy hitelesítésszolgáltató által aláírt és, hogy toohello feltöltött hello SSL-tanúsítvány [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885). További információkért lásd: [kapcsolat konfigurálása hello szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=215824).
      
      > [!NOTE]
      > Ha hozzáad egy megbízható az SSL-tanúsítvány, hello sárga figyelmeztető háromszög nem fog többé megjelenni hello **közzététele varázsló**.
      > 
      > 

## <a name="include-files-in-hello-service-package"></a>Fájlok foglalandó hello Service-csomag
A service-csomag akkor lehet szükség tooinclude meghatározott fájlokat, így elérhetők hello virtuális gépen, amely a szerepkör jön létre. Érdemes például tooadd .exe vagy az .msi fájlt egy indítási parancsfájl tooyour szolgáltatáscsomag által használt. Vagy előfordulhat, hogy egy webes szerepkör vagy a feldolgozói szerepkör projekt igénylő szerelvényt tooadd. a adott toohello megoldás az Azure alkalmazás tooinclude fájlokat kell lenniük.

### <a name="tooinclude-files-in-hello-service-package"></a>tooinclude fájlok hello service-csomag
1. egy szerelvény tooa szolgáltatáscsomag tooadd lépések hello használata:
   
   1. A **Megoldáskezelőben** nyitott hello projektcsomópontra hello projekt hiányzik a hivatkozott hello szerelvény.
   2. tooadd hello toohello szerelvényprojektet, nyissa meg hello helyi menüje hello **hivatkozások** mappa majd **hivatkozás hozzáadása**. hello hivatkozás hozzáadása párbeszédpanel jelenik meg.
   3. Válassza ki a kívánt tooadd, és válassza a hello hello hivatkozás **OK** gombra.
      
      hello hivatkozás jelenik meg hello toohello listájában **hivatkozások** mappát.
   4. Nyissa meg a hozzáadott hello szerelvény hello helyi menüt, és válassza a **tulajdonságok**. Hello **tulajdonságok** ablak jelenik meg.
      
      tooinclude hello szolgáltatásban a szerelvény csomag, a hello **másolása helyi lista** válasszon **igaz**.
2. A **Megoldáskezelőben** nyitott hello projektcsomópontra hello projekt hiányzik a hivatkozott hello szerelvény.
3. tooadd hello toohello szerelvényprojektet, nyissa meg hello helyi menüje hello **hivatkozások** mappa majd **hivatkozás hozzáadása**. Hello **hivatkozás hozzáadása** párbeszédpanel jelenik meg.
4. Válassza ki a kívánt tooadd, és válassza a hello hello hivatkozás **OK** gombra.
   
    hello hivatkozás jelenik meg hello toohello listájában **hivatkozások** mappát.
5. Nyissa meg a hozzáadott hello szerelvény hello helyi menüt, és válassza a **tulajdonságok**. hello tulajdonságai ablakban jelenik meg.
6. tooinclude hello szolgáltatásban a szerelvény csomag, a hello **másolása helyi** menüben válassza ki **igaz**.
7. tooinclude fájlok hello szolgáltatás csomag tooyour webes szerepkör projekt hozzáadott hello fájl hello helyi menü megnyitásához, és válassza **tulajdonságok**. A hello **tulajdonságok** ablakban válassza **tartalom** a hello **Build művelet** lista.
8. tooinclude fájlok hello szolgáltatás csomag hozzáadott tooyour feldolgozói szerepkör projektben nyissa meg a helyi menü hello hello fájl, és válassza **tulajdonságok**. A hello **tulajdonságok** ablakban válassza **másolhatja, ha újabb** a hello **másolási toooutput directory** lista.

## <a name="next-steps"></a>Következő lépések
toolearn a Visual Studio eszközből közzétételi tooAzure kapcsolatos további információkért lásd: [Azure alkalmazás közzététele varázsló](vs-azure-tools-publish-azure-application-wizard.md).

