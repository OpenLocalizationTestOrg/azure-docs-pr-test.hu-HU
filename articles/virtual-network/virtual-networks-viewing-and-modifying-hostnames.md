---
title: "Megtekintése és módosítása az állomásnevek |} Microsoft Docs"
description: "Hogyan megtekintése és módosítása az Azure virtuális gépek állomásnevek, webes és feldolgozói szerepkörök névfeloldás"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.openlocfilehash: 9a3a1e1b58dcb828e2d2d09c18f1aab6d46051aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="viewing-and-modifying-hostnames"></a><span data-ttu-id="7febe-103">Megtekintése és módosítása az állomásnevek</span><span class="sxs-lookup"><span data-stu-id="7febe-103">Viewing and modifying hostnames</span></span>
<span data-ttu-id="7febe-104">Az állomásnév lehet hivatkozni a szerepkörpéldányok engedélyezéséhez meg kell adni az állomásnév értéke a konfigurációs fájlban az egyes szerepkörökhöz.</span><span class="sxs-lookup"><span data-stu-id="7febe-104">To allow your role instances to be referenced by host name, you must set the value for the host name in the service configuration file for each role.</span></span> <span data-ttu-id="7febe-105">A kívánt gazdagép nevét hozzáadása ehhez a **vmName** attribútuma a **szerepkör** elemet.</span><span class="sxs-lookup"><span data-stu-id="7febe-105">You do that by adding the desired host name to the **vmName** attribute of the **Role** element.</span></span> <span data-ttu-id="7febe-106">Értékét a **vmName** attribútum alapjaként van használatban a névhez, az összes szerepkör-példány.</span><span class="sxs-lookup"><span data-stu-id="7febe-106">The value of the **vmName** attribute is used as a base for the host name of each role instance.</span></span> <span data-ttu-id="7febe-107">Például ha **vmName** van *webrole típusról* , de az adott szerepkör három alkalmazáspéldányra, az állomásneveket példánya lesz *webrole0*, *webrole1*, és *webrole2*.</span><span class="sxs-lookup"><span data-stu-id="7febe-107">For example, if **vmName** is *webrole* and there are three instances of that role, the host names of the instances will be *webrole0*, *webrole1*, and *webrole2*.</span></span> <span data-ttu-id="7febe-108">Meg kell a virtuális gépek gazdagépnevet adjon meg a konfigurációs fájlban, mivel egy virtuális gép állomásnevét a virtuális gép neve alapján van feltöltve.</span><span class="sxs-lookup"><span data-stu-id="7febe-108">You do not need to specify a host name for virtual machines in the configuration file, because the host name for a virtual machine is populated based on the virtual machine name.</span></span> <span data-ttu-id="7febe-109">A Microsoft Azure-szolgáltatás konfigurálásával kapcsolatos további információkért lásd: [Azure szolgáltatás konfigurációs séma (.cscfg fájl)](https://msdn.microsoft.com/library/azure/ee758710.aspx)</span><span class="sxs-lookup"><span data-stu-id="7febe-109">For more information about configuring a Microsoft Azure service, see [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx)</span></span>

## <a name="viewing-hostnames"></a><span data-ttu-id="7febe-110">Állomásnevek megtekintése</span><span class="sxs-lookup"><span data-stu-id="7febe-110">Viewing hostnames</span></span>
<span data-ttu-id="7febe-111">Az alábbi eszközök egyikével szerepkörpéldányokat és a virtuális gépek állomás nevét a felhőszolgáltatás tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="7febe-111">You can view the host names of virtual machines and role instances in a cloud service by using any of the tools below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="7febe-112">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7febe-112">Azure Portal</span></span>
<span data-ttu-id="7febe-113">Használhatja a [Azure-portálon](http://portal.azure.com) a virtuális gépek – áttekintés panelen a virtuális gépek állomás nevének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="7febe-113">You can use the [Azure portal](http://portal.azure.com) to view the host names for virtual machines on the overview blade for a virtual machine.</span></span> <span data-ttu-id="7febe-114">Ne feledje, hogy a panel látható értéket **neve** és **állomásnév**.</span><span class="sxs-lookup"><span data-stu-id="7febe-114">Keep in mind that the blade shows a value for **Name** and **Host Name**.</span></span> <span data-ttu-id="7febe-115">Habár ezek kezdetben, az állomásnév módosítását nem módosítja a virtuális gép vagy szerepkörpéldány neve.</span><span class="sxs-lookup"><span data-stu-id="7febe-115">Although they are initially the same, changing the host name will not change the name of the virtual machine or role instance.</span></span>

<span data-ttu-id="7febe-116">Szerepkörpéldányokat is megtekinthetők az Azure portálon, de ha felsorolja a példány egy felhőalapú szolgáltatás, az állomás neve nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7febe-116">Role instances can also be viewed in the Azure portal, but when you list the instances in a cloud service, the host name is not displayed.</span></span> <span data-ttu-id="7febe-117">Látni fogja az összes olyan példány nevét, de ugyanez a neve nem felel meg a gazdagép nevét.</span><span class="sxs-lookup"><span data-stu-id="7febe-117">You will see a name for each instance, but that name does not represent the host name.</span></span>

### <a name="service-configuration-file"></a><span data-ttu-id="7febe-118">Szolgáltatás konfigurációs fájlja</span><span class="sxs-lookup"><span data-stu-id="7febe-118">Service configuration file</span></span>
<span data-ttu-id="7febe-119">A szolgáltatás konfigurációs fájlja egy telepített szolgáltatáshoz a letöltheti a **konfigurálása** panel az Azure-portálon a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7febe-119">You can download the service configuration file for a deployed service from the **Configure** blade of the service in the Azure portal.</span></span> <span data-ttu-id="7febe-120">Majd kereshet a **vmName** az attribútum a **szerepkörnév** elem gazdagép nevének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="7febe-120">You can then look for the **vmName** attribute for the **Role name** element to see the host name.</span></span> <span data-ttu-id="7febe-121">Ne feledje, hogy a állomásnevet alapjaként nem használja az összes szerepkör-példány az állomás neve.</span><span class="sxs-lookup"><span data-stu-id="7febe-121">Keep in mind that this host name is used as a base for the host name of each role instance.</span></span> <span data-ttu-id="7febe-122">Például ha **vmName** van *webrole típusról* , de az adott szerepkör három alkalmazáspéldányra, az állomásneveket példánya lesz *webrole0*, *webrole1*, és *webrole2*.</span><span class="sxs-lookup"><span data-stu-id="7febe-122">For example, if **vmName** is *webrole* and there are three instances of that role, the host names of the instances will be *webrole0*, *webrole1*, and *webrole2*.</span></span>

### <a name="remote-desktop"></a><span data-ttu-id="7febe-123">A távoli asztal</span><span class="sxs-lookup"><span data-stu-id="7febe-123">Remote Desktop</span></span>
<span data-ttu-id="7febe-124">Miután engedélyezte a távoli asztal (Windows), a Windows PowerShell távoli eljáráshívás (Windows) vagy a virtuális gépek vagy a szerepkörpéldányok (Linux és Windows rendszerekhez) SSH-kapcsolatok, megtekintheti az állomásnév aktív távoli asztali kapcsolat különböző módokon:</span><span class="sxs-lookup"><span data-stu-id="7febe-124">After you enable Remote Desktop (Windows), Windows PowerShell remoting (Windows), or SSH (Linux and Windows) connections to your virtual machines or role instances, you can view the host name from an active Remote Desktop connection in various ways:</span></span>

* <span data-ttu-id="7febe-125">Írja be a parancssort vagy a Terminálszolgáltatások SSH állomásnév.</span><span class="sxs-lookup"><span data-stu-id="7febe-125">Type hostname at the command prompt or SSH terminal.</span></span>
* <span data-ttu-id="7febe-126">Írja be az ipconfig/minden parancsot a parancssorba (csak Windows).</span><span class="sxs-lookup"><span data-stu-id="7febe-126">Type ipconfig /all at the command prompt (Windows only).</span></span>
* <span data-ttu-id="7febe-127">A számítógép nevének megtekintéséhez a rendszer beállításai (csak Windows).</span><span class="sxs-lookup"><span data-stu-id="7febe-127">View the computer name in the system settings (Windows only).</span></span>

### <a name="azure-service-management-rest-api"></a><span data-ttu-id="7febe-128">Azure szolgáltatásfelügyelet REST API-n</span><span class="sxs-lookup"><span data-stu-id="7febe-128">Azure Service Management REST API</span></span>
<span data-ttu-id="7febe-129">A többi ügyfél kövesse az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="7febe-129">From a REST client, follow these instructions:</span></span>

1. <span data-ttu-id="7febe-130">Győződjön meg arról, hogy rendelkezik-e csatlakozni az Azure-portálon ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="7febe-130">Ensure that you have a client certificate to connect to the Azure portal.</span></span> <span data-ttu-id="7febe-131">Szerezzen be ügyféltanúsítványt, kövesse a megjelenő lépéseket [hogyan: letöltése és közzététele beállításokat importálhatja adatait és előfizetési információit](https://msdn.microsoft.com/library/dn385850.aspx).</span><span class="sxs-lookup"><span data-stu-id="7febe-131">To obtain a client certificate, follow the steps presented in [How to: Download and Import Publish Settings and Subscription Information](https://msdn.microsoft.com/library/dn385850.aspx).</span></span> 
2. <span data-ttu-id="7febe-132">Állítsa be egy x-ms-version értéke az 2013-11-01 nevű fejlécében bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="7febe-132">Set a header entry named x-ms-version with a value of 2013-11-01.</span></span>
3. <span data-ttu-id="7febe-133">Kérés küldése a következő formátumban: https://management.core.windows.net/\<subscrition-azonosító\>/services/hostedservices/\<szolgáltatásnév\>? beágyazása detail = true</span><span class="sxs-lookup"><span data-stu-id="7febe-133">Send a request in the following format: https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<service-name\>?embed-detail=true</span></span>
4. <span data-ttu-id="7febe-134">Keresse meg a **állomásnév** minden elem **RoleInstance** elemet.</span><span class="sxs-lookup"><span data-stu-id="7febe-134">Look for the **HostName** element for each **RoleInstance** element.</span></span>

> [!WARNING]
> <span data-ttu-id="7febe-135">Megtekintheti a belső tartomány utótag a felhőalapú szolgáltatás, a REST-hívást válaszban szereplő ellenőrzésével a **InternalDnsSuffix** elem, vagy futtassa az ipconfig/minden a parancssorba a távoli asztali munkamenetben (Windows), vagy futtassa a macskaeledel /etc/resolv.conf SSH terminálról (Linux).</span><span class="sxs-lookup"><span data-stu-id="7febe-135">You can also view the internal domain suffix for your cloud service from the REST call response by checking the **InternalDnsSuffix** element, or by running ipconfig /all from a command prompt in a Remote Desktop session (Windows), or by running cat /etc/resolv.conf from an SSH terminal (Linux).</span></span>
> 
> 

## <a name="modifying-a-hostname"></a><span data-ttu-id="7febe-136">Az állomásnév módosítása</span><span class="sxs-lookup"><span data-stu-id="7febe-136">Modifying a hostname</span></span>
<span data-ttu-id="7febe-137">Módosíthatja a virtuális gép vagy szerepkörpéldány állomásneve egy módosított szolgáltatáskonfigurációs fájlt feltölteni, vagy a távoli asztali munkamenetből a számítógép átnevezése.</span><span class="sxs-lookup"><span data-stu-id="7febe-137">You can modify the host name for any virtual machine or role instance by uploading a modified service configuration file, or by renaming the computer from a Remote Desktop session.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7febe-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7febe-138">Next steps</span></span>
[<span data-ttu-id="7febe-139">Névfeloldás (DNS)</span><span class="sxs-lookup"><span data-stu-id="7febe-139">Name Resolution (DNS)</span></span>](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[<span data-ttu-id="7febe-140">Az Azure szolgáltatás konfigurációs séma (.cscfg)</span><span class="sxs-lookup"><span data-stu-id="7febe-140">Azure Service Configuration Schema (.cscfg)</span></span>](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[<span data-ttu-id="7febe-141">Azure-beli virtuális hálózat konfigurációs séma</span><span class="sxs-lookup"><span data-stu-id="7febe-141">Azure Virtual Network Configuration Schema</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

[<span data-ttu-id="7febe-142">Adja meg a hálózati konfigurációs fájlokat használja a DNS-beállítások</span><span class="sxs-lookup"><span data-stu-id="7febe-142">Specify DNS settings using network configuration files</span></span>](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

