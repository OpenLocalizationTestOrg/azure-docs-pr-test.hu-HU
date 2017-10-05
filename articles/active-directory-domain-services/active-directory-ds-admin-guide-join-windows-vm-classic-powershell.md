---
title: "Az Azure Active Directory tartományi szolgáltatások: Felügyeleti útmutató |} Microsoft Docs"
description: "A Windows rendszerű virtuális gép csatlakoztatása felügyelt tartományhoz Azure PowerShell és a klasszikus telepítési modell használatával."
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9143b843-7327-43c3-baab-6e24a18db25e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 1bb1546fb616131a1e1868a0d0610c4cad5d73e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a><span data-ttu-id="55fa2-103">A Windows Server virtuális gép csatlakoztatása felügyelt tartományhoz PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="55fa2-103">Join a Windows Server virtual machine to a managed domain using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="55fa2-104">Klasszikus Azure portál – Windows</span><span class="sxs-lookup"><span data-stu-id="55fa2-104">Azure classic portal - Windows</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
> * [<span data-ttu-id="55fa2-105">PowerShell - Windows</span><span class="sxs-lookup"><span data-stu-id="55fa2-105">PowerShell - Windows</span></span>](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> <span data-ttu-id="55fa2-106">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="55fa2-106">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="55fa2-107">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="55fa2-107">This article covers using the classic deployment model.</span></span> <span data-ttu-id="55fa2-108">Azure AD tartományi szolgáltatások jelenleg nem támogatja a Resource Manager modellt.</span><span class="sxs-lookup"><span data-stu-id="55fa2-108">Azure AD Domain Services does not currently support the Resource Manager model.</span></span>
>
>

<span data-ttu-id="55fa2-109">Ezeket a lépéseket mutatja be Azure PowerShell-parancsokat, amelyek létrehozása, és előre konfigurálása a Windows-alapú Azure virtuális gép egy építőelem módszer segítségével testre szabhatja.</span><span class="sxs-lookup"><span data-stu-id="55fa2-109">These steps show you how to customize a set of Azure PowerShell commands that create and preconfigure a Windows-based Azure virtual machine by using a building block approach.</span></span> <span data-ttu-id="55fa2-110">Az alábbi lépések segítségével a Windows-alapú Azure virtuális gép létrehozása, és csatlakoztassa azt az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="55fa2-110">These steps help you build a Windows-based Azure virtual machine and join it to an Azure AD Domain Services managed domain.</span></span>

<span data-ttu-id="55fa2-111">Ezeket a lépéseket hajtsa végre az Azure PowerShell-parancs készletek létrehozásához a fill-a-a-üres cellákat megközelítést.</span><span class="sxs-lookup"><span data-stu-id="55fa2-111">These steps follow a fill-in-the-blanks approach for creating Azure PowerShell command sets.</span></span> <span data-ttu-id="55fa2-112">Ezt a módszert akkor lehet hasznos, ha még nem ismeri a PowerShell vagy meg szeretné ismerni, hogy milyen értékeket adhatja meg a sikeres konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="55fa2-112">This approach can be useful if you are new to PowerShell or you want to know what values to specify for successful configuration.</span></span> <span data-ttu-id="55fa2-113">Speciális PowerShell felhasználók igénybe vehet a parancsokat és helyettesítse a saját a változók értékeit (a "$" kezdődő sorok).</span><span class="sxs-lookup"><span data-stu-id="55fa2-113">Advanced PowerShell users can take the commands and substitute their own values for the variables (the lines beginning with "$").</span></span>

<span data-ttu-id="55fa2-114">Ha még nem tette meg, kövesse az utasításokat, a [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview) Azure PowerShell telepítése a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="55fa2-114">If you haven't done so already, use the instructions in [How to install and configure Azure PowerShell](/powershell/azure/overview) to install Azure PowerShell on your local computer.</span></span> <span data-ttu-id="55fa2-115">Ezután nyissa meg a Windows PowerShell-parancssort.</span><span class="sxs-lookup"><span data-stu-id="55fa2-115">Then, open a Windows PowerShell command prompt.</span></span>

## <a name="step-1-add-your-account"></a><span data-ttu-id="55fa2-116">1. lépés: A fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="55fa2-116">Step 1: Add your account</span></span>
1. <span data-ttu-id="55fa2-117">A PowerShell-parancssorba írja be a **Add-AzureAccount** kattintson **Enter**.</span><span class="sxs-lookup"><span data-stu-id="55fa2-117">At the PowerShell prompt, type **Add-AzureAccount** and click **Enter**.</span></span>
2. <span data-ttu-id="55fa2-118">Írja be az Azure-előfizetéshez társított e-mail címét, és kattintson a **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="55fa2-118">Type in the email address associated with your Azure subscription and click **Continue**.</span></span>
3. <span data-ttu-id="55fa2-119">Adja meg a fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="55fa2-119">Type in the password for your account.</span></span>
4. <span data-ttu-id="55fa2-120">Kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="55fa2-120">Click **Sign in**.</span></span>

## <a name="step-2-set-your-subscription-and-storage-account"></a><span data-ttu-id="55fa2-121">2. lépés: Az előfizetés és a storage-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="55fa2-121">Step 2: Set your subscription and storage account</span></span>
<span data-ttu-id="55fa2-122">Az Azure-előfizetés és a storage-fiók beállítása a Windows PowerShell parancssorába a parancsok futtatásával.</span><span class="sxs-lookup"><span data-stu-id="55fa2-122">Set your Azure subscription and storage account by running these commands at the Windows PowerShell command prompt.</span></span> <span data-ttu-id="55fa2-123">Cserélje le a mindent, ami az ajánlatokat, beleértve a < és > karakter, helyes nevét.</span><span class="sxs-lookup"><span data-stu-id="55fa2-123">Replace everything within the quotes, including the < and > characters, with the correct names.</span></span>

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

<span data-ttu-id="55fa2-124">A megfelelő előfizetés neve lekérheti a kibocsátás SubscriptionName tulajdonság a **Get-AzureSubscription** parancsot.</span><span class="sxs-lookup"><span data-stu-id="55fa2-124">You can get the correct subscription name from the SubscriptionName property of the output of the **Get-AzureSubscription** command.</span></span> <span data-ttu-id="55fa2-125">A megfelelő tárfióknév lekérheti a kimenetét Label tulajdonságának a **Get-AzureStorageAccount** parancs futtatása után a **válasszon-AzureSubscription** parancsot.</span><span class="sxs-lookup"><span data-stu-id="55fa2-125">You can get the correct storage account name from the Label property of the output of the **Get-AzureStorageAccount** command after you run the **Select-AzureSubscription** command.</span></span>

## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a><span data-ttu-id="55fa2-126">3. lépés: Részletes útmutató - a virtuális gép kiépítéséhez, és csatlakoztassa azt a felügyelt tartományhoz</span><span class="sxs-lookup"><span data-stu-id="55fa2-126">Step 3: Step-by-step walkthrough - provision the virtual machine and join it to the managed domain</span></span>
<span data-ttu-id="55fa2-127">Ez a megfelelő Azure PowerShell-parancsot, állítsa be a virtuális gép létrehozása üres sorok közötti valamennyi blokkja olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="55fa2-127">Here is the corresponding Azure PowerShell command set to create this virtual machine, with blank lines between each block for readability.</span></span>

<span data-ttu-id="55fa2-128">Adjon meg információt a Windows rendszerű virtuális gép úgy kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="55fa2-128">Specify information about the Windows virtual machine to be provisioned.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

<span data-ttu-id="55fa2-129">D-, DS- vagy G sorozatú virtuális gépek InstanceSize értékeit, lásd: [virtuális gépek és Felhőszolgáltatások mérete az Azure-](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="55fa2-129">For the InstanceSize values for D-, DS-, or G-series virtual machines, see [Virtual Machine and Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span>

<span data-ttu-id="55fa2-130">Adja meg a felügyelt tartományra.</span><span class="sxs-lookup"><span data-stu-id="55fa2-130">Provide information about the managed domain.</span></span>

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

<span data-ttu-id="55fa2-131">Adja meg a felhőalapú szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="55fa2-131">Specify the name of the cloud service.</span></span>

    $svcname="Contoso100-test"

<span data-ttu-id="55fa2-132">Adja meg, amelyhez a virtuális Géphez kell csatlakoztatni a virtuális hálózat nevét.</span><span class="sxs-lookup"><span data-stu-id="55fa2-132">Specify the name of the virtual network to which the VM should be joined.</span></span> <span data-ttu-id="55fa2-133">Győződjön meg arról, hogy a felügyelt AAD-DS-tartomány érhető el a virtuális hálózaton található.</span><span class="sxs-lookup"><span data-stu-id="55fa2-133">Ensure that the AAD-DS managed domain is available in this virtual network.</span></span>

    $vnetname="MyPreviewVnet"

<span data-ttu-id="55fa2-134">Válassza ki a virtuális gép a virtuális gép kiépítéséhez használni kívánt kép.</span><span class="sxs-lookup"><span data-stu-id="55fa2-134">Select the VM image to be used to provision the VM.</span></span>

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

<span data-ttu-id="55fa2-135">Konfigurálja a VM - virtuális gép neve, a példány mérete és a használni kívánt kép.</span><span class="sxs-lookup"><span data-stu-id="55fa2-135">Configure the VM - set VM name, instance size & image to be used.</span></span>

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

<span data-ttu-id="55fa2-136">Szerezze be a helyi rendszergazdai hitelesítő adatokat a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="55fa2-136">Obtain local administrator credentials for the VM.</span></span> <span data-ttu-id="55fa2-137">Írjon be egy erős helyi rendszergazdai jelszót.</span><span class="sxs-lookup"><span data-stu-id="55fa2-137">Choose a strong local administrator password.</span></span>

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

<span data-ttu-id="55fa2-138">Szerezze be a virtuális gép csatlakoztatása a felügyelt tartományra "AAD DC rendszergazdák" csoportba tartozó felhasználói fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="55fa2-138">Obtain credentials for a user account belonging to 'AAD DC Administrators' group to join VM to the managed domain.</span></span> <span data-ttu-id="55fa2-139">Ne adjon meg a tartománynév - példányhoz, a fenti példában, "bob" adtuk meg a felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="55fa2-139">Do not specify the domain name - for instance, in our example, we specify 'bob' as the user name.</span></span>

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

<span data-ttu-id="55fa2-140">A virtuális gép konfigurálása, mert a tartományhoz csatlakoztatás követelmény & szükséges hitelesítő adatok megadása.</span><span class="sxs-lookup"><span data-stu-id="55fa2-140">Configure the VM - specify domain join requirement & required credentials.</span></span>

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

<span data-ttu-id="55fa2-141">Állítsa be egy alhálózatot a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="55fa2-141">Set a subnet for the VM.</span></span>

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

<span data-ttu-id="55fa2-142">Választható lehetőség: Az IP-cím a tartomány mutasson.</span><span class="sxs-lookup"><span data-stu-id="55fa2-142">Optional: Point to the IP address of the domain.</span></span> <span data-ttu-id="55fa2-143">Ha az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz kell lennie a virtuális hálózat DNS-kiszolgálók IP-címét, a lépésre nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="55fa2-143">If you set the IP addresses of the Azure AD Domain Services managed domain to be the DNS servers for the virtual network, this step is not required.</span></span>

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

<span data-ttu-id="55fa2-144">Most kiépítése a tartományhoz csatlakoztatott Windows virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="55fa2-144">Now, provision the domain-joined Windows VM.</span></span>

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a><span data-ttu-id="55fa2-145">Parancsfájl a Windows virtuális gépek ellátásához, majd automatikusan csatlakoztatása az AAD tartományi szolgáltatásokra által felügyelt tartományokhoz</span><span class="sxs-lookup"><span data-stu-id="55fa2-145">Script to provision a Windows VM and automatically join it to an AAD Domain Services managed domain</span></span>
<span data-ttu-id="55fa2-146">A PowerShell parancskészlethez létrehoz egy virtuális gépet egy üzleti kiszolgáló, amely:</span><span class="sxs-lookup"><span data-stu-id="55fa2-146">This PowerShell command set creates a virtual machine for a line-of-business server that:</span></span>

* <span data-ttu-id="55fa2-147">A Windows Server 2012 R2 Datacenter rendszerképet használja.</span><span class="sxs-lookup"><span data-stu-id="55fa2-147">Uses the Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="55fa2-148">Ez egy nagyon kicsi virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="55fa2-148">Is an extra small virtual machine.</span></span>
* <span data-ttu-id="55fa2-149">A Contoso100-teszt névvel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="55fa2-149">Has the name Contoso100-test.</span></span>
* <span data-ttu-id="55fa2-150">Automatikusan tartomány a contoso100 felügyelt tartományhoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="55fa2-150">Is automatically domain joined to the contoso100 managed domain.</span></span>
* <span data-ttu-id="55fa2-151">Bekerül a felügyelt tartományra azonos virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="55fa2-151">Is added to the same virtual network as the managed domain.</span></span>

<span data-ttu-id="55fa2-152">Ez a teljes minta parancsfájl a Windows rendszerű virtuális gép létrehozásához, és automatikusan csatlakoztatása az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="55fa2-152">Here is the full sample script to create the Windows virtual machine and automatically join it to the Azure AD Domain Services managed domain.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a><span data-ttu-id="55fa2-153">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="55fa2-153">Related Content</span></span>
* [<span data-ttu-id="55fa2-154">Azure AD tartományi szolgáltatások – első lépések útmutató</span><span class="sxs-lookup"><span data-stu-id="55fa2-154">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="55fa2-155">Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete</span><span class="sxs-lookup"><span data-stu-id="55fa2-155">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
