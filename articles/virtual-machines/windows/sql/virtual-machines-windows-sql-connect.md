---
title: "SQL Server virtuális géphez (Resource Manager) aaaConnect tooa |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconnect tooSQL Server rendszert futtató Azure virtuális gépen. Ez a témakör hello klasszikus üzembe helyezési modellt használ. hello forgatókönyvek különböznek attól függően, hello hálózati konfigurációs és hello ügyfél hello helyét."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/14/2017
ms.author: jroth
ms.openlocfilehash: 7b127c14c37b9a72c19ed17f8b1dad61c7bc2d38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-sql-server-virtual-machine-on-azure-resource-manager"></a><span data-ttu-id="49c5a-105">Csatlakozás tooa SQL Server virtuális gépet az Azure (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="49c5a-105">Connect tooa SQL Server Virtual Machine on Azure (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="49c5a-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="49c5a-106">Resource Manager</span></span>](virtual-machines-windows-sql-connect.md)
> * [<span data-ttu-id="49c5a-107">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="49c5a-107">Classic</span></span>](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="49c5a-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="49c5a-108">Overview</span></span>

<span data-ttu-id="49c5a-109">Ez a témakör ismerteti, hogyan tooconnect tooyour SQL Server példányt, egy Azure virtuális gépen futó.</span><span class="sxs-lookup"><span data-stu-id="49c5a-109">This topic describes how tooconnect tooyour SQL Server instance running on an Azure virtual machine.</span></span> <span data-ttu-id="49c5a-110">Bemutatja, néhány [általános kapcsolati forgatókönyvek](#connection-scenarios) , majd [egy Azure virtuális gép az SQL Server-kapcsolat beállításának lépései részletes](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="49c5a-110">It covers some [general connectivity scenarios](#connection-scenarios) and then provides [detailed steps for configuring SQL Server connectivity in an Azure VM](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="49c5a-111">Ha inkább a teljes útmutató a kiépítést és a kapcsolat kellene lennie, lásd: [Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="49c5a-111">If you would rather have a full walk-through of both provisioning and connectivity, see [Provisioning a SQL Server Virtual Machine on Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

## <a name="connection-scenarios"></a><span data-ttu-id="49c5a-112">Kapcsolat-forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="49c5a-112">Connection scenarios</span></span>

<span data-ttu-id="49c5a-113">hello módon ügyfél csatlakozik egy virtuális gépen futó kiszolgáló eltér attól függően, hogy hello hely hello ügyfél és a hálózati konfiguráció hello tooSQL.</span><span class="sxs-lookup"><span data-stu-id="49c5a-113">hello way a client connects tooSQL Server running on a Virtual Machine differs depending on hello location of hello client and hello networking configuration.</span></span>

<span data-ttu-id="49c5a-114">Ha egy SQL Server virtuális gép hello Azure-portálon, lehetősége van hello hello típusú megadásával **SQL-kapcsolat**.</span><span class="sxs-lookup"><span data-stu-id="49c5a-114">If you provision a SQL Server VM in hello Azure portal, you have hello option of specifying hello type of **SQL connectivity**.</span></span>

![Nyilvános SQL kapcsolati lehetőséget kiépítése során](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

<span data-ttu-id="49c5a-116">A hálózati kapcsolatot a lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="49c5a-116">Your options for connectivity include:</span></span>

| <span data-ttu-id="49c5a-117">Beállítás</span><span class="sxs-lookup"><span data-stu-id="49c5a-117">Option</span></span> | <span data-ttu-id="49c5a-118">Leírás</span><span class="sxs-lookup"><span data-stu-id="49c5a-118">Description</span></span> |
|---|---|
| <span data-ttu-id="49c5a-119">**Nyilvános**</span><span class="sxs-lookup"><span data-stu-id="49c5a-119">**Public**</span></span> | <span data-ttu-id="49c5a-120">Csatlakozás tooSQL kiszolgálón keresztül hello internet</span><span class="sxs-lookup"><span data-stu-id="49c5a-120">Connect tooSQL Server over hello internet</span></span> |
| <span data-ttu-id="49c5a-121">**Magánfelhő**</span><span class="sxs-lookup"><span data-stu-id="49c5a-121">**Private**</span></span> | <span data-ttu-id="49c5a-122">Csatlakozzon a hello Server tooSQL azonos virtuális hálózatban</span><span class="sxs-lookup"><span data-stu-id="49c5a-122">Connect tooSQL Server in hello same virtual network</span></span> |
| <span data-ttu-id="49c5a-123">**Helyi**</span><span class="sxs-lookup"><span data-stu-id="49c5a-123">**Local**</span></span> | <span data-ttu-id="49c5a-124">Hello a helyi kiszolgáló tooSQL csatlakoztassa ugyanahhoz a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="49c5a-124">Connect tooSQL Server locally on hello same virtual machine</span></span> | 

<span data-ttu-id="49c5a-125">hello alábbi szakaszok ismertetik a hello **nyilvános** és **titkos** beállítások részletesebben.</span><span class="sxs-lookup"><span data-stu-id="49c5a-125">hello following sections explain hello **Public** and **Private** options in more detail.</span></span>

## <a name="connect-toosql-server-over-hello-internet"></a><span data-ttu-id="49c5a-126">Kiszolgáló tooSQL hello interneten keresztüli kapcsolódás</span><span class="sxs-lookup"><span data-stu-id="49c5a-126">Connect tooSQL Server over hello Internet</span></span>

<span data-ttu-id="49c5a-127">Ha tooconnect tooyour SQL Server adatbázismotor a hello Internet, jelölje be **nyilvános** a hello **SQL-kapcsolat** típus hello portálon kiépítése során.</span><span class="sxs-lookup"><span data-stu-id="49c5a-127">If you want tooconnect tooyour SQL Server database engine from hello Internet, select **Public** for hello **SQL connectivity** type in hello portal during provisioning.</span></span> <span data-ttu-id="49c5a-128">hello portál automatikusan hello lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="49c5a-128">hello portal automatically does hello following steps:</span></span>

* <span data-ttu-id="49c5a-129">Lehetővé teszi, hogy a TCP/IP-protokoll hello az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="49c5a-129">Enables hello TCP/IP protocol for SQL Server.</span></span>
* <span data-ttu-id="49c5a-130">Konfigurálja a tűzfal szabály tooopen hello SQL Server TCP port (alapértelmezés szerint 1433).</span><span class="sxs-lookup"><span data-stu-id="49c5a-130">Configures a firewall rule tooopen hello SQL Server TCP port (default 1433).</span></span>
* <span data-ttu-id="49c5a-131">Lehetővé teszi az SQL Server-hitelesítést, a nyilvános hozzáférés szükséges.</span><span class="sxs-lookup"><span data-stu-id="49c5a-131">Enables SQL Server Authentication, required for public access.</span></span>
* <span data-ttu-id="49c5a-132">VM tooall TCP-forgalmat az SQL Server port hello hello hello hálózati biztonsági csoport konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="49c5a-132">Configures hello network security group on hello VM tooall TCP traffic on hello SQL Server port.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="49c5a-133">az SQL Server Developer hello hello virtuálisgép-lemezképeket és Express kiadásait nem automatikusan hello TCP/IP protokoll engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="49c5a-133">hello virtual machine images for hello SQL Server Developer and Express editions do not automatically enable hello TCP/IP protocol.</span></span> <span data-ttu-id="49c5a-134">A fejlesztői és Express kiadásait kell használnia az SQL Server Configuration Manager túl[manuálisan engedélyezi a hello TCP/IP-protokoll](#manualtcp) hello létrehozása után a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="49c5a-134">For Developer and Express editions, you must use SQL Server Configuration Manager too[manually enable hello TCP/IP protocol](#manualtcp) after creating hello VM.</span></span>

<span data-ttu-id="49c5a-135">Bármely, internet-hozzáféréssel rendelkező ügyfél kapcsolódhatnak toohello SQL Server-példány nyilvános IP-cím hello hello virtuális gép vagy a bármely DNS-címke hozzárendelt toothat IP-cím megadásával.</span><span class="sxs-lookup"><span data-stu-id="49c5a-135">Any client with internet access can connect toohello SQL Server instance by specifying either hello public IP address of hello virtual machine or any DNS label assigned toothat IP address.</span></span> <span data-ttu-id="49c5a-136">Ha SQL Server port hello az 1433-as, nem kell toospecify azt hello kapcsolati karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="49c5a-136">If hello SQL Server port is 1433, you do not need toospecify it in hello connection string.</span></span> <span data-ttu-id="49c5a-137">a következő kapcsolati karakterlánc hello tooa SQL virtuális gép csatlakozik egy DNS-címke a `sqlvmlabel.eastus.cloudapp.azure.com` SQL-hitelesítéssel (is használhatja hello nyilvános IP-cím).</span><span class="sxs-lookup"><span data-stu-id="49c5a-137">hello following connection string connects tooa SQL VM with a DNS label of `sqlvmlabel.eastus.cloudapp.azure.com` using SQL Authentication (you could also use hello public IP address).</span></span>

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

<span data-ttu-id="49c5a-138">Bár ez lehetővé teszi a kapcsolatot az ügyfelek keresztül hello internet, ez nem feltétlenül jelenti azt, hogy bárki kapcsolódhatnak-e az SQL Server tooyour.</span><span class="sxs-lookup"><span data-stu-id="49c5a-138">Although this enables connectivity for clients over hello internet, this does not imply that anyone can connect tooyour SQL Server.</span></span> <span data-ttu-id="49c5a-139">Külső ügyfelek toohello helyes felhasználónévvel és jelszóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="49c5a-139">Outside clients have toohello correct username and password.</span></span> <span data-ttu-id="49c5a-140">A fokozott biztonság érdekében elkerülheti hello jól ismert 1433-as port.</span><span class="sxs-lookup"><span data-stu-id="49c5a-140">However, for additional security, you can avoid hello well-known port 1433.</span></span> <span data-ttu-id="49c5a-141">Például ha konfigurálta az SQL Server toolisten 1500 porton és a meghatározott megfelelő tűzfal- és hálózati biztonsági csoportszabályok, kapcsolódhat hozzáfűzésével hello port száma toohello kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="49c5a-141">For example, if you configured SQL Server toolisten on port 1500 and established proper firewall and network security group rules, you could connect by appending hello port number toohello Server name.</span></span> <span data-ttu-id="49c5a-142">hello alábbi példa megváltoztatja hello előzőre egy egyéni portszám hozzáadásával **1500**, toohello kiszolgáló nevét:</span><span class="sxs-lookup"><span data-stu-id="49c5a-142">hello following example alters hello previous one by adding a custom port number, **1500**, toohello server name:</span></span>

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> <span data-ttu-id="49c5a-143">Ha SQL Server virtuális gépen keresztül hello internet, minden kimenő adatok hello Azure datacenter nem tulajdonos toonormal [a kimenő adatátviteli díjszabás](https://azure.microsoft.com/pricing/details/data-transfers/).</span><span class="sxs-lookup"><span data-stu-id="49c5a-143">When you query SQL Server in a VM over hello internet, all outgoing data from hello Azure datacenter is subject toonormal [pricing on outbound data transfers](https://azure.microsoft.com/pricing/details/data-transfers/).</span></span>

## <a name="connect-toosql-server-within-a-virtual-network"></a><span data-ttu-id="49c5a-144">Csatlakozás a virtuális hálózaton belül Server tooSQL</span><span class="sxs-lookup"><span data-stu-id="49c5a-144">Connect tooSQL Server within a virtual network</span></span>

<span data-ttu-id="49c5a-145">Ha úgy dönt, **titkos** a hello **SQL-kapcsolat** típus az Azure-portálon hello konfigurálása hello beállításai megegyeznek a legtöbb túl**nyilvános**.</span><span class="sxs-lookup"><span data-stu-id="49c5a-145">When you choose **Private** for hello **SQL connectivity** type in hello portal, Azure configures most of hello settings identical too**Public**.</span></span> <span data-ttu-id="49c5a-146">hello egy különbség, hogy nincs hálózati biztonsági csoport szabály tooallow forgalom hello SQL-kiszolgáló portja (alapértelmezés szerint 1433) kívül van.</span><span class="sxs-lookup"><span data-stu-id="49c5a-146">hello one difference is that there is no network security group rule tooallow outside traffic on hello SQL Server port (default 1433).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="49c5a-147">az SQL Server Developer hello hello virtuálisgép-lemezképeket és Express kiadásait nem automatikusan hello TCP/IP protokoll engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="49c5a-147">hello virtual machine images for hello SQL Server Developer and Express editions do not automatically enable hello TCP/IP protocol.</span></span> <span data-ttu-id="49c5a-148">A fejlesztői és Express kiadásait kell használnia az SQL Server Configuration Manager túl[manuálisan engedélyezi a hello TCP/IP-protokoll](#manualtcp) hello létrehozása után a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="49c5a-148">For Developer and Express editions, you must use SQL Server Configuration Manager too[manually enable hello TCP/IP protocol](#manualtcp) after creating hello VM.</span></span>

<span data-ttu-id="49c5a-149">Magánhálózati kapcsolat gyakran használják a együtt [virtuális hálózati](../../../virtual-network/virtual-networks-overview.md), több olyan forgatókönyveket tesz lehetővé, amelyek.</span><span class="sxs-lookup"><span data-stu-id="49c5a-149">Private connectivity is often used in conjunction with [Virtual Network](../../../virtual-network/virtual-networks-overview.md), which enables several scenarios.</span></span> <span data-ttu-id="49c5a-150">Az azonos virtuális hálózatban, még akkor is, ha ezen virtuális gépek szerepelnek a különböző erőforráscsoportokra hello virtuális gépek is elérheti.</span><span class="sxs-lookup"><span data-stu-id="49c5a-150">You can connect VMs in hello same virtual network, even if those VMs exist in different resource groups.</span></span> <span data-ttu-id="49c5a-151">És egy [telephelyek közötti VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), létrehozhat egy hibrid architektúra, amely a virtuális gépek a helyszíni hálózatokkal és gépek.</span><span class="sxs-lookup"><span data-stu-id="49c5a-151">And with a [site-to-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), you can create a hybrid architecture that connects VMs with on-premises networks and machines.</span></span>

<span data-ttu-id="49c5a-152">Virtuális hálózatok az Azure virtuális gépek tooa tartomány is engedélyezheti, toojoin.</span><span class="sxs-lookup"><span data-stu-id="49c5a-152">Virtual networks also enable     you toojoin your Azure VMs tooa domain.</span></span> <span data-ttu-id="49c5a-153">Ez a hello csak úgy toouse Windows-hitelesítés tooSQL kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="49c5a-153">This is hello only way toouse Windows Authentication tooSQL Server.</span></span> <span data-ttu-id="49c5a-154">hello más kapcsolat esetekben szükséges SQL-hitelesítés a felhasználónevek és jelszavak.</span><span class="sxs-lookup"><span data-stu-id="49c5a-154">hello other connection scenarios require SQL Authentication with user names and passwords.</span></span>

<span data-ttu-id="49c5a-155">Feltételezve, hogy a virtuális hálózaton konfigurálta a DNS, hello SQL Server rendszerű virtuális számítógép neve hello kapcsolati karakterlánc megadásával kapcsolható tooyour SQL Server-példány.</span><span class="sxs-lookup"><span data-stu-id="49c5a-155">Assuming that you have configured DNS in your virtual network, you can connect tooyour SQL Server instance by specifying hello SQL Server VM computer name in hello connection string.</span></span> <span data-ttu-id="49c5a-156">a következő példa is hello feltételezi, hogy a Windows-hitelesítést is konfigurálva van, és hello felhasználó rendelkezik hozzáférési toohello SQL Server-példányt.</span><span class="sxs-lookup"><span data-stu-id="49c5a-156">hello following example also assumes that Windows Authentication has also been configured and that hello user has been granted access toohello SQL Server instance.</span></span>

```
Server=mysqlvm;Integrated Security=true
```

## <span data-ttu-id="49c5a-157"><a id="change"></a>SQL-kapcsolat beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="49c5a-157"><a id="change"></a> Change SQL connectivity settings</span></span>

<span data-ttu-id="49c5a-158">Hello kapcsolat beállításokat az SQL Server virtuális gépen a hello Azure-portálon módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="49c5a-158">You can change hello connectivity settings for your SQL Server virtual machine in hello Azure portal.</span></span>

1. <span data-ttu-id="49c5a-159">Hello Azure-portálon, válassza ki **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="49c5a-159">In hello Azure portal, select **Virtual Machines**.</span></span>

2. <span data-ttu-id="49c5a-160">Válassza ki az SQL Server rendszerű virtuális Géphez.</span><span class="sxs-lookup"><span data-stu-id="49c5a-160">Select your SQL Server VM.</span></span>

3. <span data-ttu-id="49c5a-161">A **beállítások**, kattintson a **SQL Server-konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="49c5a-161">Under **Settings**, click **SQL Server configuration**.</span></span>

4. <span data-ttu-id="49c5a-162">Változás hello **SQL kapcsolati szint** tooyour szükséges beállítást.</span><span class="sxs-lookup"><span data-stu-id="49c5a-162">Change hello **SQL connectivity level** tooyour required setting.</span></span> <span data-ttu-id="49c5a-163">Is használhat a terület toochange hello SQL Server portja vagy hello SQL-hitelesítés beállításai.</span><span class="sxs-lookup"><span data-stu-id="49c5a-163">You can optionally use this area toochange hello SQL Server port or hello SQL Authentication settings.</span></span>

   ![SQL-kapcsolat módosítása](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. <span data-ttu-id="49c5a-165">Várjon néhány percet a frissítés toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="49c5a-165">Wait several minutes for hello update toocomplete.</span></span>

   ![SQL virtuális gép frissítési értesítés](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <span data-ttu-id="49c5a-167"><a id="manualtcp"></a>Engedélyezze a TCP/IP protokollt a fejlesztői és az Express verziója</span><span class="sxs-lookup"><span data-stu-id="49c5a-167"><a id="manualtcp"></a> Enable TCP/IP for Developer and Express editions</span></span>

<span data-ttu-id="49c5a-168">SQL Server-kapcsolat beállításainak módosításakor Azure nem automatikusan hello TCP/IP protokoll engedélyezéséhez az SQL Server Developer és Express kiadásait.</span><span class="sxs-lookup"><span data-stu-id="49c5a-168">When changing SQL Server connectivity settings, Azure does not automatically enable hello TCP/IP protocol for SQL Server Developer and Express editions.</span></span> <span data-ttu-id="49c5a-169">az alábbi hello részben megtudhatja, hogyan toomanually engedélyezze a TCP/IP protokollt, hogy az IP-cím szerint távolról kapcsolódhatnak.</span><span class="sxs-lookup"><span data-stu-id="49c5a-169">hello steps below explain how toomanually enable TCP/IP so that you can connect remotely by IP address.</span></span>

<span data-ttu-id="49c5a-170">Csatlakoztasson toohello SQL Server-számítógépen a távoli asztalról.</span><span class="sxs-lookup"><span data-stu-id="49c5a-170">First, connect toohello SQL Server machine with remote desktop.</span></span>

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

<span data-ttu-id="49c5a-171">Következő lépésként engedélyezze a hello TCP/IP-protokoll **SQL Server Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="49c5a-171">Next, enable hello TCP/IP protocol with **SQL Server Configuration Manager**.</span></span>

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a><span data-ttu-id="49c5a-172">Csatlakozás SSMS segítségével</span><span class="sxs-lookup"><span data-stu-id="49c5a-172">Connect with SSMS</span></span>

<span data-ttu-id="49c5a-173">hello lépések megjelenítése toocreate egy nem kötelező DNS címke az Azure virtuális gép számára, és csatlakozzon az SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="49c5a-173">hello following steps show how toocreate an optional DNS Label for your Azure VM and then connect with SQL Server Management Studio (SSMS).</span></span>

[!INCLUDE [Connect tooSQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a><span data-ttu-id="49c5a-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49c5a-174">Next Steps</span></span>

<span data-ttu-id="49c5a-175">üzembe helyezési útmutató toosee kapcsolat lépések együtt: [Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="49c5a-175">toosee provisioning instructions along with these connectivity steps, see [Provisioning a SQL Server Virtual Machine on Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

<span data-ttu-id="49c5a-176">Egyéb témakörök kapcsolódó toorunning SQL Server Azure virtuális gépeken, a következő témakörben: [SQL Server Azure virtuális gépeken](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="49c5a-176">For other topics related toorunning SQL Server in Azure VMs, see [SQL Server on Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
