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
# <a name="enable-remote-debugging-when-using-continuous-delivery-toopublish-tooazure"></a>Folyamatos kézbesítési toopublish tooAzure használatakor távoli hibakeresésének engedélyezése
Engedélyezheti a távoli hibakeresés Azure felhőszolgáltatások vagy a virtuális gépek használatakor [folyamatos kézbesítési](cloud-services-dotnet-continuous-delivery.md) toopublish tooAzure ezeket a lépéseket követve.

## <a name="enabling-remote-debugging-for-cloud-services"></a>A felhőszolgáltatások távoli hibakeresés engedélyezése
1. Hello build ügynökön hello kezdeti környezet beállítása az Azure-bA a [parancssori létrehozása az Azure-](http://msdn.microsoft.com/library/hh535755.aspx).
2. Mivel hello távoli hibakeresési futásidejű (msvsmon.exe) hello csomag szükséges, telepítse a hello **távoli Tools for Visual Studio**.

    * [A Visual Studio 2017 távoli eszközök](https://go.microsoft.com/fwlink/?LinkId=746570)
    * [A Visual Studio 2015 távoli eszközök](https://go.microsoft.com/fwlink/?LinkId=615470)
    * [Távoli Tools for Visual Studio 2013 Update 5](https://www.microsoft.com/download/details.aspx?id=48156)
    
    Alternatív megoldásként másolhatja a rendszer, amely rendelkezik a telepített Visual Studio hello távoli hibakeresési bináris fájlok fordítását.

3. Hozzon létre egy tanúsítványt, a [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services-certs-create.md). Hello .pfx és RDP tanúsítvány ujjlenyomatát, és töltse fel a hello tanúsítvány toohello cél felhőalapú szolgáltatás.
4. Használja az alábbi beállítások hello MSBuild parancssori toobuild és a csomag engedélyezett távoli hibakereséssel hello. (Útvonalakat tooyour rendszer és a projekt hello szög zárójeles elemek fájlok helyettesítse.)
   
        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of hello certificate added toohello cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path tooyour VS solution file>"
   
    `VSX64RemoteDebuggerPath`hello elérési toohello mappa van a Visual Studio tartalmazó msvsmon.exe hello távoli eszközei.
    `RemoteDebuggerConnectorVersion`a felhőszolgáltatásban hello Azure SDK-verzió van. Azt is illeszkednie kell hello verziója van telepítve, a Visual Studio.
5. Hello előző lépésben létrehozott hello csomag és a .cscfg fájl használatával toohello cél felhőalapú szolgáltatást teszik közzé.
6. Hello tanúsítvány (.pfx-fájl) toohello gép, amely rendelkezik a Visual Studio Azure SDK-val telepített .NET-keretrendszerhez készült importálása. Győződjön meg arról, hogy tooimport toohello `CurrentUser\My` tanúsítványtárolójába, ellenkező esetben a Visual Studio csatlakoztatását toohello hibakereső sikertelen lesz.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>A virtuális gépek távoli hibakeresés engedélyezése
1. Hozzon létre egy Azure virtuális gépen. Lásd: [Windows Server rendszerű virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vagy [a Visual Studio Azure virtuális gépek létrehozására és kezelésére](../virtual-machines/windows/classic/manage-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
2. A hello [klasszikus Azure portálon](http://go.microsoft.com/fwlink/p/?LinkID=269851), hello virtuális gép irányítópult toosee hello virtuális gép megtekintése **RDP tanúsítvány UJJLENYOMATA**. Ez az érték szolgál hello `ServerThumbprint` hello bővítménykonfiguráció értéket.
3. Hozzon létre egy ügyféltanúsítványt, a [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services-certs-create.md) (ne a hello .pfx és tanúsítvány-ujjlenyomat RDP).
4. Azure Powershell telepítése (0.7.4 verzió vagy újabb) a [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
5. Futtassa a következő parancsfájl tooenable hello RemoteDebug bővítmény hello. Cserélje le a hello elérési útja és a személyes adatokat a saját, például a előfizetés nevét, a szolgáltatás neve és az ujjlenyomat.
   
   > [!NOTE]
   > Ezt a parancsfájlt a Visual Studio 2015 van konfigurálva. Ha a Visual Studio 2013 vagy Visual Studio 2017 használ, módosítsa a hello `$referenceName` és `$extensionName` az alábbi hozzárendelések túl`RemoteDebugVS2013` vagy `RemoteDebugVS2017`.

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

6. Hello tanúsítvány (.pfx) toohello gép importálása, amely rendelkezik a Visual Studio Azure SDK-val telepített .NET-keretrendszerhez készült.

