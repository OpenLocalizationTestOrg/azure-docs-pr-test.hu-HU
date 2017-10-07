---
title: "MyDriving Azure IoT-példa: első lépések |} Microsoft Docs"
description: "Ismerkedés az alkalmazást, hogyan átfogó bemutatója tooarchitect az IoT-rendszer Microsoft Azure Stream Analytics, a Machine Learning és az Event Hubs használatával."
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
ms.openlocfilehash: 411b9a992deb22b915f8291d8559e2917d976b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mydriving-iot-system-quick-start"></a><span data-ttu-id="90701-103">MyDriving IoT system: első lépések</span><span class="sxs-lookup"><span data-stu-id="90701-103">MyDriving IoT system: Quick start</span></span>
<span data-ttu-id="90701-104">MyDriving egy rendszer, amely bemutatja a hello tervezési és megvalósítási egy tipikus [az eszközök internetes hálózatát](iot-suite-overview.md) (IoT) megoldás, amely telemetriai adatokat gyűjt a eszközöket, feldolgozza ezeket az adatokat hello felhőben, és alkalmazza a gépi tanulás tooprovide egy adaptív választ.</span><span class="sxs-lookup"><span data-stu-id="90701-104">MyDriving is a system that demonstrates hello design and implementation of a typical [Internet of Things](iot-suite-overview.md) (IoT) solution that gathers telemetry from devices, processes that data in hello cloud, and applies machine learning tooprovide an adaptive response.</span></span> <span data-ttu-id="90701-105">hello bemutató a car utazgatással kapcsolatos adatokat naplózza a mobiltelefonjára, mind a kiszolgáló, amely adatokat gyűjt a car ellenőrző rendszerből származó adatokkal.</span><span class="sxs-lookup"><span data-stu-id="90701-105">hello demonstration logs data about your car trips, by using data from both your mobile phone and an adapter that collects information from your car’s control system.</span></span> <span data-ttu-id="90701-106">Az adatok tooprovide visszajelzés a vezetői stílus összehasonlító tooother felhasználók használ.</span><span class="sxs-lookup"><span data-stu-id="90701-106">It uses this data tooprovide feedback on your driving style in comparison tooother users.</span></span>

<span data-ttu-id="90701-107">hello valós MyDriving célja tooget létrehozni a saját IoT-megoldás-t elindította.</span><span class="sxs-lookup"><span data-stu-id="90701-107">hello real purpose of MyDriving is tooget you started in creating your own IoT solution.</span></span> <span data-ttu-id="90701-108">De előtt, hogy jelentkezzen be a hello MyDriving alkalmazás – a vizsgálat felhasználói csoport tagja lesz.</span><span class="sxs-lookup"><span data-stu-id="90701-108">But before that, let’s get you going with hello MyDriving app itself--as a member of our test user team.</span></span> <span data-ttu-id="90701-109">Ez lehetővé teszi egy élmény hello alkalmazás és hello rendszer mögötte vásárlói, mielőtt jobban elmélyedne hello architektúra.</span><span class="sxs-lookup"><span data-stu-id="90701-109">This gives you an experience of hello app and hello system behind it as a consumer, before you delve into hello architecture.</span></span> <span data-ttu-id="90701-110">Azt is bemutatja a tooHockeyApp, a ritkán használt adatok módja hello alpha és a béta disztribúciók a alkalmazások tootest felhasználók kezelése.</span><span class="sxs-lookup"><span data-stu-id="90701-110">It also introduces you tooHockeyApp, a cool way of managing hello alpha and beta distributions of your apps tootest users.</span></span>

## <a name="use-hello-mobile-experience"></a><span data-ttu-id="90701-111">Mobil élmény hello használata</span><span class="sxs-lookup"><span data-stu-id="90701-111">Use hello mobile experience</span></span>
<span data-ttu-id="90701-112">Hello MyDriving alkalmazást is használhatja, ha az Android, iOS vagy Windows 10-es eszköz.</span><span class="sxs-lookup"><span data-stu-id="90701-112">You can use hello MyDriving app if you have an Android, iOS, or Windows 10 device.</span></span>

### <a name="android-and-windows-10-mobile-installation"></a><span data-ttu-id="90701-113">Android és Windows 10 Mobile-telepítés</span><span class="sxs-lookup"><span data-stu-id="90701-113">Android and Windows 10 Mobile installation</span></span>
<span data-ttu-id="90701-114">Az eszközön:</span><span class="sxs-lookup"><span data-stu-id="90701-114">On your device:</span></span>

1. <span data-ttu-id="90701-115">Fejlesztői alkalmazások engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="90701-115">Allow development apps:</span></span>
   
   * <span data-ttu-id="90701-116">Android: Az **beállítások** > **biztonsági**, engedélyezi, hogy az alkalmazások **ismeretlen források**.</span><span class="sxs-lookup"><span data-stu-id="90701-116">Android: In **Settings** > **Security**, allow apps from **Unknown sources**.</span></span>
   * <span data-ttu-id="90701-117">Windows 10: Az **beállítások** > **frissítések** > **a fejlesztők**, beállíthatja **fejlesztői mód**.</span><span class="sxs-lookup"><span data-stu-id="90701-117">Windows 10: In **Settings** > **Updates** > **For Developers**, set **Developer mode**.</span></span>
2. <span data-ttu-id="90701-118">A béta teszt csapat csatlakoznak, és regisztrál, vagy a bejelentkezés, [Hockeyappra](https://rink.hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="90701-118">Join our beta test team by signing up with, or signing in to, [HockeyApp](https://rink.hockeyapp.net).</span></span> <span data-ttu-id="90701-119">Hockeyappra lehetővé teszi a tootest felhasználók könnyen toodistribute korai kiadásaiban.</span><span class="sxs-lookup"><span data-stu-id="90701-119">HockeyApp makes it easy toodistribute early releases of your app tootest users.</span></span>
   
   <span data-ttu-id="90701-120">Ha Windows 10 használata esetén használja a hello Edge böngésző.</span><span class="sxs-lookup"><span data-stu-id="90701-120">If you’re using Windows 10, use hello Edge browser.</span></span>
   
   <span data-ttu-id="90701-121">Ha egy Build 2016 résztvevő, jelentkezzen be hello azonos hello Microsoft gombok segítségével regisztrált hello konferencia, a Microsoft-fiókja e-mail.</span><span class="sxs-lookup"><span data-stu-id="90701-121">If you were a Build 2016 attendee, sign in with hello same Microsoft account email that you registered for hello conference, by using one of hello Microsoft buttons.</span></span> <span data-ttu-id="90701-122">Már regisztrálva van a Hockeyappra.</span><span class="sxs-lookup"><span data-stu-id="90701-122">You’re already signed up with HockeyApp.</span></span>
   
   ![Hockeyappra bejelentkezési képernyő](./media/iot-solution-get-started/image1.png)
3. <span data-ttu-id="90701-124">Alkalmazás letöltéséhez és telepítéséhez hello innen:</span><span class="sxs-lookup"><span data-stu-id="90701-124">Download and install hello app from here:</span></span>
   
   * [<span data-ttu-id="90701-125">Android</span><span class="sxs-lookup"><span data-stu-id="90701-125">Android</span></span>](http://rink.io/spMyDrivingAndroid)
   * [<span data-ttu-id="90701-126">Windows 10</span><span class="sxs-lookup"><span data-stu-id="90701-126">Windows 10</span></span>](http://rink.io/spMyDrivingUWP)
   
   <span data-ttu-id="90701-127">Két elemek vannak.</span><span class="sxs-lookup"><span data-stu-id="90701-127">There are two items.</span></span> <span data-ttu-id="90701-128">Telepítse a hello **megbízható személyek**.</span><span class="sxs-lookup"><span data-stu-id="90701-128">Install hello certificate in **Trusted People**.</span></span> <span data-ttu-id="90701-129">Majd telepítse hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="90701-129">Then install hello app.</span></span>

<span data-ttu-id="90701-130">*Probléma merül fel hello alkalmazás indítása a Windows 10 Mobile?*</span><span class="sxs-lookup"><span data-stu-id="90701-130">*Any issues starting hello app on Windows 10 Mobile?*</span></span> <span data-ttu-id="90701-131">A telefon lehet egy frissítést, vagy két mögött.</span><span class="sxs-lookup"><span data-stu-id="90701-131">Your phone might be an update or two behind.</span></span> <span data-ttu-id="90701-132">Győződjön meg arról, hello legújabb frissítéseinek van, vagy telepítse:</span><span class="sxs-lookup"><span data-stu-id="90701-132">Make sure you've got hello latest updates, or install:</span></span>

* [<span data-ttu-id="90701-133">Microsoft.NET.Native.Framework.1.2.appx</span><span class="sxs-lookup"><span data-stu-id="90701-133">Microsoft.NET.Native.Framework.1.2.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [<span data-ttu-id="90701-134">Microsoft.NET.Native.Runtime.1.1.appx</span><span class="sxs-lookup"><span data-stu-id="90701-134">Microsoft.NET.Native.Runtime.1.1.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [<span data-ttu-id="90701-135">Microsoft.VCLibs.ARM.14.00.appx</span><span class="sxs-lookup"><span data-stu-id="90701-135">Microsoft.VCLibs.ARM.14.00.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a><span data-ttu-id="90701-136">iOS-telepítés</span><span class="sxs-lookup"><span data-stu-id="90701-136">iOS installation</span></span>
<span data-ttu-id="90701-137">Ha a felügyelt Build 2016, töltse le a hello alkalmazást a Hockeyappra a teszt csapat tagjaként:</span><span class="sxs-lookup"><span data-stu-id="90701-137">If you attended Build 2016, download hello app as a member of our test team on HockeyApp:</span></span>

1. <span data-ttu-id="90701-138">IOS-eszközön, jelentkezzen be túl[Hockeyappra](https://rink.hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="90701-138">On your iOS device, sign in too[HockeyApp](https://rink.hockeyapp.net).</span></span>
   <span data-ttu-id="90701-139">Ugyanazon Microsoft-fiókja e-mail hello konferencia regisztrált hello hello Microsoft bejelentkezési gombok, és jelentkezzen be az egyik használható.</span><span class="sxs-lookup"><span data-stu-id="90701-139">Use one of hello Microsoft sign-in buttons, and sign in with hello same Microsoft account email that you registered with hello conference.</span></span> <span data-ttu-id="90701-140">(A hello e-mailek és a jelszó mező nem használható.)</span><span class="sxs-lookup"><span data-stu-id="90701-140">(Don’t use hello email and password fields.)</span></span>
   
   ![Hockeyappra bejelentkezési képernyő](./media/iot-solution-get-started/image1.png)
2. <span data-ttu-id="90701-142">Hello Hockeyappra irányítópulton válassza ki a MyDriving, és le is.</span><span class="sxs-lookup"><span data-stu-id="90701-142">In hello HockeyApp dashboard, select MyDriving and download it.</span></span>
3. <span data-ttu-id="90701-143">Engedélyezze a hello bétaverziót a Hockeyappra:</span><span class="sxs-lookup"><span data-stu-id="90701-143">Authorize hello beta release from HockeyApp:</span></span>
   
   <span data-ttu-id="90701-144">a.</span><span class="sxs-lookup"><span data-stu-id="90701-144">a.</span></span> <span data-ttu-id="90701-145">Nyissa meg túl**beállítások** > **általános** > **profilok és kezelése.**</span><span class="sxs-lookup"><span data-stu-id="90701-145">Go too**Settings** > **General** > **Profiles and Device Management.**</span></span>
   
   <span data-ttu-id="90701-146">b.</span><span class="sxs-lookup"><span data-stu-id="90701-146">b.</span></span> <span data-ttu-id="90701-147">Megbízható hello **Bit Stadium GmbH** tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="90701-147">Trust hello **Bit Stadium GmbH** certificate.</span></span>

<span data-ttu-id="90701-148">Ha a Build 2016 nem vették fel, létrehozhatja és saját kezűleg hello alkalmazás telepítése:</span><span class="sxs-lookup"><span data-stu-id="90701-148">If you didn’t attend Build 2016, you can build and deploy hello app yourself:</span></span>

1. <span data-ttu-id="90701-149">Töltse le a hello kód [a Githubról].</span><span class="sxs-lookup"><span data-stu-id="90701-149">Download hello code [from GitHub].</span></span>
2. <span data-ttu-id="90701-150">Hozza létre és telepíthet [Xamarin segítségével].</span><span class="sxs-lookup"><span data-stu-id="90701-150">Build and deploy by [using Xamarin].</span></span>

<span data-ttu-id="90701-151">További részletekért található hello [MyDriving használati útmutató](http://aka.ms/mydrivingdocs).</span><span class="sxs-lookup"><span data-stu-id="90701-151">Find more details in hello [MyDriving Reference Guide](http://aka.ms/mydrivingdocs).</span></span>

## <a name="get-an-obd-adapter-optional"></a><span data-ttu-id="90701-152">Első OBD adapter (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="90701-152">Get an OBD adapter (optional)</span></span>
<span data-ttu-id="90701-153">Ez az hello része, amelyek miatt ez egy valódi az eszközök internetes hálózatát rendszer!</span><span class="sxs-lookup"><span data-stu-id="90701-153">This is hello part that makes this a real Internet of Things system!</span></span> <span data-ttu-id="90701-154">Hello alkalmazással nélkül, de Szórakozás hello valós dolog, és kevésbé költséges.</span><span class="sxs-lookup"><span data-stu-id="90701-154">You can use hello app without one, but it’s more fun with hello real thing, and they aren’t expensive.</span></span>

<span data-ttu-id="90701-155">A helyi diagnosztika (OBD) a hello garázs használ tootune fel a car és páratlan zajnak vagy figyelmeztetés lámpa diagnosztizálja car hello funkciója.</span><span class="sxs-lookup"><span data-stu-id="90701-155">On-board diagnostics (OBD) is hello feature of your car that hello garage uses tootune up your car and diagnose odd noises and warning lamps.</span></span> <span data-ttu-id="90701-156">Csak a car a kiváló régiségek, ha található valahol szoftvercsatorna hello kézi, általában egy flap alatt hello irányítópult mögött.</span><span class="sxs-lookup"><span data-stu-id="90701-156">Unless your car is of great antiquity, you’ll find a socket somewhere in hello cabin, typically behind a flap under hello dashboard.</span></span> <span data-ttu-id="90701-157">Hello jobb összekötővel hello motor teljesítmény metrikákat kaphat, és egyes módosításokat.</span><span class="sxs-lookup"><span data-stu-id="90701-157">With hello right connector, you can get metrics of hello engine’s performance and make certain adjustments.</span></span> <span data-ttu-id="90701-158">Egy OBD összekötő olcsón megvásárolható hello szokásos helyről.</span><span class="sxs-lookup"><span data-stu-id="90701-158">An OBD connector can be purchased cheaply from hello usual places.</span></span> <span data-ttu-id="90701-159">A telefonján Bluetooth vagy Wi-Fi tooan alkalmazás használatával tud csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="90701-159">It connects by using Bluetooth or Wi-Fi tooan app on your phone.</span></span>

<span data-ttu-id="90701-160">Ebben az esetben az oktatóanyagban módosítjuk tooconnect a car toohello felhő.</span><span class="sxs-lookup"><span data-stu-id="90701-160">In this case, we’re going tooconnect your car toohello cloud.</span></span> <span data-ttu-id="90701-161">közvetlen kapcsolatot hello hello OBD tooyour telefon, de az alkalmazás működik, mint egy továbbítót.</span><span class="sxs-lookup"><span data-stu-id="90701-161">hello direct connection from hello OBD is tooyour phone, but our app works as a relay.</span></span> <span data-ttu-id="90701-162">A car telemetriai küldött egyenes toohello MyDriving IoT-központot, ahol feldolgozásra toolog a közúti való adatváltások számát, és a vezetői stílus értékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="90701-162">Your car's telemetry is sent straight toohello MyDriving IoT hub, where it's processed toolog your road trips and assess your driving style.</span></span>

<span data-ttu-id="90701-163">egy OBD eszköz tooconnect:</span><span class="sxs-lookup"><span data-stu-id="90701-163">tooconnect an OBD device:</span></span>

1. <span data-ttu-id="90701-164">Ellenőrizze, hogy rendelkezik-e a car egy OBD szoftvercsatorna.</span><span class="sxs-lookup"><span data-stu-id="90701-164">Check that your car has an OBD socket.</span></span>
2. <span data-ttu-id="90701-165">Szerezzen be egy OBD adapter:</span><span class="sxs-lookup"><span data-stu-id="90701-165">Obtain an OBD adapter:</span></span>
   
   * <span data-ttu-id="90701-166">Ha egy Android- vagy Windows phone használata esetén szükséges egy Bluetooth-kompatibilis OBD II adapter.</span><span class="sxs-lookup"><span data-stu-id="90701-166">If you're using an Android or Windows phone, you need a Bluetooth-enabled OBD II adapter.</span></span> <span data-ttu-id="90701-167">Használtuk [BAFX termékek 34t5 Bluetooth OBDII vizsgálati eszköz].</span><span class="sxs-lookup"><span data-stu-id="90701-167">We used [BAFX Products 34t5 Bluetooth OBDII Scan Tool].</span></span>
   * <span data-ttu-id="90701-168">Ha egy iOS-telefon használata esetén szükséges egy Wi-Fi-kompatibilis OBD adapter.</span><span class="sxs-lookup"><span data-stu-id="90701-168">If you're using an iOS phone, you need a Wi-Fi-enabled OBD adapter.</span></span> <span data-ttu-id="90701-169">Használtuk [ScanTool OBDLink MX Wi-Fi: OBD Adapter/diagnosztikai képolvasó].</span><span class="sxs-lookup"><span data-stu-id="90701-169">We used [ScanTool OBDLink MX Wi-Fi: OBD Adapter/Diagnostic Scanner].</span></span>
3. <span data-ttu-id="90701-170">Hajtsa végre a hello vonatkozó utasításokat a OBD adapter tooconnect azt tooyour phone.</span><span class="sxs-lookup"><span data-stu-id="90701-170">Follow hello instructions that come with your OBD adapter tooconnect it tooyour phone.</span></span> <span data-ttu-id="90701-171">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="90701-171">Keep hello following in mind:</span></span>
   
   * <span data-ttu-id="90701-172">A Bluetooth-adapter típusnak kell megfeleltetni hello telefon, a hello **beállítások** lap.</span><span class="sxs-lookup"><span data-stu-id="90701-172">A Bluetooth adapter must be paired with hello phone, on hello **Settings** page.</span></span>
   * <span data-ttu-id="90701-173">A Wi-Fi adapter a hello tartomány 192.168.xxx.xxx címmel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="90701-173">A Wi-Fi adapter must have an address in hello range 192.168.xxx.xxx.</span></span>
4. <span data-ttu-id="90701-174">Ha több autók, külön adapter kaphat mindegyik (legfeljebb három).</span><span class="sxs-lookup"><span data-stu-id="90701-174">If you have several cars, you can get a separate adapter for each (maximum of three).</span></span>

<span data-ttu-id="90701-175">Ha egy OBD adapter nem rendelkezik, hello alkalmazás továbbra is küldi a hely és a hello phone GPS fogadó toohello vissza sebességű adatok befejezését, majd fogja kérni, ha azt szeretné, egy OBD toosimulate.</span><span class="sxs-lookup"><span data-stu-id="90701-175">If you don’t have an OBD adapter, hello app will still send location and speed data from hello phone's GPS receiver toohello back end and will ask if you want toosimulate an OBD.</span></span>

<span data-ttu-id="90701-176">Az hello található további hogyan hello alkalmazása használja-e a hello OBD adapter származó adatok és beállítások létrehozásához a saját OBD eszközök szakaszban 2.1-es, "Az IoT-eszközök," [MyDriving használati útmutató](http://aka.ms/mydrivingdocs).</span><span class="sxs-lookup"><span data-stu-id="90701-176">You can find out more about how hello app uses data from hello OBD adapter and about options for creating your own OBD device in section 2.1, "IoT Devices," in hello [MyDriving Reference Guide](http://aka.ms/mydrivingdocs).</span></span>

## <a name="use-hello-app"></a><span data-ttu-id="90701-177">Hello alkalmazás használata</span><span class="sxs-lookup"><span data-stu-id="90701-177">Use hello app</span></span>
<span data-ttu-id="90701-178">Hello alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="90701-178">Start hello app.</span></span> <span data-ttu-id="90701-179">Egy kezdeti gyors üzembe helyezés toowalk van annak működéséről nyújt.</span><span class="sxs-lookup"><span data-stu-id="90701-179">There’s an initial Quickstart toowalk you through how it works.</span></span>

### <a name="track-your-trips"></a><span data-ttu-id="90701-180">Nyomon követheti a való adatváltások számát</span><span class="sxs-lookup"><span data-stu-id="90701-180">Track your trips</span></span>
<span data-ttu-id="90701-181">Koppintson a hello rekord gombra (nagy piros kör üdvözlő képernyőt hello alján) toostart egy út, és koppintson újra tooend.</span><span class="sxs-lookup"><span data-stu-id="90701-181">Tap hello record button (big red circle at hello bottom of hello screen) toostart a trip, and tap again tooend.</span></span>

![Hello rekord gombra nyomon követését utazás ábrája](./media/iot-solution-get-started/image2.png)

<span data-ttu-id="90701-183">Egy út minden egyes indításakor nincs OBD eszköz esetén kéri toouse hello szimulátor gombra.</span><span class="sxs-lookup"><span data-stu-id="90701-183">Each time you start a trip, if there’s no OBD device, you’ll be asked if you want toouse hello simulator.</span></span>

<span data-ttu-id="90701-184">Egy út hello végén koppintson a hello Leállítás gombra, és összefoglaló információk.</span><span class="sxs-lookup"><span data-stu-id="90701-184">At hello end of a trip, tap hello stop button, and you get a summary.</span></span>

![Egy összegző út – példa](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a><span data-ttu-id="90701-186">Tekintse át a való adatváltások számát</span><span class="sxs-lookup"><span data-stu-id="90701-186">Review your trips</span></span>
![Egy korábbi út – példa](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a><span data-ttu-id="90701-188">Tekintse át a profil</span><span class="sxs-lookup"><span data-stu-id="90701-188">Review your profile</span></span>
![A vezetés stílusú profil – példa](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a><span data-ttu-id="90701-190">Teszt visszajelzését</span><span class="sxs-lookup"><span data-stu-id="90701-190">Send us your test feedback</span></span>
<span data-ttu-id="90701-191">A saját IoT rendszerek létrehozott MyDriving toohelp ismertető, mert biztosan szeretnénk toohear az Ön kapcsolatos mennyire működik.</span><span class="sxs-lookup"><span data-stu-id="90701-191">Because we created MyDriving toohelp jumpstart your own IoT systems, we certainly want toohear from you about how well it works.</span></span> <span data-ttu-id="90701-192">Értesítsen minket, ha:</span><span class="sxs-lookup"><span data-stu-id="90701-192">Let us know if:</span></span>

* <span data-ttu-id="90701-193">Nehézségek vagy kihívásokkal futtatja.</span><span class="sxs-lookup"><span data-stu-id="90701-193">You run into difficulties or challenges.</span></span>
* <span data-ttu-id="90701-194">Van így könnyebben megfelelőbbek tooyour forgatókönyv bővítmény pont.</span><span class="sxs-lookup"><span data-stu-id="90701-194">There is an extension point that would make it more suitable tooyour scenario.</span></span>
* <span data-ttu-id="90701-195">Egy sokkal hatékonyabb módja tooaccomplish található egyes igényeinek.</span><span class="sxs-lookup"><span data-stu-id="90701-195">You find a more efficient way tooaccomplish certain needs.</span></span>
* <span data-ttu-id="90701-196">Javaslatai bármely más MyDriving vagy ebben a dokumentációban javítására.</span><span class="sxs-lookup"><span data-stu-id="90701-196">You have any other suggestions for improving MyDriving or this documentation.</span></span>

<span data-ttu-id="90701-197">Hello MyDriving App magát, használhatja a hello beépített Hockeyappra visszajelzés mechanizmus: iOS és Android rendszeren csak biztosítják a telefon egy rázó, vagy használjon hello **visszajelzés** menüparancshoz.</span><span class="sxs-lookup"><span data-stu-id="90701-197">Within hello MyDriving app itself, you can use hello built-in HockeyApp feedback mechanism: on iOS and Android, just give your phone a shake, or use hello **Feedback** menu command.</span></span> <span data-ttu-id="90701-198">Ez automatikusan elvégzi a képernyőfelvételen látható, hogy tudjuk lesz, mi, hogy van szó.</span><span class="sxs-lookup"><span data-stu-id="90701-198">This will automatically attach a screenshot, so that we’ll know what you’re talking about.</span></span> <span data-ttu-id="90701-199">És bármely kellemetlen összeomlás esetén Hockeyappra gyűjt hello összeomlási naplókat tootell velünk róluk.</span><span class="sxs-lookup"><span data-stu-id="90701-199">And if there are any unfortunate crashes, HockeyApp collects hello crash logs tootell us about them.</span></span> <span data-ttu-id="90701-200">Is adhat a visszajelzést keresztül hello [Hockeyappra portal].</span><span class="sxs-lookup"><span data-stu-id="90701-200">You can also give feedback through hello [HockeyApp portal].</span></span>

<span data-ttu-id="90701-201">Is fájl egy [problémát a Githubon], vagy hagyja meg az alábbi megjegyzést (en-us edition).</span><span class="sxs-lookup"><span data-stu-id="90701-201">You can also file an [issue on GitHub], or leave a comment below (en-us edition).</span></span>

<span data-ttu-id="90701-202">Számítunk toohearing az Ön!</span><span class="sxs-lookup"><span data-stu-id="90701-202">We look forward toohearing from you!</span></span>

## <a name="next-steps"></a><span data-ttu-id="90701-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90701-203">Next steps</span></span>
* <span data-ttu-id="90701-204">Fedezze fel hello [MyDriving a referencia-útmutató](http://aka.ms/mydrivingdocs) toounderstand hogyan előre tervezett és a beépített hello teljes MyDriving rendszer.</span><span class="sxs-lookup"><span data-stu-id="90701-204">Explore hello [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) toounderstand how we’ve designed and built hello entire MyDriving system.</span></span>
* <span data-ttu-id="90701-205">[Létrehozásakor és központi telepítésekor a rendszer a saját](iot-solution-build-system.md) az Azure Resource Manager-parancsfájlok használatával.</span><span class="sxs-lookup"><span data-stu-id="90701-205">[Create and deploy a system of your own](iot-solution-build-system.md) by using our Azure Resource Manager scripts.</span></span> <span data-ttu-id="90701-206">Hello [MyDriving használati útmutató](http://aka.ms/mydrivingdocs) is végigvezeti azokon a területeken, ahol lesz testreszabásokat hello legtöbb.</span><span class="sxs-lookup"><span data-stu-id="90701-206">hello [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) also guides you through areas where you’ll make hello most customizations.</span></span>

[a Githubról]: https://github.com/Azure-Samples/MyDriving
[Xamarin segítségével]: https://developer.xamarin.com/guides/ios/getting_started/installation/
[BAFX termékek 34t5 Bluetooth OBDII vizsgálati eszköz]: http://www.amazon.com/gp/product/B005NLQAHS
[ScanTool OBDLink MX Wi-Fi: OBD Adapter/diagnosztikai képolvasó]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
[Hockeyappra portal]: https://rink.hockeyapp.org
[problémát a Githubon]: https://github.com/Azure-Samples/MyDriving/issues
