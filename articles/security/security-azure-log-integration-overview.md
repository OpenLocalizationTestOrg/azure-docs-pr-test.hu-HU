---
title: "az Azure-erőforrások azokat a SIEM-rendszerekről aaaIntegrate naplók |} Microsoft Docs"
description: "Tudnivalók az Azure naplóelemzés integrációs, annak főbb funkcióit és annak működéséről."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/10/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 4a59ce625702e5266a7c8eb020473cfeaf6b1964
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toomicrosoft-azure-log-integration"></a>Bevezetés tooMicrosoft Azure naplóelemzés integráció
Tudnivalók az Azure naplóelemzés integrációs, annak főbb funkcióit és annak működéséről.

## <a name="overview"></a>Áttekintés

Azure naplóelemzés integrációra egy ingyenes megoldás, amely lehetővé teszi a tooyour helyszíni biztonsági adatai és az esemény felügyeleti SIEM-rendszerek toointegrate nyers napló, az Azure-erőforrások.

Azure naplóelemzés integrációs Windows-eseményeket gyűjti össze a Windows Eseménynapló-naplók [Azure tevékenységi naplóit](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [Azure Security Center riasztásait](../security-center/security-center-intro.md), és [Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) a Azure-erőforrások. Ez az integráció a SIEM-megoldás adjon meg egy új, egységesített irányítópult minden eszköz segítségével, a helyszíni vagy hello felhőben, így meg tudja-e összesíteni, összefüggéseket, elemzése és biztonsági eseményeket a riasztás.

>[!NOTE]
Jelenleg csak a támogatott hello egy felhő hozzá Azure kereskedelmi és Azure Government. Nem támogatja a többi felhőből.

![Azure-naplók integrációja][1]

## <a name="what-logs-can-i-integrate"></a>Milyen naplókat is integrálhatja?
Azure kiterjedt naplózás minden Azure Service eredményez. Ezek a naplók naplók három típusú mutatják be:

* **Ellenőrzés/management-naplók** hello betekintést nyújtanak [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) LÉTREHOZÁSI, frissítési és törlési műveletek. Az Azure tevékenységi naplóit például a következő napló.
* **Adatok sík naplók** adjon meg egy Azure-erőforrás hello használata részeként kiváltott hello események láthatósága. Az ilyen típusú napló példa: hello Windows Eseménynapló használatával **rendszer**, **biztonsági**, és **alkalmazás** csatornák Windows virtuális gépként. Egy másik példa a diagnosztikai naplózás Azure figyelő konfigurálva
* **A feldolgozott események** elemzett esemény és az Ön nevében feldolgozott riasztási adatokat. Például a következő esemény az Azure Security Center riasztásait, ahol az Azure Security Center feldolgozása és elemzése az előfizetés tooprovide riasztások vonatkozó tooyour aktuális biztonsági állapotát.

Azure napló-integráció ArcSight, QRadar és Splunk támogatja. Minden esetben segítségével ellenőrizze a SIEM-szállító tooassess hogy rendelkeznek-e egy natív összekötőt. Néhány esetben nem kell Azure napló integrációs toouse Ha natív összekötők nem érhető el. További információt a támogatott típusok látogasson el a hello gyakran ismételt kérdések.

>[!NOTE]
Míg Azure napló integrációs szabad megoldást, nincsenek eredő hello napló fájl adatokat tároló Azure storage költségei.

Közösségi támogatás érhető el hello [Azure napló integrációs MSDN fórumon](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). hello fórum biztosít hello AzLog közösségi hello képességét toosupport egymással kérdéseket, válaszokat, tippeket és trükkök hogyan tooget hello legtöbb Azure napló integrációs kívül a. Ezenkívül hello Azure napló integrációs csoport ezen a fórumon figyeli, és amikor azt is segít.

Is megnyithatja a [támogatási kérelem](../azure-supportability/how-to-create-azure-support-request.md). toodo a, válassza ki **napló integrációs** hello szolgáltatást, amelynek támogatási kérelmet.

## <a name="next-steps"></a>Következő lépések
Ebben a dokumentumban bevezetett tooAzure napló integrációs volt. További információk az Azure naplófájl, integráció és támogatott, naplók hello típusú toolearn hello következő lásd:

* [A Microsoft Azure napló integrációs](https://www.microsoft.com/download/details.aspx?id=53324) – letöltőközpontból részletes rendszerkövetelményeket, és telepítése Azure naplóelemzés integrációs utasításokat.
* [Ismerkedés az Azure naplóelemzés integrációs](security-azure-log-integration-get-started.md) – Ez az oktatóanyag végigvezeti az Azure naplóelemzés integráció telepítése, és az Azure ÜVEGVATTA storage, Azure tevékenységi naplóit, naplók integrálása az Azure Security Center riasztások és az Azure Active Directory naplók.
* [Partner konfigurációs lépések](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – ebben a blogbejegyzésben bemutatja, hogyan tooconfigure Azure naplófájl-integráció toowork Splunk, HP ArcSight és az IBM QRadar partneri megoldások. Ebben a blogban hello partneri megoldások konfigurálásáról az aktuális pozíciót jelöli. Minden esetben tekintse meg az toopartner megoldás dokumentáció először.
* [A tevékenység, és az ASC érték riasztások adott syslog tooQRadar](https://blogs.msdn.microsoft.com/azuresecurity/2016/09/24/integrate-azure-logs-to-qradar/) – ebben a blogbejegyzésben lépéseit hello toosend tevékenységet és az Azure Security Center riasztások syslog tooQRadar keresztül
* [Gyakori kérdések (GYIK) integrációs Azure naplóelemzés](security-azure-log-integration-faq.md) -Ez gyakran ismételt kérdések Azure naplóelemzés integrációs kapcsolatos kérdésekre ad választ.
* [A Security Center integrálása az Azure-ral riasztások jelentkezzen integrációs](../security-center/security-center-integrating-alerts-with-log-integration.md) – Ez a dokumentum bemutatja, hogyan toosync az Azure Security Center riasztást küld, az Azure napló integráció.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
