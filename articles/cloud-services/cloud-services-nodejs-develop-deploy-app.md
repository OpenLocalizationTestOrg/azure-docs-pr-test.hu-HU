---
title: "aaaNode.js – első lépések útmutató |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy egyszerű Node.js webalkalmazást, majd központilag telepítenie tooan Azure felhőszolgáltatást."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 50951a87-fed4-48e0-bcfa-453b9e50452e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 22945bfcc1b0e5da2a2d37dc5cc86be013cc0b5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-a-nodejs-application-tooan-azure-cloud-service"></a>Hozza létre, és a Node.js-alkalmazás tooan Azure Cloud Service telepítése

Ez az oktatóanyag bemutatja, hogyan toocreate egy egyszerű Node.js az Azure-Felhőszolgáltatásban futó alkalmazás. Cloud Services csomag építőelemei hello méretezhető felhőalapú alkalmazások az Azure-ban. Hello elkülönítését és egymástól független kezelését és az alkalmazás előtér- és összetevők kibővített lehetővé teszik.  A Cloud Services egy robusztus, dedikált virtuális gépet biztosít az egyes szerepkörök megbízható üzemeltetéséhez.

A felhőalapú szolgáltatások, valamint annak összevetése tooAzure webhelyek és a virtuális gépek további információkért lásd: [Azure Websites, a Cloud Services és a virtuális gépek összevetése].

> [!TIP]
> Egy egyszerű webhely toobuild szüksége? Ha csak egy egyszerű webhely előterét kívánja futtatni, fontolja meg egy [egyszerűsített webalkalmazás használatát]. A felhőalapú szolgáltatás tooa könnyedén frissíthet, ha a webalkalmazás növekszik és a követelmények változnak.

Az oktatóanyag utasításait követve egy webes szerepkörben lévő egyszerű webalkalmazást fog létrehozni. Ön szolgáltatás hello compute emulator tootest az alkalmazást helyileg, majd PowerShell parancssori eszközök használatával telepítse.

hello alkalmazás egy egyszerű "hello world" alkalmazást:

![A webböngészőben Hello World hello weblapot][A web browser displaying hello Hello World web page]

## <a name="prerequisites"></a>Előfeltételek
> [!NOTE]
> A jelen oktatóanyagban szereplő Azure PowerShell használatához Windows rendszer szükséges.

* Telepítse és konfigurálja az [Azure PowerShell] eszközt.
* Töltse le és telepítse a hello [Azure SDK for .NET 2.7]. Hello telepítő telepíti, válassza ki:
  * MicrosoftAzureAuthoringTools
  * MicrosoftAzureComputeEmulator

## <a name="create-an-azure-cloud-service-project"></a>Azure Cloud Service-projektet létrehozása
Hajtsa végre a következő feladatok toocreate egy új Azure Cloud Service-projekt alapszintű Node.js szerkezettel hello:

1. Futtatás **Windows PowerShell** eszközt rendszergazdaként: hello **Start menü** vagy **kezdőképernyőn**, keressen **Windows PowerShell**.
2. [PowerShell összekapcsolása] tooyour előfizetés.
3. Adja meg a következő PowerShell parancsmagot toocreate toocreate hello projekt hello:

        New-AzureServiceProject helloworld

    ![hello hello New-AzureService helloworld parancs eredménye][hello result of hello New-AzureService helloworld command]

    Hello **New-AzureServiceProject** parancsmag létrehoz egy alapszintű struktúrát egy Node.js-alkalmazás tooa felhőalapú szolgáltatás-közzététel. Közzétételi tooAzure szükséges konfigurációs fájlokat tartalmaz. hello parancsmag módosítja a directory toohello munkakönyvtára hello szolgáltatást is.

    hello parancsmag hozza létre a következő fájlok hello:

   * **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** és **ServiceDefinition.csdef**: az alkalmazás közzétételéhez szükséges Azure-specifikus fájlok. További információkért lásd: [Üzemeltetett szolgáltatás létrehozása az Azure-ban – áttekintés].
   * **deploymentSettings.json**: hello Azure PowerShell telepítési parancsmagok által használt helyi beállításokat tárolja.
4. Adja meg a következő parancs tooadd új webes szerepkör hello:

       Add-AzureNodeWebRole

   ![hello hello Add-AzureNodeWebRole parancs kimenete][hello output of hello Add-AzureNodeWebRole command]

   Hello **Add-AzureNodeWebRole** parancsmag létrehoz egy alapszintű Node.js-alkalmazást. Módosítja hello **.csfg** és **.csdef** tooadd konfigurációs bejegyzéseket hello új szerepkör-fájlokat.

   > [!NOTE]
   > Ha nem ad meg egy nevet a szerepkörhöz, alapértelmezett név lesz használva. Hello első parancsmag paraméterként megadhat egy nevet:`Add-AzureNodeWebRole MyRole`

Node.js-alkalmazás hello hello fájlban definiált **server.js**, hello hello webes szerepkör könyvtárában található (**WebRole1** alapértelmezés szerint). Íme hello kódot:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Ez a kód alapvetően megegyeznek a "Hello World" hello mintát a hello hello [nodejs.org] webhelyen, azzal a különbséggel hello hello felhőkörnyezet által hozzárendelt portszámot használja.

## <a name="deploy-hello-application-tooazure"></a>Hello alkalmazás tooAzure telepítése

> [!NOTE]
> toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége. [Aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF), vagy [regisztrálhat egy ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="download-hello-azure-publishing-settings"></a>Töltse le a hello Azure közzétételi beállítások
toodeploy az alkalmazás tooAzure először le kell töltenie hello közzétételi beállításait az Azure-előfizetéshez.

1. Futtassa a következő Azure PowerShell-parancsmag hello:

       Get-AzurePublishSettingsFile

   A böngésző használata toonavigate toohello közzététele beállítások letöltése oldalt. Előfordulhat, hogy a kért toolog be egy Microsoft Account. Ha igen, használja az Azure-előfizetéshez társított hello fiók.

   Mentse a letöltött hello profil tooa fájl helye könnyen elérhetők.
2. Futtassa a következő parancsmag tooimport hello közzétételi profil letöltött:

       Import-AzurePublishSettingsFile [path toofile]

    > [!NOTE]
    > Hello importálása után közzétételi beállítások, javasoljuk, hogy hello törlése letöltött .publishSettings-fájlt, mert nem tartalmaz információt, amelyekkel mások tooaccess fiókját.

### <a name="publish-hello-application"></a>Hello alkalmazás közzététele
Futtassa a következő parancsok hello toopublish:

      $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

* **Szolgáltatásnév -** hello telepítési hello nevét határozza meg. Ennek egyedi névnek kell lennie, ellenkező esetben hello közzétételi folyamat meghiúsul. Hello **Get-Date** parancs hozzátold egy dátum/idő karakterláncot, hogy hello neve egyedi.
* **-Hely** határozza meg az üzemeltetett hello alkalmazás hello datacenter. az elérhető adatközpontok, hello használja listáját toosee **Get-AzureLocation** parancsmag.
* **-Launch** egy böngészőablakban megnyitja és toohello üzemeltetett szolgáltatás lép a telepítés befejezése után.

Miután a közzététel sikeresen megtörtént, a válasz hasonló toohello következő jelenik meg:

![hello hello Publish-AzureService parancs kimenete][hello output of hello Publish-AzureService command]

> [!NOTE]
> Ez a hello alkalmazás toodeploy néhány percet igénybe vehet, és válnak elérhetővé, az első közzététel alkalmával.

Hello központi telepítés befejezése után egy böngészőablakban nyissa meg, és keresse meg a toohello felhőalapú szolgáltatás.

![Hello hello world oldalt megjelenítő böngészőablak hello URL-cím jelzi hello lap az Azure-on.][A browser window displaying hello hello world page; hello URL indicates hello page is hosted on Azure.]

Az alkalmazás most már az Azure-ban fut.

Hello **Publish-AzureServiceProject** parancsmaggal hello a következő lépéseket:

1. Létrehoz egy csomag toodeploy. hello csomag tartalmazza az összes hello fájlt az alkalmazás mappájában.
2. Létrehoz egy új **tárfiókot**, ha még nem létezik. hello Azure storage-fiók üzembe helyezése során használt toostore hello alkalmazáscsomag. Hello storage-fiók a telepítés befejezése után nyugodtan törölheti.
3. Létrehoz egy új **felhőszolgáltatást**, ha még nem létezik. A **felhőalapú szolgáltatás** hello tároló, amelyben az alkalmazás üzemel, ha a telepített tooAzure. További információkért lásd: [Üzemeltetett szolgáltatás létrehozása az Azure-ban – áttekintés].
4. Hello központi telepítési csomag tooAzure közzéteszi.

## <a name="stopping-and-deleting-your-application"></a>Az alkalmazás leállítása és törlése
Miután az alkalmazás telepítéséhez, érdemes lehet a toodisable azt további költségek elkerülése érdekében. Az Azure a webesszerepkör-példányok esetében óránként számol fel díjat a felhasznált kiszolgálóidő után. Kiszolgálói felhasznált után az alkalmazás van telepítve, akkor is, ha hello példányok nem futnak, és hello leállt állapotban van.

1. Hello Windows PowerShell-ablakban állítsa le a hello szolgáltatás központi telepítése a következő parancsmag hello hello előző szakaszban létrehozott:

       Stop-AzureService

   Hello szolgáltatás leállítása eltarthat néhány percig. Hello szolgáltatás leáll, amikor megjelenik egy üzenet, amely azt jelzi, hogy leállt.

   ![hello hello Stop-AzureService parancs állapota][hello status of hello Stop-AzureService command]
2. toodelete hello szolgáltatást, a következő parancsmag hívás hello:

       Remove-AzureService

   Amikor a rendszer kéri, adja meg a **Y** toodelete hello szolgáltatást.

   Hello szolgáltatás törlése eltarthat néhány percig. Hello szolgáltatás törlése után megjelenik egy üzenet, amely azt jelzi, hogy törölve lett-e a hello szolgáltatást.

   ![hello hello Remove-AzureService parancs állapota][hello status of hello Remove-AzureService command]

   > [!NOTE]
   > Hello szolgáltatás törlése nem érinti hello szolgáltatás első közzétételekor létrehozott tárfiók hello és, továbbra is fizetnie kell a tárhelyet toobe. Ha nincs más hello tárolót használ, érdemes lehet a toodelete azt.

## <a name="next-steps"></a>Következő lépések
További információkért lásd: hello [Node.js fejlesztői központ].

<!-- URL List -->

[Azure Websites, a Cloud Services és a virtuális gépek összevetése]: ../app-service-web/choose-web-site-cloud-service-vm.md
[egyszerűsített webalkalmazás használatát]: ../app-service-web/app-service-web-get-started-nodejs.md
[Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Azure SDK for .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[PowerShell összekapcsolása]: /powershell/azureps-cmdlets-docs#step-3-connect
[nodejs.org]: http://nodejs.org/
[Üzemeltetett szolgáltatás létrehozása az Azure-ban – áttekintés]: https://azure.microsoft.com/documentation/services/cloud-services/
[Node.js fejlesztői központ]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[hello result of hello New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[hello output of hello Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying hello Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[hello output of hello Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying hello hello world page; hello URL indicates hello page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[hello status of hello Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[hello status of hello Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
