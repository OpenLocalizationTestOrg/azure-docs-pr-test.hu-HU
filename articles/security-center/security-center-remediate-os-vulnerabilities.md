---
title: "az Azure Security Center aaaRemediate az operációs rendszer biztonsági réseivel |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** szervizelje az operációs rendszer biztonsági rések **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 704103f7fb15835943d74b665d2bd56cb5e0a36d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Az Azure Security Center az operációs rendszer biztonsági réseivel kapcsolat javítása
Az Azure Security Center elemzi naponta a virtuális gép (VM) operációs rendszereket a konfigurációk, hogy a virtuális gép hello sebezhetőbbé tooattack és javasolja konfiguráció módosításait tooaddress biztonsági rések. A Security Center azt javasolja, hogy a biztonsági rések oldható fel, ha a virtuális gép operációs rendszer konfigurációja nem felel meg a konfigurációs szabályok ajánlott hello.

> [!NOTE]
> Által figyelt hello konfigurációkkal kapcsolatban lásd: hello [ajánlott konfiguráció szabályok listája](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.  Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

1. A hello **javaslatok** panelen válassza **szervizelje az operációs rendszer biztonsági rések**.
   ![Operációs rendszerek sebezhetőségeinek javítása][1]

    Hello **szervizelje az operációs rendszer biztonsági rések** panel megnyílik, és a virtuális gépek operációs rendszer azon konfigurációit, amelyek nem felelnek meg a konfigurációs szabályok ajánlott hello sorolja fel.  Az egyes virtuális gépek hello panel azonosítja:

   * **Nem sikerült szabályok** – sikertelen szabályokat, amelyek a virtuális gép operációs rendszer konfigurációja hello hello száma.
   * **LEGUTÓBBI ellenőrzés időpontja** – hello dátum és idő, hogy a Security Center legutóbb ellenőrizte hello virtuális gép operációs rendszer konfigurációja.
   * **ÁLLAPOT** – hello hello biztonsági rés aktuális állapota:

     * Nyissa meg: hello biztonsági rés még nem foglalkoztak még
     * Folyamatban: Hello biztonsági rés jelenleg vonatkozik, nincs szükség felhasználói műveletek is
     * Megoldott: hello biztonsági rés már címezték (ha hello problémát sikerült megoldani, hello bejegyzés szürkén jelenik meg)
   * **SÚLYOSSÁGI** – összes biztonsági rés állítottak tooa súlyossága alacsony, ami azt jelenti, a biztonsági rés kell tömni, de nem igényel azonnali beavatkozást.

2. Jelöljön ki egy virtuális Gépet. Egy panel, amely a virtuális gép megnyílik, és azokat, amelyek nem tudták hello szabályokat jeleníti meg.
   ![Konfigurációs szabályok, amelyek nem tudták][2]

3. Jelöljön ki egy szabályt. Ebben a példában lehetővé teszi, hogy válasszon **jelszónak meg kell felelnie a bonyolultsági feltételeknek**. Megnyílik egy panel leíró hello sikertelen volt a szabály és hello hatása. Hello adatokat, és fontolja meg, hogyan alkalmazza a rendszer operációsrendszer-konfigurációval.
  ![Hello leírását meghiúsult (szabály)][3]

  A Security Center a konfigurációs szabályok Common Configuration Enumeration (CCE) tooassign egyedi azonosítót használ. a panel hello a következő információkat biztosítja:

  - NÉV – Szabály neve
  - SÚLYOSSÁG – CCE súlyosságérték kritikus, fontos vagy figyelmeztető
  - CCIED--CCE egyedi azonosítója hello szabály
  - Leírás – A szabály leírása
  - A biztonsági RÉS--Magyarázat biztonsági rés vagy kockázat, ha a szabály nem alkalmazható
  - Gyakorolt hatás – Szabály alkalmazása Üzletmenetre gyakorolt hatás
  - VÁRT érték--Értéket várt, amikor a Security Center elemzi a virtuális gép operációs rendszere konfigurációs hello szabállyal szemben
  - --A szabály szabály művelet során a virtuális gép operációs rendszere konfigurációs hello szabállyal szemben elemzése a Security Center által használt
  - TÉNYLEGES érték--Értéket adott vissza az elemzést, a virtuális gép operációs rendszere konfigurációs hello szabállyal szemben
  - KIÉRTÉKELÉSÉNEK eredménye –-elemzés eredménye: átadni, sikertelen

## <a name="see-also"></a>Lásd még:
Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "szervizelése az operációs rendszer biztonsági rések." Tekintse át a konfigurációs szabályok készletét hello [Itt](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). A Security Center a konfigurációs szabályok CCE (közös konfigurációs enumerálási) tooassign egyedi azonosítót használ. A Microsoft hello [CCE](https://nvd.nist.gov/cce/index.cfm) webhelyen olvashat.

További információ a Security Center toolearn lásd: a következő erőforrások hello:

* [Az Azure Security Center által támogatott platformok](security-center-os-coverage.md) -támogatott Windows és Linux virtuális gépek listáját tartalmazza.
* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) -megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) -megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) -megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) -megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) -megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) -gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
