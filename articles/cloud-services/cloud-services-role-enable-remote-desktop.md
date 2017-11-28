---
title: "Az Azure-Felhőszolgáltatás a távoli asztal engedélyezése |} Microsoft Docs"
description: "Az azure cloud service alkalmazás távoli asztali kapcsolatok lehetővé tételéhez konfigurálása"
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
ms.openlocfilehash: 413e72e9a39fcde84f56bfc61a6bc72dbadf1c97
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a><span data-ttu-id="e5369-103">Engedélyezze a távoli asztali kapcsolat egy szerepkör esetén az Azure Cloud Services csomag</span><span class="sxs-lookup"><span data-stu-id="e5369-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e5369-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e5369-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="e5369-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="e5369-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="e5369-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e5369-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="e5369-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5369-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)

<span data-ttu-id="e5369-108">Engedélyezheti a távoli asztali kapcsolat létrehozása a szerepkörben fejlesztése során a távoli asztal modulok belefoglalja a szolgáltatás definíciós, vagy választhatja azt is, a távoli asztal bővítményével távoli asztal engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e5369-108">You can enable a Remote Desktop connection in your role during development by including the Remote Desktop modules in your service definition or you can choose to enable Remote Desktop through the Remote Desktop Extension.</span></span> <span data-ttu-id="e5369-109">Az előnyben részesített megoldás, a távoli asztali bővítmény használatára, mert a távoli asztal az alkalmazás nem kell újratelepíteni az alkalmazás központi telepítése után is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="e5369-109">The preferred approach is to use the Remote Desktop extension as you can enable Remote Desktop even after the application is deployed without having to redeploy your application.</span></span>

## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a><span data-ttu-id="e5369-110">A távoli asztal konfigurálása a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="e5369-110">Configure Remote Desktop from the Azure classic portal</span></span>
<span data-ttu-id="e5369-111">A klasszikus Azure portálra a távoli asztali bővítmény megközelítést használ, így a távoli asztal az alkalmazás központi telepítése után is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="e5369-111">The Azure classic portal uses the Remote Desktop Extension approach so you can enable Remote Desktop even after the application is deployed.</span></span> <span data-ttu-id="e5369-112">A **konfigurálása** a felhőszolgáltatás lap lehetővé teszi a távoli asztal engedélyezéséhez módosítsa a helyi rendszergazda fiók használatával kapcsolódik a virtuális gépek, a tanúsítvány-hitelesítésben használt, és a lejárati dátuma.</span><span class="sxs-lookup"><span data-stu-id="e5369-112">The **Configure** page for your cloud service allows you to enable Remote Desktop, change the local Administrator account used to connect to the virtual machines, the certificate used in authentication and set the expiration date.</span></span>

1. <span data-ttu-id="e5369-113">Kattintson a **Felhőszolgáltatások**, kattintson a nevére, a felhőalapú szolgáltatás, és kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="e5369-113">Click **Cloud Services**, click the name of the cloud service, and then click **Configure**.</span></span>
2. <span data-ttu-id="e5369-114">Kattintson a **távoli** panel alján.</span><span class="sxs-lookup"><span data-stu-id="e5369-114">Click the **Remote** button at the bottom.</span></span>

    ![Távoli cloud services csomag](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > <span data-ttu-id="e5369-116">Összes szerepkörpéldányt újra kell indítani, amikor először a távoli asztal engedélyezése és kattintson az OK gombra (jelölő).</span><span class="sxs-lookup"><span data-stu-id="e5369-116">All role instances will be restarted when you first enable Remote Desktop and click OK (checkmark).</span></span> <span data-ttu-id="e5369-117">Újraindítás megakadályozása érdekében a jelszó titkosításához használt tanúsítványról telepítenie kell a szerepkört.</span><span class="sxs-lookup"><span data-stu-id="e5369-117">To prevent a reboot, the certificate used to encrypt the password must be installed on the role.</span></span> <span data-ttu-id="e5369-118">Egy újraindítás érdekében [töltse fel a tanúsítványt a felhőalapú szolgáltatáshoz](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) és térjen vissza ezt a párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="e5369-118">To prevent a restart, [upload a certificate for the cloud service](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) and then return to this dialog.</span></span>

3. <span data-ttu-id="e5369-119">A **szerepkörök**, válassza ki a frissíteni, vagy válassza ki a szerepkör **összes** összes szerepköre tekintetében.</span><span class="sxs-lookup"><span data-stu-id="e5369-119">In **Roles**, select the role you want to update or select **All** for all roles.</span></span>
4. <span data-ttu-id="e5369-120">Ellenőrizze a következő módosításokat:</span><span class="sxs-lookup"><span data-stu-id="e5369-120">Make any of the following changes:</span></span>

   * <span data-ttu-id="e5369-121">A távoli asztal engedélyezéséhez jelölje be a **távoli asztal engedélyezése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="e5369-121">To enable Remote Desktop, select the **Enable Remote Desktop** check box.</span></span> <span data-ttu-id="e5369-122">A távoli asztal letiltásához törölje a jelölőnégyzet jelölését.</span><span class="sxs-lookup"><span data-stu-id="e5369-122">To disable Remote Desktop, clear the check box.</span></span>
   * <span data-ttu-id="e5369-123">A távoli asztal kapcsolatokat a szerepkörpéldányok a használandó fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e5369-123">Create an account to use in Remote Desktop connections to the role instances.</span></span>
   * <span data-ttu-id="e5369-124">Frissítse a meglévő fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="e5369-124">Update the password for the existing account.</span></span>
   * <span data-ttu-id="e5369-125">A hitelesítéshez használandó feltöltött tanúsítvány kiválasztása (töltse fel a tanúsítvány használatával **feltöltése** a a **tanúsítványok** oldalon), vagy hozzon létre egy új tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="e5369-125">Select an uploaded certificate to use for authentication (upload the certificate using **Upload** on the **Certificates** page) or create a new certificate.</span></span>
   * <span data-ttu-id="e5369-126">Módosítsa a lejárati dátumot a távoli asztal konfigurációjának.</span><span class="sxs-lookup"><span data-stu-id="e5369-126">Change the expiration date for the Remote Desktop configuration.</span></span>

5. <span data-ttu-id="e5369-127">A konfigurációs módosítások befejeztével kattintson **OK** (jelölő).</span><span class="sxs-lookup"><span data-stu-id="e5369-127">When you finish your configuration updates, click **OK** (checkmark).</span></span>

## <a name="remote-into-role-instances"></a><span data-ttu-id="e5369-128">A szerepkörpéldányok távoli</span><span class="sxs-lookup"><span data-stu-id="e5369-128">Remote into role instances</span></span>
<span data-ttu-id="e5369-129">A távoli asztal engedélyezve van a szerepkörök a következőket teheti, miután távoli olyan szerepkörpéldányt, különböző eszközök használatával történő.</span><span class="sxs-lookup"><span data-stu-id="e5369-129">Once Remote Desktop is enabled on the roles you can remote into a role instance through various tools.</span></span>

<span data-ttu-id="e5369-130">A szerepkör példánya csatlakoztatja a klasszikus Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="e5369-130">To connect to a role instance from the Azure classic portal:</span></span>

1. <span data-ttu-id="e5369-131">Kattintson a **példányok** megnyitásához a **példányok** lap.</span><span class="sxs-lookup"><span data-stu-id="e5369-131">Click **Instances** to open the **Instances** page.</span></span>
2. <span data-ttu-id="e5369-132">Válassza ki a szerepkör példánya, amely a távoli asztal konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="e5369-132">Select a role instance that has Remote Desktop configured.</span></span>
3. <span data-ttu-id="e5369-133">Kattintson a **Connect**, és kövesse az utasításokat megnyitásához az asztalon.</span><span class="sxs-lookup"><span data-stu-id="e5369-133">Click **Connect**, and follow the instructions to open the desktop.</span></span>
4. <span data-ttu-id="e5369-134">Kattintson a **nyitott** , majd **Connect** a távoli asztali kapcsolat elindításához.</span><span class="sxs-lookup"><span data-stu-id="e5369-134">Click **Open** and then **Connect** to start the Remote Desktop connection.</span></span>

### <a name="use-visual-studio-to-remote-into-a-role-instance"></a><span data-ttu-id="e5369-135">Visual Studio használata a távoli be a szerepkör példánya</span><span class="sxs-lookup"><span data-stu-id="e5369-135">Use Visual Studio to remote into a role instance</span></span>
<span data-ttu-id="e5369-136">A Visual Studio Server Explorer:</span><span class="sxs-lookup"><span data-stu-id="e5369-136">In Visual Studio, Server Explorer:</span></span>

1. <span data-ttu-id="e5369-137">Bontsa ki a **Azure** > **Felhőszolgáltatások** > **[felhőszolgáltatás neve]** csomópont.</span><span class="sxs-lookup"><span data-stu-id="e5369-137">Expand the **Azure** > **Cloud Services** > **[cloud service name]** node.</span></span>
2. <span data-ttu-id="e5369-138">Bontsa ki **átmeneti** vagy **éles**.</span><span class="sxs-lookup"><span data-stu-id="e5369-138">Expand either **Staging** or **Production**.</span></span>
3. <span data-ttu-id="e5369-139">Bontsa ki az egyes szerepkör.</span><span class="sxs-lookup"><span data-stu-id="e5369-139">Expand the individual role.</span></span>
4. <span data-ttu-id="e5369-140">Kattintson a jobb gombbal a szerepkörpéldányt beállítani, kattintson a **csatlakozzon a távoli asztali kapcsolattal...** , majd adja meg a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="e5369-140">Right-click one of the role instances, click **Connect using Remote Desktop...**, and then enter the user name and password.</span></span>

![Server explorer távoli asztal](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-to-get-the-rdp-file"></a><span data-ttu-id="e5369-142">Az RDP-fájl lekérése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="e5369-142">Use PowerShell to get the RDP file</span></span>
<span data-ttu-id="e5369-143">Használhatja a [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) parancsmag segítségével kérje le a RDP-fájlt.</span><span class="sxs-lookup"><span data-stu-id="e5369-143">You can use the [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet to retrieve the RDP file.</span></span> <span data-ttu-id="e5369-144">Ezután használhatja az RDP-fájlt a távoli asztali kapcsolat a felhőalapú szolgáltatás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e5369-144">You can then use the RDP file with Remote Desktop Connection to access the cloud service.</span></span>

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a><span data-ttu-id="e5369-145">A Service Management REST API-n keresztül az RDP-fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="e5369-145">Programmatically download the RDP file through the Service Management REST API</span></span>
<span data-ttu-id="e5369-146">Használhatja a [RDP-fájl letöltése](https://msdn.microsoft.com/library/jj157183.aspx) REST-művelet az RDP-fájl letöltésére.</span><span class="sxs-lookup"><span data-stu-id="e5369-146">You can use the [Download RDP File](https://msdn.microsoft.com/library/jj157183.aspx) REST operation to download the RDP file.</span></span>

## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a><span data-ttu-id="e5369-147">A távoli asztal konfigurálása a szolgáltatásdefiníciós fájlban</span><span class="sxs-lookup"><span data-stu-id="e5369-147">To configure Remote Desktop in the service definition file</span></span>
<span data-ttu-id="e5369-148">Ez a módszer lehetővé teszi a távoli asztal engedélyezése az alkalmazás fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="e5369-148">This method allows you to enable Remote Desktop for the application during development.</span></span> <span data-ttu-id="e5369-149">Ez a megközelítés titkosított jelszavakat igényel tárolható szolgáltatás konfigurációs fájl- és a távoli asztal konfigurálásának frissítéseit az alkalmazás egy újratelepítés igényelnének.</span><span class="sxs-lookup"><span data-stu-id="e5369-149">This approach requires encrypted passwords be stored in your service configuration file and any updates to the remote desktop configuration would require a redeployment of the application.</span></span> <span data-ttu-id="e5369-150">Ha el szeretné kerülni a downsides a fent leírt-alapú távoli asztali bővítmény megközelítést kell használnia.</span><span class="sxs-lookup"><span data-stu-id="e5369-150">If you want to avoid these downsides you should use the remote desktop extension based approach described above.</span></span>  

<span data-ttu-id="e5369-151">Használhatja a Visual Studio [engedélyezése a távoli asztali kapcsolat](../vs-azure-tools-remote-desktop-roles.md) a szolgáltatás definíciós fájl módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="e5369-151">You can use Visual Studio to [enable a remote desktop connection](../vs-azure-tools-remote-desktop-roles.md) using the service definition file approach.</span></span>  
<span data-ttu-id="e5369-152">A következő lépések leírják a távoli asztal engedélyezése a szolgáltatásmodell-fájlokból szükséges módosításokat.</span><span class="sxs-lookup"><span data-stu-id="e5369-152">The steps below describe the changes needed to the service model files to enable remote desktop.</span></span> <span data-ttu-id="e5369-153">A Visual Studio automatikusan fog teszi ezeket a módosításokat, közzétételekor.</span><span class="sxs-lookup"><span data-stu-id="e5369-153">Visual Studio will automatically makes these changes when publishing.</span></span>

### <a name="set-up-the-connection-in-the-service-model"></a><span data-ttu-id="e5369-154">A szolgáltatásmodell a kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="e5369-154">Set up the connection in the service model</span></span>
<span data-ttu-id="e5369-155">Használja a **importálja** elem importálásához a **RemoteAccess** modul és a **RemoteForwarder** modult a [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) fájl.</span><span class="sxs-lookup"><span data-stu-id="e5369-155">Use the **Imports** element to import the **RemoteAccess** module and the **RemoteForwarder** module to the [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) file.</span></span>

<span data-ttu-id="e5369-156">A szolgáltatásdefiníciós fájlban az alábbi példához hasonló legyen a `<Imports>` -eleme.</span><span class="sxs-lookup"><span data-stu-id="e5369-156">The service definition file should be similar to the following example with the `<Imports>` element added.</span></span>

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
<span data-ttu-id="e5369-157">A [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) fájl kell az alábbi példához hasonló, vegye figyelembe a `<ConfigurationSettings>` és `<Certificates>` elemeket.</span><span class="sxs-lookup"><span data-stu-id="e5369-157">The [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) file should be similar to the following example, note the `<ConfigurationSettings>` and `<Certificates>` elements.</span></span> <span data-ttu-id="e5369-158">A megadott tanúsítványt kell [feltöltve felhőszolgáltatás](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="e5369-158">The Certificate specified must be [uploaded to the cloud service](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).</span></span>

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


## <a name="additional-resources"></a><span data-ttu-id="e5369-159">További források</span><span class="sxs-lookup"><span data-stu-id="e5369-159">Additional Resources</span></span>
<span data-ttu-id="e5369-160">[Felhőalapú szolgáltatások konfigurálása](cloud-services-how-to-configure.md)
[Cloud services – gyakori kérdések – a távoli asztal](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="e5369-160">[How to Configure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
