---
title: "a az OMS-kezelési megoldás aaaBuild |} Microsoft Docs"
description: "Megoldások bővíthetők hello Operations Management Suite (OMS), adja meg a csomagolt felügyeleti lehetőségeket, hogy az ügyfelek tootheir OMS-munkaterület adhat hozzá.  Ez a cikk ismerteti, hogyan hozhat létre felügyeleti megoldások toobe részleteinek a saját környezetben használt, illetve mikor elérhető tooyour ügyfelek."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dea4c0d9e608d9fe4aa41088705958c9fe999372
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-and-build-a-management-solution-in-operations-management-suite-oms-preview"></a>Tervezési és a megoldás létrehozása kezelési Operations Management Suite (OMS) (előzetes verzió)
> [!NOTE]
> Ez az előzetes dokumentum megoldások létrehozásához az OMS Szolgáltatáshoz, amely jelenleg előzetes verziójúak. Az alábbiakban semmilyen sémát tulajdonos toochange.

[Megoldások](operations-management-suite-solutions.md) bővíthetők hello Operations Management Suite (OMS), adja meg a csomagolt felügyeleti lehetőségeket, hogy az ügyfelek tootheir OMS-munkaterület adhat hozzá.  Ez a cikk mutatja be a folyamat alapvetően toodesign, és a leggyakrabban használt követelményeknek megfelelő megoldás kiépítését.  Ha új toobuilding megoldások akkor használja ezt a folyamatot a kiindulási pontként, és majd bonyolultabb megoldások, a követelmények fejlődnek hello fogalmak.

## <a name="what-is-a-management-solution"></a>Mi az a felügyeleti megoldás?

Megoldások OMS és Azure-erőforrások, amelyek együttműködése tooachieve egy adott figyelési forgatókönyv tartalmazhat.  Akkor használják, mint [erőforrás-kezelés sablonok](../azure-resource-manager/resource-manager-template-walkthrough.md) hogyan részleteit tartalmazó tooinstall és a benne lévő erőforrások konfigurálása hello megoldás telepítésekor.

Alapszintű stratégia hello t toostart felügyeleti megoldásként épület hello egyes összetevők az Azure környezetben.  Miután hello funkció megfelelően működik, majd elindíthatja csomagolására őket egy [felügyeleti megoldást fájl](operations-management-suite-solutions-solution-file.md). 


## <a name="design-your-solution"></a>A megoldás tervezése
hello leggyakoribb minta felügyeleti megoldás hello a következő ábrán látható.  Ebben a mintában a hello különböző összetevőket az alábbi hello ismerteti.

![OMS megoldási áttekintés](media/operations-management-suite-solutions/solution-overview.png)


### <a name="data-sources"></a>Adatforrások
a megoldás tervezésének első lépése hello hello adatokat, amelyekre szüksége van a Naplóelemzési adattárból hello meghatározása.  Ezek az adatok is lehet összegyűjteni a [adatforrás](../log-analytics/log-analytics-data-sources.md) vagy [egy másik megoldás](operations-management-suite-solutions.md), vagy a megoldás tooprovide hello folyamat toocollect kell azt.

Adatforrások gyűjtendő hello Naplóelemzési tárházban leírtak szerint számos módon vannak [Naplóelemzési adatforrások](../log-analytics/log-analytics-data-sources.md).  Hello Windows eseménynaplóban az eseményeket is tartalmazza, vagy a program által generált Syslog továbbá tooperformance számlálók a Windows és Linux-ügyfelek.  Az Azure-erőforrások Azure figyelő által gyűjtött adatokat is gyűjthet.  

Ha van szüksége, amely nem érhető el valamelyik hello elérhető adatforrását keresztül adatokat, akkor használhatja a hello [HTTP adatait gyűjtője API](../log-analytics/log-analytics-data-collector-api.md) toowrite toohello Naplóelemzési adattárház bármely ügyfél, amely meghívhatja a REST lehetővé teszi API-T.  hello leggyakoribb azt jelenti, hogy az egyéni adatgyűjtés megoldásra toocreate egy [az Azure Automationben runbook](../automation/automation-runbook-types.md) Azure-bA vagy külső erőforrások szükséges hello adatait gyűjti, és használja hello adatait gyűjtője API toowrite toohello tárházba.  

### <a name="log-searches"></a>Napló-keresések
[A keresések jelentkezzen](../log-analytics/log-analytics-log-searches.md) használt tooextract és elemezhetik a hello Log Analytics-tárházban.  Ezek nézetek és a riasztások hozzáadása tooallowing hello felhasználói tooperform alkalmi adatok elemzése során hello tárházban által használt.  

Meg kell határozni, hogy akkor is, ha azok nem használt nézeteket és riasztásokat is hasznos toohello felhasználói lekérdezéseket.  Ezek lesz elérhető toothem mentett hello portálon, és is hozzáadhat azokat egy [lista a lekérdezések a képi megjelenítés része](../log-analytics/log-analytics-view-designer-parts.md#list-of-queries-part) az egyéni nézetben.

### <a name="alerts"></a>Riasztások
[Log Analytics riasztások](../log-analytics/log-analytics-alerts.md) keresztül problémák azonosításához [keresések jelentkezzen](#log-searches) hello tárházban hello adatok alapján.  Vagy hello felhasználó értesítése, vagy automatikusan futtasson egy műveletet válaszként. Azonosítsa az alkalmazás különböző riasztási feltételeket kell, és megfelelő riasztási szabályok belefoglalása a megoldásfájl.

Ha hello problémát esetleg a egy automatikus folyamat javítani kell, majd lesz általában létrehozta a forgatókönyvet az Azure Automation tooperform a szervizelési.  Az Azure szolgáltatások felügyelhetők a [parancsmagok](/powershell/azure/overview) mely hello runbook volna kihasználja a tooperform ilyen funkciót.

Ha a megoldás a válasz tooan riasztás külső funkciókat igényel, akkor is használhatja a [webhook válasz](../log-analytics/log-analytics-alerts-actions.md).  Ez lehetővé teszi toocall küldését hello-riasztás alapján külső webszolgáltatásokból.

### <a name="views"></a>Nézetek
Log Analytics-nézetek adattárból hello Naplóelemzési használt toovisualize adatok.  Egyes megoldások általában fogja tartalmazni az egyetlen nézetben egy [csempe](../log-analytics/log-analytics-view-designer-tiles.md) , amely hello felhasználó fő irányítópult jelenik meg.  hello nézet tartalmazhat tetszőleges számú [képi megjelenítés részek](../log-analytics/log-analytics-view-designer-parts.md) tooprovide különböző képi hello összegyűjtött adatok toohello felhasználó.

Ön [adatforrásnézet-tervezőből hello segítségével egyéni nézeteket hozhat létre](../log-analytics/log-analytics-view-designer.md) , amelyek később exportálhatja, hogy a megoldás fájlban.  


## <a name="create-solution-file"></a>Megoldás fájl létrehozása
Miután konfigurálását és tesztelt hello összetevők, hogy a megoldás részét képező, [hozzon létre egy megoldást fájlt](operations-management-suite-solutions-solution-file.md).  A megoldás-összetevő hello megvalósítandó egy [Resource Manager-sablon](../azure-resource-manager/resource-group-authoring-templates.md) , amely tartalmazza egy [megoldás erőforrás](operations-management-suite-solutions-solution-file.md#solution-resource) kapcsolatok toohello az egyéb erőforrások hello fájl.  


## <a name="test-your-solution"></a>A megoldás tesztelése
A megoldás fejleszt, amíg tooinstall kell, és tesztelje a munkaterületen.  Ehhez bármely hello rendelkezésre álló metódusok használatával túl[tesztelése és telepítése a Resource Manager-sablonok](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="publish-your-solution"></a>A megoldás közzététele
Miután befejeződött, és tesztelése a megoldás, hogy azt vagy a következő források hello keresztül elérhető toocustomers.

- **Azure gyors üzembe helyezési sablonokat**.  [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/) hello közösségi Githubon keresztül által közzétett erőforrás-kezelő sablonok készlete.  Elérhetővé teheti a megoldás által a következő információ hello [hozzájárulás útmutató](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE).
- **Az Azure piactér**.  Hello [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/) toodistribute lehetővé teszi, és a megoldás tooother fejlesztők, ISV-k, értékesítés és informatikai szakemberek számára.  Áttekintheti, hogyan toopublish a megoldás tooAzure piactér: [hogyan toopublish és kezelése az Azure piactér hello ajánlatot](../marketplace-publishing/marketplace-publishing-getting-started.md).



## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[hozzon létre egy megoldást fájlt](operations-management-suite-solutions-solution-file.md) -kezelési megoldást.
* Ismerje meg, hello részleteit [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).
* Keresési [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates) példákért különböző Resource Manager-sablonok.