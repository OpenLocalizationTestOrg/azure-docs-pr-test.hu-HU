---
title: "Az alkalmazásproxy alkalmazás található aaaNo munkacsoport-összekötő |} Microsoft Docs"
description: "Ha nem működik az alkalmazás az Azure AD alkalmazásproxy hello összekötő csoportban összekötő esetleg felmerülő problémák megoldása"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4c4baf296b316db131929c9a7c618fb9960713e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Nem található az alkalmazásproxy alkalmazáshoz működő összekötő csoport

Ez a cikk segít tooresolve hello gyakori problémákat nincs észlelhető a következőnél: az alkalmazásproxy alkalmazás összekötő tapasztalt, Azure Active Directoryval integrált.

## <a name="overview-of-steps"></a>A lépések – áttekintés
Ha nem működő alkalmazás összekötő csoportban összekötő, van néhány módon tooresolve hello problémát:

-   Ha címterekhez hello csoportban, akkor a következőket teheti:

    -   Hello jobb helyszíni kiszolgálón új összekötő letöltése, és rendelje hozzá toothis csoport

    -   Az aktív csatlakozó áthelyezi hello csoport

-   Ha nincs aktív összekötők hello csoportban, akkor a következőket teheti:

    -   Az összekötő nem aktív hello OK azonosítása és elhárítása

    -   Az aktív csatlakozó áthelyezi hello csoport

tooknow, amelyek ezen hello probléma, az alkalmazás hello "Alkalmazásproxy" menü megnyitásához, és tekintse meg hello összekötő csoport figyelmeztető üzenet. Azt adja meg, vagy hello csoport kell legalább egy összekötő (kell hello csoport "nincs"), vagy nem aktív összekötők rendelkezik (bár valószínűleg inaktív összekötők).

   ![Az Azure portálon összekötő csoport kijelölése](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Ezen lehetőségek a részletekért lásd: hello megfelelő című részhez. Ezek mindegyikének azt feltételezi, hogy hello összekötő kezelése lap kezdve. Hello hibaüzenet jelenik meg a fenti nézi, ha a hello figyelmeztető üzenet kattintva megnyithatja toothis lap. Ellenkező esetben ez található túl címen**Azure Active Directory**a gombra, majd **vállalati alkalmazások**, majd **alkalmazásproxy.**

   ![Összekötő csoportok kezelése az Azure portálon](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Új összekötő letöltése

toodownload új összekötőt, használja a hello "Összekötő letöltése" gombot hello oldal hello tetején.

Megjegyzés: hello összekötő igények toobe közvetlen toohello a háttéralkalmazás a gépre telepítve, és általában el van helyezve hello hello alkalmazás ugyanarra a kiszolgálóra. A letöltés után hello összekötő meg kell jelennie az ebben a menüben. Kattintson a hello összekötő, és hello "Összekötő csoport" legördülő toomake meg arról, hogy toohello megfelelő csoport tartozik. Hello módosítás mentéséhez.

   ![Hello összekötő letöltését hello Azure portál](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Helyezze át egy aktív összekötő

Ha egy aktív összekötő, amely toohello csoporthoz kell tartoznia, és nem sor a láthatáron toohello célalkalmazás háttér, áthelyezheti hello összekötő hozzárendelt hello csoportjához. toodo kattintson hello összekötő. A hello "Összekötő csoport" mezőben hello legördülő tooselect hello megfelelő csoportot, és kattintson a Mentés gombra.

## <a name="resolve-an-inactive-connector"></a>Hárítsa el az inaktív csatlakozó

Ha hello csak hello csoport összekötők nem működnek, akkor ezeknél valószínűleg olyan gépen, amely nem rendelkezik az összes hello szükséges portok feloldva.

Lásd: hello portok kapcsolatos problémák elhárítása dokumentumban talál részletes információt a probléma kivizsgálása.

## <a name="next-steps"></a>Következő lépések
[Az Azure AD-alkalmazásproxy összekötők ismertetése](application-proxy-understand-connectors.md)


