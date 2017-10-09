---
title: "aaaDeploy az első alkalmazás tooCloud Foundry a Microsoft Azure |} Microsoft Docs"
description: "Egy alkalmazás tooCloud Foundry Azure telepítéséhez"
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 8fa04a58-56ad-4e6c-bef4-d02c80d4b60f
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/14/2017
ms.author: seanmck
ms.openlocfilehash: 878da38f6eabe32a339f02aa0ead811d6e5af9a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-first-app-toocloud-foundry-on-microsoft-azure"></a>Központi telepítése az első alkalmazás tooCloud Foundry a Microsoft Azure

[A felhő Foundry](http://cloudfoundry.org) érhető el egy népszerű nyílt forráskódú platform Microsoft Azure-on. Ebben a cikkben megmutatjuk, hogyan toodeploy és kezelheti egy alkalmazás felhő Foundry Azure környezetben.

## <a name="create-a-cloud-foundry-environment"></a>A felhő Foundry környezet létrehozása

Nincsenek a felhő Foundry környezet létrehozása az Azure számos lehetőség közül választhat:

- Használjon hello [döntő felhő Foundry ajánlat] [ pcf-azuremarketplace] a hello Azure piactér toocreate egy szabványos környezetben, amely tartalmazza az PCF Ops Manager és hello Azure Service Broker. Található [utasításokkal] [ pcf-azuremarketplace-pivotaldocs] hello piactér telepítéséhez kínálnak a hello döntő dokumentációját.
- Hozzon létre egy testreszabott környezetet által [döntő felhő Foundry manuális telepítése][pcf-custom].
- [Közvetlenül hello nyílt forráskódú felhő Foundry csomagok központi telepítése] [ oss-cf-bosh] be kell állítania egy [BOSH](http://bosh.io) igazgató, a virtuális gépek hello telepítési hello felhő Foundry környezet koordinálására.

> [!IMPORTANT] 
> Ha PCF hello Azure Piactérről származó telepít, jegyezze fel a hello SYSTEMDOMAINURL, és hello rendszergazdai hitelesítő adataival szükséges tooaccess hello kulcsfontosságú alkalmazások kezelő mindkettőnek hello piactér telepítési útmutatóban leírt. Akkor szükséges toocomplete ebben az oktatóanyagban vannak. Piactér-telepítések esetén hello SYSTEMDOMAINURL hello űrlap https://system van. *ip-cím*. cf.pcfazure.com.

## <a name="connect-toohello-cloud-controller"></a>Csatlakozás toohello felhő vezérlő

hello felhő vezérlő hello elsődleges belépési pont tooa felhő Foundry környezet központi telepítésére és alkalmazások felügyeletére. hello core felhő vezérlő API (CCAPI) a REST API-t, de különböző eszközök keresztül érhető el. Ebben az esetben azt vele keresztül kapcsolatba hello [felhő Foundry CLI][cf-cli]. Hello CLI telepítheti Linux, a MacOS vagy a Windows, de ha nem, akkor érhető el hello előre telepített tooinstall inkább [Azure Cloud rendszerhéj][cloudshell-docs].

a, toolog illesztenie `api` toohello SYSTEMDOMAINURL beolvasott hello piactér telepítéséből. Mivel hello alapértelmezett telepítési egy önaláírt tanúsítványt használ, akkor ki kell terjednie hello `skip-ssl-validation` váltani.

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

A felhő vezérlő toohello rákérdezéses toolog áll. Hello rendszergazdai fiók hitelesítő adatait, amely hello piactér üzembe helyezés lépései beszerzett használja.

Felhő Foundry biztosít *szervezethez* és *szóközöket* névterek tooisolate hello csoportok és belül egy megosztott telepítési környezetekben. hello PCF piactér telepítési tartalmaz hello alapértelmezett *rendszer* szervezeti és a tárolóhelyek létrehozott toocontain hello alapkomponensek, például a hello automatikus skálázás szolgáltatás és hello Azure service broker. Most, válassza ki a hello *rendszer* terület.


## <a name="create-an-org-and-space"></a>Hozzon létre egy szervezeti és lemezterület

Ha `cf apps`, megjelenik egy adott rendszer alkalmazáscsoportra hello rendszer területen belül hello system. szervezeti alkalmazott 

Hello kell tartania *rendszer* rendszer alkalmazások számára fenntartott szervezeti, hozzon létre egy szervezeti és a hely toohouse a mintaalkalmazást.

```bash
cf create-org myorg
cf create-space dev -o myorg
```

Hello cél parancs tooswitch toohello új szervezeti és helyet használja:

```bash
cf target -o testorg -s dev
```

Most az alkalmazás központi telepítésekor automatikusan létrejön hello új szervezeti és. amely jelenleg tooconfirm hello új szervezeti/terület, az alkalmazás nem írja be a `cf apps` újra.

> [!NOTE] 
> További információ a szervezethez és szóközöket és hogyan használhatók a szerepköralapú hozzáférés-vezérlést (RBAC): hello [felhő Foundry dokumentáció][cf-orgs-spaces-docs].

## <a name="deploy-an-application"></a>Alkalmazás üzembe helyezése

Most használja a felhő Foundry mintaalkalmazás Hello rugó felhő, amelyet a rendszer a Java nyelven írt hello alapján nevű [rugó keretrendszer](http://spring.io) és [rugó rendszerindító](http://projects.spring.io/spring-boot/).

### <a name="clone-hello-hello-spring-cloud-repository"></a>Hello Hello rugó felhő tárház klónozása

hello Hello rugó felhő mintaalkalmazást a Githubon érhető el. Klónozza tooyour környezetben, és hello új könyvtárba módosítása:

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-hello-application"></a>Hello alkalmazás létrehozása

Build hello alkalmazás használatával [Apache Maven](http://maven.apache.org).

```bash
mvn clean package
```

### <a name="deploy-hello-application-with-cf-push"></a>Hello alkalmazás cf leküldéses telepítését

A legtöbb alkalmazások tooCloud Foundry telepítése hello segítségével `push` parancs:

```bash
cf push
```

Ha Ön *leküldéses* egy alkalmazást, a felhő Foundry hello alkalmazástípus (ebben az esetben egy alkalmazásban Java) észlel, és azonosítja a függőséget (Ez esetben hello rugó keretrendszer). Majd csomagok mindent lemezképpel egy önálló tároló, ez a kód toorun szükséges egy *feldolgozó*. Végül felhő Foundry ütemezések hello hello rendelkezésre gépek környezetében egyik alkalmazás, és létrehoz egy URL-címet, ahol érhető el, elérhető a hello hello parancs kimenetét.

![Cf leküldéses parancs kimenete][cf-push-output]

toosee hello hello felhőalapú rugó alkalmazás, a böngészőben nyissa meg a megadott hello URL-címe:

![Alapértelmezett felhasználói felület Hello rugó felhő][hello-spring-cloud-basic]

> [!NOTE] 
> További következményeiről során toolearn `cf push`, lásd: [hogyan vannak előkészített alkalmazások] [ cf-push-docs] hello felhő Foundry dokumentációjában található.

## <a name="view-application-logs"></a>Alkalmazás-naplók megtekintése

Hello felhő Foundry CLI tooview naplók az alkalmazás használatához a neve:

```bash
cf logs hello-spring-cloud
```

Alapértelmezés szerint a hello naplózza a parancs által használt *utóhívás*, amely jeleníti meg az új naplók az oktatóprogram. Új naplók toosee jelenik meg, frissítse a hello hello-rugó-felhőalkalmazás hello böngészőben.

tooview naplók írt már, vegye fel a hello `recent` váltani:

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-hello-application"></a>Skála hello alkalmazás

Alapértelmezés szerint `cf push` csak hoz létre az alkalmazás egyetlen példányát. tooensure magas rendelkezésre állású és nagyobb átviteli sebesség eléréséhez kibővítési engedélyezése, általában a kívánt toorun több, mint az alkalmazások egy példánya. Már telepített alkalmazások hello segítségével könnyen lehet horizontálisan `scale` parancs:

```bash
cf scale -i 2 hello-spring-cloud
```

Futó hello `cf app` hello alkalmazás a parancs megjeleníti, hogy a felhő Foundry hello alkalmazás egy másik példánya hoz létre. Hello alkalmazás elindult, miután a felhő Foundry terheléselosztási forgalom tooit automatikusan elindul.


## <a name="next-steps"></a>Következő lépések

- [Olvasási hello felhő Foundry dokumentáció][cloudfoundry-docs]
- [Felhő Foundry hello Visual Studio Team Services beépülő modul beállítása][vsts-plugin]
- [Felhő Foundry Microsoft napló Analytics dugulásellenőrzési hello konfigurálása][loganalytics-nozzle]

<!-- LINKS -->

[pcf-azuremarketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry
[pcf-custom]: https://docs.pivotal.io/pivotalcf/1-10/customizing/azure.html
[oss-cf-bosh]: https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs
[pcf-azuremarketplace-pivotaldocs]: https://docs.pivotal.io/pivotalcf/customizing/pcf_azure.html
[cf-cli]: https://github.com/cloudfoundry/cli
[cloudshell-docs]: https://docs.microsoft.com/azure/cloud-shell/overview
[cf-orgs-spaces-docs]: https://docs.cloudfoundry.org/concepts/roles.html
[spring-boot]: https://projects.spring.io/spring-boot/
[spring-framework]: http://spring.io
[cf-push-docs]: https://docs.cloudfoundry.org/concepts/how-applications-are-staged.html
[cloudfoundry-docs]: https://docs.cloudfoundry.org
[vsts-plugin]: https://github.com/Microsoft/vsts-cloudfoundry
[loganalytics-nozzle]: https://github.com/Azure/oms-log-analytics-firehose-nozzle

<!-- IMAGES -->
[cf-push-output]: ./media/cloudfoundry-deploy-your-first-app/cf-push-output.png
[hello-spring-cloud-basic]: ./media/cloudfoundry-deploy-your-first-app/hello-spring-cloud-basic.png