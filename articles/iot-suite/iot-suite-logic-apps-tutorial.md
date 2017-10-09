---
title: "aaaAzure IoT Suite és a Logic Apps |} Microsoft Docs"
description: "Egy útmutatót a Logic Apps tooAzure IoT Suite üzleti folyamat másolatot toohook."
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
ms.openlocfilehash: 6ef7311ac38f4e2ddb032cff0fb73591da5f76c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-connect-logic-app-tooyour-azure-iot-suite-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="81fc9-103">Oktatóanyag: Logikai alkalmazás tooyour Azure IoT Suite távoli figyelésére szolgáló előre konfigurált megoldás csatlakozás</span><span class="sxs-lookup"><span data-stu-id="81fc9-103">Tutorial: Connect Logic App tooyour Azure IoT Suite Remote Monitoring preconfigured solution</span></span>
<span data-ttu-id="81fc9-104">Hello [Microsoft Azure IoT Suite] [ lnk-internetofthings] távoli felügyeleti előkonfigurált megoldás egy kiváló módja tooget parancsot gyorsan egy végpont készlet, amely egy IoT-megoldás exemplifies.</span><span class="sxs-lookup"><span data-stu-id="81fc9-104">hello [Microsoft Azure IoT Suite][lnk-internetofthings] remote monitoring preconfigured solution is a great way tooget started quickly with an end-to-end feature set that exemplifies an IoT solution.</span></span> <span data-ttu-id="81fc9-105">Ez az oktatóanyag bemutatja, hogyan hogyan tooadd logikai alkalmazás tooyour Microsoft Azure IoT Suite távoli megfigyelési előre konfigurált a megoldás.</span><span class="sxs-lookup"><span data-stu-id="81fc9-105">This tutorial walks you through how tooadd Logic App tooyour Microsoft Azure IoT Suite remote monitoring preconfigured solution.</span></span> <span data-ttu-id="81fc9-106">Ezen lépések azt ismertetik, hogyan készíthet az IoT-megoldásból még tovább tooa üzleti folyamat csatlakoztatásával.</span><span class="sxs-lookup"><span data-stu-id="81fc9-106">These steps demonstrate how you can take your IoT solution even further by connecting it tooa business process.</span></span>

<span data-ttu-id="81fc9-107">*Ha egy általános bemutató hogyan egy távoli megfigyelési tooprovision előre konfigurált megoldást keres, tekintse meg [oktatóanyag: Ismerkedés a hello előre konfigurált IoT-megoldások][lnk-getstarted].*</span><span class="sxs-lookup"><span data-stu-id="81fc9-107">*If you’re looking for a walkthrough on how tooprovision a remote monitoring preconfigured solution, see [Tutorial: Get started with hello IoT preconfigured solutions][lnk-getstarted].*</span></span>

<span data-ttu-id="81fc9-108">Ez az oktatóanyag megkezdése előtt a következőket:</span><span class="sxs-lookup"><span data-stu-id="81fc9-108">Before you start this tutorial, you should:</span></span>

* <span data-ttu-id="81fc9-109">Kiépítés hello távoli felügyeleti előkonfigurált megoldás az Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="81fc9-109">Provision hello remote monitoring preconfigured solution in your Azure subscription.</span></span>
* <span data-ttu-id="81fc9-110">A SendGrid fiók tooenable hozzon létre egy e-mailt, amely elindítja az üzleti folyamat toosend.</span><span class="sxs-lookup"><span data-stu-id="81fc9-110">Create a SendGrid account tooenable you toosend an email that triggers your business process.</span></span> <span data-ttu-id="81fc9-111">Regisztrálhat egy ingyenes próbafiók, a [SendGrid](https://sendgrid.com/) kattintva **próbálja szabad**.</span><span class="sxs-lookup"><span data-stu-id="81fc9-111">You can sign up for a free trial account at [SendGrid](https://sendgrid.com/) by clicking **Try for Free**.</span></span> <span data-ttu-id="81fc9-112">A próbafiókot regisztrálása után toocreate kell egy [API-kulcs](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) a SendGrid, amely engedélyt toosend mail.</span><span class="sxs-lookup"><span data-stu-id="81fc9-112">After you have registered for your free trial account, you need toocreate an [API key](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) in SendGrid that grants permissions toosend mail.</span></span> <span data-ttu-id="81fc9-113">Az API-kulcs hello oktatóanyag későbbi részében szüksége.</span><span class="sxs-lookup"><span data-stu-id="81fc9-113">You need this API key later in hello tutorial.</span></span>

<span data-ttu-id="81fc9-114">toocomplete ebben az oktatóanyagban Visual Studio 2015-öt vagy a Visual Studio 2017 toomodify hello műveletek hello előkonfigurált megoldás háttérrendszerének van szüksége.</span><span class="sxs-lookup"><span data-stu-id="81fc9-114">toocomplete this tutorial, you need Visual Studio 2015 or Visual Studio 2017 toomodify hello actions in hello preconfigured solution back end.</span></span>

<span data-ttu-id="81fc9-115">Feltéve, hogy már megtörtént a távoli figyelésének előre konfigurált megoldást, keresse meg az adott megoldás hello toohello erőforráscsoport [Azure-portálon][lnk-azureportal].</span><span class="sxs-lookup"><span data-stu-id="81fc9-115">Assuming you’ve already provisioned your remote monitoring preconfigured solution, navigate toohello resource group for that solution in hello [Azure portal][lnk-azureportal].</span></span> <span data-ttu-id="81fc9-116">hello erőforráscsoport rendelkezik azonos hello megoldás nevét, nevet hello döntött, hogy amikor a távoli felügyeleti megoldás létesített.</span><span class="sxs-lookup"><span data-stu-id="81fc9-116">hello resource group has hello same name as hello solution name you chose when you provisioned your remote monitoring solution.</span></span> <span data-ttu-id="81fc9-117">Hello erőforráscsoportban láthatja, minden hello kiépített Azure-erőforrások kivételével hello Azure Active Directory-alkalmazás, amely megtalálható a klasszikus Azure portál hello megoldást.</span><span class="sxs-lookup"><span data-stu-id="81fc9-117">In hello resource group, you can see all hello provisioned Azure resources for your solution except for hello Azure Active Directory application that you can find in hello Azure Classic Portal.</span></span> <span data-ttu-id="81fc9-118">hello alábbi képernyőfelvételen szereplő példán látható **erőforráscsoport** egy távoli figyelés panel előre konfigurált megoldást:</span><span class="sxs-lookup"><span data-stu-id="81fc9-118">hello following screenshot shows an example **Resource group** blade for a remote monitoring preconfigured solution:</span></span>

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

<span data-ttu-id="81fc9-119">toobegin, hello logic app toouse a hello beállítása előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="81fc9-119">toobegin, set up hello logic app toouse with hello preconfigured solution.</span></span>

## <a name="set-up-hello-logic-app"></a><span data-ttu-id="81fc9-120">Hello logikai alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="81fc9-120">Set up hello Logic App</span></span>
1. <span data-ttu-id="81fc9-121">Kattintson a **Hozzáadás** az erőforráscsoport panel az Azure-portálon hello hello tetején.</span><span class="sxs-lookup"><span data-stu-id="81fc9-121">Click **Add** at hello top of your resource group blade in hello Azure portal.</span></span>
2. <span data-ttu-id="81fc9-122">Keresse meg **logikai alkalmazás**, válassza ki azt, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="81fc9-122">Search for **Logic App**, select it and then click **Create**.</span></span>
3. <span data-ttu-id="81fc9-123">Töltse ki a hello **neve** és használja az azonos hello **előfizetés** és **erőforráscsoport** használja, ha a távoli felügyeleti megoldás létesített.</span><span class="sxs-lookup"><span data-stu-id="81fc9-123">Fill out hello **Name** and use hello same **Subscription** and **Resource group** that you used when you provisioned your remote monitoring solution.</span></span> <span data-ttu-id="81fc9-124">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="81fc9-124">Click **Create**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)
4. <span data-ttu-id="81fc9-125">A telepítés befejezését követően az erőforráscsoportban hello logikai alkalmazás szerepel a listán látható erőforrásként.</span><span class="sxs-lookup"><span data-stu-id="81fc9-125">When your deployment completes, you can see hello Logic App is listed as a resource in your resource group.</span></span>
5. <span data-ttu-id="81fc9-126">Kattintson a hello logikai alkalmazás toonavigate toohello logikai alkalmazás paneljén válassza hello **üres logikai alkalmazás** sablon tooopen hello **Logic Apps Designer**.</span><span class="sxs-lookup"><span data-stu-id="81fc9-126">Click hello Logic App toonavigate toohello Logic App blade, select hello **Blank Logic App** template tooopen hello **Logic Apps Designer**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)
6. <span data-ttu-id="81fc9-127">Válassza ki **kérelem**.</span><span class="sxs-lookup"><span data-stu-id="81fc9-127">Select **Request**.</span></span> <span data-ttu-id="81fc9-128">Ez a művelet határozza meg, hogy egy adott JSON azonosítójú bejövő HTTP-kérelem hasznos tevékenységéért formázva eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="81fc9-128">This action specifies that an incoming HTTP request with a specific JSON formatted payload acts as a trigger.</span></span>
7. <span data-ttu-id="81fc9-129">Illessze be a következő kódot a kérelem törzsében JSON-séma hello hello:</span><span class="sxs-lookup"><span data-stu-id="81fc9-129">Paste hello following code into hello Request Body JSON Schema:</span></span>
   
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
   > <span data-ttu-id="81fc9-130">Hello HTTP post hello URL-címe másolhatja után mentenie hello logikai alkalmazás, de először fel kell venni egy műveletet.</span><span class="sxs-lookup"><span data-stu-id="81fc9-130">You can copy hello URL for hello HTTP post after you save hello logic app, but first you must add an action.</span></span>
   > 
   > 
8. <span data-ttu-id="81fc9-131">Kattintson a **+ új lépés** a kézi indítási alatt.</span><span class="sxs-lookup"><span data-stu-id="81fc9-131">Click **+ New step** under your manual trigger.</span></span> <span data-ttu-id="81fc9-132">Kattintson a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="81fc9-132">Then click **Add an action**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)
9. <span data-ttu-id="81fc9-133">Keresse meg **SendGrid - e-mail küldése** , és kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="81fc9-133">Search for **SendGrid - Send email** and click it.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)
10. <span data-ttu-id="81fc9-134">Adja meg például hello kapcsolat nevét **SendGridConnection**, adja meg a hello **SendGrid API-kulcs** alapteljesítményhez képest, a SendGrid fiók beállítását, majd kattintson az **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="81fc9-134">Enter a name for hello connection, such as **SendGridConnection**, enter hello **SendGrid API Key** you created when you set up your SendGrid account, and click **Create**.</span></span>
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)
11. <span data-ttu-id="81fc9-135">Adja hozzá a e-mail címek saját tooboth hello **a** és **való** mezőket.</span><span class="sxs-lookup"><span data-stu-id="81fc9-135">Add email addresses you own tooboth hello **From** and **To** fields.</span></span> <span data-ttu-id="81fc9-136">Adja hozzá **távoli figyelési figyelmeztetés [DeviceId]** toohello **tulajdonos** mező.</span><span class="sxs-lookup"><span data-stu-id="81fc9-136">Add **Remote monitoring alert [DeviceId]** toohello **Subject** field.</span></span> <span data-ttu-id="81fc9-137">A hello **E-mail törzsének** mezőbe hozzáadása **[DeviceId] jelzett [measurementName] [measuredValue] értékű.**</span><span class="sxs-lookup"><span data-stu-id="81fc9-137">In hello **Email Body** field, add **Device [DeviceId] has reported [measurementName] with value [measuredValue].**</span></span> <span data-ttu-id="81fc9-138">Hozzáadhat **[DeviceId]**, **[measurementName]**, és **[measuredValue]** hello elemre kattintva **adatok beszúrásához előző lépéseiből**szakasz.</span><span class="sxs-lookup"><span data-stu-id="81fc9-138">You can add **[DeviceId]**, **[measurementName]**, and **[measuredValue]** by clicking in hello **You can insert data from previous steps** section.</span></span>
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)
12. <span data-ttu-id="81fc9-139">Kattintson a **mentése** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="81fc9-139">Click **Save** in hello top menu.</span></span>
13. <span data-ttu-id="81fc9-140">Hello kattintson **kérelem** eseményindítója és másolási hello **Http Post toothis URL-cím** érték.</span><span class="sxs-lookup"><span data-stu-id="81fc9-140">Click hello **Request** trigger and copy hello **Http Post toothis URL** value.</span></span> <span data-ttu-id="81fc9-141">Az oktatóanyag későbbi részében szüksége az URL-cím.</span><span class="sxs-lookup"><span data-stu-id="81fc9-141">You need this URL later in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="81fc9-142">A Logic Apps lehetővé teszik a toorun [művelet számos különböző típusú] [ lnk-logic-apps-actions] műveletek beleértve az Office 365-ben.</span><span class="sxs-lookup"><span data-stu-id="81fc9-142">Logic Apps enable you toorun [many different types of action][lnk-logic-apps-actions] including actions in Office 365.</span></span> 
> 
> 

## <a name="set-up-hello-eventprocessor-web-job"></a><span data-ttu-id="81fc9-143">Hello EventProcessor webes projekt beállítása</span><span class="sxs-lookup"><span data-stu-id="81fc9-143">Set up hello EventProcessor Web Job</span></span>
<span data-ttu-id="81fc9-144">Ebben a szakaszban az előkonfigurált megoldás toohello létrehozott logikai alkalmazás csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="81fc9-144">In this section, you connect your preconfigured solution toohello Logic App you created.</span></span> <span data-ttu-id="81fc9-145">toocomplete ebben a feladatban hozzáadása hello URL-tootrigger hello logikai alkalmazás toohello művelet, amely akkor következik be, ha egy eszköz érzékelő érték meghaladja a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="81fc9-145">toocomplete this task, you add hello URL tootrigger hello Logic App toohello action that fires when a device sensor value exceeds a threshold.</span></span>

1. <span data-ttu-id="81fc9-146">A git ügyfél tooclone hello legújabb verzióját használja hello [azure iot-távoli-ellenőrző github-tárházban][lnk-rmgithub].</span><span class="sxs-lookup"><span data-stu-id="81fc9-146">Use your git client tooclone hello latest version of hello [azure-iot-remote-monitoring github repository][lnk-rmgithub].</span></span> <span data-ttu-id="81fc9-147">Példa:</span><span class="sxs-lookup"><span data-stu-id="81fc9-147">For example:</span></span>
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. <span data-ttu-id="81fc9-148">A Visual Studióban nyissa meg a hello **RemoteMonitoring.sln** hello hello tárház helyi másolatát.</span><span class="sxs-lookup"><span data-stu-id="81fc9-148">In Visual Studio, open hello **RemoteMonitoring.sln** from hello local copy of hello repository.</span></span>
3. <span data-ttu-id="81fc9-149">Nyissa meg hello **ActionRepository.cs** hello fájlban **infrastruktúra\\tárház** mappát.</span><span class="sxs-lookup"><span data-stu-id="81fc9-149">Open hello **ActionRepository.cs** file in hello **Infrastructure\\Repository** folder.</span></span>
4. <span data-ttu-id="81fc9-150">Frissítés hello **actionIds** hello szótár **Http Post toothis URL-cím** , itt megjegyezni a Logic Apps alkalmazást az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="81fc9-150">Update hello **actionIds** dictionary with hello **Http Post toothis URL** you noted from your Logic App as follows:</span></span>
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post toothis URL>" },
        { "Raise Alarm", "<Http Post toothis URL>" }
    };
    ```
5. <span data-ttu-id="81fc9-151">Hello módosítások mentése a megoldásban, és zárja be a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="81fc9-151">Save hello changes in solution and exit Visual Studio.</span></span>

## <a name="deploy-from-hello-command-line"></a><span data-ttu-id="81fc9-152">Hello parancssorból telepítése</span><span class="sxs-lookup"><span data-stu-id="81fc9-152">Deploy from hello command line</span></span>
<span data-ttu-id="81fc9-153">Ebben a szakaszban központi telepítése a frissített verzió hello távoli figyelési megoldás tooreplace hello verziója fut az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="81fc9-153">In this section, you deploy your updated version of hello remote monitoring solution tooreplace hello version currently running in Azure.</span></span>

1. <span data-ttu-id="81fc9-154">Következő hello [fejlesztői beállításról] [ lnk-devsetup] utasításokat tooset telepítési környezet.</span><span class="sxs-lookup"><span data-stu-id="81fc9-154">Following hello [dev set-up][lnk-devsetup] instructions tooset up your environment for deployment.</span></span>
2. <span data-ttu-id="81fc9-155">toodeploy helyileg, hajtsa végre a hello [helyi telepítés] [ lnk-localdeploy] utasításokat.</span><span class="sxs-lookup"><span data-stu-id="81fc9-155">toodeploy locally, follow hello [local deployment][lnk-localdeploy] instructions.</span></span>
3. <span data-ttu-id="81fc9-156">toodeploy toohello a felhő és a meglévő felhőalapú telepítés frissítéséhez hajtsa végre az hello [felhőalapú üzemelő példány] [ lnk-clouddeploy] utasításokat.</span><span class="sxs-lookup"><span data-stu-id="81fc9-156">toodeploy toohello cloud and update your existing cloud deployment, follow hello [cloud deployment][lnk-clouddeploy] instructions.</span></span> <span data-ttu-id="81fc9-157">Használja az eredeti telepítés hello nevét hello üzemelő példány neve.</span><span class="sxs-lookup"><span data-stu-id="81fc9-157">Use hello name of your original deployment as hello deployment name.</span></span> <span data-ttu-id="81fc9-158">Például ha az eredeti telepítési hello hívták **demologicapp**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="81fc9-158">For example if hello original deployment was called **demologicapp**, use hello following command:</span></span>
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   <span data-ttu-id="81fc9-159">Hello összeállítása a parancsfájl futtatása, ha mindenképpen toouse hello azonos Azure-fiók, előfizetés, valamint régió és hello megoldás létesített használt Active Directory-példányban.</span><span class="sxs-lookup"><span data-stu-id="81fc9-159">When hello build script runs, be sure toouse hello same Azure account, subscription, region, and Active Directory instance you used when you provisioned hello solution.</span></span>

## <a name="see-your-logic-app-in-action"></a><span data-ttu-id="81fc9-160">Tekintse meg a Logic Apps alkalmazást működés közben</span><span class="sxs-lookup"><span data-stu-id="81fc9-160">See your Logic App in action</span></span>
<span data-ttu-id="81fc9-161">távoli felügyeleti előkonfigurált megoldás hello beállítása alapértelmezés szerint, ha a megoldás két szabályokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="81fc9-161">hello remote monitoring preconfigured solution has two rules set up by default when you provision a solution.</span></span> <span data-ttu-id="81fc9-162">Szabályokat is vannak a hello **SampleDevice001** eszköz:</span><span class="sxs-lookup"><span data-stu-id="81fc9-162">Both rules are on hello **SampleDevice001** device:</span></span>

* <span data-ttu-id="81fc9-163">Hőmérséklet > 38.00</span><span class="sxs-lookup"><span data-stu-id="81fc9-163">Temperature > 38.00</span></span>
* <span data-ttu-id="81fc9-164">Páratartalom > 48.00</span><span class="sxs-lookup"><span data-stu-id="81fc9-164">Humidity > 48.00</span></span>

<span data-ttu-id="81fc9-165">hello hőmérséklet szabály indítja hello **előléptetése riasztás** művelet és hello páratartalom szabály indítja hello **SendMessage** művelet.</span><span class="sxs-lookup"><span data-stu-id="81fc9-165">hello temperature rule triggers hello **Raise Alarm** action and hello Humidity rule triggers hello **SendMessage** action.</span></span> <span data-ttu-id="81fc9-166">Feltéve, hogy a használt hello mindkét műveletek hello azonos URL-címe **ActionRepository** osztály, vagy a szabály a logic app eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="81fc9-166">Assuming you used hello same URL for both actions hello **ActionRepository** class, your logic app triggers for either rule.</span></span> <span data-ttu-id="81fc9-167">Szabályokat is használja a SendGrid toosend egy e-mailek toohello **való** cím hello riasztás részleteit.</span><span class="sxs-lookup"><span data-stu-id="81fc9-167">Both rules use SendGrid toosend an email toohello **To** address with details of hello alert.</span></span>

> [!NOTE]
> <span data-ttu-id="81fc9-168">hello logikai alkalmazás tootrigger továbbra is, minden alkalommal, amikor hello küszöbérték elérése.</span><span class="sxs-lookup"><span data-stu-id="81fc9-168">hello Logic App continues tootrigger every time hello threshold is met.</span></span> <span data-ttu-id="81fc9-169">tooavoid szükségtelen e-mailek, vagy letilthatja hello szabályok a megoldás portal vagy letilthatja az hello logikai alkalmazást a hello a [Azure-portálon][lnk-azureportal].</span><span class="sxs-lookup"><span data-stu-id="81fc9-169">tooavoid unnecessary emails, you can either disable hello rules in your solution portal or disable hello Logic App in hello [Azure portal][lnk-azureportal].</span></span>
> 
> 

<span data-ttu-id="81fc9-170">Ezenkívül tooreceiving e-mailt is látható hello portálon hello logikai alkalmazás futtatásakor:</span><span class="sxs-lookup"><span data-stu-id="81fc9-170">In addition tooreceiving emails, you can also see when hello Logic App runs in hello portal:</span></span>

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a><span data-ttu-id="81fc9-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="81fc9-171">Next steps</span></span>
<span data-ttu-id="81fc9-172">Most, hogy már használta a Logic App tooconnect előre konfigurált hello megoldás tooa üzleti folyamatokat, további kapcsolatos előre konfigurált hello megoldások testreszabásának hello beállításai:</span><span class="sxs-lookup"><span data-stu-id="81fc9-172">Now that you've used a Logic App tooconnect hello preconfigured solution tooa business process, you can learn more about hello options for customizing hello preconfigured solutions:</span></span>

* <span data-ttu-id="81fc9-173">[Dinamikus telemetriai adatok használata a távoli felügyeleti előkonfigurált megoldás hello][lnk-dynamic]</span><span class="sxs-lookup"><span data-stu-id="81fc9-173">[Use dynamic telemetry with hello remote monitoring preconfigured solution][lnk-dynamic]</span></span>
* <span data-ttu-id="81fc9-174">[Eszköz információk metaadatait a távoli felügyeleti előkonfigurált megoldás hello][lnk-devinfo]</span><span class="sxs-lookup"><span data-stu-id="81fc9-174">[Device information metadata in hello remote monitoring preconfigured solution][lnk-devinfo]</span></span>

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
