---
title: "API-k számlázási aaaAzure |} Microsoft Docs"
description: "Tudnivalók Azure számlázási használati és RateCard API-k, amelyek az Azure erőforrás-felhasználás és trendek használt tooprovide betekintést."
services: 
documentationcenter: 
author: BryanLa
manager: tonguyen
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/18/2017
ms.author: mobandyo;bryanla
ms.openlocfilehash: b3214996cc3279f76fdc7f0dbd2059c3ae7bb15c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-billing-apis-tooprogrammatically-get-insight-into-your-azure-usage"></a>Azure számlázás API-k tooprogrammatically használata az Azure használatának webhelynaplókat
Azure számlázási API-k toopull használati és erőforrás-adatok használja azokat az elsődleges adatok elemzésére szolgáló eszközöket. hello Azure erőforrás-használat és RateCard API-k segítségével pontosan előre jelezni, és a költségek kezelésére. hello API-k által hello Azure Resource Manager API-k hello operációsrendszer-család része és erőforrás-szolgáltató valósíthatók meg.  

## <a name="azure-invoice-download-api-preview"></a>Az Azure számla letöltési API (előzetes verzió)
Egyszer hello [részt lett teljes](billing-manage-access.md#opt-in), letöltési számlák hello előzetes verzióját használja [számla API](/rest/api/billing). hello funkciók a következők:

* **Szerepköralapú hozzáférés-vezérlés az Azure** -hozzáférés konfigurálása hello házirendek [Azure-portálon](https://portal.azure.com) vagy [Azure PowerShell-parancsmagok](/powershell/azure/overview) mely felhasználók vagy alkalmazások ingyenesen juthatnak az toospecify toohello előfizetés használati adatok. Hívóknak kell használnia szabványos Azure Active Directory-jogkivonatokat a hitelesítéshez. Hello hívó tooeither hello számlázási olvasó, olvasó, tulajdonosi vagy közreműködői szerepkör tooget hozzáférés toohello használati adatok hozzáadása egy adott Azure-előfizetéséhez.
* **Dátum szerinti szűrés** -használata hello `$filter` paraméter tooget összes hello számlák által hello fordított időrendi sorrendben számla időszak vége dátuma. 

> [!NOTE]
> Ez a funkció előzetes első verziójában, és lehet tulajdonos toobackward-nem kompatibilis módosításokat. Jelenleg nincs elérhető egyes előfizetési ajánlatok (EA, CSP, nem támogatott AIO) és a Németországi Azure.

## <a name="azure-resource-usage-api-preview"></a>Az Azure erőforrás-használat API (előzetes verzió)
Használjon hello Azure [erőforrás-használati API](https://msdn.microsoft.com/library/azure/mt219003) tooget a becsült Azure fogyasztási adatokhoz. hello API tartalmazza:

* **Szerepköralapú hozzáférés-vezérlés az Azure** -hozzáférés konfigurálása hello házirendek [Azure-portálon](https://portal.azure.com) vagy [Azure PowerShell-parancsmagok](/powershell/azure/overview) mely felhasználók vagy alkalmazások ingyenesen juthatnak az toospecify toohello előfizetés használati adatok. Hívóknak kell használnia szabványos Azure Active Directory-jogkivonatokat a hitelesítéshez. Hello hívó tooeither hello számlázási olvasó, olvasó, tulajdonosi vagy közreműködői szerepkör tooget hozzáférés toohello használati adatok hozzáadása egy adott Azure-előfizetéséhez.
* **Óránkénti vagy a napi aggregáció** - hívóknak adhat meg, hogy azok szeretné-e az Azure használati adatokat óránkénti időszakok vagy napi időszakok. hello alapértelmezett érték a napi.
* **(Erőforrás-címkét tartalmaz) példány metaadatok** – például teljesen minősített erőforrás uri hello példányszintű részletek beszerzése (/subscriptions/ {előfizetés-azonosító} /..), erőforrás csoport adatai, és az erőforráscímkék hello. A metaadatok deterministically segít, és programokon keresztül oszt használati által hello címkék, például a kereszt-díjszabási használati eseteket.
* **Erőforrás-metaadatok** -erőforrás részletei például hello mérő nevét, a mérési kategória, a mérő az alkategória, a egység és a régió adjon hello hívó szeretné jobban megismerni mi felhasznált. Tooalign erőforrás metaadatok terminológia hello Azure-portál, Azure használati CSV, EA CSV, számlázási és egyéb nyilvánosan elérhető lép, akkor összefüggéseket adatok között lép toolet is dolgozunk.
* **Minden használati kínálnak típusok** – használati adatok érhető el az összes kínálnak, például a használatalapú fizetés, MSDN, pénzügyi kötelezettségvállalást, pénzügyi kreditet és EA.

## <a name="azure-resource-ratecard-api-preview"></a>Azure-erőforrás RateCard API (előzetes verzió)
Használjon hello [Azure Resource RateCard API](https://msdn.microsoft.com/library/azure/mt219005) tooget hello elérhető Azure-erőforrások és listája az egyes becsült díjszabási információkat. hello API tartalmazza:

* **Szerepköralapú hozzáférés-vezérlés az Azure** -a hozzáférési házirendek konfigurálása hello [Azure-portálon](https://portal.azure.com) vagy [Azure PowerShell-parancsmagok](/powershell/azure/overview) mely felhasználók vagy alkalmazások ingyenesen juthatnak az toospecify toohello RateCard adatokat. Hívóknak kell használnia szabványos Azure Active Directory-jogkivonatokat a hitelesítéshez. Hello hívó tooeither hello olvasó, a tulajdonos vagy közreműködő szerepkört tooget hozzáférés toohello használati adatok hozzáadása egy adott Azure-előfizetés.
* **Használatalapú fizetés, az MSDN, a pénzügyi kötelezettségvállalást a és a pénzügyi kreditet biztosít a (EA nem támogatott)** -Ez az API felület Azure ajánlat szintű arány információkat nyújt.  Ez az API felület védőfalkezelőbe hello hello ajánlat tooget erőforrás adatai és a díjszabás meg kell felelnie. Mert EA ajánlatok testreszabott beléptetési mértékek a rendszer jelenleg nem tudja tooprovide EA sebességét. 

## <a name="scenarios"></a>Forgatókönyvek
Az alábbiakban néhány lehetséges hello használati és hello RateCard API-k hello kombinációjával rendelkező végrehajtott hello forgatókönyv:

* **Azure töltött hello hónapban** -hello használati és RateCard API-k tooget jobb betekintést a felhő használatát hello együttes töltenek hello hónap során. Hello óránkénti elemezheti és használati és kell fizetni napi gyűjtők becslése.
* **Riasztások beállítása** – hello használata, illetve hello RateCard API-k tooget felhő használat és díjak becsült, és erőforrás-alapú vagy pénzügyi-alapú értesítések beállítása.
* **Számlázási előrejelzése** – Get a becsült felhasználás és a felhő kiadásokat, és gépi tanulási algoritmusok toopredict milyen hello számlázási számlázási ciklus hello hello végén lehet alkalmazni.
* **Elemzés előtti fogyasztás** – hello RateCard API toopredict mekkora a számlázási okozna a várható használati helyezi át a munkaterheléseket tooAzure használja. Ha meglévő alkalmazások más felhők vagy a privát felhők, is hozzárendelheti a felhasználást a hello Azure díjszabás tooget Azure jobb becslése televíziózással töltenek. A becsült ad meg hello képességét toopivot ajánlat, és hasonlítsa össze és közötti hello különböző típusú túl használatalapú fizetés, ezzel szemben, például pénzügyi kötelezettségvállalást a és a pénzügyi kreditet. hello API is lehetővé teszi az hello képességét toosee költség régiónként különbségek attól függnek, és lehetővé teszi egy központi telepítési döntések lehetőségelemzések költség elemzés toohelp toodo.
* **Elemzési** -
  
  * Azt is meghatározhatja, hogy-e további költséghatékony toorun munkaterhelések egy másik régióban, vagy egy másik hello Azure-erőforrás-konfigurációban. Hello használata az Azure-régió alapján Azure-erőforrás költségek eltérőek lehetnek.
  * Azt is meghatározhatja, hogy ha egy másik Azure-ajánlat típusa a nagyobb mértékben nyújt egy Azure-erőforrás.
  
## <a name="partner-solutions"></a>Partneri megoldások
[A Microsoft Azure használati és RateCard API-k engedélyezése Cloudyn tooProvide az ügyfelek ITFM](billing-usage-rate-card-partner-solution-cloudyn.md) hello integrációs élményt Azure számlázási API partner által kínált ismerteti [Cloudyn](https://www.cloudyn.com/microsoft-azure/). Ez a cikk tapasztalataikat beszél, és tartalmazza a videó bemutatja, hogyan használhatja a Cloudyn és hello Azure számlázási API-k tooget információkat kaphat a Azure fogyasztási adatokhoz.

[Felhő Cruiser és a Microsoft Azure számlázási API-integráció](billing-usage-rate-card-partner-solution-cloudcruiser.md) ismerteti, hogyan [felhő Cruiser Express Azure Pack](http://www.cloudcruiser.com/partners/microsoft/) közvetlenül portálról hello Windows Azure Pack (WAP) működik. Egyetlen felhasználói felületről zökkenőmentesen mindkét hello működési és pénzügyi aspektusainak hello Microsoft Azure magán- vagy kihelyezett nyilvános felhőbe is kezelheti.   

## <a name="next-steps"></a>Következő lépések
* Tekintse meg a hello mintakódjainak megtekintése a Githubon:
  * [Számla API kódminta](https://go.microsoft.com/fwlink/?linkid=845124)

  * [Használati API kódminta](https://github.com/Azure-Samples/billing-dotnet-usage-api)

  * [RateCard API kódminta](https://github.com/Azure-Samples/billing-dotnet-ratecard-api)

* toolearn hello Azure Resource Manager kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md). 

* Felhő ismeretét kap szükséges toohelp töltött hello eszközcsomagot jelent a további információkért lásd: hello Gartner cikk [piaci útmutató informatikai pénzügyi Management (ITFM) eszközök](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

