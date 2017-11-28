---
title: "aaaDeploy és kezelheti a Notification Hubs PowerShell használatával"
description: "Hogyan tooCreate és kezelése Notification Hubs PowerShell használatával az automatizáláshoz"
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 7c58f2c8-0399-42bc-9e1e-a7f073426451
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8835bdefa0d360354263eab8040259ad08281771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a><span data-ttu-id="bd68c-103">A Notification Hubs telepítése és kezelése a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="bd68c-103">Deploy and Manage Notification Hubs using PowerShell</span></span>
## <a name="overview"></a><span data-ttu-id="bd68c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="bd68c-104">Overview</span></span>
<span data-ttu-id="bd68c-105">Ez a cikk bemutatja, hogyan toouse létrehozása és kezelése Azure Notification Hubs PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="bd68c-105">This article shows you how toouse Create and Manage Azure Notification Hubs using PowerShell.</span></span> <span data-ttu-id="bd68c-106">Ez a témakör a következő általános automatizálási feladatok hello láthatók.</span><span class="sxs-lookup"><span data-stu-id="bd68c-106">hello following common automation tasks are shown in this topic.</span></span>

* <span data-ttu-id="bd68c-107">Egy értesítési központ létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd68c-107">Create a Notification Hub</span></span>
* <span data-ttu-id="bd68c-108">A hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="bd68c-108">Set Credentials</span></span>

<span data-ttu-id="bd68c-109">Ha a a notification hubs toocreate egy új service bus-névtér is kell, lásd: [Service Bus kezelése a PowerShell-lel](../service-bus-messaging/service-bus-powershell-how-to-provision.md).</span><span class="sxs-lookup"><span data-stu-id="bd68c-109">If you also need toocreate a new service bus namespace for your notification hubs, see [Manage Service Bus with PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).</span></span>

<span data-ttu-id="bd68c-110">Értesítések hubok kezelése nem támogatja közvetlenül az Azure PowerShell hello parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="bd68c-110">Managing Notifications Hubs is not supported directly by hello cmdlets included with Azure PowerShell.</span></span> <span data-ttu-id="bd68c-111">hello a legjobb megoldás a powershellből, tooreference hello Microsoft.Azure.NotificationHubs.dll szerelvény.</span><span class="sxs-lookup"><span data-stu-id="bd68c-111">hello best approach from PowerShell is tooreference hello Microsoft.Azure.NotificationHubs.dll assembly.</span></span> <span data-ttu-id="bd68c-112">hello szerelvény terjesztése a hello [Microsoft Azure Notification Hubs NuGet-csomag](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="bd68c-112">hello assembly is distributed with hello [Microsoft Azure Notification Hubs NuGet package](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd68c-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bd68c-113">Prerequisites</span></span>
<span data-ttu-id="bd68c-114">Ez a cikk elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="bd68c-114">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="bd68c-115">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="bd68c-115">An Azure subscription.</span></span> <span data-ttu-id="bd68c-116">Azure előfizetés-alapú platform.</span><span class="sxs-lookup"><span data-stu-id="bd68c-116">Azure is a subscription-based platform.</span></span> <span data-ttu-id="bd68c-117">Előfizetés beszerzésével kapcsolatos további információkért lásd: [megvásárlási lehetőségeinek], [ajánlatok], vagy [ingyenes].</span><span class="sxs-lookup"><span data-stu-id="bd68c-117">For more information about obtaining a subscription, see [Purchase Options], [Member Offers], or [Free Trial].</span></span>
* <span data-ttu-id="bd68c-118">Az Azure PowerShell számítógép.</span><span class="sxs-lookup"><span data-stu-id="bd68c-118">A computer with Azure PowerShell.</span></span> <span data-ttu-id="bd68c-119">Útmutatásért lásd: [telepítse és konfigurálja az Azure Powershellt].</span><span class="sxs-lookup"><span data-stu-id="bd68c-119">For instructions, see [Install and configure Azure PowerShell].</span></span>
* <span data-ttu-id="bd68c-120">A PowerShell-parancsfájlok, a NuGet-csomagok és a .NET-keretrendszer hello általános ismertetése.</span><span class="sxs-lookup"><span data-stu-id="bd68c-120">A general understanding of PowerShell scripts, NuGet packages, and hello .NET Framework.</span></span>

## <a name="including-a-reference-toohello-net-assembly-for-service-bus"></a><span data-ttu-id="bd68c-121">Többek között a Service Bus referencia toohello .NET szerelvény</span><span class="sxs-lookup"><span data-stu-id="bd68c-121">Including a reference toohello .NET assembly for Service Bus</span></span>
<span data-ttu-id="bd68c-122">Azure Notification Hubs kezelése még nincs hello Azure PowerShell PowerShell-parancsmagokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bd68c-122">Managing Azure Notification Hubs is not yet included with hello PowerShell cmdlets in Azure PowerShell.</span></span> <span data-ttu-id="bd68c-123">tooprovision notification hubs használatával hello .NET ügyfél hello megadott használható [Microsoft Azure Notification Hubs NuGet-csomag](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="bd68c-123">tooprovision notification hubs, you can use hello .NET client provided in hello [Microsoft Azure Notification Hubs NuGet package](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="bd68c-124">Először győződjön meg arról, hogy a parancsfájl megtalálhassák hello **Microsoft.Azure.NotificationHubs.dll** szerelvény, ami a Visual Studio-projektet a NuGet csomag telepítve van.</span><span class="sxs-lookup"><span data-stu-id="bd68c-124">First, make sure your script can locate hello **Microsoft.Azure.NotificationHubs.dll** assembly, which is installed as a NuGet package in a Visual Studio project.</span></span> <span data-ttu-id="bd68c-125">A sorrend toobe rugalmas hello parancsfájl ezeket a lépéseket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="bd68c-125">In order toobe flexible, hello script performs these steps:</span></span>

1. <span data-ttu-id="bd68c-126">Meghatározza, hogy hello elérési utat, ahol azt lett meghívva.</span><span class="sxs-lookup"><span data-stu-id="bd68c-126">Determines hello path at which it was invoked.</span></span>
2. <span data-ttu-id="bd68c-127">Traverses hello elérési útja, amíg nem talál egy nevű mappát `packages`.</span><span class="sxs-lookup"><span data-stu-id="bd68c-127">Traverses hello path until it finds a folder named `packages`.</span></span> <span data-ttu-id="bd68c-128">A mappa létrehozása a Visual Studio-projektek NuGet-csomagok telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="bd68c-128">This folder is created when you install NuGet packages for Visual Studio projects.</span></span>
3. <span data-ttu-id="bd68c-129">Rekurzív keresések hello `packages` egy összeállítás nevű mappa **Microsoft.Azure.NotificationHubs.dll**.</span><span class="sxs-lookup"><span data-stu-id="bd68c-129">Recursively searches hello `packages` folder for an assembly named **Microsoft.Azure.NotificationHubs.dll**.</span></span>
4. <span data-ttu-id="bd68c-130">Hivatkozások hello szerelvény, így hello típusok későbbi használat céljából érhetők el.</span><span class="sxs-lookup"><span data-stu-id="bd68c-130">References hello assembly so that hello types are available for later use.</span></span>

<span data-ttu-id="bd68c-131">Ez hogyan valósíthatók meg ezeket a lépéseket egy PowerShell-parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="bd68c-131">Here's how these steps are implemented in a PowerShell script:</span></span>

``` powershell

try
{
    # WARNING: Make sure tooreference hello latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding hello [Microsoft.Azure.NotificationHubs.dll] assembly toohello script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "hello [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added toohello script."
}

catch [System.Exception]
{
    Write-Error("Could not add hello Microsoft.Azure.NotificationHubs.dll assembly toohello script. Make sure you build hello solution before running hello provisioning script.")
}
```

## <a name="create-hello-namespacemanager-class"></a><span data-ttu-id="bd68c-132">Hello NamespaceManager osztály létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd68c-132">Create hello NamespaceManager class</span></span>
<span data-ttu-id="bd68c-133">a Notification Hubs tooprovision hello példányt létrehozni [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) hello SDK osztályt.</span><span class="sxs-lookup"><span data-stu-id="bd68c-133">tooprovision Notification Hubs, create an instance of hello [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) class from hello SDK.</span></span> 

<span data-ttu-id="bd68c-134">Használhatja a hello [Get-AzureSBAuthorizationRule] parancsmag részét képező Azure PowerShell tooretrieve az engedélyezési szabály által használt tooprovide egy kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bd68c-134">You can use hello [Get-AzureSBAuthorizationRule] cmdlet included with Azure PowerShell tooretrieve an authorization rule that's used tooprovide a connection string.</span></span> <span data-ttu-id="bd68c-135">Azt fogja tárolni egy hivatkozás toohello `NamespaceManager` példánya a hello `$NamespaceManager` változó.</span><span class="sxs-lookup"><span data-stu-id="bd68c-135">We'll store a reference toohello `NamespaceManager` instance in hello `$NamespaceManager` variable.</span></span> <span data-ttu-id="bd68c-136">Használjuk `$NamespaceManager` tooprovision egy értesítési központot.</span><span class="sxs-lookup"><span data-stu-id="bd68c-136">We will use `$NamespaceManager` tooprovision a notification hub.</span></span>

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create hello NamespaceManager object toocreate hello hub
Write-Output "Creating a NamespaceManager object for hello [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for hello [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a><span data-ttu-id="bd68c-137">Új értesítési központ kiépítése</span><span class="sxs-lookup"><span data-stu-id="bd68c-137">Provisioning a new Notification Hub</span></span>
<span data-ttu-id="bd68c-138">új értesítési központ tooprovision hello használata [.NET API-t a Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="bd68c-138">tooprovision a new notification hub, use hello [.NET API for Notification Hubs].</span></span>

<span data-ttu-id="bd68c-139">Ez a kijelző hello parancsfájl beállította négy helyi változók.</span><span class="sxs-lookup"><span data-stu-id="bd68c-139">In this part of hello script you set up four local variables.</span></span> 

1. <span data-ttu-id="bd68c-140">`$Namespace`: A toohello nevének beállítása hello névtér toocreate, ahová egy értesítési központot.</span><span class="sxs-lookup"><span data-stu-id="bd68c-140">`$Namespace` : Set this toohello name of hello namespace where you want toocreate a notification hub.</span></span>
2. <span data-ttu-id="bd68c-141">`$Path`: A hello új értesítési központ elérési toohello nevének beállítása.</span><span class="sxs-lookup"><span data-stu-id="bd68c-141">`$Path` : Set this path toohello name of hello new notification hub.</span></span>  <span data-ttu-id="bd68c-142">Például "MyHub."</span><span class="sxs-lookup"><span data-stu-id="bd68c-142">For example, "MyHub".</span></span>    
3. <span data-ttu-id="bd68c-143">`$WnsPackageSid`: Állítson be úgy a toohello csomag biztonsági azonosítója az Ön Windows-alkalmazás a hello [Windows fejlesztői központ](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="bd68c-143">`$WnsPackageSid` : Set this toohello package SID for you Windows App from hello [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span></span>
4. <span data-ttu-id="bd68c-144">`$WnsSecretkey`: Állítható be a toohello titkos kulcs az Ön Windows-alkalmazás hello [Windows fejlesztői központ](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="bd68c-144">`$WnsSecretkey`: Set this toohello secret key for you Windows App from hello [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span></span>

<span data-ttu-id="bd68c-145">Ezek a változók használt tooconnect tooyour névtérrel, és hozzon létre egy új értesítési központ konfigurálva toohandle Windows értesítési szolgáltatások (WNS) értesítések WNS hitelesítő adatokkal a Windows-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bd68c-145">These variables are used tooconnect tooyour namespace and create a new Notification Hub configured toohandle Windows Notification Services (WNS) notifications with WNS credentials for a Windows App.</span></span> <span data-ttu-id="bd68c-146">Hello beszerzéséről információt csomag biztonsági azonosítója és a titkos kulcs lásd: hello [Ismerkedés a Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="bd68c-146">For information on obtaining hello package SID and secret key see, hello [Getting Started with Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) tutorial.</span></span> 

* <span data-ttu-id="bd68c-147">hello parancsfájl részlet használ hello `NamespaceManager` toocheck toosee objektumot, ha az értesítési központ hello által azonosított `$Path` létezik-e.</span><span class="sxs-lookup"><span data-stu-id="bd68c-147">hello script snippet uses hello `NamespaceManager` object toocheck toosee if hello Notification Hub identified by `$Path` exists.</span></span>
* <span data-ttu-id="bd68c-148">Ha még nem létezik, hello parancsfájl létrehoz egy `NotificationHubDescription` WNS-sel való hitelesítő adatait, és adja át, hogy toohello `NamespaceManager` osztály `CreateNotificationHub` metódust.</span><span class="sxs-lookup"><span data-stu-id="bd68c-148">If it does not exist, hello script will create an `NotificationHubDescription` with WNS credentials and pass that toohello `NamespaceManager` class `CreateNotificationHub` method.</span></span>

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query hello namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if hello namespace already exists
if ($CurrentNamespace)
{
    Write-Output "hello namespace [$Namespace] in hello [$($CurrentNamespace.Region)] region was found."

    # Create hello NamespaceManager object used toocreate a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for hello [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for hello [$Namespace] namespace has been successfully created."

    # Check toosee if hello Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "hello [$Path] notification hub already exists in hello [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating hello [$Path] notification hub in hello [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "hello [$Path] notification hub was created in hello [$Namespace] namespace."
    }
}
else
{
    Write-Host "hello [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a><span data-ttu-id="bd68c-149">További források</span><span class="sxs-lookup"><span data-stu-id="bd68c-149">Additional Resources</span></span>
* [<span data-ttu-id="bd68c-150">A PowerShell használatával a Service Bus kezelése</span><span class="sxs-lookup"><span data-stu-id="bd68c-150">Manage Service Bus with PowerShell</span></span>](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
* [<span data-ttu-id="bd68c-151">Hogyan toocreate Service Bus üzenetsorok, témakörök és előfizetések egy PowerShell-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="bd68c-151">How toocreate Service Bus queues, topics and subscriptions using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [<span data-ttu-id="bd68c-152">Hogyan toocreate a Service Bus Namespace, és az Event Hubs egy PowerShell-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="bd68c-152">How toocreate a Service Bus Namespace and an Event Hub using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

<span data-ttu-id="bd68c-153">Néhány előre elkészített parancsfájlok letölthetők is:</span><span class="sxs-lookup"><span data-stu-id="bd68c-153">Some ready-made scripts are also available for download:</span></span>

* [<span data-ttu-id="bd68c-154">Service Bus PowerShell parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="bd68c-154">Service Bus PowerShell Scripts</span></span>](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

[megvásárlási lehetőségeinek]: http://azure.microsoft.com/pricing/purchase-options/
[ajánlatok]: http://azure.microsoft.com/pricing/member-offers/
[ingyenes]: http://azure.microsoft.com/pricing/free-trial/
[telepítse és konfigurálja az Azure Powershellt]: /powershell/azureps-cmdlets-docs
[.NET API-t a Notification Hubs]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx

