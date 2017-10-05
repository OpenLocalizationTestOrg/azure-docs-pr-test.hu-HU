---
title: "IoT Hub létrehozása az Azure-portál használatával |} Microsoft Docs"
description: "Hogyan létrehozására, kezelésére és törlése az Azure IoT hub az Azure portálon keresztül. Tarifacsomagok, a méretezés, a biztonsági, és a konfigurációs üzenetküldésre kapcsolatos adatokat tartalmaz."
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
ms.openlocfilehash: bca7eea5f44bbed3b784b56edaac235161b43e5e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a><span data-ttu-id="2852e-104">Létrehoz egy IoT-központot, az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="2852e-104">Create an IoT hub using the Azure portal</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

<span data-ttu-id="2852e-105">Ez a cikk ismerteti:</span><span class="sxs-lookup"><span data-stu-id="2852e-105">This article describes:</span></span>

* <span data-ttu-id="2852e-106">Hol találhatók az IoT-központ szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="2852e-106">How to find the IoT Hub service in the Azure portal.</span></span>
* <span data-ttu-id="2852e-107">Megtudhatja, hogyan hozhatja létre és kezelheti az IoT-központok.</span><span class="sxs-lookup"><span data-stu-id="2852e-107">How to create and manage IoT hubs.</span></span>

## <a name="where-to-find-the-iot-hub-service"></a><span data-ttu-id="2852e-108">Hol található az IoT-központ szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="2852e-108">Where to find the IoT Hub service</span></span>

<span data-ttu-id="2852e-109">Az IoT-központ szolgáltatás a következő helyeken a portálon találja meg:</span><span class="sxs-lookup"><span data-stu-id="2852e-109">You can find the IoT Hub service in the following locations in the portal:</span></span>

* <span data-ttu-id="2852e-110">Válasszon **+ új**, majd válassza a **az eszközök internetes hálózatát**.</span><span class="sxs-lookup"><span data-stu-id="2852e-110">Choose **+ New**, then choose **Internet of Things**.</span></span>
* <span data-ttu-id="2852e-111">Válassza ki a piactér **az eszközök internetes hálózatát**.</span><span class="sxs-lookup"><span data-stu-id="2852e-111">In the Marketplace, choose **Internet of Things**.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="2852e-112">IoT Hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="2852e-112">Create an IoT hub</span></span>

<span data-ttu-id="2852e-113">Az IoT-központ a következő módszerekkel hozhat létre:</span><span class="sxs-lookup"><span data-stu-id="2852e-113">You can create an IoT hub using the following methods:</span></span>

* <span data-ttu-id="2852e-114">A **+ új** lehetőség megnyitja a panel az alábbi képernyőfelvételen látható.</span><span class="sxs-lookup"><span data-stu-id="2852e-114">The **+ New** option opens the blade shown in the following screen shot.</span></span> <span data-ttu-id="2852e-115">Az IoT hub létrehozása ezzel a módszerrel, és a piactéren keresztül lépései megegyeznek.</span><span class="sxs-lookup"><span data-stu-id="2852e-115">The steps for creating the IoT hub through this method and through the marketplace are identical.</span></span>
* <span data-ttu-id="2852e-116">Válassza ki a piactér **létrehozása** az alábbi képernyőfelvételen látható panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2852e-116">In the Marketplace, choose **Create** to open the blade shown in the following screen shot.</span></span>

<span data-ttu-id="2852e-117">A következő szakaszok ismertetik a számos lépést kelljen IoT hub létrehozása:</span><span class="sxs-lookup"><span data-stu-id="2852e-117">The following sections describe the several steps to create an IoT hub:</span></span>

### <a name="choose-the-name-of-the-iot-hub"></a><span data-ttu-id="2852e-118">Válassza ki az IoT hub nevét</span><span class="sxs-lookup"><span data-stu-id="2852e-118">Choose the name of the IoT hub</span></span>

<span data-ttu-id="2852e-119">Létrehoz egy IoT-központot, adjon nevet az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="2852e-119">To create an IoT hub, you must name the IoT hub.</span></span> <span data-ttu-id="2852e-120">Ez a név minden IoT-központok között egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2852e-120">This name must be unique across all IoT hubs.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-the-pricing-tier"></a><span data-ttu-id="2852e-121">A tarifacsomag választható</span><span class="sxs-lookup"><span data-stu-id="2852e-121">Choose the pricing tier</span></span>

<span data-ttu-id="2852e-122">Négy szintek közül választhat: **szabad**, **szabványos 1** és **szabványos 2**, és **Standard S3**.</span><span class="sxs-lookup"><span data-stu-id="2852e-122">You can choose from four tiers: **Free**, **Standard 1** and **Standard 2**, and **Standard S3**.</span></span> <span data-ttu-id="2852e-123">Ingyenes szint lehetővé teszi, hogy a csatlakoztatva kell lennie az IoT-központot, és legfeljebb 8000 üzenet naponta csak 500 eszközök.</span><span class="sxs-lookup"><span data-stu-id="2852e-123">The free tier allows only 500 devices to be connected to the IoT hub and up to 8,000 messages per day.</span></span>

<span data-ttu-id="2852e-124">**Standard szintű, S1**: S1 kiadás nagyszámú eszközöket, hogy mindegyik generál kis mennyiségű adatokat az IoT-megoldások használata.</span><span class="sxs-lookup"><span data-stu-id="2852e-124">**Standard S1**: Use the S1 edition for IoT solutions with a large number of devices that each generate small amounts of data.</span></span> <span data-ttu-id="2852e-125">Az S1 kiadás minden egységével naponta 400 ezer üzenetet lehet továbbítani az összes csatlakoztatott eszközön.</span><span class="sxs-lookup"><span data-stu-id="2852e-125">Each unit of the S1 edition allows up to 400,000 messages per day across all connected devices.</span></span>

<span data-ttu-id="2852e-126">**Standard S2**: S2 verzióját használja, amelyben eszközök nagy mennyiségű adat készítése az IoT-megoldások.</span><span class="sxs-lookup"><span data-stu-id="2852e-126">**Standard S2**: Use the S2 edition for IoT solutions in which devices generate large amounts of data.</span></span> <span data-ttu-id="2852e-127">S2 kiadás tárolóegységekhez lehetővé teszi, hogy minden csatlakoztatott eszközön közötti naponta 6 millió üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="2852e-127">Each unit of the S2 edition allows up to 6 million messages per day between all connected devices.</span></span>

<span data-ttu-id="2852e-128">**Standard S3**: S3 verzióját használja, amely nagy mennyiségű adat az IoT-megoldások.</span><span class="sxs-lookup"><span data-stu-id="2852e-128">**Standard S3**: Use the S3 edition for IoT solutions that generate large amounts of data.</span></span> <span data-ttu-id="2852e-129">S3 kiadás tárolóegységekhez lehetővé teszi, hogy akár 300 millió üzenetek / nap közötti minden csatlakoztatott eszközön.</span><span class="sxs-lookup"><span data-stu-id="2852e-129">Each unit of the S3 edition allows up to 300 million messages per day between all connected devices.</span></span>

![][4]

> [!NOTE]
> <span data-ttu-id="2852e-130">Az IoT-központ csak egy ingyenes hub / Azure-előfizetés teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="2852e-130">IoT Hub only allows one free hub per Azure subscription.</span></span>

### <a name="iot-hub-units"></a><span data-ttu-id="2852e-131">IoT hub-egységek</span><span class="sxs-lookup"><span data-stu-id="2852e-131">IoT hub units</span></span>

<span data-ttu-id="2852e-132">Napi egység értesítésenként engedélyezett üzenetek száma attól függ, hogy a központ tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="2852e-132">The number of messages allowed per unit per day depends on your hub's pricing tier.</span></span> <span data-ttu-id="2852e-133">Például ha azt szeretné, hogy az IoT hub érkező üzenetek 700 000 támogatásához, választja két S1 réteg egység.</span><span class="sxs-lookup"><span data-stu-id="2852e-133">For example, if you want the IoT hub to support ingress of 700,000 messages, you choose two S1 tier units.</span></span>

### <a name="device-to-cloud-partitions-and-resource-group"></a><span data-ttu-id="2852e-134">Erőforráscsoport és a felhőalapú eszköz</span><span class="sxs-lookup"><span data-stu-id="2852e-134">Device to cloud partitions and resource group</span></span>

<span data-ttu-id="2852e-135">Módosíthatja az IoT-központ a partíciók száma.</span><span class="sxs-lookup"><span data-stu-id="2852e-135">You can change the number of partitions for an IoT hub.</span></span> <span data-ttu-id="2852e-136">A partíciók alapértelmezett értéke 4, akkor egy másik számot a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="2852e-136">The default number of partitions is 4, you can choose a different number from the drop-down list.</span></span>

<span data-ttu-id="2852e-137">Nem kell explicit módon hozzon létre egy üres erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="2852e-137">You do not need to explicitly create an empty resource group.</span></span> <span data-ttu-id="2852e-138">Amikor létrehoz egy erőforrást, válassza ki vagy hozzon létre egy új, vagy használjon egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="2852e-138">When you create a resource, you can choose either to create a new, or use an existing resource group.</span></span>

![][5]

### <a name="choose-subscription"></a><span data-ttu-id="2852e-139">Válassza ki az előfizetést</span><span class="sxs-lookup"><span data-stu-id="2852e-139">Choose subscription</span></span>

<span data-ttu-id="2852e-140">Az Azure IoT Hub automatikusan az Azure-előfizetéseket a felhasználói fiókhoz kapcsolódó sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="2852e-140">Azure IoT Hub automatically lists the Azure subscriptions the user account is linked to.</span></span> <span data-ttu-id="2852e-141">Lehetősége van az Azure-előfizetés társítása az IoT-központ számára.</span><span class="sxs-lookup"><span data-stu-id="2852e-141">You can choose the Azure subscription to associate the IoT hub to.</span></span>

### <a name="choose-the-location"></a><span data-ttu-id="2852e-142">Válassza ki azt a helyet</span><span class="sxs-lookup"><span data-stu-id="2852e-142">Choose the location</span></span>

<span data-ttu-id="2852e-143">A hely lehetőséget a régiókban, ahol az IoT-központ lehetőség a listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2852e-143">The location option provides a list of the regions where IoT Hub is available.</span></span>

### <a name="create-the-iot-hub"></a><span data-ttu-id="2852e-144">Az IoT hub létrehozása</span><span class="sxs-lookup"><span data-stu-id="2852e-144">Create the IoT hub</span></span>

<span data-ttu-id="2852e-145">Ha az összes előző lépést, létrehozhat az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="2852e-145">When all previous steps are complete, you can create the IoT hub.</span></span> <span data-ttu-id="2852e-146">Kattintson a **létrehozása** a háttér-folyamat létrehozása és telepítése az IoT-központ a kiválasztott beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="2852e-146">Click **Create** to start the back-end process to create and deploy the IoT hub with the options you chose.</span></span>

<span data-ttu-id="2852e-147">Az IoT hub létrehozása, mivel némi időre van a háttér-központi telepítés a megfelelő hely kiszolgálójára futtatásához néhány percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="2852e-147">It can take a few minutes to create the IoT hub as it takes time for the back-end deployment to run on the appropriate location servers.</span></span>

## <a name="change-the-settings-of-the-iot-hub"></a><span data-ttu-id="2852e-148">Az IoT hub beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="2852e-148">Change the settings of the IoT hub</span></span>

<span data-ttu-id="2852e-149">Módosíthatja egy meglévő IoT-központ beállításait az IoT Hub panel a létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="2852e-149">You can change the settings of an existing IoT hub after it is created from the IoT Hub blade.</span></span>

![][8]

<span data-ttu-id="2852e-150">**Megosztott hozzáférési házirendek**: ezek a házirendek meghatározása csatlakozni az IoT Hub eszközöket és szolgáltatásokat engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="2852e-150">**Shared access policies**: These policies define the permissions for devices and services to connect to IoT Hub.</span></span> <span data-ttu-id="2852e-151">Ezek a házirendek eléréséhez kattintson **megosztott elérési házirendek** alatt **általános**.</span><span class="sxs-lookup"><span data-stu-id="2852e-151">You can access these policies by clicking **Shared access policies** under **General**.</span></span> <span data-ttu-id="2852e-152">Ezen a panelen módosíthatja a meglévő házirendeket, vagy adjon hozzá egy új házirendet.</span><span class="sxs-lookup"><span data-stu-id="2852e-152">In this blade, you can either modify existing policies or add a new policy.</span></span>

### <a name="create-a-policy"></a><span data-ttu-id="2852e-153">Házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="2852e-153">Create a policy</span></span>

* <span data-ttu-id="2852e-154">Kattintson a **Hozzáadás** egy panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2852e-154">Click **Add** to open a blade.</span></span> <span data-ttu-id="2852e-155">Itt adhatja meg az új házirend nevét és a kívánt engedélyek társítása a házirend, az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="2852e-155">Here you can enter the new policy name and the permissions that you want to associate with this policy, as shown in the following figure:</span></span>

    <span data-ttu-id="2852e-156">Nincsenek társítható megosztott szabályzatokról több engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="2852e-156">There are several permissions that can be associated with these shared policies.</span></span> <span data-ttu-id="2852e-157">A **beállításkulcs olvasása** és **beállításjegyzék írási** házirendek jogosultságokat olvasási és írási hozzáférést biztosít az identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="2852e-157">The **Registry read** and **Registry write** policies grant read and write access rights to the identity registry.</span></span> <span data-ttu-id="2852e-158">Válassza az írási automatikusan úgy dönt, olvassa el a beállítást.</span><span class="sxs-lookup"><span data-stu-id="2852e-158">Choosing the write option automatically chooses the read option.</span></span>

    <span data-ttu-id="2852e-159">A **Service csatlakozás** házirend Szolgáltatásvégpontok elérésére engedélyt **eszközről a felhőbe kap**.</span><span class="sxs-lookup"><span data-stu-id="2852e-159">The **Service connect** policy grants permission to access service endpoints such as **Receive device-to-cloud**.</span></span> <span data-ttu-id="2852e-160">A **eszköz csatlakozzon** házirend engedélyt ad az IoT-központ eszközoldali végpontokon használatával üzenetek küldése és fogadása.</span><span class="sxs-lookup"><span data-stu-id="2852e-160">The **Device connect** policy grants permissions for sending and receiving messages using the IoT Hub device-side endpoints.</span></span>

* <span data-ttu-id="2852e-161">Kattintson a **létrehozása** hozzáadása az újonnan létrehozott házirend a meglévő listájához.</span><span class="sxs-lookup"><span data-stu-id="2852e-161">Click **Create** to add this newly created policy to the existing list.</span></span>

![][10]

## <a name="endpoints"></a><span data-ttu-id="2852e-162">Végpontok</span><span class="sxs-lookup"><span data-stu-id="2852e-162">Endpoints</span></span>

<span data-ttu-id="2852e-163">Kattintson a **végpontok** az IoT hub, amely módosítja a végpontok listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2852e-163">Click **Endpoints** to display a list of endpoints for the IoT hub that you are modifying.</span></span> <span data-ttu-id="2852e-164">A végpontok két típusa van: az IoT hub beépített végpontok, és a létrehozása után az IoT hub hozzáadott végpontok.</span><span class="sxs-lookup"><span data-stu-id="2852e-164">There are two types of endpoints: endpoints that are built into the IoT hub, and endpoints that you add to the IoT hub after its creation.</span></span>

![][11]

### <a name="built-in-endpoints"></a><span data-ttu-id="2852e-165">Beépített végpontok</span><span class="sxs-lookup"><span data-stu-id="2852e-165">Built-in endpoints</span></span>

<span data-ttu-id="2852e-166">Nincsenek a két beépített végpont: **eszköz visszajelzéseire felhő** és **események**.</span><span class="sxs-lookup"><span data-stu-id="2852e-166">There are two built-in endpoints: **Cloud to device feedback** and **Events**.</span></span>

* <span data-ttu-id="2852e-167">**Az eszköz visszajelzései alapján felhő** beállítások: Ezzel a beállítással rendelkezik két subsettings: **eszköz TTL felhő** (idő TTL) és **megőrzési idő** (órákban) üzenetekhez.</span><span class="sxs-lookup"><span data-stu-id="2852e-167">**Cloud to device feedback** settings: This setting has two subsettings: **Cloud to Device TTL** (time-to-live) and **Retention time** (in hours) for the messages.</span></span> <span data-ttu-id="2852e-168">Az első létrehoz egy IoT-központot, mindkét ezeket a beállításokat kell az alapértelmezett érték egy óra.</span><span class="sxs-lookup"><span data-stu-id="2852e-168">When your first create an IoT hub, both these settings have the default value of one hour.</span></span> <span data-ttu-id="2852e-169">Ezeket a beállításokat, a csúszkákkal, vagy írja be az értékeket.</span><span class="sxs-lookup"><span data-stu-id="2852e-169">To adjust these settings, use the sliders or type the values.</span></span>
* <span data-ttu-id="2852e-170">**Események** beállítások: Ezzel a beállítással rendelkezik több subsettings, amelyek némelyike csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="2852e-170">**Events** settings: This setting has several subsettings, some of which are read-only.</span></span> <span data-ttu-id="2852e-171">Az alábbi lista ismerteti ezeket a beállításokat:</span><span class="sxs-lookup"><span data-stu-id="2852e-171">The following list describes these settings:</span></span>

  * <span data-ttu-id="2852e-172">**Partíciók**: egy alapértelmezett értéke az IoT hub létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2852e-172">**Partitions**: A default value is set when the IoT hub is created.</span></span> <span data-ttu-id="2852e-173">Módosíthatja ezt a beállítást a partíciók száma.</span><span class="sxs-lookup"><span data-stu-id="2852e-173">You can change the number of partitions through this setting.</span></span>

  * <span data-ttu-id="2852e-174">**Event Hub-kompatibilis nevének és végpontjának**: Ha az IoT-központ jön létre, az Eseményközpont létrehozásakor belső, hogy szükség lehet a hozzáférést bizonyos körülmények között.</span><span class="sxs-lookup"><span data-stu-id="2852e-174">**Event Hub-compatible name and endpoint**: When the IoT hub is created, an Event Hub is created internally that you may need access to under certain circumstances.</span></span> <span data-ttu-id="2852e-175">Nem szabhatja testre az Event Hub-kompatibilis és a végpont értékét, de kattintva másolhatja **másolási**.</span><span class="sxs-lookup"><span data-stu-id="2852e-175">You cannot customize the Event Hub-compatible name and endpoint values but you can copy them by clicking **Copy**.</span></span>

  * <span data-ttu-id="2852e-176">**Megőrzési idő**: alapértelmezés szerint egy nap beállítva, de ez a legördülő lista használatával módosítható.</span><span class="sxs-lookup"><span data-stu-id="2852e-176">**Retention Time**: Set to one day by default but you can change it using the drop-down list.</span></span> <span data-ttu-id="2852e-177">Ez az érték a nap az eszközről a felhőbe beállítás van.</span><span class="sxs-lookup"><span data-stu-id="2852e-177">This value is in days for the device-to-cloud setting.</span></span>

  * <span data-ttu-id="2852e-178">**Felhasználói csoportok**: fogyasztói csoportok lehetővé teszik az üzenetek egymástól függetlenül olvasni az IoT hub több olvasók.</span><span class="sxs-lookup"><span data-stu-id="2852e-178">**Consumer Groups**: Consumer groups enable multiple readers to read messages independently from the IoT hub.</span></span> <span data-ttu-id="2852e-179">Minden IoT-központot egy alapértelmezett felhasználói csoport jön létre.</span><span class="sxs-lookup"><span data-stu-id="2852e-179">Every IoT hub is created with a default consumer group.</span></span> <span data-ttu-id="2852e-180">Azonban hozzáadása vagy az IoT-központok ezzel a beállítással a fogyasztói csoportok törlése.</span><span class="sxs-lookup"><span data-stu-id="2852e-180">However, you can add or delete consumer groups to your IoT hubs using this setting.</span></span>

  > [!NOTE]
  > <span data-ttu-id="2852e-181">Az alapértelmezett felhasználói csoport nem szerkeszthető, és nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="2852e-181">The default consumer group cannot be edited or deleted.</span></span>

### <a name="custom-endpoints"></a><span data-ttu-id="2852e-182">Egyéni végpontokat</span><span class="sxs-lookup"><span data-stu-id="2852e-182">Custom endpoints</span></span>

<span data-ttu-id="2852e-183">Egyéni végpontokat az IoT hub, a portál használatával is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="2852e-183">You can add custom endpoints on your IoT hub using the portal.</span></span> <span data-ttu-id="2852e-184">Az a **végpontok** panelen kattintson **Hozzáadás** megnyitásához tetején a **végpont hozzáadása** panel.</span><span class="sxs-lookup"><span data-stu-id="2852e-184">From the **Endpoints** blade, click **Add** at the top to open the **Add endpoint** blade.</span></span> <span data-ttu-id="2852e-185">Adja meg a szükséges adatokat, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="2852e-185">Enter the required information, then click **OK**.</span></span> <span data-ttu-id="2852e-186">Az egyéni végpontjának mostantól szerepel a fő **végpontok** panelen.</span><span class="sxs-lookup"><span data-stu-id="2852e-186">Your custom endpoint is now listed in the main **Endpoints** blade.</span></span>

![][13]

<span data-ttu-id="2852e-187">További tudnivalók az egyéni végpontokat [referencia - IoT-központok végpontjai][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="2852e-187">You can read more about custom endpoints in [Reference - IoT hub endpoints][lnk-devguide-endpoints].</span></span>

## <a name="routes"></a><span data-ttu-id="2852e-188">Útvonalak</span><span class="sxs-lookup"><span data-stu-id="2852e-188">Routes</span></span>

<span data-ttu-id="2852e-189">Kattintson a **útvonalak** hogyan IoT-központ kiszállítja az eszközről a felhőbe üzenetek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="2852e-189">Click **Routes** to manage how IoT Hub dispatches your device-to-cloud messages.</span></span>

![][14]

<span data-ttu-id="2852e-190">Hozzáadhat útvonalak az IoT hub kattintva **Hozzáadás** tetején a **útvonalak*** panelen írja be a szükséges adatokat, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="2852e-190">You can add routes to your IoT hub by clicking **Add** at the top of the **Routes*** blade, entering the required information, and clicking **OK**.</span></span> <span data-ttu-id="2852e-191">Az útvonal majd szerepel a fő **útvonalak** panelen.</span><span class="sxs-lookup"><span data-stu-id="2852e-191">Your route is then listed in the main **Routes** blade.</span></span> <span data-ttu-id="2852e-192">Útvonalak listájában kattintva szerkesztheti egy útvonalat.</span><span class="sxs-lookup"><span data-stu-id="2852e-192">You can edit a route by clicking it in the list of routes.</span></span> <span data-ttu-id="2852e-193">Ahhoz, hogy egy olyan útvonalat, kattintson az útvonalak a listában, és állítsa be a **engedélyezve** kapcsolót **ki**.</span><span class="sxs-lookup"><span data-stu-id="2852e-193">To enable a route, click it in the list of routes and set the **Enabled** toggle to **Off**.</span></span> <span data-ttu-id="2852e-194">A módosítás mentéséhez kattintson **OK** a panel alján.</span><span class="sxs-lookup"><span data-stu-id="2852e-194">To save the change, click **OK** at the bottom of the blade.</span></span>

![][15]

## <a name="pricing-and-scale"></a><span data-ttu-id="2852e-195">Díjszabás és méretezés</span><span class="sxs-lookup"><span data-stu-id="2852e-195">Pricing and scale</span></span>

<span data-ttu-id="2852e-196">Egy meglévő IoT-központot árképzési keresztül módosítható a **árazás** beállításait, a következő kivételekkel:</span><span class="sxs-lookup"><span data-stu-id="2852e-196">The pricing of an existing IoT hub can be changed through the **Pricing** settings, with the following exceptions:</span></span>

* <span data-ttu-id="2852e-197">Az aktuális megvalósításában egy IoT hubot szabad SKU nem rétegek egy olyan címre változtassák a fizetős SKU, vagy fordítva.</span><span class="sxs-lookup"><span data-stu-id="2852e-197">In the current implementation, an IoT hub with a free SKU cannot change tiers to one of the paid SKUs, or vice versa.</span></span>
* <span data-ttu-id="2852e-198">Csak lehet egy ingyenes szint IoT-központ az Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="2852e-198">There can only be one free tier IoT hub in the Azure subscription.</span></span>

![][12]

<span data-ttu-id="2852e-199">Áthelyezheti egy magasabb az alacsonyabb szint csak akkor, ha adott napon küldött üzenetek száma túllépi a kvótát, az alacsonyabb szinten.</span><span class="sxs-lookup"><span data-stu-id="2852e-199">You can move from a higher to lower tier only when the number of messages sent that day do exceed the quota for the lower tier.</span></span> <span data-ttu-id="2852e-200">Például ha naponta üzenetek száma meghaladja a 400000, majd az IoT hub rétegét módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="2852e-200">For example, if the number of messages per day exceeds 400,000, then the tier for the IoT hub can be changed.</span></span> <span data-ttu-id="2852e-201">Azonban ha megváltoztatja az S1 réteghez majd az IoT hub folyamatban van az adott nap.</span><span class="sxs-lookup"><span data-stu-id="2852e-201">However, if you change to the S1 tier then the IoT hub is throttled for that day.</span></span>

## <a name="delete-the-iot-hub"></a><span data-ttu-id="2852e-202">Az IoT hub törlése</span><span class="sxs-lookup"><span data-stu-id="2852e-202">Delete the IoT hub</span></span>

<span data-ttu-id="2852e-203">Az IoT hub törli kattintva tallózással is **Tallózás**, majd kiválasztja a megfelelő hub törlése.</span><span class="sxs-lookup"><span data-stu-id="2852e-203">You can browse to the IoT hub you want to delete by clicking **Browse**, and then choosing the appropriate hub to delete.</span></span> <span data-ttu-id="2852e-204">Az IoT hub törléséhez kattintson a **törlése** az IoT-központnév gombra.</span><span class="sxs-lookup"><span data-stu-id="2852e-204">To delete the IoT hub, click the **Delete** button below the IoT hub name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2852e-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2852e-205">Next steps</span></span>

<span data-ttu-id="2852e-206">Az alábbi hivatkozásokból tudhat meg többet az Azure IoT Hub kezelése:</span><span class="sxs-lookup"><span data-stu-id="2852e-206">Follow these links to learn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="2852e-207">[Tömeges az IoT-eszközök kezelése][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="2852e-207">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="2852e-208">[Az IoT-központ metrikák][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="2852e-208">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="2852e-209">[Figyelési műveletek][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="2852e-209">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="2852e-210">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="2852e-210">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="2852e-211">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="2852e-211">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="2852e-212">[Egy eszköz szimulálva IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="2852e-212">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="2852e-213">[Az IoT-megoldásból az alapoktól biztonságos mentése][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="2852e-213">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

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
