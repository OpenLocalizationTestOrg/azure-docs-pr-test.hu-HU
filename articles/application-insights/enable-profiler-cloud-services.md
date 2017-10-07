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
# <a name="enable-application-insights-profiler-on-an-azure-cloud-services-resource"></a>Egy Azure Felhőszolgáltatások erőforráson Application Insights Profilkészítő engedélyezése

Ez a forgatókönyv leírja, hogyan az ASP.NET-alkalmazások Azure Application Insights Profilkészítő tooenable üzemelteti-e az Azure Felhőszolgáltatások erőforrás. hello támogatja az Azure virtuális gépek, a virtuálisgép-méretezési csoportok és az Azure Service Fabric például. minden hello példák hello Azure Resource Manager üzembe helyezési modellben támogató sablonoknál támaszkodnak. Hello üzembe helyezési modellel kapcsolatos további információkért tekintse át a [Azure Resource Manager és klasszikus üzembe helyezési: üzembe helyezési modellel megértéséhez, valamint az erőforrások állapotát hello](/azure-resource-manager/resource-manager-deployment-model).

## <a name="overview"></a>Áttekintés

hello a következő ábra azt mutatja be, hello Profilkészítő működéséről az Azure Felhőszolgáltatások erőforrások. Egy Azure virtuális gép használ példaként.

![Áttekintés](./media/enable-profiler-compute/overview.png) toocollect adatainak feldolgozása és a megjelenített hello Azure-portálon, telepítenie kell a hello diagnosztikai ügynök összetevő hello Azure Felhőszolgáltatások erőforrások. hello hello forgatókönyv részeinek nyújt útmutatást hogyan tooinstall és hello diagnosztikai ügynök tooenable Application Insights Profilkészítő konfigurálása.

## <a name="prerequisites-for-hello-walkthrough"></a>Hello forgatókönyv előfeltételei

* A központi telepítés Resource Manager-sablon, amely a virtuális gépek hello hello Profilkészítő ügynökök telepíti ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) vagy méretezés beállítása ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).

* Az Application Insights példány profilkészítési engedélyezve. Útmutatásért lásd: [hello-profil engedélyezése](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).

* .NET-keretrendszer 4.6.1 vagy újabb telepíthető hello Azure Felhőszolgáltatások célerőforrás találja.

## <a name="create-a-resource-group-in-your-azure-subscription"></a>Hozzon létre egy erőforráscsoportot az Azure-előfizetéshez
hello a következő példa bemutatja, hogyan toocreate erőforrás szerint kell csoportosítani a PowerShell parancsfájl használatával:

```
New-AzureRmResourceGroup -Name "Replace_With_Resource_Group_Name" -Location "Replace_With_Resource_Group_Location"
```

## <a name="create-an-application-insights-resource-in-hello-resource-group"></a>Az Application Insights-erőforrás hello erőforráscsoport létrehozása
A hello **Application Insights** panelen adja meg az erőforrás hello adatait, ebben a példában látható módon: 

![Application Insights panel](./media/enable-profiler-compute/createai.png)

## <a name="apply-an-application-insights-instrumentation-key-in-hello-azure-resource-manager-template"></a>Az Application Insights instrumentation kulcs hello Azure Resource Manager sablon alkalmazása

1. Ha hello sablon még nem töltötte le, töltse le [GitHub](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).

2. Hello Application Insights kulcs található.
   
   ![Hello kulcs helyét](./media/enable-profiler-compute/copyaikey.png)

3. Cserélje le a hello sablon értékét.
   
   ![A sablon hello váltják le érték](./media/enable-profiler-compute/copyaikeytotemplate.png)

## <a name="create-an-azure-vm-toohost-hello-web-application"></a>Egy Azure virtuális gép toohost hello webalkalmazás létrehozása
1. Hozzon létre egy biztonságos karakterláncot toosave hello jelszót.

   ```
   $password = ConvertTo-SecureString -String "Replace_With_Your_Password" -AsPlainText -Force
   ```

2. Hello Azure Resource Manager-sablon üzembe helyezése.

   Hello directory hello PowerShell konzol toohello mappában, amely tartalmazza a Resource Manager-sablon módosítása toodeploy hello sablon, futtassa a következő parancs hello:

   ```
   New-AzureRmResourceGroupDeployment -ResourceGroupName "Replace_With_Resource_Group_Name" -TemplateFile .\WindowsVirtualMachine.json -adminUsername "Replace_With_your_user_name" -adminPassword $password -dnsNameForPublicIP "Replace_WIth_your_DNS_Name" -Verbose
   ```

Hello parancsfájl sikeres futtatása után látnia kell a virtuális gép nevű **MyWindowsVM** az erőforráscsoportban.

## <a name="configure-web-deploy-on-hello-vm"></a>Konfigurálja a Web Deploy a hello méretű VM
Győződjön meg arról, hogy a Web Deploy engedélyezve van a virtuális Gépet, a Visual Studio eszközből a webes alkalmazást is közzétehet.

a Web Deploy a virtuális gép manuálisan keresztül WebPI, tooinstall lásd [telepítése és a Web Deploy konfigurálása az IIS 8.0-s és újabb](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later). A példa bemutatja, hogyan tooautomate Web Deploy telepítése egy Azure Resource Manager-sablon használatával lásd: [létrehozása, konfigurálása, és a webes alkalmazás tooan Azure VM telepítése](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).

Ha egy ASP.NET MVC alkalmazást telepít, nyissa meg tooServer-kezelőben válassza **szerepkörök és szolgáltatások hozzáadása** > **webkiszolgáló (IIS)** > **webkiszolgáló**  >  **Alkalmazásfejlesztés**, és az ASP.NET 4.5-ös verziójának engedélyezése a kiszolgálón.

![ASP.NET hozzáadása](./media/enable-profiler-compute/addaspnet45.png)

## <a name="install-hello-azure-application-insights-sdk-for-your-project"></a>Hello Azure Application Insights SDK projekt telepítése
1. Nyissa meg az ASP.NET webalkalmazásként való kezelése a Visual Studióban.

2. Kattintson a jobb gombbal a hello projektet, és válassza ki **Hozzáadás** > **kapcsolódó szolgáltatások**.

3. Válassza ki **az Application Insights**.

4. Hello követésével hello oldalon. Válassza ki a korábban létrehozott hello Application Insights-erőforrást.

5. Jelölje be hello **regisztrálása** gombra.


## <a name="publish-hello-project-tooan-azure-vm"></a>Hello projekt tooan Azure virtuális gép közzététele
Nincsenek számos módon toopublish egy alkalmazás tooan Azure virtuális Gépen. A Visual Studio 2017 toouse egyike.

1. Kattintson a jobb gombbal a hello projektet, és válassza ki **közzététel**.

2. Válassza ki **Microsoft Azure virtuális gépek** hello közzététele tároló és hello lépésekkel.

   ![Közzététel FromVS](./media/enable-profiler-compute/publishtoVM.png)

3. Futtasson egy terheléstesztet az alkalmazás ellen. Megtekintheti az eredményeket hello Application Insights példány portál weblapon.


## <a name="enable-hello-profiler"></a>Hello Profilkészítő engedélyezése
1. Nyissa meg az Application Insights tooyour **teljesítmény** panelhez, és válassza **konfigurálása**.
   
   ![Konfigurálás ikonja](./media/enable-profiler-compute/enableprofiler1.png)
 
2. Válassza ki **engedélyezni Profilkészítő**.
   
   ![Engedélyezze a Profilkészítő ikon](./media/enable-profiler-compute/enableprofiler2.png)

## <a name="add-a-performance-test-tooyour-application"></a>Teszt teljesítménye tooyour alkalmazás felvétele
Néhány példa adatok toobe jelenik meg az Application Insights Profilkészítő is gyűjtjük, kövesse az alábbi lépéseket:

1. Keresse meg a korábban létrehozott toohello Application Insights-erőforrást. 

2. Nyissa meg toohello **rendelkezésre állási** panelen, és adja hozzá a teljesítmény teszt által küldött a webes kérelmek tooyour alkalmazás URL-címe. 

   ![Teljesítményteszt hozzáadása](./media/enable-profiler-compute/AvailabilityTest.png)

## <a name="view-your-performance-data"></a>A teljesítményadatok megtekintése

1. Várjon 10 – 15 percet hello Profilkészítő toocollect és hello adatok elemzésére. 

2. Nyissa meg toohello **teljesítmény** az Application Insights-erőforrás és hogyan működik az alkalmazás-terhelés esetén megtekintheti a panelen.

   ![Teljesítmény megtekintése](./media/enable-profiler-compute/aiperformance.png)

3. A SELECT hello ikon **példák** tooopen hello **nyomkövetési nézet** panelen.

   ![Hello nyomkövetési megtekintési paneljén megnyitása](./media/enable-profiler-compute/traceview.png)


## <a name="work-with-an-existing-template"></a>Meglévő sablon használata

1. Keresse meg a központi telepítési sablont hello Azure Diagnostics erőforrás deklarációja.
   
   Ha nem rendelkezik nyilatkozatot, létrehozhat egy alábbihoz hasonló hello deklaráció a következő példa hello. Frissítheti az hello hello sablon [Azure erőforrás-kezelő webhely](https://resources.azure.com).

2. A változások hello közzétevője `Microsoft.Azure.Diagnostics` túl`AIP.Diagnostics.Test`.

3. A `typeHandlerVersion`, használjon `0.0`.

4. Győződjön meg arról, hogy `autoUpgradeMinorVersion` értéke túl`true`.

5. Adja hozzá az új hello `ApplicationInsightsProfiler` fogadó példánya a hello `WadCfg` egy ügyfélbeállítás-objektumot, ahogy az alábbi példa hello:

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

## <a name="enable-hello-profiler-on-virtual-machine-scale-sets"></a>A virtuálisgép-méretezési csoportok hello Profilkészítő engedélyezése
toosee hogyan tooenable hello Profilkészítő, töltse le a hello [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) sablont. Alkalmazza ugyanazt a virtuális gép sablon toohello diagnosztika bővítmény erőforrás hello virtuálisgép-méretezési csoport módosításait hello.

Ellenőrizze, hogy minden példány hello méretezési csoportban lévő van-e a hozzáférés toohello internet. hello Profilkészítő ügynök is elküldheti gyűjtött hello minták tooApplication Insights megjelenítési és elemzésére.

## <a name="enable-hello-profiler-on-service-fabric-applications"></a>A Service Fabric-alkalmazások hello Profilkészítő engedélyezése
1. Kiépítés hello Service Fabric fürt toohave hello Azure Diagnostics-bővítménnyel, amely hello Profilkészítő ügynök telepíti.

2. Hello Application Insights SDK telepítése hello projektben, és az Application Insights kulcs hello konfigurálása.

3. Adja hozzá a kódot tooinstrument telemetriai.

### <a name="provision-hello-service-fabric-cluster-toohave-hello-azure-diagnostics-extension-that-installs-hello-profiler-agent"></a>Kiépítés hello Service Fabric fürt toohave hello Azure Diagnostics-bővítménnyel, amely hello Profilkészítő ügynök telepítése
A Service Fabric-fürt lehet a biztonságos vagy nem biztonságos. Beállíthat egy átjáró fürt toobe nem biztonságos, azt a tanúsítvány nem szükséges a hozzáféréshez. Kell, hogy biztonságos fürtök üzleti logikát és adatokat. Engedélyezheti a biztonságos és nem biztonságos Service Fabric-fürtök hello profiler. Ez a forgatókönyv fürtöt használnak a nem biztonságos egy példa tooexplain, hogy milyen változtatások szükséges tooenable hello profiler. A biztonságos fürt hello oszthat azonos módon.

1. Töltse le [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json). Úgy, ahogy a virtuális gépek és a virtuálisgép-méretezési csoportok, cserélje le a `Application_Insights_Key` az Application Insights kulccsal:

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

2. PowerShell parancsfájl használatával hello sablon üzembe helyezése:

   ```
   Login-AzureRmAccount
   New-AzureRmResourceGroup -Name [Your_Resource_Group_Name] -Location [Your_Resource_Group_Location] -Verbose -Force
   New-AzureRmResourceGroupDeployment -Name [Choose_An_Arbitrary_Name] -ResourceGroupName [Your_Resource_Group_Name] -TemplateFile [Path_To_Your_Template]

   ```

### <a name="install-hello-application-insights-sdk-in-hello-project-and-configure-hello-application-insights-key"></a>Hello Application Insights SDK telepítése hello projektben, és az Application Insights kulcs hello konfigurálása
Application Insights SDK hello telepíthessenek hello [NuGet-csomag](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/). Győződjön meg arról, hogy telepít egy stabil, 2.3-as vagy újabb verziója. 

A projektben az Application Insights konfigurálásával kapcsolatos további információkért lásd: [Service Fabric használatával az Application insights szolgáltatással](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).

### <a name="add-application-code-tooinstrument-telemetry"></a>Kód tooinstrument telemetriai hozzáadása
1. Minden olyan kódot, amelyet az tooinstrument, adja hozzá egy utasítást körül. 

   A következő példa hello, hello `RunAsync` metódus néhány munkát végző és hello `telemetryClient` osztály hello telemetriai adatokat rögzíti, miután elindult. hello esemény egy egyedi nevet kell hello alkalmazás között.

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

2. Az alkalmazás toohello Service Fabric-fürt központi telepítése. Hello app toorun Várjon 10 percig. Jobb hatás hello alkalmazást futtathatja egy terheléstesztet. Nyissa meg toohello Application Insights portál **teljesítmény** panelt, és meg kell példák profilkészítési nyomkövetési jelennek meg.

<!---
Commenting out these sections for now
## Enable hello Profiler on Cloud Services applications
[TODO]
## Enable hello Profiler on classic Azure Virtual Machines
[TODO]
## Enable hello Profiler on on-premise servers
[TODO]
--->

## <a name="next-steps"></a>Következő lépések

- A Profilkészítő problémák hibaelhárításával kapcsolatos található [hibaelhárítási Profilkészítő](app-insights-profiler.md#troubleshooting).

- További információk a hello Profilkészítő [Application Insights Profilkészítő](app-insights-profiler.md).
