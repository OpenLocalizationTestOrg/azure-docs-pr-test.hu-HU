---
title: "Csatlakozás egy eszközt mbed C |} Microsoft Docs"
description: "Eszköz csatlakoztatása az Azure IoT Suite előre konfigurált távoli figyelési megoldást igényelnek a C mbed-eszközön fut-e olyan alkalmazással ismerteti."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 9551075e-dcf9-488f-943e-d0eb0e6260be
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: dobett
ROBOTS: NOINDEX
ms.openlocfilehash: ef7b78f85a787f8fbe22c0e26aa34f0cd1685d58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a><span data-ttu-id="7ecaf-103">Csatlakoztassa az eszközt a távoli felügyeleti előkonfigurált megoldás (mbed)</span><span class="sxs-lookup"><span data-stu-id="7ecaf-103">Connect your device to the remote monitoring preconfigured solution (mbed)</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="7ecaf-104">Forgatókönyv áttekintése</span><span class="sxs-lookup"><span data-stu-id="7ecaf-104">Scenario overview</span></span>
<span data-ttu-id="7ecaf-105">Ebben az esetben olyan eszközt hoz létre, amely a következő telemetriát küldi az [előre konfigurált][lnk-what-are-preconfig-solutions] távoli figyelési megoldásnak:</span><span class="sxs-lookup"><span data-stu-id="7ecaf-105">In this scenario, you create a device that sends the following telemetry to the remote monitoring [preconfigured solution][lnk-what-are-preconfig-solutions]:</span></span>

* <span data-ttu-id="7ecaf-106">Külső hőmérséklet</span><span class="sxs-lookup"><span data-stu-id="7ecaf-106">External temperature</span></span>
* <span data-ttu-id="7ecaf-107">Belső hőmérséklet</span><span class="sxs-lookup"><span data-stu-id="7ecaf-107">Internal temperature</span></span>
* <span data-ttu-id="7ecaf-108">Páratartalom</span><span class="sxs-lookup"><span data-stu-id="7ecaf-108">Humidity</span></span>

<span data-ttu-id="7ecaf-109">Az egyszerűség kedvéért az eszközön lévő kód mintaértékeket hoz létre, de javasoljuk, hogy terjessze ki a mintát valós érzékelők az eszközhöz csatlakoztatása és valós telemetria küldése révén.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-109">For simplicity, the code on the device generates sample values, but we encourage you to extend the sample by connecting real sensors to your device and sending real telemetry.</span></span>

<span data-ttu-id="7ecaf-110">Az eszköz a megoldás irányítópultjáról indított metódusokra és a megoldás irányítópultján beállított tulajdonságértékekre is tud válaszolni.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-110">The device is also able to respond to methods invoked from the solution dashboard and desired property values set in the solution dashboard.</span></span>

<span data-ttu-id="7ecaf-111">Az oktatóanyag elvégzéséhez egy aktív Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-111">To complete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="7ecaf-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="7ecaf-113">További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="7ecaf-113">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="7ecaf-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="7ecaf-114">Before you start</span></span>
<span data-ttu-id="7ecaf-115">Mielőtt bármilyen kódot írna az eszközhöz, ki kell építenie az előre konfigurált távoli figyelési megoldást, és meg kell adnia egy új egyéni eszközt ebben a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-115">Before you write any code for your device, you must provision your remote monitoring preconfigured solution and provision a new custom device in that solution.</span></span>

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="7ecaf-116">Az előre konfigurált távoli figyelési megoldás kiépítése</span><span class="sxs-lookup"><span data-stu-id="7ecaf-116">Provision your remote monitoring preconfigured solution</span></span>
<span data-ttu-id="7ecaf-117">Az ebben az oktatóanyagban létrehozott eszköz adatokat küld az előre konfigurált [távoli figyelési][lnk-remote-monitoring] megoldásnak.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-117">The device you create in this tutorial sends data to an instance of the [remote monitoring][lnk-remote-monitoring] preconfigured solution.</span></span> <span data-ttu-id="7ecaf-118">Ha még nem építette ki az előre konfigurált távoli figyelési megoldást az Azure-fiókban, használja a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7ecaf-118">If you haven't already provisioned the remote monitoring preconfigured solution in your Azure account, use the following steps:</span></span>

1. <span data-ttu-id="7ecaf-119">A <https://www.azureiotsuite.com/> oldalon kattintson a **+** elemre egy megoldás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-119">On the <https://www.azureiotsuite.com/> page, click **+** to create a solution.</span></span>
2. <span data-ttu-id="7ecaf-120">Kattintson a **Kiválasztás** elemre a **Távoli figyelés** panelen a megoldás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-120">Click **Select** on the **Remote monitoring** panel to create your solution.</span></span>
3. <span data-ttu-id="7ecaf-121">A **Távoli figyelési megoldás létrehozása** oldalon írjon be egy **megoldásnevet**, válassza ki a **Régiót**, ahová üzembe szeretné helyezni azt, majd válassza ki a használni kívánt Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-121">On the **Create Remote monitoring solution** page, enter a **Solution name** of your choice, select the **Region** you want to deploy to, and select the Azure subscription to want to use.</span></span> <span data-ttu-id="7ecaf-122">Ezután kattintson a **Megoldás létrehozása** parancsra.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-122">Then click **Create solution**.</span></span>
4. <span data-ttu-id="7ecaf-123">Várja meg, amíg befejeződik a kiépítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-123">Wait until the provisioning process completes.</span></span>

> [!WARNING]
> <span data-ttu-id="7ecaf-124">Az előre konfigurált megoldások számlázható Azure-szolgáltatásokat használnak.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-124">The preconfigured solutions use billable Azure services.</span></span> <span data-ttu-id="7ecaf-125">A felesleges költségek elkerülése érdekében ügyeljen arra, hogy eltávolítsa az előre konfigurált megoldást az előfizetésből, amikor végzett annak használatával.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-125">Be sure to remove the preconfigured solution from your subscription when you are done with it to avoid any unnecessary charges.</span></span> <span data-ttu-id="7ecaf-126">Teljesen is eltávolíthatja az előre konfigurált megoldást az előfizetésből a <https://www.azureiotsuite.com/> oldalon.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-126">You can completely remove a preconfigured solution from your subscription by visiting the <https://www.azureiotsuite.com/> page.</span></span>
> 
> 

<span data-ttu-id="7ecaf-127">A távoli figyelési megoldás kiépítésének befejezte után kattintson az **Indítás** gombra a megoldás irányítópultjának a böngészőben történő megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-127">When the provisioning process for the remote monitoring solution finishes, click **Launch** to open the solution dashboard in your browser.</span></span>

![A megoldás irányítópultja][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a><span data-ttu-id="7ecaf-129">Az eszköz kiépítése a távoli figyelési megoldásban</span><span class="sxs-lookup"><span data-stu-id="7ecaf-129">Provision your device in the remote monitoring solution</span></span>
> [!NOTE]
> <span data-ttu-id="7ecaf-130">Ha már kiépített egy eszközt a megoldásában, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-130">If you have already provisioned a device in your solution, you can skip this step.</span></span> <span data-ttu-id="7ecaf-131">Ismernie kell az eszköz hitelesítő adatait az ügyfélalkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-131">You need to know the device credentials when you create the client application.</span></span>
> 
> 

<span data-ttu-id="7ecaf-132">Ahhoz, hogy egy eszköz előre konfigurált megoldáshoz csatlakozhasson, érvényes hitelesítő adatokkal kell azonosítania magát az IoT Hubon.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-132">For a device to connect to the preconfigured solution, it must identify itself to IoT Hub using valid credentials.</span></span> <span data-ttu-id="7ecaf-133">A megoldás irányítópultjáról kérheti le az eszköz hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-133">You can retrieve the device credentials from the solution dashboard.</span></span> <span data-ttu-id="7ecaf-134">Az oktatóanyagban később megadja az eszköz hitelesítő adatait az ügyfélalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-134">You include the device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="7ecaf-135">Ha eszközt szeretne hozzáadni a távoli figyelési megoldáshoz, végezze el a következő lépéseket a megoldás irányítópultján:</span><span class="sxs-lookup"><span data-stu-id="7ecaf-135">To add a device to your remote monitoring solution, complete the following steps in the solution dashboard:</span></span>

1. <span data-ttu-id="7ecaf-136">Az irányítópult bal alsó részén kattintson az **Eszköz hozzáadása** parancsra.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-136">In the lower left-hand corner of the dashboard, click **Add a device**.</span></span>
   
   ![Eszköz hozzáadása][1]
2. <span data-ttu-id="7ecaf-138">Az **Egyéni eszköz** panelen kattintson az **Új hozzáadása** parancsra.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-138">In the **Custom Device** panel, click **Add new**.</span></span>
   
   ![Egyéni eszköz hozzáadása][2]
3. <span data-ttu-id="7ecaf-140">Válassza a **Meghatározom a saját eszközazonosítómat** elemet.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-140">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="7ecaf-141">Írjon be egy eszközazonosítót, például a **sajáteszköz** kifejezést, kattintson az **Azonosító ellenőrzése** parancsra, hogy meggyőződhessen arról, hogy a név még nincs használatban, majd kattintson a **Létrehozás** parancsra az eszköz kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-141">Enter a Device ID such as **mydevice**, click **Check ID** to verify that name isn't already in use, and then click **Create** to provision the device.</span></span>
   
   ![Eszközazonosító hozzáadása][3]
4. <span data-ttu-id="7ecaf-143">Jegyezze fel az eszköz hitelesítő adatait (az eszköz azonosítóját, az IoT Hub-eszköznevet és az eszköz kulcsát).</span><span class="sxs-lookup"><span data-stu-id="7ecaf-143">Make a note the device credentials (Device ID, IoT Hub Hostname, and Device Key).</span></span> <span data-ttu-id="7ecaf-144">Az ügyfélalkalmazásnak szüksége van ezekre az értékekre, hogy a távoli figyelési megoldáshoz csatlakozhasson.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-144">Your client application needs these values to connect to the remote monitoring solution.</span></span> <span data-ttu-id="7ecaf-145">Ezután kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-145">Then click **Done**.</span></span>
   
    ![Eszköz hitelesítő adatainak megtekintése][4]
5. <span data-ttu-id="7ecaf-147">Válassza ki az eszközt a megoldás irányítópultján lévő eszközlistából.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-147">Select your device in the device list in the solution dashboard.</span></span> <span data-ttu-id="7ecaf-148">Ezután az **Eszköz részletei** panelen kattintson az **Eszköz engedélyezése** parancsra.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-148">Then, in the **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="7ecaf-149">Az eszköz mostantól **Fut** állapotú.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-149">The status of your device is now **Running**.</span></span> <span data-ttu-id="7ecaf-150">A távoli figyelési megoldás ettől kezdve telemetriát fogadhat az eszközről és metódusokat hívhat meg az eszközön.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-150">The remote monitoring solution can now receive telemetry from your device and invoke methods on the device.</span></span>

[img-dashboard]: ./media/iot-suite-connecting-devices-mbed/dashboard.png
[1]: ./media/iot-suite-connecting-devices-mbed/suite0.png
[2]: ./media/iot-suite-connecting-devices-mbed/suite1.png
[3]: ./media/iot-suite-connecting-devices-mbed/suite2.png
[4]: ./media/iot-suite-connecting-devices-mbed/suite3.png

[lnk-what-are-preconfig-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

## <a name="build-and-run-the-c-sample-solution"></a><span data-ttu-id="7ecaf-151">Hozza létre, és futtassa a C megoldást</span><span class="sxs-lookup"><span data-stu-id="7ecaf-151">Build and run the C sample solution</span></span>

<span data-ttu-id="7ecaf-152">A következő útmutatások mutatják be hoz való csatlakoztatásának lépéseit egy [mbed-kompatibilis Freescale FRDM-K64F] [ lnk-mbed-home] eszközt, hogy a távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-152">The following instructions describe the steps for connecting an [mbed-enabled Freescale FRDM-K64F][lnk-mbed-home] device to the remote monitoring solution.</span></span>

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a><span data-ttu-id="7ecaf-153">Csatlakoztassa a mbed a hálózat- és asztali számítógépen</span><span class="sxs-lookup"><span data-stu-id="7ecaf-153">Connect the mbed device to your network and desktop machine</span></span>

1. <span data-ttu-id="7ecaf-154">A hálózati Ethernet kábellel csatlakoztassa az mbed eszközt.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-154">Connect the mbed device to your network using an Ethernet cable.</span></span> <span data-ttu-id="7ecaf-155">Ez a lépés szükség, mert a mintaalkalmazás internet-hozzáférésre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-155">This step is necessary because the sample application requires internet access.</span></span>

1. <span data-ttu-id="7ecaf-156">Lásd: [Ismerkedés a mbed] [ lnk-mbed-getstarted] az mbed eszköz csatlakozni az asztali számítógép.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-156">See [Getting Started with mbed][lnk-mbed-getstarted] to connect your mbed device to your desktop PC.</span></span>

1. <span data-ttu-id="7ecaf-157">Ha az asztali számítógép Windows fut, lásd: [PC-konfigurációt] [ lnk-mbed-pcconnect] konfigurálja az mbed eszköz soros port elérését.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-157">If your desktop PC is running Windows, see [PC Configuration][lnk-mbed-pcconnect] to configure serial port access to your mbed device.</span></span>

### <a name="create-an-mbed-project-and-import-the-sample-code"></a><span data-ttu-id="7ecaf-158">Mbed projekt létrehozása és importálása a mintakód</span><span class="sxs-lookup"><span data-stu-id="7ecaf-158">Create an mbed project and import the sample code</span></span>

<span data-ttu-id="7ecaf-159">Kövesse az alábbi lépéseket néhány mintakód hozzáadása mbed projektben.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-159">Follow these steps to add some sample code to an mbed project.</span></span> <span data-ttu-id="7ecaf-160">A távoli felügyeleti alapszintű projekt importálása, és módosítsa a projektet a MQTT protokoll használata helyett az AMQP protokoll.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-160">You import the remote monitoring starter project and then change the project to use the MQTT protocol instead of the AMQP protocol.</span></span> <span data-ttu-id="7ecaf-161">Jelenleg kell használnia a MQTT protokoll az IoT-központ az eszköz felügyeleti funkcióinak használatát.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-161">Currently, you need to use the MQTT protocol to use the device management features of IoT Hub.</span></span>

1. <span data-ttu-id="7ecaf-162">A böngészőben nyissa meg a mbed.org [fejlesztői hely](https://developer.mbed.org/).</span><span class="sxs-lookup"><span data-stu-id="7ecaf-162">In your web browser, go to the mbed.org [developer site](https://developer.mbed.org/).</span></span> <span data-ttu-id="7ecaf-163">Még nem regisztrált, megjelenik egy lehetőség, hogy hozzon létre egy fiókot (szabad).</span><span class="sxs-lookup"><span data-stu-id="7ecaf-163">If you haven't signed up, you see an option to create an account (it's free).</span></span> <span data-ttu-id="7ecaf-164">Ellenkező esetben jelentkezzen be a fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-164">Otherwise, log in with your account credentials.</span></span> <span data-ttu-id="7ecaf-165">Kattintson a **fordító** az oldal jobb felső sarkában található.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-165">Then click **Compiler** in the upper right-hand corner of the page.</span></span> <span data-ttu-id="7ecaf-166">Ez a művelet számos lehetőséget kínál, a *munkaterület* felületet.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-166">This action brings you to the *Workspace* interface.</span></span>

1. <span data-ttu-id="7ecaf-167">Ellenőrizze, hogy a hardver platform használata az ablak jobb felső sarkában jelenik meg, vagy kattintson a jobb oldali sarokban válassza ki a hardver platformot ikonra.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-167">Make sure the hardware platform you're using appears in the upper right-hand corner of the window, or click the icon in the right-hand corner to select your hardware platform.</span></span>

1. <span data-ttu-id="7ecaf-168">Kattintson a **importálási** a főmenü.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-168">Click **Import** on the main menu.</span></span> <span data-ttu-id="7ecaf-169">Kattintson a **kattintson ide az URL-cím importálásához**.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-169">Then click **Click here to import from URL**.</span></span>
   
    ![Importálás mbed munkaterületet indítása][6]

1. <span data-ttu-id="7ecaf-171">Az előugró ablakban adja meg a hivatkozást a minta kód https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/, majd kattintson az **importálási**.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-171">In the pop-up window, enter the link for the sample code https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ then click **Import**.</span></span>
   
    ![Importálás mintakódot mbed munkaterület][7]

1. <span data-ttu-id="7ecaf-173">A fordítóprogram mbed ablakban látható, hogy a projekt importálása is importálja a szalagtárak különböző.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-173">You can see in the mbed compiler window that importing this project also imports various libraries.</span></span> <span data-ttu-id="7ecaf-174">Néhány biztosított és az Azure IoT-csapat által fenntartott ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), a mbed szalagtárak katalógus elérhető külső szalagtárak vannak.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-174">Some are provided and maintained by the Azure IoT team ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), while others are third-party libraries available in the mbed libraries catalog.</span></span>
   
    ![Mbed projekt megtekintése][8]

1. <span data-ttu-id="7ecaf-176">Az a **Program munkaterület**, kattintson a jobb gombbal a **IOT hubbal\_amqp\_átviteli** könyvtár, kattintson a **törlése**, és kattintson a  **OK** megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-176">In the **Program Workspace**, right-click the **iothub\_amqp\_transport** library, click **Delete**, and then click **OK** to confirm.</span></span>

1. <span data-ttu-id="7ecaf-177">Az a **Program munkaterület**, kattintson a jobb gombbal a **azure\_amqp\_c** könyvtár, kattintson a **törlése**, és kattintson a **OK** megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-177">In the **Program Workspace**, right-click the **azure\_amqp\_c** library, click **Delete**, and then click **OK** to confirm.</span></span>

1. <span data-ttu-id="7ecaf-178">Kattintson a jobb gombbal a **remote_monitoring** a projektre a **Program munkaterület**, jelölje be **importálási könyvtár**, majd jelölje be **az URL**.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-178">Right-click the **remote_monitoring** project in the **Program Workspace**, select **Import Library**, then select **From URL**.</span></span>
   
    ![Indítsa el a mbed munkaterületet Típustár importálása][6]

1. <span data-ttu-id="7ecaf-180">Az előugró ablakban adja meg a hivatkozást a MQTT átviteli könyvtár https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_átviteli / kattintson **importálási**.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-180">In the pop-up window, enter the link for the MQTT transport library https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_transport/ then click **Import**.</span></span>
   
    ![Importálás szalagtár mbed munkaterület][12]

1. <span data-ttu-id="7ecaf-182">Ismételje az előző lépést a MQTT könyvtár hozzáadása a https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c /.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-182">Repeat the previous step to add the MQTT library from https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c/.</span></span>

1. <span data-ttu-id="7ecaf-183">A munkaterület most a következőképpen néznek:</span><span class="sxs-lookup"><span data-stu-id="7ecaf-183">Your workspace now looks like the following:</span></span>

    ![Mbed munkaterület megtekintése][13]

1. <span data-ttu-id="7ecaf-185">Nyissa meg a távoli\_monitoring\remote_monitoring.c fájlt, és cserélje le a meglévő `#include` utasítások a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="7ecaf-185">Open the remote\_monitoring\remote_monitoring.c file and replace the existing `#include` statements with the following code:</span></span>

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"

    #ifdef MBED_BUILD_TIMESTAMP
    #include "certs.h"
    #endif // MBED_BUILD_TIMESTAMP
    ```
1. <span data-ttu-id="7ecaf-186">Törölje a fennmaradó kódot a távoli\_monitoring\remote\_monitoring.c fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-186">Delete all the remaining code in the remote\_monitoring\remote\_monitoring.c file.</span></span>

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a><span data-ttu-id="7ecaf-187">Hozza létre, és futtathatja a</span><span class="sxs-lookup"><span data-stu-id="7ecaf-187">Build and run the sample</span></span>

<span data-ttu-id="7ecaf-188">Adja hozzá a meghívni kívánt kódot a **távoli\_figyelési\_futtatása** működéséhez majd összeállítása, és futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-188">Add code to invoke the **remote\_monitoring\_run** function and then build and run the device application.</span></span>

1. <span data-ttu-id="7ecaf-189">Adja hozzá a **fő** függvényt a következő kódot a távoli végén\_monitoring.c fájl meghívni a **távoli\_figyelési\_futtassa** függvény:</span><span class="sxs-lookup"><span data-stu-id="7ecaf-189">Add a **main** function with following code at the end of the remote\_monitoring.c file to invoke the **remote\_monitoring\_run** function:</span></span>
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. <span data-ttu-id="7ecaf-190">Kattintson a **fordítási** hozhat létre a program.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-190">Click **Compile** to build the program.</span></span> <span data-ttu-id="7ecaf-191">Nyugodtan figyelmen kívül hagyhatja a figyelmeztetéseket, de ha a build hoz létre a hibát, javítsa ki a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-191">You can safely ignore any warnings, but if the build generates errors, fix them before proceeding.</span></span>

1. <span data-ttu-id="7ecaf-192">Ha a build sikeres, a mbed fordító webhely állít elő, a projekt neve .bin fájlt, és letölti a helyi számítógépre.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-192">If the build is successful, the mbed compiler website generates a .bin file with the name of your project and downloads it to your local machine.</span></span> <span data-ttu-id="7ecaf-193">Másolja a .bin fájlt az eszközre.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-193">Copy the .bin file to the device.</span></span> <span data-ttu-id="7ecaf-194">Az eszköz újraindításához, és futtassa a programot a .bin fájl tartalmazza a .bin fájl mentése az eszköz okozza.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-194">Saving the .bin file to the device causes the device to restart and run the program contained in the .bin file.</span></span> <span data-ttu-id="7ecaf-195">Később manuálisan is újraindíthatja a program bármikor az mbed eszközön a alaphelyzetbe állítása gomb lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-195">You can manually restart the program at any time by pressing the reset button on the mbed device.</span></span>

1. <span data-ttu-id="7ecaf-196">Az eszköz egy SSH ügyfélalkalmazást, mint a PuTTY használatával kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-196">Connect to the device using an SSH client application, such as PuTTY.</span></span> <span data-ttu-id="7ecaf-197">Azt is meghatározhatja a soros port, az eszköz használ a Windows ellenőrzésével.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-197">You can determine the serial port your device uses by checking Windows Device Manager.</span></span>
   
    ![][11]

1. <span data-ttu-id="7ecaf-198">A PuTTY, kattintson a **soros** kapcsolattípus.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-198">In PuTTY, click the **Serial** connection type.</span></span> <span data-ttu-id="7ecaf-199">Az eszköz általában 9600 átviteli csatlakozik, ezért adja meg a 9600 a **sebesség** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-199">The device typically connects at 9600 baud, so enter 9600 in the **Speed** box.</span></span> <span data-ttu-id="7ecaf-200">Kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-200">Then click **Open**.</span></span>

1. <span data-ttu-id="7ecaf-201">A program végrehajtásának elkezdése.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-201">The program starts executing.</span></span> <span data-ttu-id="7ecaf-202">Lehet, hogy a Alaphelyzet (nyomja meg a CTRL + Break vagy a tábla alaphelyzetbe állítása gomb megnyomása) Ha a program nem indul el automatikusan csatlakoztatásakor.</span><span class="sxs-lookup"><span data-stu-id="7ecaf-202">You may have to reset the board (press CTRL+Break or press the board's reset button) if the program does not start automatically when you connect.</span></span>
   
    ![][10]

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png
[12]: ./media/iot-suite-connecting-devices-mbed/mbed7.png
[13]: ./media/iot-suite-connecting-devices-mbed/mbed8.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
