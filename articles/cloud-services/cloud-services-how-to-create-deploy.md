---
title: "aaaHow toocreate és egy felhőalapú szolgáltatás üzembe helyezése |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és központi telepítése egy felhőalapú szolgáltatás metódussal hello gyors létrehozása az Azure-ban."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 0ea78ccc-5e7d-40f8-bdb6-478c0eb0e265
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 09f889f81ccee6e8a7657116183aa4100410214c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-deploy-a-cloud-service"></a>Hogyan tooCreate és egy felhőalapú szolgáltatás telepítése
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-create-deploy-portal.md)
> * [klasszikus Azure portál](cloud-services-how-to-create-deploy.md)
> 
> 

hello a klasszikus Azure portálon két lehetőséget biztosít az Ön toocreate és egy felhőalapú szolgáltatás üzembe helyezése: **Gyorslétrehozás** és **egyéni létrehozás**.

Ez a témakör azt ismerteti, hogyan toouse hello gyors létrehozás módszerrel toocreate új felhőalapú szolgáltatás, és ezután **feltöltése** tooupload és az Azure cloud service csomag telepítését. Ha ezt a módszert használja, hello a klasszikus Azure portálon elérhető kényelmes hivatkozások követelményeinek befejezése menet lehetővé teszi. Ha készen áll a felhőalapú szolgáltatás létrehozásakor toodeploy, mind hello teheti használatával **egyéni létrehozás**.

> [!NOTE]
> Ha azt tervezi, toopublish a felhőszolgáltatás a Visual Studio Team Services (VSTS), **gyors létrehozás**, majd állítsa be a VSTS-közzététel **gyors üzembe helyezés** vagy hello irányítópult.
> 
> 

## <a name="concepts"></a>Alapelvek
Három összetevők szükségesek a rendelés toodeploy egy alkalmazásra, amely egy felhőalapú szolgáltatás az Azure-ban:

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

* Ha azt szeretné, hogy egy felhőszolgáltatás, amely a Secure Sockets Layer (SSL) használ az adatok titkosításához, toodeploy [állítsa be az alkalmazását](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) SSL-t.
* Ha azt szeretné, hogy tooconfigure távoli asztali kapcsolatok toorole példányok [hello szerepkörök konfigurálása](cloud-services-role-enable-remote-desktop.md) a távoli asztal.
* Ha tooconfigure részletes figyelését a felhőalapú szolgáltatás, engedélyezze az Azure Diagnostics hello felhőalapú szolgáltatás. *Minimális figyelési* (hello alapértelmezett szint figyelési) hello állomás operációs rendszerek (a virtuális gépek). szerepkörpéldányokra szolgáltatástól összegyűjtött teljesítményszámlálókat használ. "Részletes figyelési * gyűjti a teljesítményadatokat belül hello szerepkör példányok tooenable szorosabb elemzésének kérelem feldolgozása során előforduló problémák alapján további metrikák. Hogyan toofind tooenable Azure Diagnostics, lásd: [diagnosztika engedélyezésével az Azure-ban](cloud-services-dotnet-diagnostics.md).

toocreate webes szerepkörök vagy feldolgozói szerepkörök példányai egy felhőszolgáltatás, kell [hello service-csomag létrehozása](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Előkészületek
* Ha még nem telepítette a hello Azure SDK-t, kattintson a **Azure SDK telepítése** tooopen hello [Azure letöltőoldala](https://azure.microsoft.com/downloads/), majd töltse le az SDK hello hello nyelven, ahol inkább toodevelop a kódot. (Konfigurálnia kell egy lehetőség toodo ezt később.)
* Ha a szerepkörpéldányok szükséges tanúsítvány, hello tanúsítványok létrehozása. Cloud services és a titkos kulcs egy .pfx fájlba szükséges. Is [hello tanúsítványok tooAzure feltöltése](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) létrehozásához és telepítéséhez hello felhőalapú szolgáltatás.
* Ha azt tervezi, toodeploy hello cloud service tooan affinitáscsoport, hozzon létre hello affinitáscsoport. A felhőszolgáltatás és az egyéb Azure-szolgáltatások toohello használhatja egy affinitáscsoporthoz toodeploy régióban ugyanazon a helyen. Létrehozhat hello affinitáscsoport hello **hálózatok** hello hello a klasszikus Azure portálon területe **Affinitáscsoportok** lap.

## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Útmutató: a gyors létrehozással felhőalapú szolgáltatás létrehozása
1. A hello [a klasszikus Azure portálon](http://manage.windowsazure.com/), kattintson a **új**>**számítási**>**Felhőszolgáltatás** > **Gyorslétrehozás**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)
2. A **URL-cím**, adjon meg egy altartomány neve toouse hello nyilvános URL-címben eléréséhez a felhőalapú szolgáltatás, az éles környezetekben. az üzemi környezetek hello URL-formátum: http://*myURL*. cloudapp.net.
3. A **régiót vagy Affinitáscsoportot**, válasszon hello földrajzi régiót vagy affinitáscsoportot toodeploy hello felhőalapú szolgáltatás számára. Az affinitáscsoportokban válassza, ha azt szeretné toodeploy a felhőalapú szolgáltatás toohello régión belül más Azure-szolgáltatásokkal és ugyanazon a helyen.
4. Kattintson a **Felhőalapú szolgáltatás létrehozása** elemre.
   
    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)
   
    Hello állapotának hello hello üzenet területen hello hello ablakának alsó részén figyelheti.
   
    Hello **Felhőszolgáltatások** terület megnyílik, hello új felhőszolgáltatással jelenik meg. TooCreated hello állapotának megváltozásakor felhőalapú szolgáltatás létrehozása sikeresen befejeződött.
   
    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)

## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Útmutató: a felhőalapú szolgáltatáshoz a tanúsítvány feltöltése
1. A hello [a klasszikus Azure portálon](http://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, kattintson hello felhőszolgáltatás hello nevét, majd **tanúsítványok**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)
2. Jelölje be az **tanúsítvány feltöltése** vagy **feltöltése**.
3. A **fájl**, használjon **Tallózás** tooselect hello tanúsítvány (.pfx-fájl).
4. A **jelszó**, adja meg a hello hello tanúsítvány titkos kulcsa.
5. Kattintson a **OK** (jelölő).
   
    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)
   
    Figyelheti az hello előrehaladását hello feltöltés hello üzenet terület, a lent látható módon. Hello feltöltés befejezését követően a hello tanúsítvány toohello táblázat jelenik meg. Hello üzenet területen kattintson az OK tooclose üdvözlőüzenetére.
   
    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Útmutató: a felhőalapú szolgáltatás telepítése
1. A hello [a klasszikus Azure portálon](http://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, kattintson hello felhőszolgáltatás hello nevét, majd **irányítópult**.
2. Jelölje be az **töltse fel az új éles környezet** vagy **feltöltése**.
3. A **üzemelő példány címkéje**, adjon meg egy nevet hello új központi telepítés – például MyCloudServicev4.
4. A **csomag**, használjon **Tallózás** tooselect hello szolgáltatás csomag fájl (.cspkg) toouse.
5. A **konfigurációs**, használjon **Tallózás** tooselect hello szolgáltatás fájlját (.cscfg) toouse konfigurálta.
6. Ha hello felhőszolgáltatás szerepköröket tartalmazza, ha csak egy példánya, válassza ki a hello **üzembe helyezés akkor is, ha egy vagy több szerepkör egyetlen példányt tartalmaz** jelölőnégyzetet tooenable hello telepítési tooproceed.
   
    Azure csak garantálható a 99,95 % toohello felhőalapú szolgáltatás karbantartási és a frissítések során legalább két példányt minden szerepkör-e. Ha szükséges, a hello adhat hozzá további szerepkörpéldányokat **méretezési** oldalon felhőszolgáltatás hello telepítése után. További információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).
7. Kattintson a **OK** (jelölő) toobegin hello felhőalapú szolgáltatás telepítését.
   
    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)
   
    Hello telepítés hello üzenet területen hello állapotának figyelése Kattintson az OK toohide üdvözlőüzenetére.
   
    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Ellenőrizze a telepítés sikeresen befejeződött
1. Kattintson a **irányítópult**.
   
    hello állapot jelenítsen meg, hogy van-e hello szolgáltatás **futtató**.
2. A **gyors áttekintő**, kattintson a hello webhely URL-cím tooopen webböngészőben a felhőalapú szolgáltatás.
   
    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


## <a name="next-steps"></a>Következő lépések
* [A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure.md).
* Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name.md).
* [A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage.md).
* Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate.md).

