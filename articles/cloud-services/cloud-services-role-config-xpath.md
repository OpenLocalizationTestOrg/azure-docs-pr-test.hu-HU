---
title: "aaaCloud szolgáltatások szerepkör konfigurációs XPath Adatlap |} Microsoft Docs"
description: "hello is használhatja a hello felhőalapú szolgáltatás szerepkör konfigurációs tooexpose beállításaiban, egy környezeti változó különböző XPath beállításokat."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c51e4493-0643-4d05-bc44-06c76bcbf7d1
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 27f98f956a1c790c9bb30f9fefe1ab1736b2b150
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a><span data-ttu-id="41f76-103">XPath környezeti változó, teszi közzé a szerepkör konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="41f76-103">Expose role configuration settings as an environment variable with XPath</span></span>
<span data-ttu-id="41f76-104">Hello cloud service munkavégző vagy a webes szerepkör szolgáltatásdefiníciós fájl teszi közzé futásidejű konfigurációs értékeket környezeti változóként.</span><span class="sxs-lookup"><span data-stu-id="41f76-104">In hello cloud service worker or web role service definition file, you can expose runtime configuration values as environment variables.</span></span> <span data-ttu-id="41f76-105">a következő XPath értékek hello (amely tooAPI értékek) támogatottak.</span><span class="sxs-lookup"><span data-stu-id="41f76-105">hello following XPath values are supported (which correspond tooAPI values).</span></span>

<span data-ttu-id="41f76-106">Ezeket az XPath értékeket is elérhetők hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="41f76-106">These XPath values are also available through hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) library.</span></span> 

## <a name="app-running-in-emulator"></a><span data-ttu-id="41f76-107">Emulátoron futó alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="41f76-107">App running in emulator</span></span>
<span data-ttu-id="41f76-108">Azt jelzi, hogy adott hello az alkalmazás fut. hello emulátorban.</span><span class="sxs-lookup"><span data-stu-id="41f76-108">Indicates that hello app is running in hello emulator.</span></span>

| <span data-ttu-id="41f76-109">Típus</span><span class="sxs-lookup"><span data-stu-id="41f76-109">Type</span></span> | <span data-ttu-id="41f76-110">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-110">Example</span></span> |
| --- | --- |
| <span data-ttu-id="41f76-111">XPath</span><span class="sxs-lookup"><span data-stu-id="41f76-111">XPath</span></span> |<span data-ttu-id="41f76-112">XPath = "/RoleEnvironment/Deployment/@emulated"</span><span class="sxs-lookup"><span data-stu-id="41f76-112">xpath="/RoleEnvironment/Deployment/@emulated"</span></span> |
| <span data-ttu-id="41f76-113">Kód</span><span class="sxs-lookup"><span data-stu-id="41f76-113">Code</span></span> |<span data-ttu-id="41f76-114">var x = RoleEnvironment.IsEmulated;</span><span class="sxs-lookup"><span data-stu-id="41f76-114">var x = RoleEnvironment.IsEmulated;</span></span> |

## <a name="deployment-id"></a><span data-ttu-id="41f76-115">Központi telepítési azonosítója</span><span class="sxs-lookup"><span data-stu-id="41f76-115">Deployment ID</span></span>
<span data-ttu-id="41f76-116">Lekéri a hello példány hello üzembehelyezés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="41f76-116">Retrieves hello deployment ID for hello instance.</span></span>

| <span data-ttu-id="41f76-117">Típus</span><span class="sxs-lookup"><span data-stu-id="41f76-117">Type</span></span> | <span data-ttu-id="41f76-118">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-118">Example</span></span> |
| --- | --- |
| <span data-ttu-id="41f76-119">XPath</span><span class="sxs-lookup"><span data-stu-id="41f76-119">XPath</span></span> |<span data-ttu-id="41f76-120">XPath = "/RoleEnvironment/Deployment/@id"</span><span class="sxs-lookup"><span data-stu-id="41f76-120">xpath="/RoleEnvironment/Deployment/@id"</span></span> |
| <span data-ttu-id="41f76-121">Kód</span><span class="sxs-lookup"><span data-stu-id="41f76-121">Code</span></span> |<span data-ttu-id="41f76-122">var deploymentId = RoleEnvironment.DeploymentId;</span><span class="sxs-lookup"><span data-stu-id="41f76-122">var deploymentId = RoleEnvironment.DeploymentId;</span></span> |

## <a name="role-id"></a><span data-ttu-id="41f76-123">Szerepkör-azonosítója</span><span class="sxs-lookup"><span data-stu-id="41f76-123">Role ID</span></span>
<span data-ttu-id="41f76-124">Lekéri a hello aktuális szerepkör-Azonosítót hello példány.</span><span class="sxs-lookup"><span data-stu-id="41f76-124">Retrieves hello current role ID for hello instance.</span></span>

| <span data-ttu-id="41f76-125">Típus</span><span class="sxs-lookup"><span data-stu-id="41f76-125">Type</span></span> | <span data-ttu-id="41f76-126">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-126">Example</span></span> |
| --- | --- |
| <span data-ttu-id="41f76-127">XPath</span><span class="sxs-lookup"><span data-stu-id="41f76-127">XPath</span></span> |<span data-ttu-id="41f76-128">XPath = "/RoleEnvironment/CurrentInstance/@id"</span><span class="sxs-lookup"><span data-stu-id="41f76-128">xpath="/RoleEnvironment/CurrentInstance/@id"</span></span> |
| <span data-ttu-id="41f76-129">Kód</span><span class="sxs-lookup"><span data-stu-id="41f76-129">Code</span></span> |<span data-ttu-id="41f76-130">var azonosítója = RoleEnvironment.CurrentRoleInstance.Id;</span><span class="sxs-lookup"><span data-stu-id="41f76-130">var id = RoleEnvironment.CurrentRoleInstance.Id;</span></span> |

## <a name="update-domain"></a><span data-ttu-id="41f76-131">Tartomány frissítése</span><span class="sxs-lookup"><span data-stu-id="41f76-131">Update domain</span></span>
<span data-ttu-id="41f76-132">Lekéri a hello frissítési tartomány hello példány.</span><span class="sxs-lookup"><span data-stu-id="41f76-132">Retrieves hello update domain of hello instance.</span></span>

| <span data-ttu-id="41f76-133">Típus</span><span class="sxs-lookup"><span data-stu-id="41f76-133">Type</span></span> | <span data-ttu-id="41f76-134">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-134">Example</span></span> |
| --- | --- |
| <span data-ttu-id="41f76-135">XPath</span><span class="sxs-lookup"><span data-stu-id="41f76-135">XPath</span></span> |<span data-ttu-id="41f76-136">XPath = "/RoleEnvironment/CurrentInstance/@updateDomain"</span><span class="sxs-lookup"><span data-stu-id="41f76-136">xpath="/RoleEnvironment/CurrentInstance/@updateDomain"</span></span> |
| <span data-ttu-id="41f76-137">Kód</span><span class="sxs-lookup"><span data-stu-id="41f76-137">Code</span></span> |<span data-ttu-id="41f76-138">var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain;</span><span class="sxs-lookup"><span data-stu-id="41f76-138">var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain;</span></span> |

## <a name="fault-domain"></a><span data-ttu-id="41f76-139">Hibatartomány</span><span class="sxs-lookup"><span data-stu-id="41f76-139">Fault domain</span></span>
<span data-ttu-id="41f76-140">Lekéri a hello tartalék tartomány hello példány.</span><span class="sxs-lookup"><span data-stu-id="41f76-140">Retrieves hello fault domain of hello instance.</span></span>

| <span data-ttu-id="41f76-141">Típus</span><span class="sxs-lookup"><span data-stu-id="41f76-141">Type</span></span> | <span data-ttu-id="41f76-142">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-142">Example</span></span> |
| --- | --- |
| <span data-ttu-id="41f76-143">XPath</span><span class="sxs-lookup"><span data-stu-id="41f76-143">XPath</span></span> |<span data-ttu-id="41f76-144">XPath = "/RoleEnvironment/CurrentInstance/@faultDomain"</span><span class="sxs-lookup"><span data-stu-id="41f76-144">xpath="/RoleEnvironment/CurrentInstance/@faultDomain"</span></span> |
| <span data-ttu-id="41f76-145">Kód</span><span class="sxs-lookup"><span data-stu-id="41f76-145">Code</span></span> |<span data-ttu-id="41f76-146">var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain;</span><span class="sxs-lookup"><span data-stu-id="41f76-146">var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain;</span></span> |

## <a name="role-name"></a><span data-ttu-id="41f76-147">Szerepkör neve</span><span class="sxs-lookup"><span data-stu-id="41f76-147">Role name</span></span>
<span data-ttu-id="41f76-148">Hello példányok hello szerepkör nevét kéri le.</span><span class="sxs-lookup"><span data-stu-id="41f76-148">Retrieves hello role name of hello instances.</span></span>

| <span data-ttu-id="41f76-149">Típus</span><span class="sxs-lookup"><span data-stu-id="41f76-149">Type</span></span> | <span data-ttu-id="41f76-150">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-150">Example</span></span> |
| --- | --- |
| <span data-ttu-id="41f76-151">XPath</span><span class="sxs-lookup"><span data-stu-id="41f76-151">XPath</span></span> |<span data-ttu-id="41f76-152">XPath = "/RoleEnvironment/CurrentInstance/@roleName"</span><span class="sxs-lookup"><span data-stu-id="41f76-152">xpath="/RoleEnvironment/CurrentInstance/@roleName"</span></span> |
| <span data-ttu-id="41f76-153">Kód</span><span class="sxs-lookup"><span data-stu-id="41f76-153">Code</span></span> |<span data-ttu-id="41f76-154">var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;</span><span class="sxs-lookup"><span data-stu-id="41f76-154">var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;</span></span> |

## <a name="config-setting"></a><span data-ttu-id="41f76-155">Konfigurációs beállítás</span><span class="sxs-lookup"><span data-stu-id="41f76-155">Config setting</span></span>
<span data-ttu-id="41f76-156">Lekéri hello értéket hello adott konfigurációs beállítás.</span><span class="sxs-lookup"><span data-stu-id="41f76-156">Retrieves hello value of hello specified configuration setting.</span></span>

| <span data-ttu-id="41f76-157">Típus</span><span class="sxs-lookup"><span data-stu-id="41f76-157">Type</span></span> | <span data-ttu-id="41f76-158">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-158">Example</span></span> |
| --- | --- |
| <span data-ttu-id="41f76-159">XPath</span><span class="sxs-lookup"><span data-stu-id="41f76-159">XPath</span></span> |<span data-ttu-id="41f76-160">XPath = "/ roleenvironment-et vagy a CurrentInstance/ConfigurationSettings/ConfigurationSetting [@name= 'Setting1']/@value"</span><span class="sxs-lookup"><span data-stu-id="41f76-160">xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value"</span></span> |
| <span data-ttu-id="41f76-161">Kód</span><span class="sxs-lookup"><span data-stu-id="41f76-161">Code</span></span> |<span data-ttu-id="41f76-162">var beállítás = RoleEnvironment.GetConfigurationSettingValue("Setting1");</span><span class="sxs-lookup"><span data-stu-id="41f76-162">var setting = RoleEnvironment.GetConfigurationSettingValue("Setting1");</span></span> |

## <a name="local-storage-path"></a><span data-ttu-id="41f76-163">Tárolási helyi elérési útja</span><span class="sxs-lookup"><span data-stu-id="41f76-163">Local storage path</span></span>
<span data-ttu-id="41f76-164">Lekéri a hello példány hello helyi tárolási elérési útja.</span><span class="sxs-lookup"><span data-stu-id="41f76-164">Retrieves hello local storage path for hello instance.</span></span>

| <span data-ttu-id="41f76-165">Típus</span><span class="sxs-lookup"><span data-stu-id="41f76-165">Type</span></span> | <span data-ttu-id="41f76-166">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-166">Example</span></span> |
| --- | --- |
| <span data-ttu-id="41f76-167">XPath</span><span class="sxs-lookup"><span data-stu-id="41f76-167">XPath</span></span> |<span data-ttu-id="41f76-168">XPath = "/ roleenvironment-et vagy a CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@path"</span><span class="sxs-lookup"><span data-stu-id="41f76-168">xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path"</span></span> |
| <span data-ttu-id="41f76-169">Kód</span><span class="sxs-lookup"><span data-stu-id="41f76-169">Code</span></span> |<span data-ttu-id="41f76-170">var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath;</span><span class="sxs-lookup"><span data-stu-id="41f76-170">var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1").RootPath;</span></span> |

## <a name="local-storage-size"></a><span data-ttu-id="41f76-171">Helyi tárterület mérete</span><span class="sxs-lookup"><span data-stu-id="41f76-171">Local storage size</span></span>
<span data-ttu-id="41f76-172">Lekéri a hello helyi tároló hello példány hello méretét.</span><span class="sxs-lookup"><span data-stu-id="41f76-172">Retrieves hello size of hello local storage for hello instance.</span></span>

| <span data-ttu-id="41f76-173">Típus</span><span class="sxs-lookup"><span data-stu-id="41f76-173">Type</span></span> | <span data-ttu-id="41f76-174">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-174">Example</span></span> |
| --- | --- |
| <span data-ttu-id="41f76-175">XPath</span><span class="sxs-lookup"><span data-stu-id="41f76-175">XPath</span></span> |<span data-ttu-id="41f76-176">XPath = "/ roleenvironment-et vagy a CurrentInstance/LocalResources/LocalResource [@name= 'LocalStore1']/@sizeInMB"</span><span class="sxs-lookup"><span data-stu-id="41f76-176">xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB"</span></span> |
| <span data-ttu-id="41f76-177">Kód</span><span class="sxs-lookup"><span data-stu-id="41f76-177">Code</span></span> |<span data-ttu-id="41f76-178">var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes;</span><span class="sxs-lookup"><span data-stu-id="41f76-178">var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1").MaximumSizeInMegabytes;</span></span> |

## <a name="endpoint-protocol"></a><span data-ttu-id="41f76-179">Végpont protokoll</span><span class="sxs-lookup"><span data-stu-id="41f76-179">Endpoint protocol</span></span>
<span data-ttu-id="41f76-180">Lekéri a hello végpont protokoll hello példányhoz.</span><span class="sxs-lookup"><span data-stu-id="41f76-180">Retrieves hello endpoint protocol for hello instance.</span></span>

| <span data-ttu-id="41f76-181">Típus</span><span class="sxs-lookup"><span data-stu-id="41f76-181">Type</span></span> | <span data-ttu-id="41f76-182">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-182">Example</span></span> |
| --- | --- |
| <span data-ttu-id="41f76-183">XPath</span><span class="sxs-lookup"><span data-stu-id="41f76-183">XPath</span></span> |<span data-ttu-id="41f76-184">XPath = "/ roleenvironment-et/CurrentInstance/végpont vagy végpont [@name= 'Endpoint1']/@protocol"</span><span class="sxs-lookup"><span data-stu-id="41f76-184">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol"</span></span> |
| <span data-ttu-id="41f76-185">Kód</span><span class="sxs-lookup"><span data-stu-id="41f76-185">Code</span></span> |<span data-ttu-id="41f76-186">var prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protokoll;</span><span class="sxs-lookup"><span data-stu-id="41f76-186">var prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].Protocol;</span></span> |

## <a name="endpoint-ip"></a><span data-ttu-id="41f76-187">Végpont IP</span><span class="sxs-lookup"><span data-stu-id="41f76-187">Endpoint IP</span></span>
<span data-ttu-id="41f76-188">Lekérdezi hello megadott végpont IP-címet.</span><span class="sxs-lookup"><span data-stu-id="41f76-188">Gets hello specified endpoint's IP address.</span></span>

| <span data-ttu-id="41f76-189">Típus</span><span class="sxs-lookup"><span data-stu-id="41f76-189">Type</span></span> | <span data-ttu-id="41f76-190">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-190">Example</span></span> |
| --- | --- |
| <span data-ttu-id="41f76-191">XPath</span><span class="sxs-lookup"><span data-stu-id="41f76-191">XPath</span></span> |<span data-ttu-id="41f76-192">XPath = "/ roleenvironment-et/CurrentInstance/végpont vagy végpont [@name= 'Endpoint1']/@address"</span><span class="sxs-lookup"><span data-stu-id="41f76-192">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address"</span></span> |
| <span data-ttu-id="41f76-193">Kód</span><span class="sxs-lookup"><span data-stu-id="41f76-193">Code</span></span> |<span data-ttu-id="41f76-194">var cím = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address</span><span class="sxs-lookup"><span data-stu-id="41f76-194">var address = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].IPEndpoint.Address</span></span> |

## <a name="endpoint-port"></a><span data-ttu-id="41f76-195">Végpont portja</span><span class="sxs-lookup"><span data-stu-id="41f76-195">Endpoint port</span></span>
<span data-ttu-id="41f76-196">Lekéri a hello végponti port hello példányhoz.</span><span class="sxs-lookup"><span data-stu-id="41f76-196">Retrieves hello endpoint port for hello instance.</span></span>

| <span data-ttu-id="41f76-197">Típus</span><span class="sxs-lookup"><span data-stu-id="41f76-197">Type</span></span> | <span data-ttu-id="41f76-198">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-198">Example</span></span> |
| --- | --- |
| <span data-ttu-id="41f76-199">XPath</span><span class="sxs-lookup"><span data-stu-id="41f76-199">XPath</span></span> |<span data-ttu-id="41f76-200">XPath = "/ roleenvironment-et/CurrentInstance/végpont vagy végpont [@name= 'Endpoint1']/@port"</span><span class="sxs-lookup"><span data-stu-id="41f76-200">xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port"</span></span> |
| <span data-ttu-id="41f76-201">Kód</span><span class="sxs-lookup"><span data-stu-id="41f76-201">Code</span></span> |<span data-ttu-id="41f76-202">var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port;</span><span class="sxs-lookup"><span data-stu-id="41f76-202">var port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"].IPEndpoint.Port;</span></span> |

## <a name="example"></a><span data-ttu-id="41f76-203">Példa</span><span class="sxs-lookup"><span data-stu-id="41f76-203">Example</span></span>
<span data-ttu-id="41f76-204">A feldolgozói szerepkör, amely egy indítási tevékenységhez hoz létre egy környezeti változó neve, például `TestIsEmulated` toohello beállítása [ @emulated xpath értékét](#app-running-in-emulator).</span><span class="sxs-lookup"><span data-stu-id="41f76-204">Here is an example of a worker role that creates a startup task with an environment variable named `TestIsEmulated` set toohello [@emulated xpath value](#app-running-in-emulator).</span></span> 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a><span data-ttu-id="41f76-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41f76-205">Next steps</span></span>
<span data-ttu-id="41f76-206">További tudnivalók hello [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) fájlt.</span><span class="sxs-lookup"><span data-stu-id="41f76-206">Learn more about hello [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) file.</span></span>

<span data-ttu-id="41f76-207">Hozzon létre egy [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) csomag.</span><span class="sxs-lookup"><span data-stu-id="41f76-207">Create a [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) package.</span></span>

<span data-ttu-id="41f76-208">Engedélyezése [távoli asztal](cloud-services-role-enable-remote-desktop.md) szerepkör.</span><span class="sxs-lookup"><span data-stu-id="41f76-208">Enable [remote desktop](cloud-services-role-enable-remote-desktop.md) for a role.</span></span>

