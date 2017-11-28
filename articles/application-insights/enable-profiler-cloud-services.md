---
title: "Cloud Services erőforráson Azure Application Insights Profilkészítő aaaEnable |} Microsoft Docs"
description: "Ismerje meg, hogyan tooset be az ASP.NET-alkalmazások hello Profilkészítő üzemelteti-e az Azure Felhőszolgáltatások erőforrás."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: b9ac3bca513bf4518f44780389a9f2945f6ccc98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-application-insights-profiler-on-an-azure-cloud-services-resource"></a><span data-ttu-id="15102-103">Egy Azure Felhőszolgáltatások erőforráson Application Insights Profilkészítő engedélyezése</span><span class="sxs-lookup"><span data-stu-id="15102-103">Enable Application Insights Profiler on an Azure Cloud Services resource</span></span>

<span data-ttu-id="15102-104">Ez a forgatókönyv leírja, hogyan az ASP.NET-alkalmazások Azure Application Insights Profilkészítő tooenable üzemelteti-e az Azure Felhőszolgáltatások erőforrás.</span><span class="sxs-lookup"><span data-stu-id="15102-104">This walkthrough demonstrates how tooenable Azure Application Insights Profiler on an ASP.NET application hosted by an Azure Cloud Services resource.</span></span> <span data-ttu-id="15102-105">hello támogatja az Azure virtuális gépek, a virtuálisgép-méretezési csoportok és az Azure Service Fabric például.</span><span class="sxs-lookup"><span data-stu-id="15102-105">hello examples include support for Azure Virtual Machines, virtual machine scale sets, and Azure Service Fabric.</span></span> <span data-ttu-id="15102-106">minden hello példák hello Azure Resource Manager üzembe helyezési modellben támogató sablonoknál támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="15102-106">hello examples all rely on templates that support hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="15102-107">Hello üzembe helyezési modellel kapcsolatos további információkért tekintse át a [Azure Resource Manager és klasszikus üzembe helyezési: üzembe helyezési modellel megértéséhez, valamint az erőforrások állapotát hello](/azure-resource-manager/resource-manager-deployment-model).</span><span class="sxs-lookup"><span data-stu-id="15102-107">For more information about hello deployment model, review [Azure Resource Manager vs. classic deployment: Understand deployment models and hello state of your resources](/azure-resource-manager/resource-manager-deployment-model).</span></span>

## <a name="overview"></a><span data-ttu-id="15102-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="15102-108">Overview</span></span>

<span data-ttu-id="15102-109">hello a következő ábra azt mutatja be, hello Profilkészítő működéséről az Azure Felhőszolgáltatások erőforrások.</span><span class="sxs-lookup"><span data-stu-id="15102-109">hello following diagram illustrates how hello profiler works for Azure Cloud Services resources.</span></span> <span data-ttu-id="15102-110">Egy Azure virtuális gép használ példaként.</span><span class="sxs-lookup"><span data-stu-id="15102-110">It uses an Azure virtual machine as an example.</span></span>

<span data-ttu-id="15102-111">![Áttekintés](./media/enable-profiler-compute/overview.png) toocollect adatainak feldolgozása és a megjelenített hello Azure-portálon, telepítenie kell a hello diagnosztikai ügynök összetevő hello Azure Felhőszolgáltatások erőforrások.</span><span class="sxs-lookup"><span data-stu-id="15102-111">![Overview](./media/enable-profiler-compute/overview.png) toocollect information for processing and display on hello Azure portal, you must install hello Diagnostics Agent component for hello Azure Cloud Services resources.</span></span> <span data-ttu-id="15102-112">hello hello forgatókönyv részeinek nyújt útmutatást hogyan tooinstall és hello diagnosztikai ügynök tooenable Application Insights Profilkészítő konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="15102-112">hello rest of hello walkthrough provides guidance on how tooinstall and configure hello Diagnostics Agent tooenable Application Insights Profiler.</span></span>

## <a name="prerequisites-for-hello-walkthrough"></a><span data-ttu-id="15102-113">Hello forgatókönyv előfeltételei</span><span class="sxs-lookup"><span data-stu-id="15102-113">Prerequisites for hello walkthrough</span></span>

* <span data-ttu-id="15102-114">A központi telepítés Resource Manager-sablon, amely a virtuális gépek hello hello Profilkészítő ügynökök telepíti ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) vagy méretezés beállítása ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).</span><span class="sxs-lookup"><span data-stu-id="15102-114">A deployment Resource Manager template that installs hello profiler agents on hello VMs ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) or scale sets ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).</span></span>

* <span data-ttu-id="15102-115">Az Application Insights példány profilkészítési engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="15102-115">An Application Insights instance enabled for profiling.</span></span> <span data-ttu-id="15102-116">Útmutatásért lásd: [hello-profil engedélyezése](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).</span><span class="sxs-lookup"><span data-stu-id="15102-116">For instructions, see [Enable hello profile](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).</span></span>

* <span data-ttu-id="15102-117">.NET-keretrendszer 4.6.1 vagy újabb telepíthető hello Azure Felhőszolgáltatások célerőforrás találja.</span><span class="sxs-lookup"><span data-stu-id="15102-117">.NET Framework 4.6.1 or later installed on hello target Azure Cloud Services resource.</span></span>

## <a name="create-a-resource-group-in-your-azure-subscription"></a><span data-ttu-id="15102-118">Hozzon létre egy erőforráscsoportot az Azure-előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="15102-118">Create a resource group in your Azure subscription</span></span>
<span data-ttu-id="15102-119">hello a következő példa bemutatja, hogyan toocreate erőforrás szerint kell csoportosítani a PowerShell parancsfájl használatával:</span><span class="sxs-lookup"><span data-stu-id="15102-119">hello following example demonstrates how toocreate a resource group by using a PowerShell script:</span></span>

```
New-AzureRmResourceGroup -Name "Replace_With_Resource_Group_Name" -Location "Replace_With_Resource_Group_Location"
```

## <a name="create-an-application-insights-resource-in-hello-resource-group"></a><span data-ttu-id="15102-120">Az Application Insights-erőforrás hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="15102-120">Create an Application Insights resource in hello resource group</span></span>
<span data-ttu-id="15102-121">A hello **Application Insights** panelen adja meg az erőforrás hello adatait, ebben a példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="15102-121">On hello **Application Insights** blade, enter hello information for your resource, as shown in this example:</span></span> 

![Application Insights panel](./media/enable-profiler-compute/createai.png)

## <a name="apply-an-application-insights-instrumentation-key-in-hello-azure-resource-manager-template"></a><span data-ttu-id="15102-123">Az Application Insights instrumentation kulcs hello Azure Resource Manager sablon alkalmazása</span><span class="sxs-lookup"><span data-stu-id="15102-123">Apply an Application Insights instrumentation key in hello Azure Resource Manager template</span></span>

1. <span data-ttu-id="15102-124">Ha hello sablon még nem töltötte le, töltse le [GitHub](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).</span><span class="sxs-lookup"><span data-stu-id="15102-124">If you haven't downloaded hello template yet, download it from [GitHub](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).</span></span>

2. <span data-ttu-id="15102-125">Hello Application Insights kulcs található.</span><span class="sxs-lookup"><span data-stu-id="15102-125">Find hello Application Insights key.</span></span>
   
   ![Hello kulcs helyét](./media/enable-profiler-compute/copyaikey.png)

3. <span data-ttu-id="15102-127">Cserélje le a hello sablon értékét.</span><span class="sxs-lookup"><span data-stu-id="15102-127">Replace hello template value.</span></span>
   
   ![A sablon hello váltják le érték](./media/enable-profiler-compute/copyaikeytotemplate.png)

## <a name="create-an-azure-vm-toohost-hello-web-application"></a><span data-ttu-id="15102-129">Egy Azure virtuális gép toohost hello webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="15102-129">Create an Azure VM toohost hello web application</span></span>
1. <span data-ttu-id="15102-130">Hozzon létre egy biztonságos karakterláncot toosave hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="15102-130">Create a secure string toosave hello password.</span></span>

   ```
   $password = ConvertTo-SecureString -String "Replace_With_Your_Password" -AsPlainText -Force
   ```

2. <span data-ttu-id="15102-131">Hello Azure Resource Manager-sablon üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="15102-131">Deploy hello Azure Resource Manager template.</span></span>

   <span data-ttu-id="15102-132">Hello directory hello PowerShell konzol toohello mappában, amely tartalmazza a Resource Manager-sablon módosítása</span><span class="sxs-lookup"><span data-stu-id="15102-132">Change hello directory in hello PowerShell console toohello folder that contains your Resource Manager template.</span></span> <span data-ttu-id="15102-133">toodeploy hello sablon, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="15102-133">toodeploy hello template, run hello following command:</span></span>

   ```
   New-AzureRmResourceGroupDeployment -ResourceGroupName "Replace_With_Resource_Group_Name" -TemplateFile .\WindowsVirtualMachine.json -adminUsername "Replace_With_your_user_name" -adminPassword $password -dnsNameForPublicIP "Replace_WIth_your_DNS_Name" -Verbose
   ```

<span data-ttu-id="15102-134">Hello parancsfájl sikeres futtatása után látnia kell a virtuális gép nevű **MyWindowsVM** az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="15102-134">After hello script runs successfully, you should find a VM named **MyWindowsVM** in your resource group.</span></span>

## <a name="configure-web-deploy-on-hello-vm"></a><span data-ttu-id="15102-135">Konfigurálja a Web Deploy a hello méretű VM</span><span class="sxs-lookup"><span data-stu-id="15102-135">Configure Web Deploy on hello VM</span></span>
<span data-ttu-id="15102-136">Győződjön meg arról, hogy a Web Deploy engedélyezve van a virtuális Gépet, a Visual Studio eszközből a webes alkalmazást is közzétehet.</span><span class="sxs-lookup"><span data-stu-id="15102-136">Make sure that Web Deploy is enabled on your VM so you can publish your web application from Visual Studio.</span></span>

<span data-ttu-id="15102-137">a Web Deploy a virtuális gép manuálisan keresztül WebPI, tooinstall lásd [telepítése és a Web Deploy konfigurálása az IIS 8.0-s és újabb](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later).</span><span class="sxs-lookup"><span data-stu-id="15102-137">tooinstall Web Deploy on a VM manually via WebPI, see [Installing and Configuring Web Deploy on IIS 8.0 or Later](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later).</span></span> <span data-ttu-id="15102-138">A példa bemutatja, hogyan tooautomate Web Deploy telepítése egy Azure Resource Manager-sablon használatával lásd: [létrehozása, konfigurálása, és a webes alkalmazás tooan Azure VM telepítése](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).</span><span class="sxs-lookup"><span data-stu-id="15102-138">For an example of how tooautomate installing Web Deploy by using an Azure Resource Manager template, see [Create, configure, and deploy a web application tooan Azure VM](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).</span></span>

<span data-ttu-id="15102-139">Ha egy ASP.NET MVC alkalmazást telepít, nyissa meg tooServer-kezelőben válassza **szerepkörök és szolgáltatások hozzáadása** > **webkiszolgáló (IIS)** > **webkiszolgáló**  >  **Alkalmazásfejlesztés**, és az ASP.NET 4.5-ös verziójának engedélyezése a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="15102-139">If you are deploying an ASP.NET MVC application, go tooServer Manager, select **Add Roles and Features** > **Web Server (IIS)** > **Web Server** > **Application Development**, and enable ASP.NET 4.5 on your server.</span></span>

![ASP.NET hozzáadása](./media/enable-profiler-compute/addaspnet45.png)

## <a name="install-hello-azure-application-insights-sdk-for-your-project"></a><span data-ttu-id="15102-141">Hello Azure Application Insights SDK projekt telepítése</span><span class="sxs-lookup"><span data-stu-id="15102-141">Install hello Azure Application Insights SDK for your project</span></span>
1. <span data-ttu-id="15102-142">Nyissa meg az ASP.NET webalkalmazásként való kezelése a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="15102-142">Open your ASP.NET web application in Visual Studio.</span></span>

2. <span data-ttu-id="15102-143">Kattintson a jobb gombbal a hello projektet, és válassza ki **Hozzáadás** > **kapcsolódó szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="15102-143">Right-click hello project and select **Add** > **Connected Services**.</span></span>

3. <span data-ttu-id="15102-144">Válassza ki **az Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="15102-144">Select **Application Insights**.</span></span>

4. <span data-ttu-id="15102-145">Hello követésével hello oldalon.</span><span class="sxs-lookup"><span data-stu-id="15102-145">Follow hello instructions on hello page.</span></span> <span data-ttu-id="15102-146">Válassza ki a korábban létrehozott hello Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="15102-146">Select hello Application Insights resource that you created earlier.</span></span>

5. <span data-ttu-id="15102-147">Jelölje be hello **regisztrálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="15102-147">Select hello **Register** button.</span></span>


## <a name="publish-hello-project-tooan-azure-vm"></a><span data-ttu-id="15102-148">Hello projekt tooan Azure virtuális gép közzététele</span><span class="sxs-lookup"><span data-stu-id="15102-148">Publish hello project tooan Azure VM</span></span>
<span data-ttu-id="15102-149">Nincsenek számos módon toopublish egy alkalmazás tooan Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="15102-149">There are several ways toopublish an application tooan Azure VM.</span></span> <span data-ttu-id="15102-150">A Visual Studio 2017 toouse egyike.</span><span class="sxs-lookup"><span data-stu-id="15102-150">One way is toouse Visual Studio 2017.</span></span>

1. <span data-ttu-id="15102-151">Kattintson a jobb gombbal a hello projektet, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="15102-151">Right-click hello project and select **Publish**.</span></span>

2. <span data-ttu-id="15102-152">Válassza ki **Microsoft Azure virtuális gépek** hello közzététele tároló és hello lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="15102-152">Select **Microsoft Azure Virtual Machines** as hello publish target and follow hello steps.</span></span>

   ![Közzététel FromVS](./media/enable-profiler-compute/publishtoVM.png)

3. <span data-ttu-id="15102-154">Futtasson egy terheléstesztet az alkalmazás ellen.</span><span class="sxs-lookup"><span data-stu-id="15102-154">Run a load test against your application.</span></span> <span data-ttu-id="15102-155">Megtekintheti az eredményeket hello Application Insights példány portál weblapon.</span><span class="sxs-lookup"><span data-stu-id="15102-155">You should see results on hello Application Insights instance portal webpage.</span></span>


## <a name="enable-hello-profiler"></a><span data-ttu-id="15102-156">Hello Profilkészítő engedélyezése</span><span class="sxs-lookup"><span data-stu-id="15102-156">Enable hello profiler</span></span>
1. <span data-ttu-id="15102-157">Nyissa meg az Application Insights tooyour **teljesítmény** panelhez, és válassza **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="15102-157">Go tooyour Application Insights **Performance** blade and select **Configure**.</span></span>
   
   ![Konfigurálás ikonja](./media/enable-profiler-compute/enableprofiler1.png)
 
2. <span data-ttu-id="15102-159">Válassza ki **engedélyezni Profilkészítő**.</span><span class="sxs-lookup"><span data-stu-id="15102-159">Select **Enable Profiler**.</span></span>
   
   ![Engedélyezze a Profilkészítő ikon](./media/enable-profiler-compute/enableprofiler2.png)

## <a name="add-a-performance-test-tooyour-application"></a><span data-ttu-id="15102-161">Teszt teljesítménye tooyour alkalmazás felvétele</span><span class="sxs-lookup"><span data-stu-id="15102-161">Add a performance test tooyour application</span></span>
<span data-ttu-id="15102-162">Néhány példa adatok toobe jelenik meg az Application Insights Profilkészítő is gyűjtjük, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="15102-162">Follow these steps so we can collect some sample data toobe displayed in Application Insights Profiler:</span></span>

1. <span data-ttu-id="15102-163">Keresse meg a korábban létrehozott toohello Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="15102-163">Browse toohello Application Insights resource that you created earlier.</span></span> 

2. <span data-ttu-id="15102-164">Nyissa meg toohello **rendelkezésre állási** panelen, és adja hozzá a teljesítmény teszt által küldött a webes kérelmek tooyour alkalmazás URL-címe.</span><span class="sxs-lookup"><span data-stu-id="15102-164">Go toohello **Availability** blade and add a performance test that sends web requests tooyour application URL.</span></span> 

   ![Teljesítményteszt hozzáadása](./media/enable-profiler-compute/AvailabilityTest.png)

## <a name="view-your-performance-data"></a><span data-ttu-id="15102-166">A teljesítményadatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="15102-166">View your performance data</span></span>

1. <span data-ttu-id="15102-167">Várjon 10 – 15 percet hello Profilkészítő toocollect és hello adatok elemzésére.</span><span class="sxs-lookup"><span data-stu-id="15102-167">Wait 10-15 minutes for hello profiler toocollect and analyze hello data.</span></span> 

2. <span data-ttu-id="15102-168">Nyissa meg toohello **teljesítmény** az Application Insights-erőforrás és hogyan működik az alkalmazás-terhelés esetén megtekintheti a panelen.</span><span class="sxs-lookup"><span data-stu-id="15102-168">Go toohello **Performance** blade in your Application Insights resource and view how your application is performing when it's under load.</span></span>

   ![Teljesítmény megtekintése](./media/enable-profiler-compute/aiperformance.png)

3. <span data-ttu-id="15102-170">A SELECT hello ikon **példák** tooopen hello **nyomkövetési nézet** panelen.</span><span class="sxs-lookup"><span data-stu-id="15102-170">Select hello icon under **Examples** tooopen hello **Trace View** blade.</span></span>

   ![Hello nyomkövetési megtekintési paneljén megnyitása](./media/enable-profiler-compute/traceview.png)


## <a name="work-with-an-existing-template"></a><span data-ttu-id="15102-172">Meglévő sablon használata</span><span class="sxs-lookup"><span data-stu-id="15102-172">Work with an existing template</span></span>

1. <span data-ttu-id="15102-173">Keresse meg a központi telepítési sablont hello Azure Diagnostics erőforrás deklarációja.</span><span class="sxs-lookup"><span data-stu-id="15102-173">Locate hello Azure Diagnostics resource declaration in your deployment template.</span></span>
   
   <span data-ttu-id="15102-174">Ha nem rendelkezik nyilatkozatot, létrehozhat egy alábbihoz hasonló hello deklaráció a következő példa hello.</span><span class="sxs-lookup"><span data-stu-id="15102-174">If you don't have a declaration, you can create one that resembles hello declaration in hello following example.</span></span> <span data-ttu-id="15102-175">Frissítheti az hello hello sablon [Azure erőforrás-kezelő webhely](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="15102-175">You can update hello template from hello [Azure Resource Explorer website](https://resources.azure.com).</span></span>

2. <span data-ttu-id="15102-176">A változások hello közzétevője `Microsoft.Azure.Diagnostics` túl`AIP.Diagnostics.Test`.</span><span class="sxs-lookup"><span data-stu-id="15102-176">Change hello publisher from `Microsoft.Azure.Diagnostics` too`AIP.Diagnostics.Test`.</span></span>

3. <span data-ttu-id="15102-177">A `typeHandlerVersion`, használjon `0.0`.</span><span class="sxs-lookup"><span data-stu-id="15102-177">For `typeHandlerVersion`, use `0.0`.</span></span>

4. <span data-ttu-id="15102-178">Győződjön meg arról, hogy `autoUpgradeMinorVersion` értéke túl`true`.</span><span class="sxs-lookup"><span data-stu-id="15102-178">Make sure that `autoUpgradeMinorVersion` is set too`true`.</span></span>

5. <span data-ttu-id="15102-179">Adja hozzá az új hello `ApplicationInsightsProfiler` fogadó példánya a hello `WadCfg` egy ügyfélbeállítás-objektumot, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="15102-179">Add hello new `ApplicationInsightsProfiler` sink instance in hello `WadCfg` settings object, as shown in hello following example:</span></span>

```
"resources": [
        {
          "type": "extensions",
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "apiVersion": "2016-03-30",
          "properties": {
            "publisher": "AIP.Diagnostics.Test",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "WadCfg": {
                "SinksConfig": {
                  "Sink": [
                    {
                      "name": "Give a descriptive short name. E.g.: MyApplicationInsightsProfilerSink",
                      "ApplicationInsightsProfiler": "Enter hello Application Insights instance instrumentation key guid here"
                    }
                  ]
                },
                "DiagnosticMonitorConfiguration": {
                    ...
                }
                ...
              }
              ...
            }
            ...
          }
          ...
]
```

## <a name="enable-hello-profiler-on-virtual-machine-scale-sets"></a><span data-ttu-id="15102-180">A virtuálisgép-méretezési csoportok hello Profilkészítő engedélyezése</span><span class="sxs-lookup"><span data-stu-id="15102-180">Enable hello profiler on virtual machine scale sets</span></span>
<span data-ttu-id="15102-181">toosee hogyan tooenable hello Profilkészítő, töltse le a hello [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) sablont.</span><span class="sxs-lookup"><span data-stu-id="15102-181">toosee how tooenable hello profiler, download hello [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) template.</span></span> <span data-ttu-id="15102-182">Alkalmazza ugyanazt a virtuális gép sablon toohello diagnosztika bővítmény erőforrás hello virtuálisgép-méretezési csoport módosításait hello.</span><span class="sxs-lookup"><span data-stu-id="15102-182">Apply hello same changes in a VM template toohello diagnostics extension resource for hello virtual machine scale set.</span></span>

<span data-ttu-id="15102-183">Ellenőrizze, hogy minden példány hello méretezési csoportban lévő van-e a hozzáférés toohello internet.</span><span class="sxs-lookup"><span data-stu-id="15102-183">Make sure that each instance in hello scale set has access toohello internet.</span></span> <span data-ttu-id="15102-184">hello Profilkészítő ügynök is elküldheti gyűjtött hello minták tooApplication Insights megjelenítési és elemzésére.</span><span class="sxs-lookup"><span data-stu-id="15102-184">hello Profiler Agent can then send hello collected samples tooApplication Insights for display and analysis.</span></span>

## <a name="enable-hello-profiler-on-service-fabric-applications"></a><span data-ttu-id="15102-185">A Service Fabric-alkalmazások hello Profilkészítő engedélyezése</span><span class="sxs-lookup"><span data-stu-id="15102-185">Enable hello profiler on Service Fabric applications</span></span>
1. <span data-ttu-id="15102-186">Kiépítés hello Service Fabric fürt toohave hello Azure Diagnostics-bővítménnyel, amely hello Profilkészítő ügynök telepíti.</span><span class="sxs-lookup"><span data-stu-id="15102-186">Provision hello Service Fabric cluster toohave hello Azure Diagnostics extension that installs hello Profiler Agent.</span></span>

2. <span data-ttu-id="15102-187">Hello Application Insights SDK telepítése hello projektben, és az Application Insights kulcs hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="15102-187">Install hello Application Insights SDK in hello project and configure hello Application Insights key.</span></span>

3. <span data-ttu-id="15102-188">Adja hozzá a kódot tooinstrument telemetriai.</span><span class="sxs-lookup"><span data-stu-id="15102-188">Add application code tooinstrument telemetry.</span></span>

### <a name="provision-hello-service-fabric-cluster-toohave-hello-azure-diagnostics-extension-that-installs-hello-profiler-agent"></a><span data-ttu-id="15102-189">Kiépítés hello Service Fabric fürt toohave hello Azure Diagnostics-bővítménnyel, amely hello Profilkészítő ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="15102-189">Provision hello Service Fabric cluster toohave hello Azure Diagnostics extension that installs hello Profiler Agent</span></span>
<span data-ttu-id="15102-190">A Service Fabric-fürt lehet a biztonságos vagy nem biztonságos.</span><span class="sxs-lookup"><span data-stu-id="15102-190">A Service Fabric cluster can be secure or non-secure.</span></span> <span data-ttu-id="15102-191">Beállíthat egy átjáró fürt toobe nem biztonságos, azt a tanúsítvány nem szükséges a hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="15102-191">You can set one gateway cluster toobe non-secure so it doesn't require a certificate for access.</span></span> <span data-ttu-id="15102-192">Kell, hogy biztonságos fürtök üzleti logikát és adatokat.</span><span class="sxs-lookup"><span data-stu-id="15102-192">Clusters that host business logic and data should be secure.</span></span> <span data-ttu-id="15102-193">Engedélyezheti a biztonságos és nem biztonságos Service Fabric-fürtök hello profiler.</span><span class="sxs-lookup"><span data-stu-id="15102-193">You can enable hello profiler on both secure and non-secure Service Fabric clusters.</span></span> <span data-ttu-id="15102-194">Ez a forgatókönyv fürtöt használnak a nem biztonságos egy példa tooexplain, hogy milyen változtatások szükséges tooenable hello profiler.</span><span class="sxs-lookup"><span data-stu-id="15102-194">This walkthrough uses a non-secure cluster as an example tooexplain what changes are required tooenable hello profiler.</span></span> <span data-ttu-id="15102-195">A biztonságos fürt hello oszthat azonos módon.</span><span class="sxs-lookup"><span data-stu-id="15102-195">You can provision a secure cluster in hello same way.</span></span>

1. <span data-ttu-id="15102-196">Töltse le [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json).</span><span class="sxs-lookup"><span data-stu-id="15102-196">Download [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json).</span></span> <span data-ttu-id="15102-197">Úgy, ahogy a virtuális gépek és a virtuálisgép-méretezési csoportok, cserélje le a `Application_Insights_Key` az Application Insights kulccsal:</span><span class="sxs-lookup"><span data-stu-id="15102-197">As you did for VMs and virtual machine scale sets, replace `Application_Insights_Key` with your Application Insights key:</span></span>

   ```
   "publisher": "AIP.Diagnostics.Test",
                 "settings": {
                   "WadCfg": {
                     "SinksConfig": {
                       "Sink": [
                         {
                           "name": "MyApplicationInsightsProfilerSinkVMSS",
                           "ApplicationInsightsProfiler": "[Application_Insights_Key]"
                         }
                       ]
                     },
   ```

2. <span data-ttu-id="15102-198">PowerShell parancsfájl használatával hello sablon üzembe helyezése:</span><span class="sxs-lookup"><span data-stu-id="15102-198">Deploy hello template by using a PowerShell script:</span></span>

   ```
   Login-AzureRmAccount
   New-AzureRmResourceGroup -Name [Your_Resource_Group_Name] -Location [Your_Resource_Group_Location] -Verbose -Force
   New-AzureRmResourceGroupDeployment -Name [Choose_An_Arbitrary_Name] -ResourceGroupName [Your_Resource_Group_Name] -TemplateFile [Path_To_Your_Template]

   ```

### <a name="install-hello-application-insights-sdk-in-hello-project-and-configure-hello-application-insights-key"></a><span data-ttu-id="15102-199">Hello Application Insights SDK telepítése hello projektben, és az Application Insights kulcs hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="15102-199">Install hello Application Insights SDK in hello project and configure hello Application Insights key</span></span>
<span data-ttu-id="15102-200">Application Insights SDK hello telepíthessenek hello [NuGet-csomag](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span><span class="sxs-lookup"><span data-stu-id="15102-200">Install hello Application Insights SDK from hello [NuGet package](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span></span> <span data-ttu-id="15102-201">Győződjön meg arról, hogy telepít egy stabil, 2.3-as vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="15102-201">Make sure that you install a stable version, 2.3 or later.</span></span> 

<span data-ttu-id="15102-202">A projektben az Application Insights konfigurálásával kapcsolatos további információkért lásd: [Service Fabric használatával az Application insights szolgáltatással](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).</span><span class="sxs-lookup"><span data-stu-id="15102-202">For information about configuring Application Insights in your projects, see [Using Service Fabric with Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).</span></span>

### <a name="add-application-code-tooinstrument-telemetry"></a><span data-ttu-id="15102-203">Kód tooinstrument telemetriai hozzáadása</span><span class="sxs-lookup"><span data-stu-id="15102-203">Add application code tooinstrument telemetry</span></span>
1. <span data-ttu-id="15102-204">Minden olyan kódot, amelyet az tooinstrument, adja hozzá egy utasítást körül.</span><span class="sxs-lookup"><span data-stu-id="15102-204">For any piece of code that you want tooinstrument, add a using statement around it.</span></span> 

   <span data-ttu-id="15102-205">A következő példa hello, hello `RunAsync` metódus néhány munkát végző és hello `telemetryClient` osztály hello telemetriai adatokat rögzíti, miután elindult.</span><span class="sxs-lookup"><span data-stu-id="15102-205">In hello following  example, hello `RunAsync` method is doing some work, and hello `telemetryClient` class captures hello telemetry after it starts.</span></span> <span data-ttu-id="15102-206">hello esemény egy egyedi nevet kell hello alkalmazás között.</span><span class="sxs-lookup"><span data-stu-id="15102-206">hello event needs a unique name across hello application.</span></span>

   ```
   protected override async Task RunAsync(CancellationToken cancellationToken)
       {
           // TODO: Replace hello following sample code with your own logic
           //       or remove this RunAsync override if it's not needed in your service.

           while (true)
           {
               using( var operation = telemetryClient.StartOperation<RequestTelemetry>("[Insert_Event_Unique_Name]"))
               {
                   cancellationToken.ThrowIfCancellationRequested();

                   ++this.iterations;

                   ServiceEventSource.Current.ServiceMessage(this.Context, "Working-{0}", this.iterations);

                   await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
               }

           }
       }
   ```

2. <span data-ttu-id="15102-207">Az alkalmazás toohello Service Fabric-fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="15102-207">Deploy your application toohello Service Fabric cluster.</span></span> <span data-ttu-id="15102-208">Hello app toorun Várjon 10 percig.</span><span class="sxs-lookup"><span data-stu-id="15102-208">Wait for hello app toorun for 10 minutes.</span></span> <span data-ttu-id="15102-209">Jobb hatás hello alkalmazást futtathatja egy terheléstesztet.</span><span class="sxs-lookup"><span data-stu-id="15102-209">For better effect, you can run a load test on hello app.</span></span> <span data-ttu-id="15102-210">Nyissa meg toohello Application Insights portál **teljesítmény** panelt, és meg kell példák profilkészítési nyomkövetési jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="15102-210">Go toohello Application Insights portal's **Performance** blade, and you should see examples of profiling traces appear.</span></span>

<!---
Commenting out these sections for now
## Enable hello Profiler on Cloud Services applications
[TODO]
## Enable hello Profiler on classic Azure Virtual Machines
[TODO]
## Enable hello Profiler on on-premise servers
[TODO]
--->

## <a name="next-steps"></a><span data-ttu-id="15102-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15102-211">Next steps</span></span>

- <span data-ttu-id="15102-212">A Profilkészítő problémák hibaelhárításával kapcsolatos található [hibaelhárítási Profilkészítő](app-insights-profiler.md#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="15102-212">Find help with troubleshooting profiler issues in [Profiler troubleshooting](app-insights-profiler.md#troubleshooting).</span></span>

- <span data-ttu-id="15102-213">További információk a hello Profilkészítő [Application Insights Profilkészítő](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="15102-213">Read more about hello profiler in [Application Insights Profiler](app-insights-profiler.md).</span></span>
