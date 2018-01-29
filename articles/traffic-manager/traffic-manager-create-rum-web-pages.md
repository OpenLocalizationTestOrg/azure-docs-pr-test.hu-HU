---
title: "Valós felhasználói mérések Azure Traffic Manager a weblapokat |} Microsoft Docs"
description: "Állítsa be a weblapok felvétele a Traffic Manager valós felhasználói mérések küldhet."
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: kumud
ms.custom: 
ms.openlocfilehash: 7f4088cf4470b1f9fa22c4ec83a9f92657032734
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/23/2018
---
# <a name="how-to-send-real-user-measurements-to-azure-traffic-manager-using-web-pages"></a>Valós felhasználói mérések küldése az Azure Traffic Manager weblapjainak használata

>[!NOTE]
>A tényleges felhasználói mérések szolgáltatást a Traffic Manager nyilvános előzetes verziójában, és nem rendelkezhet azonos szintű rendelkezésre állást és megbízhatóságot, szolgáltatások, amelyek általában a rendelkezésre állási kiadási. A szolgáltatás nem támogatott, van, korlátozott képességeket, és előfordulhat, hogy nem érhető el az összes Azure helyét. A rendelkezésre állás és a szolgáltatás állapotát a legfrissebb értesítések, tekintse meg a [Azure Traffic Manager-frissítések](https://azure.microsoft.com/updates/?product=traffic-manager) lap.

A weblapok valós felhasználói mérések felvétele a Traffic Manager küldhet egy valós felhasználói mérések (RUM) kulcs beszerzése és beágyazása a weblapra a generált kódot is konfigurálhat.

## <a name="obtain-a-real-user-measurements-key"></a>Egy valós felhasználói mérések kulcs beszerzése

A mérési igénybe vehet, és az ügyfélalkalmazást a Traffic Manager küldjön a szolgáltatást egy egyedi karakterlánc, úgynevezett használatával azonosítják a **valós felhasználói mérések (RUM) kulcs**. Kaphat egy RUM kulcsot az Azure-portált használja, a REST API-t vagy a PowerShell vagy az Azure parancssori felület használatával.

Az Azure-portál használatával RUM kulcs beszerzése:
1. Egy böngészőből jelentkezzen be az Azure-portálon. Ha már nincs fiókja, regisztrálhat egy ingyenes egy hónapos próbaverzióra.
2. A portál keresősávban, keresse meg a módosítani kívánt Traffic Manager-profil neve, és kattintson a Traffic Manager-profilt az eredmények között, amelyek a jelennek meg.
3. Kattintson a Traffic Manager-profil panelje **valós felhasználói mérések** alatt **beállítások**.
4. Kattintson a **készítése kulcs** RUM új kulcs létrehozásához.
 
  ![Valós felhasználói mérések kulcs létrehozása](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **1. ábra: Felhasználó mérések kulcs létrehozása**

5. A panel most hozza létre a RUM kulcsot és a JavaScript kódrészletet, amelyet a beilleszthető a HTML-weblap jeleníti meg.
 
    ![Valós felhasználói mérések kulcs JavaScript-kód](./media/traffic-manager-create-rum-web-pages/rum-javascript-code.png)

    **2. ábra: Felhasználó mérések kulcs és a mérési JavaScript**
 
6.  Kattintson a **másolási** gomb másolni a JavaScript-kódot. 

>[!IMPORTANT]
> A generált JavaScript valós felhasználói mérések funkció használatával működnek majd megfelelően. Ez a parancsfájl vagy a valós felhasználói mérések által használt parancsfájlok módosításait előre nem látható viselkedéshez vezethet.

## <a name="embed-the-code-to-an-html-web-page"></a>A kód egy HTML-weblap beágyazása

Ha beszerezte a RUM kulcsot, a következő lépéssel hoz a másolt JavaScript beágyazása a felhasználók által felkeresett HTML-lapot. HTML szerkesztésével teheti az sokféleképpen és más eszközök és a munkafolyamatok. Ez a példa bemutatja a parancsfájl hozzáadása egy HTML-weblap frissítése. Ez az útmutató segítségével a HTML-forrás felügyeleti munkafolyamat igazodjon.

1.  Egy szövegszerkesztőben nyissa meg a HTML-weblap
2.  Illessze be a másolt volt az előző lépésben a HTML törzs szakaszába JavaScript-kódot (a másolt kódot sor 8 & 9, lásd a 3. ábra:).
 
    ![Weblap valós felhasználói mérések Javascript-kód beágyazása](./media/traffic-manager-create-rum-web-pages/real-user-measurement-embed-script.png)  

    **3. ábra: A beágyazott valós felhasználói mérések JavaScript egyszerű HTML**

3.  A HTML-fájl mentése
4. Ez a lap megjelenítése egy webböngészőben, amikor legközelebb letölti a hivatkozott JavaScript, és a parancsfájl hajtja végre a mérési és jelentéskészítési műveletek.


## <a name="next-steps"></a>További lépések
- További információ [valós felhasználói mérések](traffic-manager-rum-overview.md)
- Ismerje meg, [Traffic Manager működése](traffic-manager-overview.md)
- További információ a [forgalom-útválasztási módszert](traffic-manager-routing-methods.md) Traffic Manager által támogatott
- Megtudhatja, hogyan [Traffic Manager-profil létrehozása](traffic-manager-create-profile.md)

