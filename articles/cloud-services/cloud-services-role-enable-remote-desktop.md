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
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a><span data-ttu-id="5e34e-103">Engedélyezze a távoli asztali kapcsolat egy szerepkör esetén az Azure Cloud Services csomag</span><span class="sxs-lookup"><span data-stu-id="5e34e-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5e34e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5e34e-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="5e34e-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="5e34e-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="5e34e-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e34e-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="5e34e-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e34e-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)

<span data-ttu-id="5e34e-108">Engedélyezheti a távoli asztali kapcsolat létrehozása a szerepkörben a fejlesztés során hello távoli asztal modulok belefoglalja a szolgáltatás definíciós, vagy választhatja azt a távoli asztal tooenable hello távoli asztali bővítmény keresztül.</span><span class="sxs-lookup"><span data-stu-id="5e34e-108">You can enable a Remote Desktop connection in your role during development by including hello Remote Desktop modules in your service definition or you can choose tooenable Remote Desktop through hello Remote Desktop Extension.</span></span> <span data-ttu-id="5e34e-109">hello előnyben részesített megoldás, toouse hello távoli asztali bővítmény, a távoli asztal hello alkalmazás anélkül, hogy tooredeploy az alkalmazás központi telepítése után is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="5e34e-109">hello preferred approach is toouse hello Remote Desktop extension as you can enable Remote Desktop even after hello application is deployed without having tooredeploy your application.</span></span>

## <a name="configure-remote-desktop-from-hello-azure-classic-portal"></a><span data-ttu-id="5e34e-110">A távoli asztal a klasszikus Azure portálon hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5e34e-110">Configure Remote Desktop from hello Azure classic portal</span></span>
<span data-ttu-id="5e34e-111">hello a klasszikus Azure portálon hello a távoli asztali bővítmény módszert használ, így a távoli asztal hello alkalmazás központi telepítése után is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="5e34e-111">hello Azure classic portal uses hello Remote Desktop Extension approach so you can enable Remote Desktop even after hello application is deployed.</span></span> <span data-ttu-id="5e34e-112">Hello **konfigurálása** a felhőszolgáltatás lap lehetővé teszi a távoli asztal tooenable használt tooconnect toohello virtuális gépek módosítása hello helyi rendszergazdai fiókot, hello tanúsítvány-hitelesítésben használt és hello beállítása lejárati dátuma.</span><span class="sxs-lookup"><span data-stu-id="5e34e-112">hello **Configure** page for your cloud service allows you tooenable Remote Desktop, change hello local Administrator account used tooconnect toohello virtual machines, hello certificate used in authentication and set hello expiration date.</span></span>

1. <span data-ttu-id="5e34e-113">Kattintson a **Felhőszolgáltatások**, kattintson hello felhőszolgáltatás hello nevét, majd **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="5e34e-113">Click **Cloud Services**, click hello name of hello cloud service, and then click **Configure**.</span></span>
2. <span data-ttu-id="5e34e-114">Kattintson a hello **távoli** hello alsó gombra.</span><span class="sxs-lookup"><span data-stu-id="5e34e-114">Click hello **Remote** button at hello bottom.</span></span>

    ![Távoli cloud services csomag](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > <span data-ttu-id="5e34e-116">Összes szerepkörpéldányt újra kell indítani, amikor először a távoli asztal engedélyezése és kattintson az OK gombra (jelölő).</span><span class="sxs-lookup"><span data-stu-id="5e34e-116">All role instances will be restarted when you first enable Remote Desktop and click OK (checkmark).</span></span> <span data-ttu-id="5e34e-117">Újraindítás, hello tanúsítványjelszavas használt tooencrypt hello tooprevent hello szerepkört telepíteni kell.</span><span class="sxs-lookup"><span data-stu-id="5e34e-117">tooprevent a reboot, hello certificate used tooencrypt hello password must be installed on hello role.</span></span> <span data-ttu-id="5e34e-118">Újraindítás, tooprevent [hello felhőszolgáltatás tanúsítvány feltöltése](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) , és visszatér a toothis párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5e34e-118">tooprevent a restart, [upload a certificate for hello cloud service](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) and then return toothis dialog.</span></span>

3. <span data-ttu-id="5e34e-119">A **szerepkörök**, jelölje be hello szerepkör tooupdate kívánt **összes** összes szerepköre tekintetében.</span><span class="sxs-lookup"><span data-stu-id="5e34e-119">In **Roles**, select hello role you want tooupdate or select **All** for all roles.</span></span>
4. <span data-ttu-id="5e34e-120">Végezze el a következő módosításokat hello:</span><span class="sxs-lookup"><span data-stu-id="5e34e-120">Make any of hello following changes:</span></span>

   * <span data-ttu-id="5e34e-121">Távoli asztal, jelölje be hello tooenable **távoli asztal engedélyezése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="5e34e-121">tooenable Remote Desktop, select hello **Enable Remote Desktop** check box.</span></span> <span data-ttu-id="5e34e-122">toodisable távoli asztal, törölje a jelet hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="5e34e-122">toodisable Remote Desktop, clear hello check box.</span></span>
   * <span data-ttu-id="5e34e-123">Hozzon létre egy fiókot toouse távoli asztali kapcsolatok toohello szerepkörpéldányokat.</span><span class="sxs-lookup"><span data-stu-id="5e34e-123">Create an account toouse in Remote Desktop connections toohello role instances.</span></span>
   * <span data-ttu-id="5e34e-124">Frissítse a hello hello meglévő fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="5e34e-124">Update hello password for hello existing account.</span></span>
   * <span data-ttu-id="5e34e-125">Válassza ki a hitelesítés egy feltöltött tanúsítvány toouse (feltöltési hello tanúsítvány használatával **feltöltése** a hello **tanúsítványok** lap), vagy hozzon létre egy új tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="5e34e-125">Select an uploaded certificate toouse for authentication (upload hello certificate using **Upload** on hello **Certificates** page) or create a new certificate.</span></span>
   * <span data-ttu-id="5e34e-126">Hello lejárati dátum hello távoli asztal konfigurációjának módosítása.</span><span class="sxs-lookup"><span data-stu-id="5e34e-126">Change hello expiration date for hello Remote Desktop configuration.</span></span>

5. <span data-ttu-id="5e34e-127">A konfigurációs módosítások befejeztével kattintson **OK** (jelölő).</span><span class="sxs-lookup"><span data-stu-id="5e34e-127">When you finish your configuration updates, click **OK** (checkmark).</span></span>

## <a name="remote-into-role-instances"></a><span data-ttu-id="5e34e-128">A szerepkörpéldányok távoli</span><span class="sxs-lookup"><span data-stu-id="5e34e-128">Remote into role instances</span></span>
<span data-ttu-id="5e34e-129">A távoli asztal hello szerepkörök engedélyezése után is távoli való a szerepkörpéldányt, különböző eszközök között.</span><span class="sxs-lookup"><span data-stu-id="5e34e-129">Once Remote Desktop is enabled on hello roles you can remote into a role instance through various tools.</span></span>

<span data-ttu-id="5e34e-130">tooconnect tooa szerepkör példánya a klasszikus Azure portálon hello:</span><span class="sxs-lookup"><span data-stu-id="5e34e-130">tooconnect tooa role instance from hello Azure classic portal:</span></span>

1. <span data-ttu-id="5e34e-131">Kattintson a **példányok** tooopen hello **példányok** lap.</span><span class="sxs-lookup"><span data-stu-id="5e34e-131">Click **Instances** tooopen hello **Instances** page.</span></span>
2. <span data-ttu-id="5e34e-132">Válassza ki a szerepkör példánya, amely a távoli asztal konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="5e34e-132">Select a role instance that has Remote Desktop configured.</span></span>
3. <span data-ttu-id="5e34e-133">Kattintson a **Connect**, és hajtsa végre a hello utasításokat tooopen hello asztali.</span><span class="sxs-lookup"><span data-stu-id="5e34e-133">Click **Connect**, and follow hello instructions tooopen hello desktop.</span></span>
4. <span data-ttu-id="5e34e-134">Kattintson a **nyitott** , majd **Connect** toostart hello távoli asztali kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="5e34e-134">Click **Open** and then **Connect** toostart hello Remote Desktop connection.</span></span>

### <a name="use-visual-studio-tooremote-into-a-role-instance"></a><span data-ttu-id="5e34e-135">Visual Studio tooremote használja azokat a szerepkör példánya</span><span class="sxs-lookup"><span data-stu-id="5e34e-135">Use Visual Studio tooremote into a role instance</span></span>
<span data-ttu-id="5e34e-136">A Visual Studio Server Explorer:</span><span class="sxs-lookup"><span data-stu-id="5e34e-136">In Visual Studio, Server Explorer:</span></span>

1. <span data-ttu-id="5e34e-137">Bontsa ki a hello **Azure** > **Felhőszolgáltatások** > **[felhőszolgáltatás neve]** csomópont.</span><span class="sxs-lookup"><span data-stu-id="5e34e-137">Expand hello **Azure** > **Cloud Services** > **[cloud service name]** node.</span></span>
2. <span data-ttu-id="5e34e-138">Bontsa ki **átmeneti** vagy **éles**.</span><span class="sxs-lookup"><span data-stu-id="5e34e-138">Expand either **Staging** or **Production**.</span></span>
3. <span data-ttu-id="5e34e-139">Bontsa ki a hello egyéni szerepkör.</span><span class="sxs-lookup"><span data-stu-id="5e34e-139">Expand hello individual role.</span></span>
4. <span data-ttu-id="5e34e-140">Kattintson a jobb gombbal egy hello szerepkörpéldányt beállítani, kattintson a **csatlakozzon a távoli asztali kapcsolattal...** , majd adja meg a hello felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="5e34e-140">Right-click one of hello role instances, click **Connect using Remote Desktop...**, and then enter hello user name and password.</span></span>

![Server explorer távoli asztal](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-tooget-hello-rdp-file"></a><span data-ttu-id="5e34e-142">PowerShell tooget hello RDP-fájl használata</span><span class="sxs-lookup"><span data-stu-id="5e34e-142">Use PowerShell tooget hello RDP file</span></span>
<span data-ttu-id="5e34e-143">Használhatja a hello [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) parancsmag tooretrieve hello RDP-fájlt.</span><span class="sxs-lookup"><span data-stu-id="5e34e-143">You can use hello [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet tooretrieve hello RDP file.</span></span> <span data-ttu-id="5e34e-144">Távoli asztali kapcsolat tooaccess hello felhőalapú szolgáltatással használhatja hello RDP-fájlt.</span><span class="sxs-lookup"><span data-stu-id="5e34e-144">You can then use hello RDP file with Remote Desktop Connection tooaccess hello cloud service.</span></span>

### <a name="programmatically-download-hello-rdp-file-through-hello-service-management-rest-api"></a><span data-ttu-id="5e34e-145">Hello szolgáltatásfelügyelet REST API használatával hello RDP-fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="5e34e-145">Programmatically download hello RDP file through hello Service Management REST API</span></span>
<span data-ttu-id="5e34e-146">Használhatja a hello [RDP-fájl letöltése](https://msdn.microsoft.com/library/jj157183.aspx) REST művelet toodownload hello RDP-fájlt.</span><span class="sxs-lookup"><span data-stu-id="5e34e-146">You can use hello [Download RDP File](https://msdn.microsoft.com/library/jj157183.aspx) REST operation toodownload hello RDP file.</span></span>

## <a name="tooconfigure-remote-desktop-in-hello-service-definition-file"></a><span data-ttu-id="5e34e-147">Távoli asztal hello szolgáltatásdefiníciós fájlban tooconfigure</span><span class="sxs-lookup"><span data-stu-id="5e34e-147">tooconfigure Remote Desktop in hello service definition file</span></span>
<span data-ttu-id="5e34e-148">Ez a módszer lehetővé teszi a távoli asztal tooenable hello alkalmazás fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="5e34e-148">This method allows you tooenable Remote Desktop for hello application during development.</span></span> <span data-ttu-id="5e34e-149">Ez a megközelítés titkosított jelszavakat igényel tárolja a szolgáltatás konfigurációs fájl- és a frissítések toohello a távoli asztal konfigurálásának igényelnének egy újratelepítés hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5e34e-149">This approach requires encrypted passwords be stored in your service configuration file and any updates toohello remote desktop configuration would require a redeployment of hello application.</span></span> <span data-ttu-id="5e34e-150">Ha azt szeretné, hogy tooavoid ezeket kell használnia a távoli asztali bővítmény hello downsides alapú a fent leírt módszer.</span><span class="sxs-lookup"><span data-stu-id="5e34e-150">If you want tooavoid these downsides you should use hello remote desktop extension based approach described above.</span></span>  

<span data-ttu-id="5e34e-151">Használhatja a Visual Studio túl[engedélyezése a távoli asztali kapcsolat](../vs-azure-tools-remote-desktop-roles.md) hello szolgáltatás definíciós fájl módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="5e34e-151">You can use Visual Studio too[enable a remote desktop connection](../vs-azure-tools-remote-desktop-roles.md) using hello service definition file approach.</span></span>  
<span data-ttu-id="5e34e-152">hello következő lépések leírják hello módosítások szükséges toohello szolgáltatás modell fájlok tooenable távoli asztal.</span><span class="sxs-lookup"><span data-stu-id="5e34e-152">hello steps below describe hello changes needed toohello service model files tooenable remote desktop.</span></span> <span data-ttu-id="5e34e-153">A Visual Studio automatikusan fog teszi ezeket a módosításokat, közzétételekor.</span><span class="sxs-lookup"><span data-stu-id="5e34e-153">Visual Studio will automatically makes these changes when publishing.</span></span>

### <a name="set-up-hello-connection-in-hello-service-model"></a><span data-ttu-id="5e34e-154">Hello hello szolgáltatásmodell-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="5e34e-154">Set up hello connection in hello service model</span></span>
<span data-ttu-id="5e34e-155">Használjon hello **importálja** elem tooimport hello **RemoteAccess** modul és hello **RemoteForwarder** modul toohello [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) fájlt.</span><span class="sxs-lookup"><span data-stu-id="5e34e-155">Use hello **Imports** element tooimport hello **RemoteAccess** module and hello **RemoteForwarder** module toohello [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) file.</span></span>

<span data-ttu-id="5e34e-156">hello szolgáltatásdefiníciós fájl kell lennie a következő példa a hello hasonló toohello `<Imports>` -eleme.</span><span class="sxs-lookup"><span data-stu-id="5e34e-156">hello service definition file should be similar toohello following example with hello `<Imports>` element added.</span></span>

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
<span data-ttu-id="5e34e-157">Hello [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) fájl a következő példa hasonló toohello kell, vegye figyelembe a hello `<ConfigurationSettings>` és `<Certificates>` elemeket.</span><span class="sxs-lookup"><span data-stu-id="5e34e-157">hello [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) file should be similar toohello following example, note hello `<ConfigurationSettings>` and `<Certificates>` elements.</span></span> <span data-ttu-id="5e34e-158">a megadott tanúsítvány hello kell [feltöltve felhőszolgáltatás toohello](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="5e34e-158">hello Certificate specified must be [uploaded toohello cloud service](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).</span></span>

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


## <a name="additional-resources"></a><span data-ttu-id="5e34e-159">További források</span><span class="sxs-lookup"><span data-stu-id="5e34e-159">Additional Resources</span></span>
<span data-ttu-id="5e34e-160">[Hogyan tooConfigure Felhőszolgáltatások](cloud-services-how-to-configure.md)
[Cloud services – gyakori kérdések – a távoli asztal](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="5e34e-160">[How tooConfigure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
