---
title: "aaaOMSManagement megoldás gyakorlati tanácsok |} Microsoft Docs"
description: 
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
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: 08cf1c101e301d24fb5c2bf4bc02a978e508a198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-creating-management-solutions-in-operations-management-suite-oms-preview"></a>Ajánlott eljárások létrehozása kezelési megoldásai Operations Management Suite (OMS) (előzetes verzió)
> [!NOTE]
> Ez az előzetes dokumentum megoldások létrehozásához az OMS Szolgáltatáshoz, amely jelenleg előzetes verziójúak. Az alábbiakban semmilyen sémát tulajdonos toochange.  

Ez a cikk vonatkozó gyakorlati tanácsokat [felügyeleti megoldás fájl létrehozásával](operations-management-suite-solutions-solution-file.md) Operations Management Suite (OMS).  Ezek az információk további gyakorlati tanácsok meghatározott frissül.

## <a name="data-sources"></a>Adatforrások
- Adatforrások lehet [egy Resource Manager sablonhoz konfigurált](../log-analytics/log-analytics-template-workspace-configuration.md), de azok nem tartozhatnak a megoldásfájlt.  hello oka, hogy az adatforrások konfigurálása jelenleg nem idempotent, ami azt jelenti, hogy a megoldás felülírhatja a meglévő konfiguráció hello felhasználói munkaterületen.<br><br>A megoldás például szükség lehet hello alkalmazások eseménynaplójában keresse meg a figyelmeztetési és események.  Ha megadja ezt adatforrásként a megoldásban, azzal kockáztatja eltávolítása tájékoztató eseményeket, ha hello felhasználó rendelkezik-e ez nincs konfigurálva a munkaterületen.  Szereplő összes esemény, ha majd, előfordulhat, hogy lehet túlzott információk események gyűjtése hello felhasználói munkaterületen.

- Ha a megoldás hello szabványos adatok forrásokból származó adatok szükséges, majd be kell állítania a előfeltételként.  Állapot dokumentációjának tartozó hello ügyfél kell beállítania hello adatforrás saját.  
- Adja hozzá a [az egymást követő ellenőrzése](../log-analytics/log-analytics-view-designer-tiles.md) tooany megtekinti a megoldás tooinstruct hello felhasználó adatforrásokon, hogy a szükséges adatok toobe konfigurálva kell toobe üzenet gyűjti.  Ez az üzenet a hello csempére hello nézet akkor látható, ha a szükséges adatok nem található.


## <a name="runbooks"></a>Runbookok
- Adja hozzá egy [automatizálás ütemezést](../automation/automation-schedules.md) minden runbook a megoldásban, amelyet a toorun ütemezés szerint.
- Hello tartalmaznak [IngestionAPI modul](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) a a megoldás toobe toohello Naplóelemzési adattárház írása runbookok által használt.  Hello megoldás konfigurálása túl[hivatkozás](operations-management-suite-solutions-solution-file.md#solution-resource) úgy, hogy az informatikai marad, ha eltávolítják hello megoldás ehhez az erőforráshoz.  Ez lehetővé teszi több megoldások tooshare hello modul.
- Használjon [automatizálási változók](../automation/automation-schedules.md) tooprovide értékei, felhasználók később felmerülhet toochange toohello megoldás.  Akkor is, ha hello megoldás konfigurált toocontain hello változó, értéke továbbra is módosítható.

## <a name="views"></a>Nézetek
- Minden megoldás hello felhasználói portál alkalmazásban megjelenő egyetlen nézetben kell tartalmaznia.  hello nézet tartalmazhat több [képi megjelenítés részek](../log-analytics/log-analytics-view-designer-parts.md) különböző tooillustrate adatkészletek.
- Adja hozzá a [az egymást követő ellenőrzése](../log-analytics/log-analytics-view-designer-tiles.md) tooany megtekinti a megoldás tooinstruct hello felhasználó adatforrásokon, hogy a szükséges adatok toobe konfigurálva kell toobe üzenet gyűjti.
- Hello megoldás konfigurálása túl[tartalmazhat](operations-management-suite-solutions-solution-file.md#solution-resource) hello nézet, így az törlődik, ha eltávolítják hello megoldás.

## <a name="alerts"></a>Riasztások
- Meghatározhatja hello címzettek listáját hello megoldásfájlt paraméterként, így hello felhasználói megadhatja őket, amikor hello megoldás telepítése.
- Hello megoldás konfigurálása túl[hivatkozás](operations-management-suite-solutions-solution-file.md#solution-resource) riasztás szabályok úgy, hogy a felhasználók módosíthatják a konfigurációt.  Akkor érdemes toomake módosítások – például hello címzettjeinek listája módosítása, hello küszöbérték hello riasztás módosítása vagy hello riasztási szabály letiltása. 


## <a name="next-steps"></a>Következő lépések
* Hello alapvető folyamatainak bízná [tervezéséről és kiépítéséről olyan felügyeleti megoldást](operations-management-suite-solutions-creating.md).
* Ismerje meg, hogyan túl[hozzon létre egy megoldást fájlt](operations-management-suite-solutions-solution-file.md).
* [Adja hozzá a mentett keresések és riasztások](operations-management-suite-solutions-resources-searches-alerts.md) tooyour felügyeleti megoldás.
* [Nézetek hozzáadása](operations-management-suite-solutions-resources-views.md) tooyour felügyeleti megoldás.
* [Adja hozzá az Automation-forgatókönyveket és egyéb erőforrások](operations-management-suite-solutions-resources-automation.md) tooyour felügyeleti megoldás.

