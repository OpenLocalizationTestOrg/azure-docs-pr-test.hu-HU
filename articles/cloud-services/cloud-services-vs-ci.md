---
title: "a Visual Studio Online szolgáltatások aaaContinuous kézbesítési felhő |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset folyamatos kézbesítését az Azure-bA mentése a felhőalapú alkalmazások diagnosztika tárolási kulcs toohello szolgáltatás konfigurációs fájlok mentése nélkül"
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
ms.openlocfilehash: dc87d049e46daf8b8a26ee4450ebd9b7910f287b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securely-save-cloud-services-diagnostics-storage-key-and-setup-continuous-integration-and-deployment-tooazure-using-visual-studio-online"></a>Securely mentése Cloud Services diagnosztika Biztonságitár-kulcs és a telepítő folyamatos integrációt és telepítést tooAzure Visual Studio Online használata
 Egy általános gyakorlat tooopen forrás projektek ma is. Alkalmazás titkos kulcsok menteni a konfigurációs fájlok már nem biztonságos gyakorlat, a biztonsági rések ki vannak téve a titkos kulcsok a nyilvános forráskódú vezérlők küldik. Titkos kulcs tárolása, az egyszerű szöveges fájlban folyamatos integrációt-feldolgozási folyamat nem biztonságos vagy óta lemezképfájl-kiszolgálókhoz lehet megosztott erőforrások hello felhőalapú környezetben. Ez a cikk azt ismerteti, hogyan Visual Studio és a Visual Studio Online csökkenti hello biztonsági problémáit fejlesztési és folyamatos integrációt folyamat során.

## <a name="remove-diagnostics-storage-key-secret-in-project-configuration-file"></a>Távolítsa el a diagnosztika tárolási kulcs titkos projekt konfigurációs fájlban
Cloud Services diagnosztika bővítmény diagnosztikai eredmények mentése az Azure storage igényel. Korábban hello tárolási kapcsolati karakterlánc lett megadva a hello Felhőszolgáltatások konfigurációs (.cscfg) fájlok, és sikerült beadni toosource vezérlő. A legfrissebb Azure SDK-verzióban hello változtattuk hello viselkedés tooonly tároló egy részleges kapcsolati karakterlánc szerepét a jogkivonat hello kulcs. hello lépések azt mutatják be, hello új Felhőszolgáltatás tooling működése:

### <a name="1-open-hello-role-designer"></a>1. Nyissa meg a hello szerepkör designer
* Kattintson duplán, vagy kattintson jobb gombbal a Cloud Services szerepkör tooopen szerepkör jellemzőihez

![Nyissa meg a szerepkör-Tervező][0]

### <a name="2-under-diagnostics-section-a-new-check-box-dont-remove-storage-key-secret-from-project-is-added"></a>2. A diagnosztika című szakaszban, egy új jelölőnégyzet "ne távolítsa el a projektből titkos kulcs" kerül.
* Ha hello helyi storage emulator használata esetén ezt a jelölőnégyzetet le van tiltva, mert nincs titkos toomanage hello helyi kapcsolódási karakterlánc, amely UseDevelopmentStorage = true.

![Nincs titkos helyi storage emulator kapcsolati karakterlánc][1]

* Új projekt létrehozásakor alapértelmezés szerint a jelölőnégyzet nincs bejelölve. Az eredmény hello tárolási kulcs szakaszában hello kiválasztott tárolási kapcsolati karakterlánc jogkivonatok lecserélni. hello hello token értékének találhatók hello aktuális felhasználó AppData központi mappában, például: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService

> Vegye figyelembe a hello user\AppData mappában hozzáférést által a felhasználói bejelentkezés és egy biztonságos hely toostore fejlesztési titkok számít.
> 
> 

![Biztonságitár-kulcs mentett felhasználói profil mappájába][2]

### <a name="3-select-a-diagnostics-storage-account"></a>3. Válassza ki a diagnosztikai tárfiók
* Válasszon egy tárfiókot hello "..." gombra kattintva elindított hello párbeszédpanelről gombra. Figyelje meg, hogyan hello generált tárolási kapcsolati karakterlánc nem lesz hello tárfiók kulcsa.
* Például: megadni: DefaultEndpointsProtocol = https; AccountName = contosostorage; AccountKey = $(*clouddiagstrg.key*)

### <a name="4----debugging-hello-project"></a>4.    Hibakeresési hello projekt
* A Visual Studio hibakeresési F5 toostart. Minden kell dolgoznia hello azonos módon mint korábban.
  ![Helyileg a hibakeresés elindításához][3]

### <a name="5-publish-project-from-visual-studio"></a>5. A Visual Studio projekt közzététele
* Indítási hello közzétételére párbeszédpanelt, és folytassa a bejelentkezési utasítások toopublish hello applicaion tooAzure.

### <a name="6-additional-information"></a>6. További információ
> Megjegyzés: hello beállítások panel hello szerepkör Designer marad, mert a lépést. Ha diagnosztikai toouse hello titkos felügyeleti szolgáltatást, lépjen a toohello konfiguráció lapon.
> 
> 

![Beállítások hozzáadása][4]

> Megjegyzés: Ha engedélyezve van, az Application Insights kulcs hello tárolja egyszerű szövegként. hello kulcs csak szolgál feltöltés tartalmát, a bizalmas adatok lesz feltörésének kockázata.
> 
> 

## <a name="build-and-publish-a-cloud-services-project-using-visual-studio-online-task-templates"></a>Hozza létre és közzététele a felhőalapú szolgáltatások a projekt Visual Studio online Feladatsablonok
* a lépéseket követve hello bemutatja, hogyan toosetup folyamatos integrációt a Felhőszolgáltatások projektre a Visual Studio online feladatok használata:
  ### <a name="1----obtain-a-vso-account"></a>1.    Egy VSO fiók beszerzése
* [A Visual Studio Online-fiók létrehozása] [ Create Visual Studio Online account] Ha még nem rendelkezik már
* [Hozzon létre csapatprojekt] [ Create team project] a Visual Studio online-fiók

### <a name="2----setup-source-control-in-visual-studio"></a>2.    A Visual Studio Verziókövetés beállítása
* Csatlakozás tooa csapatprojekt

![Csatlakozás tooteam projekt][5]

![Válassza ki a csapat projekt tooconnect számára][6]

* A projekt toosource vezérlő hozzáadása

![Projekt toosource vezérlő hozzáadása][7]

![Térkép vezérlőelem projekt tooa forrásmappa][8]

* Ellenőrizze a projekt csapata Intézőből

![A projekt toosource vezérlő ellenőrzése][9]

### <a name="3----configure-build-process"></a>3.    Konfigurálja a felépítési folyamat
* Keresse meg a tooyour csapatprojekt és egy új felépítési folyamat sablonok hozzáadása

![Adja hozzá egy új build][10]

* Jelölje be összeállítási feladat

![Egy összeállítási feladat hozzáadása][11]

![Válassza ki a Visual Studio Build sablon][12]

* Összeállítási feladat bemenet szerkesztése. Adja a hello build szerint tooyour paramétereit testreszabása

![Összeállítási feladat konfigurálása][13]

`/t:Publish /p:TargetProfile=$(targetProfile) /p:DebugType=None /p:SkipInvalidConfigurations=true /p:OutputPath=bin\ /p:PublishDir="$(build.artifactstagingdirectory)\\"`

* Build változók konfigurálása

![Build változók konfigurálása][14]

* Egy feladat tooupload build eldobási hozzáadása

![Válassza a build eldobási tevékenység közzététele][15]

![Konfigurálása build eldobási tevékenység közzététele][16]

* Hello build futtatása

![Új várólista létrehozása][17]

![Build összegzésének megtekintése][18]

* Ha hello build sikeres látni fogja a eredmény hasonló toobelow

![Build eredménye][19]

### <a name="4----configure-release-process"></a>4.    Kiadás folyamat beállítása
* Hozzon létre egy új kiadás

![Hozzon létre új kiadás][20]

* Válassza ki a hello Azure Cloud Services telepítési feladat

![Válassza ki a szolgáltatástelepítési feladat Azure Cloud Services csomag][21]

* Hello tárfiók kulcsa nem ellenőrzése toosource vezérlőben, toospecify hello titkos kulcsot kell diagnosztika bővítmény beállításához. Bontsa ki a hello **speciális beállítások új szolgáltatás létrehozása** szakaszt, és szerkesztheti a hello **diagnosztikai Tárfiók kulcsait** bemeneti paraméter. A bemeneti időt vesz igénybe, a kulcs-érték pár hello formátumban több sornyi **[RoleName]:$(StorageAccountKey)**

> Megjegyzés: a diagnosztikai tárfiók alatt áll hello ugyanahhoz az előfizetéshez, ahol hello Felhőszolgáltatások alkalmazás közzé szeretné tenni, ha nem rendelkezik tooenter hello kulccsal hello telepítési feladat bemeneti; hello telepítési programozott módon szerezze be a hello tárolással kapcsolatos az előfizetésből
> 
> 

![Cloud Services telepítési feladat konfigurálása][22]

* Használjon titkos kulcs létrehozása változók toosave tárolási kulcsok. Adjon meg egy változó titkos kulcsként kattintson a hello lakat ikon hello jobb oldalán hello toomask változók

![Mentési tároló kulcsok a titkos kulcs változók létrehozása][23]

* Egy kiadási létrehozása és telepítése a projekt tooAzure

![Hozzon létre új kiadás][24]

## <a name="next-steps"></a>Következő lépések
További információ az Azure Cloud Services diagnosztika bővítmények beállítása toolearn talál [a PowerShell használata Azure Cloud Services diagnosztika engedélyezése][Enable diagnostics in Azure Cloud Services using PowerShell]

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
