---
title: "aaaManage DNS-zónák az Azure DNS - Azure-portál |} Microsoft Docs"
description: "DNS-zónák hello Azure-portál használatával kezelheti. Ez a cikk ismerteti, hogyan tooupdate, törlése és a DNS-zóna létrehozása az Azure DNS szolgáltatásra"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: gwallace
ms.openlocfilehash: 0d8ce302bb7126dfe8077a6f3e33418e16fcea64
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-hello-azure-portal"></a>Hogyan toomanage DNS-zónák az hello Azure-portálon

> [!div class="op_single_selector"]
> * [Portál](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Ez a cikk bemutatja, hogyan toomanage a DNS-zónák hello Azure-portál használatával. A DNS-zónák hello platformfüggetlen segítségével is kezelheti [Azure CLI](dns-operations-dnszones-cli.md) vagy hello Azure [PowerShell](dns-operations-dnszones.md).

## <a name="create-a-dns-zone"></a>DNS-zóna létrehozása

1. Jelentkezzen be toohello Azure-portálon
2. Hello központ menüben kattintson, majd **új > Hálózat >** , majd **DNS-zóna** tooopen hello hozzon létre DNS-zóna panelen.

    ![DNS-zóna](./media/dns-operations-dnszones-portal/openzone650.png)

4. A hello **hozzon létre DNS-zóna** panelen adja meg a következő értékek hello, majd kattintson a **létrehozása**:


   | **Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Name (Név)**|contoso.com|hello hello DNS-zóna neve|
   |**Előfizetés**|[Az Ön előfizetése]|Válasszon egy előfizetés toocreate hello DNS-zónát.|
   |**Erőforráscsoport**|**Új létrehozása:** contosoDNSRG|Hozzon létre egy erőforráscsoportot. az erőforráscsoport neve hello kiválasztott hello előfizetésen belül egyedinek kell lennie. További tudnivalók az erőforráscsoportokról, olvassa el a hello toolearn [erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) a cikk áttekintése.|
   |**Hely**|USA nyugati régiója||

> [!NOTE]
> hello erőforráscsoport hello erőforráscsoport toohello helyre hivatkozik, és nincs hatással van a hello DNS-zónát. mindig "globális" Hello DNS-zóna helyét, és nem jelenik meg.

## <a name="list-dns-zones"></a>Lista DNS-zónák

Az Azure-portálon hello, keresse meg a túl**további szolgáltatások** > **hálózati** > **DNS-zónák**. Minden DNS-zóna, saját erőforrás, például-rekordhalmazok száma és a név kiszolgálók ebben a nézetben látható. hello oszlop **NÉVKISZOLGÁLÓK** nincs hello alapértelmezett nézetben tooadd azt kattintson **oszlopok**, jelölje be **névkiszolgálók** kattintson **végzett**.

![DNS-zónák listázása](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>A DNS-zóna törlése

Keresse meg a tooa DNS-zóna hello portálon. A hello **DNS-zóna** panelen kattintson a **zóna törlése**. Vannak, aki a toodelete hello DNS-zóna felszólító tooconfirm áll. A DNS-zónák törlésekor a hello hello zónában lévő összes rekordot is törlődnek.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan legyenek a DNS-zóna és a rekordok ellátogatva toowork [Ismerkedés az Azure DNS hello Azure-portál használatával](dns-getstarted-portal.md).
