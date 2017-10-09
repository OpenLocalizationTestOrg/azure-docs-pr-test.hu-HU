---
title: "aaaAlerts ellenőrzése az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum segít toovalidate hello biztonsági riasztások az Azure Security Centerben."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: yurid
ms.openlocfilehash: 030e9e74303758192eedaf517f1cb0d2e4a7852e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="alerts-validation-in-azure-security-center"></a>Riasztások érvényesítése az Azure Security Centerben
Ez a dokumentum segítségével megtudhatja, hogyan tooverify, ha a rendszer az Azure Security Center riasztásait megfelelően van konfigurálva.

## <a name="what-are-security-alerts"></a>Mik azok a biztonsági riasztások?
A Security Center automatikusan gyűjti, elemzi és integrálja az Azure-erőforrások, hello és hálózati összekapcsolt partneri megoldások, például a tűzfal és az endpoint protection megoldások, a toodetect és a riasztás akkor toothreats naplóadatait. Olvasási [az Azure Security Centerben riasztások kezelése és válaszol toosecurity](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) tájékozódhat a biztonsági riasztásokat, és olvasási [az Azure Security Center biztonsági riasztások ismertetése](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) további toolearn kapcsolatos riasztások különböző hello.

## <a name="alert-validation"></a>Riasztások érvényesítése
Miután a Security Center-ügynök telepítve van a számítógépen, lépésekkel hello alatt hello számítógépről toobe hello megtámadott erőforrások hello riasztás kívánt:

1. Másolja át egy végrehajtható (a példa calc.exe) toohello asztal, vagy más könyvtárához, a felhasználók kényelme érdekében.
2. A fájl átnevezése túl**ASC_AlertTest_662jfi039N.exe**.
3. Nyisson meg hello parancssort, és a fájl egy argumentummal (csak egy hamis argumentum nevét), például a végrehajtása: *ASC_AlertTest_662jfi039N.exe - PEL*
4. Várjon 5 too10 percet, és nyissa meg a Security Center riasztásait. Nincs látnia kell egy riasztási hasonló toofollowing egyikét:

    ![Riasztások érvényesítése](./media/security-center-alert-validation/security-center-alert-validation-fig1.png)

Ha ez a riasztás megtekintésével ellenőrizze, hogy argumentumok naplózását engedélyezni hello mező jelenik meg az igaz. Azt illusztrálja, hamis, ha szüksége tooenable naplózás parancssori argumentumokat. Engedélyezheti ezt a beállítást, a következő parancssori hello használata:

*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*


## <a name="see-also"></a>Lásd még:
Ez a cikk bevezetett toohello riasztások érvényesítési folyamata. Most, hogy ismeri az ellenőrzés, próbálja meg a következő cikkek hello:

* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Megtudhatja, hogyan toomanage riasztásokat, és válaszoljon toosecurity incidensek a Security Center.
* [Biztonsági állapotfigyelés az Azure Security Centerben](security-center-monitoring.md). Ismerje meg, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Center biztonsági riasztásainak megismerése](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). További információk a biztonsági riasztások különböző típusú hello.
* [Azure Security Center – Hibaelhárítási útmutató](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Ismerje meg, hogyan tootroubleshoot közös állít ki a biztonsági központban. 
* [Azure Security Center – gyakori kérdések](security-center-faq.md) Hello szolgáltatással kapcsolatos gyakran ismételt kérdések található.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.

