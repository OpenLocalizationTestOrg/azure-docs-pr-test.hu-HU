---
title: "Biztonsági központ Platform áttelepítési aaaAzure |} Microsoft Docs"
description: "Ez a dokumentum ismerteti az egyes módosítások toohello módja az Azure Security Center adatok gyűjtése."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 80246b00-bdb8-4bbc-af54-06b7d12acf58
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: yurid
ms.openlocfilehash: 28cb8d85912a3f62941cf113da51070081b5eda2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-platform-migration"></a>Az Azure Security Center platform migrációja

Korai. június 2017 verziótól kezdve az Azure Security Center bevezeti a fontos változások toohello módon biztonsági adatok gyűjtése és tárolni.  Ezek a változások zárolásának feloldása új szolgáltatásokat, például a hello képességét tooeasily keresési biztonsági adatokat, és más Azure kezelési és -szolgáltatások figyelésének jobban igazodik.

> [!NOTE]
> hello platform áttelepítési kell nincs hatással az éles erőforrások, és semmilyen művelet nem szükséges a oldaláról.


## <a name="whats-happening-during-this-platform-migration"></a>Mi történik a platform migrációja során?

Korábban a Security Center használt hello Azure Figyelőügynök toocollect biztonsági adatokat a virtuális gépek. Ez magában foglalja a biztonsági konfigurációkat, amelyek használt tooidentify biztonsági rések, és a biztonsági események, amelyek használt toodetect fenyegetésekkel kapcsolatos információkat. Ezeket az adatokat az Azure Storage-fiókjában tároltuk.

Továbbítja, a Security Center használja a Microsoft Monitoring Agent – hello Ez az Operations Management Suite hello és Naplóelemzés szolgáltatás által használt ugyanannak az ügynöknek hello. Ez az ügynök gyűjtött adatokat tárolja vagy egy meglévő *Naplóelemzési* [munkaterület](../log-analytics/log-analytics-manage-access.md) társított Azure-előfizetése vagy egy új munkaterületek, figyelembe véve a fiók hello földrajzi hely, a virtuális gép hello .

## <a name="agent"></a>Ügynök

Hello áttűnés részeként hello a Microsoft Monitoring Agent (a [Windows](../log-analytics/log-analytics-windows-agents.md) vagy [Linux](../log-analytics/log-analytics-linux-agents.md)) telepítve van, ahol jelenleg folyik adatgyűjtés Azure virtuális gépeken.  Ha a virtuális gép már van telepítve, a Microsoft Monitoring Agent hello hello Security Center aktuális hello használja az ügynök telepítése.

Mindkét ügynökök egy adott időn belül (általában néhány nap múlva), a futtatandó párhuzamos tooensure adatok zökkenőmentes átmenet adatvesztés nélkül. Ez lehetővé teszi a Microsoft, amely új adatok adatcsatorna hello toovalidate működőképességét megszűnő hello aktuális folyamat használata előtt. Egyszer ellenőrzött hello Azure Figyelőügynök törlődni fog a virtuális gépek. Az Ön részéről nincs szükség beavatkozásra. Az összes ügyfél migrálásának befejeződéséről e-mailben fog értesítést kapni.
 
Nem ajánlott, hogy manuálisan távolítsa el hello Azure Figyelőügynök hello áttelepítéskor mint okozhat a biztonsági adatok hézagok meghatározása érdekében. Ha további segítségre van szüksége, forduljon a [Microsoft támogatási és ügyfélszolgálatához](https://support.microsoft.com/contactus/). 

a Microsoft figyelési ügynök a Windows hello használata TCP 443-as portot, olvassa el szükséges [Azure Security Center hibaelhárítási útmutató](security-center-troubleshooting-guide.md) további információt.


> [!NOTE] 
> Mivel a Microsoft Monitoring Agent hello felhasználhatja más Azure felügyeleti és -szolgáltatások figyelésének hello ügynök nem lesz eltávolítva automatikusan kikapcsolása esetén a Security Center adatgyűjtés. Azonban kézzel hello ügynököt eltávolíthatja szükség esetén.

## <a name="workspace"></a>Munkaterület

Microsoft Monitoring Agent (nevében biztonsági központ) vannak tárolva egy meglévő Log Analyticshez hello gyűjtött adatokat a fent ismertetettek szerint, munkaterületek társított Azure-előfizetése vagy egy új munkaterületek, figyelembe véve a fiók hello földrajzi hely meghatározásának a virtuális gép hello.

Hello Azure-portálhoz böngészhet a toosee a Naplóelemzési munkaterület, így azokat a Security Center által létrehozott listáját. Az új munkaterületekhez kapcsolódó erőforráscsoport fog létrejönni. Mindkettő ezt az elnevezési konvenciót követi:

- Munkaterület: *AlapértelmezettMunkaterület-[előfizetés-azonosító]-[geo]*
- Erőforráscsoport: *AlapértelmezettErőforráscsoport-[geo]* 
 
A Security Center által létrehozott munkaterületeken az adatok 30 napig őrződnek meg. Meglévő munkaterületek megőrzési hello munkaterület árképzési szint alapul.

> [!NOTE]
> A Security Center által korábban gyűjtött adatok a Storage-fiók(ok)ban maradnak. Hello áttelepítés befejezése után törölheti a Storage-fiókok.

### <a name="oms-security-solution"></a>OMS biztonsági megoldás 

Azok számára a meglévő ügyfelek számára, akiknél nincs telepítve az OMS biztonsági megoldás, a Microsoft telepíti azt a munkaterületükre, de csak az Azure virtuális gépeket célozva. Ne távolítsa el ezt a megoldást, mert nincs automatikus javítási lehetőség, ha ezt az OMS kezelési konzoljáról teszi.


## <a name="other-updates"></a>Egyéb frissítések

Hello platform áttelepítési együtt azt állapotmodelljeiig, néhány további kisebb frissítéseit:

- További operációsrendszer-verziók lesznek támogatva. Hello tartalmazó lista [Itt](security-center-faq.md#virtual-machines).
- a program hello lista az operációs rendszer biztonsági rések bontja ki. Hello tartalmazó lista [Itt](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- A [Díjszabás](https://azure.microsoft.com/pricing/details/security-center/) óránkénti arányosítású lesz (régebben napi volt), ami bizonyos ügyfeleknél költségmegtakarítást fog eredményezni.
- Adatgyűjtés lesz szükséges, és automatikusan engedélyezi az ügyfelek a hello Standard tarifacsomagot.
- Az Azure Security Center meg fogja kezdeni az olyan kártevőirtó megoldások észlelését, amelyek nem az Azure-bővítményeken keresztül lettek telepítve. Elsőként a Symantec Endpoint Protection és a Defender for Windows 2016 lesz elérhető.
- Megelőzési házirendekhez és értesítések csak is konfigurálható: hello *előfizetés* szint, de az árképzés továbbra is be lehet állítani hello *erőforráscsoport* szint

