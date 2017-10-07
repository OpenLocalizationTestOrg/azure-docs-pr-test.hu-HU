---
title: "aaaManage az alkalmazások, a Visual Studio |} Microsoft Docs"
description: "Használja a Visual Studio toocreate, elkészítéséhez csomag, telepítése és hibakeresése a Service Fabric-alkalmazásokhoz és szolgáltatásokhoz."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: c317cb7e-7eae-466e-ba41-6aa2518be5cf
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: b2d5803d85e4f9645dcbece33a2208bc0955498d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-visual-studio-toosimplify-writing-and-managing-your-service-fabric-applications"></a>Visual Studio toosimplify írása, és a Service Fabric-alkalmazások kezelése
Az Azure Service Fabric-alkalmazások és szolgáltatások Visual Studio alkalmazással kezelheti. Miután megismerte [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md), használja a Visual Studio toocreate Service Fabric-alkalmazások, adja hozzá a szolgáltatásokhoz, vagy a csomag, regisztrálása és alkalmazások telepítése a helyi fejlesztési fürtöt.

## <a name="deploy-your-service-fabric-application"></a>A Service Fabric-alkalmazás központi telepítése
Alapértelmezés szerint az alkalmazás központi telepítése egyesíti a lépéseket követve egy egyszerű üzembe hello:

1. Hello alkalmazáscsomag létrehozása
2. Hello alkalmazás csomag toohello lemezképtárolóhoz feltöltése
3. Hello alkalmazástípus regisztrálása
4. Eltávolítani a alkalmazáspéldányok fut
5. Az alkalmazáspéldány létrehozása

A Visual Studio lenyomásával **F5** telepíti az alkalmazást, és csatlakoztassa a hello hibakereső tooall alkalmazáspéldányok. Használhat **Ctrl + F5** toodeploy anélkül, hogy hibakeresés, vagy akkor tehet közzé tooa helyi vagy távoli fürtre hello közzétételi profil. További információkért lásd: [az alkalmazás tooa távoli fürt közzététele a Visual Studio használatával](service-fabric-publish-app-remote-cluster.md).

### <a name="application-debug-mode"></a>Alkalmazás hibakeresési módban
A Visual Studio adja meg a tulajdonságot, **alkalmazás hibakeresési módban**, amely meghatározza, hogy a hibakeresés részeként Visual Studios toohandle alkalmazás központi telepítésének módját.

#### <a name="tooset-hello-application-debug-mode-property"></a>tooset hello alkalmazás hibakeresési módban tulajdonság
1. Hello Service Fabric application projekt (*.sfproj) a helyi menüben válassza a **tulajdonságok** (vagy nyomja le az hello **F4** kulcs).
2. A hello **tulajdonságok** ablakban, a set hello **alkalmazás hibakeresési módban** tulajdonság.

![Állítsa be az alkalmazás hibakeresési módban tulajdonság][debugmodeproperty]

#### <a name="application-debug-modes"></a>Alkalmazás hibakeresési üzemmód

1. **Frissítse az alkalmazás** ebben a módban tooquickly változás lehetővé teszi, és hibakeresése a kód és a statikus webes fájlok szerkesztésének hibakeresés során támogatja. Ebben az üzemmódban csak akkor működik, ha a helyi fejlesztési fürtök [1-csomópont mód](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).
2. **Távolítsa el az alkalmazás** okok hello alkalmazás toobe távolítja el, amikor hello hibakeresési munkamenet azért ér véget.
3. **Automatikus frissítés** hello alkalmazás toorun továbbra is, amikor hello hibakeresési munkamenet befejeződik. hello következő hibakeresési munkamenet kezeli hello telepítési frissítéseként. hello frissítési folyamat megőrzi a korábbi hibakeresési munkamenetben megadott adatokat.
4. **Alkalmazás megtartása** hello alkalmazás tartja hello fürt hello futó debug munkamenete véget nem ér. Hello elején hello következő hibakeresési munkamenetben, hello alkalmazás el lesz távolítva.

A **automatikus frissítése** adatok megmaradjanak hello alkalmazás frissítési lehetőségeit a Service Fabric alkalmazásával. Alkalmazások, és hogyan lehet, hogy frissíti az valós környezetben történő frissítéssel kapcsolatos további információkért lásd: [Service Fabric az alkalmazásfrissítés](service-fabric-application-upgrade.md).

## <a name="add-a-service-tooyour-service-fabric-application"></a>A szolgáltatás tooyour Service Fabric-alkalmazás hozzáadása
Új szolgáltatások tooyour alkalmazás tooextend funkciókat adhat hozzá.  hogy hello szolgáltatás szerepel-e az alkalmazáscsomag tooensure hozzáadása hello szolgáltatást keresztül hello **új Fabric-szolgáltatás...**  menüpont.

![Egy új Service Fabric-szolgáltatás hozzáadása][newservice]

Jelöljön ki egy Service Fabric projekt típus tooadd tooyour alkalmazást, és adjon meg egy nevet hello szolgáltatást.  Lásd: [keretrendszere, amely a szolgáltatás kiválasztása](service-fabric-choose-framework.md) úgy dönt, hogy melyik szolgáltatás toohelp toouse írja be.

![Jelöljön ki egy Service Fabric szolgáltatás projekt típus tooadd tooyour alkalmazást][addserviceproject]

hello új szolgáltatás tooyour megoldás és a meglévő alkalmazáscsomag hozzáadása. hello szolgáltatási hivatkozást lekérni, és egy alapértelmezett szolgáltatáspéldány hozzáadott toohello alkalmazásjegyzék, ezért hello szolgáltatás toobe létrehozott, és elindította hello hello alkalmazás következő indításakor.

![hello új szolgáltatás tooyour alkalmazásjegyzék hozzáadása][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>A Service Fabric-alkalmazás becsomagolása
toodeploy hello alkalmazás és a szolgáltatások tooa fürtje kell toocreate egy alkalmazáscsomagot.  hello csomag rendszerezi hello alkalmazásjegyzék szolgáltatás jegyzékfájlban és egyéb szükséges fájlok egy adott elrendezésben.  A Visual Studio állít be, és hello alkalmazás projekt mappájában hello "pkg" directory hello csomag kezeli.  Kattintson a **csomag** a hello **alkalmazás** helyi menü hoz létre vagy frissítések hello alkalmazáscsomagot.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Távolítsa el az alkalmazások és a Cloud Explorer használatával alkalmazástípusok
Műveleteket hajthat végre alapszintű fürt felügyeleti belül a Visual Studio használatával indítja el a hello Cloud Explorerben **nézet** menü. Például alkalmazások törlése és leépíteni a következőt: alkalmazástípusok helyi vagy távoli fürtökön.

![Alkalmazások eltávolítása][removeapplication]

> [!TIP]
> Több fürt kezelési funkcióját, lásd: [a fürt megjelenítése a Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
>
>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
* [A Service Fabric alkalmazásmodellt.](service-fabric-application-model.md)
* [A Service Fabric-alkalmazás központi telepítése](service-fabric-deploy-remove-applications.md)
* [Alkalmazás paramétereinek több környezet kezelése](service-fabric-manage-multiple-environment-app-configuration.md)
* [A Service Fabric-alkalmazás hibakeresés](service-fabric-debugging-your-application.md)
* [A fürt megjelenítése a Service Fabric Explorer használatával](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png