---
title: "A LOB-alkalmazás tesztkörnyezetben |} Microsoft Docs"
description: "Útmutató a web-alapú, üzletági alkalmazás létrehozása a hibrid felhő környezetben informatikai pro vagy fejlesztői teszteléshez."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 92d2d8ce-60ed-4512-95e5-a7fe3b0ca00b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.openlocfilehash: 8e9a380458bfede80ed0fc1ee7e62b0caec09501
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a><span data-ttu-id="0cc83-103">Webes üzletági alkalmazás telepítése hibrid felhőben tesztelés céljára</span><span class="sxs-lookup"><span data-stu-id="0cc83-103">Set up a web-based LOB application in a hybrid cloud for testing</span></span>
<span data-ttu-id="0cc83-104">Ez a témakör végigvezeti egy üzletági (LOB) alkalmazások a Microsoft Azure-ban üzemeltetett webalapú üzletági tesztelési szimulált hibrid felhőkörnyezetekben létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0cc83-104">This topic steps you through creating a simulated hybrid cloud environment for testing a web-based line of business (LOB) application hosted in Microsoft Azure.</span></span> <span data-ttu-id="0cc83-105">Ez a létrejövő konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="0cc83-105">Here is the resulting configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

<span data-ttu-id="0cc83-106">Ez a konfiguráció a következőkből áll:</span><span class="sxs-lookup"><span data-stu-id="0cc83-106">This configuration consists of:</span></span>

* <span data-ttu-id="0cc83-107">(A TestLab VNet) Azure-ban üzemeltetett szimulált a helyszíni hálózat.</span><span class="sxs-lookup"><span data-stu-id="0cc83-107">A simulated on-premises network hosted in Azure (the TestLab VNet).</span></span>
* <span data-ttu-id="0cc83-108">A létesítmények közötti virtuális hálózat (TestVNET) Azure-ban üzemeltetett.</span><span class="sxs-lookup"><span data-stu-id="0cc83-108">A cross-premises virtual network hosted in Azure (TestVNET).</span></span>
* <span data-ttu-id="0cc83-109">Egy VNet – VNet VPN-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="0cc83-109">A VNet-to-VNet VPN connection.</span></span>
* <span data-ttu-id="0cc83-110">Egy webes LOB-kiszolgáló, a SQL server és a másodlagos tartományvezérlő TestVNET virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="0cc83-110">A web-based LOB server, SQL server, and secondary domain controller in the TestVNET virtual network.</span></span>

<span data-ttu-id="0cc83-111">Ez a konfiguráció alapján és közös kiindulási pont, amelyről is biztosít:</span><span class="sxs-lookup"><span data-stu-id="0cc83-111">This configuration provides a basis and common starting point from which you can:</span></span>

* <span data-ttu-id="0cc83-112">Alkalmazások fejlesztéséhez és teszteléséhez LOB futó Internet Information Services (IIS) egy SQL Server 2014 adatbázis-fájlok az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="0cc83-112">Develop and test LOB applications hosted on Internet Information Services (IIS) with a SQL Server 2014 database backend in Azure.</span></span>
* <span data-ttu-id="0cc83-113">A szimulált hibrid felhőalapú informatikai munkaterhelés tesztek végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="0cc83-113">Perform testing of this simulated hybrid cloud-based IT workload.</span></span>

<span data-ttu-id="0cc83-114">A hibrid felhő tesztkörnyezet beállítása három fő fázisból áll:</span><span class="sxs-lookup"><span data-stu-id="0cc83-114">There are three major phases to setting up this hybrid cloud test environment:</span></span>

1. <span data-ttu-id="0cc83-115">Állítsa be a szimulált hibrid felhőalapú környezetben.</span><span class="sxs-lookup"><span data-stu-id="0cc83-115">Set up the simulated hybrid cloud environment.</span></span>
2. <span data-ttu-id="0cc83-116">Konfigurálja az SQL-kiszolgálószámítógépet (SQL1).</span><span class="sxs-lookup"><span data-stu-id="0cc83-116">Configure the SQL server computer (SQL1).</span></span>
3. <span data-ttu-id="0cc83-117">A LOB-kiszolgáló (LOB1) konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0cc83-117">Configure the LOB server (LOB1).</span></span>

<span data-ttu-id="0cc83-118">A munkaterhelés Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="0cc83-118">This workload requires an Azure subscription.</span></span> <span data-ttu-id="0cc83-119">Ha MSDN vagy a Visual Studio-előfizetéssel rendelkezik, tekintse meg [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="0cc83-119">If you have an MSDN or Visual Studio subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

<span data-ttu-id="0cc83-120">Például egy termelési LOB-alkalmazás az Azure-ban, a **üzletági alkalmazások** architektúra tervezetének: [Microsoft szoftverek architektúra diagramok és tervrajzokat](http://msdn.microsoft.com/dn630664).</span><span class="sxs-lookup"><span data-stu-id="0cc83-120">For an example of a production LOB application hosted in Azure, see the **Line of business applications** architecture blueprint at [Microsoft Software Architecture Diagrams and Blueprints](http://msdn.microsoft.com/dn630664).</span></span>

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a><span data-ttu-id="0cc83-121">1. fázis: A szimulált hibrid felhő környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="0cc83-121">Phase 1: Set up the simulated hybrid cloud environment</span></span>
<span data-ttu-id="0cc83-122">Hozzon létre a [szimulált hibrid felhő tesztkörnyezet](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0cc83-122">Create the [simulated hybrid cloud test environment](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="0cc83-123">Mivel ebben a tesztkörnyezetben nem igényel az APP1 kiszolgálót a Corpnet alhálózathoz jelenlétét, leállíthat azt a lépést.</span><span class="sxs-lookup"><span data-stu-id="0cc83-123">Because this test environment does not require the presence of the APP1 server on the Corpnet subnet, you can shut it down for now.</span></span>

<span data-ttu-id="0cc83-124">Ez az az aktuális konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="0cc83-124">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-the-sql-server-computer-sql1"></a><span data-ttu-id="0cc83-125">2. fázis: Az SQL-kiszolgálószámítógépet (SQL1) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0cc83-125">Phase 2: Configure the SQL server computer (SQL1)</span></span>
<span data-ttu-id="0cc83-126">Azure-portálról indítsa el a DC2 számítógépen, szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="0cc83-126">From the Azure portal, start the DC2 computer if needed.</span></span>

<span data-ttu-id="0cc83-127">Ezután hozzon létre egy virtuális gépet az sql1 számítógép a következő parancsokkal Azure PowerShell parancsot egy parancssorba a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0cc83-127">Next, create a virtual machine for SQL1 with these commands at an Azure PowerShell command prompt on your local computer.</span></span> <span data-ttu-id="0cc83-128">Ezek a parancsok futtatása előtt adja meg a változó értéke, és távolítsa el a < és > karakter.</span><span class="sxs-lookup"><span data-stu-id="0cc83-128">Prior to running these commands, fill in the variable values and remove the < and > characters.</span></span>

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty

    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="0cc83-129">Az Azure portál segítségével csatlakoztassa az sql1 számítógépen az sql1 számítógépen a helyi rendszergazda fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="0cc83-129">Use the Azure portal to connect to SQL1 using the local administrator account of SQL1.</span></span>

<span data-ttu-id="0cc83-130">Ezután konfigurálja a hálózati kapcsolat tesztelése és az SQL Server-forgalom engedélyezése a Windows tűzfal-szabályokat.</span><span class="sxs-lookup"><span data-stu-id="0cc83-130">Next, configure Windows Firewall rules to allow basic connectivity testing and SQL Server traffic.</span></span> <span data-ttu-id="0cc83-131">Egy rendszergazda szintű Windows PowerShell parancssorból az sql1 számítógépen futtassa az alábbi parancsokat.</span><span class="sxs-lookup"><span data-stu-id="0cc83-131">From an administrator-level Windows PowerShell command prompt on SQL1, run these commands.</span></span>

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

<span data-ttu-id="0cc83-132">A ping parancs négy sikeres választ 192.168.0.4 IP-címről eredményez.</span><span class="sxs-lookup"><span data-stu-id="0cc83-132">The ping command should result in four successful replies from IP address 192.168.0.4.</span></span>

<span data-ttu-id="0cc83-133">Ezután a lemez hozzáadása a további adatokat az sql1 számítógépen a meghajtó betűjelével F: új kötetként.</span><span class="sxs-lookup"><span data-stu-id="0cc83-133">Next, add the extra data disk on SQL1 as a new volume with the drive letter F:.</span></span>

1. <span data-ttu-id="0cc83-134">Kattintson a bal oldali ablaktáblában a Server Manager **fájl- és tárolási szolgáltatások**, és kattintson a **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-134">In the left pane of Server Manager, click **File and Storage Services**, and then click **Disks**.</span></span>
2. <span data-ttu-id="0cc83-135">A Tartalom ablaktáblán a a **lemezek** csoportjában kattintson a **2 lemez** (az a **partíció** beállítása **ismeretlen**).</span><span class="sxs-lookup"><span data-stu-id="0cc83-135">In the contents pane, in the **Disks** group, click **disk 2** (with the **Partition** set to **Unknown**).</span></span>
3. <span data-ttu-id="0cc83-136">Kattintson a **feladatok**, és kattintson a **új kötet**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-136">Click **Tasks**, and then click **New Volume**.</span></span>
4. <span data-ttu-id="0cc83-137">Az alapismeretek lapon az új kötet varázsló, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-137">On the Before you begin page of the New Volume Wizard, click **Next**.</span></span>
5. <span data-ttu-id="0cc83-138">A Select a kiszolgáló és lemez lap, kattintson a **Disk 2**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-138">On the Select the server and disk page, click **Disk 2**, and then click **Next**.</span></span> <span data-ttu-id="0cc83-139">Amikor a rendszer kéri, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-139">When prompted, click **OK**.</span></span>
6. <span data-ttu-id="0cc83-140">Kattintson a adja meg a kötet oldal méretét **következő**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-140">On the Specify the size of the volume page, click **Next**.</span></span>
7. <span data-ttu-id="0cc83-141">A hozzárendelés meghajtóbetűjel vagy mappa lapra, kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-141">On the Assign to a drive letter or folder page, click **Next**.</span></span>
8. <span data-ttu-id="0cc83-142">A fájl kijelölése rendszer beállítások lapján kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-142">On the Select file system settings page, click **Next**.</span></span>
9. <span data-ttu-id="0cc83-143">Beállítások oldalon, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-143">On the Confirm selections page, click **Create**.</span></span>
10. <span data-ttu-id="0cc83-144">Amikor végzett, kattintson **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-144">When complete, click **Close**.</span></span>

<span data-ttu-id="0cc83-145">A parancsok a Windows PowerShell parancssorába az sql1 számítógépen:</span><span class="sxs-lookup"><span data-stu-id="0cc83-145">Run these commands at the Windows PowerShell command prompt on SQL1:</span></span>

    md f:\Data
    md f:\Log
    md f:\Backup

<span data-ttu-id="0cc83-146">A következő sql1 számítógép csatlakoztatása a CORP Windows Server Active Directory-tartomány a következő parancsokkal az sql1 számítógépen a Windows PowerShell parancssorába.</span><span class="sxs-lookup"><span data-stu-id="0cc83-146">Next, join SQL1 to the CORP Windows Server Active Directory domain with these commands at the Windows PowerShell prompt on SQL1.</span></span>

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

<span data-ttu-id="0cc83-147">Tartományi fiók hitelesítő adatait használjuk a corp\felhasználó1 fiókkal, amikor a rendszer kéri a **Add-Computer** parancsot.</span><span class="sxs-lookup"><span data-stu-id="0cc83-147">Use the CORP\User1 account when prompted to supply domain account credentials for the **Add-Computer** command.</span></span>

<span data-ttu-id="0cc83-148">Az újraindítás után az Azure-portálon való csatlakozáskor használandó SQL1 *az sql1 számítógépen a helyi rendszergazdai fiókkal*.</span><span class="sxs-lookup"><span data-stu-id="0cc83-148">After restarting, use the Azure portal to connect to SQL1 *with the local administrator account of SQL1*.</span></span>

<span data-ttu-id="0cc83-149">Ezután konfigurálja az SQL Server 2014 F: meghajtó használatára, az új adatbázisokat és a felhasználói fiók engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="0cc83-149">Next, configure SQL Server 2014 to use the F: drive for new databases and for user account permissions.</span></span>

1. <span data-ttu-id="0cc83-150">Írja be a kezdőképernyőről **az SQL Server felügyeleti**, és kattintson a **SQL Server 2014 Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-150">From the Start screen, type **SQL Server Management**, and then click **SQL Server 2014 Management Studio**.</span></span>
2. <span data-ttu-id="0cc83-151">A **kapcsolódás a kiszolgálóhoz**, kattintson a **Connect**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-151">In **Connect to Server**, click **Connect**.</span></span>
3. <span data-ttu-id="0cc83-152">Az Object Explorer fát megjelenítő ablaktáblán kattintson a jobb gombbal **SQL1**, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-152">In the Object Explorer tree pane, right-click **SQL1**, and then click **Properties**.</span></span>
4. <span data-ttu-id="0cc83-153">Az a **kiszolgáló tulajdonságai** ablak, kattintson a **adatbázis beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-153">In the **Server Properties** window, click **Database Settings**.</span></span>
5. <span data-ttu-id="0cc83-154">Keresse meg a **alapértelmezett helyek adatbázis** és állítsa be ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="0cc83-154">Locate the **Database default locations** and set these values:</span></span> 
   * <span data-ttu-id="0cc83-155">A **adatok**, írja be a elérési utat **f:\Data**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-155">For **Data**, type the path **f:\Data**.</span></span>
   * <span data-ttu-id="0cc83-156">A **napló**, írja be a elérési utat **f:\Log**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-156">For **Log**, type the path **f:\Log**.</span></span>
   * <span data-ttu-id="0cc83-157">A **biztonsági mentés**, írja be a elérési utat **f:\Backup**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-157">For **Backup**, type the path **f:\Backup**.</span></span>
   * <span data-ttu-id="0cc83-158">Megjegyzés: Csak az új adatbázisok használja ezeket a helyeket.</span><span class="sxs-lookup"><span data-stu-id="0cc83-158">Note: Only new databases use these locations.</span></span>
6. <span data-ttu-id="0cc83-159">Kattintson a **OK** az ablak bezárásához.</span><span class="sxs-lookup"><span data-stu-id="0cc83-159">Click the **OK** to close the window.</span></span>
7. <span data-ttu-id="0cc83-160">Az a **Object Explorer** fát megjelenítő ablaktáblán, nyissa meg **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-160">In the **Object Explorer** tree pane, open **Security**.</span></span>
8. <span data-ttu-id="0cc83-161">Kattintson a jobb gombbal **bejelentkezések** majd **új bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-161">Right-click **Logins** and then click **New Login**.</span></span>
9. <span data-ttu-id="0cc83-162">A **bejelentkezési név**, típus **corp\felhasználó1**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-162">In **Login name**, type **CORP\User1**.</span></span>
10. <span data-ttu-id="0cc83-163">A a **kiszolgálói szerepkörök** kattintson **sysadmin**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-163">On the **Server Roles** page, click **sysadmin**, and then click **OK**.</span></span>
11. <span data-ttu-id="0cc83-164">Zárja be a Microsoft SQL Server Management Studio eszközt.</span><span class="sxs-lookup"><span data-stu-id="0cc83-164">Close Microsoft SQL Server Management Studio.</span></span>

<span data-ttu-id="0cc83-165">Ez az az aktuális konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="0cc83-165">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-the-lob-server-lob1"></a><span data-ttu-id="0cc83-166">3. fázis: A LOB-kiszolgáló (LOB1) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0cc83-166">Phase 3: Configure the LOB server (LOB1)</span></span>
<span data-ttu-id="0cc83-167">Először hozzon létre egy virtuális gépet a LOB1 a következő parancsokkal a helyi számítógépen Azure PowerShell parancssorába.</span><span class="sxs-lookup"><span data-stu-id="0cc83-167">First, create a virtual machine for LOB1 with these commands at the Azure PowerShell command prompt on your local computer.</span></span>

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="0cc83-168">Ezután használhatja az Azure-portál csatlakozni LOB1 LOB1 a helyi rendszergazdai fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="0cc83-168">Next, use the Azure portal to connect to LOB1 with the credentials of the local administrator account of LOB1.</span></span>

<span data-ttu-id="0cc83-169">Ezután konfigurálja a Windows tűzfal szabály forgalom engedélyezésére hálózati kapcsolat tesztelése.</span><span class="sxs-lookup"><span data-stu-id="0cc83-169">Next, configure a Windows Firewall rule to allow traffic for basic connectivity testing.</span></span> <span data-ttu-id="0cc83-170">Egy rendszergazda szintű Windows PowerShell parancssorból a LOB1 futtassa az alábbi parancsokat.</span><span class="sxs-lookup"><span data-stu-id="0cc83-170">From an administrator-level Windows PowerShell command prompt on LOB1, run these commands.</span></span>

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

<span data-ttu-id="0cc83-171">A ping parancs négy sikeres választ 192.168.0.4 IP-címről eredményez.</span><span class="sxs-lookup"><span data-stu-id="0cc83-171">The ping command should result in four successful replies from IP address 192.168.0.4.</span></span>

<span data-ttu-id="0cc83-172">A következő LOB1 csatlakoztatása a CORP Active Directory-tartomány, a következő parancsokkal a Windows PowerShell parancssorába.</span><span class="sxs-lookup"><span data-stu-id="0cc83-172">Next, join LOB1 to the CORP Active Directory domain with these commands at the Windows PowerShell prompt.</span></span>

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

<span data-ttu-id="0cc83-173">Tartományi fiók hitelesítő adatait használjuk a corp\felhasználó1 fiókkal, amikor a rendszer kéri a **Add-Computer** parancsot.</span><span class="sxs-lookup"><span data-stu-id="0cc83-173">Use the CORP\User1 account when prompted to supply domain account credentials for the **Add-Computer** command.</span></span>

<span data-ttu-id="0cc83-174">Az újraindítás után csatlakozhat az Azure-portálon LOB1 a corp\felhasználó1 fiókkal és jelszóval.</span><span class="sxs-lookup"><span data-stu-id="0cc83-174">After restarting, use the Azure portal to connect to LOB1 with the CORP\User1 account and password.</span></span>

<span data-ttu-id="0cc83-175">A következő LOB1 konfigurálása az IIS és az ÜGYFÉL1-hozzáférés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="0cc83-175">Next, configure LOB1 for IIS and test access from CLIENT1.</span></span>

1. <span data-ttu-id="0cc83-176">A Kiszolgálókezelőben kattintson **szerepkörök és szolgáltatások hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-176">From Server Manager, click **Add roles and features**.</span></span>
2. <span data-ttu-id="0cc83-177">Az a **megkezdése előtt** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-177">On the **Before you begin** page, click **Next**.</span></span>
3. <span data-ttu-id="0cc83-178">Az a **telepítés típusának kiválasztása** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-178">On the **Select installation type** page, click **Next**.</span></span>
4. <span data-ttu-id="0cc83-179">Az a **jelölje be a célkiszolgáló** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-179">On the **Select destination server** page, click **Next**.</span></span>
5. <span data-ttu-id="0cc83-180">Az a **kiszolgálói szerepkörök** kattintson **webkiszolgáló (IIS)** listájában **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-180">On the **Server roles** page, click **Web Server (IIS)** in the list of **Roles**.</span></span>
6. <span data-ttu-id="0cc83-181">Amikor a rendszer kéri, kattintson a **szolgáltatások hozzáadása**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-181">When prompted, click **Add Features**, and then click **Next**.</span></span>
7. <span data-ttu-id="0cc83-182">Az a **szolgáltatások kiválasztása** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-182">On the **Select features** page, click **Next**.</span></span>
8. <span data-ttu-id="0cc83-183">Az a **webkiszolgáló (IIS)** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-183">On the **Web Server (IIS)** page, click **Next**.</span></span>
9. <span data-ttu-id="0cc83-184">Az a **szerepkör-szolgáltatások kiválasztása** lapon jelölje be vagy törölje a jelet a jelölőnégyzetből, a szolgáltatások, a LOB-alkalmazás teszteléséhez kell, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-184">On the **Select role services** page, select or clear the check boxes for the services you need for testing your LOB application, and then click **Next**.</span></span>
10. <span data-ttu-id="0cc83-185">Az a **erősítse meg a telepítendő** kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-185">On the **Confirm installation selections** page, click **Install**.</span></span>
11. <span data-ttu-id="0cc83-186">Várjon, amíg az összetevők telepítése befejeződött, és kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="0cc83-186">Wait until the installation of components has completed and then click **Close**.</span></span>
12. <span data-ttu-id="0cc83-187">Azure-portálról csatlakozzon a CLIENT1 számítógépre a corp\felhasználó1 hitelesítő adatai, és indítsa el az Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="0cc83-187">From the Azure portal, connect to the CLIENT1 computer with the CORP\User1 account credentials, and then start Internet Explorer.</span></span>
13. <span data-ttu-id="0cc83-188">Írja be a címsorba **http://lob1/** , és nyomja le az ENTER BILLENTYŰT.</span><span class="sxs-lookup"><span data-stu-id="0cc83-188">In the Address bar, type **http://lob1/** and then press ENTER.</span></span> <span data-ttu-id="0cc83-189">Az alapértelmezett IIS 8 weblap kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="0cc83-189">You should see the default IIS 8 web page.</span></span>

<span data-ttu-id="0cc83-190">Ez az az aktuális konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="0cc83-190">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

<span data-ttu-id="0cc83-191">Ebben a környezetben, hogy a webes alkalmazás LOB1 telepítse és tesztelje a Corpnet alhálózathoz az ÜGYFÉL1 funkciói készen áll.</span><span class="sxs-lookup"><span data-stu-id="0cc83-191">This environment is now ready for you to deploy your web-based application on LOB1 and test functionality from CLIENT1 on the Corpnet subnet.</span></span>

## <a name="next-step"></a><span data-ttu-id="0cc83-192">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="0cc83-192">Next step</span></span>
* <span data-ttu-id="0cc83-193">Adja hozzá egy új virtuális gép használata a [Azure-portálon](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0cc83-193">Add a new virtual machine using the [Azure portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

