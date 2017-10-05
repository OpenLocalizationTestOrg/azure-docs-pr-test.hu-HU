---
title: "Az Azure IoT Suite és a Logic Apps |} Microsoft Docs"
description: "Egy útmutatót a Logic Apps alkalmazásokat Azure IoT Suite bekötése üzleti folyamat a."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4629a7af-56ca-4b21-a769-5fa18bc3ab07
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: corywink
ms.openlocfilehash: 2e7997e2a8bdeeec6083d40acb55e653f87e140b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="30f48-103">Oktatóanyag: Logikai alkalmazás csatlakoztatása az Azure IoT Suite távoli megfigyelési előre konfigurált megoldás</span><span class="sxs-lookup"><span data-stu-id="30f48-103">Tutorial: Connect Logic App to your Azure IoT Suite Remote Monitoring preconfigured solution</span></span>
<span data-ttu-id="30f48-104">A [Microsoft Azure IoT Suite] [ lnk-internetofthings] távoli felügyeleti előkonfigurált megoldás használatának gyors megkezdése egy végpont készlet, amely egy IoT-megoldás exemplifies remek módja van.</span><span class="sxs-lookup"><span data-stu-id="30f48-104">The [Microsoft Azure IoT Suite][lnk-internetofthings] remote monitoring preconfigured solution is a great way to get started quickly with an end-to-end feature set that exemplifies an IoT solution.</span></span> <span data-ttu-id="30f48-105">Ez az oktatóanyag bemutatja, hogyan logikai alkalmazás felvétele a Microsoft Azure IoT Suite távoli felügyeleti előkonfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="30f48-105">This tutorial walks you through how to add Logic App to your Microsoft Azure IoT Suite remote monitoring preconfigured solution.</span></span> <span data-ttu-id="30f48-106">A lépések bemutatják, hogyan készíthet az IoT-megoldásból még tovább csatlakozva üzleti folyamatokat.</span><span class="sxs-lookup"><span data-stu-id="30f48-106">These steps demonstrate how you can take your IoT solution even further by connecting it to a business process.</span></span>

<span data-ttu-id="30f48-107">*Ha egy távoli megfigyelési előre konfigurált megoldás kiépítését általános bemutató keres, tekintse meg [oktatóanyag: Megismerkedés a előre konfigurált IoT-megoldások][lnk-getstarted].*</span><span class="sxs-lookup"><span data-stu-id="30f48-107">*If you’re looking for a walkthrough on how to provision a remote monitoring preconfigured solution, see [Tutorial: Get started with the IoT preconfigured solutions][lnk-getstarted].*</span></span>

<span data-ttu-id="30f48-108">Ez az oktatóanyag megkezdése előtt a következőket:</span><span class="sxs-lookup"><span data-stu-id="30f48-108">Before you start this tutorial, you should:</span></span>

* <span data-ttu-id="30f48-109">A távoli Azure-előfizetése figyelési előre konfigurált megoldás kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="30f48-109">Provision the remote monitoring preconfigured solution in your Azure subscription.</span></span>
* <span data-ttu-id="30f48-110">Ahhoz, hogy küldjön egy e-mailt, amely az üzleti folyamat elindítja a SendGrid-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="30f48-110">Create a SendGrid account to enable you to send an email that triggers your business process.</span></span> <span data-ttu-id="30f48-111">Regisztrálhat egy ingyenes próbafiók, a [SendGrid](https://sendgrid.com/) kattintva **próbálja szabad**.</span><span class="sxs-lookup"><span data-stu-id="30f48-111">You can sign up for a free trial account at [SendGrid](https://sendgrid.com/) by clicking **Try for Free**.</span></span> <span data-ttu-id="30f48-112">A próbafiókot regisztrálása után kell létrehoznia egy [API-kulcs](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) a SendGrid, amely engedélyt ad az e-mailek küldését.</span><span class="sxs-lookup"><span data-stu-id="30f48-112">After you have registered for your free trial account, you need to create an [API key](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) in SendGrid that grants permissions to send mail.</span></span> <span data-ttu-id="30f48-113">Az oktatóanyag későbbi részében szüksége az API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="30f48-113">You need this API key later in the tutorial.</span></span>

<span data-ttu-id="30f48-114">Az oktatóanyag teljesítéséhez szüksége van a Visual Studio 2015-öt vagy a Visual Studio 2017 az előkonfigurált megoldás háttérbeli műveletek módosítását.</span><span class="sxs-lookup"><span data-stu-id="30f48-114">To complete this tutorial, you need Visual Studio 2015 or Visual Studio 2017 to modify the actions in the preconfigured solution back end.</span></span>

<span data-ttu-id="30f48-115">Feltéve, hogy már megtörtént a távoli figyelésének előre konfigurált megoldást, keresse meg az adott megoldáshoz tartozó erőforráscsoport a [Azure-portálon][lnk-azureportal].</span><span class="sxs-lookup"><span data-stu-id="30f48-115">Assuming you’ve already provisioned your remote monitoring preconfigured solution, navigate to the resource group for that solution in the [Azure portal][lnk-azureportal].</span></span> <span data-ttu-id="30f48-116">Az erőforráscsoport neve megegyezik a megoldás neve rendelkezik úgy döntött, hogy amikor a távoli felügyeleti megoldás létesített.</span><span class="sxs-lookup"><span data-stu-id="30f48-116">The resource group has the same name as the solution name you chose when you provisioned your remote monitoring solution.</span></span> <span data-ttu-id="30f48-117">Az erőforráscsoportot látja a megoldás, kivéve az Azure Active Directory-alkalmazás, amely a klasszikus Azure portálon található összes kiépített Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="30f48-117">In the resource group, you can see all the provisioned Azure resources for your solution except for the Azure Active Directory application that you can find in the Azure Classic Portal.</span></span> <span data-ttu-id="30f48-118">Az alábbi képernyőfelvételen szereplő példán látható **erőforráscsoport** egy távoli figyelés panel előre konfigurált megoldást:</span><span class="sxs-lookup"><span data-stu-id="30f48-118">The following screenshot shows an example **Resource group** blade for a remote monitoring preconfigured solution:</span></span>

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

<span data-ttu-id="30f48-119">Első lépésként állítsa be a logikai alkalmazást az előkonfigurált megoldás használandó.</span><span class="sxs-lookup"><span data-stu-id="30f48-119">To begin, set up the logic app to use with the preconfigured solution.</span></span>

## <a name="set-up-the-logic-app"></a><span data-ttu-id="30f48-120">A logikai alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="30f48-120">Set up the Logic App</span></span>
1. <span data-ttu-id="30f48-121">Kattintson a **Hozzáadás** tetején található az erőforráscsoport panel az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="30f48-121">Click **Add** at the top of your resource group blade in the Azure portal.</span></span>
2. <span data-ttu-id="30f48-122">Keresse meg **logikai alkalmazás**, válassza ki azt, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="30f48-122">Search for **Logic App**, select it and then click **Create**.</span></span>
3. <span data-ttu-id="30f48-123">Töltse ki a **neve** és ugyanazon **előfizetés** és **erőforráscsoport** használja, ha a távoli felügyeleti megoldás létesített.</span><span class="sxs-lookup"><span data-stu-id="30f48-123">Fill out the **Name** and use the same **Subscription** and **Resource group** that you used when you provisioned your remote monitoring solution.</span></span> <span data-ttu-id="30f48-124">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="30f48-124">Click **Create**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)
4. <span data-ttu-id="30f48-125">A telepítés befejeződése után megtekintheti a logikai alkalmazás szerepel az erőforráscsoportban erőforrásként.</span><span class="sxs-lookup"><span data-stu-id="30f48-125">When your deployment completes, you can see the Logic App is listed as a resource in your resource group.</span></span>
5. <span data-ttu-id="30f48-126">Kattintson a lehetőségre, és navigáljon a Logic App panelen válassza ki a logikai alkalmazást a **üres logikai alkalmazás** sablon megnyitásához az **Logic Apps Designer**.</span><span class="sxs-lookup"><span data-stu-id="30f48-126">Click the Logic App to navigate to the Logic App blade, select the **Blank Logic App** template to open the **Logic Apps Designer**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)
6. <span data-ttu-id="30f48-127">Válassza ki **kérelem**.</span><span class="sxs-lookup"><span data-stu-id="30f48-127">Select **Request**.</span></span> <span data-ttu-id="30f48-128">Ez a művelet határozza meg, hogy egy adott JSON azonosítójú bejövő HTTP-kérelem hasznos tevékenységéért formázva eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="30f48-128">This action specifies that an incoming HTTP request with a specific JSON formatted payload acts as a trigger.</span></span>
7. <span data-ttu-id="30f48-129">A kérelem törzsében JSON-séma illessze be a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="30f48-129">Paste the following code into the Request Body JSON Schema:</span></span>
   
    ```json
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
   
   > [!NOTE]
   > <span data-ttu-id="30f48-130">Másolhatja az URL-cím a HTTP POST után a logikai alkalmazás menti, de először fel kell venni egy műveletet.</span><span class="sxs-lookup"><span data-stu-id="30f48-130">You can copy the URL for the HTTP post after you save the logic app, but first you must add an action.</span></span>
   > 
   > 
8. <span data-ttu-id="30f48-131">Kattintson a **+ új lépés** a kézi indítási alatt.</span><span class="sxs-lookup"><span data-stu-id="30f48-131">Click **+ New step** under your manual trigger.</span></span> <span data-ttu-id="30f48-132">Kattintson a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="30f48-132">Then click **Add an action**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)
9. <span data-ttu-id="30f48-133">Keresse meg **SendGrid - e-mail küldése** , és kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="30f48-133">Search for **SendGrid - Send email** and click it.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)
10. <span data-ttu-id="30f48-134">Írjon be egy nevet a kapcsolathoz, például **SendGridConnection**, adja meg a **SendGrid API-kulcs** alapteljesítményhez képest, a SendGrid fiók beállítását, majd kattintson az **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="30f48-134">Enter a name for the connection, such as **SendGridConnection**, enter the **SendGrid API Key** you created when you set up your SendGrid account, and click **Create**.</span></span>
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)
11. <span data-ttu-id="30f48-135">Saját mindkettőnek e-mail címek hozzáadása a **a** és **való** mezőket.</span><span class="sxs-lookup"><span data-stu-id="30f48-135">Add email addresses you own to both the **From** and **To** fields.</span></span> <span data-ttu-id="30f48-136">Adja hozzá **távoli figyelési figyelmeztetés [DeviceId]** számára a **tulajdonos** mező.</span><span class="sxs-lookup"><span data-stu-id="30f48-136">Add **Remote monitoring alert [DeviceId]** to the **Subject** field.</span></span> <span data-ttu-id="30f48-137">Az a **E-mail törzsének** mezőbe hozzáadása **[DeviceId] jelzett [measurementName] [measuredValue] értékű.**</span><span class="sxs-lookup"><span data-stu-id="30f48-137">In the **Email Body** field, add **Device [DeviceId] has reported [measurementName] with value [measuredValue].**</span></span> <span data-ttu-id="30f48-138">Hozzáadhat **[DeviceId]**, **[measurementName]**, és **[measuredValue]** elemre kattintva a **adatok beszúrásához előző lépéseiből** szakasz.</span><span class="sxs-lookup"><span data-stu-id="30f48-138">You can add **[DeviceId]**, **[measurementName]**, and **[measuredValue]** by clicking in the **You can insert data from previous steps** section.</span></span>
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)
12. <span data-ttu-id="30f48-139">Kattintson a **mentése** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="30f48-139">Click **Save** in the top menu.</span></span>
13. <span data-ttu-id="30f48-140">Kattintson a **kérelem** eseményindító, és másolja a **az URL-címet a Http Post** érték.</span><span class="sxs-lookup"><span data-stu-id="30f48-140">Click the **Request** trigger and copy the **Http Post to this URL** value.</span></span> <span data-ttu-id="30f48-141">Az oktatóanyag későbbi részében szüksége az URL-cím.</span><span class="sxs-lookup"><span data-stu-id="30f48-141">You need this URL later in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="30f48-142">A Logic Apps lehetővé teszik a futtatásához [művelet számos különböző típusú] [ lnk-logic-apps-actions] műveletek beleértve az Office 365-ben.</span><span class="sxs-lookup"><span data-stu-id="30f48-142">Logic Apps enable you to run [many different types of action][lnk-logic-apps-actions] including actions in Office 365.</span></span> 
> 
> 

## <a name="set-up-the-eventprocessor-web-job"></a><span data-ttu-id="30f48-143">A EventProcessor webes feladat beállítása</span><span class="sxs-lookup"><span data-stu-id="30f48-143">Set up the EventProcessor Web Job</span></span>
<span data-ttu-id="30f48-144">Ebben a szakaszban az előkonfigurált megoldás a létrehozott logikai alkalmazás csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="30f48-144">In this section, you connect your preconfigured solution to the Logic App you created.</span></span> <span data-ttu-id="30f48-145">A feladat végrehajtásához hozzá indul el, a művelet, amely akkor következik be, ha egy eszköz érzékelő érték meghaladja a küszöbértéket a logikai alkalmazás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="30f48-145">To complete this task, you add the URL to trigger the Logic App to the action that fires when a device sensor value exceeds a threshold.</span></span>

1. <span data-ttu-id="30f48-146">A git-ügyfél segítségével klónozza a legújabb verzióját a [azure iot-távoli-ellenőrző github-tárházban][lnk-rmgithub].</span><span class="sxs-lookup"><span data-stu-id="30f48-146">Use your git client to clone the latest version of the [azure-iot-remote-monitoring github repository][lnk-rmgithub].</span></span> <span data-ttu-id="30f48-147">Példa:</span><span class="sxs-lookup"><span data-stu-id="30f48-147">For example:</span></span>
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. <span data-ttu-id="30f48-148">A Visual Studióban nyissa meg a **RemoteMonitoring.sln** a tárházat a helyi másolatát.</span><span class="sxs-lookup"><span data-stu-id="30f48-148">In Visual Studio, open the **RemoteMonitoring.sln** from the local copy of the repository.</span></span>
3. <span data-ttu-id="30f48-149">Nyissa meg a **ActionRepository.cs** fájlt a **infrastruktúra\\tárház** mappát.</span><span class="sxs-lookup"><span data-stu-id="30f48-149">Open the **ActionRepository.cs** file in the **Infrastructure\\Repository** folder.</span></span>
4. <span data-ttu-id="30f48-150">Frissítés a **actionIds** szótár a **Http Post az URL-címet** , itt megjegyezni a Logic Apps alkalmazást az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="30f48-150">Update the **actionIds** dictionary with the **Http Post to this URL** you noted from your Logic App as follows:</span></span>
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this URL>" },
        { "Raise Alarm", "<Http Post to this URL>" }
    };
    ```
5. <span data-ttu-id="30f48-151">A módosítások mentése a megoldásban, és zárja be a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="30f48-151">Save the changes in solution and exit Visual Studio.</span></span>

## <a name="deploy-from-the-command-line"></a><span data-ttu-id="30f48-152">Központi telepítése a parancssorból</span><span class="sxs-lookup"><span data-stu-id="30f48-152">Deploy from the command line</span></span>
<span data-ttu-id="30f48-153">Ebben a szakaszban központi telepítése a frissített verzióját a távoli figyelési megoldást cserélje le az Azure-ban jelenleg futó verziója.</span><span class="sxs-lookup"><span data-stu-id="30f48-153">In this section, you deploy your updated version of the remote monitoring solution to replace the version currently running in Azure.</span></span>

1. <span data-ttu-id="30f48-154">A következő a [fejlesztői beállításról] [ lnk-devsetup] állítsa be a környezetet a telepítési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="30f48-154">Following the [dev set-up][lnk-devsetup] instructions to set up your environment for deployment.</span></span>
2. <span data-ttu-id="30f48-155">Helyileg telepítéséhez kövesse a [helyi telepítés] [ lnk-localdeploy] utasításokat.</span><span class="sxs-lookup"><span data-stu-id="30f48-155">To deploy locally, follow the [local deployment][lnk-localdeploy] instructions.</span></span>
3. <span data-ttu-id="30f48-156">Telepíti a felhőbe, és a meglévő felhőalapú üzemelő példány frissítése, kövesse a [felhőalapú üzemelő példány] [ lnk-clouddeploy] utasításokat.</span><span class="sxs-lookup"><span data-stu-id="30f48-156">To deploy to the cloud and update your existing cloud deployment, follow the [cloud deployment][lnk-clouddeploy] instructions.</span></span> <span data-ttu-id="30f48-157">Használja a nevét, az eredeti telepítés a központi telepítés nevét.</span><span class="sxs-lookup"><span data-stu-id="30f48-157">Use the name of your original deployment as the deployment name.</span></span> <span data-ttu-id="30f48-158">Például ha az eredeti telepítési hívták **demologicapp**, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="30f48-158">For example if the original deployment was called **demologicapp**, use the following command:</span></span>
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   <span data-ttu-id="30f48-159">Ha fut a build parancsfájl, ügyeljen arra, hogy a azonos Azure-fiók, előfizetés, régió, valamint a megoldás létesített használt Active Directory-példányban.</span><span class="sxs-lookup"><span data-stu-id="30f48-159">When the build script runs, be sure to use the same Azure account, subscription, region, and Active Directory instance you used when you provisioned the solution.</span></span>

## <a name="see-your-logic-app-in-action"></a><span data-ttu-id="30f48-160">Tekintse meg a Logic Apps alkalmazást működés közben</span><span class="sxs-lookup"><span data-stu-id="30f48-160">See your Logic App in action</span></span>
<span data-ttu-id="30f48-161">A távoli felügyeleti előkonfigurált megoldás beállítása alapértelmezés szerint, ha a megoldás két szabályt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="30f48-161">The remote monitoring preconfigured solution has two rules set up by default when you provision a solution.</span></span> <span data-ttu-id="30f48-162">Szabályokat is szerepelnek a **SampleDevice001** eszköz:</span><span class="sxs-lookup"><span data-stu-id="30f48-162">Both rules are on the **SampleDevice001** device:</span></span>

* <span data-ttu-id="30f48-163">Hőmérséklet > 38.00</span><span class="sxs-lookup"><span data-stu-id="30f48-163">Temperature > 38.00</span></span>
* <span data-ttu-id="30f48-164">Páratartalom > 48.00</span><span class="sxs-lookup"><span data-stu-id="30f48-164">Humidity > 48.00</span></span>

<span data-ttu-id="30f48-165">A hőmérséklet szabály eseményindítók a **előléptetése riasztás** szabályok eseményindítók művelet és a páratartalom a **SendMessage** művelet.</span><span class="sxs-lookup"><span data-stu-id="30f48-165">The temperature rule triggers the **Raise Alarm** action and the Humidity rule triggers the **SendMessage** action.</span></span> <span data-ttu-id="30f48-166">Feltéve, hogy mindkét művelet azonos URL-CÍMÉT használja a **ActionRepository** osztály, vagy a szabály a logic app eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="30f48-166">Assuming you used the same URL for both actions the **ActionRepository** class, your logic app triggers for either rule.</span></span> <span data-ttu-id="30f48-167">Szabályokat is SendGrid segítségével e-mail küldése a **való** cím a riasztás részleteit.</span><span class="sxs-lookup"><span data-stu-id="30f48-167">Both rules use SendGrid to send an email to the **To** address with details of the alert.</span></span>

> [!NOTE]
> <span data-ttu-id="30f48-168">A logikai alkalmazás továbbra is fennáll, minden alkalommal, amikor a küszöbérték elérése indításához.</span><span class="sxs-lookup"><span data-stu-id="30f48-168">The Logic App continues to trigger every time the threshold is met.</span></span> <span data-ttu-id="30f48-169">Felesleges e-mailek elkerülése érdekében tiltsa le a szabályok a megoldás portálon, vagy tiltsa le a logikai alkalmazást a a [Azure-portálon][lnk-azureportal].</span><span class="sxs-lookup"><span data-stu-id="30f48-169">To avoid unnecessary emails, you can either disable the rules in your solution portal or disable the Logic App in the [Azure portal][lnk-azureportal].</span></span>
> 
> 

<span data-ttu-id="30f48-170">Mellett fogadásakor, a logikai alkalmazás futtatásakor a portálon is megtekintheti:</span><span class="sxs-lookup"><span data-stu-id="30f48-170">In addition to receiving emails, you can also see when the Logic App runs in the portal:</span></span>

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a><span data-ttu-id="30f48-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="30f48-171">Next steps</span></span>
<span data-ttu-id="30f48-172">Most, hogy a logikai alkalmazás már használta az előkonfigurált megoldás csatlakozni az üzleti folyamatokat, hogy többet is megtudhat az előkonfigurált megoldásokat testreszabására szolgáló beállítások:</span><span class="sxs-lookup"><span data-stu-id="30f48-172">Now that you've used a Logic App to connect the preconfigured solution to a business process, you can learn more about the options for customizing the preconfigured solutions:</span></span>

* <span data-ttu-id="30f48-173">[Dinamikus telemetriai adatokat a távoli felügyeleti előkonfigurált megoldás][lnk-dynamic]</span><span class="sxs-lookup"><span data-stu-id="30f48-173">[Use dynamic telemetry with the remote monitoring preconfigured solution][lnk-dynamic]</span></span>
* <span data-ttu-id="30f48-174">[Eszköz információk metaadatait a távoli felügyeleti előkonfigurált megoldás][lnk-devinfo]</span><span class="sxs-lookup"><span data-stu-id="30f48-174">[Device information metadata in the remote monitoring preconfigured solution][lnk-devinfo]</span></span>

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
