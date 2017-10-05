---
title: "Az Azure blob-tárolókban, SQL Server-adatokat, és a Hive táblák |} Microsoft Docs"
description: "Hogyan különböző Azure enviromnents tárolt adatokba."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 80a9dfae-e3a6-4cfb-aecc-5701cfc7e39d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 0683be564a88ef54882e8d882d196851ecac243d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="heading"></a>Az Azure blob-tárolókban, SQL Server-adatokat, és a Hive táblák
Ez a dokumentum témakörökre mutató hivatkozásokat tartalmaz, amely bemutatja, hogyan adhat az három különböző Azure helyek egyikén tárolt adatokat:

* **Az Azure blob-tároló adatok** van mintát az programozott módon letöltheti, és majd mintavételi minta Python kóddal.
* **SQL Server-adatok** mintát venni, SQL és a Python programozási nyelv használatával. 
* **Hive tábla adatai** Hive-lekérdezésekkel van mintát.

A következő **menü** az adatokat az Azure storage környezetben minden egyes a leíró témakörök hivatkozásait. 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Ez a mintavételi feladat Ez a lépés a [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

**Miért érdemes az adatokat?**

Ha azt tervezi, hogy elemezheti az adatkészlet túl nagy, akkor általában down kétmintás az adatokat, hogy az kisebb, de reprezentatív és könnyebben kezelhető méretű jó ötlet. Ez lehetővé teszi az adatok ismertetése, feltárása és a szolgáltatás mérnöki csapathoz. A szerepkör a Cortana Analytics folyamat ahhoz, hogy az adatok feldolgozása funkciók és a gépi tanulási modellek gyors prototípusának.

