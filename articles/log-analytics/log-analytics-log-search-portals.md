---
title: "Portálok a létrehozása és módosítása az Azure Naplóelemzés napló lekérdezések |} Microsoft Docs"
description: "Ez a cikk ismerteti a portálok, melyekkel az Azure Naplóelemzés létrehozásához és szerkesztéséhez jelentkezzen kereséseket."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: 29dfa31d38f85574f84ed351bc5c26224b1a7e16
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Portálok a létrehozása és módosítása az Azure Naplóelemzés napló lekérdezések

> [!NOTE]
> Ez a cikk ismerteti az Azure Naplóelemzés az új lekérdezés nyelven portálok.  További információ az új nyelv, és a frissítés a következő munkaterület eljárás első [Azure Naplóelemzési munkaterület frissítése új naplófájl-keresési](log-analytics-log-search-upgrade.md).  
>
> Ha a munkaterületet még nem lett frissítve az új lekérdezési nyelv, tekintse át [található adatokat, és napló kereséseket a Naplóelemzési](log-analytics-log-searches.md) információt a naplófájl-keresési portál jelenlegi verziója.

A különböző pontjain lehet elérni a Naplóelemzési napló keresések segítségével adatainak lekérése a munkaterületen.  Ténylegesen létrehozásának, és a lekérdezések használata interaktív visszaadott adatok mellett, ha szerkesztésének, két lehetősége, hogy az alábbiakban található.  

## <a name="log-search-portal"></a>Naplófájl-keresési portál
A naplófájl-keresési portál nem érhető el az Azure-portálon vagy az OMS-portálon.  Alkalmas alapvető lekérdezések külön sorba hozható létre.  A naplófájl-keresési portál egy külső portál megnyitása nélkül használható, és számos feladatot végrehajtani a napló keresések többek között a riasztási szabályok létrehozásához, számítógép-csoportok létrehozásáról és a lekérdezés eredményeit az használhatja.  

A naplófájl-keresési portál több szolgáltatásokat nyújt a lekérdezés szerkesztése a lekérdezési nyelv teljes ismerete nélkül.  Ezeket a funkciókat az összegzését kaphat [létrehozás napló keresi a keresési napló portálon Azure Naplóelemzés](log-analytics-log-search-log-search-portal.md).


![Naplófájl-keresési portál](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Speciális Analytics portál
A speciális elemzés portál egy dedikált portál, amely nem érhető el, a keresési napló portálon speciális funkciókat biztosítja.  Szolgáltatások közé tartozik a lekérdezés több sorban szerkesztése, szelektív módon hajtható végre a kódot, környezetfüggő Intellisense és intelligens Analytics képes.  Az Advanced Analytics portál legalkalmasabb összetett lekérdezések, amelyek vagy mentett napló keresést vagy másolja és illeszthetők be más Naplóelemzési elemek.  A naplófájl-keresési portál tartalmaz egy hivatkozást a Advanced Analytics portálon indíthatja el.

![Speciális Analytics portál](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


A speciális funkciók miatt általában használni a speciális elemzés portal az elsődleges eszközként létrehozása és szerkesztése a lekérdezések.  Egyszer meghatározta, hogy a lekérdezés megfelelően működik-e, majd lesz másolja és illessze be máshol például a naplófájl-keresési portál vagy az adatforrásnézet-tervezőből.  Mivel az Advanced Analytics portál már támogatja több sort tartalmazó lekérdezések ellenére kell venni a figyelembe a következőket, amikor a lekérdezés másol ezen a portálon.

- Megjegyzések a lekérdezés kell távolítania, mielőtt másolni és beilleszteni egy másik helyre.  Egy sor Megjegyzés két perjeleket az azt megelőző (/ /).  Ha több sor lekérdezés beillesztése egy egysoros, a sortörések törlődnek.  Megjegyzések érhetők el, ha az összes karakter után az első Megjegyzés a Megjegyzés részének számít.


## <a name="next-steps"></a>Következő lépések

- Az oktatóanyag bízná a [naplófájl-keresési portál](log-analytics-log-search-log-search-portal.md) vagy a [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587) létrehozhat olyan lekérdezéseket.
- Tekintse meg a [útmutató a lekérdezések írásáról](https://go.microsoft.com/fwlink/?linkid=856078) az új lekérdezés nyelven.
