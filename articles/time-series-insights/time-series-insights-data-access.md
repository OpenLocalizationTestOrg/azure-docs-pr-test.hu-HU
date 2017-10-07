---
title: "aaaData hozzáférési szabályzatokat az Azure idő adatsorozat Insights |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja toomanage adat-hozzáférési házirendjeit idő adatsorozat insightsban"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: omravi
ms.openlocfilehash: f286d26c8c5c851c523e9384760dc4b10711fa3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="grant-data-access-tooa-time-series-insights-environment-using-azure-portal"></a>Adja meg a data access tooa idő adatsorozat Insights környezet az Azure portál használatával

A Time Series Insights-környezetek két különböző típusú hozzáférési házirenddel rendelkeznek:

* Felügyeleti hozzáférési házirendek
* Adathozzáférési házirendek

Mindkét házirend különféle engedélyeket biztosít az Azure Active Directory rendszerbiztonsági tagjai (felhasználók és alkalmazások) számára egy adott környezetre vonatkozóan. hello rendszerbiztonsági tagok (felhasználók és alkalmazások) kell tartozniuk aktív toohello hello környezet tartalmazó hello előfizetéshez tartozó címtár (vagy "Azure-bérlőhöz").

Felügyeleti hozzáférési házirendek engedélyek kapcsolódó toohello konfigurációs hello környezet, például a engedélyezése
*   A környezet hello eseményforrások, létrehozását és törlését hivatkozási adatkészletek, és
*   Adat-hozzáférési házirendjeit hello kezelése.

Adat-hozzáférési házirendjeit engedélyeket tooissue lekérdezések, kezelheti a referenciaadatok hello környezetben és lekérdezések és hello környezet perspektívákat.

házirendek hello kétféle engedélyezése egyértelmű elkülönítése is megvalósuljon toohello kezelési hello környezet és a hozzáférés toohello adatok hello környezeten belül. Például is lehetséges toosetup olyan környezetet úgy, hogy hello tulajdonos/létrehozó hello környezet hello adatelérési törlődik. Továbbá a felhasználók és a szolgáltatások, amelyek számára engedélyezett tooread adatok hello környezetből adható nem toohello konfiguráció hello környezet.

## <a name="grant-data-access"></a>Adathozzáférés biztosítása
hello következő lépések bemutatják, hogyan toogrant adatok elérését egy egyszerű:

1.  Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2.  Kattintson az "Összes erőforrás" hello menü hello hello Azure-portálon a bal oldalán.
3.  Válassza ki az Azure Time Series Insights-környezetet.

  ![Kezelheti a hello idő adatsorozat Insights forrás - környezet](media/data-access/getstarted-grant-data-access1.png)

4.  Válassza az „Adatsík-hozzáférés” lehetőséget, majd kattintson a „Hozzáadás” gombra.

  ![Hello idő adatsorozat Insights forrás kezelése – hozzáadása](media/data-access/getstarted-grant-data-access2.png)

5.  Kattintson a „Felhasználó kiválasztása” gombra.
6.  Keresse meg és válassza ki a felhasználói hello e-mailben.
7.  Kattintson a „Kiválasztás” gombra a „Felhasználó kiválasztása” panelen.

  ![Hello idő adatsorozat Insights forrás - felhasználó kijelölése kezelése](media/data-access/getstarted-grant-data-access3.png)

8.  Kattintson a „Szerepkör kiválasztása” gombra.
9.  Válassza ki a "Közreműködői", ha tooallow felhasználói toochange referenciaadatok és perspektívák és lekérdezések megosztása más felhasználókkal hello környezet. Ellenkező esetben válassza a "Olvasó" tooallow felhasználói adatait hello környezetben, és mentse a személyes (megosztott) lekérdezések hello környezetben.
10. Kattintson az "Ok" hello "Szerepkör kiválasztása" panelen.

  ![Hello idő adatsorozat Insights forrás - válassza szerepkör kezelése](media/data-access/getstarted-grant-data-access4.png)

11. Kattintson az "Ok" hello "A felhasználói szerepkör kiválasztása" panelen.
12. A következőnek kell megjelennie:

  ![Hello idő adatsorozat Insights forrás - eredmények kezelése](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Következő lépések

* [Eseményforrás létrehozása](time-series-insights-add-event-source.md)
* [Események küldése](time-series-insights-send-events.md) toohello eseményforrás
* A környezet megtekintése a [Time Series Insights portálon](https://insights.timeseries.azure.com)
