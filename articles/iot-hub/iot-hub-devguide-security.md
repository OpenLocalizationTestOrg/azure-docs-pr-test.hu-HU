---
title: "Azure IoT Hub biztonsági eljárásainak megértését |} Microsoft Docs"
description: "Fejlesztői útmutató - eszközeinek alkalmazásait és háttér-alkalmazások az IoT-központ való hozzáférés vezérlése érdekében hogyan. Biztonsági jogkivonatok, és támogatja az x.509 szabványú tanúsítványokban kapcsolatos adatokat tartalmaz."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 45631e70-865b-4e06-bb1d-aae1175a52ba
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: e4fe5400ffcf4446392015aada031dd4dfbf238a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="control-access-to-iot-hub"></a><span data-ttu-id="858b6-104">IoT Hub-hozzáférés szabályozása</span><span class="sxs-lookup"><span data-stu-id="858b6-104">Control access to IoT Hub</span></span>

<span data-ttu-id="858b6-105">Ez a cikk ismerteti az IoT hub biztonsági beállításaival.</span><span class="sxs-lookup"><span data-stu-id="858b6-105">This article describes the options for securing your IoT hub.</span></span> <span data-ttu-id="858b6-106">Az IoT-központ használ *engedélyek* hozzáférést biztosít minden egyes IoT-központ végpontjának.</span><span class="sxs-lookup"><span data-stu-id="858b6-106">IoT Hub uses *permissions* to grant access to each IoT hub endpoint.</span></span> <span data-ttu-id="858b6-107">Az engedélyek korlátozhatják az IoT-központ funkciókon alapulnak.</span><span class="sxs-lookup"><span data-stu-id="858b6-107">Permissions limit the access to an IoT hub based on functionality.</span></span>

<span data-ttu-id="858b6-108">Ez a cikk ismerteti:</span><span class="sxs-lookup"><span data-stu-id="858b6-108">This article describes:</span></span>

* <span data-ttu-id="858b6-109">A különböző engedélyek, hogy egy eszköz vagy a háttér-alkalmazást, hogy az IoT hub hozzáférjen biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="858b6-109">The different permissions that you can grant to a device or back-end app to access your IoT hub.</span></span>
* <span data-ttu-id="858b6-110">A hitelesítési folyamat és a jogkivonatok segítségével ellenőrizze az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="858b6-110">The authentication process and the tokens it uses to verify permissions.</span></span>
* <span data-ttu-id="858b6-111">Hogyan hatókör hitelesítő adatokat egy adott erőforráshoz való hozzáférés korlátozására.</span><span class="sxs-lookup"><span data-stu-id="858b6-111">How to scope credentials to limit access to specific resources.</span></span>
* <span data-ttu-id="858b6-112">IoT Hub-támogatás az x.509 szabványú tanúsítványokban.</span><span class="sxs-lookup"><span data-stu-id="858b6-112">IoT Hub support for X.509 certificates.</span></span>
* <span data-ttu-id="858b6-113">Egyéni hitelesítési mechanizmusok, amelyek használják a meglévő eszköz identitása nyilvántartó vagy hitelesítési sémák.</span><span class="sxs-lookup"><span data-stu-id="858b6-113">Custom device authentication mechanisms that use existing device identity registries or authentication schemes.</span></span>

### <a name="when-to-use"></a><span data-ttu-id="858b6-114">A következő esetekben használja</span><span class="sxs-lookup"><span data-stu-id="858b6-114">When to use</span></span>

<span data-ttu-id="858b6-115">Az IoT-központok végpontjai bármelyikét eléréséhez szükséges engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="858b6-115">You must have appropriate permissions to access any of the IoT Hub endpoints.</span></span> <span data-ttu-id="858b6-116">Például egy eszköz tartalmaznia kell egy jogkivonatot tartalmazó biztonsági hitelesítő adatokat, és minden üzenetet küld az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="858b6-116">For example, a device must include a token containing security credentials along with every message it sends to IoT Hub.</span></span>

## <a name="access-control-and-permissions"></a><span data-ttu-id="858b6-117">Hozzáférés-vezérlés és engedélyek</span><span class="sxs-lookup"><span data-stu-id="858b6-117">Access control and permissions</span></span>

<span data-ttu-id="858b6-118">Meg lehet adni [engedélyek](#iot-hub-permissions) a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="858b6-118">You can grant [permissions](#iot-hub-permissions) in the following ways:</span></span>

* <span data-ttu-id="858b6-119">**Az IoT hub-szintű megosztott elérési házirendeket**.</span><span class="sxs-lookup"><span data-stu-id="858b6-119">**IoT hub-level shared access policies**.</span></span> <span data-ttu-id="858b6-120">Megosztott elérési házirendeket is biztosítani bármilyen kombinációját [engedélyek](#iot-hub-permissions).</span><span class="sxs-lookup"><span data-stu-id="858b6-120">Shared access policies can grant any combination of [permissions](#iot-hub-permissions).</span></span> <span data-ttu-id="858b6-121">Megadhatja, hogy a házirendek a [Azure-portálon][lnk-management-portal], vagy programozott módon használatával a [IoT-központ erőforrás-szolgáltató REST API-k][lnk-resource-provider-apis].</span><span class="sxs-lookup"><span data-stu-id="858b6-121">You can define policies in the [Azure portal][lnk-management-portal], or programmatically by using the [IoT Hub resource provider REST APIs][lnk-resource-provider-apis].</span></span> <span data-ttu-id="858b6-122">Egy újonnan létrehozott IoT-központ a következő alapértelmezett házirend tartozik:</span><span class="sxs-lookup"><span data-stu-id="858b6-122">A newly created IoT hub has the following default policies:</span></span>

  * <span data-ttu-id="858b6-123">**iothubowner**: házirend az összes engedélyt.</span><span class="sxs-lookup"><span data-stu-id="858b6-123">**iothubowner**: Policy with all permissions.</span></span>
  * <span data-ttu-id="858b6-124">**szolgáltatás**: a házirend **ServiceConnect** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="858b6-124">**service**: Policy with **ServiceConnect** permission.</span></span>
  * <span data-ttu-id="858b6-125">**eszköz**: a házirend **DeviceConnect** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="858b6-125">**device**: Policy with **DeviceConnect** permission.</span></span>
  * <span data-ttu-id="858b6-126">**registryRead**: a házirend **RegistryRead** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="858b6-126">**registryRead**: Policy with **RegistryRead** permission.</span></span>
  * <span data-ttu-id="858b6-127">**registryReadWrite**: a házirend **RegistryRead** és RegistryWrite engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="858b6-127">**registryReadWrite**: Policy with **RegistryRead** and RegistryWrite permissions.</span></span>
  * <span data-ttu-id="858b6-128">**Eszköz hitelesítő adatokat**.</span><span class="sxs-lookup"><span data-stu-id="858b6-128">**Per-device security credentials**.</span></span> <span data-ttu-id="858b6-129">Minden egyes IoT-központ tartalmaz egy [identitásjegyzékhez][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="858b6-129">Each IoT Hub contains an [identity registry][lnk-identity-registry].</span></span> <span data-ttu-id="858b6-130">A identitásjegyzékhez az eszközökhöz, konfigurálhatja a hitelesítő adatokat, amelyek biztosítani **DeviceConnect** hatókörét a megfelelő eszköz végpontok engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="858b6-130">For each device in this identity registry, you can configure security credentials that grant **DeviceConnect** permissions scoped to the corresponding device endpoints.</span></span>

<span data-ttu-id="858b6-131">Például a egy tipikus IoT-megoldás:</span><span class="sxs-lookup"><span data-stu-id="858b6-131">For example, in a typical IoT solution:</span></span>

* <span data-ttu-id="858b6-132">Az eszköz felügyeleti összetevő használja a *registryReadWrite* házirend.</span><span class="sxs-lookup"><span data-stu-id="858b6-132">The device management component uses the *registryReadWrite* policy.</span></span>
* <span data-ttu-id="858b6-133">Az esemény feldolgozó összetevő használja a *szolgáltatás* házirend.</span><span class="sxs-lookup"><span data-stu-id="858b6-133">The event processor component uses the *service* policy.</span></span>
* <span data-ttu-id="858b6-134">A futásidejű eszköz üzleti logika összetevő használja a *szolgáltatás* házirend.</span><span class="sxs-lookup"><span data-stu-id="858b6-134">The run-time device business logic component uses the *service* policy.</span></span>
* <span data-ttu-id="858b6-135">Egyes eszközök csatlakoznak az IoT-központ identitásjegyzékhez tárolt hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="858b6-135">Individual devices connect using credentials stored in the IoT hub's identity registry.</span></span>

> [!NOTE]
> <span data-ttu-id="858b6-136">Lásd: [engedélyek](#iot-hub-permissions) részletes információkat.</span><span class="sxs-lookup"><span data-stu-id="858b6-136">See [permissions](#iot-hub-permissions) for detailed information.</span></span>

## <a name="authentication"></a><span data-ttu-id="858b6-137">Authentication</span><span class="sxs-lookup"><span data-stu-id="858b6-137">Authentication</span></span>

<span data-ttu-id="858b6-138">Azure IoT Hub végpontok hozzáférést biztosít a megosztott elérési házirendeket és az identitás beállításjegyzék hitelesítő adatokat egy token ellenőrzésével.</span><span class="sxs-lookup"><span data-stu-id="858b6-138">Azure IoT Hub grants access to endpoints by verifying a token against the shared access policies and identity registry security credentials.</span></span>

<span data-ttu-id="858b6-139">Biztonsági hitelesítő adatok, például a szimmetrikus kulcsokat, a rendszer soha nem elküldve a hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="858b6-139">Security credentials, such as symmetric keys, are never sent over the wire.</span></span>

> [!NOTE]
> <span data-ttu-id="858b6-140">Az Azure IoT Hub erőforrás-szolgáltató védett keresztül az Azure-előfizetéshez, mert lévő összes szolgáltatónál a [Azure Resource Manager][lnk-azure-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="858b6-140">The Azure IoT Hub resource provider is secured through your Azure subscription, as are all providers in the [Azure Resource Manager][lnk-azure-resource-manager].</span></span>

<span data-ttu-id="858b6-141">Összeállítani, és biztonsági jogkivonatokat használatával kapcsolatos további információkért lásd: [IoT-központ biztonsági jogkivonatokat][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="858b6-141">For more information about how to construct and use security tokens, see [IoT Hub security tokens][lnk-sas-tokens].</span></span>

### <a name="protocol-specifics"></a><span data-ttu-id="858b6-142">Protokoll rögzítésen</span><span class="sxs-lookup"><span data-stu-id="858b6-142">Protocol specifics</span></span>

<span data-ttu-id="858b6-143">Minden támogatott protokollok, például MQTT AMQP vagy HTTP, jogkivonatok szállításokkal különböző módon.</span><span class="sxs-lookup"><span data-stu-id="858b6-143">Each supported protocol, such as MQTT, AMQP, and HTTP, transports tokens in different ways.</span></span>

<span data-ttu-id="858b6-144">MQTT használatakor a CONNECT csomag megegyezik a deviceId a ClientId, {iothubhostname} / {deviceId} a felhasználónév mezőben, és egy SAS-jogkivonat a jelszó mező.</span><span class="sxs-lookup"><span data-stu-id="858b6-144">When using MQTT, the CONNECT packet has the deviceId as the ClientId, {iothubhostname}/{deviceId} in the Username field, and a SAS token in the Password field.</span></span> <span data-ttu-id="858b6-145">{iothubhostname} kell lennie az IoT hub (például contoso.azure-devices.net) teljes CNAME-adatait.</span><span class="sxs-lookup"><span data-stu-id="858b6-145">{iothubhostname} should be the full CName of the IoT hub (for example, contoso.azure-devices.net).</span></span>

<span data-ttu-id="858b6-146">Használata esetén [AMQP][lnk-amqp], IoT-Központ támogatja [SASL egyszerű] [ lnk-sasl-plain] és [AMQP jogcím-alapú-biztonsági][lnk-cbs].</span><span class="sxs-lookup"><span data-stu-id="858b6-146">When using [AMQP][lnk-amqp], IoT Hub supports [SASL PLAIN][lnk-sasl-plain] and [AMQP Claims-Based-Security][lnk-cbs].</span></span>

<span data-ttu-id="858b6-147">Ha AMQP jogcím-alapú-biztonsági használja, a normál továbbítja ezeket a jogkivonatokat módját adja meg.</span><span class="sxs-lookup"><span data-stu-id="858b6-147">If you use AMQP claims-based-security, the standard specifies how to transmit these tokens.</span></span>

<span data-ttu-id="858b6-148">A SASL egyszerű a **felhasználónév** lehet:</span><span class="sxs-lookup"><span data-stu-id="858b6-148">For SASL PLAIN, the **username** can be:</span></span>

* <span data-ttu-id="858b6-149">`{policyName}@sas.root.{iothubName}`Ha az IoT hub-szintű jogkivonatok használatával.</span><span class="sxs-lookup"><span data-stu-id="858b6-149">`{policyName}@sas.root.{iothubName}` if using IoT hub-level tokens.</span></span>
* <span data-ttu-id="858b6-150">`{deviceId}@sas.{iothubname}`Ha az eszköz hatókörű jogkivonatok használatával.</span><span class="sxs-lookup"><span data-stu-id="858b6-150">`{deviceId}@sas.{iothubname}` if using device-scoped tokens.</span></span>

<span data-ttu-id="858b6-151">Mindkét esetben a jelszó mező a jogkivonatot tartalmaz, a [IoT-központ biztonsági jogkivonatokat][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="858b6-151">In both cases, the password field contains the token, as described in [IoT Hub security tokens][lnk-sas-tokens].</span></span>

<span data-ttu-id="858b6-152">HTTP megvalósítja a hitelesítési érvényes token-ot a **engedélyezési** kérelemfejlécet.</span><span class="sxs-lookup"><span data-stu-id="858b6-152">HTTP implements authentication by including a valid token in the **Authorization** request header.</span></span>

#### <a name="example"></a><span data-ttu-id="858b6-153">Példa</span><span class="sxs-lookup"><span data-stu-id="858b6-153">Example</span></span>

<span data-ttu-id="858b6-154">Felhasználónév (DeviceId a kis-és nagybetűket):`iothubname.azure-devices.net/DeviceId`</span><span class="sxs-lookup"><span data-stu-id="858b6-154">Username (DeviceId is case-sensitive): `iothubname.azure-devices.net/DeviceId`</span></span>

<span data-ttu-id="858b6-155">Jelszó (SAS létrehozása a lexikális elem a [eszköz explorer] [ lnk-device-explorer] eszköz):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`</span><span class="sxs-lookup"><span data-stu-id="858b6-155">Password (Generate SAS token with the [device explorer][lnk-device-explorer] tool): `SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`</span></span>

> [!NOTE]
> <span data-ttu-id="858b6-156">A [Azure IoT SDK-k] [ lnk-sdks] automatikusan hoz létre a jogkivonatokat, a szolgáltatáshoz való kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="858b6-156">The [Azure IoT SDKs][lnk-sdks] automatically generate tokens when connecting to the service.</span></span> <span data-ttu-id="858b6-157">Bizonyos esetekben az Azure IoT SDK-k nem támogatják a protokollok és a hitelesítési módszereket.</span><span class="sxs-lookup"><span data-stu-id="858b6-157">In some cases, the Azure IoT SDKs do not support all the protocols or all the authentication methods.</span></span>

### <a name="special-considerations-for-sasl-plain"></a><span data-ttu-id="858b6-158">Különleges szempontok SASL egyszerű</span><span class="sxs-lookup"><span data-stu-id="858b6-158">Special considerations for SASL PLAIN</span></span>

<span data-ttu-id="858b6-159">AMQP SASL egyszerű használ, amikor egy ügyfél csatlakozik az IoT-központ egyetlen jogkivonat egyes TCP-kapcsolatok használhatja.</span><span class="sxs-lookup"><span data-stu-id="858b6-159">When using SASL PLAIN with AMQP, a client connecting to an IoT hub can use a single token for each TCP connection.</span></span> <span data-ttu-id="858b6-160">Amikor a jogkivonat lejár, az TCP-kapcsolatot bontja a kapcsolatot a szolgáltatás, és újracsatlakozás indítása.</span><span class="sxs-lookup"><span data-stu-id="858b6-160">When the token expires, the TCP connection disconnects from the service and triggers a reconnect.</span></span> <span data-ttu-id="858b6-161">Ezt a viselkedést, amíg nem jelent problémát, egy háttér-alkalmazás van káros eszköz alkalmazások a következők lehetnek az okai:</span><span class="sxs-lookup"><span data-stu-id="858b6-161">This behavior, while not problematic for a back-end app, is damaging for a device app for the following reasons:</span></span>

* <span data-ttu-id="858b6-162">Átjárók általában sok eszközök nevében csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="858b6-162">Gateways usually connect on behalf of many devices.</span></span> <span data-ttu-id="858b6-163">SASL egyszerű használata esetén az egyes eszközök csatlakoztatása az IoT-központ különböző TCP-kapcsolat létrehozásához rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="858b6-163">When using SASL PLAIN, they have to create a distinct TCP connection for each device connecting to an IoT hub.</span></span> <span data-ttu-id="858b6-164">Ebben a forgatókönyvben jelentősen növeli a teljesítmény-és hálózati erőforrások, és minden eszköz kapcsolat a késési növeli.</span><span class="sxs-lookup"><span data-stu-id="858b6-164">This scenario considerably increases the consumption of power and networking resources, and increases the latency of each device connection.</span></span>
* <span data-ttu-id="858b6-165">Erőforrás-korlátozott eszközök megnövekedett használt erőforrások után minden jogkivonat lejáratáról vannak hátrányosan.</span><span class="sxs-lookup"><span data-stu-id="858b6-165">Resource-constrained devices are adversely affected by the increased use of resources to reconnect after each token expiration.</span></span>

## <a name="scope-iot-hub-level-credentials"></a><span data-ttu-id="858b6-166">Hatókör IoT hub-szintű hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="858b6-166">Scope IoT hub-level credentials</span></span>

<span data-ttu-id="858b6-167">Az IoT hub-szintű biztonsági házirendek hatókörét megadhatja a tokenek egy korlátozott erőforrás URI létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="858b6-167">You can scope IoT hub-level security policies by creating tokens with a restricted resource URI.</span></span> <span data-ttu-id="858b6-168">A végpont eszköz a felhőbe küldött üzeneteket küldhet egy eszközről van például **/devices/ {deviceId} / üzenetek/események**.</span><span class="sxs-lookup"><span data-stu-id="858b6-168">For example, the endpoint to send device-to-cloud messages from a device is **/devices/{deviceId}/messages/events**.</span></span> <span data-ttu-id="858b6-169">Használhatja az IoT hub-szintű megosztott elérési házirend **DeviceConnect** engedélyek egy jogkivonatot, amelynek resourceURI van bejelentkezni **/devices/ {deviceId}**.</span><span class="sxs-lookup"><span data-stu-id="858b6-169">You can also use an IoT hub-level shared access policy with **DeviceConnect** permissions to sign a token whose resourceURI is **/devices/{deviceId}**.</span></span> <span data-ttu-id="858b6-170">Ez a megközelítés létrehoz egy jogkivonatot, amely csak eszköz nevében küldéséhez használható **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="858b6-170">This approach creates a token that is only usable to send messages on behalf of device **deviceId**.</span></span>

<span data-ttu-id="858b6-171">A mechanizmus hasonlít a [Event Hubs közzétevői házirend][lnk-event-hubs-publisher-policy], és lehetővé teszi az egyéni hitelesítési módszerek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="858b6-171">This mechanism is similar to the [Event Hubs publisher policy][lnk-event-hubs-publisher-policy], and enables you to implement custom authentication methods.</span></span>

## <a name="security-tokens"></a><span data-ttu-id="858b6-172">Biztonsági jogkivonatok kiosztva</span><span class="sxs-lookup"><span data-stu-id="858b6-172">Security tokens</span></span>

<span data-ttu-id="858b6-173">Az IoT-központ biztonsági jogkivonatokat használ hitelesítéséhez, eszközöket és szolgáltatásokat lehet, ne küldjön kulcsok a keresztülhaladnak a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="858b6-173">IoT Hub uses security tokens to authenticate devices and services to avoid sending keys on the wire.</span></span> <span data-ttu-id="858b6-174">Emellett biztonsági jogkivonatok érvényesség és hatókör korlátozott.</span><span class="sxs-lookup"><span data-stu-id="858b6-174">Additionally, security tokens are limited in time validity and scope.</span></span> <span data-ttu-id="858b6-175">[Azure IoT SDK-k] [ lnk-sdks] automatikusan hoz létre jogkivonatokat anélkül, hogy semmiféle speciális beállítást.</span><span class="sxs-lookup"><span data-stu-id="858b6-175">[Azure IoT SDKs][lnk-sdks] automatically generate tokens without requiring any special configuration.</span></span> <span data-ttu-id="858b6-176">Bizonyos esetekben kell létrehozni és biztonsági jogkivonatokat közvetlenül használható.</span><span class="sxs-lookup"><span data-stu-id="858b6-176">Some scenarios do require you to generate and use security tokens directly.</span></span> <span data-ttu-id="858b6-177">Ilyen forgatókönyvek például a következők:</span><span class="sxs-lookup"><span data-stu-id="858b6-177">Such scenarios include:</span></span>

* <span data-ttu-id="858b6-178">A MQTT, az AMQP vagy a HTTP-felületek közvetlen használatát.</span><span class="sxs-lookup"><span data-stu-id="858b6-178">The direct use of the MQTT, AMQP, or HTTP surfaces.</span></span>
* <span data-ttu-id="858b6-179">A jogkivonat-szolgáltatás mintát, ahogy végrehajtásának [egyéni eszközhitelesítés][lnk-custom-auth].</span><span class="sxs-lookup"><span data-stu-id="858b6-179">The implementation of the token service pattern, as explained in [Custom device authentication][lnk-custom-auth].</span></span>

<span data-ttu-id="858b6-180">Az IoT-központ is lehetővé teszi, hogy az IoT-központ való hitelesítéshez szükséges eszközök [X.509-tanúsítványokat][lnk-x509].</span><span class="sxs-lookup"><span data-stu-id="858b6-180">IoT Hub also allows devices to authenticate with IoT Hub using [X.509 certificates][lnk-x509].</span></span>

### <a name="security-token-structure"></a><span data-ttu-id="858b6-181">Biztonsági jogkivonat szerkezete</span><span class="sxs-lookup"><span data-stu-id="858b6-181">Security token structure</span></span>

<span data-ttu-id="858b6-182">Biztonsági jogkivonatok segítségével idő határolt eszközökhöz való hozzáférést és az adott funkciót az IoT-központ szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="858b6-182">You use security tokens to grant time-bounded access to devices and services to specific functionality in IoT Hub.</span></span> <span data-ttu-id="858b6-183">Ahhoz, hogy az IoT-központ csatlakozni engedélyezési, eszközöket és szolgáltatásokat kell küldenie egy közös hozzáférésű vagy a szimmetrikus kulcs aláírva biztonsági jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="858b6-183">To get authorization to connect to IoT Hub, devices and services must send security tokens signed with either a shared access or symmetric key.</span></span> <span data-ttu-id="858b6-184">Ezek a kulcsok eszköz megadásával az identitásjegyzékhez az tárolják.</span><span class="sxs-lookup"><span data-stu-id="858b6-184">These keys are stored with a device identity in the identity registry.</span></span>

<span data-ttu-id="858b6-185">A token egy megosztott elérési kulcs engedélyezi a hozzáférést a megosztott hozzáférési házirend engedélyeket társított összes funkcióját a aláírva.</span><span class="sxs-lookup"><span data-stu-id="858b6-185">A token signed with a shared access key grants access to all the functionality associated with the shared access policy permissions.</span></span> <span data-ttu-id="858b6-186">Egy eszközidentitás szimmetrikus kulcs csak biztosít aláírt jogkivonat a **DeviceConnect** engedély a társított eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="858b6-186">A token signed with a device identity's symmetric key only grants the **DeviceConnect** permission for the associated device identity.</span></span>

<span data-ttu-id="858b6-187">A biztonsági jogkivonat formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="858b6-187">The security token has the following format:</span></span>

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

<span data-ttu-id="858b6-188">Az alábbiakban a várt értékek:</span><span class="sxs-lookup"><span data-stu-id="858b6-188">Here are the expected values:</span></span>

| <span data-ttu-id="858b6-189">Érték</span><span class="sxs-lookup"><span data-stu-id="858b6-189">Value</span></span> | <span data-ttu-id="858b6-190">Leírás</span><span class="sxs-lookup"><span data-stu-id="858b6-190">Description</span></span> |
| --- | --- |
| <span data-ttu-id="858b6-191">{aláírás}</span><span class="sxs-lookup"><span data-stu-id="858b6-191">{signature}</span></span> |<span data-ttu-id="858b6-192">Egy HMAC-SHA256 aláírás karakterlánc a következő formátumban: `{URL-encoded-resourceURI} + "\n" + expiry`.</span><span class="sxs-lookup"><span data-stu-id="858b6-192">An HMAC-SHA256 signature string of the form: `{URL-encoded-resourceURI} + "\n" + expiry`.</span></span> <span data-ttu-id="858b6-193">**Fontos**: A kulcs a Base64 kódolású anyag dekódolni, és a HMAC-SHA256 számítási végrehajtásához kulcsként.</span><span class="sxs-lookup"><span data-stu-id="858b6-193">**Important**: The key is decoded from base64 and used as key to perform the HMAC-SHA256 computation.</span></span> |
| <span data-ttu-id="858b6-194">{resourceURI}</span><span class="sxs-lookup"><span data-stu-id="858b6-194">{resourceURI}</span></span> |<span data-ttu-id="858b6-195">URI-előtag (által szegmens) a végpontok hozzáfér a tokenhez, kezdve IoT-központ (protokoll) állomásnevét.</span><span class="sxs-lookup"><span data-stu-id="858b6-195">URI prefix (by segment) of the endpoints that can be accessed with this token, starting with host name of the IoT hub (no protocol).</span></span> <span data-ttu-id="858b6-196">Például: `myHub.azure-devices.net/devices/device1`</span><span class="sxs-lookup"><span data-stu-id="858b6-196">For example, `myHub.azure-devices.net/devices/device1`</span></span> |
| <span data-ttu-id="858b6-197">{a lejárati}</span><span class="sxs-lookup"><span data-stu-id="858b6-197">{expiry}</span></span> |<span data-ttu-id="858b6-198">UTF8 karakterláncok esetében a epoch 00:00:00 UTC 1970. január 1. a másodpercek száma.</span><span class="sxs-lookup"><span data-stu-id="858b6-198">UTF8 strings for number of seconds since the epoch 00:00:00 UTC on 1 January 1970.</span></span> |
| <span data-ttu-id="858b6-199">{URL-kódolású-resourceURI}</span><span class="sxs-lookup"><span data-stu-id="858b6-199">{URL-encoded-resourceURI}</span></span> |<span data-ttu-id="858b6-200">Alacsonyabb eset az URL-kódolást a nagybetűs erőforrás URI</span><span class="sxs-lookup"><span data-stu-id="858b6-200">Lower case URL-encoding of the lower case resource URI</span></span> |
| <span data-ttu-id="858b6-201">{Házirendnév}</span><span class="sxs-lookup"><span data-stu-id="858b6-201">{policyName}</span></span> |<span data-ttu-id="858b6-202">A megosztott elérési házirendet, amely a token hivatkozik neve.</span><span class="sxs-lookup"><span data-stu-id="858b6-202">The name of the shared access policy to which this token refers.</span></span> <span data-ttu-id="858b6-203">Hiányzik, ha a jogkivonat-eszközbeállításjegyzékben hitelesítő adatok hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="858b6-203">Absent if the token refers to device-registry credentials.</span></span> |

<span data-ttu-id="858b6-204">**Megjegyzés: előtag alapján**: az URI-előtag számított szegmens és nem karakter.</span><span class="sxs-lookup"><span data-stu-id="858b6-204">**Note on prefix**: The URI prefix is computed by segment and not by character.</span></span> <span data-ttu-id="858b6-205">Például `/a/b` előtagja, az `/a/b/c` nem `/a/bc`.</span><span class="sxs-lookup"><span data-stu-id="858b6-205">For example `/a/b` is a prefix for `/a/b/c` but not for `/a/bc`.</span></span>

<span data-ttu-id="858b6-206">Az alábbi Node.js kódrészletben láthatja a hívott függvény **generateSasToken** , amely kiszámítja a jogkivonatot a bemenetek `resourceUri, signingKey, policyName, expiresInMins`.</span><span class="sxs-lookup"><span data-stu-id="858b6-206">The following Node.js snippet shows a function called **generateSasToken** that computes the token from the inputs `resourceUri, signingKey, policyName, expiresInMins`.</span></span> <span data-ttu-id="858b6-207">A következő részek a különböző bemeneti adatokat a különböző token használatából adódó inicializálásával.</span><span class="sxs-lookup"><span data-stu-id="858b6-207">The next sections detail how to initialize the different inputs for the different token use cases.</span></span>

```nodejs
var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
    resourceUri = encodeURIComponent(resourceUri);

    // Set expiration in seconds
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    expires = Math.ceil(expires);
    var toSign = resourceUri + '\n' + expires;

    // Use crypto
    var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
    hmac.update(toSign);
    var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

    // Construct autorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
};
```

<span data-ttu-id="858b6-208">Összehasonlítása, mint a biztonsági jogkivonat létrehozásához egyenértékű Python kódot van:</span><span class="sxs-lookup"><span data-stu-id="858b6-208">As a comparison, the equivalent Python code to generate a security token is:</span></span>

```python
from base64 import b64encode, b64decode
from hashlib import sha256
from time import time
from urllib import quote_plus, urlencode
from hmac import HMAC

def generate_sas_token(uri, key, policy_name, expiry=3600):
    ttl = time() + expiry
    sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
    print sign_key
    signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

    rawtoken = {
        'sr' :  uri,
        'sig': signature,
        'se' : str(int(ttl))
    }

    if policy_name is not None:
        rawtoken['skn'] = policy_name

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

> [!NOTE]
> <span data-ttu-id="858b6-209">Az idő a token érvényességének IoT-központ gépeken van hitelesítve, mivel az óra, a gép, amely a token hoz létre az eltéréseket minimális kell lennie.</span><span class="sxs-lookup"><span data-stu-id="858b6-209">Since the time validity of the token is validated on IoT Hub machines, the drift on the clock of the machine that generates the token must be minimal.</span></span>

### <a name="use-sas-tokens-in-a-device-app"></a><span data-ttu-id="858b6-210">Használja a SAS-tokenje eszköz alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="858b6-210">Use SAS tokens in a device app</span></span>

<span data-ttu-id="858b6-211">Két módon beszerzése **DeviceConnect** IoT-központ engedélyeket a biztonsági jogkivonatokat: használja a [eszköz szimmetrikus kulcsot az identitásjegyzékhez az](#use-a-symmetric-key-in-the-identity-registry), vagy használjon egy [megosztott elérési kulcsával](#use-a-shared-access-policy).</span><span class="sxs-lookup"><span data-stu-id="858b6-211">There are two ways to obtain **DeviceConnect** permissions with IoT Hub with security tokens: use a [symmetric device key from the identity registry](#use-a-symmetric-key-in-the-identity-registry), or use a [shared access key](#use-a-shared-access-policy).</span></span>

<span data-ttu-id="858b6-212">Ne feledje, hogy eszközökhöz elérhető összes funkciót tesz elérhetővé előtaggal rendelkező végpontokon tervezési `/devices/{deviceId}`.</span><span class="sxs-lookup"><span data-stu-id="858b6-212">Remember that all functionality accessible from devices is exposed by design on endpoints with prefix `/devices/{deviceId}`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="858b6-213">Az egyetlen lehetőség, hogy az IoT-központ végez hitelesítést egy adott eszköz eszköz identitása szimmetrikus kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="858b6-213">The only way that IoT Hub authenticates a specific device is using the device identity symmetric key.</span></span> <span data-ttu-id="858b6-214">Azokban az esetekben, amikor egy megosztott elérési házirendet eszköz funkció eléréséhez használt a megoldás figyelembe kell vennie a megbízható részletes a biztonsági jogkivonat kiállító összetevő.</span><span class="sxs-lookup"><span data-stu-id="858b6-214">In cases when a shared access policy is used to access device functionality, the solution must consider the component issuing the security token as a trusted subcomponent.</span></span>

<span data-ttu-id="858b6-215">Az eszköz felé néző végpontok (függetlenül a protocol) a következők:</span><span class="sxs-lookup"><span data-stu-id="858b6-215">The device-facing endpoints are (irrespective of the protocol):</span></span>

| <span data-ttu-id="858b6-216">Végpont</span><span class="sxs-lookup"><span data-stu-id="858b6-216">Endpoint</span></span> | <span data-ttu-id="858b6-217">Funkció</span><span class="sxs-lookup"><span data-stu-id="858b6-217">Functionality</span></span> |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |<span data-ttu-id="858b6-218">Eszköz-felhő üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="858b6-218">Send device-to-cloud messages.</span></span> |
| `{iot hub host name}/devices/{deviceId}/devicebound` |<span data-ttu-id="858b6-219">Felhő eszközre üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="858b6-219">Receive cloud-to-device messages.</span></span> |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a><span data-ttu-id="858b6-220">A szimmetrikus kulcsot használja az identitásjegyzékhez</span><span class="sxs-lookup"><span data-stu-id="858b6-220">Use a symmetric key in the identity registry</span></span>

<span data-ttu-id="858b6-221">Amikor egy eszközidentitás szimmetrikus kulcs használatával hoz létre jogkivonatokat, a házirendnév (`skn`) elem a jogkivonat nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="858b6-221">When using a device identity's symmetric key to generate a token, the policyName (`skn`) element of the token is omitted.</span></span>

<span data-ttu-id="858b6-222">Például egy jogkivonatot létrehozott összes eszköz funkciók eléréséhez a következő paramétereket kell rendelkezniük:</span><span class="sxs-lookup"><span data-stu-id="858b6-222">For example, a token created to access all device functionality should have the following parameters:</span></span>

* <span data-ttu-id="858b6-223">erőforrás URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span><span class="sxs-lookup"><span data-stu-id="858b6-223">resource URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span></span>
* <span data-ttu-id="858b6-224">aláírási kulcs: bármely szimmetrikus kulcs a `{device id}` identitása</span><span class="sxs-lookup"><span data-stu-id="858b6-224">signing key: any symmetric key for the `{device id}` identity,</span></span>
* <span data-ttu-id="858b6-225">Nincs házirend neve</span><span class="sxs-lookup"><span data-stu-id="858b6-225">no policy name,</span></span>
* <span data-ttu-id="858b6-226">a lejárati idő.</span><span class="sxs-lookup"><span data-stu-id="858b6-226">any expiration time.</span></span>

<span data-ttu-id="858b6-227">Egy példa az előző Node.js-függvény használatával a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="858b6-227">An example using the preceding Node.js function would be:</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

<span data-ttu-id="858b6-228">Az eredmény, az összes funkciót device1 hozzáférést biztosít, amely a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="858b6-228">The result, which grants access to all functionality for device1, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> <span data-ttu-id="858b6-229">A .NET használatával SAS-jogkivonat készítése lehet [eszköz explorer] [ lnk-device-explorer] eszköz vagy a platformfüggetlen, csomópont-alapú [IOT hubbal-explorer] [ lnk-iothub-explorer] parancssori segédprogram.</span><span class="sxs-lookup"><span data-stu-id="858b6-229">It is possible to generate a SAS token using the .NET [device explorer][lnk-device-explorer] tool or the cross-platform, node-based [iothub-explorer][lnk-iothub-explorer] command-line utility.</span></span>

### <a name="use-a-shared-access-policy"></a><span data-ttu-id="858b6-230">Egy megosztott elérési házirendet használja</span><span class="sxs-lookup"><span data-stu-id="858b6-230">Use a shared access policy</span></span>

<span data-ttu-id="858b6-231">Amikor egy jogkivonatot hoz létre egy megosztott elérési házirendet, állítsa be a `skn` mezőjét a házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="858b6-231">When you create a token from a shared access policy, set the `skn` field to the name of the policy.</span></span> <span data-ttu-id="858b6-232">Ez a házirend biztosítania kell a **DeviceConnect** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="858b6-232">This policy must grant the **DeviceConnect** permission.</span></span>

<span data-ttu-id="858b6-233">A megosztott elérési házirendeket eszköz funkció eléréséhez használt kétféle módon történhet a következők:</span><span class="sxs-lookup"><span data-stu-id="858b6-233">The two main scenarios for using shared access policies to access device functionality are:</span></span>

* <span data-ttu-id="858b6-234">[a felhő protokoll átjárók][lnk-endpoints],</span><span class="sxs-lookup"><span data-stu-id="858b6-234">[cloud protocol gateways][lnk-endpoints],</span></span>
* <span data-ttu-id="858b6-235">[a token szolgáltatások] [ lnk-custom-auth] valósítja meg az egyéni hitelesítési sémák használatával.</span><span class="sxs-lookup"><span data-stu-id="858b6-235">[token services][lnk-custom-auth] used to implement custom authentication schemes.</span></span>

<span data-ttu-id="858b6-236">Mivel a megosztott elérési házirend potenciálisan hozzáférést biztosíthat bármely olyan eszközről csatlakozni, fontos biztonsági jogkivonatokat létrehozásakor használt a helyes erőforrás URI azonosítója.</span><span class="sxs-lookup"><span data-stu-id="858b6-236">Since the shared access policy can potentially grant access to connect as any device, it is important to use the correct resource URI when creating security tokens.</span></span> <span data-ttu-id="858b6-237">Ez a beállítás különösen fontos a token szolgáltatásokat, amelyek a jogkivonatot az erőforrás URI azonosítója egy adott eszközt hatókörének rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="858b6-237">This setting is especially important for token services, which have to scope the token to a specific device using the resource URI.</span></span> <span data-ttu-id="858b6-238">Ezt a pontot fontos kevesebb protokoll átjárók mivel azokat már van közvetítő forgalmat az összes eszköz.</span><span class="sxs-lookup"><span data-stu-id="858b6-238">This point is less relevant for protocol gateways as they are already mediating traffic for all devices.</span></span>

<span data-ttu-id="858b6-239">Tegyük fel, a jogkivonat-szolgáltatás használatával az előre létrehozott megosztott hozzáférési házirend nevű **eszköz** jogkivonat hozna létre a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="858b6-239">As an example, a token service using the pre-created shared access policy called **device** would create a token with the following parameters:</span></span>

* <span data-ttu-id="858b6-240">erőforrás URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span><span class="sxs-lookup"><span data-stu-id="858b6-240">resource URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span></span>
* <span data-ttu-id="858b6-241">aláírási kulcs: egyik kulcsának a `device` házirend,</span><span class="sxs-lookup"><span data-stu-id="858b6-241">signing key: one of the keys of the `device` policy,</span></span>
* <span data-ttu-id="858b6-242">házirend neve: `device`,</span><span class="sxs-lookup"><span data-stu-id="858b6-242">policy name: `device`,</span></span>
* <span data-ttu-id="858b6-243">a lejárati idő.</span><span class="sxs-lookup"><span data-stu-id="858b6-243">any expiration time.</span></span>

<span data-ttu-id="858b6-244">Egy példa az előző Node.js-függvény használatával a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="858b6-244">An example using the preceding Node.js function would be:</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

<span data-ttu-id="858b6-245">Az eredmény, az összes funkciót device1 hozzáférést biztosít, amely a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="858b6-245">The result, which grants access to all functionality for device1, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

<span data-ttu-id="858b6-246">A protokoll-átjáró használhatja ugyanazokat az összes eszköz egyszerűen beállítása az erőforrás URI azonosítója a `myhub.azure-devices.net/devices`.</span><span class="sxs-lookup"><span data-stu-id="858b6-246">A protocol gateway could use the same token for all devices simply setting the resource URI to `myhub.azure-devices.net/devices`.</span></span>

### <a name="use-security-tokens-from-service-components"></a><span data-ttu-id="858b6-247">Használja a biztonsági jogkivonatokat a szolgáltatás-összetevők</span><span class="sxs-lookup"><span data-stu-id="858b6-247">Use security tokens from service components</span></span>

<span data-ttu-id="858b6-248">Szolgáltatás-összetevők csak hoz létre a megfelelő engedélyek megadását, amint azt korábban megosztott elérési házirendeket használó biztonsági jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="858b6-248">Service components can only generate security tokens using shared access policies granting the appropriate permissions as explained previously.</span></span>

<span data-ttu-id="858b6-249">Ez a szolgáltatás funkciók elérhetők a végpontok:</span><span class="sxs-lookup"><span data-stu-id="858b6-249">Here is the service functions exposed on the endpoints:</span></span>

| <span data-ttu-id="858b6-250">Végpont</span><span class="sxs-lookup"><span data-stu-id="858b6-250">Endpoint</span></span> | <span data-ttu-id="858b6-251">Funkció</span><span class="sxs-lookup"><span data-stu-id="858b6-251">Functionality</span></span> |
| --- | --- |
| `{iot hub host name}/devices` |<span data-ttu-id="858b6-252">Létrehozása, frissítése, lekérése és törlése eszköz identitások.</span><span class="sxs-lookup"><span data-stu-id="858b6-252">Create, update, retrieve, and delete device identities.</span></span> |
| `{iot hub host name}/messages/events` |<span data-ttu-id="858b6-253">Eszköz-felhő üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="858b6-253">Receive device-to-cloud messages.</span></span> |
| `{iot hub host name}/servicebound/feedback` |<span data-ttu-id="858b6-254">A felhő-eszközre küldött üzenetek visszajelzést kap.</span><span class="sxs-lookup"><span data-stu-id="858b6-254">Receive feedback for cloud-to-device messages.</span></span> |
| `{iot hub host name}/devicebound` |<span data-ttu-id="858b6-255">Felhő-eszközre küldött üzenetek küldése.</span><span class="sxs-lookup"><span data-stu-id="858b6-255">Send cloud-to-device messages.</span></span> |

<span data-ttu-id="858b6-256">Tegyük fel, a megosztott hozzáférési házirend nevű szolgáltatás létrehozásakor használja az előre létrehozott **registryRead** jogkivonat hozna létre a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="858b6-256">As an example, a service generating using the pre-created shared access policy called **registryRead** would create a token with the following parameters:</span></span>

* <span data-ttu-id="858b6-257">erőforrás URI: `{IoT hub name}.azure-devices.net/devices`,</span><span class="sxs-lookup"><span data-stu-id="858b6-257">resource URI: `{IoT hub name}.azure-devices.net/devices`,</span></span>
* <span data-ttu-id="858b6-258">aláírási kulcs: egyik kulcsának a `registryRead` házirend,</span><span class="sxs-lookup"><span data-stu-id="858b6-258">signing key: one of the keys of the `registryRead` policy,</span></span>
* <span data-ttu-id="858b6-259">házirend neve: `registryRead`,</span><span class="sxs-lookup"><span data-stu-id="858b6-259">policy name: `registryRead`,</span></span>
* <span data-ttu-id="858b6-260">a lejárati idő.</span><span class="sxs-lookup"><span data-stu-id="858b6-260">any expiration time.</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

<span data-ttu-id="858b6-261">Az eredmény, az eszköz identitások olvasási hozzáférést biztosítanak, amely a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="858b6-261">The result, which would grant access to read all device identities, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a><span data-ttu-id="858b6-262">Támogatott X.509-tanúsítványokat</span><span class="sxs-lookup"><span data-stu-id="858b6-262">Supported X.509 certificates</span></span>

<span data-ttu-id="858b6-263">X.509-tanúsítvány használatával hitelesíteni egy eszközt az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="858b6-263">You can use any X.509 certificate to authenticate a device with IoT Hub.</span></span> <span data-ttu-id="858b6-264">A tanúsítványok tartalmaznak:</span><span class="sxs-lookup"><span data-stu-id="858b6-264">Certificates include:</span></span>

* <span data-ttu-id="858b6-265">**Meglévő X.509 tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="858b6-265">**An existing X.509 certificate**.</span></span> <span data-ttu-id="858b6-266">Előfordulhat, hogy egy eszköz már társítva X.509 tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="858b6-266">A device may already have an X.509 certificate associated with it.</span></span> <span data-ttu-id="858b6-267">Az eszköz a tanúsítvány használatával hitelesítik magukat az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="858b6-267">The device can use this certificate to authenticate with IoT Hub.</span></span>
* <span data-ttu-id="858b6-268">**A önálló jön létre, és önaláírt tanúsítvány X-509**.</span><span class="sxs-lookup"><span data-stu-id="858b6-268">**A self-generated and self-signed X-509 certificate**.</span></span> <span data-ttu-id="858b6-269">A gyártó vagy a belső fejlesztésű deployer ezeket a tanúsítványokat létrehozni és és tárolására is a megfelelő titkos kulcsnak (tanúsítvány) az eszközön.</span><span class="sxs-lookup"><span data-stu-id="858b6-269">A device manufacturer or in-house deployer can generate these certificates and store the corresponding private key (and certificate) on the device.</span></span> <span data-ttu-id="858b6-270">Használhatja például a [OpenSSL] [ lnk-openssl] és [Windows SelfSignedCertificate] [ lnk-selfsigned] segédprogram erre a célra.</span><span class="sxs-lookup"><span data-stu-id="858b6-270">You can use tools such as [OpenSSL][lnk-openssl] and [Windows SelfSignedCertificate][lnk-selfsigned] utility for this purpose.</span></span>
* <span data-ttu-id="858b6-271">**X.509 tanúsítvány hitelesítésszolgáltató által aláírt**.</span><span class="sxs-lookup"><span data-stu-id="858b6-271">**CA-signed X.509 certificate**.</span></span> <span data-ttu-id="858b6-272">Egy eszköz azonosítására és sikerült magát hitelesítenie, IoT-központ, egy X.509 tanúsítvány jön létre, és aláírt által hitelesítésszolgáltató (CA) is használhat.</span><span class="sxs-lookup"><span data-stu-id="858b6-272">To identify a device and authenticate it with IoT Hub, you can use an X.509 certificate generated and signed by a Certification Authority (CA).</span></span> <span data-ttu-id="858b6-273">Az IoT-központ csak ellenőrzi, hogy az ujjlenyomat jelenik meg a megegyezik-e a konfigurált ujjlenyomattal.</span><span class="sxs-lookup"><span data-stu-id="858b6-273">IoT Hub only verifies that the thumbprint presented matches the configured thumbprint.</span></span> <span data-ttu-id="858b6-274">IOT hubbal nem felel meg a tanúsítványlánc.</span><span class="sxs-lookup"><span data-stu-id="858b6-274">IotHub does not validate the certificate chain.</span></span>

<span data-ttu-id="858b6-275">Egy eszköz vagy használhat egy X.509 tanúsítvány vagy egy biztonsági jogkivonatot a hitelesítés, de soha sem egyszerre mindkettőre.</span><span class="sxs-lookup"><span data-stu-id="858b6-275">A device may either use an X.509 certificate or a security token for authentication, but not both.</span></span>

### <a name="register-an-x509-certificate-for-a-device"></a><span data-ttu-id="858b6-276">Egy eszköz X.509 tanúsítvány regisztrálása</span><span class="sxs-lookup"><span data-stu-id="858b6-276">Register an X.509 certificate for a device</span></span>

<span data-ttu-id="858b6-277">A [Azure IoT szolgáltatás SDK for C#] [ lnk-service-sdk] (verzió 1.0.8+) támogatja az olyan eszköz, amely a hitelesítéshez használja egy X.509 tanúsítvány regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="858b6-277">The [Azure IoT Service SDK for C#][lnk-service-sdk] (version 1.0.8+) supports registering a device that uses an X.509 certificate for authentication.</span></span> <span data-ttu-id="858b6-278">Például az importálási/exportálási az eszközök más API-k X.509-tanúsítványokat is támogatja.</span><span class="sxs-lookup"><span data-stu-id="858b6-278">Other APIs such as import/export of devices also support X.509 certificates.</span></span>

### <a name="c-support"></a><span data-ttu-id="858b6-279">C\# támogatása</span><span class="sxs-lookup"><span data-stu-id="858b6-279">C\# Support</span></span>

<span data-ttu-id="858b6-280">A **RegistryManager** osztály programozott módon kell regisztrálni egy eszközt biztosít.</span><span class="sxs-lookup"><span data-stu-id="858b6-280">The **RegistryManager** class provides a programmatic way to register a device.</span></span> <span data-ttu-id="858b6-281">Különösen a **AddDeviceAsync** és **UpdateDeviceAsync** módszerek lehetővé teszik a regisztrálja, és frissítse egy eszközt az IoT-központ identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="858b6-281">In particular, the **AddDeviceAsync** and **UpdateDeviceAsync** methods enable you to register and update a device in the IoT Hub identity registry.</span></span> <span data-ttu-id="858b6-282">Ez a két módszer igénybe vehet egy **eszköz** példány bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="858b6-282">These two methods take a **Device** instance as input.</span></span> <span data-ttu-id="858b6-283">A **eszköz** osztály tartalmaz egy **hitelesítési** tulajdonság, amely lehetővé teszi az elsődleges és másodlagos X.509 tanúsítvány-ujjlenyomatok megadását.</span><span class="sxs-lookup"><span data-stu-id="858b6-283">The **Device** class includes an **Authentication** property that allows you to specify primary and secondary X.509 certificate thumbprints.</span></span> <span data-ttu-id="858b6-284">Az ujjlenyomat (kódolással bináris DER tárolt) X.509-tanúsítvány egy SHA-1 kivonatoló jelöli.</span><span class="sxs-lookup"><span data-stu-id="858b6-284">The thumbprint represents a SHA-1 hash of the X.509 certificate (stored using binary DER encoding).</span></span> <span data-ttu-id="858b6-285">Lehetősége van egy elsődleges ujjlenyomatot vagy egy másodlagos ujjlenyomatot, vagy mindkettő megadására.</span><span class="sxs-lookup"><span data-stu-id="858b6-285">You have the option of specifying a primary thumbprint or a secondary thumbprint or both.</span></span> <span data-ttu-id="858b6-286">Elsődleges és másodlagos ujjlenyomatok tanúsítvány helyettesítő helyzetek kezelésére használhatók.</span><span class="sxs-lookup"><span data-stu-id="858b6-286">Primary and secondary thumbprints are supported to handle certificate rollover scenarios.</span></span>

> [!NOTE]
> <span data-ttu-id="858b6-287">Az IoT-központ nem kér és nem tárolja a teljes X.509-tanúsítványt, csak az ujjlenyomat.</span><span class="sxs-lookup"><span data-stu-id="858b6-287">IoT Hub does not require or store the entire X.509 certificate, only the thumbprint.</span></span>

<span data-ttu-id="858b6-288">Íme egy minta C\# kell regisztrálni egy X.509 tanúsítvánnyal eszközt kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="858b6-288">Here is a sample C\# code snippet to register a device using an X.509 certificate:</span></span>

```csharp
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-certificate-during-run-time-operations"></a><span data-ttu-id="858b6-289">Egy X.509 tanúsítvány közbeni futásidejű műveletek</span><span class="sxs-lookup"><span data-stu-id="858b6-289">Use an X.509 certificate during run-time operations</span></span>

<span data-ttu-id="858b6-290">A [Azure IoT-eszközök SDK for .NET] [ lnk-client-sdk] (verzió 1.0.11+) X.509 tanúsítvány használatát támogatja.</span><span class="sxs-lookup"><span data-stu-id="858b6-290">The [Azure IoT device SDK for .NET][lnk-client-sdk] (version 1.0.11+) supports the use of X.509 certificates.</span></span>

### <a name="c-support"></a><span data-ttu-id="858b6-291">C\# támogatása</span><span class="sxs-lookup"><span data-stu-id="858b6-291">C\# Support</span></span>

<span data-ttu-id="858b6-292">Az osztály **DeviceAuthenticationWithX509Certificate** a létrehozását támogatja **DeviceClient** példányok X.509 tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="858b6-292">The class **DeviceAuthenticationWithX509Certificate** supports the creation of **DeviceClient** instances using an X.509 certificate.</span></span> <span data-ttu-id="858b6-293">Az X.509 tanúsítvány a titkos kulcsot tartalmazó PFX (más néven PKCS #12) formátumban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="858b6-293">The X.509 certificate must be in the PFX (also called PKCS #12) format that includes the private key.</span></span>

<span data-ttu-id="858b6-294">Íme egy minta kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="858b6-294">Here is a sample code snippet:</span></span>

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a><span data-ttu-id="858b6-295">Egyéni eszközhitelesítés</span><span class="sxs-lookup"><span data-stu-id="858b6-295">Custom device authentication</span></span>

<span data-ttu-id="858b6-296">Használhatja az IoT Hub [identitásjegyzékhez] [ lnk-identity-registry] eszközönkénti biztonsági hitelesítő adatok beállítása és a hozzáférés vezérlése segítségével [jogkivonatok][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="858b6-296">You can use the IoT Hub [identity registry][lnk-identity-registry] to configure per-device security credentials and access control using [tokens][lnk-sas-tokens].</span></span> <span data-ttu-id="858b6-297">Ha egy IoT-megoldás már van egy egyéni identitás beállításjegyzék és/vagy hitelesítési séma, érdemes létrehozni egy *token service* Ez az infrastruktúra integrálása az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="858b6-297">If an IoT solution already has a custom identity registry and/or authentication scheme, consider creating a *token service* to integrate this infrastructure with IoT Hub.</span></span> <span data-ttu-id="858b6-298">Ily módon használhatja más IoT-szolgáltatásokat a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="858b6-298">In this way, you can use other IoT features in your solution.</span></span>

<span data-ttu-id="858b6-299">A jogkivonat-szolgáltatás egy olyan egyéni felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="858b6-299">A token service is a custom cloud service.</span></span> <span data-ttu-id="858b6-300">Az IoT-központ használ *megosztott hozzáférési házirend* rendelkező **DeviceConnect** létrehozásához szükséges engedélyek *eszköz hatókörű* jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="858b6-300">It uses an IoT Hub *shared access policy* with **DeviceConnect** permissions to create *device-scoped* tokens.</span></span> <span data-ttu-id="858b6-301">Ezeket a jogkivonatokat engedélyezheti az IoT hub való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="858b6-301">These tokens enable a device to connect to your IoT hub.</span></span>

![A jogkivonat-szolgáltatás minta lépései][img-tokenservice]

<span data-ttu-id="858b6-303">A jogkivonat-szolgáltatás minta fő lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="858b6-303">Here are the main steps of the token service pattern:</span></span>

1. <span data-ttu-id="858b6-304">Az IoT-központot, megosztott hozzáférési házirend létrehozása **DeviceConnect** az IoT hub engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="858b6-304">Create an IoT Hub shared access policy with **DeviceConnect** permissions for your IoT hub.</span></span> <span data-ttu-id="858b6-305">Ezt a házirendet hozhat létre a [Azure-portálon] [ lnk-management-portal] vagy programon keresztül.</span><span class="sxs-lookup"><span data-stu-id="858b6-305">You can create this policy in the [Azure portal][lnk-management-portal] or programmatically.</span></span> <span data-ttu-id="858b6-306">A jogkivonat-szolgáltatás használja ezt a házirendet hoz létre a jogkivonatok aláírásához.</span><span class="sxs-lookup"><span data-stu-id="858b6-306">The token service uses this policy to sign the tokens it creates.</span></span>
1. <span data-ttu-id="858b6-307">Ha egy eszközt az IoT hub hozzáférésre van szüksége, egy aláírt jogkivonat kér a jogkivonat-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="858b6-307">When a device needs to access your IoT hub, it requests a signed token from your token service.</span></span> <span data-ttu-id="858b6-308">Az eszköz hitelesíthető az egyéni identitás-beállításjegyzék/hitelesítési séma annak meghatározásához, hogy a jogkivonat-szolgáltatás a jogkivonat létrehozásához használja az eszközidentitást.</span><span class="sxs-lookup"><span data-stu-id="858b6-308">The device can authenticate with your custom identity registry/authentication scheme to determine the device identity that the token service uses to create the token.</span></span>
1. <span data-ttu-id="858b6-309">A jogkivonat-szolgáltatás egy jogkivonatot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="858b6-309">The token service returns a token.</span></span> <span data-ttu-id="858b6-310">A jogkivonat segítségével hozhatók létre `/devices/{deviceId}` , `resourceURI`, a `deviceId` hitelesített eszközként.</span><span class="sxs-lookup"><span data-stu-id="858b6-310">The token is created by using `/devices/{deviceId}` as `resourceURI`, with `deviceId` as the device being authenticated.</span></span> <span data-ttu-id="858b6-311">A jogkivonat-szolgáltatás a megosztott elérési házirendet használja a jogkivonat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="858b6-311">The token service uses the shared access policy to construct the token.</span></span>
1. <span data-ttu-id="858b6-312">Az eszköz közvetlenül és az IoT hub használja a jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="858b6-312">The device uses the token directly with the IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="858b6-313">Használhatja a .NET-osztályt [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] vagy a Java-osztály [IotHubServiceSasToken] [ lnk-java-sas] a jogkivonat-szolgáltatás a jogkivonat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="858b6-313">You can use the .NET class [SharedAccessSignatureBuilder][lnk-dotnet-sas] or the Java class [IotHubServiceSasToken][lnk-java-sas] to create a token in your token service.</span></span>

<span data-ttu-id="858b6-314">A jogkivonat-szolgáltatás tetszés szerint állíthatja be a jogkivonat lejáratáról.</span><span class="sxs-lookup"><span data-stu-id="858b6-314">The token service can set the token expiration as desired.</span></span> <span data-ttu-id="858b6-315">Amikor a jogkivonat lejár, az IoT-központ kiszolgálója az eszköz kapcsolata.</span><span class="sxs-lookup"><span data-stu-id="858b6-315">When the token expires, the IoT hub severs the device connection.</span></span> <span data-ttu-id="858b6-316">Ezt követően az eszköz egy új jogkivonatot kell kérniük a jogkivonat-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="858b6-316">Then, the device must request a new token from the token service.</span></span> <span data-ttu-id="858b6-317">Egy rövid lejárati idejének fokozott terhelést jelent a jogkivonat-szolgáltatás és az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="858b6-317">A short expiry time increases the load on both the device and the token service.</span></span>

<span data-ttu-id="858b6-318">Egy eszköz szeretne az elosztóhoz csatakoztatni, hozzá kell adnia továbbra is azt az IoT-központ identitásjegyzékhez – annak ellenére, hogy az eszköz használ egy jogkivonatot, és nem eszközkulcs való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="858b6-318">For a device to connect to your hub, you must still add it to the IoT Hub identity registry — even though the device is using a token and not a device key to connect.</span></span> <span data-ttu-id="858b6-319">Ezért továbbra is engedélyezésével vagy letiltásával eszköz identitásokat az eszköz hozzáférés-vezérlés használata a [identitásjegyzékhez][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="858b6-319">Therefore, you can continue to use per-device access control by enabling or disabling device identities in the [identity registry][lnk-identity-registry].</span></span> <span data-ttu-id="858b6-320">Ez a megközelítés azzal csökkenti az a tokenek használatára hosszú lejárati idő feltüntetésével kockázatát.</span><span class="sxs-lookup"><span data-stu-id="858b6-320">This approach mitigates the risks of using tokens with long expiry times.</span></span>

### <a name="comparison-with-a-custom-gateway"></a><span data-ttu-id="858b6-321">Egy egyéni átjáró összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="858b6-321">Comparison with a custom gateway</span></span>

<span data-ttu-id="858b6-322">A jogkivonat-szolgáltatás minta nem az ajánlott módszer az IoT-központot egy egyéni identitás rendszerleíró adatbázis-hitelesítési séma végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="858b6-322">The token service pattern is the recommended way to implement a custom identity registry/authentication scheme with IoT Hub.</span></span> <span data-ttu-id="858b6-323">Ez a minta használata ajánlott, mivel az IoT-központ továbbra is a megoldás forgalom nagy részét kezeli.</span><span class="sxs-lookup"><span data-stu-id="858b6-323">This pattern is recommended because IoT Hub continues to handle most of the solution traffic.</span></span> <span data-ttu-id="858b6-324">Azonban ha az egyéni hitelesítési séma így egymásba kapcsolódik, a protokollal, szükség lehet egy *egyéni átjáró* feldolgozni a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="858b6-324">However, if the custom authentication scheme is so intertwined with the protocol, you may require a *custom gateway* to process all the traffic.</span></span> <span data-ttu-id="858b6-325">Ilyen eset például használ[Transport Layer Security (TLS) és előmegosztott kulccsal (PSKs)][lnk-tls-psk].</span><span class="sxs-lookup"><span data-stu-id="858b6-325">An example of such a scenario is using[Transport Layer Security (TLS) and pre-shared keys (PSKs)][lnk-tls-psk].</span></span> <span data-ttu-id="858b6-326">További információkért lásd: a [protokoll-átjáró] [ lnk-protocols] témakör.</span><span class="sxs-lookup"><span data-stu-id="858b6-326">For more information, see the [protocol gateway][lnk-protocols] topic.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="858b6-327">Referencia-témaköreit:</span><span class="sxs-lookup"><span data-stu-id="858b6-327">Reference topics:</span></span>

<span data-ttu-id="858b6-328">A következő témaköröket további tudnivalókat az IoT hub hozzáférést biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="858b6-328">The following reference topics provide you with more information about controlling access to your IoT hub.</span></span>

## <a name="iot-hub-permissions"></a><span data-ttu-id="858b6-329">Az IoT-központ engedélyek</span><span class="sxs-lookup"><span data-stu-id="858b6-329">IoT Hub permissions</span></span>

<span data-ttu-id="858b6-330">Az alábbi táblázat az engedélyeket, használhatja az IoT hub való hozzáférés vezérlése érdekében.</span><span class="sxs-lookup"><span data-stu-id="858b6-330">The following table lists the permissions you can use to control access to your IoT hub.</span></span>

| <span data-ttu-id="858b6-331">Engedély</span><span class="sxs-lookup"><span data-stu-id="858b6-331">Permission</span></span> | <span data-ttu-id="858b6-332">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="858b6-332">Notes</span></span> |
| --- | --- |
| <span data-ttu-id="858b6-333">**RegistryRead**</span><span class="sxs-lookup"><span data-stu-id="858b6-333">**RegistryRead**</span></span> |<span data-ttu-id="858b6-334">Olvasási hozzáférést biztosít az identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="858b6-334">Grants read access to the identity registry.</span></span> <span data-ttu-id="858b6-335">További információkért lásd: [identitásjegyzékhez][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="858b6-335">For more information, see [Identity registry][lnk-identity-registry].</span></span> <br/><span data-ttu-id="858b6-336">Háttér-felhőszolgáltatások használja ezt az engedélyt.</span><span class="sxs-lookup"><span data-stu-id="858b6-336">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="858b6-337">**RegistryReadWrite**</span><span class="sxs-lookup"><span data-stu-id="858b6-337">**RegistryReadWrite**</span></span> |<span data-ttu-id="858b6-338">Biztosít az olvasási és írási hozzáférést biztosít az identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="858b6-338">Grants read and write access to the identity registry.</span></span> <span data-ttu-id="858b6-339">További információkért lásd: [identitásjegyzékhez][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="858b6-339">For more information, see [Identity registry][lnk-identity-registry].</span></span> <br/><span data-ttu-id="858b6-340">Háttér-felhőszolgáltatások használja ezt az engedélyt.</span><span class="sxs-lookup"><span data-stu-id="858b6-340">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="858b6-341">**ServiceConnect**</span><span class="sxs-lookup"><span data-stu-id="858b6-341">**ServiceConnect**</span></span> |<span data-ttu-id="858b6-342">Cloud service irányuló kommunikációra és figyelési végpontok biztosít hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="858b6-342">Grants access to cloud service-facing communication and monitoring endpoints.</span></span> <br/><span data-ttu-id="858b6-343">Engedélyt ad eszközről a felhőbe üzeneteket fogadni, felhőalapú-eszközre küldött üzenetek küldése és a megfelelő kézbesítési visszaigazolások beolvasása.</span><span class="sxs-lookup"><span data-stu-id="858b6-343">Grants permission to receive device-to-cloud messages, send cloud-to-device messages, and retrieve the corresponding delivery acknowledgments.</span></span> <br/><span data-ttu-id="858b6-344">Engedélyt beolvasása kézbesítési a nyugtázás a fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="858b6-344">Grants permission to retrieve delivery acknowledgements for file uploads.</span></span> <br/><span data-ttu-id="858b6-345">Engedélyt ad a címkék és a kívánt tulajdonságok frissítése, jelentett tulajdonságainak beolvasása és lekérdezéseket futtathat eszköz twins eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="858b6-345">Grants permission to access device twins to update tags and desired properties, retrieve reported properties, and run queries.</span></span> <br/><span data-ttu-id="858b6-346">Háttér-felhőszolgáltatások használja ezt az engedélyt.</span><span class="sxs-lookup"><span data-stu-id="858b6-346">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="858b6-347">**DeviceConnect**</span><span class="sxs-lookup"><span data-stu-id="858b6-347">**DeviceConnect**</span></span> |<span data-ttu-id="858b6-348">Engedélyezi a hozzáférést a eszköz felé néző végpontok.</span><span class="sxs-lookup"><span data-stu-id="858b6-348">Grants access to device-facing endpoints.</span></span> <br/><span data-ttu-id="858b6-349">Engedélyt ad felhő eszközre üzeneteket eszköz a felhőbe küldött üzeneteket küldjön és fogadjon.</span><span class="sxs-lookup"><span data-stu-id="858b6-349">Grants permission to send device-to-cloud messages and receive cloud-to-device messages.</span></span> <br/><span data-ttu-id="858b6-350">Engedélyt ad egy eszközről fájlfeltöltés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="858b6-350">Grants permission to perform file upload from a device.</span></span> <br/><span data-ttu-id="858b6-351">Eszköz szükséges iker tulajdonság értesítéseket, és frissítse az eszköz a két engedélyt jelentett tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="858b6-351">Grants permission to receive device twin desired property notifications and update device twin reported properties.</span></span> <br/><span data-ttu-id="858b6-352">Engedélyt hajtsa végre a fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="858b6-352">Grants permission to perform file uploads.</span></span> <br/><span data-ttu-id="858b6-353">Ez az engedély eszközöket használják.</span><span class="sxs-lookup"><span data-stu-id="858b6-353">This permission is used by devices.</span></span> |

## <a name="additional-reference-material"></a><span data-ttu-id="858b6-354">További referenciaanyag</span><span class="sxs-lookup"><span data-stu-id="858b6-354">Additional reference material</span></span>

<span data-ttu-id="858b6-355">Az IoT Hub fejlesztői útmutató más hivatkozás témaköröket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="858b6-355">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="858b6-356">[IoT-központok végpontjai] [ lnk-endpoints] ismerteti a különböző végpontok, amelyek minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek.</span><span class="sxs-lookup"><span data-stu-id="858b6-356">[IoT Hub endpoints][lnk-endpoints] describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="858b6-357">[Sávszélesség-szabályozási és kvóták] [ lnk-quotas] ismerteti a kvóták és a szabályozás viselkedéseket, amelyek az IoT-központ szolgáltatás vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="858b6-357">[Throttling and quotas][lnk-quotas] describes the quotas and throttling behaviors that apply to the IoT Hub service.</span></span>
* <span data-ttu-id="858b6-358">[Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] felsorolja a különböző nyelvi használhatja az eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor SDK-k.</span><span class="sxs-lookup"><span data-stu-id="858b6-358">[Azure IoT device and service SDKs][lnk-sdks] lists the various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="858b6-359">[Az IoT-központ lekérdezési nyelv] [ lnk-query] a lekérdezési nyelv segítségével adatok lekérését az IoT-központ az eszköz twins és feladatokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="858b6-359">[IoT Hub query language][lnk-query] describes the query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="858b6-360">[Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] IoT-központ támogatásával kapcsolatos további információkat biztosít a MQTT protokoll.</span><span class="sxs-lookup"><span data-stu-id="858b6-360">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="858b6-361">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="858b6-361">Next steps</span></span>

<span data-ttu-id="858b6-362">Most már rendelkezik megtudta, hogyan való hozzáférést az IoT-központ, a következő IoT Hub fejlesztői útmutató témakörei iránt érdeklődik esetleg:</span><span class="sxs-lookup"><span data-stu-id="858b6-362">Now you have learned how to control access IoT Hub, you may be interested in the following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="858b6-363">[Eszköz twins segítségével szinkronizálja az állapotot és konfigurációk][lnk-devguide-device-twins]</span><span class="sxs-lookup"><span data-stu-id="858b6-363">[Use device twins to synchronize state and configurations][lnk-devguide-device-twins]</span></span>
* <span data-ttu-id="858b6-364">[Az eszközön közvetlen metódus][lnk-devguide-directmethods]</span><span class="sxs-lookup"><span data-stu-id="858b6-364">[Invoke a direct method on a device][lnk-devguide-directmethods]</span></span>
* <span data-ttu-id="858b6-365">[Több eszközön feladatok ütemezése][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="858b6-365">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="858b6-366">Ha azt szeretné, hogy próbálja ki azokat a jelen cikkben ismertetett fogalmakat, esetleg érdekli az IoT-központ anyagra:</span><span class="sxs-lookup"><span data-stu-id="858b6-366">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorials:</span></span>

* <span data-ttu-id="858b6-367">[Ismerkedés az Azure IoT Hub][lnk-getstarted-tutorial]</span><span class="sxs-lookup"><span data-stu-id="858b6-367">[Get started with Azure IoT Hub][lnk-getstarted-tutorial]</span></span>
* <span data-ttu-id="858b6-368">[Az IoT hubbal felhő eszközre üzenetek küldése][lnk-c2d-tutorial]</span><span class="sxs-lookup"><span data-stu-id="858b6-368">[How to send cloud-to-device messages with IoT Hub][lnk-c2d-tutorial]</span></span>
* <span data-ttu-id="858b6-369">[Az IoT-központ eszközről a felhőbe üzenetek feldolgozása][lnk-d2c-tutorial]</span><span class="sxs-lookup"><span data-stu-id="858b6-369">[How to process IoT Hub device-to-cloud messages][lnk-d2c-tutorial]</span></span>

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth._iot_hub_service_sas_token
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
