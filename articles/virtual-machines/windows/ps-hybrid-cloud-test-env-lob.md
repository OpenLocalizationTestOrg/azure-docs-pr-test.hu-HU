---
title: "aaaLOB alkalmazás tesztkörnyezetben |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate a web-alapú, üzletági alkalmazás egy hibrid felhőalapú környezetet informatikai pro vagy fejlesztői teszteléshez."
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
ms.openlocfilehash: e9f825bbb1cbeb841ee3c974ebf6721dc236c311
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a><span data-ttu-id="296b0-103">Webes üzletági alkalmazás telepítése hibrid felhőben tesztelés céljára</span><span class="sxs-lookup"><span data-stu-id="296b0-103">Set up a web-based LOB application in a hybrid cloud for testing</span></span>
<span data-ttu-id="296b0-104">Ez a témakör végigvezeti egy üzletági (LOB) alkalmazások a Microsoft Azure-ban üzemeltetett webalapú üzletági tesztelési szimulált hibrid felhőkörnyezetekben létrehozása.</span><span class="sxs-lookup"><span data-stu-id="296b0-104">This topic steps you through creating a simulated hybrid cloud environment for testing a web-based line of business (LOB) application hosted in Microsoft Azure.</span></span> <span data-ttu-id="296b0-105">Itt található hello létrejövő konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="296b0-105">Here is hello resulting configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

<span data-ttu-id="296b0-106">Ez a konfiguráció a következőkből áll:</span><span class="sxs-lookup"><span data-stu-id="296b0-106">This configuration consists of:</span></span>

* <span data-ttu-id="296b0-107">A szimulált a helyi hálózaton (hello TestLab VNet) Azure-ban üzemeltetett.</span><span class="sxs-lookup"><span data-stu-id="296b0-107">A simulated on-premises network hosted in Azure (hello TestLab VNet).</span></span>
* <span data-ttu-id="296b0-108">A létesítmények közötti virtuális hálózat (TestVNET) Azure-ban üzemeltetett.</span><span class="sxs-lookup"><span data-stu-id="296b0-108">A cross-premises virtual network hosted in Azure (TestVNET).</span></span>
* <span data-ttu-id="296b0-109">Egy VNet – VNet VPN-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="296b0-109">A VNet-to-VNet VPN connection.</span></span>
* <span data-ttu-id="296b0-110">A web-alapú LOB server, a SQL server és a másodlagos tartományvezérlő hello TestVNET virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="296b0-110">A web-based LOB server, SQL server, and secondary domain controller in hello TestVNET virtual network.</span></span>

<span data-ttu-id="296b0-111">Ez a konfiguráció alapján és közös kiindulási pont, amelyről is biztosít:</span><span class="sxs-lookup"><span data-stu-id="296b0-111">This configuration provides a basis and common starting point from which you can:</span></span>

* <span data-ttu-id="296b0-112">Alkalmazások fejlesztéséhez és teszteléséhez LOB futó Internet Information Services (IIS) egy SQL Server 2014 adatbázis-fájlok az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="296b0-112">Develop and test LOB applications hosted on Internet Information Services (IIS) with a SQL Server 2014 database backend in Azure.</span></span>
* <span data-ttu-id="296b0-113">A szimulált hibrid felhőalapú informatikai munkaterhelés tesztek végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="296b0-113">Perform testing of this simulated hybrid cloud-based IT workload.</span></span>

<span data-ttu-id="296b0-114">A hibrid felhő tesztkörnyezet három fő fázisokat toosetting van:</span><span class="sxs-lookup"><span data-stu-id="296b0-114">There are three major phases toosetting up this hybrid cloud test environment:</span></span>

1. <span data-ttu-id="296b0-115">Hello szimulált hibrid felhő környezet beállítása.</span><span class="sxs-lookup"><span data-stu-id="296b0-115">Set up hello simulated hybrid cloud environment.</span></span>
2. <span data-ttu-id="296b0-116">Hello az SQL server számítógép (SQL1) konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="296b0-116">Configure hello SQL server computer (SQL1).</span></span>
3. <span data-ttu-id="296b0-117">Hello LOB-kiszolgáló (LOB1) konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="296b0-117">Configure hello LOB server (LOB1).</span></span>

<span data-ttu-id="296b0-118">A munkaterhelés Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="296b0-118">This workload requires an Azure subscription.</span></span> <span data-ttu-id="296b0-119">Ha MSDN vagy a Visual Studio-előfizetéssel rendelkezik, tekintse meg [havi Azure-kredit a Visual Studio-előfizetők](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="296b0-119">If you have an MSDN or Visual Studio subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

<span data-ttu-id="296b0-120">Az Azure-ban tárolt LOB-alkalmazás termelési példáért lásd: hello **üzletági alkalmazások** architektúra tervezetének: [Microsoft szoftverek architektúra diagramok és tervrajzokat](http://msdn.microsoft.com/dn630664).</span><span class="sxs-lookup"><span data-stu-id="296b0-120">For an example of a production LOB application hosted in Azure, see hello **Line of business applications** architecture blueprint at [Microsoft Software Architecture Diagrams and Blueprints](http://msdn.microsoft.com/dn630664).</span></span>

## <a name="phase-1-set-up-hello-simulated-hybrid-cloud-environment"></a><span data-ttu-id="296b0-121">1. fázis: Hello szimulált hibrid felhő környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="296b0-121">Phase 1: Set up hello simulated hybrid cloud environment</span></span>
<span data-ttu-id="296b0-122">Hozzon létre hello [szimulált hibrid felhő tesztkörnyezet](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="296b0-122">Create hello [simulated hybrid cloud test environment](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="296b0-123">Mivel ebben a tesztkörnyezetben nem szükséges hello az APP1 kiszolgáló hello Corpnet alhálózat hello jelenlétét, leállíthat azt a lépést.</span><span class="sxs-lookup"><span data-stu-id="296b0-123">Because this test environment does not require hello presence of hello APP1 server on hello Corpnet subnet, you can shut it down for now.</span></span>

<span data-ttu-id="296b0-124">Ez az az aktuális konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="296b0-124">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-hello-sql-server-computer-sql1"></a><span data-ttu-id="296b0-125">2. fázis: Hello az SQL server számítógép (SQL1) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="296b0-125">Phase 2: Configure hello SQL server computer (SQL1)</span></span>
<span data-ttu-id="296b0-126">Hello Azure-portálon, a start hello DC2 számítógépen, szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="296b0-126">From hello Azure portal, start hello DC2 computer if needed.</span></span>

<span data-ttu-id="296b0-127">Ezután hozzon létre egy virtuális gépet az sql1 számítógép a következő parancsokkal Azure PowerShell parancsot egy parancssorba a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="296b0-127">Next, create a virtual machine for SQL1 with these commands at an Azure PowerShell command prompt on your local computer.</span></span> <span data-ttu-id="296b0-128">Előzetes toorunning ezek a parancsok adja meg hello változók értékeinek és eltávolítása hello < és > karakter.</span><span class="sxs-lookup"><span data-stu-id="296b0-128">Prior toorunning these commands, fill in hello variable values and remove hello < and > characters.</span></span>

    $rgName="<your resource group name>"
    $locName="<hello Azure location of your resource group>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for hello SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="296b0-129">Használja az Azure portál tooconnect tooSQL1 hello hello helyi rendszergazdai fiókkal az SQL1.</span><span class="sxs-lookup"><span data-stu-id="296b0-129">Use hello Azure portal tooconnect tooSQL1 using hello local administrator account of SQL1.</span></span>

<span data-ttu-id="296b0-130">Ezután konfigurálja a Windows tűzfal szabályai tooallow hálózati kapcsolat tesztelése és az SQL Server-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="296b0-130">Next, configure Windows Firewall rules tooallow basic connectivity testing and SQL Server traffic.</span></span> <span data-ttu-id="296b0-131">Egy rendszergazda szintű Windows PowerShell parancssorból az sql1 számítógépen futtassa az alábbi parancsokat.</span><span class="sxs-lookup"><span data-stu-id="296b0-131">From an administrator-level Windows PowerShell command prompt on SQL1, run these commands.</span></span>

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

<span data-ttu-id="296b0-132">hello ping parancs négy sikeres választ 192.168.0.4 IP-címről eredményez.</span><span class="sxs-lookup"><span data-stu-id="296b0-132">hello ping command should result in four successful replies from IP address 192.168.0.4.</span></span>

<span data-ttu-id="296b0-133">Ezután adja hozzá a hello további adatlemezt az sql1 számítógépen hello meghajtóbetűjellel F: új kötetként.</span><span class="sxs-lookup"><span data-stu-id="296b0-133">Next, add hello extra data disk on SQL1 as a new volume with hello drive letter F:.</span></span>

1. <span data-ttu-id="296b0-134">Hello bal oldali ablaktáblán Kiszolgálókezelő kattintson **fájl- és tárolási szolgáltatások**, és kattintson a **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="296b0-134">In hello left pane of Server Manager, click **File and Storage Services**, and then click **Disks**.</span></span>
2. <span data-ttu-id="296b0-135">Hello Tartalom ablaktáblán, a hello **lemezek** csoportjában kattintson a **2 lemez** (a hello **partíció** túl beállítása**ismeretlen**).</span><span class="sxs-lookup"><span data-stu-id="296b0-135">In hello contents pane, in hello **Disks** group, click **disk 2** (with hello **Partition** set too**Unknown**).</span></span>
3. <span data-ttu-id="296b0-136">Kattintson a **feladatok**, és kattintson a **új kötet**.</span><span class="sxs-lookup"><span data-stu-id="296b0-136">Click **Tasks**, and then click **New Volume**.</span></span>
4. <span data-ttu-id="296b0-137">A hello hello új kötet varázsló oldalán elkezdéséhez kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="296b0-137">On hello Before you begin page of hello New Volume Wizard, click **Next**.</span></span>
5. <span data-ttu-id="296b0-138">Hello válassza hello kiszolgáló és lemez oldal, kattintson a **Disk 2**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="296b0-138">On hello Select hello server and disk page, click **Disk 2**, and then click **Next**.</span></span> <span data-ttu-id="296b0-139">Amikor a rendszer kéri, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="296b0-139">When prompted, click **OK**.</span></span>
6. <span data-ttu-id="296b0-140">A hello hello méret megadása hello kötet lap, kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="296b0-140">On hello Specify hello size of hello volume page, click **Next**.</span></span>
7. <span data-ttu-id="296b0-141">Oldalon hello hozzárendelése tooa meghajtó betűjele vagy a mappát, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="296b0-141">On hello Assign tooa drive letter or folder page, click **Next**.</span></span>
8. <span data-ttu-id="296b0-142">Hello fájl kijelölése rendszer beállítások lapon kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="296b0-142">On hello Select file system settings page, click **Next**.</span></span>
9. <span data-ttu-id="296b0-143">Hello megerősítése összetevők megerősítése oldalon kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="296b0-143">On hello Confirm selections page, click **Create**.</span></span>
10. <span data-ttu-id="296b0-144">Amikor végzett, kattintson **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="296b0-144">When complete, click **Close**.</span></span>

<span data-ttu-id="296b0-145">A parancsok hello Windows PowerShell parancssorába az sql1 számítógépen:</span><span class="sxs-lookup"><span data-stu-id="296b0-145">Run these commands at hello Windows PowerShell command prompt on SQL1:</span></span>

    md f:\Data
    md f:\Log
    md f:\Backup

<span data-ttu-id="296b0-146">A következő csatlakoztassa az sql1 számítógép toohello CORP Windows Server Active Directory-tartomány az sql1 számítógépen a Windows PowerShell-parancssorba hello parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="296b0-146">Next, join SQL1 toohello CORP Windows Server Active Directory domain with these commands at hello Windows PowerShell prompt on SQL1.</span></span>

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

<span data-ttu-id="296b0-147">Hello corp\felhasználó1 fiókkal, amikor a rendszer kéri a hello toosupply tartományi fiók hitelesítő adatainak **Add-Computer** parancsot.</span><span class="sxs-lookup"><span data-stu-id="296b0-147">Use hello CORP\User1 account when prompted toosupply domain account credentials for hello **Add-Computer** command.</span></span>

<span data-ttu-id="296b0-148">Az újraindítás után használja az Azure portál tooconnect tooSQL1 hello *hello helyi rendszergazdai fiókkal az SQL1*.</span><span class="sxs-lookup"><span data-stu-id="296b0-148">After restarting, use hello Azure portal tooconnect tooSQL1 *with hello local administrator account of SQL1*.</span></span>

<span data-ttu-id="296b0-149">Ezután konfigurálja az SQL Server 2014 toouse hello F: meghajtó az új adatbázisokat és a felhasználói fiók engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="296b0-149">Next, configure SQL Server 2014 toouse hello F: drive for new databases and for user account permissions.</span></span>

1. <span data-ttu-id="296b0-150">Hello kezdőképernyőről írja be a **az SQL Server felügyeleti**, és kattintson a **SQL Server 2014 Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="296b0-150">From hello Start screen, type **SQL Server Management**, and then click **SQL Server 2014 Management Studio**.</span></span>
2. <span data-ttu-id="296b0-151">A **tooServer csatlakozás**, kattintson a **Connect**.</span><span class="sxs-lookup"><span data-stu-id="296b0-151">In **Connect tooServer**, click **Connect**.</span></span>
3. <span data-ttu-id="296b0-152">Hello Object Explorer navigációs ablaktáblájában kattintson a jobb gombbal **SQL1**, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="296b0-152">In hello Object Explorer tree pane, right-click **SQL1**, and then click **Properties**.</span></span>
4. <span data-ttu-id="296b0-153">A hello **kiszolgáló tulajdonságai** ablak, kattintson a **adatbázis beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="296b0-153">In hello **Server Properties** window, click **Database Settings**.</span></span>
5. <span data-ttu-id="296b0-154">Keresse meg a hello **alapértelmezett helyek adatbázis** és állítsa be ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="296b0-154">Locate hello **Database default locations** and set these values:</span></span> 
   * <span data-ttu-id="296b0-155">A **adatok**, a típus hello elérési útjának **f:\Data**.</span><span class="sxs-lookup"><span data-stu-id="296b0-155">For **Data**, type hello path **f:\Data**.</span></span>
   * <span data-ttu-id="296b0-156">A **napló**, a típus hello elérési útjának **f:\Log**.</span><span class="sxs-lookup"><span data-stu-id="296b0-156">For **Log**, type hello path **f:\Log**.</span></span>
   * <span data-ttu-id="296b0-157">A **biztonsági mentés**, a típus hello elérési útjának **f:\Backup**.</span><span class="sxs-lookup"><span data-stu-id="296b0-157">For **Backup**, type hello path **f:\Backup**.</span></span>
   * <span data-ttu-id="296b0-158">Megjegyzés: Csak az új adatbázisok használja ezeket a helyeket.</span><span class="sxs-lookup"><span data-stu-id="296b0-158">Note: Only new databases use these locations.</span></span>
6. <span data-ttu-id="296b0-159">Kattintson a hello **OK** tooclose hello ablak.</span><span class="sxs-lookup"><span data-stu-id="296b0-159">Click hello **OK** tooclose hello window.</span></span>
7. <span data-ttu-id="296b0-160">A hello **Object Explorer** fát megjelenítő ablaktáblán, nyissa meg **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="296b0-160">In hello **Object Explorer** tree pane, open **Security**.</span></span>
8. <span data-ttu-id="296b0-161">Kattintson a jobb gombbal **bejelentkezések** majd **új bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="296b0-161">Right-click **Logins** and then click **New Login**.</span></span>
9. <span data-ttu-id="296b0-162">A **bejelentkezési név**, típus **corp\felhasználó1**.</span><span class="sxs-lookup"><span data-stu-id="296b0-162">In **Login name**, type **CORP\User1**.</span></span>
10. <span data-ttu-id="296b0-163">A hello **kiszolgálói szerepkörök** kattintson **sysadmin**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="296b0-163">On hello **Server Roles** page, click **sysadmin**, and then click **OK**.</span></span>
11. <span data-ttu-id="296b0-164">Zárja be a Microsoft SQL Server Management Studio eszközt.</span><span class="sxs-lookup"><span data-stu-id="296b0-164">Close Microsoft SQL Server Management Studio.</span></span>

<span data-ttu-id="296b0-165">Ez az az aktuális konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="296b0-165">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-hello-lob-server-lob1"></a><span data-ttu-id="296b0-166">3. fázis: Hello LOB-kiszolgáló (LOB1) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="296b0-166">Phase 3: Configure hello LOB server (LOB1)</span></span>
<span data-ttu-id="296b0-167">Először hozzon létre egy virtuális gépet a LOB1 a következő parancsokkal hello Azure PowerShell-parancssorba a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="296b0-167">First, create a virtual machine for LOB1 with these commands at hello Azure PowerShell command prompt on your local computer.</span></span>

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="296b0-168">Ezt követően használható hello Azure portál tooconnect tooLOB1 hello LOB1 hello helyi rendszergazdai fiókjának hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="296b0-168">Next, use hello Azure portal tooconnect tooLOB1 with hello credentials of hello local administrator account of LOB1.</span></span>

<span data-ttu-id="296b0-169">Ezután konfigurálja a hálózati kapcsolat tesztelése a Windows tűzfal szabály tooallow forgalom.</span><span class="sxs-lookup"><span data-stu-id="296b0-169">Next, configure a Windows Firewall rule tooallow traffic for basic connectivity testing.</span></span> <span data-ttu-id="296b0-170">Egy rendszergazda szintű Windows PowerShell parancssorból a LOB1 futtassa az alábbi parancsokat.</span><span class="sxs-lookup"><span data-stu-id="296b0-170">From an administrator-level Windows PowerShell command prompt on LOB1, run these commands.</span></span>

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

<span data-ttu-id="296b0-171">hello ping parancs négy sikeres választ 192.168.0.4 IP-címről eredményez.</span><span class="sxs-lookup"><span data-stu-id="296b0-171">hello ping command should result in four successful replies from IP address 192.168.0.4.</span></span>

<span data-ttu-id="296b0-172">A következő tartományhoz LOB1 toohello CORP Active Directory Windows PowerShell-parancssorba hello parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="296b0-172">Next, join LOB1 toohello CORP Active Directory domain with these commands at hello Windows PowerShell prompt.</span></span>

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

<span data-ttu-id="296b0-173">Hello corp\felhasználó1 fiókkal, amikor a rendszer kéri a hello toosupply tartományi fiók hitelesítő adatainak **Add-Computer** parancsot.</span><span class="sxs-lookup"><span data-stu-id="296b0-173">Use hello CORP\User1 account when prompted toosupply domain account credentials for hello **Add-Computer** command.</span></span>

<span data-ttu-id="296b0-174">Az újraindítás után használni hello Azure portál tooconnect tooLOB1 hello corp\felhasználó1 fiókot és jelszót.</span><span class="sxs-lookup"><span data-stu-id="296b0-174">After restarting, use hello Azure portal tooconnect tooLOB1 with hello CORP\User1 account and password.</span></span>

<span data-ttu-id="296b0-175">A következő LOB1 konfigurálása az IIS és az ÜGYFÉL1-hozzáférés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="296b0-175">Next, configure LOB1 for IIS and test access from CLIENT1.</span></span>

1. <span data-ttu-id="296b0-176">A Kiszolgálókezelőben kattintson **szerepkörök és szolgáltatások hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="296b0-176">From Server Manager, click **Add roles and features**.</span></span>
2. <span data-ttu-id="296b0-177">A hello **megkezdése előtt** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="296b0-177">On hello **Before you begin** page, click **Next**.</span></span>
3. <span data-ttu-id="296b0-178">A hello **telepítés típusának kiválasztása** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="296b0-178">On hello **Select installation type** page, click **Next**.</span></span>
4. <span data-ttu-id="296b0-179">A hello **jelölje be a célkiszolgáló** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="296b0-179">On hello **Select destination server** page, click **Next**.</span></span>
5. <span data-ttu-id="296b0-180">A hello **kiszolgálói szerepkörök** kattintson **webkiszolgáló (IIS)** hello listájában **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="296b0-180">On hello **Server roles** page, click **Web Server (IIS)** in hello list of **Roles**.</span></span>
6. <span data-ttu-id="296b0-181">Amikor a rendszer kéri, kattintson a **szolgáltatások hozzáadása**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="296b0-181">When prompted, click **Add Features**, and then click **Next**.</span></span>
7. <span data-ttu-id="296b0-182">A hello **szolgáltatások kiválasztása** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="296b0-182">On hello **Select features** page, click **Next**.</span></span>
8. <span data-ttu-id="296b0-183">A hello **webkiszolgáló (IIS)** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="296b0-183">On hello **Web Server (IIS)** page, click **Next**.</span></span>
9. <span data-ttu-id="296b0-184">A hello **szerepkör-szolgáltatások kiválasztása** lapon válassza ki vagy törölje a LOB-alkalmazás teszteléséhez kell hello szolgáltatások hello jelölőnégyzeteit, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="296b0-184">On hello **Select role services** page, select or clear hello check boxes for hello services you need for testing your LOB application, and then click **Next**.</span></span>
10. <span data-ttu-id="296b0-185">A hello **erősítse meg a telepítendő** kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="296b0-185">On hello **Confirm installation selections** page, click **Install**.</span></span>
11. <span data-ttu-id="296b0-186">Várjon, amíg az összetevők hello telepítése befejeződött, és kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="296b0-186">Wait until hello installation of components has completed and then click **Close**.</span></span>
12. <span data-ttu-id="296b0-187">Hello Azure-portálon, a csatlakozás toohello ÜGYFÉL1 számítógép hello corp\felhasználó1 fiók hitelesítő adatait, és indítsa el az Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="296b0-187">From hello Azure portal, connect toohello CLIENT1 computer with hello CORP\User1 account credentials, and then start Internet Explorer.</span></span>
13. <span data-ttu-id="296b0-188">Hello címsorába írja be a **http://lob1/** , és nyomja le az ENTER BILLENTYŰT.</span><span class="sxs-lookup"><span data-stu-id="296b0-188">In hello Address bar, type **http://lob1/** and then press ENTER.</span></span> <span data-ttu-id="296b0-189">Hello alapértelmezett IIS 8 weblap kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="296b0-189">You should see hello default IIS 8 web page.</span></span>

<span data-ttu-id="296b0-190">Ez az az aktuális konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="296b0-190">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

<span data-ttu-id="296b0-191">Ez a környezet már készen áll a akkor toodeploy a webes alkalmazás LOB1 és a vizsgálati funkcióival ÜGYFÉL1 hello Corpnet alhálózat.</span><span class="sxs-lookup"><span data-stu-id="296b0-191">This environment is now ready for you toodeploy your web-based application on LOB1 and test functionality from CLIENT1 on hello Corpnet subnet.</span></span>

## <a name="next-step"></a><span data-ttu-id="296b0-192">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="296b0-192">Next step</span></span>
* <span data-ttu-id="296b0-193">Adja hozzá egy új virtuális gép hello [Azure-portálon](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="296b0-193">Add a new virtual machine using hello [Azure portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

