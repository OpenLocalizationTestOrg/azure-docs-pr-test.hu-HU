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
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>A Notification Hubs telepítése és kezelése a PowerShell-lel
## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja, hogyan toouse létrehozása és kezelése Azure Notification Hubs PowerShell használatával. Ez a témakör a következő általános automatizálási feladatok hello láthatók.

* Egy értesítési központ létrehozása
* A hitelesítő adatokat

Ha a a notification hubs toocreate egy új service bus-névtér is kell, lásd: [Service Bus kezelése a PowerShell-lel](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Értesítések hubok kezelése nem támogatja közvetlenül az Azure PowerShell hello parancsmagok. hello a legjobb megoldás a powershellből, tooreference hello Microsoft.Azure.NotificationHubs.dll szerelvény. hello szerelvény terjesztése a hello [Microsoft Azure Notification Hubs NuGet-csomag](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk elkezdéséhez hello következő kell rendelkeznie:

* Azure-előfizetés. Azure előfizetés-alapú platform. Előfizetés beszerzésével kapcsolatos további információkért lásd: [megvásárlási lehetőségeinek], [ajánlatok], vagy [ingyenes].
* Az Azure PowerShell számítógép. Útmutatásért lásd: [telepítse és konfigurálja az Azure Powershellt].
* A PowerShell-parancsfájlok, a NuGet-csomagok és a .NET-keretrendszer hello általános ismertetése.

## <a name="including-a-reference-toohello-net-assembly-for-service-bus"></a>Többek között a Service Bus referencia toohello .NET szerelvény
Azure Notification Hubs kezelése még nincs hello Azure PowerShell PowerShell-parancsmagokat tartalmazza. tooprovision notification hubs használatával hello .NET ügyfél hello megadott használható [Microsoft Azure Notification Hubs NuGet-csomag](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Először győződjön meg arról, hogy a parancsfájl megtalálhassák hello **Microsoft.Azure.NotificationHubs.dll** szerelvény, ami a Visual Studio-projektet a NuGet csomag telepítve van. A sorrend toobe rugalmas hello parancsfájl ezeket a lépéseket hajtja végre:

1. Meghatározza, hogy hello elérési utat, ahol azt lett meghívva.
2. Traverses hello elérési útja, amíg nem talál egy nevű mappát `packages`. A mappa létrehozása a Visual Studio-projektek NuGet-csomagok telepítésekor.
3. Rekurzív keresések hello `packages` egy összeállítás nevű mappa **Microsoft.Azure.NotificationHubs.dll**.
4. Hivatkozások hello szerelvény, így hello típusok későbbi használat céljából érhetők el.

Ez hogyan valósíthatók meg ezeket a lépéseket egy PowerShell-parancsfájlt:

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

## <a name="create-hello-namespacemanager-class"></a>Hello NamespaceManager osztály létrehozása
a Notification Hubs tooprovision hello példányt létrehozni [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) hello SDK osztályt. 

Használhatja a hello [Get-AzureSBAuthorizationRule] parancsmag részét képező Azure PowerShell tooretrieve az engedélyezési szabály által használt tooprovide egy kapcsolati karakterláncot. Azt fogja tárolni egy hivatkozás toohello `NamespaceManager` példánya a hello `$NamespaceManager` változó. Használjuk `$NamespaceManager` tooprovision egy értesítési központot.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create hello NamespaceManager object toocreate hello hub
Write-Output "Creating a NamespaceManager object for hello [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for hello [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Új értesítési központ kiépítése
új értesítési központ tooprovision hello használata [.NET API-t a Notification Hubs].

Ez a kijelző hello parancsfájl beállította négy helyi változók. 

1. `$Namespace`: A toohello nevének beállítása hello névtér toocreate, ahová egy értesítési központot.
2. `$Path`: A hello új értesítési központ elérési toohello nevének beállítása.  Például "MyHub."    
3. `$WnsPackageSid`: Állítson be úgy a toohello csomag biztonsági azonosítója az Ön Windows-alkalmazás a hello [Windows fejlesztői központ](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Állítható be a toohello titkos kulcs az Ön Windows-alkalmazás hello [Windows fejlesztői központ](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Ezek a változók használt tooconnect tooyour névtérrel, és hozzon létre egy új értesítési központ konfigurálva toohandle Windows értesítési szolgáltatások (WNS) értesítések WNS hitelesítő adatokkal a Windows-alkalmazás. Hello beszerzéséről információt csomag biztonsági azonosítója és a titkos kulcs lásd: hello [Ismerkedés a Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) oktatóanyag. 

* hello parancsfájl részlet használ hello `NamespaceManager` toocheck toosee objektumot, ha az értesítési központ hello által azonosított `$Path` létezik-e.
* Ha még nem létezik, hello parancsfájl létrehoz egy `NotificationHubDescription` WNS-sel való hitelesítő adatait, és adja át, hogy toohello `NamespaceManager` osztály `CreateNotificationHub` metódust.

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




## <a name="additional-resources"></a>További források
* [A PowerShell használatával a Service Bus kezelése](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
* [Hogyan toocreate Service Bus üzenetsorok, témakörök és előfizetések egy PowerShell-parancsfájl használatával](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Hogyan toocreate a Service Bus Namespace, és az Event Hubs egy PowerShell-parancsfájl használatával](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Néhány előre elkészített parancsfájlok letölthetők is:

* [Service Bus PowerShell parancsfájlok](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

[megvásárlási lehetőségeinek]: http://azure.microsoft.com/pricing/purchase-options/
[ajánlatok]: http://azure.microsoft.com/pricing/member-offers/
[ingyenes]: http://azure.microsoft.com/pricing/free-trial/
[telepítse és konfigurálja az Azure Powershellt]: /powershell/azureps-cmdlets-docs
[.NET API-t a Notification Hubs]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx

