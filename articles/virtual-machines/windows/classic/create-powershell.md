---
title: "a PowerShell használatával Windows virtuális gép aaaCreate |} Microsoft Docs"
description: "Hozzon létre Windows virtuális gépek Azure PowerShell és a hello klasszikus telepítési modell használatával."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 42c0d4be-573c-45d1-b9b0-9327537702f7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 5339f458c1f08f6e1752a53191f19402fab8ea25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell-and-hello-classic-deployment-model"></a><span data-ttu-id="d7767-103">Windows virtuális gép létrehozása a PowerShell és hello klasszikus telepítési modell</span><span class="sxs-lookup"><span data-stu-id="d7767-103">Create a Windows virtual machine with PowerShell and hello classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d7767-104">Azure portál – Windows</span><span class="sxs-lookup"><span data-stu-id="d7767-104">Azure portal - Windows</span></span>](tutorial.md)
> * [<span data-ttu-id="d7767-105">PowerShell - Windows</span><span class="sxs-lookup"><span data-stu-id="d7767-105">PowerShell - Windows</span></span>](create-powershell.md)
> 
> 

<br>

> [!IMPORTANT] 
> <span data-ttu-id="d7767-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d7767-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d7767-107">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="d7767-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="d7767-108">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="d7767-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="d7767-109">Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7767-109">Learn how too[perform these steps using hello Resource Manager model](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="d7767-110">Ezen lépések bemutatják, hogyan toocustomize Azure PowerShell készlete parancsok, amelyek létrehozása, és előre konfigurálása a Windows-alapú Azure virtuális gép egy építőelem módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="d7767-110">These steps show you how toocustomize a set of Azure PowerShell commands that create and preconfigure a Windows-based Azure virtual machine by using a building block approach.</span></span> <span data-ttu-id="d7767-111">Ez a folyamat használhat tooquickly létrehozhat egy új Windows-alapú virtuális gép egy parancsot, és bontsa ki a meglévő telepítés vagy egy egyéni fejlesztési és tesztelési célú vagy pro IT-környezet létrehozása több parancs beállítja, hogy gyorsan toocreate.</span><span class="sxs-lookup"><span data-stu-id="d7767-111">You can use this process tooquickly create a command set for a new Windows-based virtual machine and expand an existing deployment or toocreate multiple command sets that quickly build out a custom dev/test or IT pro environment.</span></span>

<span data-ttu-id="d7767-112">Ezeket a lépéseket hajtsa végre az Azure PowerShell-parancs készletek létrehozásához a fill-a-a-üres cellákat megközelítést.</span><span class="sxs-lookup"><span data-stu-id="d7767-112">These steps follow a fill-in-the-blanks approach for creating Azure PowerShell command sets.</span></span> <span data-ttu-id="d7767-113">Ezt a módszert akkor lehet hasznos, ha új tooPowerShell, vagy csak szeretné tooknow milyen értékeket toospecify sikeres konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="d7767-113">This approach can be useful if you are new tooPowerShell or you just want tooknow what values toospecify for successful configuration.</span></span> <span data-ttu-id="d7767-114">Speciális PowerShell felhasználók hello parancsok igénybe, és a hello változók (hello sorok kezdve a "$") a saját értékeit helyettesítse.</span><span class="sxs-lookup"><span data-stu-id="d7767-114">Advanced PowerShell users can take hello commands and substitute their own values for hello variables (hello lines beginning with "$").</span></span>

<span data-ttu-id="d7767-115">Ha még nem tette meg, használja a hello utasításait [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) tooinstall Azure PowerShell, a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d7767-115">If you haven't done so already, use hello instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) tooinstall Azure PowerShell on your local computer.</span></span> <span data-ttu-id="d7767-116">Ezután nyissa meg a Windows PowerShell-parancssort.</span><span class="sxs-lookup"><span data-stu-id="d7767-116">Then, open a Windows PowerShell command prompt.</span></span>

## <a name="step-1-add-your-account"></a><span data-ttu-id="d7767-117">1. lépés: A fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d7767-117">Step 1: Add your account</span></span>
1. <span data-ttu-id="d7767-118">Hello PowerShell parancssorába írja be a **Add-AzureAccount** kattintson **Enter**.</span><span class="sxs-lookup"><span data-stu-id="d7767-118">At hello PowerShell prompt, type **Add-AzureAccount** and click **Enter**.</span></span> 
2. <span data-ttu-id="d7767-119">Írja be az Azure-előfizetéshez társított hello e-mail címét, és kattintson a **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="d7767-119">Type in hello email address associated with your Azure subscription and click **Continue**.</span></span> 
3. <span data-ttu-id="d7767-120">Adja meg a fiókhoz tartozó jelszót hello.</span><span class="sxs-lookup"><span data-stu-id="d7767-120">Type in hello password for your account.</span></span> 
4. <span data-ttu-id="d7767-121">Kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d7767-121">Click **Sign in**.</span></span>

## <a name="step-2-set-your-subscription-and-storage-account"></a><span data-ttu-id="d7767-122">2. lépés: Az előfizetés és a storage-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="d7767-122">Step 2: Set your subscription and storage account</span></span>
<span data-ttu-id="d7767-123">Ezek a parancsok futtatásával hello Windows PowerShell parancssorába állítsa be a Azure-előfizetés és a storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="d7767-123">Set your Azure subscription and storage account by running these commands at hello Windows PowerShell command prompt.</span></span> <span data-ttu-id="d7767-124">Cserélje le mindent, ami hello idézőjelek között, beleértve a hello < és > karakter, helyes nevek hello.</span><span class="sxs-lookup"><span data-stu-id="d7767-124">Replace everything within hello quotes, including hello < and > characters, with hello correct names.</span></span>

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

<span data-ttu-id="d7767-125">Hello megfelelő előfizetés neve letölthető hello hello hello kimenete SubscriptionName tulajdonságának **Get-AzureSubscription** parancsot.</span><span class="sxs-lookup"><span data-stu-id="d7767-125">You can get hello correct subscription name from hello SubscriptionName property of hello output of hello **Get-AzureSubscription** command.</span></span> <span data-ttu-id="d7767-126">Hello helyes-e a tárfiók nevének beolvasása hello hello hello kimenete Label tulajdonságának **Get-AzureStorageAccount** parancsot, miután lefuttatta a hello **válasszon-AzureSubscription** parancsot.</span><span class="sxs-lookup"><span data-stu-id="d7767-126">You can get hello correct storage account name from hello Label property of hello output of hello **Get-AzureStorageAccount** command after you run hello **Select-AzureSubscription** command.</span></span>

## <a name="step-3-determine-hello-imagefamily"></a><span data-ttu-id="d7767-127">3. lépés: Hello ImageFamily meghatározása</span><span class="sxs-lookup"><span data-stu-id="d7767-127">Step 3: Determine hello ImageFamily</span></span>
<span data-ttu-id="d7767-128">A következő lépésben toodetermine hello ImageFamily vagy a címkeérték hello azokba megfelelő toohello toocreate kívánt Azure virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="d7767-128">Next, you need toodetermine hello ImageFamily or Label value for hello specific image corresponding toohello Azure virtual machine you want toocreate.</span></span> <span data-ttu-id="d7767-129">Hello elérhető ImageFamily értékek listáját ezzel a paranccsal kaphat.</span><span class="sxs-lookup"><span data-stu-id="d7767-129">You can get hello list of available ImageFamily values with this command.</span></span>

    Get-AzureVMImage | select ImageFamily -Unique

<span data-ttu-id="d7767-130">Íme néhány példa a Windows-alapú számítógépek ImageFamily értékeket:</span><span class="sxs-lookup"><span data-stu-id="d7767-130">Here are some examples of ImageFamily values for Windows-based computers:</span></span>

* <span data-ttu-id="d7767-131">Windows Server 2012 R2 Datacenter</span><span class="sxs-lookup"><span data-stu-id="d7767-131">Windows Server 2012 R2 Datacenter</span></span>
* <span data-ttu-id="d7767-132">Windows Server 2008 R2 SP1</span><span class="sxs-lookup"><span data-stu-id="d7767-132">Windows Server 2008 R2 SP1</span></span>
* <span data-ttu-id="d7767-133">A Windows Server 2016 Technical Preview 4</span><span class="sxs-lookup"><span data-stu-id="d7767-133">Windows Server 2016 Technical Preview 4</span></span>
* <span data-ttu-id="d7767-134">SQL Server 2012 SP1 Enterprise, Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="d7767-134">SQL Server 2012 SP1 Enterprise on Windows Server 2012</span></span>

<span data-ttu-id="d7767-135">Ha megtalálta a keresett hello kép, nyissa meg a friss példányának hello szövegszerkesztőben a választott vagy hello PowerShell integrált parancsfájlkezelési környezet (ISE).</span><span class="sxs-lookup"><span data-stu-id="d7767-135">If you find hello image you are looking for, open a fresh instance of hello text editor of your choice or hello PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="d7767-136">Hello következő hello új szöveges fájlt vagy a PowerShell ISE hello ImageFamily érték és hello másolja.</span><span class="sxs-lookup"><span data-stu-id="d7767-136">Copy hello following into hello new text file or hello PowerShell ISE, substituting hello ImageFamily value.</span></span>

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

<span data-ttu-id="d7767-137">Bizonyos esetekben hello lemezképnév hello Label tulajdonságának hello ImageFamily érték helyett szerepel.</span><span class="sxs-lookup"><span data-stu-id="d7767-137">In some cases, hello image name is in hello Label property instead of hello ImageFamily value.</span></span> <span data-ttu-id="d7767-138">Ha a keresett hello ImageFamily tulajdonsággal hello kép nem található, listában hello képek ezzel a paranccsal a címke tulajdonság által.</span><span class="sxs-lookup"><span data-stu-id="d7767-138">If you didn't find hello image that you are looking for using hello ImageFamily property, list hello images by their Label property with this command.</span></span>

    Get-AzureVMImage | select Label -Unique

<span data-ttu-id="d7767-139">Ha hello jobb kép ezzel a paranccsal, nyissa meg a friss példánya a választott vagy a PowerShell ISE hello hello szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="d7767-139">If you find hello right image with this command, open a fresh instance of hello text editor of your choice or hello PowerShell ISE.</span></span> <span data-ttu-id="d7767-140">Hello következő másolása hello új szöveges fájlt vagy a PowerShell ISE hello címkeérték és hello.</span><span class="sxs-lookup"><span data-stu-id="d7767-140">Copy hello following into hello new text file or hello PowerShell ISE, substituting hello Label value.</span></span>

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a><span data-ttu-id="d7767-141">4. lépés: A parancs-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7767-141">Step 4: Build your command set</span></span>
<span data-ttu-id="d7767-142">A parancs, állítsa be az új szöveges fájlba vagy hello ISE hello változó kitöltése és hello < és > karakter eltávolítása hello megfelelő állít elő alatt másolásával hello részeinek felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d7767-142">Build hello rest of your command set by copying hello appropriate set of blocks below into your new text file or hello ISE and then filling in hello variable values and removing hello < and > characters.</span></span> <span data-ttu-id="d7767-143">Lásd: a két hello [példák](#examples) hello hello végeredményt a képet a jelen cikk végén.</span><span class="sxs-lookup"><span data-stu-id="d7767-143">See hello two [examples](#examples) at hello end of this article for an idea of hello final result.</span></span>

<span data-ttu-id="d7767-144">Indítsa el a parancs, válassza ki az egyik alábbi két parancs blokkok (kötelező).</span><span class="sxs-lookup"><span data-stu-id="d7767-144">Start your command set by choosing one of these two command blocks (required).</span></span>

<span data-ttu-id="d7767-145">1. lehetőség: Adja meg a virtuális gép nevét és méretét.</span><span class="sxs-lookup"><span data-stu-id="d7767-145">Option 1: Specify a virtual machine name and a size.</span></span>

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

<span data-ttu-id="d7767-146">2. lehetőség: Adja meg a nevét, méretét, és rendelkezésre állási készlet neve.</span><span class="sxs-lookup"><span data-stu-id="d7767-146">Option 2: Specify a name, size, and availability set name.</span></span>

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

<span data-ttu-id="d7767-147">Hello InstanceSize D-, DS- vagy értékeit G sorozatú virtuális gépek, lásd: [virtuális gépek és Felhőszolgáltatások mérete az Azure-](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7767-147">For hello InstanceSize values for D-, DS-, or G-series virtual machines, see [Virtual Machine and Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="d7767-148">Ha egy frissítési garancia nagyvállalati szerződéssel, és tootake előnyeit, Windows Server hello kíván [hibrid használata juttatás](https://azure.microsoft.com/pricing/hybrid-use-benefit/), adja hozzá a **- LicenseType** paraméter toohello  **Új AzureVMConfig** parancsmag hello értéket átadja **Windows_Server** a hello tipikus használati eset.</span><span class="sxs-lookup"><span data-stu-id="d7767-148">If you have an Enterprise Agreement with Software Assurance, and intend tootake advantage of hello Windows Server [Hybrid Use Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/), add the **-LicenseType** parameter toohello **New-AzureVMConfig** cmdlet, passing hello value **Windows_Server** for hello typical use case.</span></span>  <span data-ttu-id="d7767-149">Lehet, hogy a képfájl feltöltött; nem használhat egy szabványos lemezkép hello gyűjteménye hello hibrid használja juttatásra.</span><span class="sxs-lookup"><span data-stu-id="d7767-149">Be sure you are using an image you have uploaded; you cannot use a standard image from hello Gallery with hello Hybrid Use Benefit.</span></span>
> 
> 

<span data-ttu-id="d7767-150">Önálló Windows számítógép esetén megadhat hello helyi rendszergazda fiókot és jelszót.</span><span class="sxs-lookup"><span data-stu-id="d7767-150">Optionally, for a standalone Windows computer, specify hello local administrator account and password.</span></span>

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

<span data-ttu-id="d7767-151">Írjon be egy erős jelszót.</span><span class="sxs-lookup"><span data-stu-id="d7767-151">Choose a strong password.</span></span> <span data-ttu-id="d7767-152">toocheck annak erőssége, lásd: [jelszóellenőrző: az erős jelszavak használata](https://www.microsoft.com/security/pc-security/password-checker.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7767-152">toocheck its strength, see [Password Checker: Using Strong Passwords](https://www.microsoft.com/security/pc-security/password-checker.aspx).</span></span>

<span data-ttu-id="d7767-153">Tooadd hello Windows számítógép tooan meglévő Active Directory-tartomány, megadhat hello helyi rendszergazdai fiók és jelszó, hello a tartományban, és hello nevét és egy tartományi fiók.</span><span class="sxs-lookup"><span data-stu-id="d7767-153">Optionally, tooadd hello Windows computer tooan existing Active Directory domain, specify hello local administrator account and password, hello domain, and hello name and password of a domain account.</span></span>

    $cred1=Get-Credential –Message "Type hello name and password of hello local administrator account."
    $cred2=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account that has permission tooadd hello machine toohello domain."
    $domaindns="<FQDN of hello domain that hello machine is joining>"
    $domacctdomain="<domain of hello account that has permission tooadd hello machine toohello domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

<span data-ttu-id="d7767-154">További előtti konfigurációs beállítások a Windows-alapú virtuális gépekhez, lásd: hello hello szintaxisának **Windows** és **WindowsDomain és** paraméterkészletek [ Adja hozzá AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).</span><span class="sxs-lookup"><span data-stu-id="d7767-154">For additional pre-configuration options for Windows-based virtual machines, see hello syntax for hello **Windows** and **WindowsDomain** parameter sets in [Add-AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).</span></span>

<span data-ttu-id="d7767-155">Másik lehetőségként hozzárendelése hello virtuális gép egy adott IP-cím, egy statikus DIP néven.</span><span class="sxs-lookup"><span data-stu-id="d7767-155">Optionally, assign hello virtual machine a specific IP address, known as a static DIP.</span></span>

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

<span data-ttu-id="d7767-156">Ellenőrizheti, hogy az elérhető legyen-e egy adott IP-cím:</span><span class="sxs-lookup"><span data-stu-id="d7767-156">You can verify that a specific IP address is available with:</span></span>

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

<span data-ttu-id="d7767-157">Másik lehetőségként hozzárendelése hello virtuális gép tooa megadott alhálózat egy Azure virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="d7767-157">Optionally, assign hello virtual machine tooa specific subnet in an Azure virtual network.</span></span>

    $vm1 | Set-AzureSubnet -SubnetNames "<name of hello subnet>"

<span data-ttu-id="d7767-158">Opcionálisan adja hozzá a virtuális gép egyetlen lemez toohello.</span><span class="sxs-lookup"><span data-stu-id="d7767-158">Optionally, add a single data disk toohello virtual machine.</span></span>

    $disksize=<size of hello disk in GB>
    $disklabel="<hello label on hello disk>"
    $lun=<Logical Unit Number (LUN) of hello disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

<span data-ttu-id="d7767-159">Egy Active Directory-tartományvezérlő beállítása $hcaching túl "None".</span><span class="sxs-lookup"><span data-stu-id="d7767-159">For an Active Directory domain controller, set $hcaching too"None".</span></span>

<span data-ttu-id="d7767-160">Opcionálisan adja hozzá a hello virtuális gép tooan elosztott terhelésű készlet a külső forgalom.</span><span class="sxs-lookup"><span data-stu-id="d7767-160">Optionally, add hello virtual machine tooan existing load-balanced set for external traffic.</span></span>

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of hello internal port>
    $pubport=<port number of hello external port>
    $endpointname="<name of hello endpoint>"
    $lbsetname="<name of hello existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

<span data-ttu-id="d7767-161">Végül válasszon egyet ezek szükséges parancs blokkok hello virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d7767-161">Finally, choose one of these required command blocks for creating hello virtual machine.</span></span>

<span data-ttu-id="d7767-162">1. lehetőség: Hozzon létre egy meglévő felhőszolgáltatáshoz hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d7767-162">Option 1: Create hello virtual machine in an existing cloud service.</span></span>

    New-AzureVM –ServiceName "<short name of hello cloud service>" -VMs $vm1

<span data-ttu-id="d7767-163">hello rövid neve hello felhőalapú szolgáltatás nem hello név jelenik meg a hello listája Felhőszolgáltatások hello Azure-portálon vagy hello hello Azure-portálon az erőforráscsoportok listáját.</span><span class="sxs-lookup"><span data-stu-id="d7767-163">hello short name of hello cloud service is hello name that appears in hello list of Cloud Services in hello Azure portal or in hello list of Resource Groups in hello Azure portal.</span></span>

<span data-ttu-id="d7767-164">2. lehetőség: Hello virtuális gép létrehozása egy létező felhőalapú szolgáltatást és a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="d7767-164">Option 2: Create hello virtual machine in an existing cloud service and virtual network.</span></span>

    $svcname="<short name of hello cloud service>"
    $vnetname="<name of hello virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a><span data-ttu-id="d7767-165">5. lépés: Futtassa a parancsot</span><span class="sxs-lookup"><span data-stu-id="d7767-165">Step 5: Run your command set</span></span>
<span data-ttu-id="d7767-166">Tekintse át a szövegszerkesztőben parancsfájlkezelő hello Azure PowerShell parancskészlethez vagy hello a PowerShell ISE álló a több blokkra parancsok a 4. lépéssel.</span><span class="sxs-lookup"><span data-stu-id="d7767-166">Review hello Azure PowerShell command set you built in your text editor or hello PowerShell ISE consisting of multiple blocks of commands from step 4.</span></span> <span data-ttu-id="d7767-167">Győződjön meg arról, hogy megadta-e minden szükséges hello változót, és hogy hello helyes az értékük.</span><span class="sxs-lookup"><span data-stu-id="d7767-167">Ensure that you have specified all hello needed variables and that they have hello correct values.</span></span> <span data-ttu-id="d7767-168">Ellenőrizze azt is, hogy eltávolította-e az összes hello < és > karakter.</span><span class="sxs-lookup"><span data-stu-id="d7767-168">Also make sure that you have removed all hello < and > characters.</span></span>

<span data-ttu-id="d7767-169">Használatakor egy szövegszerkesztőben, a Másolás hello parancs toohello vágólapra beállítása, és kattintson a jobb gombbal a megnyitott Windows PowerShell-parancssort.</span><span class="sxs-lookup"><span data-stu-id="d7767-169">If you are using a text editor, copy hello command set toohello clipboard and then right-click your open Windows PowerShell command prompt.</span></span> <span data-ttu-id="d7767-170">A PowerShell-parancsok sorozataként hello parancskészlethez ki és hozza létre az Azure virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d7767-170">This will issue hello command set as a series of PowerShell commands and create your Azure virtual machine.</span></span> <span data-ttu-id="d7767-171">Alternatív megoldásként futtassa a PowerShell ISE hello beállítása hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="d7767-171">Alternately, run hello command set in hello PowerShell ISE.</span></span>

<span data-ttu-id="d7767-172">Ha létrehozni újra a virtuális gép vagy egy hasonló, akkor a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="d7767-172">If you will be creating this virtual machine again or a similar one, you can:</span></span>

* <span data-ttu-id="d7767-173">Ez a parancs egy olyan PowerShell-parancsfájlt (*.ps1) állítja mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="d7767-173">Save this command set as a PowerShell script file (*.ps1).</span></span>
* <span data-ttu-id="d7767-174">Ez a parancs egy Azure Automation-runbook a hello állítja mentéséhez **Automation-fiók** hello Azure-portálon szakasza.</span><span class="sxs-lookup"><span data-stu-id="d7767-174">Save this command set as an Azure Automation runbook in hello **Automation Accounts** section of hello Azure portal.</span></span>

## <span data-ttu-id="d7767-175"><a id="examples"></a>Példák</span><span class="sxs-lookup"><span data-stu-id="d7767-175"><a id="examples"></a>Examples</span></span>
<span data-ttu-id="d7767-176">Az alábbiakban a két példa hello lépésekkel fent toobuild Azure PowerShell parancs beállítja, hogy a Windows-alapú Azure virtuális gépek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d7767-176">Here are two examples of using hello steps above toobuild Azure PowerShell command sets that create Windows-based Azure virtual machines.</span></span>

### <a name="example-1"></a><span data-ttu-id="d7767-177">1. példa</span><span class="sxs-lookup"><span data-stu-id="d7767-177">Example 1</span></span>
<span data-ttu-id="d7767-178">A PowerShell kell parancskészlet toocreate hello kezdeti virtuális gép egy Active Directory-tartományvezérlőhöz, amelyek:</span><span class="sxs-lookup"><span data-stu-id="d7767-178">I need a PowerShell command set toocreate hello initial virtual machine for an Active Directory domain controller that:</span></span>

* <span data-ttu-id="d7767-179">Hello Windows Server 2012 R2 Datacenter rendszerképet használja.</span><span class="sxs-lookup"><span data-stu-id="d7767-179">Uses hello Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="d7767-180">AZDC1 hello névvel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d7767-180">Has hello name AZDC1.</span></span>
* <span data-ttu-id="d7767-181">Egy olyan önálló számítógép.</span><span class="sxs-lookup"><span data-stu-id="d7767-181">Is a standalone computer.</span></span>
* <span data-ttu-id="d7767-182">Rendelkezik egy további adatlemezt 20 GB.</span><span class="sxs-lookup"><span data-stu-id="d7767-182">Has an additional data disk of 20 GB.</span></span>
* <span data-ttu-id="d7767-183">Hello statikus IP-címmel 192.168.244.4 rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d7767-183">Has hello static IP address 192.168.244.4.</span></span>
* <span data-ttu-id="d7767-184">Hello BackEnd alhálózathoz hello AZDatacenter virtuális hálózat szerepel.</span><span class="sxs-lookup"><span data-stu-id="d7767-184">Is in hello BackEnd subnet of hello AZDatacenter virtual network.</span></span>
* <span data-ttu-id="d7767-185">Azure-KELJFELJANCSI hello felhőszolgáltatás van.</span><span class="sxs-lookup"><span data-stu-id="d7767-185">Is in hello Azure-TailspinToys cloud service.</span></span>

<span data-ttu-id="d7767-186">Ez hello megfelelő Azure PowerShell paranccsal állítsa toocreate a virtuális gépre, az üres sorok közötti valamennyi blokkja olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="d7767-186">Here is hello corresponding Azure PowerShell command set toocreate this virtual machine, with blank lines between each block for readability.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

### <a name="example-2"></a><span data-ttu-id="d7767-187">2. példa</span><span class="sxs-lookup"><span data-stu-id="d7767-187">Example 2</span></span>
<span data-ttu-id="d7767-188">A PowerShell kell parancskészlet toocreate üzleti kiszolgáló virtuális gép, amely:</span><span class="sxs-lookup"><span data-stu-id="d7767-188">I need a PowerShell command set toocreate a virtual machine for a line-of-business server that:</span></span>

* <span data-ttu-id="d7767-189">Hello Windows Server 2012 R2 Datacenter rendszerképet használja.</span><span class="sxs-lookup"><span data-stu-id="d7767-189">Uses hello Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="d7767-190">LOB1 hello névvel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d7767-190">Has hello name LOB1.</span></span>
* <span data-ttu-id="d7767-191">Hello corp.contoso.com tartomány tagja.</span><span class="sxs-lookup"><span data-stu-id="d7767-191">Is a member of hello corp.contoso.com domain.</span></span>
* <span data-ttu-id="d7767-192">200 GB-os egy további adatok lemezzel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d7767-192">Has an additional data disk of 200 GB.</span></span>
* <span data-ttu-id="d7767-193">Hello FrontEnd alhálózathoz hello AZDatacenter virtuális hálózat szerepel.</span><span class="sxs-lookup"><span data-stu-id="d7767-193">Is in hello FrontEnd subnet of hello AZDatacenter virtual network.</span></span>
* <span data-ttu-id="d7767-194">Azure-KELJFELJANCSI hello felhőszolgáltatás van.</span><span class="sxs-lookup"><span data-stu-id="d7767-194">Is in hello Azure-TailspinToys cloud service.</span></span>

<span data-ttu-id="d7767-195">Ez hello megfelelő Azure PowerShell paranccsal állítsa toocreate ezt a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d7767-195">Here is hello corresponding Azure PowerShell command set toocreate this virtual machine.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type hello name and password of hello local administrator account."
    $cred2=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account that has permission tooadd hello machine toohello domain."
    $domaindns="corp.contoso.com"
    $domacctdomain="CORP"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "FrontEnd"

    $disksize=200
    $disklabel="LOBData"
    $lun=0
    $hcaching="ReadWrite"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="next-steps"></a><span data-ttu-id="d7767-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7767-196">Next steps</span></span>
<span data-ttu-id="d7767-197">Ha egy 127 GB-nál nagyobb méretű operációsrendszer-lemez van szüksége, akkor [bontsa ki az operációs rendszer hello meghajtó](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7767-197">If you need an OS disk that is larger than 127 GB, you can [expand hello OS drive](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

