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
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a><span data-ttu-id="81716-103">DNS-beállítások megadása a virtuális hálózati konfigurációs fájlban</span><span class="sxs-lookup"><span data-stu-id="81716-103">Specifying DNS settings in a virtual network configuration file</span></span>
<span data-ttu-id="81716-104">A hálózati konfigurációs fájl használható toospecify tartománynévrendszer (DNS) beállítások két elemmel rendelkezik: **DnsServers** és **DnsServerRef**.</span><span class="sxs-lookup"><span data-stu-id="81716-104">A network configuration file has two elements that you can use toospecify Domain Name System (DNS) settings: **DnsServers** and **DnsServerRef**.</span></span> <span data-ttu-id="81716-105">Adja hozzá a DNS-kiszolgálók listáját az IP-címek megadásával, és hivatkozzon nevek toohello **DnsServers** elemet.</span><span class="sxs-lookup"><span data-stu-id="81716-105">You can add a list of DNS servers by specifying their IP addresses and reference names toohello **DnsServers** element.</span></span> <span data-ttu-id="81716-106">Ezután egy **DnsServerRef** elem toospecify mely DNS-kiszolgálóbejegyzéseik hello DnsServers elemből használják a virtuális hálózaton belüli különböző hálózati helyek.</span><span class="sxs-lookup"><span data-stu-id="81716-106">You can then use a **DnsServerRef** element toospecify which DNS server entries from hello DnsServers element are used for different network sites within your virtual network.</span></span>

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="81716-107">Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="81716-107">This article covers hello classic deployment model.</span></span>

<span data-ttu-id="81716-108">hello hálózati konfigurációs fájl a következő elemek hello tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="81716-108">hello network configuration file may contain hello following elements.</span></span> <span data-ttu-id="81716-109">hello minden elem neve csatolt tooa lap, amely hello elem további információkkal szolgál a érték beállításait.</span><span class="sxs-lookup"><span data-stu-id="81716-109">hello title of each element is linked tooa page that provides additional information about hello element value settings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="81716-110">További információ a hogyan tooconfigure hello hálózati konfigurációs fájl: [konfigurálja a virtuális hálózat használatával a hálózati konfigurációs fájl](virtual-networks-using-network-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="81716-110">For information about how tooconfigure hello network configuration file, see [Configure a Virtual Network Using a Network Configuration File](virtual-networks-using-network-configuration-file.md).</span></span> <span data-ttu-id="81716-111">További információ az egyes hello hálózati konfigurációs fájlban lévő elem: [Azure virtuális hálózat konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="81716-111">For information about each element contained in hello network configuration file, see [Azure Virtual Network Configuration Schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
> 
> 

[<span data-ttu-id="81716-112">DNS-elem</span><span class="sxs-lookup"><span data-stu-id="81716-112">Dns Element</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> <span data-ttu-id="81716-113">Hello **neve** hello attribútum **DNS-kiszolgáló** elem csak referenciaként használatos hello **DnsServerRef** elemet.</span><span class="sxs-lookup"><span data-stu-id="81716-113">hello **name** attribute in hello **DnsServer** element is used only as a reference for hello **DnsServerRef** element.</span></span> <span data-ttu-id="81716-114">Nem tartozik hello hello DNS-kiszolgáló állomásneve.</span><span class="sxs-lookup"><span data-stu-id="81716-114">It does not represent hello host name for hello DNS server.</span></span> <span data-ttu-id="81716-115">Minden egyes **DNS-kiszolgáló** attribútumérték belül egyedinek kell lennie hello teljes Microsoft Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="81716-115">Each **DnsServer** attribute value must be unique across hello entire Microsoft Azure subscription</span></span>
> 
> 

[<span data-ttu-id="81716-116">Virtuális hálózati helyek elem</span><span class="sxs-lookup"><span data-stu-id="81716-116">Virtual Network Sites Element</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> <span data-ttu-id="81716-117">A rendezés toospecify hello virtuális hálózati helyek elem ezt a beállítást, azt korábban definiálni kell a hello DNS elemben.</span><span class="sxs-lookup"><span data-stu-id="81716-117">In order toospecify this setting for hello Virtual Network Sites element, it must be previously defined in hello DNS element.</span></span> <span data-ttu-id="81716-118">hello DnsServerRef *neve* hello virtuális hálózati helyek elem alatt kell tooa név-érték hello DNS elem a DNS-kiszolgáló a megadott *neve*.</span><span class="sxs-lookup"><span data-stu-id="81716-118">hello DnsServerRef *name* in hello Virtual Network Sites element must refer tooa name value specified in hello DNS element for DnsServer *name*.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="81716-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="81716-119">Next steps</span></span>
* <span data-ttu-id="81716-120">Hello megértéséhez [Azure virtuális hálózat konfigurációs séma](http://go.microsoft.com/fwlink/?LinkId=248093).</span><span class="sxs-lookup"><span data-stu-id="81716-120">Understand hello [Azure Virtual Network Configuration Schema](http://go.microsoft.com/fwlink/?LinkId=248093).</span></span>
* <span data-ttu-id="81716-121">Hello megértéséhez [Azure szolgáltatás konfigurációs séma](https://msdn.microsoft.com/library/windowsazure/ee758710).</span><span class="sxs-lookup"><span data-stu-id="81716-121">Understand hello [Azure Service Configuration Schema](https://msdn.microsoft.com/library/windowsazure/ee758710).</span></span>
* <span data-ttu-id="81716-122">[Konfigurálja a hálózati konfigurációs fájlokat használó virtuális hálózatot](virtual-networks-using-network-configuration-file.md).</span><span class="sxs-lookup"><span data-stu-id="81716-122">[Configure a virtual network using Network configuration files](virtual-networks-using-network-configuration-file.md).</span></span>

