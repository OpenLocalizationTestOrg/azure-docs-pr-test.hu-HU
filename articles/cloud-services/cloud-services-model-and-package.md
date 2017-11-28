---
title: "aaaWhat egy felhőalapú szolgáltatás modell és csomag |} Microsoft Docs"
description: "Ismerteti a hello felhő szolgáltatásmodell (.csdef, .cscfg) és a csomagba (.cspkg) az Azure-ban"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4ce2feb5-0437-496c-98da-1fb6dcb7f59e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 5280cdca4810859b6afdbbe1359fc2fabe871894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-cloud-service-model-and-how-do-i-package-it"></a><span data-ttu-id="80d7d-103">Hello Felhőszolgáltatás modell és hogyan tegye csomag azt?</span><span class="sxs-lookup"><span data-stu-id="80d7d-103">What is hello Cloud Service model and how do I package it?</span></span>
<span data-ttu-id="80d7d-104">Egy felhőalapú szolgáltatás létrehozása az három összetevővel, hello szolgáltatásdefiníció *(.csdef)*, hello szolgáltatás konfigurációs *(.cscfg)*, és a szolgáltatáscsomagot *(.cspkg)*.</span><span class="sxs-lookup"><span data-stu-id="80d7d-104">A cloud service is created from three components, hello service definition *(.csdef)*, hello service config *(.cscfg)*, and a service package *(.cspkg)*.</span></span> <span data-ttu-id="80d7d-105">Mindkét hello **ServiceDefinition.csdef** és **ServiceConfig.cscfg** fájlok XML-alapú, és együttesen: hello modell hello felhőalapú szolgáltatás, és hogyan van konfigurálva; hello szerkezete ismertetik.</span><span class="sxs-lookup"><span data-stu-id="80d7d-105">Both hello **ServiceDefinition.csdef** and **ServiceConfig.cscfg** files are XML-based and describe hello structure of hello cloud service and how it's configured; collectively called hello model.</span></span> <span data-ttu-id="80d7d-106">Hello **ServicePackage.cspkg** egy zip-fájl, amely hello jönnek létre **ServiceDefinition.csdef** és többek között az összes szükséges hello bináris alapú függőségeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="80d7d-106">hello **ServicePackage.cspkg** is a zip file that is generated from hello **ServiceDefinition.csdef** and among other things, contains all hello required binary-based dependencies.</span></span> <span data-ttu-id="80d7d-107">Azure létrehoz egy felhőalapú szolgáltatás mindkét hello **ServicePackage.cspkg** és hello **ServiceConfig.cscfg**.</span><span class="sxs-lookup"><span data-stu-id="80d7d-107">Azure creates a cloud service from both hello **ServicePackage.cspkg** and hello **ServiceConfig.cscfg**.</span></span>

<span data-ttu-id="80d7d-108">Miután hello felhőalapú szolgáltatás fut az Azure-ban, konfigurálhatja újra keresztül hello **ServiceConfig.cscfg** fájlt, de hello definíciója nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="80d7d-108">Once hello cloud service is running in Azure, you can reconfigure it through hello **ServiceConfig.cscfg** file, but you cannot alter hello definition.</span></span>

## <a name="what-would-you-like-tooknow-more-about"></a><span data-ttu-id="80d7d-109">Mit szeretne többet tooknow?</span><span class="sxs-lookup"><span data-stu-id="80d7d-109">What would you like tooknow more about?</span></span>
* <span data-ttu-id="80d7d-110">További információk hello tooknow kívánt [ServiceDefinition.csdef](#csdef) és [ServiceConfig.cscfg](#cscfg) fájlokat.</span><span class="sxs-lookup"><span data-stu-id="80d7d-110">I want tooknow more about hello [ServiceDefinition.csdef](#csdef) and [ServiceConfig.cscfg](#cscfg) files.</span></span>
* <span data-ttu-id="80d7d-111">Arról, hogy már ismeri, adja meg [néhány példa](#next-steps) a Mi lehet konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="80d7d-111">I already know about that, give me [some examples](#next-steps) on what I can configure.</span></span>
* <span data-ttu-id="80d7d-112">Szeretném toocreate hello [ServicePackage.cspkg](#cspkg).</span><span class="sxs-lookup"><span data-stu-id="80d7d-112">I want toocreate hello [ServicePackage.cspkg](#cspkg).</span></span>
* <span data-ttu-id="80d7d-113">Visual Studio használok, és szeretnék...</span><span class="sxs-lookup"><span data-stu-id="80d7d-113">I am using Visual Studio and I want to...</span></span>
  * <span data-ttu-id="80d7d-114">[Felhőalapú szolgáltatás létrehozása][vs_create]</span><span class="sxs-lookup"><span data-stu-id="80d7d-114">[Create a cloud service][vs_create]</span></span>
  * <span data-ttu-id="80d7d-115">[Egy meglévő felhőszolgáltatáshoz újrakonfigurálása][vs_reconfigure]</span><span class="sxs-lookup"><span data-stu-id="80d7d-115">[Reconfigure an existing cloud service][vs_reconfigure]</span></span>
  * <span data-ttu-id="80d7d-116">[Egy Felhőszolgáltatás-projekt telepítése][vs_deploy]</span><span class="sxs-lookup"><span data-stu-id="80d7d-116">[Deploy a Cloud Service project][vs_deploy]</span></span>
  * <span data-ttu-id="80d7d-117">[Távoli asztali kapcsolatot a felhő példánya.][remotedesktop]</span><span class="sxs-lookup"><span data-stu-id="80d7d-117">[Remote desktop into a cloud service instance][remotedesktop]</span></span>

<a name="csdef"></a>

## <a name="servicedefinitioncsdef"></a><span data-ttu-id="80d7d-118">ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="80d7d-118">ServiceDefinition.csdef</span></span>
<span data-ttu-id="80d7d-119">Hello **ServiceDefinition.csdef** fájl határozza meg az Azure tooconfigure által használt hello beállításokat egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="80d7d-119">hello **ServiceDefinition.csdef** file specifies hello settings that are used by Azure tooconfigure a cloud service.</span></span> <span data-ttu-id="80d7d-120">Hello [Azure szolgáltatás definíciós séma (.csdef fájl)](https://msdn.microsoft.com/library/azure/ee758711.aspx) hello engedélyezett formátum biztosít a szolgáltatásdefiníciós fájlban.</span><span class="sxs-lookup"><span data-stu-id="80d7d-120">hello [Azure Service Definition Schema (.csdef File)](https://msdn.microsoft.com/library/azure/ee758711.aspx) provides hello allowable format for a service definition file.</span></span> <span data-ttu-id="80d7d-121">hello alábbi példa bemutatja, amelyek az hello webes és feldolgozói szerepkörök hello-beállítások:</span><span class="sxs-lookup"><span data-stu-id="80d7d-121">hello following example shows hello settings that can be defined for hello Web and Worker roles:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

<span data-ttu-id="80d7d-122">Olvassa el a toohello [szolgáltatás definíciós séma](https://msdn.microsoft.com/library/azure/ee758711.aspx) kra itt használt hello XML-séma, azonban itt van néhány hello elemek gyors magyarázata:</span><span class="sxs-lookup"><span data-stu-id="80d7d-122">You can refer toohello [Service Definition Schema](https://msdn.microsoft.com/library/azure/ee758711.aspx) for a better understanding of hello XML schema used here, however, here is a quick explanation of some of hello elements:</span></span>

<span data-ttu-id="80d7d-123">**Webhelyek**</span><span class="sxs-lookup"><span data-stu-id="80d7d-123">**Sites**</span></span>  
<span data-ttu-id="80d7d-124">Webhelyekhez vagy webes alkalmazásokhoz, amelyek az IIS7 hello definícióit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="80d7d-124">Contains hello definitions for websites or web applications that are hosted in IIS7.</span></span>

<span data-ttu-id="80d7d-125">**InputEndpoints**</span><span class="sxs-lookup"><span data-stu-id="80d7d-125">**InputEndpoints**</span></span>  
<span data-ttu-id="80d7d-126">-Végpontokat hello definícióját tartalmazza, toocontact hello felhőalapú szolgáltatás használt.</span><span class="sxs-lookup"><span data-stu-id="80d7d-126">Contains hello definitions for endpoints that are used toocontact hello cloud service.</span></span>

<span data-ttu-id="80d7d-127">**InternalEndpoints**</span><span class="sxs-lookup"><span data-stu-id="80d7d-127">**InternalEndpoints**</span></span>  
<span data-ttu-id="80d7d-128">Szerepkör példányok toocommunicate egymással által használt hello definícióit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="80d7d-128">Contains hello definitions for endpoints that are used by role instances toocommunicate with each other.</span></span>

<span data-ttu-id="80d7d-129">**ConfigurationSettings**</span><span class="sxs-lookup"><span data-stu-id="80d7d-129">**ConfigurationSettings**</span></span>  
<span data-ttu-id="80d7d-130">Egy adott szerepkör szolgáltatások hello beállítás definícióit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="80d7d-130">Contains hello setting definitions for features of a specific role.</span></span>

<span data-ttu-id="80d7d-131">**Tanúsítványok**</span><span class="sxs-lookup"><span data-stu-id="80d7d-131">**Certificates**</span></span>  
<span data-ttu-id="80d7d-132">A tanúsítványok, a szerepkör szükséges hello definícióit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="80d7d-132">Contains hello definitions for certificates that are needed for a role.</span></span> <span data-ttu-id="80d7d-133">hello előző példakód bemutatja a hello konfigurálása Azure Connect használt tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="80d7d-133">hello previous code example shows a certificate that is used for hello configuration of Azure Connect.</span></span>

<span data-ttu-id="80d7d-134">**LocalResources**</span><span class="sxs-lookup"><span data-stu-id="80d7d-134">**LocalResources**</span></span>  
<span data-ttu-id="80d7d-135">Helyi tároló-erőforrások hello definícióit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="80d7d-135">Contains hello definitions for local storage resources.</span></span> <span data-ttu-id="80d7d-136">A helyi tároló egyik erőforrásához hello fájlrendszerben hello virtuális gép szerepkör példánya fut. fenntartott könyvtár.</span><span class="sxs-lookup"><span data-stu-id="80d7d-136">A local storage resource is a reserved directory on hello file system of hello virtual machine in which an instance of a role is running.</span></span>

<span data-ttu-id="80d7d-137">**Importálása**</span><span class="sxs-lookup"><span data-stu-id="80d7d-137">**Imports**</span></span>  
<span data-ttu-id="80d7d-138">Importált modulok hello definícióit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="80d7d-138">Contains hello definitions for imported modules.</span></span> <span data-ttu-id="80d7d-139">hello előző példakódban hello modulok távoli asztali kapcsolat és az Azure Connect jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="80d7d-139">hello previous code example shows hello modules for Remote Desktop Connection and Azure Connect.</span></span>

<span data-ttu-id="80d7d-140">**Indítása**</span><span class="sxs-lookup"><span data-stu-id="80d7d-140">**Startup**</span></span>  
<span data-ttu-id="80d7d-141">Hello szerepkör indításakor futtatott feladatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="80d7d-141">Contains tasks that are run when hello role starts.</span></span> <span data-ttu-id="80d7d-142">hello feladatok .cmd vagy végrehajtható fájlban vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="80d7d-142">hello tasks are defined in a .cmd or executable file.</span></span>

<a name="cscfg"></a>

## <a name="serviceconfigurationcscfg"></a><span data-ttu-id="80d7d-143">ServiceConfiguration.cscfg</span><span class="sxs-lookup"><span data-stu-id="80d7d-143">ServiceConfiguration.cscfg</span></span>
<span data-ttu-id="80d7d-144">hello konfiguráció hello a felhőalapú szolgáltatás határozza meg a hello hello értékek **ServiceConfiguration.cscfg** fájlt.</span><span class="sxs-lookup"><span data-stu-id="80d7d-144">hello configuration of hello settings for your cloud service is determined by hello values in hello **ServiceConfiguration.cscfg** file.</span></span> <span data-ttu-id="80d7d-145">Megadja a hello toodeploy szeretné az egyes szerepkörökhöz, a fájlban található példányok száma.</span><span class="sxs-lookup"><span data-stu-id="80d7d-145">You specify hello number of instances that you want toodeploy for each role in this file.</span></span> <span data-ttu-id="80d7d-146">hello értékek hello szolgáltatásdefiníciós fájlban meghatározott hello konfigurációs beállítások hozzáadása toohello szolgáltatás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="80d7d-146">hello values for hello configuration settings that you defined in hello service definition file are added toohello service configuration file.</span></span> <span data-ttu-id="80d7d-147">bármely hello felhőszolgáltatás társított felügyeleti tanúsítványok ujjlenyomatait hello is bekerül toohello fájlt.</span><span class="sxs-lookup"><span data-stu-id="80d7d-147">hello thumbprints for any management certificates that are associated with hello cloud service are also added toohello file.</span></span> <span data-ttu-id="80d7d-148">Hello [Azure szolgáltatás konfigurációs séma (.cscfg fájl)](https://msdn.microsoft.com/library/azure/ee758710.aspx) hello engedélyezett formátum biztosít a szolgáltatás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="80d7d-148">hello [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx) provides hello allowable format for a service configuration file.</span></span>

<span data-ttu-id="80d7d-149">hello szolgáltatás konfigurációs fájlja nem hello alkalmazás együtt van csomagolva, de külön fájlként feltöltött tooAzure pedig használt tooconfigure hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="80d7d-149">hello service configuration file is not packaged with hello application, but is uploaded tooAzure as a separate file and is used tooconfigure hello cloud service.</span></span> <span data-ttu-id="80d7d-150">A felhőalapú szolgáltatás ismételt üzembe helyezésével feltöltheti egy új szolgáltatás konfigurációs fájljában.</span><span class="sxs-lookup"><span data-stu-id="80d7d-150">You can upload a new service configuration file without redeploying your cloud service.</span></span> <span data-ttu-id="80d7d-151">hello konfigurációs értékek hello felhőszolgáltatás módosítása hello felhőalapú szolgáltatás futása közben.</span><span class="sxs-lookup"><span data-stu-id="80d7d-151">hello configuration values for hello cloud service can be changed while hello cloud service is running.</span></span> <span data-ttu-id="80d7d-152">hello alábbi példa bemutatja hello konfigurációs beállítások hello webes és feldolgozói szerepkörök adható meg:</span><span class="sxs-lookup"><span data-stu-id="80d7d-152">hello following example shows hello configuration settings that can be defined for hello Web and Worker roles:</span></span>

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

<span data-ttu-id="80d7d-153">Olvassa el a toohello [szolgáltatás konfigurációs séma](https://msdn.microsoft.com/library/azure/ee758710.aspx) jobb megértése hello az XML-séma itt használni, itt azonban egy gyors hello elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="80d7d-153">You can refer toohello [Service Configuration Schema](https://msdn.microsoft.com/library/azure/ee758710.aspx) for better understanding hello XML schema used here, however, here is a quick explanation of hello elements:</span></span>

<span data-ttu-id="80d7d-154">**Példányok**</span><span class="sxs-lookup"><span data-stu-id="80d7d-154">**Instances**</span></span>  
<span data-ttu-id="80d7d-155">Konfigurálja az hello futtató hello szerepkör példányainak számát.</span><span class="sxs-lookup"><span data-stu-id="80d7d-155">Configures hello number of running instances for hello role.</span></span> <span data-ttu-id="80d7d-156">tooprevent a felhőalapú szolgáltatást a frissítéskor esetlegesen elérhetetlenné válik, javasoljuk, hogy telepít-e a felé néző webes szerepkörök egynél több példánya.</span><span class="sxs-lookup"><span data-stu-id="80d7d-156">tooprevent your cloud service from potentially becoming unavailable during upgrades, it is recommended that you deploy more than one instance of your web-facing roles.</span></span> <span data-ttu-id="80d7d-157">Egynél több példány telepítésével hello toohello irányelveinek vannak betartásáért [Azure számítási szolgáltatás szolgáltatói szerződésben (SLA)](http://azure.microsoft.com/support/legal/sla/), amely biztosítja, hogy az 99,95 % külső kapcsolatot internetre szerepkörökhöz, ha két vagy több szerepkör példányok a szolgáltatás telepítésére.</span><span class="sxs-lookup"><span data-stu-id="80d7d-157">By deploying more than one instance, you are adhering toohello guidelines in hello [Azure Compute Service Level Agreement (SLA)](http://azure.microsoft.com/support/legal/sla/), which guarantees 99.95% external connectivity for Internet-facing roles when two or more role instances are deployed for a service.</span></span>

<span data-ttu-id="80d7d-158">**ConfigurationSettings**</span><span class="sxs-lookup"><span data-stu-id="80d7d-158">**ConfigurationSettings**</span></span>  
<span data-ttu-id="80d7d-159">A szerepkör példánya fut hello hello-beállítások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="80d7d-159">Configures hello settings for hello running instances for a role.</span></span> <span data-ttu-id="80d7d-160">hello hello neve `<Setting>` elemet meg kell egyeznie a hello beállításdefiníciókra hello szolgáltatásdefiníciós fájlban.</span><span class="sxs-lookup"><span data-stu-id="80d7d-160">hello name of hello `<Setting>` elements must match hello setting definitions in hello service definition file.</span></span>

<span data-ttu-id="80d7d-161">**Tanúsítványok**</span><span class="sxs-lookup"><span data-stu-id="80d7d-161">**Certificates**</span></span>  
<span data-ttu-id="80d7d-162">Konfigurálja az hello hello szolgáltatás által használt tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="80d7d-162">Configures hello certificates that are used by hello service.</span></span> <span data-ttu-id="80d7d-163">hello előző példakód bemutatja, hogyan toodefine hello hello RemoteAccess modul tanúsítványa.</span><span class="sxs-lookup"><span data-stu-id="80d7d-163">hello previous code example shows how toodefine hello certificate for hello RemoteAccess module.</span></span> <span data-ttu-id="80d7d-164">hello értékének hello *ujjlenyomat* attribútumot úgy kell beállítani a hello tanúsítvány toouse toohello ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="80d7d-164">hello value of hello *thumbprint* attribute must be set toohello thumbprint of hello certificate toouse.</span></span>

<p/>

> [!NOTE]
> <span data-ttu-id="80d7d-165">hello tanúsítványának ujjlenyomata hello felveheti toohello konfigurációs fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="80d7d-165">hello thumbprint for hello certificate can be added toohello configuration file by using a text editor.</span></span> <span data-ttu-id="80d7d-166">Vagy hello érték hello vehetők fel **tanúsítványok** hello lapján **tulajdonságok** hello szerepkör a Visual Studio oldalán.</span><span class="sxs-lookup"><span data-stu-id="80d7d-166">Or, hello value can be added on hello **Certificates** tab of hello **Properties** page of hello role in Visual Studio.</span></span>
> 
> 

## <a name="defining-ports-for-role-instances"></a><span data-ttu-id="80d7d-167">A szerepkörpéldányokért portok meghatározása</span><span class="sxs-lookup"><span data-stu-id="80d7d-167">Defining ports for role instances</span></span>
<span data-ttu-id="80d7d-168">Azure lehetővé teszi, hogy csak egy belépési pont tooa webes szerepkör.</span><span class="sxs-lookup"><span data-stu-id="80d7d-168">Azure allows only one entry point tooa web role.</span></span> <span data-ttu-id="80d7d-169">Ami azt jelenti, hogy az összes forgalom egy IP-címen keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="80d7d-169">Meaning that all traffic occurs through one IP address.</span></span> <span data-ttu-id="80d7d-170">A webhelyek tooshare port konfigurálásával hello host fejléc toodirect hello kérelem toohello megfelelő helyen konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="80d7d-170">You can configure your websites tooshare a port by configuring hello host header toodirect hello request toohello correct location.</span></span> <span data-ttu-id="80d7d-171">Az alkalmazások toolisten toowell ismert portok hello IP-cím is elvégezheti.</span><span class="sxs-lookup"><span data-stu-id="80d7d-171">You can also configure your applications toolisten toowell-known ports on hello IP address.</span></span>

<span data-ttu-id="80d7d-172">hello következő minta hello konfiguráció látható egy webes szerepkör webhely és a webes alkalmazásokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="80d7d-172">hello following sample shows hello configuration for a web role with a website and web application.</span></span> <span data-ttu-id="80d7d-173">hello webhely hello alapértelmezett belépési helyére, a 80-as port van beállítva, és egy másodlagos állomásfejléc "mail.mysite.cloudapp.net" is nevezett konfigurált tooreceive kérelmeinek hello webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="80d7d-173">hello website is configured as hello default entry location on port 80, and hello web applications are configured tooreceive requests from an alternate host header that is called “mail.mysite.cloudapp.net”.</span></span>

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" port="80" />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" hostheader="mail.mysite.cloudapp.net" />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-hello-configuration-of-a-role"></a><span data-ttu-id="80d7d-174">Egy szerepkör hello konfiguráció módosítása</span><span class="sxs-lookup"><span data-stu-id="80d7d-174">Changing hello configuration of a role</span></span>
<span data-ttu-id="80d7d-175">Frissítheti a hello konfigurációját a felhőalapú szolgáltatás, az Azure nélkül offline állapotba helyezése hello szolgáltatás futása közben.</span><span class="sxs-lookup"><span data-stu-id="80d7d-175">You can update hello configuration of your cloud service while it is running in Azure, without taking hello service offline.</span></span> <span data-ttu-id="80d7d-176">toochange konfigurációs adatokat, vagy feltöltheti egy új konfigurációs fájlt, vagy Szerkesztés hello konfigurációs fájl helyezze el, majd telepítse a szolgáltatást futtató tooyour.</span><span class="sxs-lookup"><span data-stu-id="80d7d-176">toochange configuration information, you can either upload a new configuration file, or edit hello configuration file in place and apply it tooyour running service.</span></span> <span data-ttu-id="80d7d-177">hello következő módosítható a szolgáltatás toohello konfigurációját:</span><span class="sxs-lookup"><span data-stu-id="80d7d-177">hello following changes can be made toohello configuration of a service:</span></span>

* <span data-ttu-id="80d7d-178">**A konfigurációs beállítások hello értékek módosítása**</span><span class="sxs-lookup"><span data-stu-id="80d7d-178">**Changing hello values of configuration settings**</span></span>  
  <span data-ttu-id="80d7d-179">Amikor egy konfigurációs beállítás módosításokat, a szerepkör példánya választhat tooapply hello módosítása közben hello példány online állapotban, vagy toorecycle hello példány szabályosan és hello módosítás alkalmazásához, miközben hello példány offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="80d7d-179">When a configuration setting changes, a role instance can choose tooapply hello change while hello instance is online, or toorecycle hello instance gracefully and apply hello change while hello instance is offline.</span></span>
* <span data-ttu-id="80d7d-180">**Hello szolgáltatás topológiát szerepkörpéldányt beállítani módosítása**</span><span class="sxs-lookup"><span data-stu-id="80d7d-180">**Changing hello service topology of role instances**</span></span>  
  <span data-ttu-id="80d7d-181">Topológia változtatások nincsenek hatással a futó példányát, kivéve ahol távolít el egy példányát.</span><span class="sxs-lookup"><span data-stu-id="80d7d-181">Topology changes do not affect running instances, except where an instance is being removed.</span></span> <span data-ttu-id="80d7d-182">Összes többi példányt általában nem szükséges toobe felhasználását; azonban választhatja ki toorecycle szerepkörpéldányokat válasz tooa topológiamódosítás.</span><span class="sxs-lookup"><span data-stu-id="80d7d-182">All remaining instances generally do not need toobe recycled; however, you can choose toorecycle role instances in response tooa topology change.</span></span>
* <span data-ttu-id="80d7d-183">**Tanúsítvány-ujjlenyomat hello módosítása**</span><span class="sxs-lookup"><span data-stu-id="80d7d-183">**Changing hello certificate thumbprint**</span></span>  
  <span data-ttu-id="80d7d-184">A tanúsítvány csak akkor frissíthető, ha a szerepkörpéldány offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="80d7d-184">You can only update a certificate when a role instance is offline.</span></span> <span data-ttu-id="80d7d-185">Ha egy tanúsítvány hozzáadott, törölték, vagy módosítható, amíg a szerepkör példánya online állapotban, a Azure szabályosan hello példány offline tooupdate hello tanúsítvány szükséges, és ismét online állapotba hello módosítás befejezése után.</span><span class="sxs-lookup"><span data-stu-id="80d7d-185">If a certificate is added, deleted, or changed while a role instance is online, Azure gracefully takes hello instance offline tooupdate hello certificate and bring it back online after hello change is complete.</span></span>

### <a name="handling-configuration-changes-with-service-runtime-events"></a><span data-ttu-id="80d7d-186">Konfigurációs módosítások szolgáltatás futásidejű események kezelése</span><span class="sxs-lookup"><span data-stu-id="80d7d-186">Handling configuration changes with Service Runtime Events</span></span>
<span data-ttu-id="80d7d-187">Hello [Azure futásidejű kódtár](https://msdn.microsoft.com/library/azure/mt419365.aspx) hello tartalmaz [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) névtér, amely osztályok biztosít az Azure környezetben szerepkörről hello interakció.</span><span class="sxs-lookup"><span data-stu-id="80d7d-187">hello [Azure Runtime Library](https://msdn.microsoft.com/library/azure/mt419365.aspx) includes hello [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) namespace, which provides classes for interacting with hello Azure environment from a role.</span></span> <span data-ttu-id="80d7d-188">Hello [roleenvironment-et](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) osztály határozza meg a következő előtt és után konfigurációmódosítás előállított események hello:</span><span class="sxs-lookup"><span data-stu-id="80d7d-188">hello [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) class defines hello following events that are raised before and after a configuration change:</span></span>

* <span data-ttu-id="80d7d-189">**[Módosítása](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) esemény**</span><span class="sxs-lookup"><span data-stu-id="80d7d-189">**[Changing](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) event**</span></span>  
  <span data-ttu-id="80d7d-190">Ez akkor fordul elő, mielőtt hello konfigurációváltozás alkalmazott tooa megadott felkínálva hello szerepkörpéldányokat le egy alkalommal tootake szükség szerepkör példánya.</span><span class="sxs-lookup"><span data-stu-id="80d7d-190">This occurs before hello configuration change is applied tooa specified instance of a role giving you a chance tootake down hello role instances if necessary.</span></span>
* <span data-ttu-id="80d7d-191">**[Módosított](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) esemény**</span><span class="sxs-lookup"><span data-stu-id="80d7d-191">**[Changed](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) event**</span></span>  
  <span data-ttu-id="80d7d-192">Egy szerepkör megadott példánya alkalmazott tooa hello konfigurációs módosítás után következik be.</span><span class="sxs-lookup"><span data-stu-id="80d7d-192">Occurs after hello configuration change is applied tooa specified instance of a role.</span></span>

> [!NOTE]
> <span data-ttu-id="80d7d-193">Tanúsítvány módosítások mindig egy hello példánya offline állapotba, mert azok nem indíthat hello RoleEnvironment.Changing vagy RoleEnvironment.Changed eseményeket.</span><span class="sxs-lookup"><span data-stu-id="80d7d-193">Because certificate changes always take hello instances of a role offline, they do not raise hello RoleEnvironment.Changing or RoleEnvironment.Changed events.</span></span>
> 
> 

<a name="cspkg"></a>

## <a name="servicepackagecspkg"></a><span data-ttu-id="80d7d-194">ServicePackage.cspkg</span><span class="sxs-lookup"><span data-stu-id="80d7d-194">ServicePackage.cspkg</span></span>
<span data-ttu-id="80d7d-195">egy alkalmazás az Azure-ban felhőszolgáltatásként toodeploy, kell első csomag hello alkalmazás hello megfelelő formátumban.</span><span class="sxs-lookup"><span data-stu-id="80d7d-195">toodeploy an application as a cloud service in Azure, you must first package hello application in hello appropriate format.</span></span> <span data-ttu-id="80d7d-196">Használhatja a hello **CSPack** parancssori eszköz (hello telepített [Azure SDK](https://azure.microsoft.com/downloads/)) toocreate hello csomag fájlt egy másik tooVisual Studio.</span><span class="sxs-lookup"><span data-stu-id="80d7d-196">You can use hello **CSPack** command-line tool (installed with hello [Azure SDK](https://azure.microsoft.com/downloads/)) toocreate hello package file as an alternative tooVisual Studio.</span></span>

<span data-ttu-id="80d7d-197">**CSPack** használ hello hello szolgáltatás definíciós fájl és szolgáltatás konfigurációs fájl toodefine hello hello csomag tartalmának tartalmát.</span><span class="sxs-lookup"><span data-stu-id="80d7d-197">**CSPack** uses hello contents of hello service definition file and service configuration file toodefine hello contents of hello package.</span></span> <span data-ttu-id="80d7d-198">**CSPack** hoz létre egy alkalmazás-csomagfájlját (.cspkg), hogy feltöltheti tooAzure hello segítségével [Azure-portálon](cloud-services-how-to-create-deploy-portal.md#create-and-deploy).</span><span class="sxs-lookup"><span data-stu-id="80d7d-198">**CSPack** generates an application package file (.cspkg) that you can upload tooAzure by using hello [Azure portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy).</span></span> <span data-ttu-id="80d7d-199">Alapértelmezés szerint hello csomag nevű `[ServiceDefinitionFileName].cspkg`, de megadhat egy másik nevet a hello segítségével `/out` lehetőség a **CSPack**.</span><span class="sxs-lookup"><span data-stu-id="80d7d-199">By default, hello package is named `[ServiceDefinitionFileName].cspkg`, but you can specify a different name by using hello `/out` option of **CSPack**.</span></span>

<span data-ttu-id="80d7d-200">**CSPack** itt található:</span><span class="sxs-lookup"><span data-stu-id="80d7d-200">**CSPack** is located at</span></span>  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

> [!NOTE]
> <span data-ttu-id="80d7d-201">(Windowson) CSPack.exe érhető hello futtatásával **Microsoft Azure parancssori** hello SDK telepített helyi.</span><span class="sxs-lookup"><span data-stu-id="80d7d-201">CSPack.exe (on windows) is available by running hello **Microsoft Azure Command Prompt** shortcut that is installed with hello SDK.</span></span>  
> 
> <span data-ttu-id="80d7d-202">Programfuttatást hello CSPack.exe önmagában toosee dokumentációjában az összes lehetséges hello kapcsolók és parancsok.</span><span class="sxs-lookup"><span data-stu-id="80d7d-202">Run hello CSPack.exe program by itself toosee documentation about all hello possible switches and commands.</span></span>
> 
> 

<p />

> [!TIP]
> <span data-ttu-id="80d7d-203">Futtassa helyileg a felhőalapú szolgáltatás hello **Microsoft Azure Compute Emulator**, használja a hello **/copyonly** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="80d7d-203">Run your cloud service locally in hello **Microsoft Azure Compute Emulator**, use hello **/copyonly** option.</span></span> <span data-ttu-id="80d7d-204">Ez a beállítás hello alkalmazás tooa directory elrendezés, amelyből kell futtathatók legyenek hello compute emulator hello bináris fájlokat másolja.</span><span class="sxs-lookup"><span data-stu-id="80d7d-204">This option copies hello binary files for hello application tooa directory layout from which they can be run in hello compute emulator.</span></span>
> 
> 

### <a name="example-command-toopackage-a-cloud-service"></a><span data-ttu-id="80d7d-205">Példa parancs toopackage egy felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="80d7d-205">Example command toopackage a cloud service</span></span>
<span data-ttu-id="80d7d-206">hello alábbi példa létrehoz egy alkalmazáscsomagot, amely a webes szerepkör hello információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="80d7d-206">hello following example creates an application package that contains hello information for a web role.</span></span> <span data-ttu-id="80d7d-207">hello parancs hello szolgáltatás definíciós fájl toouse határozza meg, ahol bináris fájlok lehet hello directory található, hello hello csomag fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="80d7d-207">hello command specifies hello service definition file toouse, hello directory where binary files can be found, and hello name of hello package file.</span></span>

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /out:[OutputFileName]
```

<span data-ttu-id="80d7d-208">Ha hello alkalmazás egy webes és feldolgozói szerepkörök is tartalmaz, hello következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="80d7d-208">If hello application contains both a web role and a worker role, hello following command is used:</span></span>

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /out:[OutputFileName]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]
```

<span data-ttu-id="80d7d-209">Ha hello változóit az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="80d7d-209">Where hello variables are defined as follows:</span></span>

| <span data-ttu-id="80d7d-210">Változó</span><span class="sxs-lookup"><span data-stu-id="80d7d-210">Variable</span></span> | <span data-ttu-id="80d7d-211">Érték</span><span class="sxs-lookup"><span data-stu-id="80d7d-211">Value</span></span> |
| --- | --- |
| <span data-ttu-id="80d7d-212">\[Könyvtárnév\]</span><span class="sxs-lookup"><span data-stu-id="80d7d-212">\[DirectoryName\]</span></span> |<span data-ttu-id="80d7d-213">hello alkönyvtár gyökérkönyvtárban hello projekt, amely tartalmazza az Azure-projekt hello hello .csdef fájl.</span><span class="sxs-lookup"><span data-stu-id="80d7d-213">hello subdirectory under hello root project directory that contains hello .csdef file of hello Azure project.</span></span> |
| <span data-ttu-id="80d7d-214">\[ServiceDefinition\]</span><span class="sxs-lookup"><span data-stu-id="80d7d-214">\[ServiceDefinition\]</span></span> |<span data-ttu-id="80d7d-215">hello hello szolgáltatásdefiníciós fájl neve.</span><span class="sxs-lookup"><span data-stu-id="80d7d-215">hello name of hello service definition file.</span></span> <span data-ttu-id="80d7d-216">Alapértelmezés szerint a fájl neve ServiceDefinition.csdef.</span><span class="sxs-lookup"><span data-stu-id="80d7d-216">By default, this file is named ServiceDefinition.csdef.</span></span> |
| <span data-ttu-id="80d7d-217">\[Kimenetifajlneve\]</span><span class="sxs-lookup"><span data-stu-id="80d7d-217">\[OutputFileName\]</span></span> |<span data-ttu-id="80d7d-218">hello hello nevét csomagfájl jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="80d7d-218">hello name for hello generated package file.</span></span> <span data-ttu-id="80d7d-219">Általában értéke hello alkalmazás toohello nevét.</span><span class="sxs-lookup"><span data-stu-id="80d7d-219">Typically, this is set toohello name of hello application.</span></span> <span data-ttu-id="80d7d-220">Ha nincs fájl neve meg van adva, a hello alkalmazáscsomag elemként jön létre \[ApplicationName\].cspkg.</span><span class="sxs-lookup"><span data-stu-id="80d7d-220">If no file name is specified, hello application package is created as \[ApplicationName\].cspkg.</span></span> |
| <span data-ttu-id="80d7d-221">\[RoleName\]</span><span class="sxs-lookup"><span data-stu-id="80d7d-221">\[RoleName\]</span></span> |<span data-ttu-id="80d7d-222">hello szerepkör hello szolgáltatásdefiníciós fájlban meghatározott hello neve.</span><span class="sxs-lookup"><span data-stu-id="80d7d-222">hello name of hello role as defined in hello service definition file.</span></span> |
| <span data-ttu-id="80d7d-223">\[RoleBinariesDirectory]</span><span class="sxs-lookup"><span data-stu-id="80d7d-223">\[RoleBinariesDirectory]</span></span> |<span data-ttu-id="80d7d-224">hello hello hello szerepkör bináris fájljainak helyét.</span><span class="sxs-lookup"><span data-stu-id="80d7d-224">hello location of hello binary files for hello role.</span></span> |
| <span data-ttu-id="80d7d-225">\[VirtualPath\]</span><span class="sxs-lookup"><span data-stu-id="80d7d-225">\[VirtualPath\]</span></span> |<span data-ttu-id="80d7d-226">hello hello szolgáltatás definíció webhelyhez tartozó szakaszban meghatározott hello fizikai könyvtárak minden egyes virtuális elérési úthoz.</span><span class="sxs-lookup"><span data-stu-id="80d7d-226">hello physical directories for each virtual path defined in hello Sites section of hello service definition.</span></span> |
| <span data-ttu-id="80d7d-227">\[PhysicalPath\]</span><span class="sxs-lookup"><span data-stu-id="80d7d-227">\[PhysicalPath\]</span></span> |<span data-ttu-id="80d7d-228">hello hello hely csomópontot hello szolgáltatás definíciójában meghatározott minden egyes virtuális elérési hello tartalmának fizikai könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="80d7d-228">hello physical directories of hello contents for each virtual path defined in hello site node of hello service definition.</span></span> |
| <span data-ttu-id="80d7d-229">\[RoleAssemblyName\]</span><span class="sxs-lookup"><span data-stu-id="80d7d-229">\[RoleAssemblyName\]</span></span> |<span data-ttu-id="80d7d-230">hello bináris fájl hello szerepkör hello neve.</span><span class="sxs-lookup"><span data-stu-id="80d7d-230">hello name of hello binary file for hello role.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="80d7d-231">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80d7d-231">Next steps</span></span>
<span data-ttu-id="80d7d-232">I vagyok létrehozni egy cloud service-csomag, és szeretnék...</span><span class="sxs-lookup"><span data-stu-id="80d7d-232">I'm creating a cloud service package and I want to...</span></span>

* <span data-ttu-id="80d7d-233">[Távoli asztal beállítása a felhő példánya.][remotedesktop]</span><span class="sxs-lookup"><span data-stu-id="80d7d-233">[Setup remote desktop for a cloud service instance][remotedesktop]</span></span>
* <span data-ttu-id="80d7d-234">[Egy Felhőszolgáltatás-projekt telepítése][deploy]</span><span class="sxs-lookup"><span data-stu-id="80d7d-234">[Deploy a Cloud Service project][deploy]</span></span>

<span data-ttu-id="80d7d-235">Visual Studio használok, és szeretnék...</span><span class="sxs-lookup"><span data-stu-id="80d7d-235">I am using Visual Studio and I want to...</span></span>

* <span data-ttu-id="80d7d-236">[Új felhőalapú szolgáltatás létrehozása][vs_create]</span><span class="sxs-lookup"><span data-stu-id="80d7d-236">[Create a new cloud service][vs_create]</span></span>
* <span data-ttu-id="80d7d-237">[Egy meglévő felhőszolgáltatáshoz újrakonfigurálása][vs_reconfigure]</span><span class="sxs-lookup"><span data-stu-id="80d7d-237">[Reconfigure an existing cloud service][vs_reconfigure]</span></span>
* <span data-ttu-id="80d7d-238">[Egy Felhőszolgáltatás-projekt telepítése][vs_deploy]</span><span class="sxs-lookup"><span data-stu-id="80d7d-238">[Deploy a Cloud Service project][vs_deploy]</span></span>
* <span data-ttu-id="80d7d-239">[Távoli asztal beállítása a felhő példánya.][vs_remote]</span><span class="sxs-lookup"><span data-stu-id="80d7d-239">[Setup remote desktop for a cloud service instance][vs_remote]</span></span>

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
