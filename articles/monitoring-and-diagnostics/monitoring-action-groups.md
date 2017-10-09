---
title: "aaaCreate és hello Azure-portálon művelet csoportok kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és hello Azure-portálon művelet csoportok kezelése."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: ancav
ms.openlocfilehash: 97e0b22bea7787fff6856f895a7e6256c177efd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-action-groups-in-hello-azure-portal"></a>Az Azure-portálon hello művelet csoportok létrehozása és kezelése
## <a name="overview"></a>Áttekintés ##
Ez a cikk bemutatja, hogyan toocreate és hello Azure-portálon művelet csoportok kezelése.

A művelet csoportok konfigurálható azon műveletek listáját. Ezek a csoportok majd használható napló tevékenységriasztásokat definiálásakor. Ezek a csoportok majd minden egyes napló figyelmeztetés a meghatározásához, győződjön meg arról, hogy ugyanazon műveleteket hajtja végre hello figyelmeztetés a naplófájl minden egyes indításakor hello felhasználhatók.

Egy művelet típusonkénti too10 legfeljebb tartalmazhat. Egyes műveletek során a következő tulajdonságok hello tevődik össze:

* **Név**: hello művelet csoporton belül egyedi azonosítója.  
* **Művelet típusa**: SMS küldése, küldjön egy e-mailt, vagy hívja a webhook.  
* **Részletek**: hello a megfelelő telefonszám, e-mail címét vagy webhook URI.

Információ toouse Azure Resource Manager sablonok tooconfigure művelet csoportok: [művelet csoport Resource Manager-sablonok](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-hello-azure-portal"></a>Egy művelet csoport létrehozása hello Azure-portál használatával ##
1. A hello [portal](https://portal.azure.com), jelölje be **figyelő**. Hello **figyelő** panel összes figyelési beállítások és adatok az egyik nézetben összesíti.

    ![hello "Figyelés" szolgáltatás](./media/monitoring-action-groups/home-monitor.png)
2. A hello **tevékenységnapló** szakaszban jelölje be **művelet csoportok**.

    ![hello "Művelet csoportok" lap](./media/monitoring-action-groups/action-groups-blade.png)
3. Válassza ki **művelet csoport hozzáadása**, és töltse ki hello mezőket.

    ![hello "Művelet csoport hozzáadása" parancs](./media/monitoring-action-groups/add-action-group.png)
4. Adjon meg egy nevet a hello **művelet csoportnév** mezőbe, majd írjon be egy nevet a hello **rövid név** mezőbe. hello rövid nevét helyett egy teljes művelet felügyeleticsoport-nevet használja, ha ez a csoport értesítések küldése.

      ![hello a művelet csoport hozzáadása"párbeszédpanel](./media/monitoring-action-groups/action-group-define.png)

5. Hello **előfizetés** a jelenlegi előfizetés autofills mezőben. Ez az előfizetés egy hello mely hello művelet csoport menti.

6. Jelölje be hello **erőforráscsoport** mely hello művelet csoport-t menti.

7. Adja meg azon műveletek listáját, adja meg a minden egyes művelethez:

    a. **Név**: Adjon meg egy egyedi azonosítót ehhez a művelethez.

    b. **Művelet típusa**: válassza ki az SMS, e-mailek vagy webhook.

    c. **Részletek**: hello művelet típusa alapján, adjon meg egy telefonszámot, az e-mail címet, illetve a webhook URI.

8. Válassza ki **OK** toocreate hello művelet csoport.

## <a name="manage-your-action-groups"></a>A művelet csoportok kezelése ##
Egy művelet csoport létrehozása után is látható a hello **művelet csoportok** hello szakasza **figyelő** panelen. Válassza ki hello művelet csoportot, hogy toomanage:

* Adja hozzá, szerkeszthet és eltávolíthat műveletek.
* Hello művelet csoport törlése.

## <a name="next-steps"></a>Következő lépések ##
* További információ [SMS riasztási viselkedés](monitoring-sms-alert-behavior.md).  
* Szerezzen egy [hello műveletnapló riasztási webhook séma megismerni](monitoring-activity-log-alerts-webhook.md).  
* További információ [sebességkorlátozást](monitoring-alerts-rate-limiting.md) értesítésekről. 
* Lekérése egy [tevékenység napló riasztások áttekintése](monitoring-overview-alerts.md), és megtudhatja, hogyan tooreceive riasztásokat.  
* Ismerje meg, hogyan túl[riasztások konfigurálása, ha az állapotfigyelő szolgáltatáshoz értesítést visszaküldi](monitoring-activity-log-alerts-on-service-notifications.md).
