---
title: "MyDriving Azure IoT-példa: első lépések |} Microsoft Docs"
description: "Ismerkedés az alkalmazást, a Microsoft Azure Stream Analytics, a Machine Learning és az Event Hubs használatával az IoT-rendszer tervezővel hogyan átfogó bemutatója."
services: 
documentationcenter: .net
suite: 
author: harikmenon
manager: douge
ms.assetid: f40ea71b-5721-4a6b-a886-53c2e9dffe8f
ms.service: multiple
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: dotnet
ms.topic: article
ms.date: 03/25/2016
ms.author: harikm
ms.openlocfilehash: 031b492df1f186087e7b91102cbb44f552999293
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="mydriving-iot-system-quick-start"></a><span data-ttu-id="39ec6-103">MyDriving IoT system: első lépések</span><span class="sxs-lookup"><span data-stu-id="39ec6-103">MyDriving IoT system: Quick start</span></span>
<span data-ttu-id="39ec6-104">MyDriving egy rendszer, amely bemutatja a megtervezését és megvalósítását egy tipikus [az eszközök internetes hálózatát](iot-suite-overview.md) (IoT) megoldás, amely telemetriai adatokat gyűjt a eszközöket, feldolgozza ezeket az adatokat a felhőben, és gépi tanulással adjon meg érvényes egy adaptív választ.</span><span class="sxs-lookup"><span data-stu-id="39ec6-104">MyDriving is a system that demonstrates the design and implementation of a typical [Internet of Things](iot-suite-overview.md) (IoT) solution that gathers telemetry from devices, processes that data in the cloud, and applies machine learning to provide an adaptive response.</span></span> <span data-ttu-id="39ec6-105">A bemutató a car utazgatással kapcsolatos adatokat naplózza a mobiltelefonjára, mind a kiszolgáló, amely adatokat gyűjt a car ellenőrző rendszerből származó adatokkal.</span><span class="sxs-lookup"><span data-stu-id="39ec6-105">The demonstration logs data about your car trips, by using data from both your mobile phone and an adapter that collects information from your car’s control system.</span></span> <span data-ttu-id="39ec6-106">A más felhasználókkal szemben a vezetői style visszajelzést használja ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="39ec6-106">It uses this data to provide feedback on your driving style in comparison to other users.</span></span>

<span data-ttu-id="39ec6-107">A valós MyDriving célja a saját IoT-megoldás létrehozása a kezdéshez.</span><span class="sxs-lookup"><span data-stu-id="39ec6-107">The real purpose of MyDriving is to get you started in creating your own IoT solution.</span></span> <span data-ttu-id="39ec6-108">De előtt, hogy jelentkezzen be a MyDriving maga az alkalmazás – a vizsgálat felhasználói csoport tagja lesz.</span><span class="sxs-lookup"><span data-stu-id="39ec6-108">But before that, let’s get you going with the MyDriving app itself--as a member of our test user team.</span></span> <span data-ttu-id="39ec6-109">Ez lehetővé teszi a számára az alkalmazás és a rendszer mögötte vásárlói, mielőtt jobban elmélyedne architektúrájának.</span><span class="sxs-lookup"><span data-stu-id="39ec6-109">This gives you an experience of the app and the system behind it as a consumer, before you delve into the architecture.</span></span> <span data-ttu-id="39ec6-110">Azt is bemutatja a Hockeyappra, egy ritkán használt adatok módszer az alkalmazások alpha és a béta disztribúciók kezelése felhasználók tesztelése.</span><span class="sxs-lookup"><span data-stu-id="39ec6-110">It also introduces you to HockeyApp, a cool way of managing the alpha and beta distributions of your apps to test users.</span></span>

## <a name="use-the-mobile-experience"></a><span data-ttu-id="39ec6-111">A mobil élmény használata</span><span class="sxs-lookup"><span data-stu-id="39ec6-111">Use the mobile experience</span></span>
<span data-ttu-id="39ec6-112">A MyDriving alkalmazást is használhatja, ha az Android, iOS vagy Windows 10-es eszköz.</span><span class="sxs-lookup"><span data-stu-id="39ec6-112">You can use the MyDriving app if you have an Android, iOS, or Windows 10 device.</span></span>

### <a name="android-and-windows-10-mobile-installation"></a><span data-ttu-id="39ec6-113">Android és Windows 10 Mobile-telepítés</span><span class="sxs-lookup"><span data-stu-id="39ec6-113">Android and Windows 10 Mobile installation</span></span>
<span data-ttu-id="39ec6-114">Az eszközön:</span><span class="sxs-lookup"><span data-stu-id="39ec6-114">On your device:</span></span>

1. <span data-ttu-id="39ec6-115">Fejlesztői alkalmazások engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="39ec6-115">Allow development apps:</span></span>
   
   * <span data-ttu-id="39ec6-116">Android: Az **beállítások** > **biztonsági**, engedélyezi, hogy az alkalmazások **ismeretlen források**.</span><span class="sxs-lookup"><span data-stu-id="39ec6-116">Android: In **Settings** > **Security**, allow apps from **Unknown sources**.</span></span>
   * <span data-ttu-id="39ec6-117">Windows 10: Az **beállítások** > **frissítések** > **a fejlesztők**, beállíthatja **fejlesztői mód**.</span><span class="sxs-lookup"><span data-stu-id="39ec6-117">Windows 10: In **Settings** > **Updates** > **For Developers**, set **Developer mode**.</span></span>
2. <span data-ttu-id="39ec6-118">A béta teszt csapat csatlakoznak, és regisztrál, vagy a bejelentkezés, [Hockeyappra](https://rink.hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="39ec6-118">Join our beta test team by signing up with, or signing in to, [HockeyApp](https://rink.hockeyapp.net).</span></span> <span data-ttu-id="39ec6-119">Hockeyappra megkönnyíti, hogy az alkalmazás tesztelése a felhasználók korai kiadásaiban terjesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="39ec6-119">HockeyApp makes it easy to distribute early releases of your app to test users.</span></span>
   
   <span data-ttu-id="39ec6-120">Windows 10 használata, használja az Edge böngészőben.</span><span class="sxs-lookup"><span data-stu-id="39ec6-120">If you’re using Windows 10, use the Edge browser.</span></span>
   
   <span data-ttu-id="39ec6-121">Ha egy Build 2016 résztvevő, jelentkezzen be a ugyanazon Microsoft-fiókja e-mail regisztrálva a a konferencia használatával a Microsoft gombokra.</span><span class="sxs-lookup"><span data-stu-id="39ec6-121">If you were a Build 2016 attendee, sign in with the same Microsoft account email that you registered for the conference, by using one of the Microsoft buttons.</span></span> <span data-ttu-id="39ec6-122">Már regisztrálva van a Hockeyappra.</span><span class="sxs-lookup"><span data-stu-id="39ec6-122">You’re already signed up with HockeyApp.</span></span>
   
   ![Hockeyappra bejelentkezési képernyő](./media/iot-solution-get-started/image1.png)
3. <span data-ttu-id="39ec6-124">Töltse le és telepítse az alkalmazást innen:</span><span class="sxs-lookup"><span data-stu-id="39ec6-124">Download and install the app from here:</span></span>
   
   * [<span data-ttu-id="39ec6-125">Android</span><span class="sxs-lookup"><span data-stu-id="39ec6-125">Android</span></span>](http://rink.io/spMyDrivingAndroid)
   * [<span data-ttu-id="39ec6-126">Windows 10</span><span class="sxs-lookup"><span data-stu-id="39ec6-126">Windows 10</span></span>](http://rink.io/spMyDrivingUWP)
   
   <span data-ttu-id="39ec6-127">Két elemek vannak.</span><span class="sxs-lookup"><span data-stu-id="39ec6-127">There are two items.</span></span> <span data-ttu-id="39ec6-128">Telepítse a tanúsítványt **megbízható személyek**.</span><span class="sxs-lookup"><span data-stu-id="39ec6-128">Install the certificate in **Trusted People**.</span></span> <span data-ttu-id="39ec6-129">Telepítse az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="39ec6-129">Then install the app.</span></span>

<span data-ttu-id="39ec6-130">*Probléma merül fel az alkalmazás indítása a Windows 10 Mobile?*</span><span class="sxs-lookup"><span data-stu-id="39ec6-130">*Any issues starting the app on Windows 10 Mobile?*</span></span> <span data-ttu-id="39ec6-131">A telefon lehet egy frissítést, vagy két mögött.</span><span class="sxs-lookup"><span data-stu-id="39ec6-131">Your phone might be an update or two behind.</span></span> <span data-ttu-id="39ec6-132">Győződjön meg arról, hogy telepítve vannak-e a legújabb frissítéseket, vagy telepítse:</span><span class="sxs-lookup"><span data-stu-id="39ec6-132">Make sure you've got the latest updates, or install:</span></span>

* [<span data-ttu-id="39ec6-133">Microsoft.NET.Native.Framework.1.2.appx</span><span class="sxs-lookup"><span data-stu-id="39ec6-133">Microsoft.NET.Native.Framework.1.2.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [<span data-ttu-id="39ec6-134">Microsoft.NET.Native.Runtime.1.1.appx</span><span class="sxs-lookup"><span data-stu-id="39ec6-134">Microsoft.NET.Native.Runtime.1.1.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [<span data-ttu-id="39ec6-135">Microsoft.VCLibs.ARM.14.00.appx</span><span class="sxs-lookup"><span data-stu-id="39ec6-135">Microsoft.VCLibs.ARM.14.00.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a><span data-ttu-id="39ec6-136">iOS-telepítés</span><span class="sxs-lookup"><span data-stu-id="39ec6-136">iOS installation</span></span>
<span data-ttu-id="39ec6-137">Ha a felügyelt Build 2016, töltse le az alkalmazást a Hockeyappra a teszt csapat tagjaként:</span><span class="sxs-lookup"><span data-stu-id="39ec6-137">If you attended Build 2016, download the app as a member of our test team on HockeyApp:</span></span>

1. <span data-ttu-id="39ec6-138">Az iOS-eszközön bejelentkezni [Hockeyappra](https://rink.hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="39ec6-138">On your iOS device, sign in to [HockeyApp](https://rink.hockeyapp.net).</span></span>
   <span data-ttu-id="39ec6-139">A Microsoft bejelentkezési gombok, és jelentkezzen be a ugyanazon Microsoft-fiókja e-mail a konferencia regisztrált egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="39ec6-139">Use one of the Microsoft sign-in buttons, and sign in with the same Microsoft account email that you registered with the conference.</span></span> <span data-ttu-id="39ec6-140">(Az e-mailek és a jelszó mező nem használható.)</span><span class="sxs-lookup"><span data-stu-id="39ec6-140">(Don’t use the email and password fields.)</span></span>
   
   ![Hockeyappra bejelentkezési képernyő](./media/iot-solution-get-started/image1.png)
2. <span data-ttu-id="39ec6-142">A Hockeyappra irányítópulton MyDriving válassza ki, és töltse le azt.</span><span class="sxs-lookup"><span data-stu-id="39ec6-142">In the HockeyApp dashboard, select MyDriving and download it.</span></span>
3. <span data-ttu-id="39ec6-143">A bétaverzió a Hockeyappra engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="39ec6-143">Authorize the beta release from HockeyApp:</span></span>
   
   <span data-ttu-id="39ec6-144">a.</span><span class="sxs-lookup"><span data-stu-id="39ec6-144">a.</span></span> <span data-ttu-id="39ec6-145">Ugrás a **beállítások** > **általános** > **profilok és kezelése.**</span><span class="sxs-lookup"><span data-stu-id="39ec6-145">Go to **Settings** > **General** > **Profiles and Device Management.**</span></span>
   
   <span data-ttu-id="39ec6-146">b.</span><span class="sxs-lookup"><span data-stu-id="39ec6-146">b.</span></span> <span data-ttu-id="39ec6-147">Megbízható a **Bit Stadium GmbH** tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="39ec6-147">Trust the **Bit Stadium GmbH** certificate.</span></span>

<span data-ttu-id="39ec6-148">Ha a Build 2016 nem vették fel, létrehozhatja és telepítse az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="39ec6-148">If you didn’t attend Build 2016, you can build and deploy the app yourself:</span></span>

1. <span data-ttu-id="39ec6-149">A kód letöltése [a Githubról].</span><span class="sxs-lookup"><span data-stu-id="39ec6-149">Download the code [from GitHub].</span></span>
2. <span data-ttu-id="39ec6-150">Hozza létre és telepíthet [Xamarin segítségével].</span><span class="sxs-lookup"><span data-stu-id="39ec6-150">Build and deploy by [using Xamarin].</span></span>

<span data-ttu-id="39ec6-151">A további részletekért a [MyDriving használati útmutató](http://aka.ms/mydrivingdocs).</span><span class="sxs-lookup"><span data-stu-id="39ec6-151">Find more details in the [MyDriving Reference Guide](http://aka.ms/mydrivingdocs).</span></span>

## <a name="get-an-obd-adapter-optional"></a><span data-ttu-id="39ec6-152">Első OBD adapter (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="39ec6-152">Get an OBD adapter (optional)</span></span>
<span data-ttu-id="39ec6-153">Ez az a része, amelyek miatt ez egy valódi az eszközök internetes hálózatát rendszer!</span><span class="sxs-lookup"><span data-stu-id="39ec6-153">This is the part that makes this a real Internet of Things system!</span></span> <span data-ttu-id="39ec6-154">Használhatja az alkalmazás nélkül, de valódi szórakozás, és kevésbé költséges.</span><span class="sxs-lookup"><span data-stu-id="39ec6-154">You can use the app without one, but it’s more fun with the real thing, and they aren’t expensive.</span></span>

<span data-ttu-id="39ec6-155">A helyi diagnosztika (OBD) az a, amely a garázsnak használatával a car hangolása és páratlan zajnak vagy figyelmeztetés lámpa diagnosztizálja car szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="39ec6-155">On-board diagnostics (OBD) is the feature of your car that the garage uses to tune up your car and diagnose odd noises and warning lamps.</span></span> <span data-ttu-id="39ec6-156">Csak a car a kiváló régiségek, ha található valahol szoftvercsatorna kézi, általában egy flap alatt az irányítópult mögött.</span><span class="sxs-lookup"><span data-stu-id="39ec6-156">Unless your car is of great antiquity, you’ll find a socket somewhere in the cabin, typically behind a flap under the dashboard.</span></span> <span data-ttu-id="39ec6-157">A jobb oldali összekötővel a motor teljesítmény metrikákat kaphat, és egyes módosításokat.</span><span class="sxs-lookup"><span data-stu-id="39ec6-157">With the right connector, you can get metrics of the engine’s performance and make certain adjustments.</span></span> <span data-ttu-id="39ec6-158">Egy OBD összekötő olcsón lehet beszerezni a szokásos helyek.</span><span class="sxs-lookup"><span data-stu-id="39ec6-158">An OBD connector can be purchased cheaply from the usual places.</span></span> <span data-ttu-id="39ec6-159">Egy alkalmazást a telefonra Bluetooth vagy Wi-Fi használatával tud csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="39ec6-159">It connects by using Bluetooth or Wi-Fi to an app on your phone.</span></span>

<span data-ttu-id="39ec6-160">Ebben az esetben lesz, a car csatlakozni a felhőhöz.</span><span class="sxs-lookup"><span data-stu-id="39ec6-160">In this case, we’re going to connect your car to the cloud.</span></span> <span data-ttu-id="39ec6-161">A közvetlen kapcsolat az OBD a telefonjára, de az alkalmazás működik, mint egy továbbítót.</span><span class="sxs-lookup"><span data-stu-id="39ec6-161">The direct connection from the OBD is to your phone, but our app works as a relay.</span></span> <span data-ttu-id="39ec6-162">A car telemetriai adatokat küld rögtön a MyDriving IoT-központ feldolgozza naplózni a közúti való adatváltások számát és a vezetői stílus értékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="39ec6-162">Your car's telemetry is sent straight to the MyDriving IoT hub, where it's processed to log your road trips and assess your driving style.</span></span>

<span data-ttu-id="39ec6-163">Csatlakoztassa az OBD eszközt:</span><span class="sxs-lookup"><span data-stu-id="39ec6-163">To connect an OBD device:</span></span>

1. <span data-ttu-id="39ec6-164">Ellenőrizze, hogy rendelkezik-e a car egy OBD szoftvercsatorna.</span><span class="sxs-lookup"><span data-stu-id="39ec6-164">Check that your car has an OBD socket.</span></span>
2. <span data-ttu-id="39ec6-165">Szerezzen be egy OBD adapter:</span><span class="sxs-lookup"><span data-stu-id="39ec6-165">Obtain an OBD adapter:</span></span>
   
   * <span data-ttu-id="39ec6-166">Ha egy Android- vagy Windows phone használata esetén szükséges egy Bluetooth-kompatibilis OBD II adapter.</span><span class="sxs-lookup"><span data-stu-id="39ec6-166">If you're using an Android or Windows phone, you need a Bluetooth-enabled OBD II adapter.</span></span> <span data-ttu-id="39ec6-167">Használtuk [BAFX termékek 34t5 Bluetooth OBDII vizsgálati eszköz].</span><span class="sxs-lookup"><span data-stu-id="39ec6-167">We used [BAFX Products 34t5 Bluetooth OBDII Scan Tool].</span></span>
   * <span data-ttu-id="39ec6-168">Ha egy iOS-telefon használata esetén szükséges egy Wi-Fi-kompatibilis OBD adapter.</span><span class="sxs-lookup"><span data-stu-id="39ec6-168">If you're using an iOS phone, you need a Wi-Fi-enabled OBD adapter.</span></span> <span data-ttu-id="39ec6-169">Használtuk [ScanTool OBDLink MX Wi-Fi: OBD Adapter/diagnosztikai képolvasó].</span><span class="sxs-lookup"><span data-stu-id="39ec6-169">We used [ScanTool OBDLink MX Wi-Fi: OBD Adapter/Diagnostic Scanner].</span></span>
3. <span data-ttu-id="39ec6-170">Kövesse az utasításokat, amelyek a OBD adapterhez csatlakozzanak a telefonjára.</span><span class="sxs-lookup"><span data-stu-id="39ec6-170">Follow the instructions that come with your OBD adapter to connect it to your phone.</span></span> <span data-ttu-id="39ec6-171">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="39ec6-171">Keep the following in mind:</span></span>
   
   * <span data-ttu-id="39ec6-172">A Bluetooth-adapter típusnak kell megfeleltetni a telefont, a a **beállítások** lap.</span><span class="sxs-lookup"><span data-stu-id="39ec6-172">A Bluetooth adapter must be paired with the phone, on the **Settings** page.</span></span>
   * <span data-ttu-id="39ec6-173">A Wi-Fi adapter az a tartomány 192.168.xxx.xxx címmel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="39ec6-173">A Wi-Fi adapter must have an address in the range 192.168.xxx.xxx.</span></span>
4. <span data-ttu-id="39ec6-174">Ha több autók, külön adapter kaphat mindegyik (legfeljebb három).</span><span class="sxs-lookup"><span data-stu-id="39ec6-174">If you have several cars, you can get a separate adapter for each (maximum of three).</span></span>

<span data-ttu-id="39ec6-175">Ha egy OBD adapter nem rendelkezik, az alkalmazás lesz továbbra is adatokat küldeni a hely és sebességét, a telefon GPS fogadó a háttérben, és ekkor megkérdezi, hogy szeretné-e egy OBD szimulálásához.</span><span class="sxs-lookup"><span data-stu-id="39ec6-175">If you don’t have an OBD adapter, the app will still send location and speed data from the phone's GPS receiver to the back end and will ask if you want to simulate an OBD.</span></span>

<span data-ttu-id="39ec6-176">Az található további az alkalmazás hogyan használja az adatok OBD adapteréről és beállítások létrehozásához a saját OBD eszközök szakaszban 2.1-es, "Az IoT-eszközök," a [MyDriving használati útmutató](http://aka.ms/mydrivingdocs).</span><span class="sxs-lookup"><span data-stu-id="39ec6-176">You can find out more about how the app uses data from the OBD adapter and about options for creating your own OBD device in section 2.1, "IoT Devices," in the [MyDriving Reference Guide](http://aka.ms/mydrivingdocs).</span></span>

## <a name="use-the-app"></a><span data-ttu-id="39ec6-177">Az alkalmazás használata</span><span class="sxs-lookup"><span data-stu-id="39ec6-177">Use the app</span></span>
<span data-ttu-id="39ec6-178">Indítsa el az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="39ec6-178">Start the app.</span></span> <span data-ttu-id="39ec6-179">Egy kezdeti gyors üzembe helyezés hogyan működik lépésre van.</span><span class="sxs-lookup"><span data-stu-id="39ec6-179">There’s an initial Quickstart to walk you through how it works.</span></span>

### <a name="track-your-trips"></a><span data-ttu-id="39ec6-180">Nyomon követheti a való adatváltások számát</span><span class="sxs-lookup"><span data-stu-id="39ec6-180">Track your trips</span></span>
<span data-ttu-id="39ec6-181">Koppintson a rekord gombra (nagy piros kör a képernyő alján) egy út elindításához, és koppintson újra befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="39ec6-181">Tap the record button (big red circle at the bottom of the screen) to start a trip, and tap again to end.</span></span>

![A rekord gombra nyomon követését utazás ábrája](./media/iot-solution-get-started/image2.png)

<span data-ttu-id="39ec6-183">Utazás, minden egyes indításakor nincs OBD eszköz esetén meg kell adnia a szimulátor használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="39ec6-183">Each time you start a trip, if there’s no OBD device, you’ll be asked if you want to use the simulator.</span></span>

<span data-ttu-id="39ec6-184">Egy út végén koppintson a Leállítás gombra, és összefoglaló információk.</span><span class="sxs-lookup"><span data-stu-id="39ec6-184">At the end of a trip, tap the stop button, and you get a summary.</span></span>

![Egy összegző út – példa](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a><span data-ttu-id="39ec6-186">Tekintse át a való adatváltások számát</span><span class="sxs-lookup"><span data-stu-id="39ec6-186">Review your trips</span></span>
![Egy korábbi út – példa](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a><span data-ttu-id="39ec6-188">Tekintse át a profil</span><span class="sxs-lookup"><span data-stu-id="39ec6-188">Review your profile</span></span>
![A vezetés stílusú profil – példa](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a><span data-ttu-id="39ec6-190">Teszt visszajelzését</span><span class="sxs-lookup"><span data-stu-id="39ec6-190">Send us your test feedback</span></span>
<span data-ttu-id="39ec6-191">MyDriving ismertető segítségével létrehozott saját IoT rendszerek, mert biztosan szeretnénk megosztaná velünk, milyen jól működik kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="39ec6-191">Because we created MyDriving to help jumpstart your own IoT systems, we certainly want to hear from you about how well it works.</span></span> <span data-ttu-id="39ec6-192">Értesítsen minket, ha:</span><span class="sxs-lookup"><span data-stu-id="39ec6-192">Let us know if:</span></span>

* <span data-ttu-id="39ec6-193">Nehézségek vagy kihívásokkal futtatja.</span><span class="sxs-lookup"><span data-stu-id="39ec6-193">You run into difficulties or challenges.</span></span>
* <span data-ttu-id="39ec6-194">Van így könnyebben megfelelőbbek, adott esetben bővítmény pont.</span><span class="sxs-lookup"><span data-stu-id="39ec6-194">There is an extension point that would make it more suitable to your scenario.</span></span>
* <span data-ttu-id="39ec6-195">Egy sokkal hatékonyabb módja bizonyos kell látnia.</span><span class="sxs-lookup"><span data-stu-id="39ec6-195">You find a more efficient way to accomplish certain needs.</span></span>
* <span data-ttu-id="39ec6-196">Javaslatai bármely más MyDriving vagy ebben a dokumentációban javítására.</span><span class="sxs-lookup"><span data-stu-id="39ec6-196">You have any other suggestions for improving MyDriving or this documentation.</span></span>

<span data-ttu-id="39ec6-197">Az alkalmazásban MyDriving magát, használhatja a beépített Hockeyappra visszajelzés mechanizmus: iOS és Android rendszeren csak biztosítják a telefon egy rázó, vagy használja a **visszajelzés** menüparancshoz.</span><span class="sxs-lookup"><span data-stu-id="39ec6-197">Within the MyDriving app itself, you can use the built-in HockeyApp feedback mechanism: on iOS and Android, just give your phone a shake, or use the **Feedback** menu command.</span></span> <span data-ttu-id="39ec6-198">Ez automatikusan elvégzi a képernyőfelvételen látható, hogy tudjuk lesz, mi, hogy van szó.</span><span class="sxs-lookup"><span data-stu-id="39ec6-198">This will automatically attach a screenshot, so that we’ll know what you’re talking about.</span></span> <span data-ttu-id="39ec6-199">És bármely kellemetlen összeomlás esetén Hockeyappra gyűjti a mondja el nekünk azokat-összeomlási naplókat.</span><span class="sxs-lookup"><span data-stu-id="39ec6-199">And if there are any unfortunate crashes, HockeyApp collects the crash logs to tell us about them.</span></span> <span data-ttu-id="39ec6-200">Visszajelzést keresztül is biztosíthat a [Hockeyappra portal].</span><span class="sxs-lookup"><span data-stu-id="39ec6-200">You can also give feedback through the [HockeyApp portal].</span></span>

<span data-ttu-id="39ec6-201">Is fájl egy [problémát a Githubon], vagy hagyja meg az alábbi megjegyzést (en-us edition).</span><span class="sxs-lookup"><span data-stu-id="39ec6-201">You can also file an [issue on GitHub], or leave a comment below (en-us edition).</span></span>

<span data-ttu-id="39ec6-202">Kíváncsian nyújtanak segítséget az Ön!</span><span class="sxs-lookup"><span data-stu-id="39ec6-202">We look forward to hearing from you!</span></span>

## <a name="next-steps"></a><span data-ttu-id="39ec6-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="39ec6-203">Next steps</span></span>
* <span data-ttu-id="39ec6-204">Megismerkedhet a [MyDriving használati útmutató](http://aka.ms/mydrivingdocs) tudni, hogyan előre tervezett és a teljes MyDriving rendszer beépített.</span><span class="sxs-lookup"><span data-stu-id="39ec6-204">Explore the [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) to understand how we’ve designed and built the entire MyDriving system.</span></span>
* <span data-ttu-id="39ec6-205">[Létrehozásakor és központi telepítésekor a rendszer a saját](iot-solution-build-system.md) az Azure Resource Manager-parancsfájlok használatával.</span><span class="sxs-lookup"><span data-stu-id="39ec6-205">[Create and deploy a system of your own](iot-solution-build-system.md) by using our Azure Resource Manager scripts.</span></span> <span data-ttu-id="39ec6-206">A [MyDriving használati útmutató](http://aka.ms/mydrivingdocs) is végigvezeti azokon a területeken, ahol lesz testreszabásokat a legtöbb.</span><span class="sxs-lookup"><span data-stu-id="39ec6-206">The [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) also guides you through areas where you’ll make the most customizations.</span></span>

<span data-ttu-id="39ec6-207">[a Githubról]: https://github.com/Azure-Samples/MyDriving</span><span class="sxs-lookup"><span data-stu-id="39ec6-207">[from GitHub]: https://github.com/Azure-Samples/MyDriving</span></span>
<span data-ttu-id="39ec6-208">[Xamarin segítségével]: https://developer.xamarin.com/guides/ios/getting_started/installation/</span><span class="sxs-lookup"><span data-stu-id="39ec6-208">[using Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/</span></span>
<span data-ttu-id="39ec6-209">[BAFX termékek 34t5 Bluetooth OBDII vizsgálati eszköz]: http://www.amazon.com/gp/product/B005NLQAHS</span><span class="sxs-lookup"><span data-stu-id="39ec6-209">[BAFX Products 34t5 Bluetooth OBDII Scan Tool]: http://www.amazon.com/gp/product/B005NLQAHS</span></span>
<span data-ttu-id="39ec6-210">[ScanTool OBDLink MX Wi-Fi: OBD Adapter/diagnosztikai képolvasó]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP</span><span class="sxs-lookup"><span data-stu-id="39ec6-210">[ScanTool OBDLink MX Wi-Fi: OBD Adapter/Diagnostic Scanner]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP</span></span>
<span data-ttu-id="39ec6-211">[Hockeyappra portal]: https://rink.hockeyapp.org</span><span class="sxs-lookup"><span data-stu-id="39ec6-211">[HockeyApp portal]: https://rink.hockeyapp.org</span></span>
<span data-ttu-id="39ec6-212">[problémát a Githubon]: https://github.com/Azure-Samples/MyDriving/issues</span><span class="sxs-lookup"><span data-stu-id="39ec6-212">[issue on GitHub]: https://github.com/Azure-Samples/MyDriving/issues</span></span>
