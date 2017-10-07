---
title: "aaaAzure SDK for .NET 2.8 kibocsátási megjegyzései"
description: "Az Azure SDK for .NET 2,8 kibocsátási megjegyzései"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: de7207ff-ba4f-4008-9141-8742fcaa3254
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 2722dcdd27bf9ab65b48e91dcdb545536f987dd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-28-281-and-282"></a>2.8, 2.8.1-es verziójának és 2.8.2 .NET-keretrendszerhez készült Azure SDK
## <a name="overview"></a>Áttekintés
Ez a cikk hello kibocsátási megjegyzéseket (ismert problémák és a jelentős változásokat tartalmaz) a .NET 2.8 2.8.1-es verziójának és 2.8.2 kiadott hello Azure SDK tartalmazza. 

Új szolgáltatásokat és az ebben a kiadásban végrehajtott frissítések teljes listájáért lásd: hello [Azure SDK 2.8 a Visual Studio 2013 és a Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) közlemény. 

## <a name="azure-sdk-for-net-28"></a>Azure SDK for .NET 2.8
### <a name="download-azure-sdk-for-net-28"></a>2.8 .NET-keretrendszerhez készült Azure SDK letöltése
[Az Azure SDK for .NET 2.8 a Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Az Azure SDK for .NET 2.8 a Visual Studio 2013-hoz](http://go.microsoft.com/fwlink/?LinkId=699287)

### <a name="net-452-support"></a>Támogatja a .NET 4.5.2.
#### <a name="known-issues"></a>Ismert problémák
Az Azure .NET SDK 2.8 lehetővé teszi, hogy Ön toocreate .NET 4.5.2 Cloud Service-csomagok. Azonban .NET 4.5.2 keretrendszer hello alapértelmezett kiadási vendég operációs rendszer lemezképek csak 2016. január vendég operációs rendszer nem lesz telepítve. Adott, a .NET 4.5.2 előtt keretrendszer egy külön vendég operációs rendszer verziója – 2015-02 November érhető el. Lásd: hello [Azure vendég operációs rendszereinek kiadásait és SDK-kompatibilitási mátrixát](../cloud-services/cloud-services-guestos-update-matrix.md) lap tootrack amikor kiadjuk az hello kép.  Miután hello November 2015-02 kép megjelenik szerint is választhat toouse lemezkép frissítése a felhő szolgáltatás konfigurációs fájlját (.cscfg) fájljában. A hello szolgáltatás konfigurációs fájl hello osVersion attribútum hello ServiceConfiguration elem toohello karakterlánc beállítása "WA-VENDÉG-operációsrendszer-4.26_201511-02". Ha ez a rendszerkép kiválasztása tooopt a toouse majd többé nem kap az automatikus frissítések toohello vendég operációs rendszer. tooget hello automatikus frissítések hello osVersion túl állítható be "*" és a .NET 4.5.2 csak az automatikus frissítések 2016. január érhető el.

### <a name="azure-data-factory"></a>Azure Data Factory
#### <a name="known-issues"></a>Ismert problémák
Során egy **Data Factory sablon** projekt létrehozása érintő mintaadatok, az azure PowerShell parancsfájl meghiúsulhat, ha azure power shell hello gépen telepített verziója 0.9.8-as után.

Ahhoz, toosuccessfully hozzon létre ilyen típusú projekt, telepítenie kell a [azure power shell verzió 0.9.8-as](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).

### <a name="azure-resource-manager-tools"></a>Az Azure erőforrás-kezelő eszközei
#### <a name="breaking-changes"></a>Módosítások megszakítása
hello hello Azure erőforráscsoport-projekt által biztosított PowerShell-parancsfájl frissültek a kiadási toowork hello új Azure PowerShell-parancsmagokkal 1.0-s verziója.  Ez a parancsfájl nem fog működni a Visual Studio egy hello SDK előzetes too2.8 verziójának használatakor.  

Hello SDK korábbi verzióiban létrehozott projektek parancsfájlok nem fog futni a Visual Studio amikor 2,8 SDK használatával hello.  Az összes parancsfájl továbbra is toowork kívül Visual Studio hello Azure PowerShell-parancsmagok a hello megfelelő verziójával.  

hello 2,8 SDK szükséges hello Azure PowerShell-parancsmagok 1.0-s verzióját.  Hello SDK egyéb verziói hello Azure PowerShell-parancsmagok 0.9.8-as verziója szükséges.  További információ: [ez](http://go.microsoft.com/fwlink/?LinkID=623011) blog.

### <a name="web-tools-extensions"></a>Webes bővítmények eszközei
#### <a name="known-issues"></a>Ismert problémák
hello a következő ismert problémák kiiktatása a kiadásban a következő hello.

* App Service kapcsolódó felhő és a Server Explorer nem éles környezetben (például az Azure China vagy Azure verem ügyfelek) hitelesítési mód nem működnek. Az ügyfelek érintett évi hello letöltése közzététele hello Azure-portálon a profil lesz lehetőség van a közzététel. Egy későbbi kiadásban fog javítása kézmozdulatok például "Csatolása hibakereső" és "Folyamatos átviteli naplók megtekintése" Azure Kína és verem ügyfelek. 
* Ügyfelek App Service USA keleti régiója nem abban a régióban van létrehozása, amikor hello App Insights példány toowhich azok telepítése során hiba jelenhet meg. Ezekben az esetekben egy App Service létrehozása hello portálon, és hello letöltése közzétételi profil közzétételi forgatókönyvek engedélyezi. 

### <a name="azure-hdinsight-tools"></a>Az Azure HDInsight eszközök
#### <a name="new-updates"></a>Új frissítések
* A Hive-lekérdezést végrehajtani hello fürt hiveserver2-n keresztül, szinte nincs terhelés mellett, és tekintse meg a hello feladat jelentkezik be a valós idejű.
* Hello segítségével új Hive feladatvégrehajtási nézet meg is elmerülne a rendszer a feladat mélyebb, a további részletekért, és lehetséges problémák azonosítása.

További információ: [Azure SDK 2.8 a Visual Studio 2013 és a Visual Studio 2015-öt](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure SDK a .NET 2.8.1-es verziójához
### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>A Visual Studio 2013 és a Visual Studio 2015-öt kapcsolatos ismert problémák
1. Indított webjobs-feladat közzéteszi tooslots lesz megjelenítése és a hiba, és nem beállított ütemezés szerint, de küldi hello webjobs-feladat tooAzure. Az ügyfelek, akik le egy ütemezett feladatot kell majd használható hello Azure Portal tooset hello ütemezés be hello webjobs-feladat. 
2. Python ügyfelek hibakereső problémákba ütközhetnek. Csoport folyamatban van egy javítsa ki a, de ha ügyfelet érintenének, kérjük, tájékoztasson Microsoft hello fórumokon vagy hello közlemény blogjában tudja, vagy a kibocsátási megjegyzések Megjegyzések szakasz. 
3. Felhasználók (például a Dél-India) egyes régiókban fog tapasztalni App Service-kiépítési hibák. Megfelel az hello portálon, és az ügyfelek, akik a probléma használhatja az Azure portál toorequest hozzáférés toopublish toothese földrajzi-régiók hello. Miután kérnek hozzáférést toothese régiókat hello Azure portál kiépítés kell működnie. 

## <a name="azure-sdk-for-net-282"></a>Az Azure SDK for .NET 2.8.2
Hello telepítése után hello 2.8.2 eszközök ügyfél problémákat tapasztalhat a probléma a következő hello.         

* Ha a Windows 10 használata nem telepítette az Internet Explorer, előfordulhat, hogy "Az Internet Explorer nem található" hibaüzenetet kap.
  tooresolve hello probléma, telepítse az Internet Explorer használatával hello hozzáadása/eltávolítása Windows-összetevők párbeszédpanel.

Ha a probléma, használja a Küldés a mosolynál hello szolgáltatás tooreport azt.

További információkért lásd: [ez](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) utáni.

## <a name="other-updates"></a>Egyéb frissítések
Más frissítéseket, lásd: [Azure SDK 2.8 közlemény post](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="also-see"></a>Lásd még:
[Az Azure SDK 2.8 közlemény post](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Hello Azure SDK for .NET és API-k történő támogatása és kivonása](https://msdn.microsoft.com/library/azure/dn479282.aspx)

