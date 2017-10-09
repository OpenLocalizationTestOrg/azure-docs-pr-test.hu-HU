---
title: "létrehozásának és szerkesztésének napló lekérdezések az Azure Naplóelemzés aaaPortals |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogy az Azure Naplóelemzés toocreate használ, és szerkesztésére napló hello portálok."
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
ms.openlocfilehash: 7a2657574a132b2c4298511bb31259e68113ac91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Portálok a létrehozása és módosítása az Azure Naplóelemzés napló lekérdezések

> [!NOTE]
> Ez a cikk ismerteti a portálon az Azure Naplóelemzés hello új lekérdezési nyelv használatával.  További tudnivalók hello új nyelv, és hello eljárás tooupgrade lekérése a munkaterületen a [az Azure Naplóelemzés munkaterület toonew napló keresés frissítése](log-analytics-log-search-upgrade.md).  
>
> Ha a munkaterületet még nem frissített toohello új lekérdező nyelv, tekintse át túl[található adatokat, és napló kereséseket a Naplóelemzési](log-analytics-log-searches.md) hello napló keresése portál jelenlegi verziója hello olvashat.

A különböző pontjain lehet elérni a Naplóelemzési tooretrieve adatok napló keresések hello munkaterületről használhatja.  A ténylegesen létrehozása és szerkesztése a lekérdezések továbbá tooworking interaktív módon az adatokat adott vissza, ha, két lehetőség közül választhat, amelyek folyamata az alábbiakban olvasható.  

## <a name="log-search-portal"></a>Naplófájl-keresési portál
hello napló keresése portal hello Azure-portálon vagy hello OMS-portálon elérhető-e.  Alkalmas alapvető lekérdezések külön sorba hozható létre.  hello napló keresése portál egy külső portál megnyitása nélkül használható, és használhatja tooperform számos feladatot a napló keresések többek között a riasztási szabályok létrehozásához, számítógép-csoportok létrehozásáról és hello hello lekérdezés eredményeit.  

hello napló keresése portal több szolgáltatásokat nyújt hello lekérdezés szerkesztése hello lekérdezési nyelv teljes ismerete nélkül.  Ezeket a funkciókat az összegzését kaphat [létrehozás napló megkeresi a hello napló keresése portálon Azure Naplóelemzés](log-analytics-log-search-log-search-portal.md).


![Naplófájl-keresési portál](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Speciális Analytics portál
hello Advanced Analytics portál egy dedikált portál, amely a speciális hello napló keresése portál nem érhető el.  Szolgáltatások hello képességét tooedit több sorban lekérdezés tartalmazza, szelektív módon hajtható végre a kódot, környezetfüggő Intellisense és intelligens elemzés.  hello Advanced Analytics portál legalkalmasabb összetett lekérdezések, amelyek vagy mentett napló keresést vagy másolja és illeszthetők be más Naplóelemzési elemek.  Hello Advanced Analytics portál hello napló keresése portálon hivatkozás indíthatja el.

![Speciális Analytics portál](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


A speciális funkciók miatt általában fogjuk hello Advanced Analytics portál az elsődleges eszközként létrehozásának és szerkesztésének lekérdezések.  Miután meghatározta, hogy hello lekérdezés megfelelően működik-e, majd lesz másolja és illessze be máshol például hello napló keresése portálon vagy az adatforrásnézet-tervezőből.  Mivel hello Advanced Analytics portál azonban támogatja a több sort tartalmazó lekérdezések, meg kell tootake hello következő számításba venni, ha egy lekérdezést a portálról.

- Megjegyzések hello lekérdezés kell távolítania, mielőtt másolni és beilleszteni egy másik helyre.  Egy sor Megjegyzés két perjeleket az azt megelőző (/ /).  Ha több sor lekérdezés beillesztése egy egysoros, a sortörések törlődnek.  Megjegyzések érhetők el, ha az összes karakter után hello első megjegyzés hello Megjegyzés részének számít.


## <a name="next-steps"></a>Következő lépések

- Végezze el az oktatóanyagnak hello használatával [naplófájl-keresési portál](log-analytics-log-search-log-search-portal.md) vagy hello [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587) toocreate lekérdezések.
- Tekintse meg a [útmutató a lekérdezések írásáról](https://go.microsoft.com/fwlink/?linkid=856078) hello új lekérdezési nyelv használatával.
