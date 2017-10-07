---
title: "az ügyfél virtuális hálózat az Azure RemoteApp telepített portok és URL-címek toowhitelist aaaList |} Microsoft Docs"
description: "Ismerje meg, melyik portokra és URL-címek tooconfigure az Azure RemoteApp-kommunikációra lesz szüksége."
services: remoteapp
documentationcenter: 
author: mghosh1616
manager: mbaldwin
ms.assetid: 5a001ff7-14c9-47fa-9b39-78fd5a5f0250
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 039866f7b64ac763ca833d66031ade3def1d3543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="list-of-ports-and-urls-toopermit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Lista toopermit hozzáférési portok és URL-címek az Azure RemoteApp telepített ügyfelek virtuális hálózati
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Központi telepítése egy virtuális hálózatot (VNET) az Azure RemoteApp felhőalapú vagy hibrid gyűjteményt, tekintse át a következő portinformációkat hello. További információk a virtuális hálózatokon, [virtuális hálózat áttekintése](../virtual-network/virtual-networks-overview.md). Ha létrehozott egy hálózati biztonsági csoport (NSG) korlátozása a forgalom toohello virtuális hálózati erőforrásokhoz a gyűjteményben, ellenőrizze, a következő portok hello elérhető és engedélyezett hello biztonsági házirendek hello virtuális hálózaton keresztül. Hálózati biztonsági csoportokkal kapcsolatos további információkért olvassa el a [Mi az a hálózati biztonsági csoport? (NSG) ](../virtual-network/virtual-networks-nsg.md).

## <a name="azure-remoteapp-subnet-needs-access-toothese-endpoints-and-urls"></a>Azure RemoteApp-alhálózatban kell hozzáférést toothese végpontok és URL-címek:
* *. servicebus.windows.net
* *. servicebus.net
* https://*.RemoteApp.windowsazure.com  
* https://www.RemoteApp.windowsazure.com 
* https://*RemoteApp.windowsazure.com  
* https://*.Core.Windows.NET  
* Kimenő: TCP: TCP: 443, 9351, 9352, 10101-10175 
* Nem kötelező – UDP: 10201-10275  

## <a name="azure-remoteapp-clients-need-access-toothese-endpoints-and-urls"></a>Az Azure RemoteApp-ügyfelek toothese végpontok és URL-címeket kell elérni:
I jelenti stb hello asztali számítógépek, eszközök, amelyet az ügyfelek által használható tooconnect toohello alkalmazások üzembe helyezése a hello Azure RemoteApp-gyűjtemény.

* https://telemetry.RemoteApp.windowsazure.com  
* https://*.RemoteApp.windowsazure.com (hello választható UDP-portok vannak ehhez a címhez) 
* https://login.windows.net  
* https://login.microsoftonline.com  
* https://www.RemoteApp.windowsazure.com 
* https://*.Core.Windows.NET  
* Kimenő: TCP: 443  
* Választható - UDP: 3391 

