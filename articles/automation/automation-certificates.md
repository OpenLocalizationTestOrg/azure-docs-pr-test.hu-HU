---
title: "Az Azure Automationben eszközök tanúsítvány |} Microsoft Docs"
description: "Tanúsítványok tárolhatja biztonságosan Azure Automation, azok a runbookok vagy DSC-konfigurációk hitelesítése az Azure és a harmadik féltől származó erőforrások elérhetők.  Ez a cikk ismerteti a tanúsítványok és a szöveges és a grafikus szerzői őket munkavégzés részleteit."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: ac9c22ae-501f-42b9-9543-ac841cf2cc36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2016
ms.author: magoedte;bwren
ms.openlocfilehash: fd1737a420c132dace9307436bfea98a9bde94a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="certificate-assets-in-azure-automation"></a><span data-ttu-id="ad4f4-104">Azure Automation szolgáltatásbeli tanúsítvány eszközök</span><span class="sxs-lookup"><span data-stu-id="ad4f4-104">Certificate assets in Azure Automation</span></span>

<span data-ttu-id="ad4f4-105">Tanúsítványok tárolhatja biztonságosan Azure Automation, azok a runbookok vagy a DSC-konfigurációk használatával elérhetők a **Get-AzureRmAutomationRmCertificate** Azure Resource Manager erőforrások tevékenység.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-105">Certificates can be stored securely in Azure Automation so they can be accessed by runbooks or DSC configurations using the **Get-AzureRmAutomationRmCertificate** activity for Azure Resource Manager resources.</span></span> <span data-ttu-id="ad4f4-106">Ez lehetővé teszi a runbookok és a hitelesítési tanúsítványokat használó DSC-konfigurációk létrehozásához, vagy hozzáadja azokat az Azure vagy harmadik féltől származó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-106">This allows you to create runbooks and DSC configurations that use certificates for authentication or adds them to Azure or third party resources.</span></span>

> [!NOTE] 
> <span data-ttu-id="ad4f4-107">Az Azure Automationben biztonságos eszközök közé tartozik a hitelesítő adatokat, a tanúsítványokat, a kapcsolatok és a titkosított változók.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-107">Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables.</span></span> <span data-ttu-id="ad4f4-108">Ezek az eszközök titkosítva, és tárolja az Azure Automation létrehozott egyedi kulcs segítségével minden egyes automation-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-108">These assets are encrypted and stored in the Azure Automation using a unique key that is generated for each automation account.</span></span> <span data-ttu-id="ad4f4-109">Ezt a kulcsot egy mestertanúsítvány titkosítja és az Azure Automationben tárolja.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-109">This key is encrypted by a master certificate and stored in Azure Automation.</span></span> <span data-ttu-id="ad4f4-110">Előtt tárolása biztonságos eszköz, az automatizálási fiók kulcs visszafejtése a mestertanúsítvány, és majd az eszköz titkosításához használt.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-110">Before storing a secure asset, the key for the automation account is decrypted using the master certificate and then used to encrypt the asset.</span></span>
> 

## <a name="windows-powershell-cmdlets"></a><span data-ttu-id="ad4f4-111">Windows PowerShell-parancsmagjai</span><span class="sxs-lookup"><span data-stu-id="ad4f4-111">Windows PowerShell Cmdlets</span></span>

<span data-ttu-id="ad4f4-112">A következő táblázatban található parancsmagokkal létrehozása és kezelése az automation tanúsítvány eszközök a Windows PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-112">The cmdlets in the following table are used to create and manage automation certificate assets with Windows PowerShell.</span></span> <span data-ttu-id="ad4f4-113">Részét képezi a [Azure PowerShell modul](../powershell-install-configure.md) elérhető Automation-forgatókönyveket és a DSC-konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-113">They ship as part of the [Azure PowerShell module](../powershell-install-configure.md) which is available for use in Automation runbooks and DSC configurations.</span></span>

|<span data-ttu-id="ad4f4-114">Parancsmagok</span><span class="sxs-lookup"><span data-stu-id="ad4f4-114">Cmdlets</span></span>|<span data-ttu-id="ad4f4-115">Leírás</span><span class="sxs-lookup"><span data-stu-id="ad4f4-115">Description</span></span>|
|:---|:---|
|[<span data-ttu-id="ad4f4-116">Get-AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="ad4f4-116">Get-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603765.aspx)|<span data-ttu-id="ad4f4-117">Lekéri az információkat a runbookot vagy a DSC-konfiguráció használandó tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-117">Retrieves information about a certificate to use in a runbook or DSC configuration.</span></span> <span data-ttu-id="ad4f4-118">Maga a tanúsítvány csak Get-AutomationCertificate tevékenység kérhetnek le.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-118">You can only retrieve the certificate itself from Get-AutomationCertificate activity.</span></span>|
|[<span data-ttu-id="ad4f4-119">Új AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="ad4f4-119">New-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603604.aspx)|<span data-ttu-id="ad4f4-120">Létrehoz egy új tanúsítványt az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-120">Creates a new certificate into Azure Automation.</span></span>|
[<span data-ttu-id="ad4f4-121">Remove-AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="ad4f4-121">Remove-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603529.aspx)|<span data-ttu-id="ad4f4-122">Azure Automation szolgáltatásbeli tanúsítvány eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-122">Removes a certificate from Azure Automation.</span></span>|<span data-ttu-id="ad4f4-123">Létrehoz egy új tanúsítványt az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-123">Creates a new certificate into Azure Automation.</span></span>
|[<span data-ttu-id="ad4f4-124">Set-AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="ad4f4-124">Set-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603760.aspx)|<span data-ttu-id="ad4f4-125">Beleértve a tanúsítványfájl feltöltését és a jelszót a .pfx fájlhoz beállítás meglévő tanúsítvány tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-125">Sets the properties for an existing certificate including uploading the certificate file and setting the password for a .pfx.</span></span>|
|[<span data-ttu-id="ad4f4-126">Adja hozzá AzureCertificate</span><span class="sxs-lookup"><span data-stu-id="ad4f4-126">Add-AzureCertificate</span></span>](https://msdn.microsoft.com/library/azure/dn495214.aspx)|<span data-ttu-id="ad4f4-127">A megadott felhőszolgáltatás egy szolgáltatástanúsítványt feltöltését.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-127">Uploads a service certificate for the specified cloud service.</span></span>|


## <a name="creating-a-new-certificate"></a><span data-ttu-id="ad4f4-128">Egy új tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad4f4-128">Creating a new certificate</span></span>

<span data-ttu-id="ad4f4-129">Amikor létrehoz egy új tanúsítványt, .cer vagy .pfx-fájlt töltsön fel az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-129">When you create a new certificate, you upload a .cer or .pfx file to Azure Automation.</span></span> <span data-ttu-id="ad4f4-130">Ha a tanúsítvány exportálhatóként, majd azt át is az Azure Automation tanúsítványtároló kívül.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-130">If you mark the certificate as exportable, then you can transfer it out of the Azure Automation certificate store.</span></span> <span data-ttu-id="ad4f4-131">Ha nem exportálható, majd csak használat belül a runbookot vagy a DSC-konfiguráció az aláíráshoz.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-131">If it is not exportable, then it can only be used for signing within the runbook or DSC configuration.</span></span>


### <a name="to-create-a-new-certificate-with-the-azure-portal"></a><span data-ttu-id="ad4f4-132">Új tanúsítvány létrehozása az Azure portállal</span><span class="sxs-lookup"><span data-stu-id="ad4f4-132">To create a new certificate with the Azure portal</span></span>

1. <span data-ttu-id="ad4f4-133">Az Automation-fiók, kattintson a **eszközök** csempére kattintva nyissa meg a **eszközök** panelen.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-133">From your Automation account, click the **Assets** tile to open the **Assets** blade.</span></span>
1. <span data-ttu-id="ad4f4-134">Kattintson a **tanúsítványok** csempére kattintva nyissa meg a **tanúsítványok** panelen.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-134">Click the **Certificates** tile to open the **Certificates** blade.</span></span>
1. <span data-ttu-id="ad4f4-135">Kattintson a **tanúsítvány hozzáadása** a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-135">Click **Add a certificate** at the top of the blade.</span></span>
2. <span data-ttu-id="ad4f4-136">Adjon meg egy nevet a tanúsítványt a **neve** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-136">Type a name for the certificate in the **Name** box.</span></span>
2. <span data-ttu-id="ad4f4-137">Kattintson a **válasszon ki egy fájlt** alatt **tanúsítvány-fájl feltöltése** egy .cer vagy .pfx-fájl.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-137">Click **Select a file** under **Upload a certificate file** to browse for a .cer or .pfx file.</span></span>  <span data-ttu-id="ad4f4-138">Ha kiválaszt egy .pfx fájlt, adja meg a jelszót, és hogy azt engedélyezni kell exportálni.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-138">If you select a .pfx file, specify a password and whether it should be allowed to be exported.</span></span>
1. <span data-ttu-id="ad4f4-139">Kattintson a **létrehozása** menti az új tanúsítvány adategység.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-139">Click **Create** to save the new certificate asset.</span></span>


### <a name="to-create-a-new-certificate-with-windows-powershell"></a><span data-ttu-id="ad4f4-140">Új tanúsítvány létrehozása a Windows PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="ad4f4-140">To create a new certificate with Windows PowerShell</span></span>

<span data-ttu-id="ad4f4-141">A következő példa bemutatja, hogyan hozzon létre egy új Automation-tanúsítvány, és jelölje be a exportálható.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-141">The following example demonstrates how to create a new Automation certificate and mark it exportable.</span></span> <span data-ttu-id="ad4f4-142">Ez importálja a meglévő .pfx fájlt.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-142">This imports an existing .pfx file.</span></span>

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    $ResourceGroup = "ResourceGroup01"
    
    New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup

## <a name="using-a-certificate"></a><span data-ttu-id="ad4f4-143">Olyan tanúsítványt használ</span><span class="sxs-lookup"><span data-stu-id="ad4f4-143">Using a certificate</span></span>

<span data-ttu-id="ad4f4-144">Kell használnia a **Get-AutomationCertificate** tevékenység tanúsítvány használatára.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-144">You must use the **Get-AutomationCertificate** activity to use a certificate.</span></span> <span data-ttu-id="ad4f4-145">Nem használhatja a [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) parancsmag, mivel a tanúsítvány eszköz, de nem maga a tanúsítvány kapcsolatos információkat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-145">You cannot use the [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) cmdlet since it returns information about the certificate asset but not the certificate itself.</span></span>

### <a name="textual-runbook-sample"></a><span data-ttu-id="ad4f4-146">Szöveges forgatókönyvként minta</span><span class="sxs-lookup"><span data-stu-id="ad4f4-146">Textual runbook sample</span></span>

<span data-ttu-id="ad4f4-147">Az alábbi mintakód bemutatja, hogyan tanúsítvány hozzáadása a runbookban egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-147">The following sample code shows how to add a certificate to a cloud service in a runbook.</span></span> <span data-ttu-id="ad4f4-148">Ez a példa a jelszó lekért egy titkosított automation változó.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-148">In this sample, the password is retrieved from an encrypted automation variable.</span></span>

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a><span data-ttu-id="ad4f4-149">Grafikus forgatókönyv minta</span><span class="sxs-lookup"><span data-stu-id="ad4f4-149">Graphical runbook sample</span></span>

<span data-ttu-id="ad4f4-150">Hozzáadhat egy **Get-AutomationCertificate** egy grafikus forgatókönyvnek a tanúsítványt a könyvtár ablaktáblán grafikus szerkesztő csomagot jobb gombbal, majd válassza a **vászonra Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-150">You add a **Get-AutomationCertificate** to a graphical runbook by right-clicking on the certificate in the Library pane of the graphical editor and selecting **Add to canvas**.</span></span>

![Tanúsítvány felvétele a vászonra](media/automation-certificates/automation-certificate-add-to-canvas.png)

<span data-ttu-id="ad4f4-152">Az alábbi ábrán például egy grafikus runbookban levő olyan tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-152">The following image shows an example of using a certificate in a graphical runbook.</span></span>  <span data-ttu-id="ad4f4-153">Ez az ugyanebben a példában egy felhőalapú szolgáltatás szöveges runbookból egy tanúsítvány felvétele a fent látható.</span><span class="sxs-lookup"><span data-stu-id="ad4f4-153">This is the same example shown above for adding a certificate to a cloud service from a textual runbook.</span></span>

![<span data-ttu-id="ad4f4-154">Példa grafikus készítése</span><span class="sxs-lookup"><span data-stu-id="ad4f4-154">Example Graphical Authoring</span></span> ](media/automation-certificates/graphical-runbook-add-certificate.png)


## <a name="next-steps"></a><span data-ttu-id="ad4f4-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ad4f4-155">Next steps</span></span>

- <span data-ttu-id="ad4f4-156">A tevékenységek a runbookban az célja, hogy végre logikai üzenetáramlásának szabályozására hivatkozások használata kapcsolatos további információkért lásd: [hivatkozások grafikus szerzői](automation-graphical-authoring-intro.md#links-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="ad4f4-156">To learn more about working with links to control the logical flow of activities your runbook is designed to perform, see [Links in graphical authoring](automation-graphical-authoring-intro.md#links-and-workflow).</span></span> 
