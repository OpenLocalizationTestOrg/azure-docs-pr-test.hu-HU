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
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a><span data-ttu-id="0bc3c-103">DNS-beállítások megadása a szolgáltatás konfigurációs fájljában</span><span class="sxs-lookup"><span data-stu-id="0bc3c-103">Specifying DNS Settings in a Service Configuration File</span></span>
## <a name="dns-elements"></a><span data-ttu-id="0bc3c-104">DNS-elemek</span><span class="sxs-lookup"><span data-stu-id="0bc3c-104">DNS elements</span></span>
<span data-ttu-id="0bc3c-105">A szolgáltatás konfigurációs fájlja tartalmazhat a DnsServers elemnek hello tartománynévrendszer (DNS) kiszolgálók hello szolgáltatás által használt IPv4-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="0bc3c-105">A service configuration file may contain a DnsServers element with a list of IPv4 addresses for hello Domain Name System (DNS) servers that hello service will use.</span></span> <span data-ttu-id="0bc3c-106">Hello szolgáltatás konfigurációs fájljában érvényesülnek hello hálózati konfigurációs fájl beállításait.</span><span class="sxs-lookup"><span data-stu-id="0bc3c-106">Settings in hello service configuration file take precedence over settings in hello network configuration file.</span></span> <span data-ttu-id="0bc3c-107">További információkért lásd: [Azure szolgáltatás konfigurációs séma (.cscfg fájl)](https://msdn.microsoft.com/library/azure/ee758710.aspx).</span><span class="sxs-lookup"><span data-stu-id="0bc3c-107">For more information, see [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx).</span></span>

<span data-ttu-id="0bc3c-108">**NetworkConfiguration elemet**</span><span class="sxs-lookup"><span data-stu-id="0bc3c-108">**NetworkConfiguration element**</span></span>

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> <span data-ttu-id="0bc3c-109">Hello **neve** hello attribútum **DNS-kiszolgáló** elem csak egy hivatkozásnév használatos.</span><span class="sxs-lookup"><span data-stu-id="0bc3c-109">hello **name** attribute in hello **DnsServer** element is used only as a reference name.</span></span> <span data-ttu-id="0bc3c-110">Nem tartozik hello hello DNS-kiszolgáló állomásneve.</span><span class="sxs-lookup"><span data-stu-id="0bc3c-110">It does not represent hello host name for hello DNS server.</span></span> <span data-ttu-id="0bc3c-111">Minden egyes **DNS-kiszolgáló** attribútum értékének egyedinek kell lennie hello teljes Microsoft Azure-előfizetés között.</span><span class="sxs-lookup"><span data-stu-id="0bc3c-111">Each **DnsServer** attribute value must be unique across hello entire Microsoft Azure subscription.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="0bc3c-112">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0bc3c-112">See Also</span></span>
[<span data-ttu-id="0bc3c-113">Az Azure szolgáltatás konfigurációs séma (.cscfg)</span><span class="sxs-lookup"><span data-stu-id="0bc3c-113">Azure Service Configuration Schema (.cscfg)</span></span>](https://msdn.microsoft.com/library/windowsazure/ee758710)

[<span data-ttu-id="0bc3c-114">Azure-beli virtuális hálózat konfigurációs séma</span><span class="sxs-lookup"><span data-stu-id="0bc3c-114">Azure Virtual Network Configuration Schema</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

[<span data-ttu-id="0bc3c-115">Virtuális hálózat használatával a hálózati konfigurációs fájlok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0bc3c-115">Configure a Virtual Network Using Network Configuration Files</span></span>](http://go.microsoft.com/fwlink/?LinkId=248094)

[<span data-ttu-id="0bc3c-116">Virtuális hálózati beállításai a kezelési portál hello kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="0bc3c-116">About Virtual Network settings in hello Management Portal</span></span>](http://go.microsoft.com/fwlink/?LinkId=248092)

