---
title: "aaaUse hello Azure portál toocreate az IoT-központ |} Microsoft Docs"
description: "Hogyan toocreate, kezelése és törlése az Azure IoT hub hello Azure-portálon keresztül. Tarifacsomagok, a méretezés, a biztonsági, és a konfigurációs üzenetküldésre kapcsolatos adatokat tartalmaz."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0909cd2b-4c1e-49e0-b68a-75532caf0a6a
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: 383968c90ee7ef3bff85a6c90efbf5f0e8fbb208
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-portal"></a><span data-ttu-id="ab04f-104">Létrehoz egy IoT-központot hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="ab04f-104">Create an IoT hub using hello Azure portal</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="ab04f-105">Ez a cikk ismerteti:</span><span class="sxs-lookup"><span data-stu-id="ab04f-105">This article describes:</span></span>

* <span data-ttu-id="ab04f-106">Hogyan toofind hello hello Azure-portálon az IoT-központ szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ab04f-106">How toofind hello IoT Hub service in hello Azure portal.</span></span>
* <span data-ttu-id="ab04f-107">Hogyan toocreate és az IoT-központok kezelése.</span><span class="sxs-lookup"><span data-stu-id="ab04f-107">How toocreate and manage IoT hubs.</span></span>

## <a name="where-toofind-hello-iot-hub-service"></a><span data-ttu-id="ab04f-108">Ha toofind hello IoT-központ szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ab04f-108">Where toofind hello IoT Hub service</span></span>

<span data-ttu-id="ab04f-109">Az alábbi helyek hello portálon hello hello IoT-központ szolgáltatás található:</span><span class="sxs-lookup"><span data-stu-id="ab04f-109">You can find hello IoT Hub service in hello following locations in hello portal:</span></span>

* <span data-ttu-id="ab04f-110">Válasszon **+ új**, majd válassza a **az eszközök internetes hálózatát**.</span><span class="sxs-lookup"><span data-stu-id="ab04f-110">Choose **+ New**, then choose **Internet of Things**.</span></span>
* <span data-ttu-id="ab04f-111">Hello piactér, válassza a **az eszközök internetes hálózatát**.</span><span class="sxs-lookup"><span data-stu-id="ab04f-111">In hello Marketplace, choose **Internet of Things**.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="ab04f-112">IoT Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab04f-112">Create an IoT hub</span></span>

<span data-ttu-id="ab04f-113">Létrehozhat az IoT-központ a következő módszerek hello használata:</span><span class="sxs-lookup"><span data-stu-id="ab04f-113">You can create an IoT hub using hello following methods:</span></span>

* <span data-ttu-id="ab04f-114">Hello **+ új** beállítás látható a következő képernyőfelvétel hello hello paneljének megnyitása.</span><span class="sxs-lookup"><span data-stu-id="ab04f-114">hello **+ New** option opens hello blade shown in hello following screen shot.</span></span> <span data-ttu-id="ab04f-115">hello IoT-központ létrehozásának, ezzel a metódussal és hello piactéren keresztül hello lépései megegyeznek.</span><span class="sxs-lookup"><span data-stu-id="ab04f-115">hello steps for creating hello IoT hub through this method and through hello marketplace are identical.</span></span>
* <span data-ttu-id="ab04f-116">Hello piactér, válassza a **létrehozása** tooopen hello panel hello a következő képernyőfelvételen látható.</span><span class="sxs-lookup"><span data-stu-id="ab04f-116">In hello Marketplace, choose **Create** tooopen hello blade shown in hello following screen shot.</span></span>

<span data-ttu-id="ab04f-117">hello következő részek a hello több lépéseket toocreate az IoT-központ:</span><span class="sxs-lookup"><span data-stu-id="ab04f-117">hello following sections describe hello several steps toocreate an IoT hub:</span></span>

### <a name="choose-hello-name-of-hello-iot-hub"></a><span data-ttu-id="ab04f-118">Válassza ki az IoT-központ hello hello neve</span><span class="sxs-lookup"><span data-stu-id="ab04f-118">Choose hello name of hello IoT hub</span></span>

<span data-ttu-id="ab04f-119">az IoT-központ toocreate, adjon nevet az IoT-központ hello.</span><span class="sxs-lookup"><span data-stu-id="ab04f-119">toocreate an IoT hub, you must name hello IoT hub.</span></span> <span data-ttu-id="ab04f-120">Ez a név minden IoT-központok között egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ab04f-120">This name must be unique across all IoT hubs.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-hello-pricing-tier"></a><span data-ttu-id="ab04f-121">Hello tarifacsomag kiválasztása</span><span class="sxs-lookup"><span data-stu-id="ab04f-121">Choose hello pricing tier</span></span>

<span data-ttu-id="ab04f-122">Négy szintek közül választhat: **szabad**, **szabványos 1** és **szabványos 2**, és **Standard S3**.</span><span class="sxs-lookup"><span data-stu-id="ab04f-122">You can choose from four tiers: **Free**, **Standard 1** and **Standard 2**, and **Standard S3**.</span></span> <span data-ttu-id="ab04f-123">ingyenes szint hello lehetővé teszi, hogy csak 500 eszközök toobe csatlakozott toohello IoT-központot, illetve a hierarchiában felfelé too8, 000 üzenet naponta.</span><span class="sxs-lookup"><span data-stu-id="ab04f-123">hello free tier allows only 500 devices toobe connected toohello IoT hub and up too8,000 messages per day.</span></span>

<span data-ttu-id="ab04f-124">**Standard szintű, S1**: hello S1 edition használja, hogy mindegyik generál kis mennyiségű adatokat eszközök nagy számú IoT-megoldások.</span><span class="sxs-lookup"><span data-stu-id="ab04f-124">**Standard S1**: Use hello S1 edition for IoT solutions with a large number of devices that each generate small amounts of data.</span></span> <span data-ttu-id="ab04f-125">Hello S1 kiadás tárolóegységekhez lehetővé teszi, hogy másolatot too400, az összes csatlakoztatott eszközön naponta 000 üzenetek.</span><span class="sxs-lookup"><span data-stu-id="ab04f-125">Each unit of hello S1 edition allows up too400,000 messages per day across all connected devices.</span></span>

<span data-ttu-id="ab04f-126">**Standard S2**: hello S2 edition használja, amelyben eszközök nagy mennyiségű adat készítése az IoT-megoldások.</span><span class="sxs-lookup"><span data-stu-id="ab04f-126">**Standard S2**: Use hello S2 edition for IoT solutions in which devices generate large amounts of data.</span></span> <span data-ttu-id="ab04f-127">Egyes Munkaegységek hello S2 edition lehetővé teszi, hogy másolatot too6 millió üzenetek / nap közötti minden csatlakoztatott eszközön.</span><span class="sxs-lookup"><span data-stu-id="ab04f-127">Each unit of hello S2 edition allows up too6 million messages per day between all connected devices.</span></span>

<span data-ttu-id="ab04f-128">**Standard S3**: hello S3 edition használja, amely nagy mennyiségű adat az IoT-megoldások.</span><span class="sxs-lookup"><span data-stu-id="ab04f-128">**Standard S3**: Use hello S3 edition for IoT solutions that generate large amounts of data.</span></span> <span data-ttu-id="ab04f-129">Egyes Munkaegységek hello S3 edition lehetővé teszi, hogy másolatot too300 millió üzenetek / nap közötti minden csatlakoztatott eszközön.</span><span class="sxs-lookup"><span data-stu-id="ab04f-129">Each unit of hello S3 edition allows up too300 million messages per day between all connected devices.</span></span>

![][4]

> [!NOTE]
> <span data-ttu-id="ab04f-130">Az IoT-központ csak egy ingyenes hub / Azure-előfizetés teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="ab04f-130">IoT Hub only allows one free hub per Azure subscription.</span></span>

### <a name="iot-hub-units"></a><span data-ttu-id="ab04f-131">IoT hub-egységek</span><span class="sxs-lookup"><span data-stu-id="ab04f-131">IoT hub units</span></span>

<span data-ttu-id="ab04f-132">napi egység értesítésenként engedélyezett üzenetek hello száma a központ tarifacsomag függ.</span><span class="sxs-lookup"><span data-stu-id="ab04f-132">hello number of messages allowed per unit per day depends on your hub's pricing tier.</span></span> <span data-ttu-id="ab04f-133">Például ha azt szeretné, hogy az IoT hub toosupport érkező üzenetek 700 000 hello, választja két S1 réteg egység.</span><span class="sxs-lookup"><span data-stu-id="ab04f-133">For example, if you want hello IoT hub toosupport ingress of 700,000 messages, you choose two S1 tier units.</span></span>

### <a name="device-toocloud-partitions-and-resource-group"></a><span data-ttu-id="ab04f-134">Eszköz toocloud partíciók és erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="ab04f-134">Device toocloud partitions and resource group</span></span>

<span data-ttu-id="ab04f-135">Az IoT-központ tároló partíciók száma hello módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="ab04f-135">You can change hello number of partitions for an IoT hub.</span></span> <span data-ttu-id="ab04f-136">partíciók száma hello alapértelmezett érték 4, akkor egy másik számot hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="ab04f-136">hello default number of partitions is 4, you can choose a different number from hello drop-down list.</span></span>

<span data-ttu-id="ab04f-137">Nem kell tooexplicitly hozzon létre egy üres erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="ab04f-137">You do not need tooexplicitly create an empty resource group.</span></span> <span data-ttu-id="ab04f-138">Amikor létrehoz egy erőforrást, vagy új toocreate válasszon, vagy használjon egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="ab04f-138">When you create a resource, you can choose either toocreate a new, or use an existing resource group.</span></span>

![][5]

### <a name="choose-subscription"></a><span data-ttu-id="ab04f-139">Válassza ki az előfizetést</span><span class="sxs-lookup"><span data-stu-id="ab04f-139">Choose subscription</span></span>

<span data-ttu-id="ab04f-140">Az Azure IoT Hub automatikusan listák hello Azure-előfizetések hello felhasználói fiók van csatolva.</span><span class="sxs-lookup"><span data-stu-id="ab04f-140">Azure IoT Hub automatically lists hello Azure subscriptions hello user account is linked to.</span></span> <span data-ttu-id="ab04f-141">Kiválaszthatja a hello Azure-előfizetés tooassociate hello IoT-központ számára.</span><span class="sxs-lookup"><span data-stu-id="ab04f-141">You can choose hello Azure subscription tooassociate hello IoT hub to.</span></span>

### <a name="choose-hello-location"></a><span data-ttu-id="ab04f-142">Hello helyének kiválasztása</span><span class="sxs-lookup"><span data-stu-id="ab04f-142">Choose hello location</span></span>

<span data-ttu-id="ab04f-143">hello hely beállítás hello régiókban, ahol az IoT-központ lehetőség a listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ab04f-143">hello location option provides a list of hello regions where IoT Hub is available.</span></span>

### <a name="create-hello-iot-hub"></a><span data-ttu-id="ab04f-144">Hello IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab04f-144">Create hello IoT hub</span></span>

<span data-ttu-id="ab04f-145">Ha az összes előző lépést, hello IoT-központ is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="ab04f-145">When all previous steps are complete, you can create hello IoT hub.</span></span> <span data-ttu-id="ab04f-146">Kattintson a **létrehozása** toostart háttérfolyamatot toocreate hello, és úgy döntött, hogy hello beállításokkal hello IoT-központ telepítése.</span><span class="sxs-lookup"><span data-stu-id="ab04f-146">Click **Create** toostart hello back-end process toocreate and deploy hello IoT hub with hello options you chose.</span></span>

<span data-ttu-id="ab04f-147">Mivel némi időre van a háttér-telepítési toorun hello hello megfelelő helyet kiszolgálókon is igénybe vehet néhány percet toocreate hello IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="ab04f-147">It can take a few minutes toocreate hello IoT hub as it takes time for hello back-end deployment toorun on hello appropriate location servers.</span></span>

## <a name="change-hello-settings-of-hello-iot-hub"></a><span data-ttu-id="ab04f-148">Az IoT-központ hello hello beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="ab04f-148">Change hello settings of hello IoT hub</span></span>

<span data-ttu-id="ab04f-149">Módosíthatja egy meglévő IoT-központot hello beállításainak hello IoT Hub panel a létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="ab04f-149">You can change hello settings of an existing IoT hub after it is created from hello IoT Hub blade.</span></span>

![][8]

<span data-ttu-id="ab04f-150">**Megosztott hozzáférési házirendek**: ezek a házirendek eszközökön és szolgáltatásokon tooconnect tooIoT Hub hello engedélyeinek megadása.</span><span class="sxs-lookup"><span data-stu-id="ab04f-150">**Shared access policies**: These policies define hello permissions for devices and services tooconnect tooIoT Hub.</span></span> <span data-ttu-id="ab04f-151">Ezek a házirendek eléréséhez kattintson **megosztott elérési házirendek** alatt **általános**.</span><span class="sxs-lookup"><span data-stu-id="ab04f-151">You can access these policies by clicking **Shared access policies** under **General**.</span></span> <span data-ttu-id="ab04f-152">Ezen a panelen módosíthatja a meglévő házirendeket, vagy adjon hozzá egy új házirendet.</span><span class="sxs-lookup"><span data-stu-id="ab04f-152">In this blade, you can either modify existing policies or add a new policy.</span></span>

### <a name="create-a-policy"></a><span data-ttu-id="ab04f-153">Házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab04f-153">Create a policy</span></span>

* <span data-ttu-id="ab04f-154">Kattintson a **Hozzáadás** tooopen egy panel.</span><span class="sxs-lookup"><span data-stu-id="ab04f-154">Click **Add** tooopen a blade.</span></span> <span data-ttu-id="ab04f-155">Itt adhatja meg hello új házirend nevét és hello engedélyeit, hogy a házirend tooassociate hello alábbi ábrán. ábra:</span><span class="sxs-lookup"><span data-stu-id="ab04f-155">Here you can enter hello new policy name and hello permissions that you want tooassociate with this policy, as shown in hello following figure:</span></span>

    <span data-ttu-id="ab04f-156">Nincsenek társítható megosztott szabályzatokról több engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="ab04f-156">There are several permissions that can be associated with these shared policies.</span></span> <span data-ttu-id="ab04f-157">Hello **beállításkulcs olvasása** és **beállításjegyzék írási** házirendek adjon olvasási és írási hozzáférési jogok toohello identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="ab04f-157">hello **Registry read** and **Registry write** policies grant read and write access rights toohello identity registry.</span></span> <span data-ttu-id="ab04f-158">A beállítás hello írási automatikusan úgy dönt, hello olvasása lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ab04f-158">Choosing hello write option automatically chooses hello read option.</span></span>

    <span data-ttu-id="ab04f-159">Hello **Service csatlakozás** házirend ad engedélyt tooaccess Szolgáltatásvégpontok például **eszközről a felhőbe kap**.</span><span class="sxs-lookup"><span data-stu-id="ab04f-159">hello **Service connect** policy grants permission tooaccess service endpoints such as **Receive device-to-cloud**.</span></span> <span data-ttu-id="ab04f-160">Hello **eszköz csatlakozzon** házirend engedélyt ad az IoT-központ eszközoldali hello végpontok használatával üzenetek küldése és fogadása.</span><span class="sxs-lookup"><span data-stu-id="ab04f-160">hello **Device connect** policy grants permissions for sending and receiving messages using hello IoT Hub device-side endpoints.</span></span>

* <span data-ttu-id="ab04f-161">Kattintson a **létrehozása** tooadd ebben az újonnan létrehozott házirend toohello meglévő listájához.</span><span class="sxs-lookup"><span data-stu-id="ab04f-161">Click **Create** tooadd this newly created policy toohello existing list.</span></span>

![][10]

## <a name="endpoints"></a><span data-ttu-id="ab04f-162">Végpontok</span><span class="sxs-lookup"><span data-stu-id="ab04f-162">Endpoints</span></span>

<span data-ttu-id="ab04f-163">Kattintson a **végpontok** toodisplay hello IoT-központ, amely módosítja a végpontok listáját.</span><span class="sxs-lookup"><span data-stu-id="ab04f-163">Click **Endpoints** toodisplay a list of endpoints for hello IoT hub that you are modifying.</span></span> <span data-ttu-id="ab04f-164">A végpontok két típusa van: hello IoT-központ beépített végpontok és, hogy a létrehozása után adja hozzá az IoT-központ toohello végpontok.</span><span class="sxs-lookup"><span data-stu-id="ab04f-164">There are two types of endpoints: endpoints that are built into hello IoT hub, and endpoints that you add toohello IoT hub after its creation.</span></span>

![][11]

### <a name="built-in-endpoints"></a><span data-ttu-id="ab04f-165">Beépített végpontok</span><span class="sxs-lookup"><span data-stu-id="ab04f-165">Built-in endpoints</span></span>

<span data-ttu-id="ab04f-166">Nincsenek a két beépített végpont: **toodevice visszajelzés Cloud** és **események**.</span><span class="sxs-lookup"><span data-stu-id="ab04f-166">There are two built-in endpoints: **Cloud toodevice feedback** and **Events**.</span></span>

* <span data-ttu-id="ab04f-167">**Felhőalapú toodevice visszajelzés** beállítások: Ezzel a beállítással rendelkezik két subsettings: **tooDevice TTL Cloud** (idő TTL) és **megőrzési idő** (órákban) hello üzenetek.</span><span class="sxs-lookup"><span data-stu-id="ab04f-167">**Cloud toodevice feedback** settings: This setting has two subsettings: **Cloud tooDevice TTL** (time-to-live) and **Retention time** (in hours) for hello messages.</span></span> <span data-ttu-id="ab04f-168">Az első létrehoz egy IoT-központot, mindkét ezeket a beállításokat kell egy óra hello alapértelmezett értékét.</span><span class="sxs-lookup"><span data-stu-id="ab04f-168">When your first create an IoT hub, both these settings have hello default value of one hour.</span></span> <span data-ttu-id="ab04f-169">tooadjust ezeket a beállításokat hello csúszkákkal, vagy írja be a hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="ab04f-169">tooadjust these settings, use hello sliders or type hello values.</span></span>
* <span data-ttu-id="ab04f-170">**Események** beállítások: Ezzel a beállítással rendelkezik több subsettings, amelyek némelyike csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="ab04f-170">**Events** settings: This setting has several subsettings, some of which are read-only.</span></span> <span data-ttu-id="ab04f-171">a következő lista hello ezeket a beállításokat mutatja be:</span><span class="sxs-lookup"><span data-stu-id="ab04f-171">hello following list describes these settings:</span></span>

  * <span data-ttu-id="ab04f-172">**Partíciók**: egy alapértelmezett értéke hello IoT-központ létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="ab04f-172">**Partitions**: A default value is set when hello IoT hub is created.</span></span> <span data-ttu-id="ab04f-173">Ez a beállítás a partíciók száma hello módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="ab04f-173">You can change hello number of partitions through this setting.</span></span>

  * <span data-ttu-id="ab04f-174">**Event Hub-kompatibilis nevének és végpontjának**: hello IoT-központ létrehozásakor az Eseményközpont létrehozásakor belső hogy előfordulhat, hogy kell-e hozzáférési toounder bizonyos körülmények között.</span><span class="sxs-lookup"><span data-stu-id="ab04f-174">**Event Hub-compatible name and endpoint**: When hello IoT hub is created, an Event Hub is created internally that you may need access toounder certain circumstances.</span></span> <span data-ttu-id="ab04f-175">Nem szabhatja testre a hello Event Hub-kompatibilis és a végpont értékét, de kattintva másolhatja **másolási**.</span><span class="sxs-lookup"><span data-stu-id="ab04f-175">You cannot customize hello Event Hub-compatible name and endpoint values but you can copy them by clicking **Copy**.</span></span>

  * <span data-ttu-id="ab04f-176">**Megőrzési idő**: tooone nap meg alapértelmezés szerint azonban hello legördülő lista használatával módosíthat.</span><span class="sxs-lookup"><span data-stu-id="ab04f-176">**Retention Time**: Set tooone day by default but you can change it using hello drop-down list.</span></span> <span data-ttu-id="ab04f-177">Ez az érték nap hello eszközről a felhőbe beállítás van.</span><span class="sxs-lookup"><span data-stu-id="ab04f-177">This value is in days for hello device-to-cloud setting.</span></span>

  * <span data-ttu-id="ab04f-178">**Felhasználói csoportok**: felhasználói csoportok lehetővé teszik az több olvasók tooread üzenetet az IoT-központ hello től függetlenül.</span><span class="sxs-lookup"><span data-stu-id="ab04f-178">**Consumer Groups**: Consumer groups enable multiple readers tooread messages independently from hello IoT hub.</span></span> <span data-ttu-id="ab04f-179">Minden IoT-központot egy alapértelmezett felhasználói csoport jön létre.</span><span class="sxs-lookup"><span data-stu-id="ab04f-179">Every IoT hub is created with a default consumer group.</span></span> <span data-ttu-id="ab04f-180">Azonban hozzáadása vagy törlése a fogyasztói csoportok tooyour IoT-központok ezt a beállítást használja.</span><span class="sxs-lookup"><span data-stu-id="ab04f-180">However, you can add or delete consumer groups tooyour IoT hubs using this setting.</span></span>

  > [!NOTE]
  > <span data-ttu-id="ab04f-181">hello alapértelmezett felhasználói csoport nem szerkeszthető, és nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="ab04f-181">hello default consumer group cannot be edited or deleted.</span></span>

### <a name="custom-endpoints"></a><span data-ttu-id="ab04f-182">Egyéni végpontokat</span><span class="sxs-lookup"><span data-stu-id="ab04f-182">Custom endpoints</span></span>

<span data-ttu-id="ab04f-183">Egyéni végpontokat az IoT hub hello portál használatával is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="ab04f-183">You can add custom endpoints on your IoT hub using hello portal.</span></span> <span data-ttu-id="ab04f-184">A hello **végpontok** panelen kattintson **Hozzáadás** : hello felső tooopen hello **végpont hozzáadása** panel.</span><span class="sxs-lookup"><span data-stu-id="ab04f-184">From hello **Endpoints** blade, click **Add** at hello top tooopen hello **Add endpoint** blade.</span></span> <span data-ttu-id="ab04f-185">Adja meg a hello szükséges információkat, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="ab04f-185">Enter hello required information, then click **OK**.</span></span> <span data-ttu-id="ab04f-186">Az egyéni végpontjának megjelenik a fő hello **végpontok** panelen.</span><span class="sxs-lookup"><span data-stu-id="ab04f-186">Your custom endpoint is now listed in hello main **Endpoints** blade.</span></span>

![][13]

<span data-ttu-id="ab04f-187">További tudnivalók az egyéni végpontokat [referencia - IoT-központok végpontjai][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="ab04f-187">You can read more about custom endpoints in [Reference - IoT hub endpoints][lnk-devguide-endpoints].</span></span>

## <a name="routes"></a><span data-ttu-id="ab04f-188">Útvonalak</span><span class="sxs-lookup"><span data-stu-id="ab04f-188">Routes</span></span>

<span data-ttu-id="ab04f-189">Kattintson a **útvonalak** toomanage hogyan IoT-központ kiszállítja az eszközről a felhőbe üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="ab04f-189">Click **Routes** toomanage how IoT Hub dispatches your device-to-cloud messages.</span></span>

![][14]

<span data-ttu-id="ab04f-190">Útvonalak tooyour IoT-központ kattintva vehet fel **Hozzáadás** hello hello tetején **útvonalak*** hello szükséges adatok bevitele, és kattintson a panel **OK**.</span><span class="sxs-lookup"><span data-stu-id="ab04f-190">You can add routes tooyour IoT hub by clicking **Add** at hello top of hello **Routes*** blade, entering hello required information, and clicking **OK**.</span></span> <span data-ttu-id="ab04f-191">Az útvonal majd szerepel a fő hello **útvonalak** panelen.</span><span class="sxs-lookup"><span data-stu-id="ab04f-191">Your route is then listed in hello main **Routes** blade.</span></span> <span data-ttu-id="ab04f-192">Útvonalak listájában hello kattintva szerkesztheti egy útvonalat.</span><span class="sxs-lookup"><span data-stu-id="ab04f-192">You can edit a route by clicking it in hello list of routes.</span></span> <span data-ttu-id="ab04f-193">tooenable egy útvonalat, kattintson az útvonalak hello listában, és állítsa be a hello **engedélyezve** túl váltása**ki**.</span><span class="sxs-lookup"><span data-stu-id="ab04f-193">tooenable a route, click it in hello list of routes and set hello **Enabled** toggle too**Off**.</span></span> <span data-ttu-id="ab04f-194">toosave hello módosítása, kattintson a **OK** hello hello panel alsó részén.</span><span class="sxs-lookup"><span data-stu-id="ab04f-194">toosave hello change, click **OK** at hello bottom of hello blade.</span></span>

![][15]

## <a name="pricing-and-scale"></a><span data-ttu-id="ab04f-195">Díjszabás és méretezés</span><span class="sxs-lookup"><span data-stu-id="ab04f-195">Pricing and scale</span></span>

<span data-ttu-id="ab04f-196">egy meglévő IoT-központot árképzése hello hello keresztül módosítható **árazás** beállításait, a következő kivételek hello:</span><span class="sxs-lookup"><span data-stu-id="ab04f-196">hello pricing of an existing IoT hub can be changed through hello **Pricing** settings, with hello following exceptions:</span></span>

* <span data-ttu-id="ab04f-197">Hello aktuális megvalósításban egy IoT hubot szabad Termékváltozat nem módosítható rétegek tooone SKU, fizetős hello, vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="ab04f-197">In hello current implementation, an IoT hub with a free SKU cannot change tiers tooone of hello paid SKUs, or vice versa.</span></span>
* <span data-ttu-id="ab04f-198">Csak lehet egy ingyenes szint IoT-központ hello Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="ab04f-198">There can only be one free tier IoT hub in hello Azure subscription.</span></span>

![][12]

<span data-ttu-id="ab04f-199">Áthelyezheti a magasabb szintű toolower használható csak akkor, ha adott napon küldött üzenetek hello száma meghaladja a hello kvóta hello alacsonyabb szinten.</span><span class="sxs-lookup"><span data-stu-id="ab04f-199">You can move from a higher toolower tier only when hello number of messages sent that day do exceed hello quota for hello lower tier.</span></span> <span data-ttu-id="ab04f-200">Például ha naponta üzenetek hello száma meghaladja a 400000, majd hello réteget a hello IoT hub módosítható.</span><span class="sxs-lookup"><span data-stu-id="ab04f-200">For example, if hello number of messages per day exceeds 400,000, then hello tier for hello IoT hub can be changed.</span></span> <span data-ttu-id="ab04f-201">Azonban toohello S1 réteg módosítása ezután hello IoT-központ szabályozva van napra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="ab04f-201">However, if you change toohello S1 tier then hello IoT hub is throttled for that day.</span></span>

## <a name="delete-hello-iot-hub"></a><span data-ttu-id="ab04f-202">Az IoT-központ hello törlése</span><span class="sxs-lookup"><span data-stu-id="ab04f-202">Delete hello IoT hub</span></span>

<span data-ttu-id="ab04f-203">Toohello IoT-központ toodelete kívánt kattintva tallózással is kikeresheti **Tallózás**, és megfelelő hub toodelete lehetőséget választja hello.</span><span class="sxs-lookup"><span data-stu-id="ab04f-203">You can browse toohello IoT hub you want toodelete by clicking **Browse**, and then choosing hello appropriate hub toodelete.</span></span> <span data-ttu-id="ab04f-204">toodelete hello IoT-központot, kattintson a hello **törlése** hello IoT-központnév gombra.</span><span class="sxs-lookup"><span data-stu-id="ab04f-204">toodelete hello IoT hub, click hello **Delete** button below hello IoT hub name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab04f-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ab04f-205">Next steps</span></span>

<span data-ttu-id="ab04f-206">Kövesse az alábbi hivatkozások toolearn Azure IoT Hub kezelésével kapcsolatos további:</span><span class="sxs-lookup"><span data-stu-id="ab04f-206">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="ab04f-207">[Tömeges az IoT-eszközök kezelése][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="ab04f-207">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="ab04f-208">[Az IoT-központ metrikák][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="ab04f-208">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="ab04f-209">[Figyelési műveletek][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="ab04f-209">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="ab04f-210">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="ab04f-210">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="ab04f-211">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="ab04f-211">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="ab04f-212">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="ab04f-212">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="ab04f-213">[Az IoT-megoldásból hello szabad a biztonságos][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="ab04f-213">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

[4]: ./media/iot-hub-create-through-portal/create-iothub.png
[5]: ./media/iot-hub-create-through-portal/location1.png
[8]: ./media/iot-hub-create-through-portal/portal-settings.png
[10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
[11]: ./media/iot-hub-create-through-portal/messaging-settings.png
[12]: ./media/iot-hub-create-through-portal/pricing-error.png
[13]: ./media/iot-hub-create-through-portal/endpoint-creation.png
[14]: ./media/iot-hub-create-through-portal/routes-list.png
[15]: ./media/iot-hub-create-through-portal/route-edit.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
