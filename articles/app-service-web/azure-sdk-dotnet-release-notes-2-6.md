---
title: "aaaAzure SDK for .NET 2.6 kibocsátási megjegyzései"
description: "Az Azure SDK for .NET 2.6 kibocsátási megjegyzések"
services: app-service/web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: b45853d5-a2b8-4962-a22d-579cb36ae14c
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: ac613cf20da4f731fab6f35ccbf6dbeaf18c0ec4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-26-release-notes"></a>Az Azure SDK for .NET 2.6 kibocsátási megjegyzések
Ez a dokumentum hello Azure SDK .NET 2.6 kiadás kibocsátási megjegyzései hello tartalmaz. 

Az Azure SDK 2.6 fejleszthet felhőalapú szolgáltatás alkalmazások (PaaS) célcsoport-kezelési .NET 4.5.2 vagy .NET 4.6 feltéve, hogy a .NET-keretrendszer célzott hello manuálisan telepíthető hello felhő szerepkör-szolgáltatás. Lásd: [.NET telepíthető egy felhőalapú szolgáltatás szerepkör](http://go.microsoft.com/fwlink/?LinkID=309796).

## <a name="service-bus-updates"></a>A Service Bus frissítések
* Az Event Hubs: 
  
  * Most már lehetővé teszi, hogy célzott hozzáférés-vezérlés jelentkezik, mintha a további publisher végpont az Event Hubs számára események küldésekor.
  * Stabilitását és fokozása hozzáadott tooEvent Hubs szolgáltatást.
  * Üzenetküldési felvételekor WebSocket keresztül Amqp protokoll támogatása és az Event Hubs.

## <a name="hdinsight-tools-for-visual-studio-updates"></a>A HDInsight Tools for Visual Studio frissítések
* **IntelliSense a fejlesztés**: távoli metaadatok javaslat
  
    A HDInsight Tools for Visual Studio támogatja távoli a metaadatokat a Hive parancsfájl szerkesztésekor. Beírhatja például **válasszon * FROM** és jelenik meg az összes hello táblanevek. Hello oszlopnevek is, megjelenik egy tábla megadása után.
* **HDInsight emulátor támogatása**
  
    A HDInsight Tools for Visual Studio támogatási csatlakozás tooHDInsight emulátor, így azt fejleszteni a Hive parancsfájlok helyileg vezet be, semmilyen költség nélkül most majd ezeket a HDInsight-fürtök parancsprogramok hajtható végre. 
  
    További információkért tekintse meg túl[a manuális](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).
* **A HDInsight Tools for Visual Studio támogatja az általános Hadoop-fürtök** (előzetes verzió)
  
    A HDInsight Tools for Visual Studio támogassák az általános Hadoop-fürtök, hogy használhassa a HDInsight Tools Visual Studio toodo hello következő:
  
  * Csatlakoztassa tooyour fürtöt 
  * továbbfejlesztett IntelliSense/automatikus-végrehajtási-támogatással rendelkező Hive-lekérdezések írása 
  * összes hello feladat megtekintéséhez a fürt egy egyszerűen elsajátítható felhasználói felületén. 
    
    További információkért tekintse meg túl[a manuális](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

## <a name="in-role-cache-updates"></a>Szerepköralapú gyorsítótár-frissítéseinek
* **Szerepköralapú gyorsítótár** frissített toouse lett **Microsoft Azure Storage szolgáltatás SDK** 4.3 verziója. Eddig hello **szerepköralapú gyorsítótár** használta az Azure Storage szolgáltatás SDK 1.7-es verziójának.
  
    Az ügyfelek használata az Azure SDK 2.5 vagy az alábbiakban kell módosítása tooAzure SDK 2.6 és áthelyezése az Azure Storage szolgáltatás SDK toohello új verziója. 
  
    Ez idő Azure Storage-verzió 2011-08-18-ra ütemezett toobe 2016 augusztusától 1 eltávolítani. Bármely áttelepítések szerepköralapú gyorsítótár az Azure SDK 2.5-ös vagy annak szintje alatt too2.6 időpontig kell fejeződnie. Az Azure Storage 2011-08-18-as verzió hello kivonása további információkért lásd: [Microsoft Azure Storage szolgáltatás eltávolítása Verziófrissítés: bővítmény too2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

> [!IMPORTANT]
> Jelenleg folyamatban bejelentése hello 30 2016 novemberétől kezdve az Azure Managed Cache Service és az Azure szerepköralapú gyorsítótár kivonásig. Azt javasoljuk, hogy az áttelepített tooAzure Redis Cache a használatból való kivonást előkészítésekor. A dátumok és áttelepítési útmutató további információkért lásd: [amely Azure Cache-ajánlatot megfelelő a számomra?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)
> 
> 

## <a name="azure-app-service-tools"></a>Az Azure App Service-eszközök
a következő elemek hello hello Azure SDK 2.6 kiadásban frissült.

* Az Azure közzétételi fokozott tooinclude Azure API-alkalmazások központi telepítés céljaként.
* API-alkalmazások létesítési funkció tooenable felhasználók API-alkalmazás létrehozása, és üzembe helyezési funkció.
* Server Explorer megváltozott tooreflect új App Service csomópont, a webes, mobil és API apps erőforráscsoport szerint csoportosítva.
* Adja hozzá az Azure API App ügyfél-hitelesítési mód hozzáadott toomost C# projekt, amely lehetővé teszi a Swagger-kompatibilis API-alkalmazások futtatása a felhasználó Azure-előfizetés automatikus előállításához.
* API-alkalmazások tooling és a Server Explorer App Service-csomópontok érhetők el a Visual Studio 2013 csak. 

## <a name="azure-resource-manager-tools-updates"></a>Frissíti az Azure erőforrás-kezelő eszközei
hello Azure resource manager eszközök törölték a virtuális gépekhez, hálózati és tárolási frissített tooinclude sablonok. hello JSON szerkesztési élmény lett frissített tooinclude sablonok és hello képességét tooedit hello sablonok JSON kódtöredékek használatával új vázlat nézet. A Visual Studio telepített sablonok biztosít hello projekt esetében, így a toohello parancsfájl végrehajtott módosításokat a Visual Studio által használandó PowerShell parancsfájl használata.

## <a name="diagnostics-improvements-for-cloud-services"></a>Cloud Services diagnosztika érintő fejlesztések
Az Azure SDK 2.6 számos lehetőséget kínál vissza hello Azure compute emulator a diagnosztikai naplók gyűjtésére, és mentse őket támogatása toodevelopment tároló. A diagnosztikai naplók (beleértve az alkalmazás nyomkövetési naplókat, esemény-nyomkövetés (ETW) Windows-naplók, a teljesítményszámlálók, az infrastruktúra naplók és a windows eseménynaplóit) előállított amikor hello alkalmazás fut hello emulátorban lehet átvitt toodevelopment tárolási tooverify, hogy a diagnosztikai naplózás működik a helyi számítógépen. 

hello diagnosztikai tárfiók most hello szolgáltatás konfigurációs (.cscfg) fájljában így egyszerűbb toouse különböző diagnosztikai tárfiók különböző környezetek adható meg. Bizonyos különbségek vannak a figyelmet a jelentősebb között hogyan Azure SDK 2.4 és az Azure SDK 2.6 működtek-e a hello kapcsolati karakterláncot. További információ a hogyan toouse hello diagnosztika tárolási kapcsolati karakterlánc, és milyen hatással van a projektek: [diagnosztika konfigurálása az Azure-szolgáltatásokhoz](http://go.microsoft.com/fwlink/?LinkID=532784).

## <a name="breaking-changes"></a>Módosítások megszakítása
### <a name="azure-resource-manager-tools"></a>Az Azure erőforrás-kezelő eszközei
* Hello **felhő telepítési projekt** elérhető Azure SDK 2.5 túl át lett nevezve hello projekttípus**Azure erőforráscsoport**.
* **A felhő telepítési projekt** hello Azure SDK 2.5 projektek típusú 2.6 is használható, de a Visual Studio eszközből telepítését hello sablon sikertelen lesz. Azonban a hello PowerShell-parancsfájl központi telepítése továbbra is működni fog, ahogy azt korábban.  Információ toouse **felhő telepítési projekt** 2.6, olvassa el ezt a [utáni](http://go.microsoft.com/fwlink/?LinkID=534086).

## <a name="known-issues"></a>Ismert problémák
* Egy 64 bites operációs rendszer hello emulátorban diagnosztikai naplók gyűjtésére van szükség. Ha egy 32 bites operációs rendszeren futtatja, a diagnosztikai naplók nem lesznek összegyűjtve. Ez nem befolyásolja a más emulátor funkciók. 
* 4/29/2015-én kiadott Azure SDK 2.6 kellett két problémákat: 
  
  * Univerzális alkalmazás nem sikerült betölteni a Visual Studio 2015, amikor Azure SDK 2.6 hello gépen lett telepítve.
  * Egy Felhőszolgáltatás-projekt hibakeresési sikertelen lesz a Visual Studio 2013 és a Visual Studio 2015, ahol a Visual Studio nem válaszol, és összeomlik "Configuring diagnosztika emulátor a" hello üzenettel párbeszédpanel megjelenítése során.
    
    Egy frissítés tooAzure SDK 2.6 5/18/2015 lett szabadítva. hello frissített verziója 2.6.30508.1601; a fent leírt két probléma javítását tartalmaz. Azonosíthatja, hogy hello hello buildverzióját SDK Vezérlőpult -> Programok és szolgáltatások -> Microsoft Azure-eszközök a Microsoft Visual Studio 2013 – v 2.6. hello verzió oszlop hello buildszáma jeleníti meg.
    
    Ha a fenti problémák hello továbbra is küzdenek, telepítse hello legújabb verzióját hello Azure 2.6 SDK for [Visual STUDIO 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [VS 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) vagy [VS 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).

## <a name="see-also"></a>Lásd még:
[Hello Azure SDK for .NET és API-k történő támogatása és kivonása](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

