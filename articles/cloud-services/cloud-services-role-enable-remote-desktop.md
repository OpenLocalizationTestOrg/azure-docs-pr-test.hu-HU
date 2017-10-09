---
title: "Azure Cloud Service a távoli asztal aaaEnable |} Microsoft Docs"
description: "Hogyan tooconfigure az azure felhőalapú szolgáltatás alkalmazás tooallow távoli asztali kapcsolatok"
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: d3110ee8-6526-4585-aba5-d0bc9a713e9b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: b3c0180bf5ad29cb77e5303ccbd6f9ccc44b7b0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Engedélyezze a távoli asztali kapcsolat egy szerepkör esetén az Azure Cloud Services csomag

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [klasszikus Azure portál](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)

Engedélyezheti a távoli asztali kapcsolat létrehozása a szerepkörben a fejlesztés során hello távoli asztal modulok belefoglalja a szolgáltatás definíciós, vagy választhatja azt a távoli asztal tooenable hello távoli asztali bővítmény keresztül. hello előnyben részesített megoldás, toouse hello távoli asztali bővítmény, a távoli asztal hello alkalmazás anélkül, hogy tooredeploy az alkalmazás központi telepítése után is engedélyezheti.

## <a name="configure-remote-desktop-from-hello-azure-classic-portal"></a>A távoli asztal a klasszikus Azure portálon hello konfigurálása
hello a klasszikus Azure portálon hello a távoli asztali bővítmény módszert használ, így a távoli asztal hello alkalmazás központi telepítése után is engedélyezheti. Hello **konfigurálása** a felhőszolgáltatás lap lehetővé teszi a távoli asztal tooenable használt tooconnect toohello virtuális gépek módosítása hello helyi rendszergazdai fiókot, hello tanúsítvány-hitelesítésben használt és hello beállítása lejárati dátuma.

1. Kattintson a **Felhőszolgáltatások**, kattintson hello felhőszolgáltatás hello nevét, majd **konfigurálása**.
2. Kattintson a hello **távoli** hello alsó gombra.

    ![Távoli cloud services csomag](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > Összes szerepkörpéldányt újra kell indítani, amikor először a távoli asztal engedélyezése és kattintson az OK gombra (jelölő). Újraindítás, hello tanúsítványjelszavas használt tooencrypt hello tooprevent hello szerepkört telepíteni kell. Újraindítás, tooprevent [hello felhőszolgáltatás tanúsítvány feltöltése](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) , és visszatér a toothis párbeszédpanel.

3. A **szerepkörök**, jelölje be hello szerepkör tooupdate kívánt **összes** összes szerepköre tekintetében.
4. Végezze el a következő módosításokat hello:

   * Távoli asztal, jelölje be hello tooenable **távoli asztal engedélyezése** jelölőnégyzetet. toodisable távoli asztal, törölje a jelet hello jelölőnégyzetet.
   * Hozzon létre egy fiókot toouse távoli asztali kapcsolatok toohello szerepkörpéldányokat.
   * Frissítse a hello hello meglévő fiók jelszavát.
   * Válassza ki a hitelesítés egy feltöltött tanúsítvány toouse (feltöltési hello tanúsítvány használatával **feltöltése** a hello **tanúsítványok** lap), vagy hozzon létre egy új tanúsítványt.
   * Hello lejárati dátum hello távoli asztal konfigurációjának módosítása.

5. A konfigurációs módosítások befejeztével kattintson **OK** (jelölő).

## <a name="remote-into-role-instances"></a>A szerepkörpéldányok távoli
A távoli asztal hello szerepkörök engedélyezése után is távoli való a szerepkörpéldányt, különböző eszközök között.

tooconnect tooa szerepkör példánya a klasszikus Azure portálon hello:

1. Kattintson a **példányok** tooopen hello **példányok** lap.
2. Válassza ki a szerepkör példánya, amely a távoli asztal konfigurálva van.
3. Kattintson a **Connect**, és hajtsa végre a hello utasításokat tooopen hello asztali.
4. Kattintson a **nyitott** , majd **Connect** toostart hello távoli asztali kapcsolat.

### <a name="use-visual-studio-tooremote-into-a-role-instance"></a>Visual Studio tooremote használja azokat a szerepkör példánya
A Visual Studio Server Explorer:

1. Bontsa ki a hello **Azure** > **Felhőszolgáltatások** > **[felhőszolgáltatás neve]** csomópont.
2. Bontsa ki **átmeneti** vagy **éles**.
3. Bontsa ki a hello egyéni szerepkör.
4. Kattintson a jobb gombbal egy hello szerepkörpéldányt beállítani, kattintson a **csatlakozzon a távoli asztali kapcsolattal...** , majd adja meg a hello felhasználónevet és jelszót.

![Server explorer távoli asztal](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-tooget-hello-rdp-file"></a>PowerShell tooget hello RDP-fájl használata
Használhatja a hello [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) parancsmag tooretrieve hello RDP-fájlt. Távoli asztali kapcsolat tooaccess hello felhőalapú szolgáltatással használhatja hello RDP-fájlt.

### <a name="programmatically-download-hello-rdp-file-through-hello-service-management-rest-api"></a>Hello szolgáltatásfelügyelet REST API használatával hello RDP-fájl letöltése
Használhatja a hello [RDP-fájl letöltése](https://msdn.microsoft.com/library/jj157183.aspx) REST művelet toodownload hello RDP-fájlt.

## <a name="tooconfigure-remote-desktop-in-hello-service-definition-file"></a>Távoli asztal hello szolgáltatásdefiníciós fájlban tooconfigure
Ez a módszer lehetővé teszi a távoli asztal tooenable hello alkalmazás fejlesztése során. Ez a megközelítés titkosított jelszavakat igényel tárolja a szolgáltatás konfigurációs fájl- és a frissítések toohello a távoli asztal konfigurálásának igényelnének egy újratelepítés hello alkalmazás. Ha azt szeretné, hogy tooavoid ezeket kell használnia a távoli asztali bővítmény hello downsides alapú a fent leírt módszer.  

Használhatja a Visual Studio túl[engedélyezése a távoli asztali kapcsolat](../vs-azure-tools-remote-desktop-roles.md) hello szolgáltatás definíciós fájl módszer használatával.  
hello következő lépések leírják hello módosítások szükséges toohello szolgáltatás modell fájlok tooenable távoli asztal. A Visual Studio automatikusan fog teszi ezeket a módosításokat, közzétételekor.

### <a name="set-up-hello-connection-in-hello-service-model"></a>Hello hello szolgáltatásmodell-kapcsolat beállítása
Használjon hello **importálja** elem tooimport hello **RemoteAccess** modul és hello **RemoteForwarder** modul toohello [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) fájlt.

hello szolgáltatásdefiníciós fájl kell lennie a következő példa a hello hasonló toohello `<Imports>` -eleme.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
Hello [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) fájl a következő példa hasonló toohello kell, vegye figyelembe a hello `<ConfigurationSettings>` és `<Certificates>` elemeket. a megadott tanúsítvány hello kell [feltöltve felhőszolgáltatás toohello](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>További források
[Hogyan tooConfigure Felhőszolgáltatások](cloud-services-how-to-configure.md)
[Cloud services – gyakori kérdések – a távoli asztal](cloud-services-faq.md)
