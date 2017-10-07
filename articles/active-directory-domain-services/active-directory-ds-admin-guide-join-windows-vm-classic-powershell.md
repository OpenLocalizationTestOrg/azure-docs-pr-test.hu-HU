---
title: "Az Azure Active Directory tartományi szolgáltatások: Felügyeleti útmutató |} Microsoft Docs"
description: "Csatlakozás a Windows virtuális gép tooa felügyelt tartományhoz Azure PowerShell és a hello klasszikus telepítési modell használatával."
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
ms.openlocfilehash: 21bc5930d84c5368a120f9d81f09320e2a0168fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-windows-server-virtual-machine-tooa-managed-domain-using-powershell"></a><span data-ttu-id="2a50b-103">Csatlakozás a Windows Server virtuális gép tooa felügyelt tartományhoz PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2a50b-103">Join a Windows Server virtual machine tooa managed domain using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2a50b-104">Klasszikus Azure portál – Windows</span><span class="sxs-lookup"><span data-stu-id="2a50b-104">Azure classic portal - Windows</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
> * [<span data-ttu-id="2a50b-105">PowerShell - Windows</span><span class="sxs-lookup"><span data-stu-id="2a50b-105">PowerShell - Windows</span></span>](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> <span data-ttu-id="2a50b-106">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="2a50b-106">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="2a50b-107">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="2a50b-107">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="2a50b-108">Azure AD tartományi szolgáltatások jelenleg nem támogatja a hello Resource Manager modellt.</span><span class="sxs-lookup"><span data-stu-id="2a50b-108">Azure AD Domain Services does not currently support hello Resource Manager model.</span></span>
>
>

<span data-ttu-id="2a50b-109">Ezen lépések bemutatják, hogyan toocustomize Azure PowerShell készlete parancsok, amelyek létrehozása, és előre konfigurálása a Windows-alapú Azure virtuális gép egy építőelem módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="2a50b-109">These steps show you how toocustomize a set of Azure PowerShell commands that create and preconfigure a Windows-based Azure virtual machine by using a building block approach.</span></span> <span data-ttu-id="2a50b-110">Az alábbi lépések segítségével egy Windows-alapú Azure virtuális gép létrehozása és tooan Azure AD tartományi szolgáltatások által kezelt tartomány csatlakozzon hozzá.</span><span class="sxs-lookup"><span data-stu-id="2a50b-110">These steps help you build a Windows-based Azure virtual machine and join it tooan Azure AD Domain Services managed domain.</span></span>

<span data-ttu-id="2a50b-111">Ezeket a lépéseket hajtsa végre az Azure PowerShell-parancs készletek létrehozásához a fill-a-a-üres cellákat megközelítést.</span><span class="sxs-lookup"><span data-stu-id="2a50b-111">These steps follow a fill-in-the-blanks approach for creating Azure PowerShell command sets.</span></span> <span data-ttu-id="2a50b-112">Ezt a módszert akkor lehet hasznos, ha új tooPowerShell vagy kívánt tooknow milyen értékeket toospecify sikeres konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="2a50b-112">This approach can be useful if you are new tooPowerShell or you want tooknow what values toospecify for successful configuration.</span></span> <span data-ttu-id="2a50b-113">Speciális PowerShell felhasználók hello parancsok igénybe, és a hello változók (hello sorok kezdve a "$") a saját értékeit helyettesítse.</span><span class="sxs-lookup"><span data-stu-id="2a50b-113">Advanced PowerShell users can take hello commands and substitute their own values for hello variables (hello lines beginning with "$").</span></span>

<span data-ttu-id="2a50b-114">Ha még nem tette meg, használja a hello utasításait [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) tooinstall Azure PowerShell, a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2a50b-114">If you haven't done so already, use hello instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) tooinstall Azure PowerShell on your local computer.</span></span> <span data-ttu-id="2a50b-115">Ezután nyissa meg a Windows PowerShell-parancssort.</span><span class="sxs-lookup"><span data-stu-id="2a50b-115">Then, open a Windows PowerShell command prompt.</span></span>

## <a name="step-1-add-your-account"></a><span data-ttu-id="2a50b-116">1. lépés: A fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2a50b-116">Step 1: Add your account</span></span>
1. <span data-ttu-id="2a50b-117">Hello PowerShell parancssorába írja be a **Add-AzureAccount** kattintson **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2a50b-117">At hello PowerShell prompt, type **Add-AzureAccount** and click **Enter**.</span></span>
2. <span data-ttu-id="2a50b-118">Írja be az Azure-előfizetéshez társított hello e-mail címét, és kattintson a **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="2a50b-118">Type in hello email address associated with your Azure subscription and click **Continue**.</span></span>
3. <span data-ttu-id="2a50b-119">Adja meg a fiókhoz tartozó jelszót hello.</span><span class="sxs-lookup"><span data-stu-id="2a50b-119">Type in hello password for your account.</span></span>
4. <span data-ttu-id="2a50b-120">Kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2a50b-120">Click **Sign in**.</span></span>

## <a name="step-2-set-your-subscription-and-storage-account"></a><span data-ttu-id="2a50b-121">2. lépés: Az előfizetés és a storage-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="2a50b-121">Step 2: Set your subscription and storage account</span></span>
<span data-ttu-id="2a50b-122">Ezek a parancsok futtatásával hello Windows PowerShell parancssorába állítsa be a Azure-előfizetés és a storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="2a50b-122">Set your Azure subscription and storage account by running these commands at hello Windows PowerShell command prompt.</span></span> <span data-ttu-id="2a50b-123">Cserélje le mindent, ami hello idézőjelek között, beleértve a hello < és > karakter, helyes nevek hello.</span><span class="sxs-lookup"><span data-stu-id="2a50b-123">Replace everything within hello quotes, including hello < and > characters, with hello correct names.</span></span>

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

<span data-ttu-id="2a50b-124">Hello megfelelő előfizetés neve letölthető hello hello hello kimenete SubscriptionName tulajdonságának **Get-AzureSubscription** parancsot.</span><span class="sxs-lookup"><span data-stu-id="2a50b-124">You can get hello correct subscription name from hello SubscriptionName property of hello output of hello **Get-AzureSubscription** command.</span></span> <span data-ttu-id="2a50b-125">Hello helyes-e a tárfiók nevének beolvasása hello hello hello kimenete Label tulajdonságának **Get-AzureStorageAccount** parancsot, miután lefuttatta a hello **válasszon-AzureSubscription** parancsot.</span><span class="sxs-lookup"><span data-stu-id="2a50b-125">You can get hello correct storage account name from hello Label property of hello output of hello **Get-AzureStorageAccount** command after you run hello **Select-AzureSubscription** command.</span></span>

## <a name="step-3-step-by-step-walkthrough---provision-hello-virtual-machine-and-join-it-toohello-managed-domain"></a><span data-ttu-id="2a50b-126">3. lépés: Részletes útmutató - rendszerű hello virtuális gép, és hozzá toohello által felügyelt tartományokhoz</span><span class="sxs-lookup"><span data-stu-id="2a50b-126">Step 3: Step-by-step walkthrough - provision hello virtual machine and join it toohello managed domain</span></span>
<span data-ttu-id="2a50b-127">Ez hello megfelelő Azure PowerShell paranccsal állítsa toocreate a virtuális gépre, az üres sorok közötti valamennyi blokkja olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="2a50b-127">Here is hello corresponding Azure PowerShell command set toocreate this virtual machine, with blank lines between each block for readability.</span></span>

<span data-ttu-id="2a50b-128">Adjon meg információt hello Windows virtuális gép toobe kiépítve.</span><span class="sxs-lookup"><span data-stu-id="2a50b-128">Specify information about hello Windows virtual machine toobe provisioned.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

<span data-ttu-id="2a50b-129">Hello InstanceSize D-, DS- vagy értékeit G sorozatú virtuális gépek, lásd: [virtuális gépek és Felhőszolgáltatások mérete az Azure-](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="2a50b-129">For hello InstanceSize values for D-, DS-, or G-series virtual machines, see [Virtual Machine and Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span>

<span data-ttu-id="2a50b-130">Hello által kezelt tartomány ismertetik.</span><span class="sxs-lookup"><span data-stu-id="2a50b-130">Provide information about hello managed domain.</span></span>

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

<span data-ttu-id="2a50b-131">Adja meg a felhőalapú szolgáltatás hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="2a50b-131">Specify hello name of hello cloud service.</span></span>

    $svcname="Contoso100-test"

<span data-ttu-id="2a50b-132">Adja meg hello hello virtuális hálózati toowhich hello kell csatlakoztatni a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="2a50b-132">Specify hello name of hello virtual network toowhich hello VM should be joined.</span></span> <span data-ttu-id="2a50b-133">Győződjön meg arról, hogy hello felügyelt AAD-DS-tartomány érhető el a virtuális hálózaton található.</span><span class="sxs-lookup"><span data-stu-id="2a50b-133">Ensure that hello AAD-DS managed domain is available in this virtual network.</span></span>

    $vnetname="MyPreviewVnet"

<span data-ttu-id="2a50b-134">Válassza ki a virtuális gép lemezképének használt toobe tooprovision hello VM hello.</span><span class="sxs-lookup"><span data-stu-id="2a50b-134">Select hello VM image toobe used tooprovision hello VM.</span></span>

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

<span data-ttu-id="2a50b-135">Konfigurálja a VM - VM neve, hello használt példány méret és a lemezkép toobe.</span><span class="sxs-lookup"><span data-stu-id="2a50b-135">Configure hello VM - set VM name, instance size & image toobe used.</span></span>

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

<span data-ttu-id="2a50b-136">Szerezze be a virtuális gép hello helyi rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="2a50b-136">Obtain local administrator credentials for hello VM.</span></span> <span data-ttu-id="2a50b-137">Írjon be egy erős helyi rendszergazdai jelszót.</span><span class="sxs-lookup"><span data-stu-id="2a50b-137">Choose a strong local administrator password.</span></span>

    $localadmincred=Get-Credential –Message "Type hello name and password of hello local administrator account."

<span data-ttu-id="2a50b-138">Szerezze be tartozó too'AAD DC rendszergazdák csoport toojoin VM toohello kezelt tartományi felhasználói fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="2a50b-138">Obtain credentials for a user account belonging too'AAD DC Administrators' group toojoin VM toohello managed domain.</span></span> <span data-ttu-id="2a50b-139">Ne adjon hello tartománynév - példány, a jelen példában adja meg, hogy "Belinszky" hello felhasználónévként.</span><span class="sxs-lookup"><span data-stu-id="2a50b-139">Do not specify hello domain name - for instance, in our example, we specify 'bob' as hello user name.</span></span>

    $domainadmincred=Get-Credential –Message "Now type hello name (DO NOT INCLUDE hello DOMAIN) and password of an account in hello AAD DC Administrators group, that has permission tooadd hello machine toohello domain."

<span data-ttu-id="2a50b-140">Konfigurálja a virtuális gép hello – adja meg a tartományhoz csatlakoztatás követelmény & szükséges hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="2a50b-140">Configure hello VM - specify domain join requirement & required credentials.</span></span>

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

<span data-ttu-id="2a50b-141">Állítsa be a hello Virtuálisgép-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="2a50b-141">Set a subnet for hello VM.</span></span>

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

<span data-ttu-id="2a50b-142">Választható lehetőség: Pont toohello IP-cím hello tartomány.</span><span class="sxs-lookup"><span data-stu-id="2a50b-142">Optional: Point toohello IP address of hello domain.</span></span> <span data-ttu-id="2a50b-143">Ha IP-címei hello hello Azure AD tartományi szolgáltatások által kezelt tartomány toobe hello hello virtuális hálózat DNS-kiszolgálók, a lépésre nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="2a50b-143">If you set hello IP addresses of hello Azure AD Domain Services managed domain toobe hello DNS servers for hello virtual network, this step is not required.</span></span>

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

<span data-ttu-id="2a50b-144">Most kiépítés hello tartományhoz csatlakozó windowsos virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2a50b-144">Now, provision hello domain-joined Windows VM.</span></span>

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-tooprovision-a-windows-vm-and-automatically-join-it-tooan-aad-domain-services-managed-domain"></a><span data-ttu-id="2a50b-145">Parancsfájl-tooprovision egy Windows virtuális Gépet, és automatikusan hozzá tooan AAD tartományi szolgáltatásokra által felügyelt tartományokhoz</span><span class="sxs-lookup"><span data-stu-id="2a50b-145">Script tooprovision a Windows VM and automatically join it tooan AAD Domain Services managed domain</span></span>
<span data-ttu-id="2a50b-146">A PowerShell parancskészlethez létrehoz egy virtuális gépet egy üzleti kiszolgáló, amely:</span><span class="sxs-lookup"><span data-stu-id="2a50b-146">This PowerShell command set creates a virtual machine for a line-of-business server that:</span></span>

* <span data-ttu-id="2a50b-147">Hello Windows Server 2012 R2 Datacenter rendszerképet használja.</span><span class="sxs-lookup"><span data-stu-id="2a50b-147">Uses hello Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="2a50b-148">Ez egy nagyon kicsi virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2a50b-148">Is an extra small virtual machine.</span></span>
* <span data-ttu-id="2a50b-149">Contoso100-teszt hello névvel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2a50b-149">Has hello name Contoso100-test.</span></span>
* <span data-ttu-id="2a50b-150">A rendszer automatikusan tartományhoz csatlakoztatott toohello contoso100 által felügyelt tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="2a50b-150">Is automatically domain joined toohello contoso100 managed domain.</span></span>
* <span data-ttu-id="2a50b-151">Toohello hozzáadott hello megegyező virtuális hálózatban felügyelt tartomány.</span><span class="sxs-lookup"><span data-stu-id="2a50b-151">Is added toohello same virtual network as hello managed domain.</span></span>

<span data-ttu-id="2a50b-152">Ez a teljes minta parancsprogram toocreate hello Windows rendszerű virtuális gép hello, és automatikusan hozzá toohello Azure AD tartományi szolgáltatások által felügyelt tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="2a50b-152">Here is hello full sample script toocreate hello Windows virtual machine and automatically join it toohello Azure AD Domain Services managed domain.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type hello name and password of hello local administrator account."

    $domainadmincred=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account in hello AAD DC Administrators group, that has permission tooadd hello machine toohello domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a><span data-ttu-id="2a50b-153">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="2a50b-153">Related Content</span></span>
* [<span data-ttu-id="2a50b-154">Azure AD tartományi szolgáltatások – első lépések útmutató</span><span class="sxs-lookup"><span data-stu-id="2a50b-154">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="2a50b-155">Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete</span><span class="sxs-lookup"><span data-stu-id="2a50b-155">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
