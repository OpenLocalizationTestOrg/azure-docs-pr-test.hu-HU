---
title: "aaaHow toouse az Office 365-előfizetéshez az Azure RemoteApp |} Microsoft Docs"
description: "Ismerje meg, hogyan használhatja az Office 365-előfizetéshez az Azure RemoteApp tooshare Office-alkalmazásokat."
services: remoteapp
documentationcenter: 
author: piotrci
manager: mbaldwin
ms.assetid: f82b6e23-2b71-47be-846d-fd93f2652f3c
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 2648868dd28cbcd93e38461ae6dce25eaa5d5e99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-your-office-365-subscription-with-azure-remoteapp"></a>Hogyan toouse az Office 365-előfizetéshez az Azure RemoteApp
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Tudta, hogy a meglévő Office 365-előfizetésbe használhatja az Azure RemoteApp tooshare Office-alkalmazásokban hello felhőből? Office 365 kapcsolatos többek között olyan hivatkozásokat tooarticles, amelyek segítenek az Office 365 + az Azure RemoteApp beállítások információkat mutatunk be, ellenőrizze, hello leginkább az előfizetéséhez.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Miként használható az Office 365-fiókok az Azure RemoteApp?
Az összes információt hello Péter új cikk kivétele: [hogyan toouse Azure RemoteApp az Office 365 felhasználói fiókok](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-toorun-office-applications-in-azure-remoteapp"></a>Használhatók az Office 365-előfizetés toorun Office-alkalmazások az Azure Remoteappban?
Igen! A gyakorlatban az Office 365-előfizetését használja az hello módon toobring csak az Office-alkalmazások tooAzure RemoteApp.

(Megjegyzés: Ha az Azure RemoteApp-telepítés hozta üzemeltetési partner, Office-licenccel rendelkező alapján képes tooprovide lehet egy [Service Provider licencszerződés](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx))

hello az Office 365-előfizetéssel kapcsolatos kiváló dolog, hogy az informatikai használata lehetővé teszi több különböző platformokon és környezetekben, beleértve az Azure-felhőbe hello azonos felhasználói licenc hello. Office-alkalmazások az Azure Remoteappban használatakor nem kell toopurchase további licencek vagy külön módon konfigurálhatja a meglévő licenceket. Szüksége, amely tartalmazza az Office 365-előfizetéssel [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx).

Az Office 365 ProPlus lehetővé teszi, hogy [számítógép aktiválási megosztott](https://technet.microsoft.com/library/Dn782860.aspx) – Ez a funkció lehetővé teszi, hogy ideiglenes felhasználó-alapú aktiválás Office virtuális és felhő környezetekben, például az Azure RemoteApp (és a távoli asztali szolgáltatások).

Melyik Office 365 tervek tartalmazza Office 365 ProPlus? Tekintse meg a hello [szolgáltatás rendelkezésre állási belül minden terv](https://technet.microsoft.com/library/office-365-plan-options.aspx) tábla. Fontos megjegyezni, hogy nem minden tervek Office 365 ProPlus (például hello Office 365 üzleti terv) tartalmazza. Ha a csomag nem létezik, fontolja meg az áttérést tooa tervet, amelyet (például Office 365 oktatási E3).

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>OK, hogyan az Office 365 ProPlus licencek használja az Azure RemoteApp?
Minden felhasználói licenc, az Office 365 ProPlus lehetővé teszi, hogy egy felhasználó aktiválása az Office-alkalmazásokat too5 számítógépek plus rendszerű táblagépek és telefonok be. Az egyes aktiválások hello felhasználói mindaddig, amíg azok Office inaktiválása hello eszközön van regisztrálva. (Felhasználók kezelhetik az eszközeiket a hello [Office 365 portál](https://portal.office365.com/).)

Az Azure RemoteApp egy-egy felhasználóhoz előfordulhat, hogy jelentkezzen be a hello több olyan számítógéppel azonos nap, miközben azt. Ennek oka hello szolgáltatás automatikusan kezeli, és méretezi hello felhőben található erőforrásokat, miközben hello felhasználónál csak hello alkalmazások és programok, akivel megosztotta. Az Office 365 ProPlus igénybevételéhez egy megosztott számítógép aktiválási mód – Ez azt jelenti, hogy a felhasználó nem kell toodo bármely licenc felügyeleti tooaccess ezeket az erőforrásokat, és adott hello az egyes számítógépek nem számítanak elleni hello 5 számítógép aktiválási korlátot.

Mindaddig, amíg Ön (Üdvözöljük a rendszergazdákat) Office 365 ProPlus licencek hozzárendelése tooyour felhasználók, Office használhatnak, a személyes eszközeiken, valamint az Azure RemoteApp-gyűjteménnyel keresztül.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Office alkalmazások is használja az Office 365 és Azure RemoteApp?
Az Office 365-előfizetés tooactivate használja, és Office 2013 megosztani az Azure RemoteApp-környezetekben. Jelenleg nem támogatjuk az Azure RemoteApp Office egyéb verziói hello használatát. Ez magában foglalja az Office 2003, Office 2007, Office 2010 és Office 2016.

## <a name="what-about-visio-pro-or-project-pro"></a>Mi a helyzet a Visio Pro és a Project Pro?
hello Office 365 ProPlus-rendszerképet szerepel a RemoteApp-előfizetés tartoznak a Visio Pro és a Project Pro alkalmazásokat. Azonban nem használható az Office 365 ProPlus előfizetés tooactivate általi - ezek mindegyike saját licenccel rendelkezik. A hello aktiválhatja [Office 365 portál](https://portal.office365.com/). 

Nincs toolicense ezeket a programokat, ha nem szeretné, hogy toouse őket. Csak aktiválni szeretné, hogy toouse - és kihagyja a hello programok hello mások számára. Továbbra is láthatja azokat hello kép, de nem használhat. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Hogyan kezdjem el az Office 365 és Azure RemoteApp?
Most, hogy ismeri az Office 365 licencelési hello részleteit, jelentkezzen be használatának az Azure RemoteApp - nagyon egyszerű:

Az Azure RemoteApp-gyűjteménnyel létrehozásakor használjon hello **Office 365 ProPlus (előfizetés szükséges)** kép.

![Az Office 365 Pro Plus Azure RemoteApp kép](./media/remoteapp-officesubscription/remoteapp-officeimage.png)

Ez a kép tartalmazza a Windows Server és az Office 365 ProPlus hello legújabb verziójára. A gyűjtemény (beleértve a közzétételi alkalmazások is), a felhasználók egyszerűen konfigurálása után jelentkezzen be Azure RemoteApp (a RemoteApp-ügyfél segítségével), és Office alkalmazások Office 365 hitelesítőadatainak megadására. Licencek automatikusan érkeznek nélkül bármilyen számkészletet vagy felügyeleti szükséges.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Hozható létre egyéni lemezképként az Office 365 ProPlus?
A gyűjteményhez, amely tartalmazza az Office 365 ProPlus létrehozhat egy egyéni lemezképet. 2 döntéseket – hello Azure katalógusában kép használata nyújtunk, vagy létrehozhat saját egyéni rendszerképét és van az Office 365 ProPlus telepítése.

### <a name="use-hello-azure-gallery-image"></a>Hello Azure katalógusában kép használata
hello legegyszerűbb módja toodeploy Office 365 ProPlus tooa gyűjtemény túl van[hello Azure katalógusában képek egyikével](remoteapp-image-on-azurevm.md) az Azure RemoteApp előfizetés tartalmazza. Ellenőrizze, hogy hello **Windows Server távoli asztali munkamenetgazda az Office 365 ProPlus előtelepített** kép. Majd telepítse azt szeretné, minden más alkalmazás a rendszerképen, és jó toogo.

### <a name="use-a-custom-image"></a>Egyéni lemezkép
Bármikor létrehozhat egy egyéni lemezkép - hozhat létre egy [Azure virtuális gép](remoteapp-image-on-azurevm.md) vagy [hello lemezképet helyileg](remoteapp-create-custom-image.md) , és töltse fel tooAzure. Mindkét esetben győződjön meg arról, Office 365 ProPlus hello megosztott számítógép aktiválási csomópont használatával telepít. Használjon hello [Office-telepítő eszköz](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) , és kövesse a hello [utasításokat](https://technet.microsoft.com/library/Dn782858.aspx) telepítéséhez.  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Az Office 365 ProPlus automatikus frissítések letiltásához a egyéni lemezkép - fontos
Az egyéni lemezképet használják az Azure RemoteApp sablonként hello igény szerint további erőforrások hozzáadása a a felhasználók növeli a. tooprevent késést és a csatlakozási problémáit, tiltsa le az automatikus frissítések Office hello lemezképben. Ha nem, akkor minden sablonnal létrehozott erőforrás indításakor automatikusan frissíti. Ehelyett használjon hello normál Azure RemoteApp folyamat frissítése az egyéni lemezképet. Ezzel a módszerrel hello Office-alkalmazásokat egyszer hello sablonlemezkép frissíti, és hagyja meg az Azure RemoteApp első hello frissítések tooyour felhasználók kezeli.

toodisable automatikus frissítések, adja hozzá a következő toohello Office-telepítő eszköz konfigurációs fájl hello:

        <Updates Enabled="FALSE" />

A konfigurációs fájl most ezeket a sorokat kell tartalmaznia:

        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>Igen, hogyan frissíthetők a képfájl az Office 365 ProPlus?
Nincsenek számos okból tooupdate hello kép a gyűjteményben. Az alábbiakban néhány:

* Hello legújabb Windows-frissítések beszerzése 
* Hello legújabb Office 365 ProPlus alkalmazás frissítéseinek beszerzése
* Az egyéni alkalmazás frissítése
* Hello lemezképet más konfigurációs beállításainak módosítása

A végpont lépéseket hello a gyűjtemény toouse hello lemezkép frissítése frissítéséhez lásd [Itt](remoteapp-update.md). De hogyan tooupdate hello lemezképet és az Office 365 ProPlus információkért tekintse meg a következő információ hello.

A lemezkép frissítése a két lehetősége van – a lemezkép egy teljesen új vagy manuálisan lecserélni a meglévő lemezkép frissítéséhez.

### <a name="replace-your-image-with-hello-latest-azure-gallery-image--add-customizations"></a>Cserélje le a lemezképet hello legújabb Azure katalógusában kép + olyan testreszabásokat vegyen fel
Ezzel a beállítással lehetővé teszik a Microsoft hello Windows Server és az Office 365 ProPlus-frissítések kezeli. Helyett a meglévő lemezkép frissítése egy teljesen új képet hello legújabb gyűjtemény lemezkép alapján hoz létre. Ezután ismételje meg az egyik lépéshez sem toocustomize hello lemezkép - előtt tette egyéni alkalmazásokat telepíteni, előbb módosítsa a hello kép konfigurációs stb.

hello gyűjtemény lemezképei rendszeresen frissíteni, így hagyhatja egyszerű, ismerete, hogy a Windows Server és az Office 365 ProPlus-alkalmazások toodate be vannak-e. Természetesen hello kompromisszum kell tooapply a testreszabások minden alkalommal, amikor egy új lemezképet kap. Parancsfájlok tooautomate beállítása a testreszabásokat is létrehozhat.

### <a name="manually-update-your-existing-image"></a>Manuálisan frissítse a meglévő lemezkép
Ezzel a lehetőséggel a szabványos Windows eszközök tooapply frissítések toohello lemezképet használ. Az Office 365 ProPlus hello Office központi telepítési eszköz toodownload használja, és telepítse a legújabb frissítéseinek hello vagy az Office 365 ProPlus verzió.

> [!IMPORTANT]
> Ne feledje – hello Office 365 ProPlus automatikus frissítések letiltásához.
> 
> 

Frissítések hello Office-telepítő eszköz használatáról további információra van szüksége?

* [Az Office 365 termékek való kattintásra telepítését hello Office-telepítő eszköz](https://technet.microsoft.com/library/JJ219423.aspx)
* [Központi telepítése és frissítése Office 365 ProPlus használatával hello Office-telepítő eszköz](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (videó)
* [Az Office 365 ProPlus frissítési beállítások konfigurálása](https://technet.microsoft.com/library/dn761708.aspx)

