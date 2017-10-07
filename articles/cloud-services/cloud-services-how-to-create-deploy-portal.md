---
title: "aaaHow toocreate és egy felhőalapú szolgáltatás üzembe helyezése |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és központi telepítése egy felhőalapú szolgáltatás hello Azure-portál használatával."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 56ea2f14-34a2-4ed9-857c-82be4c9d0579
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: dc8b81a594f3514e662c49c9a46a33da8a551f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-deploy-a-cloud-service"></a>Hogyan toocreate és egy felhőalapú szolgáltatás telepítése
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-create-deploy-portal.md)
> * [klasszikus Azure portál](cloud-services-how-to-create-deploy.md)
>
>

hello Azure-portálon két lehetőséget biztosít az Ön toocreate és egy felhőalapú szolgáltatás üzembe helyezése: *Gyorslétrehozás* és *egyéni létrehozás*.

Ez a cikk azt ismerteti, hogyan toouse hello gyors létrehozás módszerrel toocreate új felhőalapú szolgáltatás, és ezután **feltöltése** tooupload és az Azure cloud service csomag telepítését. Ha ezt a módszert használja, hello Azure-portálon teszi követelményeinek befejezése menet elérhető kényelmes hivatkozásokat. Ha készen áll a felhőalapú szolgáltatás létrehozásakor toodeploy, mind hello teheti ugyanannyi időt vesz igénybe, használja a egyéni létrehozás.

> [!NOTE]
> Ha azt tervezi, toopublish a felhőszolgáltatás a Visual Studio Team Services (VSTS), használja a Gyorslétrehozás, és VSTS közzététel hello Azure gyors üzembe helyezési vagy hello irányítópultról majd beállítása. További információkért lásd: [folyamatos kézbesítési tooAzure használatával Visual Studio Team Services által][TFSTutorialForCloudService], vagy tekintse át a súgóban talál hello **gyors üzembe helyezés** lap.
>
>

## <a name="concepts"></a>Alapelvek
Három kötelező toodeploy egy alkalmazás az Azure-ban felhőszolgáltatásként:

* **Szolgáltatásdefiníció**  
  hello felhő szolgáltatásdefiníciós fájl (.csdef) hello modell, beleértve a szerepkörök hello számának meghatározása.
* **Szolgáltatás konfigurációja**  
  hello felhőalapú szolgáltatás konfigurációs fájlját (.cscfg) hello felhőalapú szolgáltatás és az adott szerepkörök, beleértve a szerepkörpéldányok számát hello konfigurációs beállításait biztosítja.
* **Szolgáltatáscsomag**  
  hello szolgáltatás csomagba (.cspkg) hello alkalmazáskód és konfigurációk és hello szolgáltatásdefiníciós fájl tartalmazza.

További ezekről, és hogyan toocreate csomag [Itt](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Az alkalmazás előkészítése
Egy felhőalapú szolgáltatás telepítése előtt létre kell hoznia hello cloud service csomagba (.cspkg) alkalmazáskódjában és egy felhőalapú szolgáltatás konfigurációs fájlját (.cscfg). hello Azure SDK eszközöket biztosít a szükséges központi telepítés fájlok előkészítése. Hello SDK hello telepítheti [Azure letölti](https://azure.microsoft.com/downloads/) lap, ahol inkább toodevelop hello nyelvi az alkalmazás kódjában.

Három cloud service funkciókhoz speciális konfigurációk service-csomag exportálása előtt:

* Ha azt szeretné, hogy egy felhőszolgáltatás, amely a Secure Sockets Layer (SSL) használ az adatok titkosításához, toodeploy [állítsa be az alkalmazását](cloud-services-configure-ssl-certificate-portal.md#modify) SSL-t.
* Ha azt szeretné, hogy tooconfigure távoli asztali kapcsolatok toorole példányok [hello szerepkörök konfigurálása](cloud-services-role-enable-remote-desktop-new-portal.md) a távoli asztal.
* Ha tooconfigure részletes figyelését a felhőalapú szolgáltatás, engedélyezze az Azure Diagnostics hello felhőalapú szolgáltatás. *Minimális figyelési* (hello alapértelmezett szint figyelési) hello állomás operációs rendszerek (a virtuális gépek). szerepkörpéldányokra szolgáltatástól összegyűjtött teljesítményszámlálókat használ. *Részletes figyelési* gyűjti a teljesítményadatokat belül hello szerepkör példányok tooenable szorosabb elemzésének kérelem feldolgozása során előforduló problémák alapján további metrikák. Hogyan toofind tooenable Azure Diagnostics, lásd: [engedélyezése az Azure diagnostics](cloud-services-dotnet-diagnostics.md).

toocreate webes szerepkörök vagy feldolgozói szerepkörök példányai egy felhőszolgáltatás, kell [hello service-csomag létrehozása](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Előkészületek
* Ha még nem telepítette a hello Azure SDK-t, kattintson a **Azure SDK telepítése** tooopen hello [Azure letöltőoldala](https://azure.microsoft.com/downloads/), majd töltse le az SDK hello hello nyelven, ahol inkább toodevelop a kódot. (Konfigurálnia kell egy lehetőség toodo ezt később.)
* Ha a szerepkörpéldányok szükséges tanúsítvány, hello tanúsítványok létrehozása. Cloud services és a titkos kulcs egy .pfx fájlba szükséges. Feltöltheti hello tanúsítványok tooAzure létrehozásához és telepítéséhez hello felhőalapú szolgáltatás.

## <a name="create-and-deploy"></a>Létrehozása és telepítése
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **új > számítási**, majd görgessen lefelé tooand kattintson **Felhőszolgáltatás**.

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. Az új hello **Felhőszolgáltatás** panelen adjon meg egy értéket a hello **DNS-név**.
4. Hozzon létre egy új **erőforráscsoport** vagy válasszon egy meglévőt.
5. Válasszon ki egy **helyet**.
6. Kattintson a **csomag**. Ekkor megnyílik hello **a csomag feltöltése** panelen. Hello kötelező mezők kitöltésével. Ha egyetlen példányt tartalmaz, a szerepkörökben, győződjön meg arról **üzembe helyezés akkor is, ha egy vagy több szerepkör egyetlen példányt tartalmaz** van kiválasztva.
7. Győződjön meg arról, hogy **indítsa el a központi telepítés** van kiválasztva.
8. Kattintson a **OK** amely bezárul hello **a csomag feltöltése** panelen.
9. Ha nem rendelkezik a tanúsítványok tooadd, kattintson a **létrehozása**.

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>A tanúsítvány feltöltése
Ha a központi telepítési csomag [toouse tanúsítványok konfigurált](cloud-services-configure-ssl-certificate-portal.md#modify), most már feltöltheti hello tanúsítványt.

1. Válassza ki **tanúsítványok**, és a hello **tanúsítványok hozzáadása** panelen válassza ki a hello SSL tanúsítvány .pfx fájlját, és adja meg a hello **jelszó** hello tanúsítvány
2. Kattintson a **Attach tanúsítvány**, és kattintson a **OK** a hello **adja hozzá a tanúsítványok** panelen.
3. Kattintson a **létrehozása** a hello **Felhőszolgáltatás** panelen. Amikor hello telepítési elérte hello **készen** állapotát, folytathatja a következő lépések toohello.

    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a>Ellenőrizze a telepítés sikeresen befejeződött
1. Kattintson a hello cloud service-példány.

    hello állapot jelenítsen meg, hogy van-e hello szolgáltatás **futtató**.
2. A **Essentials**, hello kattintson **webhely URL-címe** tooopen a felhőalapú szolgáltatás egy webböngészőben.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Következő lépések
* [A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure-portal.md).
* Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name-portal.md).
* [A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage-portal.md).
* Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md).
