---
title: "Az Azure Stream Analytics-feladatok folyamatossága |} Microsoft Docs"
description: "Így a Stream Analytics útmutatást frissítés rugalmas feladatok."
services: stream-analytics
documentationCenter: 
authors: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 3ac65c93ecb47e93e963dd9869a7af70f73b19c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Stream Analytics-feladat megbízhatóságának biztosítása szolgáltatás frissítései során

Egy teljes körűen felügyelt szolgáltatás, amely nem része hello funkció toointroduce új szolgáltatás funkcióit és fejlesztéseit gyors ütemben. Ennek eredményeképpen a Stream Analytics heti (vagy gyakoribb) telepítése szolgáltatásfrissítés is rendelkezhetnek. Függetlenül történik, hogy mekkora tesztelése nincs továbbra is fennáll a kockázata, hogy egy meglévő, futó feladat megszakadhat toohello bevezetése egy hiba miatt. Az ügyfelek, akik kritikus adatfolyam feldolgozási feladatok futtatása a kockázatok kell toobe kerülni. A mechanizmus az ügyfelek használhatják tooreduce ennek a veszélyét Azure  **[párosított régió](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  modell. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Hogyan hajtsa végre az Azure párosított régiók ezen probléma megoldásának?

A Stream Analytics biztosítja, hogy külön párosított régiókban feladatok frissülnek. Ennek eredményeképpen nincs elegendő idő hiány között hello frissítések tooidentify lehetséges legfrissebb hibák elhárítása, és elháríthatja a.

_Közép-Indiában hello kivételével_ (amelynek párosított régió, Dél-Indiában és nem rendelkezik Stream Analytics jelenléte), egy frissítés tooStream elemzés nem lépne hello: hello telepítését azonos idő párosított régiók meg. A központi telepítések több régióba **hello a ugyanabban a csoportban** fordulhat elő, **: hello ugyanannyi időt vesz igénybe**.

hello foglalkozó  **[rendelkezésre állását és párosított régiók](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)**  hello legtöbb naprakész információt, amelyen a régiók párosítva van.

Elkeverni toodeploy azonos feladatok párosítva tooboth régiók azokat a felhasználókat. Továbbá a belső figyelési képességek, az ügyfelek Analytics megtalálhatók tooStream ajánlott toomonitor hello feladatok, mintha **mindkét** éles feladat. Ha szünet azonosított toobe hello Stream Analytics szolgáltatás frissítés eredménye, eszkalálása megfelelően, és alsóbb rétegbeli fogyasztók toohello kifogástalan feladat kimenete a feladatátvétel. Eszkalációs toosupport hello párosított régió megakadályozása hello új központi telepítési vonatkozzon, és a párosítva hello feladatok hello integritásának fenntartása.
