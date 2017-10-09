---
title: "DNS-beállításokat a szolgáltatás konfigurációs fájljában aaaSpecifying |} Microsoft Docs"
description: "a virtuális hálózati szolgáltatás konfigurációs fájl segítségével egyéni DNS-beállítások megadása"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 467a4b99-8691-40b3-b640-e25e49675c71
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: jdial
ms.openlocfilehash: f192e33566dd8e669da04e6378a0c8e4b0b35ecc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>DNS-beállítások megadása a szolgáltatás konfigurációs fájljában
## <a name="dns-elements"></a>DNS-elemek
A szolgáltatás konfigurációs fájlja tartalmazhat a DnsServers elemnek hello tartománynévrendszer (DNS) kiszolgálók hello szolgáltatás által használt IPv4-címek listáját. Hello szolgáltatás konfigurációs fájljában érvényesülnek hello hálózati konfigurációs fájl beállításait. További információkért lásd: [Azure szolgáltatás konfigurációs séma (.cscfg fájl)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration elemet**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> Hello **neve** hello attribútum **DNS-kiszolgáló** elem csak egy hivatkozásnév használatos. Nem tartozik hello hello DNS-kiszolgáló állomásneve. Minden egyes **DNS-kiszolgáló** attribútum értékének egyedinek kell lennie hello teljes Microsoft Azure-előfizetés között.
> 
> 

## <a name="see-also"></a>Lásd még:
[Az Azure szolgáltatás konfigurációs séma (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure-beli virtuális hálózat konfigurációs séma](http://go.microsoft.com/fwlink/?LinkId=248093)

[Virtuális hálózat használatával a hálózati konfigurációs fájlok konfigurálása](http://go.microsoft.com/fwlink/?LinkId=248094)

[Virtuális hálózati beállításai a kezelési portál hello kapcsolatos](http://go.microsoft.com/fwlink/?LinkId=248092)

