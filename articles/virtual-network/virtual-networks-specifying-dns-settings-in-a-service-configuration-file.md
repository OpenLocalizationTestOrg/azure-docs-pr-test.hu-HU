---
title: "DNS-beállítások megadása a szolgáltatás konfigurációs fájljában |} Microsoft Docs"
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
ms.openlocfilehash: 0fba2ea06827aff29a7a092933edb8120d668b29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a><span data-ttu-id="57dd9-103">DNS-beállítások megadása a szolgáltatás konfigurációs fájljában</span><span class="sxs-lookup"><span data-stu-id="57dd9-103">Specifying DNS Settings in a Service Configuration File</span></span>
## <a name="dns-elements"></a><span data-ttu-id="57dd9-104">DNS-elemek</span><span class="sxs-lookup"><span data-stu-id="57dd9-104">DNS elements</span></span>
<span data-ttu-id="57dd9-105">A szolgáltatás konfigurációs fájlja tartalmazhat a DnsServers elemnek a tartománynévrendszer (DNS) kiszolgálók, a szolgáltatás által használt IPv4-címek listáját.</span><span class="sxs-lookup"><span data-stu-id="57dd9-105">A service configuration file may contain a DnsServers element with a list of IPv4 addresses for the Domain Name System (DNS) servers that the service will use.</span></span> <span data-ttu-id="57dd9-106">A szolgáltatás konfigurációs fájljában érvényesülnek beállításokat a hálózati konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="57dd9-106">Settings in the service configuration file take precedence over settings in the network configuration file.</span></span> <span data-ttu-id="57dd9-107">További információkért lásd: [Azure szolgáltatás konfigurációs séma (.cscfg fájl)](https://msdn.microsoft.com/library/azure/ee758710.aspx).</span><span class="sxs-lookup"><span data-stu-id="57dd9-107">For more information, see [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx).</span></span>

<span data-ttu-id="57dd9-108">**NetworkConfiguration elemet**</span><span class="sxs-lookup"><span data-stu-id="57dd9-108">**NetworkConfiguration element**</span></span>

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> <span data-ttu-id="57dd9-109">A **neve** attribútumnak a **DNS-kiszolgáló** elem csak egy hivatkozásnév használatos.</span><span class="sxs-lookup"><span data-stu-id="57dd9-109">The **name** attribute in the **DnsServer** element is used only as a reference name.</span></span> <span data-ttu-id="57dd9-110">Mert nem felel meg a DNS-kiszolgáló állomásneve.</span><span class="sxs-lookup"><span data-stu-id="57dd9-110">It does not represent the host name for the DNS server.</span></span> <span data-ttu-id="57dd9-111">Minden egyes **DNS-kiszolgáló** attribútum értékének egyedinek kell lennie a teljes Microsoft Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="57dd9-111">Each **DnsServer** attribute value must be unique across the entire Microsoft Azure subscription.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="57dd9-112">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="57dd9-112">See Also</span></span>
[<span data-ttu-id="57dd9-113">Az Azure szolgáltatás konfigurációs séma (.cscfg)</span><span class="sxs-lookup"><span data-stu-id="57dd9-113">Azure Service Configuration Schema (.cscfg)</span></span>](https://msdn.microsoft.com/library/windowsazure/ee758710)

[<span data-ttu-id="57dd9-114">Azure-beli virtuális hálózat konfigurációs séma</span><span class="sxs-lookup"><span data-stu-id="57dd9-114">Azure Virtual Network Configuration Schema</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

[<span data-ttu-id="57dd9-115">Virtuális hálózat használatával a hálózati konfigurációs fájlok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="57dd9-115">Configure a Virtual Network Using Network Configuration Files</span></span>](http://go.microsoft.com/fwlink/?LinkId=248094)

[<span data-ttu-id="57dd9-116">A kezelési portál virtuális hálózati beállításaival kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="57dd9-116">About Virtual Network settings in the Management Portal</span></span>](http://go.microsoft.com/fwlink/?LinkId=248092)

