---
title: "aaaAzure SDK for .NET 2.7 és .NET 2.7.1-es kibocsátási megjegyzések"
description: "Az Azure SDK for .NET 2.7 és .NET 2.7.1-es kibocsátási megjegyzések"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: 877d070a-9bd5-49b3-8fac-6bb5f65c3554
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 8ec72b0f18702e6d811f0cbe4790685be7881d96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Az Azure SDK for .NET 2.7 és .NET 2.7.1-es kibocsátási megjegyzések
## <a name="overview"></a>Áttekintés
Ez a dokumentum hello Azure SDK .NET 2.7 kiadás kibocsátási megjegyzései hello tartalmaz. 

hello dokumentum is tartalmaz hello kibocsátási megjegyzések a hello Azure SDK-t, a .NET 2.7.1-es kiadási.

Az Azure SDK 2.7 csak akkor támogatott a Visual Studio 2015-öt és a Visual Studio 2013. [Az Azure SDK 2.6](https://azure.microsoft.com/downloads/) hello utolsó támogatott SDK a Visual Studio 2012.

Ebben a kiadásban kapcsolatos részletes információkért lásd: [Azure SDK 2.7 közlemény post](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) és [Azure SDK 2.7.1-es közlemény post](http://go.microsoft.com/fwlink/?LinkId=623850).

## <a name="azure-sdk-for-net-27"></a>Azure SDK for .NET 2.7
### <a name="sign-in-improvements-for-visual-studio-2015"></a>Jelentkezzen be a Visual Studio 2015 fejlesztései
A Visual Studio 2015 Azure SDK 2.7 hello új identitáskezelési funkciókat támogatja a Visual Studio 2015-öt.  Ez magában foglalja a fiókokhoz való hozzáférés az Azure szerepköralapú hozzáférés-vezérlés, a felhőalapú megoldás szolgáltatókat, a DreamSpark és a fiókja és -előfizetése egyéb keresztül támogatása.

a Visual Studio 2015 hello bejelentkezési tartalmaz módosításokat az Azure SDK 2.7 csak érhetők el. Azure SDK 2.7.1-es támogatja a Visual Studio 2013 része.

### <a name="mobile-sdk"></a>Mobil SDK
Frissített **Mobile Apps** sablonok tooreflect legújabb [NuGet-csomag](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) és telepítési folyamata.

### <a name="service-bus"></a>Service Bus
Általános hibajavításokat és fejlesztéseket. A részletek a frissítésekkel és szolgáltatásokkal, tekintse meg az toohello kibocsátási megjegyzések a legújabb hello [Service Bus NuGet](http://www.nuget.org/packages/WindowsAzure.ServiceBus/).

### <a name="hdinsight-tools"></a>A HDInsight Tools
Ebben a kiadásban hello a következő frissítés megtörtént. Ezek a frissítések még csak előzetes verziójúak. További információkért lásd: [ebben a blogban](http://go.microsoft.com/fwlink/?LinkId=619108).

* Hive diagramok a Hive on Tez feladatokhoz
* A DML-IntelliSense Hive teljes körű támogatása
* A Pig sablon támogatása
* A Storm sablonok az Azure-szolgáltatások

#### <a name="breaking-changes"></a>Módosítások megszakítása
* Régi **Storm** projekt hello eszközök ezen verziója használatakor frissíteni kell. További információkért lásd: [ebben a blogban](http://go.microsoft.com/fwlink/?LinkId=619108).
* A Visual Studio Web Express már nem támogatott. További információkért lásd: [ebben a blogban](http://go.microsoft.com/fwlink/?LinkId=619108).

### <a name="azure-app-service-tools"></a>Az Azure App Service-eszközök
Ebben a kiadásban hello a következő frissítések tooWeb eszközök bővítmények került sor. További információ: [ez](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blog. 

* DreamSpark-fiókok hozzáadott támogatása
* TooAzure toosupport hello új Azure Resource API-val készített eszközök teljes módosítása
* Az Azure App Service-támogatás hozzá túl[Cloud Explorer](#cloud_explorer)

#### <a name="known-issues"></a>Ismert problémák
Webes alkalmazás központi telepítési tárhely csomópontot nem a Server Explorer hello üzembe helyezési ponti csomópontban jelennek meg, és a webes alkalmazás központi telepítési tárolóhely gyermekcsomópontok nincs betöltés Cloud Explorer alatt. A probléma meg lett oldva és előkészítve a hello következő SDK-verzióban. 

### <a name="cloud_explorer"></a>A Visual Studio 2015 cloud Explorer
Azure SDK 2.7 tartalmaz Cloud Explorerben a Visual Studio 2015, ami lehetővé teszi a tooview az Azure-erőforrások, vizsgálja meg a tulajdonságaikról és műveleteket kulcs fejlesztők a Visual Studio. 

Cloud explorer hello következőket támogatja:

* Az Azure-erőforrások erőforráscsoport és az erőforrástípus nézetei 
* Erőforrások keresése név (erőforrástípus nézetben érhető el) alapján
* Az előfizetések és -erőforrásokra, amelyekre a szerepköralapú hozzáférés vezérlés (RBAC) alkalmazott támogatása 
* Integrált műveletpulton szerinti műveletek developer-arra irányul, hogy adott tooselected erőforrásokat. Például: távoli hibakereső csatlakoztatása a virtuális gépek használatával létrehozott hello Resource Manager-készletben, diagnosztikai adatok megtekintése a virtuális gépek stb.
* Integrált Tulajdonságok panel fejlesztés/tesztelés során általában szükség fejlesztői tárgyalt tulajdonságai ablakban 
* Gyors váltás hello fiók toouse, amikor (használja a beállítások parancsot eszköztár) erőforrások számbavétele 
* Az előfizetések toouse szűrést, ha (használja a beállítások parancsot eszköztár) erőforrások számbavétele 
* Mélyhivatkozással toohello Azure portálon az erőforrások és csoportok kezelése 

### <a name="azure-resource-manager-tools"></a>Az Azure erőforrás-kezelő eszközei
hello Azure Resource Manager eszközök a szerepkör alapú hozzáférés vezérlés (RBAC) és új előfizetéstípusok frissített toowork törölték.  Ezek a változások mellékelt van hello képességét toouse új tárfiókot, továbbá tooclassic tárolási toostore összetevők telepítése során.  

Az SDK 2.7 hello használata az Azure erőforráscsoport-projekt hello SDK korábbi verziójából származó, egy új központi telepítési parancsfájlt az új tárfiók helyett hagyományos tárolási szükséges toodeploy.  Mielőtt módosításai tooyour projekt tooadd hello új parancsfájl kéri.  hello régi parancsfájl kap, és újra kell toomanually bármilyen módosítást toohello új parancsfájl.

### <a name="storage-explorer-tools"></a>Tárolási Explorer Eszközök
* Támogatás a hozzáfűző BLOB megtekintése. További információ a [ebben a blogbejegyzésben](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
* Prémium szintű Storage-fiókok a Server Explorer megtekintése támogatása. Prémium szintű storage-fiókok csak a támogatott hello típus szerint Server Explorer csak jelenik meg prémium szintű storage-fiókok lapblobokat.

### <a name="azure-data-factory-tools-for-visual-studio"></a>Az Azure Data Factory Tools for Visual Studio
Introducing **az Azure Data Factory eszközök** Visual Studio. Az alábbiakban hello engedélyezett szolgáltatások. Lásd: [ebben a blogban](http://go.microsoft.com/fwlink/?LinkId=617530) további információt.

* **Sablon alapú szerzői**: alapján sablonok használata cased, adatok mozgása sablonok vagy az adatfeldolgozás sablonok toodeploy adatok végpontok közötti integrációs megoldás kiválasztása és az első lépések gyorsan adat-előállító gyakorlatban használják. 
* **Integráció a Solution Explorer szerzői és központi telepítése a Data Factory entitások**: központi telepítése, folyamatok és a kapcsolódó entitásokból, mint a Visual Studio-projektek & létrehozása. 
* **Integráció a Diagram nézet visual kapcsolati tevékenység készítése során**: vizuálisan létre adatcsatornák és a Diagram nézet hello támogatással adatkészletek. 
* **Integráció a Server explorer keresse meg és a már telepített entitások való együttműködéshez**: emelés hello Server Explorer toobrowse már telepítették az adat-előállítók és a megfelelő entitásokat. Egy telepített adat-előállító vagy egyetlen entitás (sorban, a társított szolgáltatás, adatkészleteket) importálnia kell a projekthez. 
* **JSON Szerkesztés a séma érvényesítése és a gazdag intellisense**: hatékonyan konfigurálása, és az adat-előállító entitások gazdag intellisense és a séma érvényesítése a JSON-dokumentumok szerkesztése 
* **Több környezet közzététel**: szerzői folyamatok toodev, a vizsgálat vagy a termék környezet közzététele minden környezet esetében külön konfigurációs fájlok létrehozásával.
* **A Pig, a Hive és a .net-alapú adatfeldolgozási támogatási**: támogatja a Pig és a Hive parancsfájlok Data Factory-projektben. C#-projektet hivatkozó .net kezelésére szolgáló támogatása tevékenység.

## <a name="azure-sdk-for-net-271"></a>Azure SDK a .NET 2.7.1-es verziójához
a következő szakasz hello bevezetett hello Azure SDK-t a .NET 2.7.1-es vonatkozó frissítéseket tartalmazza.

### <a name="hdinsight-tools"></a>A HDInsight Tools
Részletesebb tájékoztatás a HDInsight eszközök frissítések, lásd: [ebben a blogban](http://go.microsoft.com/fwlink/?LinkId=623831).

* Hive feladat operátor nézet (egy új szolgáltatás)
  
    a Hive-lekérdezések jobb-hello Hive operátor nézet szolgáltatás tisztában toohelp hozzá lett adva. toosee összes hello operátort csúcspont, kattintson duplán a hello feladatgrafikon hello csúcspont. tooview adott operátor rámutat egy adott operátor további részleteit.
* Hive hiba jelölő (egy új szolgáltatás)
  
    tooenable tooview hello nyelvtani hibákat azonnal, hello Hive hiba jelölő szolgáltatás meg lett hozzáadva. Ezenkívül hibaüzenetek volt fokozott, és azonnal részletes nyelvtani hibákat most már megtekintheti (ebben a kiadásban, amíg nem volt toosubmit a Hive parancsfájl toohello fürt, és várjon egy kis ideig, a hibaüzenet részleteit elérése előtt).  
* Storm-topológia Graph (egy új szolgáltatás)
  
    Megjelenítése akkor nagyon fontos, ha azt szeretné toosee, ha a topológia a várt módon működik. Ebben a kiadásban hozzáadott képi megjelenítés a Storm diagramjait. Hello fontos metrikák jelenítheti meg a topológia (például egy színt azt jelzi, időjárási egy bizonyos Bolt "foglalt" vagy nem). Duplán kattintva hello Bolt/Spout tooview további részleteket.
* Hello Azure Portal (hibajavítás) létrehozott HDInsight-fürtök támogatása
  
    Most már használhatja a Visual Studio tooview és küldje el a HDInsight-fürtök függetlenül attól, hol hello fürt létrehozott feladatok tooall.
* További támogatási IntelliSense & gyorsabban Hive metaadatok betöltése (javítása)
  
    Továbbfejlesztettük hello IntelliSense további felhasználóbarát javaslatok hozzáadásával. Például tábla alias is most is a javasolt az IntelliSense így könnyebben írhat a lekérdezést. Emellett továbbfejlesztettük hello Hive-metaadatok betöltése, így csak néhány másodperc toolist összes hello adatbázisok, táblák és a Hive metaadattárhoz oszlopait.

Részletesebb tájékoztatás a HDInsight eszközök frissítések, lásd: [ebben a blogban](http://go.microsoft.com/fwlink/?LinkId=623831).

### <a name="improvements-in-visual-studio-2013"></a>A Visual Studio 2013 fejlesztései
* Az Azure SDK 2.7.1-es lehetővé teszi a Visual Studio 2013 tooaccess Azure fiókok és a szerepköralapú hozzáférés-vezérlés, a megoldás szolgáltatók és a Dreamspark-előfizetések.
* Az Azure SDK-val 2.7.1-es hello új Cloud Explorer eszköz ablak már is elérhető a Visual Studio 2013.

### <a name="known-issues"></a>Ismert problémák
Hello Azure SDK 2.6 vagy telepítése 2.7.1-es a Visual Studio Community 2013 a egy nem - angol nyelvű operációs rendszer figyelmeztetés jelenik meg, amely hello angol és a Visual Studio nem angol nyelvű erőforrásokat lehet, hogy egyeznek. Ez a figyelmeztetés lehet, hogy el biztonságosan. Ha hello machine nem tartalmazta a számítógépen megtalálható a Visual Studio Community 2013, és telepíti hello SDK meg egy nem - angol nyelvű operációs rendszer akkor történik. hello figyelmeztetés látható hello nyelvi csomag hello RTM erőforrások tooVisual Studio vonatkozik, de előtt Update 4 vonatkozik. Hello figyelmeztetés kikapcsolásakor lehetővé teszi hello nyelvi csomag toocontinue és teljes alkalmazása hello Update 4 hello nyelvi csomag verzióját tartalom.

LightSwitch-projektek nincsenek compatibile ebben a kiadásban. A probléma a következő SDK kiadási hello megszűnik.

## <a name="also-see"></a>Lásd még:
[Az Azure SDK 2.7.1-es közlemény post](http://go.microsoft.com/fwlink/?LinkId=623850)

[Az Azure SDK 2.7 közlemény post](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Hello Azure SDK for .NET és API-k történő támogatása és kivonása](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

