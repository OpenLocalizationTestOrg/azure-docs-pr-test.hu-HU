---
title: "távoli hibakeresés folyamatos kézbesítésével aaaEnable |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable távoli hibakeresés folyamatos kézbesítési toodeploy tooAzure használata esetén"
services: cloud-services
documentationcenter: .net
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7d423639-3b2f-4ca5-ac5a-9ac19a217c29
ms.service: cloud-services
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: d9d9d1cfe5304c9526586a9164f172746a448e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-debugging-when-using-continuous-delivery-toopublish-tooazure"></a><span data-ttu-id="ca4a9-103">Folyamatos kézbesítési toopublish tooAzure használatakor távoli hibakeresésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ca4a9-103">Enable remote debugging when using continuous delivery toopublish tooAzure</span></span>
<span data-ttu-id="ca4a9-104">Engedélyezheti a távoli hibakeresés Azure felhőszolgáltatások vagy a virtuális gépek használatakor [folyamatos kézbesítési](cloud-services-dotnet-continuous-delivery.md) toopublish tooAzure ezeket a lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-104">You can enable remote debugging in Azure, for cloud services or virtual machines, when you use [continuous delivery](cloud-services-dotnet-continuous-delivery.md) toopublish tooAzure by following these steps.</span></span>

## <a name="enabling-remote-debugging-for-cloud-services"></a><span data-ttu-id="ca4a9-105">A felhőszolgáltatások távoli hibakeresés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ca4a9-105">Enabling remote debugging for cloud services</span></span>
1. <span data-ttu-id="ca4a9-106">Hello build ügynökön hello kezdeti környezet beállítása az Azure-bA a [parancssori létrehozása az Azure-](http://msdn.microsoft.com/library/hh535755.aspx).</span><span class="sxs-lookup"><span data-stu-id="ca4a9-106">On hello build agent, set up hello initial environment for Azure as outlined in [Command-Line Build for Azure](http://msdn.microsoft.com/library/hh535755.aspx).</span></span>
2. <span data-ttu-id="ca4a9-107">Mivel hello távoli hibakeresési futásidejű (msvsmon.exe) hello csomag szükséges, telepítse a hello **távoli Tools for Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-107">Because hello remote debug runtime (msvsmon.exe) is required for hello package, install hello **Remote Tools for Visual Studio**.</span></span>

    * [<span data-ttu-id="ca4a9-108">A Visual Studio 2017 távoli eszközök</span><span class="sxs-lookup"><span data-stu-id="ca4a9-108">Remote Tools for Visual Studio 2017</span></span>](https://go.microsoft.com/fwlink/?LinkId=746570)
    * [<span data-ttu-id="ca4a9-109">A Visual Studio 2015 távoli eszközök</span><span class="sxs-lookup"><span data-stu-id="ca4a9-109">Remote Tools for Visual Studio 2015</span></span>](https://go.microsoft.com/fwlink/?LinkId=615470)
    * [<span data-ttu-id="ca4a9-110">Távoli Tools for Visual Studio 2013 Update 5</span><span class="sxs-lookup"><span data-stu-id="ca4a9-110">Remote Tools for Visual Studio 2013 Update 5</span></span>](https://www.microsoft.com/download/details.aspx?id=48156)
    
    <span data-ttu-id="ca4a9-111">Alternatív megoldásként másolhatja a rendszer, amely rendelkezik a telepített Visual Studio hello távoli hibakeresési bináris fájlok fordítását.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-111">As an alternative, you can copy hello remote debug binaries from a system that has Visual Studio installed.</span></span>

3. <span data-ttu-id="ca4a9-112">Hozzon létre egy tanúsítványt, a [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="ca4a9-112">Create a certificate as outlined in [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span> <span data-ttu-id="ca4a9-113">Hello .pfx és RDP tanúsítvány ujjlenyomatát, és töltse fel a hello tanúsítvány toohello cél felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-113">Keep hello .pfx and RDP certificate thumbprint and upload hello certificate toohello target cloud service.</span></span>
4. <span data-ttu-id="ca4a9-114">Használja az alábbi beállítások hello MSBuild parancssori toobuild és a csomag engedélyezett távoli hibakereséssel hello.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-114">Use hello following options in hello MSBuild command line toobuild and package with remote debug enabled.</span></span> <span data-ttu-id="ca4a9-115">(Útvonalakat tooyour rendszer és a projekt hello szög zárójeles elemek fájlok helyettesítse.)</span><span class="sxs-lookup"><span data-stu-id="ca4a9-115">(Substitute actual paths tooyour system and project files for hello angle-bracketed items.)</span></span>
   
        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of hello certificate added toohello cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path tooyour VS solution file>"
   
    <span data-ttu-id="ca4a9-116">`VSX64RemoteDebuggerPath`hello elérési toohello mappa van a Visual Studio tartalmazó msvsmon.exe hello távoli eszközei.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-116">`VSX64RemoteDebuggerPath` is hello path toohello folder containing msvsmon.exe in hello Remote Tools for Visual Studio.</span></span>
    <span data-ttu-id="ca4a9-117">`RemoteDebuggerConnectorVersion`a felhőszolgáltatásban hello Azure SDK-verzió van.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-117">`RemoteDebuggerConnectorVersion` is hello Azure SDK version in your cloud service.</span></span> <span data-ttu-id="ca4a9-118">Azt is illeszkednie kell hello verziója van telepítve, a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-118">It should also match hello version installed with Visual Studio.</span></span>
5. <span data-ttu-id="ca4a9-119">Hello előző lépésben létrehozott hello csomag és a .cscfg fájl használatával toohello cél felhőalapú szolgáltatást teszik közzé.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-119">Publish toohello target cloud service by using hello package and .cscfg file generated in hello previous step.</span></span>
6. <span data-ttu-id="ca4a9-120">Hello tanúsítvány (.pfx-fájl) toohello gép, amely rendelkezik a Visual Studio Azure SDK-val telepített .NET-keretrendszerhez készült importálása.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-120">Import hello certificate (.pfx file) toohello machine that has Visual Studio with Azure SDK for .NET installed.</span></span> <span data-ttu-id="ca4a9-121">Győződjön meg arról, hogy tooimport toohello `CurrentUser\My` tanúsítványtárolójába, ellenkező esetben a Visual Studio csatlakoztatását toohello hibakereső sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-121">Make sure tooimport toohello `CurrentUser\My` certificate store, otherwise attaching toohello debugger in Visual Studio will fail.</span></span>

## <a name="enabling-remote-debugging-for-virtual-machines"></a><span data-ttu-id="ca4a9-122">A virtuális gépek távoli hibakeresés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ca4a9-122">Enabling remote debugging for virtual machines</span></span>
1. <span data-ttu-id="ca4a9-123">Hozzon létre egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-123">Create an Azure virtual machine.</span></span> <span data-ttu-id="ca4a9-124">Lásd: [Windows Server rendszerű virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vagy [a Visual Studio Azure virtuális gépek létrehozására és kezelésére](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ca4a9-124">See [Create a Virtual Machine Running Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) or [Create and Manage Azure Virtual Machines in Visual Studio](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
2. <span data-ttu-id="ca4a9-125">A hello [klasszikus Azure portálon](http://go.microsoft.com/fwlink/p/?LinkID=269851), hello virtuális gép irányítópult toosee hello virtuális gép megtekintése **RDP tanúsítvány UJJLENYOMATA**.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-125">On hello [Azure classic portal page](http://go.microsoft.com/fwlink/p/?LinkID=269851), view hello virtual machine's dashboard toosee hello virtual machine’s **RDP CERTIFICATE THUMBPRINT**.</span></span> <span data-ttu-id="ca4a9-126">Ez az érték szolgál hello `ServerThumbprint` hello bővítménykonfiguráció értéket.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-126">This value is used for hello `ServerThumbprint` value in hello extension configuration.</span></span>
3. <span data-ttu-id="ca4a9-127">Hozzon létre egy ügyféltanúsítványt, a [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services-certs-create.md) (ne a hello .pfx és tanúsítvány-ujjlenyomat RDP).</span><span class="sxs-lookup"><span data-stu-id="ca4a9-127">Create a client certificate as outlined in [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md) (keep hello .pfx and RDP certificate thumbprint).</span></span>
4. <span data-ttu-id="ca4a9-128">Azure Powershell telepítése (0.7.4 verzió vagy újabb) a [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ca4a9-128">Install Azure Powershell (version 0.7.4 or later) as outlined in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
5. <span data-ttu-id="ca4a9-129">Futtassa a következő parancsfájl tooenable hello RemoteDebug bővítmény hello.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-129">Run hello following script tooenable hello RemoteDebug extension.</span></span> <span data-ttu-id="ca4a9-130">Cserélje le a hello elérési útja és a személyes adatokat a saját, például a előfizetés nevét, a szolgáltatás neve és az ujjlenyomat.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-130">Replace hello paths and personal data with your own, such as your subscription name, service name, and thumbprint.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ca4a9-131">Ezt a parancsfájlt a Visual Studio 2015 van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-131">This script is configured for Visual Studio 2015.</span></span> <span data-ttu-id="ca4a9-132">Ha a Visual Studio 2013 vagy Visual Studio 2017 használ, módosítsa a hello `$referenceName` és `$extensionName` az alábbi hozzárendelések túl`RemoteDebugVS2013` vagy `RemoteDebugVS2017`.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-132">If you’re using Visual Studio 2013 or Visual Studio 2017, modify hello `$referenceName` and `$extensionName` assignments below too`RemoteDebugVS2013` or `RemoteDebugVS2017`.</span></span>

    ```powershell   
    Add-AzureAccount

    Select-AzureSubscription "My Microsoft Subscription"

    $vm = Get-AzureVM -ServiceName "mytestvm1" -Name "mytestvm1"

    $endpoints = @(
                    ,@{Name="RDConnVS2013"; PublicPort=30400; PrivatePort=30398}
                    ,@{Name="RDFwdrVS2013"; PublicPort=31400; PrivatePort=31398}
                )

    foreach($endpoint in $endpoints)
    {
        Add-AzureEndpoint -VM $vm -Name $endpoint.Name -Protocol tcp -PublicPort $endpoint.PublicPort -LocalPort $endpoint.PrivatePort
    }

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015"
    $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug"
    $extensionName = "RemoteDebugVS2015"
    $version = "1.*"
    $publicConfiguration = "<PublicConfig><Connector.Enabled>true</Connector.Enabled><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set-AzureVMExtension -ReferenceName $referenceName -Publisher $publisher -ExtensionName $extensionName -Version $version -PublicConfiguration $publicConfiguration

    foreach($extension in $vm.VM.ResourceExtensionReferences)
    {
        if(($extension.ReferenceName -eq $referenceName) `
        -and ($extension.Publisher -eq $publisher) `
        -and ($extension.Name -eq $extensionName) `
        -and ($extension.Version -eq $version))
        {
            $extension.ResourceExtensionParameterValues[0].Key = 'config.txt'
            break
        }
    }

    $vm | Update-AzureVM
    ```

6. <span data-ttu-id="ca4a9-133">Hello tanúsítvány (.pfx) toohello gép importálása, amely rendelkezik a Visual Studio Azure SDK-val telepített .NET-keretrendszerhez készült.</span><span class="sxs-lookup"><span data-stu-id="ca4a9-133">Import hello certificate (.pfx) toohello machine that has Visual Studio with Azure SDK for .NET installed.</span></span>

