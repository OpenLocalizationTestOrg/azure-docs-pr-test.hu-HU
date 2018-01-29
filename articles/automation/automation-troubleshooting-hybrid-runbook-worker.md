---
title: "Azure Automation szolgáltatásbeli hibrid forgatókönyv-feldolgozó hibaelhárítása |} Microsoft Docs"
description: "A jelenség okok és az Azure Automation a leggyakrabban használt hibrid forgatókönyv-feldolgozó problémák megoldási ismertetik."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 02c6606e-8924-4328-a196-45630c2255e9
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: magoedte
ms.openlocfilehash: 75f4ac1bc940a2b1d8e4ac6aeac8b80c642489da
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/14/2017
---
# <a name="troubleshooting-tips-for-hybrid-runbook-worker"></a>Hibaelhárítási tippek a hibrid forgatókönyv-feldolgozó

Ez a cikk ismerteti a hibaelhárítást hibákat tapasztalhat az Automation hibrid forgatókönyv-feldolgozók és azok megoldását lehetséges megoldások javasolja.

## <a name="a-runbook-job-terminates-with-a-status-of-suspended"></a>Egy runbook-feladat leállítása felfüggesztett állapotú

A runbook hamarosan után kísérel meg végrehajtani, akkor állítsa be háromszor fel van függesztve. Feltételek vonatkoznak, amelyek megszakíthatja a runbook sikeres befejezését, és a kapcsolódó hibaüzenet tartalmazza, amely jelzi, hogy miért további adatokat. Ez a cikk nyújt a hibrid forgatókönyv-feldolgozót a runbook végrehajtása hibákhoz kapcsolódó problémák hibaelhárítási lépéseket.

Ha az Azure nem problémával az ebben a cikkben, látogasson el az Azure-fórumok a [MSDN és a Stack Overflow](https://azure.microsoft.com/support/forums/). A probléma, beküldheti, ezek fórumokban, vagy [ @AzureSupport a Twitteren](https://twitter.com/AzureSupport). Emellett fájlt a az Azure támogatási kérelmet kiválasztásával **segítségre van szüksége** a a [az Azure támogatási](https://azure.microsoft.com/support/options/) hely.

### <a name="symptom"></a>Jelenség
A Runbook végrehajtása sikertelen, és a visszaadott hiba a következő, "a feladat művelete"Aktiválás"Feladatművelet nem futtatható, mert a folyamat váratlanul leállt. A feladat művelet végrehajtására történt kísérlet 3-szor."

Nincsenek a hiba lehetséges okai: 

1. A hibrid feldolgozó a proxy vagy az tűzfal mögött van
2. A hibrid feldolgozó futtató számítógép nem felel meg a minimális [hardverkövetelmények](automation-offering-get-started.md#hybrid-runbook-worker)  
3. A runbookok nem tudja hitelesíteni a helyi erőforrások

#### <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>1. ok: Az hibrid forgatókönyv-feldolgozó proxy és tűzfal mögött van.
A hibrid forgatókönyv-feldolgozó fut a számítógépen van egy tűzfal vagy a proxykiszolgáló mögött, és a kimenő hálózati hozzáférés nem engedélyezett, lehet, illetve megfelelően konfigurálva.

#### <a name="solution"></a>Megoldás
Ellenőrizze, hogy a számítógép *.azure-automation.net kimenő hozzáféréssel rendelkezzen a 443-as porton. 

#### <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>2. ok: A számítógépben kisebb, mint a minimális hardverkövetelmények
A hibrid forgatókönyv-feldolgozó futtató számítógépeken meg kell felelnie a minimális hardverkövetelményeknek, ez a szolgáltatás futtatásához kijelölése előtt. Ellenkező esetben az attól függően, hogy az erőforrás-használat más háttérfolyamatot és végrehajtása során runbookok által okozott versengés, a számítógép túlterhelt fog válik, és runbook-feladat késleltetés vagy időtúllépések okozhat. 

#### <a name="solution"></a>Megoldás
Először ellenőrizze a hibrid forgatókönyv-feldolgozó szolgáltatás futtatására kijelölt számítógép megfelel a minimális hardverkövetelményeknek.  Ha igen, figyelheti a Processzor- és memóriafelhasználását a hibrid forgatókönyv-feldolgozó folyamat teljesítményét és a Windows között a korrelációs meghatározásához.  Ha memória vagy a CPU-terhelés, jelezheti, hogy a szükséges frissítése vagy további processzorok hozzáadásával, vagy növelje a memória erőforrás szűk cím, és hárítsa el a hibát. Azt is megteheti válassza ki a különböző számítási erőforrása, amely támogathatja a minimális követelményeknek és a skála, ha terheléshez növelését szükség.         

#### <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>3. ok: A Runbookok nem tudják hitelesíteni magukat a helyi erőforrások

#### <a name="solution"></a>Megoldás
Ellenőrizze a **Microsoft-SMA** egy kapcsolódó esemény leírása az eseménynaplóban *Win32 a folyamat kilépett a következő kód [4294967295]*.  Ez a hiba oka még nem konfigurált hitelesítési a runbookok vagy a hibrid feldolgozócsoport a futtató hitelesítő adatokat adott.  Tekintse át [Runbookokra vonatkozó engedélyek](automation-hrw-run-runbooks.md#runbook-permissions) a runbookok megfelelően konfigurált hitelesítési megerősítéséhez.  

## <a name="next-steps"></a>Következő lépések

Az automatizálás egyéb problémák elhárításához talál segítséget [közös Azure Automation-problémák elhárítása](automation-troubleshooting-automation-errors.md) 