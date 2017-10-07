---
title: "az Azure Automationben aaaCertificate eszközök |} Microsoft Docs"
description: "Tanúsítványok tárolhatja biztonságosan Azure Automation, a runbookot vagy a DSC-konfigurációk tooauthenticate Azure és a harmadik féltől származó erőforrások elleni hozzáférhetők.  Ez a cikk ismerteti a tanúsítvány hello adatait, és hogyan toowork velük a szöveges és a grafikus szerzői."
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
ms.openlocfilehash: 2c25bee937890438ea9022669be2c24c77a110b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="certificate-assets-in-azure-automation"></a><span data-ttu-id="29528-104">Azure Automation szolgáltatásbeli tanúsítvány eszközök</span><span class="sxs-lookup"><span data-stu-id="29528-104">Certificate assets in Azure Automation</span></span>

<span data-ttu-id="29528-105">Tanúsítványok tárolhatja biztonságosan Azure Automation, azok a runbookok vagy hello segítségével a DSC-konfigurációk elérhetők **Get-AzureRmAutomationRmCertificate** Azure Resource Manager erőforrások tevékenység.</span><span class="sxs-lookup"><span data-stu-id="29528-105">Certificates can be stored securely in Azure Automation so they can be accessed by runbooks or DSC configurations using hello **Get-AzureRmAutomationRmCertificate** activity for Azure Resource Manager resources.</span></span> <span data-ttu-id="29528-106">Ez lehetővé teszi az toocreate runbookok és a DSC-konfigurációk, amelyek tanúsítványokat használnak a hitelesítéshez, vagy azokat beveszi tooAzure vagy harmadik féltől származó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="29528-106">This allows you toocreate runbooks and DSC configurations that use certificates for authentication or adds them tooAzure or third party resources.</span></span>

> [!NOTE] 
> <span data-ttu-id="29528-107">Az Azure Automationben biztonságos eszközök közé tartozik a hitelesítő adatokat, a tanúsítványokat, a kapcsolatok és a titkosított változók.</span><span class="sxs-lookup"><span data-stu-id="29528-107">Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables.</span></span> <span data-ttu-id="29528-108">Ezek az eszközök titkosítottak és a tárolt hello Azure Automation létrehozott egyedi kulcs segítségével minden egyes automation-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="29528-108">These assets are encrypted and stored in hello Azure Automation using a unique key that is generated for each automation account.</span></span> <span data-ttu-id="29528-109">Ezt a kulcsot egy mestertanúsítvány titkosítja és az Azure Automationben tárolja.</span><span class="sxs-lookup"><span data-stu-id="29528-109">This key is encrypted by a master certificate and stored in Azure Automation.</span></span> <span data-ttu-id="29528-110">Tárolása biztonságos eszköz, mielőtt hello kulcs hello automation-fiók visszafejtése hello fő tanúsítványt használ, és akkor használja a tooencrypt hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="29528-110">Before storing a secure asset, hello key for hello automation account is decrypted using hello master certificate and then used tooencrypt hello asset.</span></span>
> 

## <a name="windows-powershell-cmdlets"></a><span data-ttu-id="29528-111">Windows PowerShell-parancsmagjai</span><span class="sxs-lookup"><span data-stu-id="29528-111">Windows PowerShell Cmdlets</span></span>

<span data-ttu-id="29528-112">hello parancsmagok a következő táblázat hello használt toocreate és a Windows PowerShell-lel automation-tanúsítvány eszközök kezelése.</span><span class="sxs-lookup"><span data-stu-id="29528-112">hello cmdlets in hello following table are used toocreate and manage automation certificate assets with Windows PowerShell.</span></span> <span data-ttu-id="29528-113">Ezek hello részét képezi [Azure PowerShell modul](../powershell-install-configure.md) elérhető Automation-forgatókönyveket és a DSC-konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="29528-113">They ship as part of hello [Azure PowerShell module](../powershell-install-configure.md) which is available for use in Automation runbooks and DSC configurations.</span></span>

|<span data-ttu-id="29528-114">Parancsmagok</span><span class="sxs-lookup"><span data-stu-id="29528-114">Cmdlets</span></span>|<span data-ttu-id="29528-115">Leírás</span><span class="sxs-lookup"><span data-stu-id="29528-115">Description</span></span>|
|:---|:---|
|[<span data-ttu-id="29528-116">Get-AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="29528-116">Get-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603765.aspx)|<span data-ttu-id="29528-117">Lekéri a runbookot vagy a DSC-konfiguráció egy tanúsítvány toouse kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="29528-117">Retrieves information about a certificate toouse in a runbook or DSC configuration.</span></span> <span data-ttu-id="29528-118">Maga hello tanúsítvány csak Get-AutomationCertificate tevékenység kérhetnek le.</span><span class="sxs-lookup"><span data-stu-id="29528-118">You can only retrieve hello certificate itself from Get-AutomationCertificate activity.</span></span>|
|[<span data-ttu-id="29528-119">Új AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="29528-119">New-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603604.aspx)|<span data-ttu-id="29528-120">Létrehoz egy új tanúsítványt az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="29528-120">Creates a new certificate into Azure Automation.</span></span>|
[<span data-ttu-id="29528-121">Remove-AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="29528-121">Remove-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603529.aspx)|<span data-ttu-id="29528-122">Azure Automation szolgáltatásbeli tanúsítvány eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="29528-122">Removes a certificate from Azure Automation.</span></span>|<span data-ttu-id="29528-123">Létrehoz egy új tanúsítványt az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="29528-123">Creates a new certificate into Azure Automation.</span></span>
|[<span data-ttu-id="29528-124">Set-AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="29528-124">Set-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603760.aspx)|<span data-ttu-id="29528-125">Beállítja egy létező tanúsítványt, beleértve a tanúsítványfájl hello és beállítás hello jelszót a .pfx fájlhoz feltöltése hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="29528-125">Sets hello properties for an existing certificate including uploading hello certificate file and setting hello password for a .pfx.</span></span>|
|[<span data-ttu-id="29528-126">Adja hozzá AzureCertificate</span><span class="sxs-lookup"><span data-stu-id="29528-126">Add-AzureCertificate</span></span>](https://msdn.microsoft.com/library/azure/dn495214.aspx)|<span data-ttu-id="29528-127">Egy szolgáltatástanúsítványa hello feltöltések megadott felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="29528-127">Uploads a service certificate for hello specified cloud service.</span></span>|


## <a name="creating-a-new-certificate"></a><span data-ttu-id="29528-128">Egy új tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="29528-128">Creating a new certificate</span></span>

<span data-ttu-id="29528-129">Amikor létrehoz egy új tanúsítványt, egy .cer vagy .pfx fájlt tooAzure Automation töltse fel.</span><span class="sxs-lookup"><span data-stu-id="29528-129">When you create a new certificate, you upload a .cer or .pfx file tooAzure Automation.</span></span> <span data-ttu-id="29528-130">Ha exportálható hello tanúsítványok, majd azt át is hello Azure Automation-tanúsítványtároló kívül.</span><span class="sxs-lookup"><span data-stu-id="29528-130">If you mark hello certificate as exportable, then you can transfer it out of hello Azure Automation certificate store.</span></span> <span data-ttu-id="29528-131">Ha nem exportálható, majd csak használat belül hello runbookot vagy a DSC-konfiguráció az aláíráshoz.</span><span class="sxs-lookup"><span data-stu-id="29528-131">If it is not exportable, then it can only be used for signing within hello runbook or DSC configuration.</span></span>


### <a name="toocreate-a-new-certificate-with-hello-azure-portal"></a><span data-ttu-id="29528-132">toocreate egy új tanúsítványt hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="29528-132">toocreate a new certificate with hello Azure portal</span></span>

1. <span data-ttu-id="29528-133">Kattintson az Automation-fiók hello **eszközök** csempe tooopen hello **eszközök** panelen.</span><span class="sxs-lookup"><span data-stu-id="29528-133">From your Automation account, click hello **Assets** tile tooopen hello **Assets** blade.</span></span>
1. <span data-ttu-id="29528-134">Kattintson a hello **tanúsítványok** csempe tooopen hello **tanúsítványok** panelen.</span><span class="sxs-lookup"><span data-stu-id="29528-134">Click hello **Certificates** tile tooopen hello **Certificates** blade.</span></span>
1. <span data-ttu-id="29528-135">Kattintson a **tanúsítvány hozzáadása** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="29528-135">Click **Add a certificate** at hello top of hello blade.</span></span>
2. <span data-ttu-id="29528-136">Írja be egy nevet a tanúsítványnak hello hello **neve** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="29528-136">Type a name for hello certificate in hello **Name** box.</span></span>
2. <span data-ttu-id="29528-137">Kattintson a **válasszon ki egy fájlt** alatt **tanúsítvány-fájl feltöltése** toobrowse egy .cer vagy .pfx-fájl.</span><span class="sxs-lookup"><span data-stu-id="29528-137">Click **Select a file** under **Upload a certificate file** toobrowse for a .cer or .pfx file.</span></span>  <span data-ttu-id="29528-138">Ha kiválaszt egy .pfx fájlt, adja meg a jelszót, és hogy azt engedélyezni kell a toobe exportált.</span><span class="sxs-lookup"><span data-stu-id="29528-138">If you select a .pfx file, specify a password and whether it should be allowed toobe exported.</span></span>
1. <span data-ttu-id="29528-139">Kattintson a **létrehozása** toosave hello új tanúsítvány adategység.</span><span class="sxs-lookup"><span data-stu-id="29528-139">Click **Create** toosave hello new certificate asset.</span></span>


### <a name="toocreate-a-new-certificate-with-windows-powershell"></a><span data-ttu-id="29528-140">a Windows PowerShell használatával új tanúsítványt toocreate</span><span class="sxs-lookup"><span data-stu-id="29528-140">toocreate a new certificate with Windows PowerShell</span></span>

<span data-ttu-id="29528-141">hello következő példa bemutatja, hogyan toocreate egy új Automation tanúsítványt, és jelölje meg exportálható.</span><span class="sxs-lookup"><span data-stu-id="29528-141">hello following example demonstrates how toocreate a new Automation certificate and mark it exportable.</span></span> <span data-ttu-id="29528-142">Ez importálja a meglévő .pfx fájlt.</span><span class="sxs-lookup"><span data-stu-id="29528-142">This imports an existing .pfx file.</span></span>

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    $ResourceGroup = "ResourceGroup01"
    
    New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup

## <a name="using-a-certificate"></a><span data-ttu-id="29528-143">Olyan tanúsítványt használ</span><span class="sxs-lookup"><span data-stu-id="29528-143">Using a certificate</span></span>

<span data-ttu-id="29528-144">Hello kell használnia **Get-AutomationCertificate** tevékenység toouse egy tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="29528-144">You must use hello **Get-AutomationCertificate** activity toouse a certificate.</span></span> <span data-ttu-id="29528-145">Nem használhat hello [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) parancsmag óta hello tanúsítványeszköz, de nem maga hello tanúsítvány kapcsolatos információkat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="29528-145">You cannot use hello [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) cmdlet since it returns information about hello certificate asset but not hello certificate itself.</span></span>

### <a name="textual-runbook-sample"></a><span data-ttu-id="29528-146">Szöveges forgatókönyvként minta</span><span class="sxs-lookup"><span data-stu-id="29528-146">Textual runbook sample</span></span>

<span data-ttu-id="29528-147">a következő példakód hello bemutatja, hogyan tooadd egy tanúsítvány tooa felhőszolgáltatás egy runbookban.</span><span class="sxs-lookup"><span data-stu-id="29528-147">hello following sample code shows how tooadd a certificate tooa cloud service in a runbook.</span></span> <span data-ttu-id="29528-148">Ez a példa hello jelszó lekért egy titkosított automation változó.</span><span class="sxs-lookup"><span data-stu-id="29528-148">In this sample, hello password is retrieved from an encrypted automation variable.</span></span>

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a><span data-ttu-id="29528-149">Grafikus forgatókönyv minta</span><span class="sxs-lookup"><span data-stu-id="29528-149">Graphical runbook sample</span></span>

<span data-ttu-id="29528-150">Hozzáadhat egy **Get-AutomationCertificate** tooa grafikus forgatókönyvnek hello tanúsítványa hello grafikus szerkesztőben, majd válassza a hello könyvtár ablaktáblán kattintson a jobb gombbal **toocanvas hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="29528-150">You add a **Get-AutomationCertificate** tooa graphical runbook by right-clicking on hello certificate in hello Library pane of hello graphical editor and selecting **Add toocanvas**.</span></span>

![Vegye fel a tanúsítvány toohello vászonra](media/automation-certificates/automation-certificate-add-to-canvas.png)

<span data-ttu-id="29528-152">hello következő kép bemutatja egy olyan tanúsítványt használ az olyan grafikus forgatókönyvnek példát.</span><span class="sxs-lookup"><span data-stu-id="29528-152">hello following image shows an example of using a certificate in a graphical runbook.</span></span>  <span data-ttu-id="29528-153">Ez a hello ugyanebben a példában a tanúsítvány tooa felhőszolgáltatás hozzáadásához szöveges forgatókönyvként a fent látható.</span><span class="sxs-lookup"><span data-stu-id="29528-153">This is hello same example shown above for adding a certificate tooa cloud service from a textual runbook.</span></span>

![<span data-ttu-id="29528-154">Példa grafikus készítése</span><span class="sxs-lookup"><span data-stu-id="29528-154">Example Graphical Authoring</span></span> ](media/automation-certificates/graphical-runbook-add-certificate.png)


## <a name="next-steps"></a><span data-ttu-id="29528-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29528-155">Next steps</span></span>

- <span data-ttu-id="29528-156">legalább hivatkozások toocontrol hello tételéig tevékenységek a runbookban használatáról toolearn tervezett tooperform című [hivatkozások grafikus szerzői](automation-graphical-authoring-intro.md#links-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="29528-156">toolearn more about working with links toocontrol hello logical flow of activities your runbook is designed tooperform, see [Links in graphical authoring](automation-graphical-authoring-intro.md#links-and-workflow).</span></span> 
