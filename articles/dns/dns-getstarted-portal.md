---
title: "aaaGet lépések az Azure DNS hello Azure-portál használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate DNS zóna, illetve az Azure DNS-rekord. Ez egy részletes útmutató toocreate, és kezeli az első DNS-zóna és a rekord hello Azure-portál használatával."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 5cea01d840d794001cccac64defed8b329d948db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-hello-azure-portal"></a>Ismerkedés az Azure DNS hello Azure-portál használatával

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Ez a cikk bemutatja, hogyan hello lépéseket toocreate az első DNS-zóna és a rekord hello Azure-portál használatával. Hajtsa végre az alábbi lépéseket az Azure PowerShell vagy platformok közötti Azure CLI hello is.

A DNS-zónák használt toohost hello DNS-rekordok az adott tartományban. az Azure DNS, a tartomány toostart toocreate DNS-zóna van szüksége a tartománynevet. Ezután a tartománya összes DNS-rekordja ebben a DNS-zónában jön létre. Végezetül toopublish a DNS-zónák toohello Internet, tooconfigure hello névkiszolgálók hello tartomány van szüksége. A lépések leírását a következő lépéseket hello.

## <a name="create-a-dns-zone"></a>DNS-zóna létrehozása

1. Jelentkezzen be toohello Azure-portálon
2. Hello központ menüben kattintson, majd **új > Hálózat >** , majd **DNS-zóna** tooopen hello hozzon létre DNS-zóna panelen.

    ![DNS-zóna](./media/dns-getstarted-portal/openzone650.png)

4. A hello **hozzon létre DNS-zóna** panelen adja meg a következő értékek hello, majd kattintson a **létrehozása**:


   | **Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Name (Név)**|contoso.com|hello hello DNS-zóna neve|
   |**Előfizetés**|[Az Ön előfizetése]|Válasszon egy előfizetés toocreate hello DNS-zónát.|
   |**Erőforráscsoport**|**Új létrehozása:** contosoDNSRG|Hozzon létre egy erőforráscsoportot. az erőforráscsoport neve hello kiválasztott hello előfizetésen belül egyedinek kell lennie. További tudnivalók az erőforráscsoportokról, olvassa el a hello toolearn [erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) a cikk áttekintése.|
   |**Hely**|USA nyugati régiója||

> [!NOTE]
> hello erőforráscsoport hello erőforráscsoport toohello helyre hivatkozik, és nincs hatással van a hello DNS-zónát. mindig "globális" Hello DNS-zóna helyét, és nem jelenik meg.

## <a name="create-a-dns-record"></a>DNS-rekord létrehozása

hello alábbi példa bemutatja, hogyan hello folyamatot, amely új "A" rekord létrehozása. Más rekordtípusokhoz és toomodify meglévő rekordokat: [kezelése DNS-rekordok és a rekordhalmazok hello Azure-portálon](dns-operations-recordsets-portal.md). 

1. Hello DNS-zóna jönnek létre, az Azure-portálon hello **Kedvencek** ablaktáblán kattintson a **összes erőforrás**. Kattintson a hello **contoso.com** hello összes erőforrás panel DNS-zónák. Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja **contoso.com** a hello **Szűrés név alapján...** tooeasily hozzáférés hello DNS-zóna mezőben.

1. Hello hello tetején **DNS-zóna** panelen válassza **+ rekordhalmaz** tooopen hello **adja hozzá a rekordhalmaz** panelen.

1. A hello **adja hozzá a rekordhalmaz** panelen adja meg a következő értékek hello, és kattintson a **OK**. Ebben a példában egy A rekordot hozhat létre.

   |**Beállítás** | **Érték** | **Részletek** |
   |---|---|---|
   |**Name (Név)**|www|Hello bejegyzés neve|
   |**Típus**|A| Típusú DNS-rekord toocreate, az elfogadható értékek a következők A, AAAA, CNAME, MX, NS, SRV, TXT és PTR.  A rekordok típusaival kapcsolatos további információért tekintse meg a [DNS-zónákat és -rekordokat áttekintő](dns-zones-records.md) cikket.|
   |**TTL**|1|Idő TTL hello DNS-kérelem.|
   |**TTL mértékegysége**|Óra|A TTL értékének időmértékegysége.|
   |**IP-cím**|ipAddressValue| Ezt az értéket, amely hello DNS-rekord hello IP-cím.|

## <a name="view-records"></a>A rekordok megtekintése

Hello alsó részén hello DNS-zóna panelen lásd: hello rekordok hello DNS-zónához. Hello alapértelmezett DNS- és SOA típusú rekordok, minden zóna jöttek létre, és a létrehozott új rekordok kell megjelennie.

![zóna](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>A névkiszolgálók frissítése

Ha mindent megfelelőnek talált, hogy a DNS-zóna és a rekordok beállított megfelelően, tooconfigure van szüksége a tartománynév toouse hello Azure DNS névkiszolgálóit. Ez lehetővé teszi a más felhasználóktól a hello Internet toofind a DNS-rekordokat.

a zóna névkiszolgálóit hello hello Azure-portálon szerepelnek:

![zóna](./media/dns-getstarted-portal/viewzonens500.png)

Ezeket a kiszolgálókat (vásárolta hello tartománynevet) hello regisztrációs kell konfigurálni. A regisztráló hello beállítás tooset hello neve kiszolgáló hello tartomány kínál. További információkért lásd: [delegálása a tartományi tooAzure DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Az összes erőforrás törlése

toodelete összes erőforrás létrehozása ebben a cikkben teljes hello a következő lépéseket:

1. Az Azure-portálon hello **Kedvencek** ablaktáblán kattintson a **összes erőforrás**. Kattintson a hello **MyResourceGroup** erőforráscsoport hello az összes erőforrás panel. Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja **MyResourceGroup** a hello **Szűrés név alapján...** mezőbe tooeasily hozzáférés hello erőforráscsoportot.
1. A hello **MyResourceGroup** paneljén kattintson hello **törlése** gombra.
1. hello portal meg tootype hello nevét, amelyet az toodelete hello erőforrás csoport tooconfirm azt. Kattintson a **törlése**, típus *MyResourceGroup* hello erőforráscsoport-nevet, majd kattintson **törlése**. Erőforráscsoport törlésével törli az összes erőforrás hello erőforráscsoporton belül, ezért mindig meg arról, hogy tooconfirm erőforráscsoport hello tartalmának törlése előtt. hello portal hello erőforráscsoporton belül található összes erőforrást törli, majd törli a hello erőforráscsoport magát. Ez a folyamat több percig is eltarthat.


## <a name="next-steps"></a>Következő lépések

További információ az Azure DNS-beli toolearn lásd [Azure DNS áttekintése](dns-overview.md).

További információ az Azure DNS-, DNS-rekordok kezelése toolearn lásd [kezelése DNS-rekordok és a rekordhalmazok hello Azure-portálon](dns-operations-recordsets-portal.md).

