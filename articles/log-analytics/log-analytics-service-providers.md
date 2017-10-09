---
title: "Elemzési szolgáltatások szolgáltatók aaaLog |} Microsoft Docs"
description: "Naplóelemzési segítségével felügyelt szolgáltató (MSPs), a nagyobb vállalatok független szoftver szállítói (ISV-k) és az üzemeltetési szolgáltatók kezelni és megfigyelni a kiszolgálók az ügyfél helyszíni vagy felhőalapú infrastruktúrában."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: c07f0b9f-ec37-480d-91ec-d9bcf6786464
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: richrund
ms.openlocfilehash: 3c0a93232293f90385c6c724be436cee1cf855f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-features-for-service-providers"></a>Napló Analytics szolgáltatások szolgáltatók
A Naplóelemzési segítségével felügyelt szolgáltató (MSPs), a nagyobb vállalatok, független szoftvergyártók (ISV-k) és üzemeltetési szolgáltatók kezelni és megfigyelni a kiszolgálók az ügyfél helyszíni vagy felhőalapú infrastruktúrában. 

A nagyobb vállalatok megosztása sok a hasonlóság szolgáltatók, különösen, ha egy központi informatikai csoportját, amelyek felügyeletéért felelős informatikai számos különböző üzleti egységek számára. Az egyszerűség, a dokumentum hello kifejezést használja *szolgáltató* , de hello ugyanezeket a funkciókat is vállalatok és más ügyfelek számára érhető el.

## <a name="cloud-solution-provider"></a>Cloud Solution Provider program
A partnerek és szolgáltatók, amelyek hello [Cloud Solution Provider (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programot, Log Analyticshez egyike hello CSP előfizetés elérhető Azure-szolgáltatásokhoz. 

A Naplóelemzési, a következő képességeket hello engedélyezve van az *Cloud Solution Provider* előfizetések.

Mint egy *Cloud Solution Provider* is:

* A Naplóelemzési munkaterület létrehozása a bérlő (ügyfél) az előfizetéshez.
* Hozzáférés a bérlők által létrehozott munkaterületek. 
* Adja hozzá, és távolítsa el a felhasználói hozzáférés toohello munkaterület Azure felhasználói eszköz használatával. Ha a bérlő munkaterületén hello OMS portál hello felhasználófelügyeleti lapra lépéshez a beállítások nem érhető el
  * A Naplóelemzési nem támogatja a szerepköralapú hozzáférés még - ad egy felhasználónak `reader` hello Azure-portálon az engedély lehetővé teszi, hogy azokat toomake konfigurációs módosítások az hello OMS-portálon

toolog tooa bérlői előfizetéshez, toospecify hello bérlői azonosító szükséges. hello bérlői azonosító gyakran hello e-mail cím toosign az utolsó része.

* Az OMS-portálon hello hozzáadása `?tenant=contoso.com` hello portál hello URL-címben. Például: `mms.microsoft.com/?tenant=contoso.com`
* A PowerShellben használja az hello `-Tenant contoso.com` paraméter használatakor `Add-AzureRmAccount` parancsmag
* hello bérlői azonosító automatikusan fel lesz véve hello használatakor `OMS portal` hello Azure portál tooopen hivatkozásra, és jelentkezzen be a kijelölt hello munkaterület toohello OMS-portálon

Mint egy *ügyfél* , a Cloud Solution Provider is:

* Napló analytics munkaterületek egy CSP-előfizetés létrehozása
* Hello CSP által létrehozott Access munkaterületek
  * Használjon hello `OMS portal` hello Azure portál tooopen hivatkozásra, és jelentkezzen be a kijelölt hello munkaterület toohello OMS-portálon
* Megtekintése és használata hello felhasználófelügyeleti lapra lépéshez a beállítások az hello OMS-portálon

> [!NOTE]
> hello részei biztonsági mentés és a Naplóelemzési megoldások a Site Recovery nem képes tooconnect tooa Recovery Services-tároló, és nem lehet konfigurálni egy CSP-előfizetésben. 
> 
> 

## <a name="managing-multiple-customers-using-log-analytics"></a>A Naplóelemzési több felhasználója kezelése
Javasoljuk, hogy hozzon létre egy Naplóelemzési munkaterület minden felügyelt ügyfél esetében. A Naplóelemzési munkaterület biztosítja:

* A tárolt adatok toobe földrajzi helyét. 
* A számlázáshoz szükséges részletes adatokat 
* Az adatok elkülönítését 
* Egyedi konfigurációja

Hozzon létre egy ügyfél egy munkaterület, képes tookeep külön minden egyes felhasználói adatok és is az egyes ügyfelek hello használatának követése.

További részleteket a időpontját és okát toocreate több munkaterületek ismertetett [hozzáférés toolog analytics kezelése az](log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Létrehozása és ügyfél munkaterületek konfigurációjának automatizálható használatával [PowerShell](log-analytics-powershell-workspace-configuration.md), [Resource Manager-sablonok](log-analytics-template-workspace-configuration.md), vagy hello segítségével [REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).

munkaterület konfigurációs hello Resource Manager-sablonok használata lehetővé teszi toohave egy konfigurációs használt toocreate és munkaterületek konfigurálása. Biztos lehet abban, hogy munkaterületek hoz létre, azok automatikusan ügyfelek tooyour követelmények konfigurálva. Ha frissíti a követelmények, hello sablon frissül, és újra alkalmazza a hello meglévő munkaterületek. Ez a folyamat biztosítja, hogy még meglévő munkaterületek megfelelnek-e az új szabványok.    

Több Naplóelemzési munkaterület kezelése esetén ajánlott minden egyes munkaterület integrálása a meglévő hibajegyalapú rendszert / operatív konzol használatával hello [riasztások](log-analytics-alerts.md) funkciót. A meglévő rendszerekkel integrálásával támogató személyzete számára továbbra is toofollow a megszokott eljárások. A Naplóelemzési rendszeresen ellenőrzi az egyes munkaterületeken hello riasztási megadott feltételek alapján, és riasztást küld, ha nincs szükség műveletre.

Az adatok személyre szabott nézeteket, használja a hello [irányítópult](../azure-portal/azure-portal-dashboards.md) hello Azure-portálon képességét.  

Végrehajtó szint adatainak összefoglalója használható munkaterületek között jelentések hello Naplóelemzési közötti integrációt és [Power bi](log-analytics-powerbi.md). Ha egy másik jelentéskészítő rendszer toointegrate van szüksége, használhatja a hello Search API (PowerShell vagy [REST](log-analytics-log-search-api.md)) toorun lekérdezések és exportálás a keresési eredményekben.

## <a name="next-steps"></a>Következő lépések
* Létrehozása és konfigurálása munkaterületek használatával automatizálhatja [Resource Manager-sablonok](log-analytics-template-workspace-configuration.md)
* Használatával munkaterületek létrehozásának automatizálása [PowerShell](log-analytics-powershell-workspace-configuration.md) 
* Használjon [riasztások](log-analytics-alerts.md) toointegrate meglévő rendszerekkel
* Összegző jelentéseket használatával [Power BI](log-analytics-powerbi.md)

