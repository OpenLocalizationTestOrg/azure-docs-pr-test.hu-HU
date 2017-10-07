---
title: "aaaUnderstand és hárítsa el a HDInsight - Azure WebHCat-hibák |} Microsoft Docs"
description: "Megtudhatja, hogyan hdinsight WebHCat által visszaadott tooabout előforduló hibákat, és hogyan tooresolve őket."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1b3d94b1-207d-4550-aece-21dc45485549
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 0071a1e9ed448ae146b93c8f4f518e31b95d27c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a>Ismertetés és a HDInsight WebHCat hibák megoldásához

Tudnivalók a WebHCat használata a hdinsight eszközzel, és hogyan hibák tooresolve őket. WebHCat belső használatára szolgál az ügyféloldali eszközök például az Azure PowerShell és hello Data Lake Tools for Visual Studio.

## <a name="what-is-webhcat"></a>Mi az a WebHCat

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) egy REST API a [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), egy tábla és a Hadoop tárolási alkalmazáskezelési réteg. WebHCat a HDInsight-fürtökön alapértelmezés szerint engedélyezve van, és különböző eszközök toosubmit feladatok által használt, majd a feladat állapotát és hasonló toohello fürt naplózása nélkül.

## <a name="modifying-configuration"></a>Konfigurációjának módosítása

> [!IMPORTANT]
> A jelen dokumentumban szereplő hello hibák számos fordulhat elő, mert túllépte a beállított maximális. Hello feloldási lépés akkor említi, hogy egy érték módosítható, ha Ön hello tooperform hello módosítása a következő egyikét kell használnia:

* A **Windows** fürtök: parancsfájl művelet tooconfigure hello értéket használjon a fürt létrehozása során. További információkért lásd: [Parancsfájlműveletek fejlesztése](hdinsight-hadoop-script-actions.md).

* A **Linux** fürtök: (web- vagy REST API-t) használja Ambari toomodify hello érték. További információkért lásd: [kezelése HDInsight Ambari használatával](hdinsight-hadoop-manage-ambari.md)

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

### <a name="default-configuration"></a>Alapértelmezett konfigurációja

Ha hello a következő alapértelmezett értékek számát, azt WebHCat teljesítményét, vagy hibákat okozhatnak:

| Beállítás | Funkció | Alapértelmezett érték |
| --- | --- | --- |
| [yarn.Scheduler.Capacity.maximum-alkalmazások][maximum-applications] |hello aktív lehet egyidejű feladatok maximális száma (folyamatban vagy fut) |10,000 |
| [templeton.Exec.max-procs][max-procs] |hello egyidejűleg szolgáltatható kérelmek maximális száma |20 |
| [mapreduce.jobhistory.max-kor-ms][max-age-ms] |Hello, hogy hány napig feladatelőzmények megmaradnak |7 nap |

## <a name="too-many-requests"></a>Túl sok kérelem

**HTTP-állapotkód**: 429

| Ok | Megoldás: |
| --- | --- |
| Túllépte a hello maximális párhuzamos által kiszolgált kérelmek WebHCat / perc (alapértelmezett érték 20) |Csökkentse a munkaterhelés tooensure, hogy Ön nem nyújt több mint hello egyidejű kérelmek maximális számát, vagy növelje hello egyidejűleg futtatható kérelmek maximális módosításával `templeton.exec.max-procs`. További információkért lásd: [konfiguráció módosítása](#modifying-configuration) |

## <a name="server-unavailable"></a>A kiszolgáló nem érhető el

**HTTP-állapotkód**: 503-as

| Ok | Megoldás: |
| --- | --- |
| Ez az állapot kód általában akkor fordul elő, hello elsődleges és másodlagos közötti feladatátvételkor HeadNode hello fürt |Várjon két percet, majd próbálja megismételni a műveletet hello |

## <a name="bad-request-content-could-not-find-job"></a>Hibás kérés tartalma: nem található feladat

**HTTP-állapotkód**: 400

| Ok | Megoldás: |
| --- | --- |
| Feladat részleteinek törölve lettek által hello feladatelőzmények tisztító |hello alapértelmezett megőrzési időtartamot feladatelőzmények 7 nap. hello alapértelmezett megőrzési időtartamot a módosításával lehet megváltoztatni `mapreduce.jobhistory.max-age-ms`. További információkért lásd: [konfiguráció módosítása](#modifying-configuration) |
| Feladat leállította tooa feladatátvétel miatt |Ismételje meg a feladat elküldése a mentést tootwo perc |
| Feladatazonosító érvénytelen lett megadva. |Ellenőrizze, hogy helyes-e-e hello feladatazonosító: |

## <a name="bad-gateway"></a>Hibás átjáró

**HTTP-állapotkód**: 502-es

| Ok | Megoldás: |
| --- | --- |
| Belső szemétgyűjtés belül hello WebHCat folyamat folyamatban van |Szemétgyűjtési gyűjtemény toofinish várja meg, vagy hello WebHCat szolgáltatás újraindítása |
| Erőforrás-kezelő szolgáltatás hello válaszára várakozás időkorlátja lejárt. Ez a hiba akkor fordulhat elő, amikor aktív kérelmek száma hello konfigurált hello maximálisan engedélyezett (alapértelmezés szerint 10 000) |Várjon, amíg a jelenleg futó feladatok toocomplete, vagy növelje a hello egyidejű feladat korlát módosításával `yarn.scheduler.capacity.maximum-applications`. További információkért lásd: hello [módosítása konfigurációs](#modifying-configuration) szakasz. |
| Kísérlet tooretrieve keresztül hello összes feladat [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) hívás közben `Fields` túl van beállítva`*` |Nem beolvasni a *összes* feladat részletei. Ehelyett használja `jobid` feladatok csak az egyes feladatazonosítót nagyobb tooretrieve részleteit. Vagy, ne használja`Fields` |
| hello WebHCat-szolgáltatás nem működik HeadNode feladatátvétel során |Két perc várakozás, majd próbálja megismételni a műveletet hello |
| Webhcaten keresztül küldött 500-nál több függőben lévő feladatok |Várjon, amíg jelenleg függő feladatok már befejeződtek több feladat elküldése előtt |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
