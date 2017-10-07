---
title: "Azure Active Directory-naplók napló integrációja aaaAzure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall hello Azure napló integrációs szolgáltatás, és integrálhatja a naplók az Azure felügyeleti naplók"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/08/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 3ee8fa3b8b5e9bd33202e57ed5327cd8d3127f00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-active-directory-audit-logs"></a>Integrálása az Azure Active Directory-naplók

Az Azure Active Directory (Azure AD) naplózási események segítségével azonosíthatja a privilegizált műveletekhez, hogy megtörtént az Azure Active Directoryban. Megtekintheti a hello típusú események vizsgálatával nyomon követett [Azure Active Directory auditnaplójának jelentési eseményei](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).

> [!NOTE]
> A cikkben ismertetett hello-megkezdése előtt át kell néznie hello [Ismerkedés](security-azure-log-integration-get-started.md) és annak lépésekkel hello van.

## <a name="steps-toointegrate-azure-active-directory-audit-logs"></a>A naplók lépéseket toointegrate Azure Active Directoryban

1. Nyissa meg a hello parancssort, és futtassa ezt a parancsot:

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. Futtassa ezt a parancsot: 
 
   ``azlog createazureid``

   Ez a parancs kéri az Azure bejelentkezési. hello parancs majd hoz létre egy Azure Active Directory szolgáltatás egyszerű hello Azure AD bérlők üzemeltető hello Azure-előfizetéseket, mely hello bejelentkezett felhasználó, a rendszergazda, a társadminisztrátorának vagy a tulajdonos. hello parancs sikertelen lesz, ha hello bejelentkezett felhasználó csak a Vendég felhasználó hello Azure AD-bérlő. Hitelesítési tooAzure az Azure AD segítségével történik. Hello arra, hogy hozzáférési tooread az Azure-előfizetések az Azure AD identity egy egyszerű Azure napló integrációs szolgáltatás létrehozásakor létrejön.

3. Futtassa a következő parancs tooprovide hello a bérlő azonosítójával. Hello Bérlői rendszergazda szerepkör toorun hello parancs toobe tagja van szüksége.

   ``Azlog.exe authorizedirectoryreader tenantId``

   Példa:

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. Ellenőrizze, hogy a következő mappák tooconfirm, amely Azure Active Directory JSON naplófájlok hello hello jönnek létre őket:

   * **C:\Users\azlog\AzureActiveDirectoryJson**
   * **C:\Users\azlog\AzureActiveDirectoryJsonLD**

hello következő videó bemutatja a cikkben szereplő hello lépéseket:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> A JSON-fájlok hello hello információk üzembe a biztonsági információk és eseménykezelő rendszer (SIEM) kapcsolatos tudnivalókat forduljon a SIEM gyártójához.

Közösségi támogatás érhető el hello [Azure napló integrációs MSDN fórumon](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Ezen a fórumon lehetővé teszi, hogy munkatársai hello Azure napló integráció közösségi toosupport egymással kérdéseket, válaszokat, tippeket és trükkök. Ezenkívül hello Azure napló integrációs csoport ezen a fórumon figyeli, és amikor azt is segít.

Is megnyithatja a [támogatási kérelem](../azure-supportability/how-to-create-azure-support-request.md). Válassza ki **napló integrációs** hello szolgáltatást, amelynek támogatási kérelmet.

## <a name="next-steps"></a>Következő lépések
toolearn Azure napló-nal kapcsolatos további információkért lásd:

* [A Microsoft Azure napló integráció az Azure-naplók](https://www.microsoft.com/download/details.aspx?id=53324): A letöltőközpontból weblap, amely részleteit, rendszerkövetelmények és telepítési utasításokat az Azure napló integráció.
* [Bevezetés tooAzure napló integrációs](security-azure-log-integration-overview.md): Ez a cikk bemutatja a napló integrációs tooAzure, annak főbb funkcióit és annak működéséről.
* [Partner konfigurációs lépések](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Ebben a blogbejegyzésben bemutatja, hogyan tooconfigure Azure napló integrációs toowork a partnermegoldások Splunk, HP ArcSight, és az IBM QRadar.
* [Az Azure napló integrációs gyakran ismételt kérdések](security-azure-log-integration-faq.md): Ez a cikk Azure napló integrációs kapcsolatos kérdésekre ad választ.
* [A Security Center riasztásait integrálása Azure napló integrációs](../security-center/security-center-integrating-alerts-with-log-integration.md): Ez a cikk bemutatja, hogyan toosync Security Center riasztást küld, és a virtuális gép biztonsági események Azure Diagnostics és az Azure naplókat, és a naplóelemzési által gyűjtött vagy SIEM-megoldás.
* [Az Azure Diagnostics és az Azure új szolgáltatásai naplók](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): Ebben a blogbejegyzésben bemutatja tooAzure naplókat, és egyéb szolgáltatásokat, amelyek segítenek betekintést nyerhet az Azure-erőforrások hello működésére.
