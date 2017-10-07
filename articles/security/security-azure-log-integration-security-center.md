---
title: "a security Center napló integrációs aaaAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooget Azure Security center riasztások napló integrációs használata"
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
ms.date: 03/22/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: bcc208d071ec03738215f2aee3b71c7b10927904
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-your-security-center-alerts-in-azure-log-integration"></a>Hogyan tooget a Security Center riasztást küld az Azure naplóelemzés integráció
Ez a cikk hello lépéseket szükséges tooenable hello Azure napló integrációs szolgáltatás toopull az Azure Security Center által generált biztonsági riasztások információkat tartalmazza. Sikeresen végrehajtotta a hello hello lépéseket [Ismerkedés](security-azure-log-integration-get-started.md) cikket ebben a cikkben hello lépések végrehajtása előtt.

## <a name="detailed-steps"></a>Részletes lépések
hello lépések hoz létre hello szükség Azure Active Directory szolgáltatás egyszerű és hozzárendelése hello szolgáltatás elv olvasási engedéllyel toohello előfizetés:
1. Nyissa meg a hello parancssort, és keresse meg a túl**c:\Program Files\Microsoft Azure napló integráció**
2. Hello parancs futtatása``azlog createazureid``

    Ez a parancs kéri az Azure bejelentkezési. hello parancs létrehoz egy [Azure Active Directory szolgáltatás egyszerű](../active-directory/develop/active-directory-application-objects.md) hello az Azure AD-bérlő üzemeltető hello Azure-előfizetések a bejelentkezett felhasználó mely hello esetében a rendszergazda, a Társadminisztrátorának vagy a tulajdonos. hello parancs sikertelen lesz, ha a bejelentkezett felhasználó hello csak a Vendég felhasználó hello Azure AD-bérlő. Hitelesítési tooAzure keresztül Azure Active Directory (AD) történik. Hello kapott hozzáférési tooread az Azure-előfizetések az Azure AD identity Azlog integrációs egyszerű szolgáltatás létrehozása hoz létre.

2. Ezután a hello előfizetés toohello egyszerű szolgáltatásnév a 2-olvasó hozzáférési jogosultságot rendel parancsot kell futtatnia. Ha nem ad meg egy előfizetés-azonosító, hello parancs tooassign hello szolgáltatás egyszerű olvasó szerepkört tooall előfizetések toowhich bármely hozzáférhetnek megpróbál. </br></br>
``azlog authorize <SubscriptionID>`` </br> például </br>
``azlog authorize 0ee55555-0bc4-4a32-a4e8-c29980000000``

    >[!NOTE]
    Ha futtatja a hello figyelmeztetések jelenhetnek engedélyezik a parancs azonnal hello createazureid parancs után. Néhány hello Azure AD-fiók létrehozásakor, és ha hello fiók használható közötti késés van. Ha vár 60 másodperc hello createazureid parancs toorun hello futtatása után engedélyezze a parancsot, majd figyelmeztetések nem szabadna megjelennie.

4. Ellenőrizze, hogy a következő mappák tooconfirm, amely a naplófájlok JSON hello hello vannak:
 * **c:\Users\azlog\AzureResourceManagerJson**
 * **c:\Users\azlog\AzureResourceManagerJsonLD** </br></br>
5. Ellenőrizze a következő mappák tooconfirm, amely a Security Center riasztásait szerepel azokat hello:</br></br>
 * **c:\Users\azlog\AzureSecurityCenterJson**
 * **c:\Users\azlog\AzureSecurityCenterJsonLD** </br></br>

Ha probléma merül fel hello telepítés és konfigurálás során, nyisson meg egy [támogatási kérelem](/azure-supportability/how-to-create-azure-support-request.md), jelölje be **napló integrációs** hello szolgáltatást, amelynek támogatási kérelmet.

## <a name="next-steps"></a>Következő lépések
toolearn Azure napló-nal kapcsolatos további információkért tekintse meg a következő dokumentumok hello:

* [A Microsoft Azure napló integráció az Azure-naplók](https://www.microsoft.com/download/details.aspx?id=53324) – letöltőközpontból részletes rendszerkövetelményeket, és telepítése Azure naplóelemzés integrációs utasításokat.
* [Bevezetés tooAzure napló integrációs](security-azure-log-integration-overview.md) – Ez a dokumentum bemutatja a tooAzure napló integrációs, annak főbb funkcióit és annak működéséről.
* [Partner konfigurációs lépések](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – ebben a blogbejegyzésben bemutatja, hogyan tooconfigure Azure naplófájl-integráció toowork Splunk, HP ArcSight és az IBM QRadar partneri megoldások.
* [Gyakori kérdések (GYIK) integrációs Azure naplóelemzés](security-azure-log-integration-faq.md) -Ez gyakran ismételt kérdések Azure naplóelemzés integrációs kapcsolatos kérdésekre ad választ.
* [A Security Center integrálása az Azure-ral riasztások jelentkezzen integrációs](../security-center/security-center-integrating-alerts-with-log-integration.md) – Ez a dokumentum bemutatja, hogyan toosync Security Center riasztást küld, és a virtuális gép biztonsági eseményeket a naplóelemzési az Azure Diagnostics és az Azure-beli Auditnaplók által gyűjtött vagy SIEM-megoldás.
* [Az Azure diagnostics és az Azure-beli Auditnaplók újdonságai](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – ebben a blogbejegyzésben bemutatja tooAzure naplókat, és egyéb szolgáltatásokat, amelyek segítenek betekintést nyerhet az Azure-erőforrások hello működésére.
