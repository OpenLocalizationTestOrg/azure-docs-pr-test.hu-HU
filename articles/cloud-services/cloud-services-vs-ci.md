---
title: "A Visual Studio Online szolgáltatások folyamatos kézbesítési felhő |} Microsoft Docs"
description: "Ismerje meg, hogyan állíthat be Azure felhőalapú alkalmazásokat folyamatos kézbesítési diagnosztika biztonságitár-kulcs a szolgáltatás konfigurációs fájlok mentése nélkül"
services: cloud-services
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 148b2959-c5db-4e4a-a7e9-fccb252e7e8a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: cawa
ms.openlocfilehash: 7e70f92d4d1333ca6cbac5876e5ccbc763bd915c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="securely-save-cloud-services-diagnostics-storage-key-and-setup-continuous-integration-and-deployment-to-azure-using-visual-studio-online"></a>Securely Cloud Services diagnosztika tárolási kulcs mentése és a telepítő folyamatos integrációt és a használatával a Visual Studio Online telepítési
 Általános gyakorlat, nyissa meg a forrás-projektek ma is. Alkalmazás titkos kulcsok menteni a konfigurációs fájlok már nem biztonságos gyakorlat, a biztonsági rések ki vannak téve a titkos kulcsok a nyilvános forráskódú vezérlők küldik. Titkos kulcs tárolása, az egyszerű szöveges fájlban folyamatos integrációt-feldolgozási folyamat nem biztonságos vagy óta lemezképfájl-kiszolgálókhoz lehet megosztott erőforrások a felhőalapú környezetben. Ez a cikk azt ismerteti, hogyan Visual Studio és a Visual Studio Online csökkenti a biztonsági aggályokon fejlesztési és folyamatos integrációt folyamat során.

## <a name="remove-diagnostics-storage-key-secret-in-project-configuration-file"></a>Távolítsa el a diagnosztika tárolási kulcs titkos projekt konfigurációs fájlban
Cloud Services diagnosztika bővítmény diagnosztikai eredmények mentése az Azure storage igényel. Korábban a tárolási kapcsolati karakterlánc lett megadva a Felhőszolgáltatások konfigurációs (.cscfg) fájlokat, és sikerült beadni a verziókövetési rendszerrel. A a legutóbb kiadott Azure SDK változtattuk a viselkedés csak tárolásához egy részleges kapcsolati karakterlánc szerepét a jogkivonatot a kulccsal. A következő lépések bemutatják, hogyan működik, az új Felhőszolgáltatás eszközt használunk erre:

### <a name="1-open-the-role-designer"></a>1. Nyissa meg a szerepkör-Tervező
* Kattintson duplán vagy felhőalapú szolgáltatások szerepkör szerepkör designer megnyitásához kattintson a jobb gombbal

![Nyissa meg a szerepkör-Tervező][0]

### <a name="2-under-diagnostics-section-a-new-check-box-dont-remove-storage-key-secret-from-project-is-added"></a>2. A diagnosztika című szakaszban, egy új jelölőnégyzet "ne távolítsa el a projektből titkos kulcs" kerül.
* A helyi storage emulator használ, ha a jelölőnégyzet le van tiltva, mert nincs titkos kulcsot a helyi kapcsolati karakterlánc, amely UseDevelopmentStorage kezeléséhez = true.

![Nincs titkos helyi storage emulator kapcsolati karakterlánc][1]

* Új projekt létrehozásakor alapértelmezés szerint a jelölőnégyzet nincs bejelölve. Ennek eredményeképp a tárolási fő szakasz a kiválasztott tárolási kapcsolati karakterlánc jogkivonatok lecserélni. A token értékét fogja található az aktuális felhasználó AppData központi mappában, például: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService

> Vegye figyelembe, hogy a user\AppData mappa hozzáférést által a felhasználói bejelentkezés és tekinthető fejlesztési titkos kulcsokat tárolja biztonságos helyen.
> 
> 

![Biztonságitár-kulcs mentett felhasználói profil mappájába][2]

### <a name="3-select-a-diagnostics-storage-account"></a>3. Válassza ki a diagnosztikai tárfiók
* Válasszon egy tárfiókot a párbeszédpanelen kattintva elindítja a "..." gombra. Figyelje meg, hogyan generált tárolási kapcsolati karakterlánc nem lesz a tárfiók kulcsára.
* Például: megadni: DefaultEndpointsProtocol = https; AccountName = contosostorage; AccountKey = $(*clouddiagstrg.key*)

### <a name="4----debugging-the-project"></a>4.    A projekt hibakeresési
* F5 billentyűt a Visual Studio a hibakeresés elindításához. Minden előtt a azonos módon működnek.
  ![Helyileg a hibakeresés elindításához][3]

### <a name="5-publish-project-from-visual-studio"></a>5. A Visual Studio projekt közzététele
* Indítsa el a közzététel párbeszédpanel, és folytassa a bejelentkezési utasításokat a applicaion közzététele az Azure-bA.

### <a name="6-additional-information"></a>6. További információ
> Megjegyzés: A beállítások panelen a szerepkör-tervezőben marad, mert a lépést. Ha azt szeretné, a titkos felügyeleti szolgáltatás használatához diagnosztikai, nyissa meg a konfiguráció lapon.
> 
> 

![Beállítások hozzáadása][4]

> Megjegyzés: Ha engedélyezve van, az Application Insights kulcs tárolja egyszerű szövegként. A kulcs csak szolgál feltöltés tartalmát, a bizalmas adatok lesz feltörésének kockázata.
> 
> 

## <a name="build-and-publish-a-cloud-services-project-using-visual-studio-online-task-templates"></a>Hozza létre és közzététele a felhőalapú szolgáltatások a projekt Visual Studio online Feladatsablonok
* A következő lépések a Visual Studio online feladatainak Felhőszolgáltatások projekt folyamatos integráció telepítője mutatja be:
  ### <a name="1----obtain-a-vso-account"></a>1.    Egy VSO fiók beszerzése
* [A Visual Studio Online-fiók létrehozása] [ Create Visual Studio Online account] Ha még nem rendelkezik már
* [Hozzon létre csapatprojekt] [ Create team project] a Visual Studio online-fiók

### <a name="2----setup-source-control-in-visual-studio"></a>2.    A Visual Studio Verziókövetés beállítása
* A csapatprojekt kapcsolódni

![Csapatprojekt kapcsolódni][5]

![Válassza ki a csapatprojekt való kapcsolódáshoz][6]

* Adja hozzá a projekthez a verziókövetési rendszerrel

![A verziókövetési projekt hozzáadása][7]

![A vezérlő mappa projekt leképezése][8]

* Ellenőrizze a projekt csapata Intézőből

![A verziókövetési be][9]

### <a name="3----configure-build-process"></a>3.    Konfigurálja a felépítési folyamat
* Keresse meg a csapatprojekt és egy új felépítési folyamat sablonok hozzáadása

![Adja hozzá egy új build][10]

* Jelölje be összeállítási feladat

![Egy összeállítási feladat hozzáadása][11]

![Válassza ki a Visual Studio Build sablon][12]

* Összeállítási feladat bemenet szerkesztése. A build paraméterek az igényeknek megfelelően adjon testreszabása

![Összeállítási feladat konfigurálása][13]

`/t:Publish /p:TargetProfile=$(targetProfile) /p:DebugType=None /p:SkipInvalidConfigurations=true /p:OutputPath=bin\ /p:PublishDir="$(build.artifactstagingdirectory)\\"`

* Build változók konfigurálása

![Build változók konfigurálása][14]

* Adjon hozzá egy feladatot build eldobási feltöltése

![Válassza a build eldobási tevékenység közzététele][15]

![Konfigurálása build eldobási tevékenység közzététele][16]

* Futtassa a build

![Új várólista létrehozása][17]

![Build összegzésének megtekintése][18]

* Ha a build sikeres látni fogja a hasonló alatt eredményt

![Build eredménye][19]

### <a name="4----configure-release-process"></a>4.    Kiadás folyamat beállítása
* Hozzon létre egy új kiadás

![Hozzon létre új kiadás][20]

* Válassza ki az Azure felhőalapú szolgáltatásokhoz központi telepítési feladatot

![Válassza ki a szolgáltatástelepítési feladat Azure Cloud Services csomag][21]

* A tárfiók kulcsa nem ellenőrzése verziókövetési rendszerrel, igazolnia kell a titkos kulcsot diagnosztika bővítmények beállítása. Bontsa ki a **speciális beállítások új szolgáltatás létrehozása** szakaszt, és szerkesztheti a **diagnosztikai Tárfiók kulcsait** bemeneti paraméter. A bemeneti időt vesz igénybe, a kulcs-érték pár formátumban több sornyi **[RoleName]:$(StorageAccountKey)**

> Megjegyzés: a diagnosztikai tárfiók ugyanahhoz az előfizetéshez, mint ahol teszi közzé a Cloud Services alkalmazás alatt áll, ha azt nem kell megadnia a kulcsot a telepítési tevékenység bemeneti; a központi telepítés programozott módon beszerzi a tárolással kapcsolatos az előfizetéshez
> 
> 

![Cloud Services telepítési feladat konfigurálása][22]

* Használjon titkos hozhat létre. változók tárolási kulcsok mentéséhez. Maszkolás titkos kulcsként változó kattintson a jobb oldalán a változók bemeneti zárolási ikonra

![Mentési tároló kulcsok a titkos kulcs változók létrehozása][23]

* Egy kiadási létrehozása és központi telepítése a projekthez az Azure-bA

![Hozzon létre új kiadás][24]

## <a name="next-steps"></a>Következő lépések
Az Azure Cloud Services diagnosztika bővítmények beállításával kapcsolatos további tudnivalókért tekintse meg [a PowerShell használata Azure Cloud Services diagnosztika engedélyezése][Enable diagnostics in Azure Cloud Services using PowerShell]

[Create Visual Studio Online account]:https://www.visualstudio.com/team-services/
[Create team project]: https://www.visualstudio.com/it-it/docs/setup-admin/team-services/connect-to-visual-studio-team-services
[Enable diagnostics in Azure Cloud Services using PowerShell]:https://azure.microsoft.com/en-us/documentation/articles/cloud-services-diagnostics-powershell/

[0]: ./media/cloud-services-vs-ci/vs-01.png
[1]: ./media/cloud-services-vs-ci/vs-02.png
[2]: ./media/cloud-services-vs-ci/file-01.png
[3]: ./media/cloud-services-vs-ci/vs-03.png
[4]: ./media/cloud-services-vs-ci/vs-04.png
[5]: ./media/cloud-services-vs-ci/vs-05.png
[6]: ./media/cloud-services-vs-ci/vs-06.png
[7]: ./media/cloud-services-vs-ci/vs-07.png
[8]: ./media/cloud-services-vs-ci/vs-08.png
[9]: ./media/cloud-services-vs-ci/vs-09.png
[10]: ./media/cloud-services-vs-ci/vso-01.png
[11]: ./media/cloud-services-vs-ci/vso-02.png
[12]: ./media/cloud-services-vs-ci/vso-03.png
[13]: ./media/cloud-services-vs-ci/vso-04.png
[14]: ./media/cloud-services-vs-ci/vso-05.png
[15]: ./media/cloud-services-vs-ci/vso-06.png
[16]: ./media/cloud-services-vs-ci/vso-07.png
[17]: ./media/cloud-services-vs-ci/vso-08.png
[18]: ./media/cloud-services-vs-ci/vso-09.png
[19]: ./media/cloud-services-vs-ci/vso-10.png
[20]: ./media/cloud-services-vs-ci/vso-11.png
[21]: ./media/cloud-services-vs-ci/vso-12.png
[22]: ./media/cloud-services-vs-ci/vso-13.png
[23]: ./media/cloud-services-vs-ci/vso-14.png
[24]: ./media/cloud-services-vs-ci/vso-15.png
