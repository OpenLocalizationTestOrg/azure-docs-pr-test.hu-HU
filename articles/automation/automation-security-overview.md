---
title: az Azure Automationben aaaIntro tooauthentication |} Microsoft Docs
description: "Ez a cikk áttekintést Automation biztonsági és hello különböző hitelesítési módszer áll rendelkezésre az Automation-fiók az Azure Automationben."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
keywords: "automation-biztonság, automation biztonságossá tétele; automation-hitelesítés"
ms.assetid: 4a6bc2f5-c5a2-4dfb-b10d-7950d750dee8
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2017
ms.author: magoedte
ROBOTS: NOINDEX
ms.openlocfilehash: 4b4409b5be010c16f7bf00a9a0f617e3617d4663
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooauthentication-in-azure-automation"></a>Az Azure Automationben bemutatása tooauthentication  
Azure Automation szolgáltatással tooautomate feladatok erőforrásokon Azure, a helyszínen, és más például az Amazon Web Services (AWS) szolgáltatók.  Ahhoz, hogy egy runbook tooperform a szükséges műveleteket kell rendelkeznie engedélyek toosecurely hello erőforrások eléréséhez szükséges hello előfizetésen belül hello minimális jogosultságokkal.

Ez a cikk kiterjed az Azure Automation által támogatott különböző hitelesítési forgatókönyvek hello és tooget indításának alapján hello környezet vagy a környezetben, megjelenítése kell toomanage.  

## <a name="automation-account-overview"></a>Az Automation-fiókok áttekintése
Amikor elkezdi Azure Automation hello az első alkalommal, létre kell hoznia legalább egy Automation-fiók. Automation-fiókok lehetővé teszik az Automation-erőforrások (runbookok, eszközök, konfigurációk) a hello más Automation-fiók található erőforrások tooisolate. Automation-fiókok tooseparate erőforrások külön logikai környezetekben is használhatja. Használhat például egy fiókot fejlesztéshez, egy másikat az üzemi használatra, egy harmadikat pedig a helyszíni környezethez.  Az Azure Automation-fiók különbözik a Microsoft-fiókjától vagy az Azure-előfizetésében létrehozott fiókoktól.

hello Automation erőforrások minden Automation-fiókhoz társított egyetlen Azure-régió, de az Automation-fiók kezelhető az összes hello erőforrást az előfizetésében. hello legfőbb oka toocreate Automation-fiók különböző régiókban lenne, ha telepítette a házirendekben, amelyek az adatok és erőforrások toobe adott elkülönített tooa-terület szükséges.

> [!NOTE]
> Automation-fiókok és hello erőforrásokat tartalmaznak, amelyek hello Azure-portálon létrehozott, a klasszikus Azure portálon hello nem érhető el. Ha azt szeretné toomanage ezeket a fiókokat, vagy a Windows PowerShell-lel erőforrásaikat, hello Azure Resource Manager modulok kell használnia.
>

Azure Resource Manager és az Azure-parancsmagokkal hello használata az Azure Automationben erőforrásokon végrehajtott hello feladatokat kell hitelesítenie tooAzure Azure Active Directory szervezeti azonosító hitelesítőadat-alapú hitelesítést használ.  Tanúsítványalapú hitelesítés hello eredeti hitelesítési módszer az Azure szolgáltatásfelügyeleti módban volt, de toosetup bonyolult volt.  Hitelesítéséhez az Azure AD-felhasználó tooAzure volt a 2014 toonot bevezetett vissza csak egyszerűsítése hello folyamat tooconfigure egy hitelesítési fiók, hanem a támogatási hello képességét toonon interaktív hitelesítéshez tooAzure működő egyetlen felhasználói fiókkal az Azure Resource Manager és a hagyományos erőforrások.   

Jelenleg Ha hello Azure-portálon létrehoz egy új Automation-fiók, automatikusan létrehoz:

* Futtató fiók, amely létrehoz egy új szolgáltatás egyszerű az Azure Active Directoryban, a tanúsítványt, és hozzárendeli a hello közreműködői szerepkör alapú hozzáférés-vezérlés (RBAC), ez az használt toomanage Resource Managerhez tartozó erőforrások runbookok használata.
* Klasszikus Futtatás mint fiók felügyeleti tanúsítvány, amelyek használt toomanage Azure szolgáltatásfelügyelet vagy runbookok használatával hagyományos erőforrások feltöltésével.  

Szerepköralapú hozzáférés-vezérlés érhető el az Azure Resource Manager toogrant engedélyezett műveletek tooan Azure AD-felhasználói fiókot és a Futtatás mint fiókot, és a szolgáltatás egyszerű hitelesítést.  Kérjük, olvassa el [szerepköralapú hozzáférés-vezérlés az Azure Automation-cikkben](automation-role-based-access-control.md) további tájékoztatást talál toohelp fejlesztése a kezelésének Automation engedélyek modelljét.  

Az Adatközpont vagy AWS szolgáltatások számítástechnikai elleni a hibrid forgatókönyv-feldolgozók futó Runbookok nem használható hello azonos runbookok tooAzure erőforrások hitelesítéséhez használt módszert.  Ennek az oka, hogy ezeket az erőforrásokat Azure-on kívüli futnak, és ezért esetén saját automatizálási tooauthenticate tooresources, amely akkor érik el a helyileg definiált biztonsági hitelesítő adatokat.  

## <a name="authentication-methods"></a>Hitelesítési módszerek
hello következő táblázat összefoglalja hello különböző hitelesítési módszerek minden Azure Automation és hello cikk leíró által támogatott környezet hogyan toosetup hitelesítési a runbookok.

| Módszer | Környezet | Cikk |
| --- | --- | --- |
| Azure AD felhasználói fiók |Azure Resource Manager és Azure szolgáltatásfelügyelet |[Runbookok hitelesítése Azure AD-felhasználói fiókkal](automation-create-aduser-account.md) |
| Azure-futtatófiók |Azure Resource Manager |[Runbookok hitelesítése Azure-beli futtató fiókkal](automation-sec-configure-azure-runas-account.md) |
| Klasszikus Azure-futtatófiók |Azure szolgáltatásfelügyelet |[Runbookok hitelesítése Azure-beli futtató fiókkal](automation-sec-configure-azure-runas-account.md) |
| Windows-hitelesítés |Helyszíni adatközpont |[Runbookok hitelesítése hibrid runbook-feldolgozókhoz](automation-hybrid-runbook-worker.md) |
| AWS hitelesítő adatok |Amazon webszolgáltatások |[Runbookok hitelesítése az Amazon webszolgáltatásokkal (AWS)](automation-config-aws-account.md) |
