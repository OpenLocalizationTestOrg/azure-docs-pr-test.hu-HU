---
title: "aaaAzure IoT Suite – gyakori kérdések |} Microsoft Docs"
description: "Gyakran ismételt kérdések az IoT Suite-ról"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: cb39e24af6d1ce2afea554285512d05b2d7c721e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-iot-suite"></a><span data-ttu-id="88e47-103">Gyakran ismételt kérdések az IoT Suite-ról</span><span class="sxs-lookup"><span data-stu-id="88e47-103">Frequently asked questions for IoT Suite</span></span>

<span data-ttu-id="88e47-104">Lásd még hello csatlakoztatott gyári specifikus [gyakran ismételt kérdések](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="88e47-104">See also, hello connected factory specific [FAQ](iot-suite-faq-cf.md).</span></span>

### <a name="where-can-i-find-hello-source-code-for-hello-preconfigured-solutions"></a><span data-ttu-id="88e47-105">Hol találok hello forráskód előre konfigurált hello megoldások?</span><span class="sxs-lookup"><span data-stu-id="88e47-105">Where can I find hello source code for hello preconfigured solutions?</span></span>

<span data-ttu-id="88e47-106">a következő GitHub-adattárak hello hello forráskód tárolja:</span><span class="sxs-lookup"><span data-stu-id="88e47-106">hello source code is stored in hello following GitHub repositories:</span></span>
* <span data-ttu-id="88e47-107">[Távoli figyelési előkonfigurált megoldás][lnk-remote-monitoring-github]</span><span class="sxs-lookup"><span data-stu-id="88e47-107">[Remote monitoring preconfigured solution][lnk-remote-monitoring-github]</span></span>
* <span data-ttu-id="88e47-108">[Előre konfigurált prediktív karbantartási megoldás][lnk-predictive-maintenance-github]</span><span class="sxs-lookup"><span data-stu-id="88e47-108">[Predictive maintenance preconfigured solution][lnk-predictive-maintenance-github]</span></span>
* [<span data-ttu-id="88e47-109">Előre konfigurált csatlakoztatott gyári megoldás</span><span class="sxs-lookup"><span data-stu-id="88e47-109">Connected factory preconfigured solution</span></span>](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-do-i-update-toohello-latest-version-of-hello-remote-monitoring-preconfigured-solution-that-uses-hello-iot-hub-device-management-features"></a><span data-ttu-id="88e47-110">Hogyan frissíthetők a toohello legújabb verziójának hello távoli figyelési előkonfigurált megoldás, hogy a használt hello IoT Hub eszköz kezelési funkciókat?</span><span class="sxs-lookup"><span data-stu-id="88e47-110">How do I update toohello latest version of hello remote monitoring preconfigured solution that uses hello IoT Hub device management features?</span></span>

* <span data-ttu-id="88e47-111">Ha egy előre konfigurált megoldást hello https://www.azureiotsuite.com/ helyet telepít, mindig telepíti hello hello megoldás legújabb verziója egy új példányát.</span><span class="sxs-lookup"><span data-stu-id="88e47-111">If you deploy a preconfigured solution from hello https://www.azureiotsuite.com/ site, it always deploys a new instance of hello latest version of hello solution.</span></span>
* <span data-ttu-id="88e47-112">Ha telepít egy előre konfigurált megoldás hello parancssorból, frissítheti egy meglévő központi telepítési új kóddal.</span><span class="sxs-lookup"><span data-stu-id="88e47-112">If you deploy a preconfigured solution using hello command line, you can update an existing deployment with new code.</span></span> <span data-ttu-id="88e47-113">Lásd: [felhőalapú üzemelő példány] [ lnk-cloud-deployment] a Githubon hello [tárház][lnk-remote-monitoring-github].</span><span class="sxs-lookup"><span data-stu-id="88e47-113">See [Cloud deployment][lnk-cloud-deployment] in hello GitHub [repository][lnk-remote-monitoring-github].</span></span>

### <a name="how-can-i-add-support-for-a-new-device-method-toohello-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="88e47-114">Hogyan vehetek fel egy új eszköz metódus toohello távoli felügyeleti előkonfigurált megoldás támogatását?</span><span class="sxs-lookup"><span data-stu-id="88e47-114">How can I add support for a new device method toohello remote monitoring preconfigured solution?</span></span>

<span data-ttu-id="88e47-115">Hello című [egy új módszer toohello szimulátor támogatást] [ lnk-add-method] a hello [előkonfigurált megoldás testreszabása] [ lnk-customize] cikk.</span><span class="sxs-lookup"><span data-stu-id="88e47-115">See hello section [Add support for a new method toohello simulator][lnk-add-method] in hello [Customize a preconfigured solution][lnk-customize] article.</span></span>

### <a name="hello-simulated-device-is-ignoring-my-desired-property-changes-why"></a><span data-ttu-id="88e47-116">hello szimulált eszköz figyelmen kívül hagyja a kívánt tulajdonságok módosítását, miért?</span><span class="sxs-lookup"><span data-stu-id="88e47-116">hello simulated device is ignoring my desired property changes, why?</span></span>
<span data-ttu-id="88e47-117">A hello távoli megfigyelési előre konfigurált megoldás, hello szimulált eszköz kód csak akkor használja, hello **Desired.Config.TemperatureMeanValue** és **Desired.Config.TelemetryInterval** szükségeskonfiguráció-tulajdonságok tooupdate hello jelentett tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="88e47-117">In hello remote monitoring preconfigured solution, hello simulated device code only uses hello **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties tooupdate hello reported properties.</span></span> <span data-ttu-id="88e47-118">Az összes többi kívánt tulajdonság változáskérések figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="88e47-118">All other desired property change requests are ignored.</span></span>

### <a name="my-device-does-not-appear-in-hello-list-of-devices-in-hello-solution-dashboard-why"></a><span data-ttu-id="88e47-119">Az eszköz nem jelenik meg hello listán szereplő eszközök hello megoldás irányítópultjának, miért?</span><span class="sxs-lookup"><span data-stu-id="88e47-119">My device does not appear in hello list of devices in hello solution dashboard, why?</span></span>

<span data-ttu-id="88e47-120">eszközök hello megoldás irányítópultjának hello listáját használja a lekérdezés tooreturn hello eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="88e47-120">hello list of devices in hello solution dashboard uses a query tooreturn hello list of devices.</span></span> <span data-ttu-id="88e47-121">Jelenleg a lekérdezés nem adhat vissza több mint 10 KB-os eszközök.</span><span class="sxs-lookup"><span data-stu-id="88e47-121">Currently, a query cannot return more than 10K devices.</span></span> <span data-ttu-id="88e47-122">Próbáljon meg a lekérdezés hello keresési feltételeit szigorúbb.</span><span class="sxs-lookup"><span data-stu-id="88e47-122">Try making hello search criteria for your query more restrictive.</span></span>

### <a name="whats-hello-difference-between-deleting-a-resource-group-in-hello-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a><span data-ttu-id="88e47-123">Mi az, hogy egy erőforráscsoportot az Azure portál és gombra kattintva törli a azureiotsuite.com előkonfigurált megoldást hello törlése hello különbségének?</span><span class="sxs-lookup"><span data-stu-id="88e47-123">What's hello difference between deleting a resource group in hello Azure portal and clicking delete on a preconfigured solution in azureiotsuite.com?</span></span>

* <span data-ttu-id="88e47-124">Ha törli az előre konfigurált hello megoldás [azureiotsuite.com][lnk-azureiotsuite], törli az összes hello erőforrásokat kiépített előre konfigurált hello megoldás létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="88e47-124">If you delete hello preconfigured solution in [azureiotsuite.com][lnk-azureiotsuite], you delete all hello resources that were provisioned when you created hello preconfigured solution.</span></span> <span data-ttu-id="88e47-125">Ha további erőforrások toohello erőforráscsoport, ezeket az erőforrásokat is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="88e47-125">If you added additional resources toohello resource group, these resources are also deleted.</span></span> 
* <span data-ttu-id="88e47-126">Ha törli a hello erőforráscsoportja hello [Azure-portálon][lnk-azure-portal], csak törli az erőforráscsoport hello erőforrások.</span><span class="sxs-lookup"><span data-stu-id="88e47-126">If you delete hello resource group in hello [Azure portal][lnk-azure-portal], you only delete hello resources in that resource group.</span></span> <span data-ttu-id="88e47-127">Szükség toodelete hello Azure Active Directory-alkalmazás társított előre konfigurált hello megoldás hello [a klasszikus Azure portálon][lnk-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="88e47-127">You also need toodelete hello Azure Active Directory application associated with hello preconfigured solution in hello [Azure classic portal][lnk-classic-portal].</span></span>

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a><span data-ttu-id="88e47-128">Az IoT-központ hány példányban hozhat létre az előfizetést?</span><span class="sxs-lookup"><span data-stu-id="88e47-128">How many IoT Hub instances can I provision in a subscription?</span></span>

<span data-ttu-id="88e47-129">Alapértelmezés szerint oszthat [10 IoT-központok előfizetésenként][link-azuresublimits].</span><span class="sxs-lookup"><span data-stu-id="88e47-129">By default you can provision [10 IoT hubs per subscription][link-azuresublimits].</span></span> <span data-ttu-id="88e47-130">Létrehozhat egy [az Azure támogatási jegy] [ link-azuresupportticket] tooraise ezt a határt.</span><span class="sxs-lookup"><span data-stu-id="88e47-130">You can create an [Azure support ticket][link-azuresupportticket] tooraise this limit.</span></span> <span data-ttu-id="88e47-131">Ennek eredményeképpen minden előkonfigurált megoldás szánt új IoT-központ óta is csak másolatot too10 kiépítése előre konfigurált egy adott előfizetésben megoldások.</span><span class="sxs-lookup"><span data-stu-id="88e47-131">As a result, since every preconfigured solution provisions a new IoT Hub, you can only provision up too10 preconfigured solutions in a given subscription.</span></span> 

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a><span data-ttu-id="88e47-132">Azure Cosmos DB hány példányban lehet kiépíteni egy előfizetésben?</span><span class="sxs-lookup"><span data-stu-id="88e47-132">How many Azure Cosmos DB instances can I provision in a subscription?</span></span>

<span data-ttu-id="88e47-133">Ötven.</span><span class="sxs-lookup"><span data-stu-id="88e47-133">Fifty.</span></span> <span data-ttu-id="88e47-134">Létrehozhat egy [az Azure támogatási jegy] [ link-azuresupportticket] tooraise ez korlátozza, de alapértelmezés szerint csak oszthat előfizetésenként legfeljebb 50 Cosmos DB példányt.</span><span class="sxs-lookup"><span data-stu-id="88e47-134">You can create an [Azure support ticket][link-azuresupportticket] tooraise this limit, but by default, you can only provision 50 Cosmos DB instances per subscription.</span></span> 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a><span data-ttu-id="88e47-135">Hány ingyenes Bing Térképek API-t adhatok meg egy előfizetésben?</span><span class="sxs-lookup"><span data-stu-id="88e47-135">How many Free Bing Maps APIs can I provision in a subscription?</span></span>

<span data-ttu-id="88e47-136">Kettőt.</span><span class="sxs-lookup"><span data-stu-id="88e47-136">Two.</span></span> <span data-ttu-id="88e47-137">Az Azure-előfizetéssel csak két belső tranzakciók szintjét 1 a Bing Maps vállalati terveket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="88e47-137">You can create only two Internal Transactions Level 1 Bing Maps for Enterprise plans in an Azure subscription.</span></span> <span data-ttu-id="88e47-138">hello távoli figyelési megoldást hello belső tranzakciók szintjét 1 csomaggal alapértelmezés szerint ki van építve.</span><span class="sxs-lookup"><span data-stu-id="88e47-138">hello remote monitoring solution is provisioned by default with hello Internal Transactions Level 1 plan.</span></span> <span data-ttu-id="88e47-139">Ennek eredményeképpen csak oszthat tootwo távoli figyelési megoldásoknak a módosítás nélkül előfizetés mentése.</span><span class="sxs-lookup"><span data-stu-id="88e47-139">As a result, you can only provision up tootwo remote monitoring solutions in a subscription with no modifications.</span></span>

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a><span data-ttu-id="88e47-140">Statikus térképpel rendelkező távoli figyelő megoldásom van, hogyan adhatok hozzá interaktív Bing-térképet?</span><span class="sxs-lookup"><span data-stu-id="88e47-140">I have a remote monitoring solution deployment with a static map, how do I add an interactive Bing map?</span></span>

1. <span data-ttu-id="88e47-141">A Bing térképek API beszerzése a vállalati QueryKey [Azure-portálon][lnk-azure-portal]:</span><span class="sxs-lookup"><span data-stu-id="88e47-141">Get your Bing Maps API for Enterprise QueryKey from [Azure portal][lnk-azure-portal]:</span></span> 
   
   1. <span data-ttu-id="88e47-142">Keresse meg az erőforráscsoportot, ahol a Bing térképek API-t a vállalati megtalálható-e hello toohello [Azure-portálon][lnk-azure-portal].</span><span class="sxs-lookup"><span data-stu-id="88e47-142">Navigate toohello Resource Group where your Bing Maps API for Enterprise is in hello [Azure portal][lnk-azure-portal].</span></span>
   2. <span data-ttu-id="88e47-143">Kattintson a **összes beállítás**, majd **kulcskezelés**.</span><span class="sxs-lookup"><span data-stu-id="88e47-143">Click **All Settings**, then **Key Management**.</span></span> 
   3. <span data-ttu-id="88e47-144">Két kulcsokat: **főkulcsos** és **QueryKey**.</span><span class="sxs-lookup"><span data-stu-id="88e47-144">You can see two keys: **MasterKey** and **QueryKey**.</span></span> <span data-ttu-id="88e47-145">Másolja a hello értéke **QueryKey**.</span><span class="sxs-lookup"><span data-stu-id="88e47-145">Copy hello value for **QueryKey**.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="88e47-146">Nem rendelkezik Vállalati Bing Térképek API-fiókkal?</span><span class="sxs-lookup"><span data-stu-id="88e47-146">Don't have a Bing Maps API for Enterprise account?</span></span> <span data-ttu-id="88e47-147">Hozzon létre egyet a hello [Azure-portálon] [ lnk-azure-portal] kattintva + új, a Bing térképek API-t a vállalati és kövesse a keresés kéri toocreate.</span><span class="sxs-lookup"><span data-stu-id="88e47-147">Create one in hello [Azure portal][lnk-azure-portal] by clicking + New, searching for Bing Maps API for Enterprise and follow prompts toocreate.</span></span>
      > 
      > 
2. <span data-ttu-id="88e47-148">Húzza le a legfrissebb kódot hello hello [Azure IoT-távoli-ellenőrző][lnk-remote-monitoring-github].</span><span class="sxs-lookup"><span data-stu-id="88e47-148">Pull down hello latest code from hello [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].</span></span>
3. <span data-ttu-id="88e47-149">Futtassa a helyi vagy felhőalapú üzemelő példány a következő hello parancssori üzembe helyezési útmutatót hello tárházban hello /docs/ mappában.</span><span class="sxs-lookup"><span data-stu-id="88e47-149">Run a local or cloud deployment following hello command-line deployment guidance in hello /docs/ folder in hello repository.</span></span> 
4. <span data-ttu-id="88e47-150">Azt követően már futtatta a helyi vagy felhőalapú üzemelő példány, keresse meg a gyökérmappában hello a *. telepítése során létrehozott user.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="88e47-150">After you've run a local or cloud deployment, look in your root folder for hello *.user.config file created during deployment.</span></span> <span data-ttu-id="88e47-151">Nyissa meg ezt a fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="88e47-151">Open this file in a text editor.</span></span> 
5. <span data-ttu-id="88e47-152">Változás hello következő sora tooinclude hello érték, amelyből másolta a **QueryKey**:</span><span class="sxs-lookup"><span data-stu-id="88e47-152">Change hello following line tooinclude hello value you copied from your **QueryKey**:</span></span> 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a><span data-ttu-id="88e47-153">Létrehozhatok előre konfigurált megoldást, ha Microsoft Azure for DreamSpark-fiókom van?</span><span class="sxs-lookup"><span data-stu-id="88e47-153">Can I create a preconfigured solution if I have Microsoft Azure for DreamSpark?</span></span>

<span data-ttu-id="88e47-154">Jelenleg nem hozható létre egy előre konfigurált megoldást egy [dreamspark révén a Microsoft Azure] [ lnk-dreamspark] fiók.</span><span class="sxs-lookup"><span data-stu-id="88e47-154">Currently, you cannot create a preconfigured solution with a [Microsoft Azure for DreamSpark][lnk-dreamspark] account.</span></span> <span data-ttu-id="88e47-155">Azonban létrehozhat egy [ingyenes próba-fiókot az Azure-] [ lnk-30daytrial] mindössze néhány percet, amely lehetővé teszi az előkonfigurált megoldás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="88e47-155">However, you can create a [free trial account for Azure][lnk-30daytrial] in just a couple of minutes that enables you create a preconfigured solution.</span></span>

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a><span data-ttu-id="88e47-156">Létrehozhatók előkonfigurált megoldás, ha a Cloud Solution Provider (CSP) előfizetés van?</span><span class="sxs-lookup"><span data-stu-id="88e47-156">Can I create a preconfigured solution if I have Cloud Solution Provider (CSP) subscription?</span></span>

<span data-ttu-id="88e47-157">A Cloud Solution Provider (CSP) előfizetést jelenleg nem hozható létre előre konfigurált megoldást.</span><span class="sxs-lookup"><span data-stu-id="88e47-157">Currently, you cannot create a preconfigured solution with a Cloud Solution Provider (CSP) subscription.</span></span> <span data-ttu-id="88e47-158">Azonban létrehozhat egy [ingyenes próba-fiókot az Azure-] [ lnk-30daytrial] mindössze néhány percet, amely lehetővé teszi az előkonfigurált megoldás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="88e47-158">However, you can create a [free trial account for Azure][lnk-30daytrial] in just a couple of minutes that enables you create a preconfigured solution.</span></span>

### <a name="how-do-i-delete-an-aad-tenant"></a><span data-ttu-id="88e47-159">Hogyan törölhetek egy AAD-bérlőt?</span><span class="sxs-lookup"><span data-stu-id="88e47-159">How do I delete an AAD tenant?</span></span>

<span data-ttu-id="88e47-160">Eric Golpe blogbejegyzésből [bemutató az Azure AD-bérlő törlésével][lnk-delete-aad-tennant].</span><span class="sxs-lookup"><span data-stu-id="88e47-160">See Eric Golpe's blog post [Walkthrough of Deleting an Azure AD Tenant][lnk-delete-aad-tennant].</span></span>

### <a name="next-steps"></a><span data-ttu-id="88e47-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="88e47-161">Next steps</span></span>

<span data-ttu-id="88e47-162">Akkor is is felfedezheti hello más szolgáltatásokat és képességeket hello előre konfigurált IoT Suite megoldások:</span><span class="sxs-lookup"><span data-stu-id="88e47-162">You can also explore some of hello other features and capabilities of hello IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="88e47-163">[Előre konfigurált prediktív karbantartási megoldás áttekintése][lnk-predictive-overview]</span><span class="sxs-lookup"><span data-stu-id="88e47-163">[Predictive maintenance preconfigured solution overview][lnk-predictive-overview]</span></span>
* [<span data-ttu-id="88e47-164">Előre konfigurált csatlakoztatott gyári megoldási áttekintés</span><span class="sxs-lookup"><span data-stu-id="88e47-164">Connected factory preconfigured solution overview</span></span>](iot-suite-connected-factory-overview.md)
* <span data-ttu-id="88e47-165">[A hello IoT biztonsági szabad][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="88e47-165">[IoT security from hello ground up][lnk-security-groundup]</span></span>

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
[lnk-cloud-deployment]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
[lnk-add-method]: iot-suite-guidance-on-customizing-preconfigured-solutions.md#add-support-for-a-new-method-to-the-simulator
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-predictive-maintenance-github]: https://github.com/Azure/azure-iot-predictive-maintenance
