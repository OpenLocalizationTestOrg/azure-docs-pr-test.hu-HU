---
title: "DNS-beállítások megadása a virtuális hálózati konfigurációs fájlban |} Microsoft Docs"
description: "A klasszikus üzembe helyezési modellel virtuális hálózati konfigurációs fájl használatával egy virtuális hálózat DNS-kiszolgáló beállításainak módosítása"
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
ms.openlocfilehash: ec33268915a1888509834ce6a5b2bc782a12ce4a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a><span data-ttu-id="b8147-103">DNS-beállítások megadása a virtuális hálózati konfigurációs fájlban</span><span class="sxs-lookup"><span data-stu-id="b8147-103">Specifying DNS settings in a virtual network configuration file</span></span>
<span data-ttu-id="b8147-104">A hálózati konfigurációs fájl van két olyan elemet, a tartománynévrendszer (DNS) beállítások megadására használható: **DnsServers** és **DnsServerRef**.</span><span class="sxs-lookup"><span data-stu-id="b8147-104">A network configuration file has two elements that you can use to specify Domain Name System (DNS) settings: **DnsServers** and **DnsServerRef**.</span></span> <span data-ttu-id="b8147-105">Adja hozzá a DNS-kiszolgálók listáját az IP-címek megadásával, és hivatkozzon a neveket a **DnsServers** elemet.</span><span class="sxs-lookup"><span data-stu-id="b8147-105">You can add a list of DNS servers by specifying their IP addresses and reference names to the **DnsServers** element.</span></span> <span data-ttu-id="b8147-106">Ezután egy **DnsServerRef** elemet adja meg, melyik DNS-kiszolgálóbejegyzéseik az DnsServers elemből különböző hálózati helyek, virtuális hálózaton belül használják.</span><span class="sxs-lookup"><span data-stu-id="b8147-106">You can then use a **DnsServerRef** element to specify which DNS server entries from the DnsServers element are used for different network sites within your virtual network.</span></span>

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="b8147-107">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b8147-107">This article covers the classic deployment model.</span></span>

<span data-ttu-id="b8147-108">A hálózati konfigurációs fájlt a következő elemet tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="b8147-108">The network configuration file may contain the following elements.</span></span> <span data-ttu-id="b8147-109">A cím az egyes elemek csatolva van egy oldal, amely további információkat találhat az elem érték beállításait.</span><span class="sxs-lookup"><span data-stu-id="b8147-109">The title of each element is linked to a page that provides additional information about the element value settings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b8147-110">A hálózati konfigurációs fájl konfigurálásával kapcsolatos további információkért lásd: [konfigurálja a virtuális hálózat használatával a hálózati konfigurációs fájl](virtual-networks-using-network-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="b8147-110">For information about how to configure the network configuration file, see [Configure a Virtual Network Using a Network Configuration File](virtual-networks-using-network-configuration-file.md).</span></span> <span data-ttu-id="b8147-111">További információ az egyes elemei a hálózati konfigurációs fájlban: [Azure virtuális hálózat konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8147-111">For information about each element contained in the network configuration file, see [Azure Virtual Network Configuration Schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
> 
> 

[<span data-ttu-id="b8147-112">DNS-elem</span><span class="sxs-lookup"><span data-stu-id="b8147-112">Dns Element</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> <span data-ttu-id="b8147-113">A **neve** attribútumnak a **DNS-kiszolgáló** elem használata csak a hivatkozásként a **DnsServerRef** elemet.</span><span class="sxs-lookup"><span data-stu-id="b8147-113">The **name** attribute in the **DnsServer** element is used only as a reference for the **DnsServerRef** element.</span></span> <span data-ttu-id="b8147-114">Mert nem felel meg a DNS-kiszolgáló állomásneve.</span><span class="sxs-lookup"><span data-stu-id="b8147-114">It does not represent the host name for the DNS server.</span></span> <span data-ttu-id="b8147-115">Minden egyes **DNS-kiszolgáló** attribútumérték belül egyedinek kell lennie a teljes Microsoft Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="b8147-115">Each **DnsServer** attribute value must be unique across the entire Microsoft Azure subscription</span></span>
> 
> 

[<span data-ttu-id="b8147-116">Virtuális hálózati helyek elem</span><span class="sxs-lookup"><span data-stu-id="b8147-116">Virtual Network Sites Element</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> <span data-ttu-id="b8147-117">Ahhoz, hogy adja meg ezt a beállítást, a virtuális hálózati helyek elemhez, azt korábban definiálni kell a DNS-elemben.</span><span class="sxs-lookup"><span data-stu-id="b8147-117">In order to specify this setting for the Virtual Network Sites element, it must be previously defined in the DNS element.</span></span> <span data-ttu-id="b8147-118">A DnsServerRef *neve* a virtuális hálózati helyek elem hivatkozhatnak egy DNS-kiszolgáló a DNS-elemben megadott név-érték *neve*.</span><span class="sxs-lookup"><span data-stu-id="b8147-118">The DnsServerRef *name* in the Virtual Network Sites element must refer to a name value specified in the DNS element for DnsServer *name*.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="b8147-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b8147-119">Next steps</span></span>
* <span data-ttu-id="b8147-120">Megérteni a [Azure-beli virtuális hálózat konfigurációs séma](http://go.microsoft.com/fwlink/?LinkId=248093).</span><span class="sxs-lookup"><span data-stu-id="b8147-120">Understand the [Azure Virtual Network Configuration Schema](http://go.microsoft.com/fwlink/?LinkId=248093).</span></span>
* <span data-ttu-id="b8147-121">Megérteni a [Azure szolgáltatás konfigurációs séma](https://msdn.microsoft.com/library/windowsazure/ee758710).</span><span class="sxs-lookup"><span data-stu-id="b8147-121">Understand the [Azure Service Configuration Schema](https://msdn.microsoft.com/library/windowsazure/ee758710).</span></span>
* <span data-ttu-id="b8147-122">[Konfigurálja a hálózati konfigurációs fájlokat használó virtuális hálózatot](virtual-networks-using-network-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="b8147-122">[Configure a virtual network using Network configuration files](virtual-networks-using-network-configuration-file.md).</span></span>

