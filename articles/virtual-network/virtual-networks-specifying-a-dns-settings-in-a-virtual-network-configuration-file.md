---
title: "DNS-beállításokat a virtuális hálózati konfigurációs fájlban aaaSpecifying |} Microsoft Docs"
description: "Hogyan toochange DNS-kiszolgáló beállításainak egy virtuális hálózat virtuális hálózati konfigurációval fájlt hello klasszikus telepítési modell"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a8905927-92ac-42b5-8c33-8e42c000692c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.openlocfilehash: d53a658773e1c930b5a28a701db0be9edd26565e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>DNS-beállítások megadása a virtuális hálózati konfigurációs fájlban
A hálózati konfigurációs fájl használható toospecify tartománynévrendszer (DNS) beállítások két elemmel rendelkezik: **DnsServers** és **DnsServerRef**. Adja hozzá a DNS-kiszolgálók listáját az IP-címek megadásával, és hivatkozzon nevek toohello **DnsServers** elemet. Ezután egy **DnsServerRef** elem toospecify mely DNS-kiszolgálóbejegyzéseik hello DnsServers elemből használják a virtuális hálózaton belüli különböző hálózati helyek.

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.

hello hálózati konfigurációs fájl a következő elemek hello tartalmazhat. hello minden elem neve csatolt tooa lap, amely hello elem további információkkal szolgál a érték beállításait.

> [!IMPORTANT]
> További információ a hogyan tooconfigure hello hálózati konfigurációs fájl: [konfigurálja a virtuális hálózat használatával a hálózati konfigurációs fájl](virtual-networks-using-network-configuration-file.md). További információ az egyes hello hálózati konfigurációs fájlban lévő elem: [Azure virtuális hálózat konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).
> 
> 

[DNS-elem](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> Hello **neve** hello attribútum **DNS-kiszolgáló** elem csak referenciaként használatos hello **DnsServerRef** elemet. Nem tartozik hello hello DNS-kiszolgáló állomásneve. Minden egyes **DNS-kiszolgáló** attribútumérték belül egyedinek kell lennie hello teljes Microsoft Azure-előfizetés
> 
> 

[Virtuális hálózati helyek elem](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> A rendezés toospecify hello virtuális hálózati helyek elem ezt a beállítást, azt korábban definiálni kell a hello DNS elemben. hello DnsServerRef *neve* hello virtuális hálózati helyek elem alatt kell tooa név-érték hello DNS elem a DNS-kiszolgáló a megadott *neve*.
> 
> 

## <a name="next-steps"></a>Következő lépések
* Hello megértéséhez [Azure virtuális hálózat konfigurációs séma](http://go.microsoft.com/fwlink/?LinkId=248093).
* Hello megértéséhez [Azure szolgáltatás konfigurációs séma](https://msdn.microsoft.com/library/windowsazure/ee758710).
* [Konfigurálja a hálózati konfigurációs fájlokat használó virtuális hálózatot](virtual-networks-using-network-configuration-file.md).

