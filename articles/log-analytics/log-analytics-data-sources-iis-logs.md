---
title: "Naplóelemzési bejelentkezik aaaIIS |} Microsoft Docs"
description: "Internet Information Services (IIS) felhasználói tevékenység Naplóelemzési által gyűjtendő naplófájlokat tárolja.  Ez a cikk ismerteti, hogyan hello rekordok részleteit és IIS-napló tooconfigure gyűjtésére hoznak létre a hello OMS-tárházban."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: bwren
ms.openlocfilehash: c5575351090cdabaf651bcb49867794ee3a4b6e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="iis-logs-in-log-analytics"></a>A Naplóelemzési naplózza az IIS
Internet Information Services (IIS) felhasználói tevékenység Naplóelemzési által gyűjtendő naplófájlokat tárolja.  

![IIS-naplók](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Naplózza az IIS konfigurálása
A Naplóelemzési bejegyzéseket gyűjt, meg kell, az IIS által létrehozott naplófájlok [az IIS naplózás konfigurálása](https://technet.microsoft.com/library/hh831775.aspx).

A Naplóelemzési csak támogatja az IIS W3C formátumban tárolja, és nem támogatja az egyéni mezők és az IIS naplózás speciális.  
A Naplóelemzési nem naplógyűjtéshez NCSA vagy az IIS natív formátumban.

IIS-napló állítsa a Naplóelemzési hello [Naplóelemzés beállításai adatok menüben](log-analytics-data-sources.md#configuring-data-sources).  Nincs a konfiguráció nem szükséges másik kiválasztásával **gyűjtése W3C-formátum az IIS-naplófájljai**.

Azt javasoljuk, hogy ha engedélyezi az IIS naplógyűjtést, konfigurálnia kell hello IIS napló átfordulási beállítás az egyes kiszolgálókon.

## <a name="data-collection"></a>Adatgyűjtés
A Naplóelemzési gyűjti össze az IIS-napló bejegyzései minden csatlakoztatott forrás körülbelül 15 percenként.  hello ügynök helyére azt gyűjti össze az egyes eseménynapló rögzíti.  Hello ügynök offline állapotba kerül, ha majd Naplóelemzési gyűjt eseményeket ahol utolsó abbamaradtak, még akkor is, ha olyan eseményeket hozta létre, miközben hello ügynök offline állapotban volt.

## <a name="iis-log-record-properties"></a>IIS-napló rekord tulajdonságai
Naplórekordok az IIS típusa lehet **W3CIISLog** és a következő táblázat hello hello jellemzőkkel rendelkezik:

| Tulajdonság | Leírás |
|:--- |:--- |
| Computer |Az összegyűjtött esemény hello hello számítógép nevét. |
| cIP |Hello ügyfél IP-címe. |
| csMethod |Hello kérelem például a GET vagy POST metódus. |
| csReferer |Adott hello hely felhasználó hivatkozásán toohello aktuális helyről. |
| csUserAgent |Hello ügyfél böngésző típusa. |
| csUserName |Hello neve hitelesítette a felhasználót, amely hello kiszolgáló érhető el. A névtelen felhasználókat kötőjel jelzi. |
| csUriStem |Például egy weblap hello kérelem célját. |
| csUriQuery |Lekérdezés, ha van ilyen, hogy hello ügyfél próbált tooperform. |
| ManagementGroupName |Az Operations Manager-ügynökök hello felügyeleti csoport neve.  Más ügynökök, ez pedig AOI -\<munkaterület azonosítója\> |
| RemoteIPCountry |Ország hello hello ügyfél IP-címe. |
| RemoteIPLatitude |A földrajzi hosszúság hello ügyfél IP-cím. |
| RemoteIPLongitude |A földrajzi hosszúság értéke hello ügyfél IP-cím. |
| scStatus |HTTP-állapotkódot. |
| scSubStatus |A részállapot hibakód. |
| scWin32Status |Windows állapotkódját. |
| a sIP |Hello webalkalmazás-kiszolgáló IP-címe. |
| SourceSystem |Az OpsMgr |
| sPort |Hello server hello ügyfél-portjához csatlakozik. |
| sSiteName |Hello IIS-webhely neve. |
| TimeGenerated |Dátum és idő hello bejegyzés naplózásának. |
| TimeTaken |Idő tooprocess hello hosszát ezredmásodpercben kérelmet. |

## <a name="log-searches-with-iis-logs"></a>Napló keresések az IIS-napló
hello következő táblázat példákat különböző napló lekérdezések IIS naplórekordokat beolvasása.

| Lekérdezés | Leírás |
|:--- |:--- |
| Típus = W3CIISLog |Az összes IIS-napló rögzíti. |
| Típus W3CIISLog scStatus = = 500 |Az összes IIS napló rekordot 500 visszaadott állapotát. |
| Típus = W3CIISLog &#124; Mérték count() cIP által |Száma az IIS naplóbejegyzéseket, ügyfél IP-cím alapján. |
| Típus = W3CIISLog csHost = "www.contoso.com" &#124; Mérték count() csUriStem által |Száma az IIS a naplóbejegyzéseket, URL-cím által hello állomás www.contoso.com. |
| Típus = W3CIISLog &#124; Mérték Sum(csBytes) számítógép &#124; a legfontosabb 500 000 összeget |Minden egyes IIS-számítógép által fogadott összes bájt. |

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezések hello megváltozna toohello következő.

> | Lekérdezés | Leírás |
|:--- |:--- |
| W3CIISLog |Az összes IIS-napló rögzíti. |
| W3CIISLog &#124; Ha scStatus == 500 |Az összes IIS napló rekordot 500 visszaadott állapotát. |
| W3CIISLog &#124; count() összesíteni cIP |Száma az IIS naplóbejegyzéseket, ügyfél IP-cím alapján. |
| W3CIISLog &#124; Ha csHost == "www.contoso.com" &#124; count() összesíteni csUriStem |Száma az IIS a naplóbejegyzéseket, URL-cím által hello állomás www.contoso.com. |
| W3CIISLog &#124; Számítógép &#124; sum(csBytes) összefoglalója 500 000 összeget igénybe |Minden egyes IIS-számítógép által fogadott összes bájt. |

## <a name="next-steps"></a>Következő lépések
* Más Naplóelemzési toocollect konfigurálása [adatforrások](log-analytics-data-sources.md) elemzés céljából.
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat.
* Riasztások konfigurálása a Log Analyticshez tooproactively értesíti az IIS-naplókba található fontos feltételek.
