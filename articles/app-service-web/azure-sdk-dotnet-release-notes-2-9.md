---
title: "aaaAzure SDK for .NET 2.9 kibocsátási megjegyzései"
description: "Az Azure SDK for .NET 2.9 kibocsátási megjegyzések"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 96df2b80224190cc2093e6bf350eaec224ac2e98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-29-release-notes"></a>Az Azure SDK for .NET 2.9 kibocsátási megjegyzései

Ez a témakör a .NET 2.9 és az Azure SDK 2.9.6 kibocsátási megjegyzései tartalmaz.

##<a name="azure-sdk-for-net-296-release-summary"></a>Az Azure SDK for .NET 2.9.6 kiadási összegzése

Kiadás dátuma: 2016. 11/16
 
Ebben a kiadásban nem a legfrissebb változtatásokat toohello Azure SDK 2.9 vezettek be. Nincs még nincs szükség, a frissítési folyamat tooleverage az SDK-val meglévő Felhőszolgáltatás-projektek.

### <a name="visual-studio-2017-release-candidate"></a>A Visual Studio 2017 kiadásra jelölt verziójára

- A Visual Studio 2017 RC ezen kiadásával a hello Azure SDK for .NET részét hello Azure munkaterhelés. Minden hello eszközökkel toodo Azure fejlesztési továbbítja a Visual Studio 2017 RC része lesz. A Visual Studio 2015-öt és a Visual Studio 2013 hello SDK továbbra is elérhetők WebPI keresztül. Azt fogja kell megszűnő Azure SDK .NET kiadásokban a Visual Studio 2013, ha a Visual Studio 2017 feloldja a végső termék. Kövesse a hivatkozást toodownload Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/

### <a name="azure-diagnostics"></a>Azure Diagnostics

- Módosított hello viselkedés tooonly részleges kapcsolati karakterlánc szerepét a Cloud Services diagnosztika tárolási kapcsolati karakterlánc egy token hello kulccsal tárolja. hello tényleges biztonságitár-kulcs most tárolja a hello felhasználói profil mappában, így a hozzáférése vezérelhető. A Visual Studio hello biztonságitár-kulcs olvasását helyi Hibakeresés és a közzétételi folyamat felhasználói profil mappájába. 
- Válasz toohello módosítása a fent leírt, a Visual Studio Online csapat továbbfejlesztett hello Azure Cloud Services központi telepítési sablon, a felhasználók hello kulcs diagnosztika bővítmény beállításához folyamatos integrációt tooAzure közzétételekor kell megadni és üzembe helyezés.
- Az hajtottunk lehetséges toostore a biztonságos kapcsolati karakterláncot és létrehozása az Azure Diagnostics (ÜVEGVATTA), toohelp konfigurációjával kapcsolatos problémák megoldása environements között.
 
### <a name="windows-server-2016-virtual-machines"></a>Windows Server 2016-os virtuális gépek

- A Visual Studio mostantól támogatja a központi telepítés Felhőszolgáltatások tooOS termékcsalád 5 (Windows Server 2016) virtuális gépeket. A meglévő felhőszolgáltatások, módosíthatja a beállításokat tootarget új operációsrendszer-család hello. Amikor új felhőalapú szolgáltatások hoz létre, ha úgy dönt, hogy toocreate hello szolgáltatást használó .net 4.6-os vagy újabb, az alapértelmezés szerint hello szolgáltatás toouse operációsrendszer-család 5.  További információkért tekintse át hello [Vendég operációsrendszer-család támogatja a tábla](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).

#### <a name="known-issues"></a>Ismert problémák

- Az Azure .NET SDK 2.9.6 rendszerben jelent meg, amely megakadályozza a központi telepítés használata nem támogatott a .NET keretrendszer (például .NET 4.6) tooany operációsrendszer-család projektek korlátozás < 5. A megoldás biztosítja [Itt](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).

 
### <a name="azure-in-role-cache"></a>Azure szerepköralapú gyorsítótár 

- Támogatás a 2016. November 30 Azure szerepköralapú gyorsítótár végződik. További információért kattintson [Itt](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).

### <a name="azure-resource-manager-templates-for-azure-stack"></a>Az Azure-bA az Azure Resource Manager-sablonok a verem

- A Microsoft már megjelent Azure verem telepítési cél az Azure Resource Manager-sablonok.


## <a name="azure-sdk-for-net-29-summary"></a>Az Azure SDK for .NET 2.9 összegzése

## <a name="overview"></a>Áttekintés
Ez a dokumentum hello Azure SDK .NET 2.9 kiadás kibocsátási megjegyzései hello tartalmaz. 

Ebben a kiadásban frissítésekkel kapcsolatos részletes információkért lásd: hello [Azure SDK 2.9 közlemény post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>A Visual Studio 2015 Update 2 és a Visual Studio "15" az Azure SDK 2.9 megtekintése
A frissítés tartalmazza a következő hibajavítások hello:

* Kapcsolódó tooREST API-ügyfél generációs ki a mely hello karakterláncban "Ismeretlen Type" jelent hello hello kód-generációs mappa neve és/vagy a generált hello kódra eldobott hello névtér hello nevét.
* Ki, melyik hello a hitelesítési adatokat nem működött toohello Feladatütemező üzembe helyezési folyamat átadott toobe kapcsolódó tooScheduled webjobs-feladatok.

A frissítés tartalmazza a következő új szolgáltatás hello:

* Másodlagos alkalmazásszolgáltatások üzembe helyezési App Service párbeszédpanelen hello hello "Szolgáltatások" lapján támogatása. 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Az Azure Data Lake Tools for Visual Studio 2015 Update 2
A frissítések hello következőket tartalmazza:

* **Az Azure Data Lake Tools** a Visual Studio most egyesítve hello Azure SDK .NET kiadáshoz. hello eszköz automatikusan települ Azure SDK telepítésekor. 
  
    hello eszköz gyakran frissül, nyissa meg [Itt](http://aka.ms/datalaketool) tooget hello frissítések.
* **Server Explorer** mostantól lehetővé teszi az összes tooview és néhány U-SQL-metaadatok entitásokat hozhatnak létre. További információkért lásd: [ez](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.

## <a name="hdinsight-tools"></a>A HDInsight Tools
**A HDInsight Tools** mostantól támogatja a HDInsight 3.3-as, beleértve a Tez ábrákat és egyéb nyelvi verzió kijavítja a Visual Studio.

## <a name="azure-resource-manager"></a>Azure Resource Manager
Ez a kiadás [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) Resource Manager-sablonok támogatása.

## <a name="see-also"></a>Lásd még:
[Az Azure SDK 2.9 közlemény post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

