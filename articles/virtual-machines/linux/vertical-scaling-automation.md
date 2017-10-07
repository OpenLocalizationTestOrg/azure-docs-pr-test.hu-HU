---
title: "aaaVertically scale Azure virtuális gép az Azure Automation szolgáltatásban |} Microsoft Docs"
description: "Hogyan toovertically méretezése válasz toomonitoring értesítések az Azure Automation szolgáltatásban, a Linux virtuális gépek"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: dcee199e-fa25-44d5-9b25-df564cee9b45
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/29/2016
ms.author: singhkay
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee4c1c33a588bd907d107f1828380a8afdaa725e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vertically-scale-azure-linux-virtual-machine-with-azure-automation"></a>Függőleges skálázása az Azure Linux virtuális gép az Azure Automation szolgáltatásban
Függőleges skálázás rendszer hello növelésével vagy csökkentésével hello erőforrások a gépek válasz toohello terheléseknél engedélyezett. Az Azure-ban ehhez hello virtuális gép méretét hello módosításával. Ez segítheti a következő forgatókönyvek hello

* Ha a virtuális gép hello gyakran nincs használatban, átméretezhető tooa kisebb méretű tooreduce le a havi költségeket
* Ha a virtuális gép hello a csúcsterhelés lát, is lehet átméretezett tooa nagyobb méretű tooincrease a kapacitás

hello vázlat hello lépéseket tooaccomplish ez van, mint az alábbi

1. Azure Automation tooaccess beállítása a virtuális gépek
2. Azure Automation függőleges méretezési runbookokat hello importálnia kell az előfizetéshez
3. A webhook tooyour runbook hozzáadása
4. Egy riasztás tooyour virtuális gép hozzáadása

> [!NOTE]
> Hello hello mérete miatt korlátozott lehet első virtuális gépen, akkor is méretezhető, hello mérete miatt hello toohello rendelkezésre állását más méretek hello fürt aktuális virtuális gép üzemel. Hello közzé azt ebben az esetben mi gondoskodunk és a virtuális gép mérete párok alatt hello belül csak méret cikkben használt automation-forgatókönyv. Ez azt jelenti, hogy egy Standard_D1v2 virtuális gép lesz nem hirtelen tooStandard_G5 kiterjesztett vagy méretezhető tooBasic_A0 le.
> 
> | Virtuálisgép-méretek pár skálázás |  |
> | --- | --- |
> | Basic_A0 |Basic_A4 |
> | Standard_A0 |Standard_A4 |
> | Standard_A5 |Standard_A7 |
> | Standard_A8 |Standard_A9 |
> | Standard_A10 |Standard_A11 |
> | Standard_D1 |Standard_D4 |
> | Standard_D11 |Standard_D14 |
> | Standard_DS1 |Standard_DS4 |
> | Standard_DS11 |Standard_DS14 |
> | Standard_D1v2 |Standard_D5v2 |
> | Standard_D11v2 |Standard_D14v2 |
> | Standard_G1 |Standard szintű, G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## <a name="setup-azure-automation-tooaccess-your-virtual-machines"></a>Azure Automation tooaccess beállítása a virtuális gépek
hello elsőként toodo szüksége, hozzon létre egy Azure Automation-fiók hello használt runbookok tooscale hello Virtuálisgép-méretezési csoport példányait futtató. Hello Automation szolgáltatás új hello "Futtatás mint fiók" funkció automatikusan futtatása hello runbookokat hello felhasználó nevében nagyon egyszerű szolgáltatásnevet hello beállítása így. További a hello a cikkben az alábbi:

* [Runbookok hitelesítése Azure-beli futtató fiókkal](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-hello-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Azure Automation függőleges méretezési runbookokat hello importálnia kell az előfizetéshez
Függőleges a virtuális gép már közzé hello Azure Automation-Runbook gyűjteménye méretezéshez szükséges runbookokat hello. Tooimport kell őket az előfizetéshez. Megtudhatja, hogyan olvasásával tooimport runbookokat hello következő cikket.

* [Az Azure Automation forgatókönyv- és minták](../../automation/automation-runbook-gallery.md)

importált toobe igénylő runbookokat hello hello az alábbi képen látható

![Runbookok importálása](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-tooyour-runbook"></a>A webhook tooyour runbook hozzáadása
Miután importált runbookokat hello szüksége lesz tooadd webhook toohello runbook úgy is elindítható a virtuális gép riasztások alapján. a webhook létrehozása a runbook hello részleteit itt olvasható

* [Azure Automation-webhook](../../automation/automation-webhooks.md)

Ellenőrizze, hogy hello webhook hello webhook párbeszédpanel bezárása, szüksége lesz a következő szakaszban hello előtt másolja át.

## <a name="add-an-alert-tooyour-virtual-machine"></a>Egy riasztás tooyour virtuális gép hozzáadása
1. Válassza ki a virtuális gép beállításait
2. Válassza ki a "Riasztási szabályok"
3. Válassza a "Riasztás hozzáadása"
4. Jelölje be a metrika toofire hello riasztások a
5. Válassza ki azt a feltételt, amely teljesítése lesz hello riasztási toofire okozhat
6. 5. lépésben válassza ki a hello feltétel küszöbértéket. toobe teljesítése
7. Lépéseket 5 és 6 hello feltétel és a küszöbérték felett mely hello szolgáltatás ellenőrzi időszak kiválasztása
8. Illessze be a hello előző szakaszából másolt hello webhook.

![Riasztási tooVirtual gép 1 hozzáadása](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Riasztási tooVirtual gép 2 hozzáadása](./media/vertical-scaling-automation/add-alert-webhook-2.png)

