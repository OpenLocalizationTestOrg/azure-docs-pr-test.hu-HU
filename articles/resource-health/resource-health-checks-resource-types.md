---
title: "Azure-erőforrás állapota keresztül erőforrástípusok aaaSupported |} Microsoft Docs"
description: "Támogatott erőforrástípus keresztül Azure-erőforrás állapota"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: resource-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 03/19/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: a37d5c33f6ef6fdfbce0daf392bc7fa57853c7d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resource-types-and-health-checks-in-azure-resource-health"></a>Erőforrástípusok és állapotát ellenőrzi az Azure-erőforrás állapota
Az alábbiakban az erőforrás állapota keresztül hajtja végre erőforrástípusok összes hello ellenőrzések teljes listáját.

## <a name="microsoftcacheredisredis"></a>Microsoft.CacheRedis/Redis
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Minden hello gyorsítótár-csomópontok fel, és futtatja?</li><li>Hello gyorsítótárában elérhető a hello adatközponton belül?</li><li>Rendelkezik a gyorsítótár elérte a kapcsolatok maximális számát hello hello?</li><li> Hello gyorsítótár kimerítette a rendelkezésre álló memória? </li><li>Hello gyorsítótár nagy mennyiségű laphibát tapasztal?</li><li>Túl nagy terhelés alatt van hello a gyorsítótár?</li></ul>|

## <a name="microsoftcdnprofile"></a>Microsoft.CDN/profile
|Végrehajtott ellenőrzések|
|---|
|<ul> <li>Bármely hello végpontok leállt, eltávolított vagy helytelenül konfigurált?</li><li>Az elérhető a CDN a konfigurálási műveletek hello kiegészítő portálon?</li><li>Vannak-e folyamatban lévő kézbesítési problémái hello CDN-végpontok?</li><li>Módosíthatja a felhasználók a CDN erőforrásaik hello konfigurációját?</li><li>Konfigurációs módosítások várt hello díj propagálása vannak?</li><li>Kezelheti a felhasználók hello CDN konfigurációs hello Azure-portálon, a PowerShell vagy a hello API használatával?</li> </ul>|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.classiccompute/virtualmachines
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Hello kiszolgáló működik-e és fut?</li><li>Befejeződött hello állomás operációs rendszer rendszerindítást?</li><li>Hello virtuálisgép-tárolót üzembe helyezve és kapcsolt?</li><li>Hello állomás és hello tárfiók közötti hálózati kapcsolat van?</li><li>Hello rendszerindítását hello vendég operációs rendszer befejeződött?</li><li>Nem tervezett karbantartást?</li></ul>|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.cognitiveservices/accounts
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Hello fiók elérhető a hello adatközponton belül?</li><li>Van hello kognitív szolgáltatások erőforrás-szolgáltató elérhető?</li><li>Van hello kognitív hello megfelelő régióban elérhető szolgáltatás?</li><li>Elolvashatják műveletek hajthatók végre hello erőforrás metaadatok okozó hello tárfiók?</li><li>Elérte a hello API-hívás kvóta?</li><li>Elérte a hello API hívása olvasás-korlátot?</li></ul>|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.COMPUTE/virtualmachines
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Hello server mentése a virtuális gépet szolgáltató, és fut?</li><li>Befejeződött hello állomás operációs rendszer rendszerindítást?</li><li>Hello virtuálisgép-tárolót üzembe helyezve és kapcsolt?</li><li>Hello állomás és hello tárfiók közötti hálózati kapcsolat van?</li><li>Hello rendszerindítását hello vendég operációs rendszer befejeződött?</li><li>Nem tervezett karbantartást?</li></ul>|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.datalakeanalytics/accounts
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Is submit feladatok tooData Lake Analytics a felhasználók hello régió?</li><li>Alapvető feladatok lefutott, és sikeresen megtörtént a teljes hello régió elvégezni?</li><li>Felhasználók készíthetünk katalóguselemek hello régióban?</li>|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.datalakestore/accounts
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Feltöltheti a felhasználók tooData Lake adattárban hello régióban?</li><li>Felhasználók is a Data Lake Store hello régióban le adatokat?</li></ul>|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.documentdb/databaseAccounts
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Nem volt tooa DocumentDB szolgáltatás elérhetetlensége miatt nem szolgálható ki semmilyen adatbázis vagy a gyűjtemény kérelmet?</li><li>A dokumentum kéréseit nem szolgálható ki tooa DocumentDB szolgáltatás elérhetetlensége miatt nem lett volna?</li></ul>|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/Connections
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Hello VPN-alagút csatlakoztatva van?</li><li>Vannak-e konfigurációs ütközést hello kapcsolatban?</li><li>Hello előmegosztott kulccsal megfelelően legyenek konfigurálva?</li><li>Hello VPN helyi eszköz érhető el?</li><li>Vannak-e eltérést hello IPSec/IKE biztonsági házirendben?</li><li>Az hello S2S VPN-kapcsolat megfelelően kiosztott vagy hibás állapotban lévő?</li><li>Megfelelően kiosztott vagy hibás állapotban, az hello VNET – VNET-kapcsolatot?</li></ul>|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.network/virtualNetworkGateways
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Hello VPN-átjáró elérhető az hello internet?</li><li>Készenléti üzemmódban van hello a VPN-átjáró?</li><li>Hello VPN szolgáltatás fut a hello átjáró?</li></ul>|

## <a name="microsoftnotificationhubsnamespace"></a>Microsoft.NotificationHubs/namespace
|Végrehajtott ellenőrzések|
|---|
|<ul><li> Regisztráció, a telepítés vagy a küldési futásidejű műveletek a végrehajtható hello névtér?</li></ul>|

## <a name="microsoftpowerbiworkspacecollections"></a>Microsoft.PowerBI/workspaceCollections
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Hello gazda operációs rendszer működik-e és fut?</li><li>Az elérhető hello adatközponton kívülről hello workspaceCollection?</li><li>Van rendelkezésre álló Power bi erőforrás-szolgáltató hello?</li><li>Van hello hello megfelelő régióban elérhető Power bi szolgáltatás?</li></ul>|

## <a name="microsoftsearchsearchservices"></a>Microsoft.search/searchServices
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Diagnosztikai műveletek végrehajtható hello fürtön?</li></ul>|

## <a name="microsoftsqlserverdatabase"></a>Microsoft.SQL/Server/database
|Végrehajtott ellenőrzések|
|---|
|<ul><li> Nem volt bejelentkezések toohello adatbázis?</li></ul>|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs
|Végrehajtott ellenőrzések|
|---|
|<ul><li>A rendszer minden hello gazdagép, ahol hello feladat végrehajtása fel, és fut?</li><li>Hello feladat nem toostart volt?</li><li>Vannak-e folyamatban lévő futásidejű frissítéseket?</li><li>Hello feladat (például fut vagy leállt ügyfél) várt állapotban van?</li><li>Hello feladat észlelt kimenő memória kivételek?</li><li>Vannak-e folyamatban lévő ütemezett számítási frissítéseket?</li><li>A végrehajtás-kezelő (vezérlési tervek) elérhető van hello?</li></ul>|

## <a name="microsoftwebserverfarms"></a>Microsoft.web/serverFarms
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Hello kiszolgáló működik-e és fut?</li><li>Az Internet Information Services fut?</li><li>Hello terheléselosztó fut?</li><li>Hello Web Service-csomag elérhető a hello adatközponton belül?</li><li>E hello tárolási fiók üzemeltetési hello helyek hello kiszolgálófarm az elérhető??</li></ul>|

## <a name="microsoftwebsites"></a>Microsoft.Web/Sites
|Végrehajtott ellenőrzések|
|---|
|<ul><li>Hello kiszolgáló működik-e és fut?</li><li>Internet Information server fut?</li><li>Hello terheléselosztó fut?</li><li>Hello webalkalmazás elérhetőségének a hello adatközponton belül?</li><li>Hello tárfiók üzemeltető hello hely tartalom érhető el?</li></ul>|

Lásd: ezen erőforrás állapota kapcsolatos további erőforrások toolearn:
-  [Bevezetés tooAzure erőforrás állapota](Resource-health-overview.md)
-  [Azure-erőforrás állapotának kapcsolatos gyakori kérdések](Resource-health-faq.md)

