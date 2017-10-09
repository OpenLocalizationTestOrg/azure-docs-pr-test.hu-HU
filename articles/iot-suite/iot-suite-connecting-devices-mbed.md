---
title: "egy eszköz használatával C mbed aaaConnect |} Microsoft Docs"
description: "Ismerteti, hogyan tooconnect egy eszköz toohello Azure IoT Suite előre konfigurált távoli figyelési megoldást igényelnek a C egy mbed eszközön futó alkalmazást használ."
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
ms.openlocfilehash: dcd1e74635e8dec678a59bff060a73f7cfabd124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-mbed"></a><span data-ttu-id="db70b-103">Csatlakozzon a távoli felügyeleti előkonfigurált megoldás (mbed) eszköz toohello</span><span class="sxs-lookup"><span data-stu-id="db70b-103">Connect your device toohello remote monitoring preconfigured solution (mbed)</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="db70b-104">Forgatókönyv áttekintése</span><span class="sxs-lookup"><span data-stu-id="db70b-104">Scenario overview</span></span>
<span data-ttu-id="db70b-105">Ebben a forgatókönyvben a következő telemetriai toohello távoli megfigyelési hello küldő eszköz létrehozása [előre konfigurált megoldás][lnk-what-are-preconfig-solutions]:</span><span class="sxs-lookup"><span data-stu-id="db70b-105">In this scenario, you create a device that sends hello following telemetry toohello remote monitoring [preconfigured solution][lnk-what-are-preconfig-solutions]:</span></span>

* <span data-ttu-id="db70b-106">Külső hőmérséklet</span><span class="sxs-lookup"><span data-stu-id="db70b-106">External temperature</span></span>
* <span data-ttu-id="db70b-107">Belső hőmérséklet</span><span class="sxs-lookup"><span data-stu-id="db70b-107">Internal temperature</span></span>
* <span data-ttu-id="db70b-108">Páratartalom</span><span class="sxs-lookup"><span data-stu-id="db70b-108">Humidity</span></span>

<span data-ttu-id="db70b-109">Az egyszerűség kedvéért hello kód hello eszközön mintaértékek állít elő, de javasoljuk, tooextend hello minta valós érzékelők tooyour eszköz csatlakoztatása és valós telemetriai adatok küldését.</span><span class="sxs-lookup"><span data-stu-id="db70b-109">For simplicity, hello code on hello device generates sample values, but we encourage you tooextend hello sample by connecting real sensors tooyour device and sending real telemetry.</span></span>

<span data-ttu-id="db70b-110">hello eszköz is, képes toorespond toomethods hello megoldás irányítópultja meghívni, és tulajdonságértékeket hello megoldás irányítópultjának beállítása szükséges.</span><span class="sxs-lookup"><span data-stu-id="db70b-110">hello device is also able toorespond toomethods invoked from hello solution dashboard and desired property values set in hello solution dashboard.</span></span>

<span data-ttu-id="db70b-111">toocomplete ebben az oktatóanyagban egy aktív Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="db70b-111">toocomplete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="db70b-112">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="db70b-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="db70b-113">További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="db70b-113">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="db70b-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="db70b-114">Before you start</span></span>
<span data-ttu-id="db70b-115">Mielőtt bármilyen kódot írna az eszközhöz, ki kell építenie az előre konfigurált távoli figyelési megoldást, és meg kell adnia egy új egyéni eszközt ebben a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="db70b-115">Before you write any code for your device, you must provision your remote monitoring preconfigured solution and provision a new custom device in that solution.</span></span>

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="db70b-116">Az előre konfigurált távoli figyelési megoldás kiépítése</span><span class="sxs-lookup"><span data-stu-id="db70b-116">Provision your remote monitoring preconfigured solution</span></span>
<span data-ttu-id="db70b-117">Ebben az oktatóanyagban létrehozhat hello eszköz küldi hello adatok tooan példányának [távoli megfigyelési] [ lnk-remote-monitoring] előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="db70b-117">hello device you create in this tutorial sends data tooan instance of hello [remote monitoring][lnk-remote-monitoring] preconfigured solution.</span></span> <span data-ttu-id="db70b-118">Ha még nem már kiépített hello távoli felügyeleti előkonfigurált megoldás az Azure-fiókjába, használja a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="db70b-118">If you haven't already provisioned hello remote monitoring preconfigured solution in your Azure account, use hello following steps:</span></span>

1. <span data-ttu-id="db70b-119">A hello <https://www.azureiotsuite.com/> kattintson  **+**  toocreate megoldást.</span><span class="sxs-lookup"><span data-stu-id="db70b-119">On hello <https://www.azureiotsuite.com/> page, click **+** toocreate a solution.</span></span>
2. <span data-ttu-id="db70b-120">Kattintson a **válasszon** a hello **távoli megfigyelési** panel toocreate a megoldás.</span><span class="sxs-lookup"><span data-stu-id="db70b-120">Click **Select** on hello **Remote monitoring** panel toocreate your solution.</span></span>
3. <span data-ttu-id="db70b-121">A hello **létrehozása távoli felügyeleti megoldás** lapján adjon meg egy **megoldásnév** az Ön által választott, válassza ki a hello **régió** szeretné, hogy toodeploy, és válassza ki a hello Azure előfizetés toowant toouse.</span><span class="sxs-lookup"><span data-stu-id="db70b-121">On hello **Create Remote monitoring solution** page, enter a **Solution name** of your choice, select hello **Region** you want toodeploy to, and select hello Azure subscription toowant toouse.</span></span> <span data-ttu-id="db70b-122">Ezután kattintson a **Megoldás létrehozása** parancsra.</span><span class="sxs-lookup"><span data-stu-id="db70b-122">Then click **Create solution**.</span></span>
4. <span data-ttu-id="db70b-123">Várjon, amíg a kiépítési folyamat hello befejeződik.</span><span class="sxs-lookup"><span data-stu-id="db70b-123">Wait until hello provisioning process completes.</span></span>

> [!WARNING]
> <span data-ttu-id="db70b-124">előre konfigurált hello megoldások számlázható Azure-szolgáltatásokat használja.</span><span class="sxs-lookup"><span data-stu-id="db70b-124">hello preconfigured solutions use billable Azure services.</span></span> <span data-ttu-id="db70b-125">Ne feledje tooremove hello előkonfigurált megoldás az előfizetésből, amikor elkészült, azt tooavoid a szükségtelen díjakat.</span><span class="sxs-lookup"><span data-stu-id="db70b-125">Be sure tooremove hello preconfigured solution from your subscription when you are done with it tooavoid any unnecessary charges.</span></span> <span data-ttu-id="db70b-126">Teljesen eltávolíthatja a előkonfigurált megoldás az előfizetésből hello felkeresésével <https://www.azureiotsuite.com/> lap.</span><span class="sxs-lookup"><span data-stu-id="db70b-126">You can completely remove a preconfigured solution from your subscription by visiting hello <https://www.azureiotsuite.com/> page.</span></span>
> 
> 

<span data-ttu-id="db70b-127">Ha létesítésének folyamatát kell használnia a távoli felügyeleti megoldás hello hello befejeződik, kattintson a **indítása** tooopen hello megoldás irányítópultja a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="db70b-127">When hello provisioning process for hello remote monitoring solution finishes, click **Launch** tooopen hello solution dashboard in your browser.</span></span>

![A megoldás irányítópultja][img-dashboard]

### <a name="provision-your-device-in-hello-remote-monitoring-solution"></a><span data-ttu-id="db70b-129">Az eszköz a távoli felügyeleti megoldás hello kiépítése</span><span class="sxs-lookup"><span data-stu-id="db70b-129">Provision your device in hello remote monitoring solution</span></span>
> [!NOTE]
> <span data-ttu-id="db70b-130">Ha már kiépített egy eszközt a megoldásában, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="db70b-130">If you have already provisioned a device in your solution, you can skip this step.</span></span> <span data-ttu-id="db70b-131">Hello ügyfélalkalmazás létrehozásakor tooknow hello eszköz hitelesítő adatok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="db70b-131">You need tooknow hello device credentials when you create hello client application.</span></span>
> 
> 

<span data-ttu-id="db70b-132">Eszköz tooconnect előre konfigurált toohello megoldás, azt kell azonosítani magát tooIoT Hub érvényes hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="db70b-132">For a device tooconnect toohello preconfigured solution, it must identify itself tooIoT Hub using valid credentials.</span></span> <span data-ttu-id="db70b-133">Hello megoldás irányítópultja hello eszközhitelesítő adatok kérhetnek le.</span><span class="sxs-lookup"><span data-stu-id="db70b-133">You can retrieve hello device credentials from hello solution dashboard.</span></span> <span data-ttu-id="db70b-134">Hello eszközhitelesítő adatok szerepeljenek az ügyfélalkalmazás az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="db70b-134">You include hello device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="db70b-135">tooadd eszköz tooyour távoli figyelési megoldás, a következő teljes hello hello megoldás irányítópultja lépései:</span><span class="sxs-lookup"><span data-stu-id="db70b-135">tooadd a device tooyour remote monitoring solution, complete hello following steps in hello solution dashboard:</span></span>

1. <span data-ttu-id="db70b-136">Hello bal alsó sarkában hello irányítópultot, kattintson **hozzáad egy eszközt**.</span><span class="sxs-lookup"><span data-stu-id="db70b-136">In hello lower left-hand corner of hello dashboard, click **Add a device**.</span></span>
   
   ![Eszköz hozzáadása][1]
2. <span data-ttu-id="db70b-138">A hello **egyéni eszköz** panelen, kattintson a **új hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="db70b-138">In hello **Custom Device** panel, click **Add new**.</span></span>
   
   ![Egyéni eszköz hozzáadása][2]
3. <span data-ttu-id="db70b-140">Válassza a **Meghatározom a saját eszközazonosítómat** elemet.</span><span class="sxs-lookup"><span data-stu-id="db70b-140">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="db70b-141">Adja meg például az Eszközazonosítót **mydevice**, kattintson a **ellenőrizze azonosító** tooverify ugyanez a neve már nincs használatban, és kattintson a **létrehozása** tooprovision hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="db70b-141">Enter a Device ID such as **mydevice**, click **Check ID** tooverify that name isn't already in use, and then click **Create** tooprovision hello device.</span></span>
   
   ![Eszközazonosító hozzáadása][3]
4. <span data-ttu-id="db70b-143">Megjegyzés: hello eszköz tétele (Eszközazonosító IoT Hub állomásnév és eszközkulcs) hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="db70b-143">Make a note hello device credentials (Device ID, IoT Hub Hostname, and Device Key).</span></span> <span data-ttu-id="db70b-144">Az ügyfélalkalmazás kell ezen értékek tooconnect toohello távoli felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="db70b-144">Your client application needs these values tooconnect toohello remote monitoring solution.</span></span> <span data-ttu-id="db70b-145">Ezután kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="db70b-145">Then click **Done**.</span></span>
   
    ![Eszköz hitelesítő adatainak megtekintése][4]
5. <span data-ttu-id="db70b-147">Jelölje ki az eszköz hello megoldás irányítópultjának hello eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="db70b-147">Select your device in hello device list in hello solution dashboard.</span></span> <span data-ttu-id="db70b-148">Ezt követően a hello **eszközadatok** panelen, kattintson a **eszköz engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="db70b-148">Then, in hello **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="db70b-149">hello az eszköz állapota ezután **futtató**.</span><span class="sxs-lookup"><span data-stu-id="db70b-149">hello status of your device is now **Running**.</span></span> <span data-ttu-id="db70b-150">Mostantól telemetriai adatok fogadhatók az eszközről és metódusok hello eszközön hello távoli figyelési megoldást igényelnek.</span><span class="sxs-lookup"><span data-stu-id="db70b-150">hello remote monitoring solution can now receive telemetry from your device and invoke methods on hello device.</span></span>

[img-dashboard]: ./media/iot-suite-connecting-devices-mbed/dashboard.png
[1]: ./media/iot-suite-connecting-devices-mbed/suite0.png
[2]: ./media/iot-suite-connecting-devices-mbed/suite1.png
[3]: ./media/iot-suite-connecting-devices-mbed/suite2.png
[4]: ./media/iot-suite-connecting-devices-mbed/suite3.png

[lnk-what-are-preconfig-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

## <a name="build-and-run-hello-c-sample-solution"></a><span data-ttu-id="db70b-151">Hozza létre, és futtassa a hello C megoldást</span><span class="sxs-lookup"><span data-stu-id="db70b-151">Build and run hello C sample solution</span></span>

<span data-ttu-id="db70b-152">hello következő útmutatások mutatják be hello hoz való csatlakoztatásának lépéseit egy [mbed-kompatibilis Freescale FRDM-K64F] [ lnk-mbed-home] eszköz toohello távoli felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="db70b-152">hello following instructions describe hello steps for connecting an [mbed-enabled Freescale FRDM-K64F][lnk-mbed-home] device toohello remote monitoring solution.</span></span>

### <a name="connect-hello-mbed-device-tooyour-network-and-desktop-machine"></a><span data-ttu-id="db70b-153">Csatlakoztassa hello mbed eszköz tooyour hálózati és az asztali számítógép</span><span class="sxs-lookup"><span data-stu-id="db70b-153">Connect hello mbed device tooyour network and desktop machine</span></span>

1. <span data-ttu-id="db70b-154">Hello mbed eszköz tooyour hálózati Ethernet kábellel csatlakoztassa.</span><span class="sxs-lookup"><span data-stu-id="db70b-154">Connect hello mbed device tooyour network using an Ethernet cable.</span></span> <span data-ttu-id="db70b-155">Ez a lépés szükség, mert hello mintaalkalmazás internet-hozzáférésre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="db70b-155">This step is necessary because hello sample application requires internet access.</span></span>

1. <span data-ttu-id="db70b-156">Lásd: [Ismerkedés a mbed] [ lnk-mbed-getstarted] tooconnect mbed eszköz tooyour asztali PC.</span><span class="sxs-lookup"><span data-stu-id="db70b-156">See [Getting Started with mbed][lnk-mbed-getstarted] tooconnect your mbed device tooyour desktop PC.</span></span>

1. <span data-ttu-id="db70b-157">Ha az asztali számítógép Windows fut, lásd: [PC-konfigurációt] [ lnk-mbed-pcconnect] tooconfigure soros port hozzáférés tooyour mbed eszköz.</span><span class="sxs-lookup"><span data-stu-id="db70b-157">If your desktop PC is running Windows, see [PC Configuration][lnk-mbed-pcconnect] tooconfigure serial port access tooyour mbed device.</span></span>

### <a name="create-an-mbed-project-and-import-hello-sample-code"></a><span data-ttu-id="db70b-158">Mbed projekt létrehozása és importálása hello mintakód</span><span class="sxs-lookup"><span data-stu-id="db70b-158">Create an mbed project and import hello sample code</span></span>

<span data-ttu-id="db70b-159">Kövesse ezeket a lépéseket tooadd néhány kódot tooan mbed mintaprojektet.</span><span class="sxs-lookup"><span data-stu-id="db70b-159">Follow these steps tooadd some sample code tooan mbed project.</span></span> <span data-ttu-id="db70b-160">Hello távoli figyelési alapszintű projekt importálása, és módosítsa a hello projekt toouse hello MQTT protokoll hello AMQP protokoll helyett.</span><span class="sxs-lookup"><span data-stu-id="db70b-160">You import hello remote monitoring starter project and then change hello project toouse hello MQTT protocol instead of hello AMQP protocol.</span></span> <span data-ttu-id="db70b-161">Jelenleg toouse hello MQTT protokoll toouse hello eszköz felügyeleti funkciókat az IoT-központ van szüksége.</span><span class="sxs-lookup"><span data-stu-id="db70b-161">Currently, you need toouse hello MQTT protocol toouse hello device management features of IoT Hub.</span></span>

1. <span data-ttu-id="db70b-162">A böngészőben nyissa meg toohello mbed.org [fejlesztői hely](https://developer.mbed.org/).</span><span class="sxs-lookup"><span data-stu-id="db70b-162">In your web browser, go toohello mbed.org [developer site](https://developer.mbed.org/).</span></span> <span data-ttu-id="db70b-163">Ha még nem regisztrált, megjelenik egy beállítás toocreate (szabad) fiók.</span><span class="sxs-lookup"><span data-stu-id="db70b-163">If you haven't signed up, you see an option toocreate an account (it's free).</span></span> <span data-ttu-id="db70b-164">Ellenkező esetben jelentkezzen be a fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="db70b-164">Otherwise, log in with your account credentials.</span></span> <span data-ttu-id="db70b-165">Kattintson a **fordító** hello jobb felső sarokban hello lap.</span><span class="sxs-lookup"><span data-stu-id="db70b-165">Then click **Compiler** in hello upper right-hand corner of hello page.</span></span> <span data-ttu-id="db70b-166">Ez a művelet hozása toohello *munkaterület* felületet.</span><span class="sxs-lookup"><span data-stu-id="db70b-166">This action brings you toohello *Workspace* interface.</span></span>

1. <span data-ttu-id="db70b-167">Győződjön meg arról, hello hardver platform használata hello jobb felső sarkában hello ablak jelenik meg, vagy kattintson a jobb sarkában tooselect hello hello ikonra a hardver platform.</span><span class="sxs-lookup"><span data-stu-id="db70b-167">Make sure hello hardware platform you're using appears in hello upper right-hand corner of hello window, or click hello icon in hello right-hand corner tooselect your hardware platform.</span></span>

1. <span data-ttu-id="db70b-168">Kattintson a **importálási** hello főmenü.</span><span class="sxs-lookup"><span data-stu-id="db70b-168">Click **Import** on hello main menu.</span></span> <span data-ttu-id="db70b-169">Kattintson a **kattintson ide a URL-CÍMRŐL tooimport**.</span><span class="sxs-lookup"><span data-stu-id="db70b-169">Then click **Click here tooimport from URL**.</span></span>
   
    ![Indítsa el az importálási toombed munkaterület][6]

1. <span data-ttu-id="db70b-171">A hello előugró ablak, írja be hello hivatkozás hello minta kód https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/, majd kattintson az **importálási**.</span><span class="sxs-lookup"><span data-stu-id="db70b-171">In hello pop-up window, enter hello link for hello sample code https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ then click **Import**.</span></span>
   
    ![A minta kód toombed munkaterület importálása][7]

1. <span data-ttu-id="db70b-173">Hello mbed fordító ablakban látható, hogy a projekt importálása is importálja a szalagtárak különböző.</span><span class="sxs-lookup"><span data-stu-id="db70b-173">You can see in hello mbed compiler window that importing this project also imports various libraries.</span></span> <span data-ttu-id="db70b-174">Néhány megadott és hello Azure IoT csapat által fenntartott ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), a külső szalagtárak hello mbed szalagtárak katalógus elérhető vannak.</span><span class="sxs-lookup"><span data-stu-id="db70b-174">Some are provided and maintained by hello Azure IoT team ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), while others are third-party libraries available in hello mbed libraries catalog.</span></span>
   
    ![Mbed projekt megtekintése][8]

1. <span data-ttu-id="db70b-176">A hello **Program munkaterület**, kattintson a jobb gombbal hello **IOT hubbal\_amqp\_átviteli** könyvtár, kattintson **törlése**, és kattintson a **OK** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="db70b-176">In hello **Program Workspace**, right-click hello **iothub\_amqp\_transport** library, click **Delete**, and then click **OK** tooconfirm.</span></span>

1. <span data-ttu-id="db70b-177">A hello **Program munkaterület**, kattintson a jobb gombbal hello **azure\_amqp\_c** könyvtár, kattintson a **törlése**, és kattintson a **OK**  tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="db70b-177">In hello **Program Workspace**, right-click hello **azure\_amqp\_c** library, click **Delete**, and then click **OK** tooconfirm.</span></span>

1. <span data-ttu-id="db70b-178">Kattintson a jobb gombbal hello **remote_monitoring** projektre a hello **Program munkaterület**, jelölje be **importálási könyvtár**, majd jelölje be **URL-ből**.</span><span class="sxs-lookup"><span data-stu-id="db70b-178">Right-click hello **remote_monitoring** project in hello **Program Workspace**, select **Import Library**, then select **From URL**.</span></span>
   
    ![Indítsa el a könyvtár importálási toombed munkaterület][6]

1. <span data-ttu-id="db70b-180">A hello előugró ablak, írja be hello hivatkozás hello MQTT átviteli könyvtár https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_átviteli / kattintson **importálási**.</span><span class="sxs-lookup"><span data-stu-id="db70b-180">In hello pop-up window, enter hello link for hello MQTT transport library https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_transport/ then click **Import**.</span></span>
   
    ![Könyvtár toombed munkaterület importálása][12]

1. <span data-ttu-id="db70b-182">Ismétlődő hello előző lépés tooadd hello MQTT kódtárat https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c /.</span><span class="sxs-lookup"><span data-stu-id="db70b-182">Repeat hello previous step tooadd hello MQTT library from https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c/.</span></span>

1. <span data-ttu-id="db70b-183">A munkaterület most következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="db70b-183">Your workspace now looks like hello following:</span></span>

    ![Mbed munkaterület megtekintése][13]

1. <span data-ttu-id="db70b-185">Nyissa meg hello távoli\_monitoring\remote_monitoring.c fájl- és a név felülírandó hello meglévő `#include` utasítások a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="db70b-185">Open hello remote\_monitoring\remote_monitoring.c file and replace hello existing `#include` statements with hello following code:</span></span>

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
1. <span data-ttu-id="db70b-186">Távoli hello kód fennmaradó összes hello törlése\_monitoring\remote\_monitoring.c fájlt.</span><span class="sxs-lookup"><span data-stu-id="db70b-186">Delete all hello remaining code in hello remote\_monitoring\remote\_monitoring.c file.</span></span>

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-hello-sample"></a><span data-ttu-id="db70b-187">Hozza létre és futtasson hello mintát</span><span class="sxs-lookup"><span data-stu-id="db70b-187">Build and run hello sample</span></span>

<span data-ttu-id="db70b-188">Adja hozzá a kódot tooinvoke hello **távoli\_figyelési\_futtatása** funkciót, majd létre és hello eszköz alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="db70b-188">Add code tooinvoke hello **remote\_monitoring\_run** function and then build and run hello device application.</span></span>

1. <span data-ttu-id="db70b-189">Adja hozzá a **fő** függvény a következő kódot a távoli hello hello végén\_monitoring.c fájl tooinvoke hello **távoli\_figyelési\_futtatása** függvény:</span><span class="sxs-lookup"><span data-stu-id="db70b-189">Add a **main** function with following code at hello end of hello remote\_monitoring.c file tooinvoke hello **remote\_monitoring\_run** function:</span></span>
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. <span data-ttu-id="db70b-190">Kattintson a **fordítási** toobuild hello program.</span><span class="sxs-lookup"><span data-stu-id="db70b-190">Click **Compile** toobuild hello program.</span></span> <span data-ttu-id="db70b-191">Nyugodtan figyelmen kívül hagyhatja a figyelmeztetéseket, de ha hello összeállítási hibát, javítsa ki a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="db70b-191">You can safely ignore any warnings, but if hello build generates errors, fix them before proceeding.</span></span>

1. <span data-ttu-id="db70b-192">Sikeres hello build esetén hello mbed fordító webhelyet hoz létre a projekt hello nevű .bin fájl, és letölti tooyour helyi számítógép.</span><span class="sxs-lookup"><span data-stu-id="db70b-192">If hello build is successful, hello mbed compiler website generates a .bin file with hello name of your project and downloads it tooyour local machine.</span></span> <span data-ttu-id="db70b-193">Másolás hello .bin fájl toohello eszköz.</span><span class="sxs-lookup"><span data-stu-id="db70b-193">Copy hello .bin file toohello device.</span></span> <span data-ttu-id="db70b-194">Hello .bin fájl toohello eszköz mentése hello eszköz toorestart azt eredményezi, és futtassa hello programot hello .bin fájl található.</span><span class="sxs-lookup"><span data-stu-id="db70b-194">Saving hello .bin file toohello device causes hello device toorestart and run hello program contained in hello .bin file.</span></span> <span data-ttu-id="db70b-195">Később manuálisan is újraindíthatja hello program bármikor hello mbed eszközön hello alaphelyzetbe állítása gomb lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="db70b-195">You can manually restart hello program at any time by pressing hello reset button on hello mbed device.</span></span>

1. <span data-ttu-id="db70b-196">Csatlakozzon SSH ügyfél, mint a PuTTY alkalmazást használ toohello eszköz.</span><span class="sxs-lookup"><span data-stu-id="db70b-196">Connect toohello device using an SSH client application, such as PuTTY.</span></span> <span data-ttu-id="db70b-197">Azt is meghatározhatja a eszköz által használt által a Windows hello soros port.</span><span class="sxs-lookup"><span data-stu-id="db70b-197">You can determine hello serial port your device uses by checking Windows Device Manager.</span></span>
   
    ![][11]

1. <span data-ttu-id="db70b-198">A PuTTY, kattintson a hello **soros** kapcsolattípus.</span><span class="sxs-lookup"><span data-stu-id="db70b-198">In PuTTY, click hello **Serial** connection type.</span></span> <span data-ttu-id="db70b-199">hello általában eszköze 9600 átviteli sebességgel, ezért adja meg 9600 hello **sebesség** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="db70b-199">hello device typically connects at 9600 baud, so enter 9600 in hello **Speed** box.</span></span> <span data-ttu-id="db70b-200">Kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="db70b-200">Then click **Open**.</span></span>

1. <span data-ttu-id="db70b-201">hello program végrehajtásának elkezdése.</span><span class="sxs-lookup"><span data-stu-id="db70b-201">hello program starts executing.</span></span> <span data-ttu-id="db70b-202">Előfordulhat, hogy tooreset hello board (nyomja meg a CTRL + Break vagy nyomja le az hello board alaphelyzetbe állítása gomb) Ha hello program nem indul el automatikusan csatlakoztatásakor.</span><span class="sxs-lookup"><span data-stu-id="db70b-202">You may have tooreset hello board (press CTRL+Break or press hello board's reset button) if hello program does not start automatically when you connect.</span></span>
   
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
