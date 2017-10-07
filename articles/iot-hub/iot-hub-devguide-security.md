---
title: "Azure IoT Hub biztonsági aaaUnderstand |} Microsoft Docs"
description: "Fejlesztői útmutató - hogyan férhetnek hozzá a toocontrol tooIoT Hub eszköz alkalmazások és a háttér-alkalmazásokat. Biztonsági jogkivonatok, és támogatja az x.509 szabványú tanúsítványokban kapcsolatos adatokat tartalmaz."
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
ms.openlocfilehash: 717726328a6bb5c5c334a123d0abfed711b2c3b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="control-access-tooiot-hub"></a><span data-ttu-id="a6dbc-104">Központi hozzáférési tooIoT szabályozása</span><span class="sxs-lookup"><span data-stu-id="a6dbc-104">Control access tooIoT Hub</span></span>

<span data-ttu-id="a6dbc-105">Ez a cikk ismerteti az IoT hub biztonságához hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-105">This article describes hello options for securing your IoT hub.</span></span> <span data-ttu-id="a6dbc-106">Az IoT-központ használ *engedélyek* toogrant hozzáférés tooeach IoT-központ végpontjának.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-106">IoT Hub uses *permissions* toogrant access tooeach IoT hub endpoint.</span></span> <span data-ttu-id="a6dbc-107">Engedélyek korlátozása hello hozzáférés tooan IoT hub funkciókon alapulnak.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-107">Permissions limit hello access tooan IoT hub based on functionality.</span></span>

<span data-ttu-id="a6dbc-108">Ez a cikk ismerteti:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-108">This article describes:</span></span>

* <span data-ttu-id="a6dbc-109">hello különböző engedélyeket, hogy is meg lehet adni tooa eszköz vagy a háttér-alkalmazás tooaccess az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-109">hello different permissions that you can grant tooa device or back-end app tooaccess your IoT hub.</span></span>
* <span data-ttu-id="a6dbc-110">hello tooverify engedélyeket használ a hitelesítési folyamat és hello tokenek.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-110">hello authentication process and hello tokens it uses tooverify permissions.</span></span>
* <span data-ttu-id="a6dbc-111">Hogyan tooscope hitelesítő adatok toolimit toospecific-erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-111">How tooscope credentials toolimit access toospecific resources.</span></span>
* <span data-ttu-id="a6dbc-112">IoT Hub-támogatás az x.509 szabványú tanúsítványokban.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-112">IoT Hub support for X.509 certificates.</span></span>
* <span data-ttu-id="a6dbc-113">Egyéni hitelesítési mechanizmusok, amelyek használják a meglévő eszköz identitása nyilvántartó vagy hitelesítési sémák.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-113">Custom device authentication mechanisms that use existing device identity registries or authentication schemes.</span></span>

### <a name="when-toouse"></a><span data-ttu-id="a6dbc-114">Ha toouse</span><span class="sxs-lookup"><span data-stu-id="a6dbc-114">When toouse</span></span>

<span data-ttu-id="a6dbc-115">Szükséges megfelelő engedélyek tooaccess bármelyik hello IoT-központok végpontjai.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-115">You must have appropriate permissions tooaccess any of hello IoT Hub endpoints.</span></span> <span data-ttu-id="a6dbc-116">Például egy eszköz tartalmaznia kell egy jogkivonatot tartalmazó biztonsági hitelesítő adatokat, és minden üzenetet küld tooIoT Hub.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-116">For example, a device must include a token containing security credentials along with every message it sends tooIoT Hub.</span></span>

## <a name="access-control-and-permissions"></a><span data-ttu-id="a6dbc-117">Hozzáférés-vezérlés és engedélyek</span><span class="sxs-lookup"><span data-stu-id="a6dbc-117">Access control and permissions</span></span>

<span data-ttu-id="a6dbc-118">Meg lehet adni [engedélyek](#iot-hub-permissions) a következő módokon hello:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-118">You can grant [permissions](#iot-hub-permissions) in hello following ways:</span></span>

* <span data-ttu-id="a6dbc-119">**Az IoT hub-szintű megosztott elérési házirendeket**.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-119">**IoT hub-level shared access policies**.</span></span> <span data-ttu-id="a6dbc-120">Megosztott elérési házirendeket is biztosítani bármilyen kombinációját [engedélyek](#iot-hub-permissions).</span><span class="sxs-lookup"><span data-stu-id="a6dbc-120">Shared access policies can grant any combination of [permissions](#iot-hub-permissions).</span></span> <span data-ttu-id="a6dbc-121">Házirendek megadására a hello [Azure-portálon][lnk-management-portal], vagy programozott módon hello segítségével [IoT-központ erőforrás-szolgáltató REST API-k][lnk-resource-provider-apis].</span><span class="sxs-lookup"><span data-stu-id="a6dbc-121">You can define policies in hello [Azure portal][lnk-management-portal], or programmatically by using hello [IoT Hub resource provider REST APIs][lnk-resource-provider-apis].</span></span> <span data-ttu-id="a6dbc-122">Egy újonnan létrehozott IoT-központ rendelkezik a következő alapértelmezett házirendek hello:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-122">A newly created IoT hub has hello following default policies:</span></span>

  * <span data-ttu-id="a6dbc-123">**iothubowner**: házirend az összes engedélyt.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-123">**iothubowner**: Policy with all permissions.</span></span>
  * <span data-ttu-id="a6dbc-124">**szolgáltatás**: a házirend **ServiceConnect** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-124">**service**: Policy with **ServiceConnect** permission.</span></span>
  * <span data-ttu-id="a6dbc-125">**eszköz**: a házirend **DeviceConnect** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-125">**device**: Policy with **DeviceConnect** permission.</span></span>
  * <span data-ttu-id="a6dbc-126">**registryRead**: a házirend **RegistryRead** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-126">**registryRead**: Policy with **RegistryRead** permission.</span></span>
  * <span data-ttu-id="a6dbc-127">**registryReadWrite**: a házirend **RegistryRead** és RegistryWrite engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-127">**registryReadWrite**: Policy with **RegistryRead** and RegistryWrite permissions.</span></span>
  * <span data-ttu-id="a6dbc-128">**Eszköz hitelesítő adatokat**.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-128">**Per-device security credentials**.</span></span> <span data-ttu-id="a6dbc-129">Minden egyes IoT-központ tartalmaz egy [identitásjegyzékhez][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="a6dbc-129">Each IoT Hub contains an [identity registry][lnk-identity-registry].</span></span> <span data-ttu-id="a6dbc-130">A identitásjegyzékhez az eszközökhöz, konfigurálhatja a hitelesítő adatokat, amelyek biztosítani **DeviceConnect** engedélyek hatókörű toohello megfelelő eszköz végpontok.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-130">For each device in this identity registry, you can configure security credentials that grant **DeviceConnect** permissions scoped toohello corresponding device endpoints.</span></span>

<span data-ttu-id="a6dbc-131">Például a egy tipikus IoT-megoldás:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-131">For example, in a typical IoT solution:</span></span>

* <span data-ttu-id="a6dbc-132">hello eszköz felügyeleti összetevő használja hello *registryReadWrite* házirend.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-132">hello device management component uses hello *registryReadWrite* policy.</span></span>
* <span data-ttu-id="a6dbc-133">hello esemény feldolgozó összetevő használja hello *szolgáltatás* házirend.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-133">hello event processor component uses hello *service* policy.</span></span>
* <span data-ttu-id="a6dbc-134">hello futásidejű eszköz üzleti logika egyik összetevője hello *szolgáltatás* házirend.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-134">hello run-time device business logic component uses hello *service* policy.</span></span>
* <span data-ttu-id="a6dbc-135">Egyes eszközök hello IoT hub identitásjegyzékhez tárolt hitelesítő adatok keresztül csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-135">Individual devices connect using credentials stored in hello IoT hub's identity registry.</span></span>

> [!NOTE]
> <span data-ttu-id="a6dbc-136">Lásd: [engedélyek](#iot-hub-permissions) részletes információkat.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-136">See [permissions](#iot-hub-permissions) for detailed information.</span></span>

## <a name="authentication"></a><span data-ttu-id="a6dbc-137">Authentication</span><span class="sxs-lookup"><span data-stu-id="a6dbc-137">Authentication</span></span>

<span data-ttu-id="a6dbc-138">Azure IoT Hub hozzáférés tooendpoints ellenőrzésével egy token hello megosztott hozzáférési házirendek és identitás beállításjegyzék hitelesítő adatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-138">Azure IoT Hub grants access tooendpoints by verifying a token against hello shared access policies and identity registry security credentials.</span></span>

<span data-ttu-id="a6dbc-139">Biztonsági hitelesítő adatok, például a szimmetrikus kulcsokat, soha nem kerülnek hello hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-139">Security credentials, such as symmetric keys, are never sent over hello wire.</span></span>

> [!NOTE]
> <span data-ttu-id="a6dbc-140">hello Azure IoT Hub erőforrás-szolgáltató védett keresztül az Azure-előfizetéshez, mert hello lévő összes szolgáltatónál [Azure Resource Manager][lnk-azure-resource-manager].</span><span class="sxs-lookup"><span data-stu-id="a6dbc-140">hello Azure IoT Hub resource provider is secured through your Azure subscription, as are all providers in hello [Azure Resource Manager][lnk-azure-resource-manager].</span></span>

<span data-ttu-id="a6dbc-141">További információ tooconstruct és -felhasználási biztonsági jogkivonatokat, lásd: [IoT-központ biztonsági jogkivonatokat][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="a6dbc-141">For more information about how tooconstruct and use security tokens, see [IoT Hub security tokens][lnk-sas-tokens].</span></span>

### <a name="protocol-specifics"></a><span data-ttu-id="a6dbc-142">Protokoll rögzítésen</span><span class="sxs-lookup"><span data-stu-id="a6dbc-142">Protocol specifics</span></span>

<span data-ttu-id="a6dbc-143">Minden támogatott protokollok, például MQTT AMQP vagy HTTP, jogkivonatok szállításokkal különböző módon.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-143">Each supported protocol, such as MQTT, AMQP, and HTTP, transports tokens in different ways.</span></span>

<span data-ttu-id="a6dbc-144">MQTT használatakor hello CONNECT csomag megegyezik hello deviceId hello ClientId, {iothubhostname} / {deviceId} hello felhasználónév mezőbe, és egy SAS-jogkivonat hello jelszó mezőben.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-144">When using MQTT, hello CONNECT packet has hello deviceId as hello ClientId, {iothubhostname}/{deviceId} in hello Username field, and a SAS token in hello Password field.</span></span> <span data-ttu-id="a6dbc-145">{iothubhostname} hello IoT hub (például contoso.azure-devices.net) teljes CName hello kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-145">{iothubhostname} should be hello full CName of hello IoT hub (for example, contoso.azure-devices.net).</span></span>

<span data-ttu-id="a6dbc-146">Használata esetén [AMQP][lnk-amqp], IoT-Központ támogatja [SASL egyszerű] [ lnk-sasl-plain] és [AMQP jogcím-alapú-biztonsági][lnk-cbs].</span><span class="sxs-lookup"><span data-stu-id="a6dbc-146">When using [AMQP][lnk-amqp], IoT Hub supports [SASL PLAIN][lnk-sasl-plain] and [AMQP Claims-Based-Security][lnk-cbs].</span></span>

<span data-ttu-id="a6dbc-147">Ha AMQP jogcím-alapú-biztonsági használja, szabványos hello határozza meg hogy tootransmit ezeket a jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-147">If you use AMQP claims-based-security, hello standard specifies how tootransmit these tokens.</span></span>

<span data-ttu-id="a6dbc-148">SASL egyszerű hello **felhasználónév** lehet:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-148">For SASL PLAIN, hello **username** can be:</span></span>

* <span data-ttu-id="a6dbc-149">`{policyName}@sas.root.{iothubName}`Ha az IoT hub-szintű jogkivonatok használatával.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-149">`{policyName}@sas.root.{iothubName}` if using IoT hub-level tokens.</span></span>
* <span data-ttu-id="a6dbc-150">`{deviceId}@sas.{iothubname}`Ha az eszköz hatókörű jogkivonatok használatával.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-150">`{deviceId}@sas.{iothubname}` if using device-scoped tokens.</span></span>

<span data-ttu-id="a6dbc-151">Mindkét esetben a jelszó mező hello hello jogkivonatot tartalmaz, a [IoT-központ biztonsági jogkivonatokat][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="a6dbc-151">In both cases, hello password field contains hello token, as described in [IoT Hub security tokens][lnk-sas-tokens].</span></span>

<span data-ttu-id="a6dbc-152">HTTP megvalósítja a hitelesítési belefoglalja egy érvényes tokent hello **engedélyezési** kérelemfejlécet.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-152">HTTP implements authentication by including a valid token in hello **Authorization** request header.</span></span>

#### <a name="example"></a><span data-ttu-id="a6dbc-153">Példa</span><span class="sxs-lookup"><span data-stu-id="a6dbc-153">Example</span></span>

<span data-ttu-id="a6dbc-154">Felhasználónév (DeviceId a kis-és nagybetűket):`iothubname.azure-devices.net/DeviceId`</span><span class="sxs-lookup"><span data-stu-id="a6dbc-154">Username (DeviceId is case-sensitive): `iothubname.azure-devices.net/DeviceId`</span></span>

<span data-ttu-id="a6dbc-155">Jelszó (készítése SAS-jogkivonatot az hello [eszköz explorer] [ lnk-device-explorer] eszköz):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`</span><span class="sxs-lookup"><span data-stu-id="a6dbc-155">Password (Generate SAS token with hello [device explorer][lnk-device-explorer] tool): `SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`</span></span>

> [!NOTE]
> <span data-ttu-id="a6dbc-156">Hello [Azure IoT SDK-k] [ lnk-sdks] automatikusan hoz létre jogkivonatokat toohello szolgáltatáshoz való kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-156">hello [Azure IoT SDKs][lnk-sdks] automatically generate tokens when connecting toohello service.</span></span> <span data-ttu-id="a6dbc-157">Bizonyos esetekben hello Azure IoT SDK-k nem támogatják az hello protokollok vagy hello hitelesítési módszerek mindegyikéhez.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-157">In some cases, hello Azure IoT SDKs do not support all hello protocols or all hello authentication methods.</span></span>

### <a name="special-considerations-for-sasl-plain"></a><span data-ttu-id="a6dbc-158">Különleges szempontok SASL egyszerű</span><span class="sxs-lookup"><span data-stu-id="a6dbc-158">Special considerations for SASL PLAIN</span></span>

<span data-ttu-id="a6dbc-159">Az AMQP SASL egyszerű használatakor ügyfél csatlakozik az IoT-központ tooan egyetlen jogkivonat a TCP-kapcsolatokat használhatja.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-159">When using SASL PLAIN with AMQP, a client connecting tooan IoT hub can use a single token for each TCP connection.</span></span> <span data-ttu-id="a6dbc-160">Amikor hello-token érvényessége lejár, a hello TCP-kapcsolatot bontja a hello szolgáltatást, és újracsatlakozás elindítja.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-160">When hello token expires, hello TCP connection disconnects from hello service and triggers a reconnect.</span></span> <span data-ttu-id="a6dbc-161">Ez a viselkedés, amíg nem jelent problémát, egy háttér-alkalmazás van káros hello a következő okok miatt az eszköz alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-161">This behavior, while not problematic for a back-end app, is damaging for a device app for hello following reasons:</span></span>

* <span data-ttu-id="a6dbc-162">Átjárók általában sok eszközök nevében csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-162">Gateways usually connect on behalf of many devices.</span></span> <span data-ttu-id="a6dbc-163">SASL egyszerű használatakor rendelkeznek toocreate az egyes eszközök csatlakoztatása az IoT-központ tooan különböző TCP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-163">When using SASL PLAIN, they have toocreate a distinct TCP connection for each device connecting tooan IoT hub.</span></span> <span data-ttu-id="a6dbc-164">Ebben a forgatókönyvben jelentősen növeli a teljesítmény és a hálózati erőforrások fogyasztásának hello, és növeli a hello késés minden eszköz kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-164">This scenario considerably increases hello consumption of power and networking resources, and increases hello latency of each device connection.</span></span>
* <span data-ttu-id="a6dbc-165">Erőforrás-korlátozott eszközök vannak hátrányosan erőforrások tooreconnect nőtt hello használata minden egyes jogkivonat lejárata után.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-165">Resource-constrained devices are adversely affected by hello increased use of resources tooreconnect after each token expiration.</span></span>

## <a name="scope-iot-hub-level-credentials"></a><span data-ttu-id="a6dbc-166">Hatókör IoT hub-szintű hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="a6dbc-166">Scope IoT hub-level credentials</span></span>

<span data-ttu-id="a6dbc-167">Az IoT hub-szintű biztonsági házirendek hatókörét megadhatja a tokenek egy korlátozott erőforrás URI létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-167">You can scope IoT hub-level security policies by creating tokens with a restricted resource URI.</span></span> <span data-ttu-id="a6dbc-168">Például a végpont toosend eszközről a felhőbe köszönőüzenetei egy eszközről van **/devices/ {deviceId} / üzenetek/események**.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-168">For example, hello endpoint toosend device-to-cloud messages from a device is **/devices/{deviceId}/messages/events**.</span></span> <span data-ttu-id="a6dbc-169">Használhatja az IoT hub-szintű megosztott elérési házirend **DeviceConnect** engedélyek toosign egy jogkivonatot, amelynek resourceURI van **/devices/ {deviceId}**.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-169">You can also use an IoT hub-level shared access policy with **DeviceConnect** permissions toosign a token whose resourceURI is **/devices/{deviceId}**.</span></span> <span data-ttu-id="a6dbc-170">Ez a megközelítés létrehoz egy jogkivonatot, amely csak akkor használható toosend üzenetek eszköz nevében **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-170">This approach creates a token that is only usable toosend messages on behalf of device **deviceId**.</span></span>

<span data-ttu-id="a6dbc-171">Ez a módszer használata hasonló toohello [Event Hubs közzétevői házirend][lnk-event-hubs-publisher-policy], és lehetővé teszi a tooimplement egyéni hitelesítési módszerek.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-171">This mechanism is similar toohello [Event Hubs publisher policy][lnk-event-hubs-publisher-policy], and enables you tooimplement custom authentication methods.</span></span>

## <a name="security-tokens"></a><span data-ttu-id="a6dbc-172">Biztonsági jogkivonatok kiosztva</span><span class="sxs-lookup"><span data-stu-id="a6dbc-172">Security tokens</span></span>

<span data-ttu-id="a6dbc-173">Az IoT-központ által használt biztonsági jogkivonatok tooauthenticate eszközöket, és a szolgáltatások tooavoid kulcsok küldése hello keresztülhaladnak a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-173">IoT Hub uses security tokens tooauthenticate devices and services tooavoid sending keys on hello wire.</span></span> <span data-ttu-id="a6dbc-174">Emellett biztonsági jogkivonatok érvényesség és hatókör korlátozott.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-174">Additionally, security tokens are limited in time validity and scope.</span></span> <span data-ttu-id="a6dbc-175">[Azure IoT SDK-k] [ lnk-sdks] automatikusan hoz létre jogkivonatokat anélkül, hogy semmiféle speciális beállítást.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-175">[Azure IoT SDKs][lnk-sdks] automatically generate tokens without requiring any special configuration.</span></span> <span data-ttu-id="a6dbc-176">Bizonyos esetekben kell toogenerate és biztonsági jogkivonatokat közvetlenül használható.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-176">Some scenarios do require you toogenerate and use security tokens directly.</span></span> <span data-ttu-id="a6dbc-177">Ilyen forgatókönyvek például a következők:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-177">Such scenarios include:</span></span>

* <span data-ttu-id="a6dbc-178">hello hello MQTT, az AMQP vagy a HTTP-felületek közvetlen használatát.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-178">hello direct use of hello MQTT, AMQP, or HTTP surfaces.</span></span>
* <span data-ttu-id="a6dbc-179">hello végrehajtásának hello biztonságijogkivonat-szolgáltatás a mintában a [egyéni eszközhitelesítés][lnk-custom-auth].</span><span class="sxs-lookup"><span data-stu-id="a6dbc-179">hello implementation of hello token service pattern, as explained in [Custom device authentication][lnk-custom-auth].</span></span>

<span data-ttu-id="a6dbc-180">Az IoT-központ is lehetővé teszi, hogy eszközök tooauthenticate az IoT-központ [X.509-tanúsítványokat][lnk-x509].</span><span class="sxs-lookup"><span data-stu-id="a6dbc-180">IoT Hub also allows devices tooauthenticate with IoT Hub using [X.509 certificates][lnk-x509].</span></span>

### <a name="security-token-structure"></a><span data-ttu-id="a6dbc-181">Biztonsági jogkivonat szerkezete</span><span class="sxs-lookup"><span data-stu-id="a6dbc-181">Security token structure</span></span>

<span data-ttu-id="a6dbc-182">Biztonsági jogkivonatok toogrant idő határolt hozzáférés toodevices és a szolgáltatások toospecific a funkcióival IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-182">You use security tokens toogrant time-bounded access toodevices and services toospecific functionality in IoT Hub.</span></span> <span data-ttu-id="a6dbc-183">tooget engedélyezési tooconnect tooIoT Hub, eszközöket és szolgáltatásokat kell küldenie egy közös hozzáférésű vagy a szimmetrikus kulcs aláírva biztonsági jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-183">tooget authorization tooconnect tooIoT Hub, devices and services must send security tokens signed with either a shared access or symmetric key.</span></span> <span data-ttu-id="a6dbc-184">Ezek a kulcsok egy eszközidentitás hello identitás beállításjegyzék tárolja.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-184">These keys are stored with a device identity in hello identity registry.</span></span>

<span data-ttu-id="a6dbc-185">A token egy megosztott elérési kulcs biztosít hozzáférést tooall hello funkció hello megosztott hozzáférési házirend engedélyek társított aláírva.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-185">A token signed with a shared access key grants access tooall hello functionality associated with hello shared access policy permissions.</span></span> <span data-ttu-id="a6dbc-186">Egy eszközidentitás szimmetrikus kulcs csak biztosít hello aláírt jogkivonat **DeviceConnect** hello engedély társított eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-186">A token signed with a device identity's symmetric key only grants hello **DeviceConnect** permission for hello associated device identity.</span></span>

<span data-ttu-id="a6dbc-187">hello biztonsági jogkivonatának hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-187">hello security token has hello following format:</span></span>

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

<span data-ttu-id="a6dbc-188">Az alábbiakban hello várt értékek:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-188">Here are hello expected values:</span></span>

| <span data-ttu-id="a6dbc-189">Érték</span><span class="sxs-lookup"><span data-stu-id="a6dbc-189">Value</span></span> | <span data-ttu-id="a6dbc-190">Leírás</span><span class="sxs-lookup"><span data-stu-id="a6dbc-190">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a6dbc-191">{aláírás}</span><span class="sxs-lookup"><span data-stu-id="a6dbc-191">{signature}</span></span> |<span data-ttu-id="a6dbc-192">Hello űrlap aláírás HMAC-SHA256 karakterláncra: `{URL-encoded-resourceURI} + "\n" + expiry`.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-192">An HMAC-SHA256 signature string of hello form: `{URL-encoded-resourceURI} + "\n" + expiry`.</span></span> <span data-ttu-id="a6dbc-193">**Fontos**: hello kulcsot dekódolni a Base64 kódolású anyag és kulcs tooperform hello HMAC-SHA256 számítási használja.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-193">**Important**: hello key is decoded from base64 and used as key tooperform hello HMAC-SHA256 computation.</span></span> |
| <span data-ttu-id="a6dbc-194">{resourceURI}</span><span class="sxs-lookup"><span data-stu-id="a6dbc-194">{resourceURI}</span></span> |<span data-ttu-id="a6dbc-195">URI-előtag (által szegmens) hello végpontok a jogkivonatok hello IoT-központ (protokoll) állomásneve kezdődően elérhető.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-195">URI prefix (by segment) of hello endpoints that can be accessed with this token, starting with host name of hello IoT hub (no protocol).</span></span> <span data-ttu-id="a6dbc-196">Például: `myHub.azure-devices.net/devices/device1`</span><span class="sxs-lookup"><span data-stu-id="a6dbc-196">For example, `myHub.azure-devices.net/devices/device1`</span></span> |
| <span data-ttu-id="a6dbc-197">{a lejárati}</span><span class="sxs-lookup"><span data-stu-id="a6dbc-197">{expiry}</span></span> |<span data-ttu-id="a6dbc-198">UTF8 karakterláncok hello epoch 00:00:00 UTC 1970. január 1. a másodpercek száma.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-198">UTF8 strings for number of seconds since hello epoch 00:00:00 UTC on 1 January 1970.</span></span> |
| <span data-ttu-id="a6dbc-199">{URL-kódolású-resourceURI}</span><span class="sxs-lookup"><span data-stu-id="a6dbc-199">{URL-encoded-resourceURI}</span></span> |<span data-ttu-id="a6dbc-200">Alacsonyabb eset az URL-kódolást hello kisbetű erőforrás URI</span><span class="sxs-lookup"><span data-stu-id="a6dbc-200">Lower case URL-encoding of hello lower case resource URI</span></span> |
| <span data-ttu-id="a6dbc-201">{Házirendnév}</span><span class="sxs-lookup"><span data-stu-id="a6dbc-201">{policyName}</span></span> |<span data-ttu-id="a6dbc-202">Ez a token hivatkozik hello megosztott hozzáférési házirend toowhich hello neve.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-202">hello name of hello shared access policy toowhich this token refers.</span></span> <span data-ttu-id="a6dbc-203">Hiányzik, ha hello token hivatkozik toodevice-beállításjegyzék hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-203">Absent if hello token refers toodevice-registry credentials.</span></span> |

<span data-ttu-id="a6dbc-204">**Megjegyzés: előtag alapján**: hello URI-előtag számított szegmens és nem karakter.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-204">**Note on prefix**: hello URI prefix is computed by segment and not by character.</span></span> <span data-ttu-id="a6dbc-205">Például `/a/b` előtagja, az `/a/b/c` nem `/a/bc`.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-205">For example `/a/b` is a prefix for `/a/b/c` but not for `/a/bc`.</span></span>

<span data-ttu-id="a6dbc-206">hello Node.js alábbi kódrészletben láthatja a hívott függvény **generateSasToken** , hogy a hello bemenetei token kiszámítja hello `resourceUri, signingKey, policyName, expiresInMins`.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-206">hello following Node.js snippet shows a function called **generateSasToken** that computes hello token from hello inputs `resourceUri, signingKey, policyName, expiresInMins`.</span></span> <span data-ttu-id="a6dbc-207">hello következő részek ismertetik, hogyan tooinitialize hello különböző bemenetek hello különböző jogkivonat használati esetekben.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-207">hello next sections detail how tooinitialize hello different inputs for hello different token use cases.</span></span>

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

<span data-ttu-id="a6dbc-208">Összehasonlítása, mint a biztonsági jogkivonat egyenértékű Python kódját toogenerate hello:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-208">As a comparison, hello equivalent Python code toogenerate a security token is:</span></span>

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
> <span data-ttu-id="a6dbc-209">Hello érvényesség hello jogkivonat IoT-központ gépeken van hitelesítve, mivel hello óra hello gép hello token generáló hello eltéréseket minimális kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-209">Since hello time validity of hello token is validated on IoT Hub machines, hello drift on hello clock of hello machine that generates hello token must be minimal.</span></span>

### <a name="use-sas-tokens-in-a-device-app"></a><span data-ttu-id="a6dbc-210">Használja a SAS-tokenje eszköz alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="a6dbc-210">Use SAS tokens in a device app</span></span>

<span data-ttu-id="a6dbc-211">Nincsenek két módon tooobtain **DeviceConnect** IoT-központ engedélyeket a biztonsági jogkivonatokat: használja egy [eszköz szimmetrikus kulcsot az hello identitásjegyzékhez](#use-a-symmetric-key-in-the-identity-registry), vagy használjon egy [megosztottelérésikulcsot](#use-a-shared-access-policy).</span><span class="sxs-lookup"><span data-stu-id="a6dbc-211">There are two ways tooobtain **DeviceConnect** permissions with IoT Hub with security tokens: use a [symmetric device key from hello identity registry](#use-a-symmetric-key-in-the-identity-registry), or use a [shared access key](#use-a-shared-access-policy).</span></span>

<span data-ttu-id="a6dbc-212">Ne feledje, hogy eszközökhöz elérhető összes funkciót tesz elérhetővé előtaggal rendelkező végpontokon tervezési `/devices/{deviceId}`.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-212">Remember that all functionality accessible from devices is exposed by design on endpoints with prefix `/devices/{deviceId}`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a6dbc-213">hello csak úgy, hogy az IoT-központ végez hitelesítést egy adott eszköz hello eszköz identitása szimmetrikus kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-213">hello only way that IoT Hub authenticates a specific device is using hello device identity symmetric key.</span></span> <span data-ttu-id="a6dbc-214">Azokban az esetekben, amikor egy megosztott elérési házirendet használt tooaccess eszköz funkciót, hello megoldás figyelembe kell vennie megbízható részletes hello biztonsági jogkivonat kiállító hello összetevő.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-214">In cases when a shared access policy is used tooaccess device functionality, hello solution must consider hello component issuing hello security token as a trusted subcomponent.</span></span>

<span data-ttu-id="a6dbc-215">hello eszköz felé néző végpontok (függetlenül hello protocol) a következők:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-215">hello device-facing endpoints are (irrespective of hello protocol):</span></span>

| <span data-ttu-id="a6dbc-216">Végpont</span><span class="sxs-lookup"><span data-stu-id="a6dbc-216">Endpoint</span></span> | <span data-ttu-id="a6dbc-217">Funkció</span><span class="sxs-lookup"><span data-stu-id="a6dbc-217">Functionality</span></span> |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |<span data-ttu-id="a6dbc-218">Eszköz-felhő üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-218">Send device-to-cloud messages.</span></span> |
| `{iot hub host name}/devices/{deviceId}/devicebound` |<span data-ttu-id="a6dbc-219">Felhő eszközre üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-219">Receive cloud-to-device messages.</span></span> |

### <a name="use-a-symmetric-key-in-hello-identity-registry"></a><span data-ttu-id="a6dbc-220">Hello identitásjegyzékhez a szimmetrikus kulcs használata</span><span class="sxs-lookup"><span data-stu-id="a6dbc-220">Use a symmetric key in hello identity registry</span></span>

<span data-ttu-id="a6dbc-221">Ha egy eszközidentitás szimmetrikus kulcs toogenerate jogkivonatot, hello házirendnév (`skn`) elem hello jogkivonat nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-221">When using a device identity's symmetric key toogenerate a token, hello policyName (`skn`) element of hello token is omitted.</span></span>

<span data-ttu-id="a6dbc-222">Például, egy token létre tooaccess összes eszköz funkciót kell rendelkeznie hello a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-222">For example, a token created tooaccess all device functionality should have hello following parameters:</span></span>

* <span data-ttu-id="a6dbc-223">erőforrás URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span><span class="sxs-lookup"><span data-stu-id="a6dbc-223">resource URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span></span>
* <span data-ttu-id="a6dbc-224">aláírási kulcs: hello bármely szimmetrikus kulcs `{device id}` identitása</span><span class="sxs-lookup"><span data-stu-id="a6dbc-224">signing key: any symmetric key for hello `{device id}` identity,</span></span>
* <span data-ttu-id="a6dbc-225">Nincs házirend neve</span><span class="sxs-lookup"><span data-stu-id="a6dbc-225">no policy name,</span></span>
* <span data-ttu-id="a6dbc-226">a lejárati idő.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-226">any expiration time.</span></span>

<span data-ttu-id="a6dbc-227">Node.js-függvény megelőző hello segítségével például a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-227">An example using hello preceding Node.js function would be:</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

<span data-ttu-id="a6dbc-228">hello eredményt – amely device1 hozzáférés tooall funkciókat biztosít, a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-228">hello result, which grants access tooall functionality for device1, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> <span data-ttu-id="a6dbc-229">Azt lehetséges toogenerate van egy SAS-jogkivonat használatával hello .NET [eszköz explorer] [ lnk-device-explorer] eszköz vagy platformok, csomópont-alapú hello [IOT hubbal-explorer] [ lnk-iothub-explorer] parancssori segédprogram.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-229">It is possible toogenerate a SAS token using hello .NET [device explorer][lnk-device-explorer] tool or hello cross-platform, node-based [iothub-explorer][lnk-iothub-explorer] command-line utility.</span></span>

### <a name="use-a-shared-access-policy"></a><span data-ttu-id="a6dbc-230">Egy megosztott elérési házirendet használja</span><span class="sxs-lookup"><span data-stu-id="a6dbc-230">Use a shared access policy</span></span>

<span data-ttu-id="a6dbc-231">Amikor egy jogkivonatot hoz létre egy megosztott elérési házirendet, állítsa be a hello `skn` mező toohello hello házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-231">When you create a token from a shared access policy, set hello `skn` field toohello name of hello policy.</span></span> <span data-ttu-id="a6dbc-232">Ez a házirend biztosítania kell a hello **DeviceConnect** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-232">This policy must grant hello **DeviceConnect** permission.</span></span>

<span data-ttu-id="a6dbc-233">hello kétféle módon történhet a megosztott elérési házirendek tooaccess eszköz segítségével a következők:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-233">hello two main scenarios for using shared access policies tooaccess device functionality are:</span></span>

* <span data-ttu-id="a6dbc-234">[a felhő protokoll átjárók][lnk-endpoints],</span><span class="sxs-lookup"><span data-stu-id="a6dbc-234">[cloud protocol gateways][lnk-endpoints],</span></span>
* <span data-ttu-id="a6dbc-235">[a token szolgáltatások] [ lnk-custom-auth] tooimplement egyéni hitelesítési sémák használt.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-235">[token services][lnk-custom-auth] used tooimplement custom authentication schemes.</span></span>

<span data-ttu-id="a6dbc-236">Hello óta megosztott elérési házirend potenciálisan biztosíthat hozzáférést tooconnect bármely eszközre, mint az fontos toouse hello helyes erőforrás URI biztonsági jogkivonatokat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-236">Since hello shared access policy can potentially grant access tooconnect as any device, it is important toouse hello correct resource URI when creating security tokens.</span></span> <span data-ttu-id="a6dbc-237">Ez a beállítás különösen fontos a token szolgáltatásokat, amelyek tooscope hello token tooa adott eszköz használatával hello erőforrás URI azonosítója.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-237">This setting is especially important for token services, which have tooscope hello token tooa specific device using hello resource URI.</span></span> <span data-ttu-id="a6dbc-238">Ezt a pontot fontos kevesebb protokoll átjárók mivel azokat már van közvetítő forgalmat az összes eszköz.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-238">This point is less relevant for protocol gateways as they are already mediating traffic for all devices.</span></span>

<span data-ttu-id="a6dbc-239">Tegyük fel, előre létrehozott hello jogkivonat szolgáltatás megosztott hozzáférési házirend nevű **eszköz** jogkivonat hozna létre a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-239">As an example, a token service using hello pre-created shared access policy called **device** would create a token with hello following parameters:</span></span>

* <span data-ttu-id="a6dbc-240">erőforrás URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span><span class="sxs-lookup"><span data-stu-id="a6dbc-240">resource URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,</span></span>
* <span data-ttu-id="a6dbc-241">aláírási kulcs: hello hello kulcsokkal egyik `device` házirend,</span><span class="sxs-lookup"><span data-stu-id="a6dbc-241">signing key: one of hello keys of hello `device` policy,</span></span>
* <span data-ttu-id="a6dbc-242">házirend neve: `device`,</span><span class="sxs-lookup"><span data-stu-id="a6dbc-242">policy name: `device`,</span></span>
* <span data-ttu-id="a6dbc-243">a lejárati idő.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-243">any expiration time.</span></span>

<span data-ttu-id="a6dbc-244">Node.js-függvény megelőző hello segítségével például a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-244">An example using hello preceding Node.js function would be:</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

<span data-ttu-id="a6dbc-245">hello eredményt – amely device1 hozzáférés tooall funkciókat biztosít, a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-245">hello result, which grants access tooall functionality for device1, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

<span data-ttu-id="a6dbc-246">Protokoll-átjáró használhatja ugyanazt a token az összes eszköz egyszerűen hello erőforrás URI azonosítója túl hello`myhub.azure-devices.net/devices`.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-246">A protocol gateway could use hello same token for all devices simply setting hello resource URI too`myhub.azure-devices.net/devices`.</span></span>

### <a name="use-security-tokens-from-service-components"></a><span data-ttu-id="a6dbc-247">Használja a biztonsági jogkivonatokat a szolgáltatás-összetevők</span><span class="sxs-lookup"><span data-stu-id="a6dbc-247">Use security tokens from service components</span></span>

<span data-ttu-id="a6dbc-248">Szolgáltatás-összetevők csak hozhat létre a biztonsági jogkivonatokat hello megfelelő engedélyek megadását, amint azt korábban megosztott elérési házirendek segítségével.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-248">Service components can only generate security tokens using shared access policies granting hello appropriate permissions as explained previously.</span></span>

<span data-ttu-id="a6dbc-249">Íme hello végpontokon kitett hello funkciói:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-249">Here is hello service functions exposed on hello endpoints:</span></span>

| <span data-ttu-id="a6dbc-250">Végpont</span><span class="sxs-lookup"><span data-stu-id="a6dbc-250">Endpoint</span></span> | <span data-ttu-id="a6dbc-251">Funkció</span><span class="sxs-lookup"><span data-stu-id="a6dbc-251">Functionality</span></span> |
| --- | --- |
| `{iot hub host name}/devices` |<span data-ttu-id="a6dbc-252">Létrehozása, frissítése, lekérése és törlése eszköz identitások.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-252">Create, update, retrieve, and delete device identities.</span></span> |
| `{iot hub host name}/messages/events` |<span data-ttu-id="a6dbc-253">Eszköz-felhő üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-253">Receive device-to-cloud messages.</span></span> |
| `{iot hub host name}/servicebound/feedback` |<span data-ttu-id="a6dbc-254">A felhő-eszközre küldött üzenetek visszajelzést kap.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-254">Receive feedback for cloud-to-device messages.</span></span> |
| `{iot hub host name}/devicebound` |<span data-ttu-id="a6dbc-255">Felhő-eszközre küldött üzenetek küldése.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-255">Send cloud-to-device messages.</span></span> |

<span data-ttu-id="a6dbc-256">Például egy előre létrehozott hello segítségével generálása szolgáltatás megosztott hozzáférési házirend nevű **registryRead** jogkivonat hozna létre a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-256">As an example, a service generating using hello pre-created shared access policy called **registryRead** would create a token with hello following parameters:</span></span>

* <span data-ttu-id="a6dbc-257">erőforrás URI: `{IoT hub name}.azure-devices.net/devices`,</span><span class="sxs-lookup"><span data-stu-id="a6dbc-257">resource URI: `{IoT hub name}.azure-devices.net/devices`,</span></span>
* <span data-ttu-id="a6dbc-258">aláírási kulcs: hello hello kulcsokkal egyik `registryRead` házirend,</span><span class="sxs-lookup"><span data-stu-id="a6dbc-258">signing key: one of hello keys of hello `registryRead` policy,</span></span>
* <span data-ttu-id="a6dbc-259">házirend neve: `registryRead`,</span><span class="sxs-lookup"><span data-stu-id="a6dbc-259">policy name: `registryRead`,</span></span>
* <span data-ttu-id="a6dbc-260">a lejárati idő.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-260">any expiration time.</span></span>

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

<span data-ttu-id="a6dbc-261">hello eredményt – amely biztosítanak hozzáférést tooread minden eszköz identitások, a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-261">hello result, which would grant access tooread all device identities, would be:</span></span>

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a><span data-ttu-id="a6dbc-262">Támogatott X.509-tanúsítványokat</span><span class="sxs-lookup"><span data-stu-id="a6dbc-262">Supported X.509 certificates</span></span>

<span data-ttu-id="a6dbc-263">Az IoT-központ bármely X.509 tanúsítvány tooauthenticate eszköz használható.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-263">You can use any X.509 certificate tooauthenticate a device with IoT Hub.</span></span> <span data-ttu-id="a6dbc-264">A tanúsítványok tartalmaznak:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-264">Certificates include:</span></span>

* <span data-ttu-id="a6dbc-265">**Meglévő X.509 tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-265">**An existing X.509 certificate**.</span></span> <span data-ttu-id="a6dbc-266">Előfordulhat, hogy egy eszköz már társítva X.509 tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-266">A device may already have an X.509 certificate associated with it.</span></span> <span data-ttu-id="a6dbc-267">hello eszköz használatakor a tanúsítvány tooauthenticate IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-267">hello device can use this certificate tooauthenticate with IoT Hub.</span></span>
* <span data-ttu-id="a6dbc-268">**A önálló jön létre, és önaláírt tanúsítvány X-509**.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-268">**A self-generated and self-signed X-509 certificate**.</span></span> <span data-ttu-id="a6dbc-269">A gyártó vagy a belső fejlesztésű deployer ezeket a tanúsítványokat létrehozni és és tárolására is hello megfelelő titkos kulcsnak (tanúsítvány) hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-269">A device manufacturer or in-house deployer can generate these certificates and store hello corresponding private key (and certificate) on hello device.</span></span> <span data-ttu-id="a6dbc-270">Használhatja például a [OpenSSL] [ lnk-openssl] és [Windows SelfSignedCertificate] [ lnk-selfsigned] segédprogram erre a célra.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-270">You can use tools such as [OpenSSL][lnk-openssl] and [Windows SelfSignedCertificate][lnk-selfsigned] utility for this purpose.</span></span>
* <span data-ttu-id="a6dbc-271">**X.509 tanúsítvány hitelesítésszolgáltató által aláírt**.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-271">**CA-signed X.509 certificate**.</span></span> <span data-ttu-id="a6dbc-272">egy eszköz tooidentify és a hitelesítést az IoT-központ, egy X.509 tanúsítvány létrehozott és aláírt által hitelesítésszolgáltató (CA) is használható.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-272">tooidentify a device and authenticate it with IoT Hub, you can use an X.509 certificate generated and signed by a Certification Authority (CA).</span></span> <span data-ttu-id="a6dbc-273">Az IoT-központ csak ellenőrzi, hogy hello ujjlenyomat jelenik meg a konfigurált hello ujjlenyomatnak megfelelő.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-273">IoT Hub only verifies that hello thumbprint presented matches hello configured thumbprint.</span></span> <span data-ttu-id="a6dbc-274">IOT hubbal nem felel meg a tanúsítványlánc hello.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-274">IotHub does not validate hello certificate chain.</span></span>

<span data-ttu-id="a6dbc-275">Egy eszköz vagy használhat egy X.509 tanúsítvány vagy egy biztonsági jogkivonatot a hitelesítés, de soha sem egyszerre mindkettőre.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-275">A device may either use an X.509 certificate or a security token for authentication, but not both.</span></span>

### <a name="register-an-x509-certificate-for-a-device"></a><span data-ttu-id="a6dbc-276">Egy eszköz X.509 tanúsítvány regisztrálása</span><span class="sxs-lookup"><span data-stu-id="a6dbc-276">Register an X.509 certificate for a device</span></span>

<span data-ttu-id="a6dbc-277">Hello [Azure IoT szolgáltatás SDK for C#] [ lnk-service-sdk] (verzió 1.0.8+) támogatja az olyan eszköz, amely a hitelesítéshez használja egy X.509 tanúsítvány regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-277">hello [Azure IoT Service SDK for C#][lnk-service-sdk] (version 1.0.8+) supports registering a device that uses an X.509 certificate for authentication.</span></span> <span data-ttu-id="a6dbc-278">Például az importálási/exportálási az eszközök más API-k X.509-tanúsítványokat is támogatja.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-278">Other APIs such as import/export of devices also support X.509 certificates.</span></span>

### <a name="c-support"></a><span data-ttu-id="a6dbc-279">C\# támogatása</span><span class="sxs-lookup"><span data-stu-id="a6dbc-279">C\# Support</span></span>

<span data-ttu-id="a6dbc-280">Hello **RegistryManager** osztály biztosít egy programozott módon tooregister eszköz.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-280">hello **RegistryManager** class provides a programmatic way tooregister a device.</span></span> <span data-ttu-id="a6dbc-281">Különösen hello **AddDeviceAsync** és **UpdateDeviceAsync** módszerek lehetővé teszik a tooregister, és frissítse egy eszközt az IoT-központ identitásjegyzékhez hello.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-281">In particular, hello **AddDeviceAsync** and **UpdateDeviceAsync** methods enable you tooregister and update a device in hello IoT Hub identity registry.</span></span> <span data-ttu-id="a6dbc-282">Ez a két módszer igénybe vehet egy **eszköz** példány bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-282">These two methods take a **Device** instance as input.</span></span> <span data-ttu-id="a6dbc-283">Hello **eszköz** osztály tartalmaz egy **hitelesítési** , amely lehetővé teszi toospecify elsődleges és másodlagos X.509 tanúsítvány-ujjlenyomatok tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-283">hello **Device** class includes an **Authentication** property that allows you toospecify primary and secondary X.509 certificate thumbprints.</span></span> <span data-ttu-id="a6dbc-284">hello ujjlenyomat hello X.509 tanúsítvány (kódolással bináris DER tárolása) egy SHA-1 kivonatoló jelöli.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-284">hello thumbprint represents a SHA-1 hash of hello X.509 certificate (stored using binary DER encoding).</span></span> <span data-ttu-id="a6dbc-285">Lehetősége van hello egy elsődleges ujjlenyomatot vagy egy másodlagos ujjlenyomatot, vagy mindkettő megadására.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-285">You have hello option of specifying a primary thumbprint or a secondary thumbprint or both.</span></span> <span data-ttu-id="a6dbc-286">Elsődleges és másodlagos ujjlenyomatok támogatott toohandle tanúsítvány helyettesítő forgatókönyv áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-286">Primary and secondary thumbprints are supported toohandle certificate rollover scenarios.</span></span>

> [!NOTE]
> <span data-ttu-id="a6dbc-287">Az IoT-központ nem kér és hello teljes X.509 tanúsítvány, csak hello ujjlenyomat tárolja.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-287">IoT Hub does not require or store hello entire X.509 certificate, only hello thumbprint.</span></span>

<span data-ttu-id="a6dbc-288">Íme egy minta C\# code részlet tooregister egy eszközt egy X.509 tanúsítvány:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-288">Here is a sample C\# code snippet tooregister a device using an X.509 certificate:</span></span>

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

### <a name="use-an-x509-certificate-during-run-time-operations"></a><span data-ttu-id="a6dbc-289">Egy X.509 tanúsítvány közbeni futásidejű műveletek</span><span class="sxs-lookup"><span data-stu-id="a6dbc-289">Use an X.509 certificate during run-time operations</span></span>

<span data-ttu-id="a6dbc-290">Hello [Azure IoT-eszközök SDK for .NET] [ lnk-client-sdk] (verzió 1.0.11+) támogatja hello X.509-tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-290">hello [Azure IoT device SDK for .NET][lnk-client-sdk] (version 1.0.11+) supports hello use of X.509 certificates.</span></span>

### <a name="c-support"></a><span data-ttu-id="a6dbc-291">C\# támogatása</span><span class="sxs-lookup"><span data-stu-id="a6dbc-291">C\# Support</span></span>

<span data-ttu-id="a6dbc-292">osztály hello **DeviceAuthenticationWithX509Certificate** támogatja hello létrehozása **DeviceClient** példányok X.509 tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-292">hello class **DeviceAuthenticationWithX509Certificate** supports hello creation of **DeviceClient** instances using an X.509 certificate.</span></span> <span data-ttu-id="a6dbc-293">hello X.509 tanúsítvány, amely tartalmazza a titkos kulcs hello hello (más néven PKCS #12) PFX formátumban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-293">hello X.509 certificate must be in hello PFX (also called PKCS #12) format that includes hello private key.</span></span>

<span data-ttu-id="a6dbc-294">Íme egy minta kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-294">Here is a sample code snippet:</span></span>

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a><span data-ttu-id="a6dbc-295">Egyéni eszközhitelesítés</span><span class="sxs-lookup"><span data-stu-id="a6dbc-295">Custom device authentication</span></span>

<span data-ttu-id="a6dbc-296">Használhatja az IoT-központ hello [identitásjegyzékhez] [ lnk-identity-registry] tooconfigure eszközönkénti hitelesítő adatokat és a hozzáférés szabályozásához használatával [jogkivonatok] [ lnk-sas-tokens] .</span><span class="sxs-lookup"><span data-stu-id="a6dbc-296">You can use hello IoT Hub [identity registry][lnk-identity-registry] tooconfigure per-device security credentials and access control using [tokens][lnk-sas-tokens].</span></span> <span data-ttu-id="a6dbc-297">Ha egy IoT-megoldás már van egy egyéni identitás beállításjegyzék és/vagy hitelesítési séma, érdemes létrehozni egy *token service* toointegrate az infrastruktúra és az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-297">If an IoT solution already has a custom identity registry and/or authentication scheme, consider creating a *token service* toointegrate this infrastructure with IoT Hub.</span></span> <span data-ttu-id="a6dbc-298">Ily módon használhatja más IoT-szolgáltatásokat a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-298">In this way, you can use other IoT features in your solution.</span></span>

<span data-ttu-id="a6dbc-299">A jogkivonat-szolgáltatás egy olyan egyéni felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-299">A token service is a custom cloud service.</span></span> <span data-ttu-id="a6dbc-300">Az IoT-központ használ *megosztott hozzáférési házirend* rendelkező **DeviceConnect** engedélyek toocreate *eszköz hatókörű* jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-300">It uses an IoT Hub *shared access policy* with **DeviceConnect** permissions toocreate *device-scoped* tokens.</span></span> <span data-ttu-id="a6dbc-301">Ezeket a jogkivonatokat engedélyezése az eszköz tooconnect tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-301">These tokens enable a device tooconnect tooyour IoT hub.</span></span>

![Hello biztonságijogkivonat-szolgáltatás minta lépései][img-tokenservice]

<span data-ttu-id="a6dbc-303">Az alábbiakban hello fő lépésből hello biztonságijogkivonat-szolgáltatás minta:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-303">Here are hello main steps of hello token service pattern:</span></span>

1. <span data-ttu-id="a6dbc-304">Az IoT-központot, megosztott hozzáférési házirend létrehozása **DeviceConnect** az IoT hub engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-304">Create an IoT Hub shared access policy with **DeviceConnect** permissions for your IoT hub.</span></span> <span data-ttu-id="a6dbc-305">Ezt a házirendet hozhat létre a hello [Azure-portálon] [ lnk-management-portal] vagy programon keresztül.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-305">You can create this policy in hello [Azure portal][lnk-management-portal] or programmatically.</span></span> <span data-ttu-id="a6dbc-306">hello biztonságijogkivonat-szolgáltatás a házirend toosign hello jogkivonatokat hoz létre használ.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-306">hello token service uses this policy toosign hello tokens it creates.</span></span>
1. <span data-ttu-id="a6dbc-307">Ha egy eszköz tooaccess az IoT hub, aláírt jogkivonat kér a jogkivonat-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-307">When a device needs tooaccess your IoT hub, it requests a signed token from your token service.</span></span> <span data-ttu-id="a6dbc-308">hello eszközt, hogy a hello biztonságijogkivonat-szolgáltatás toocreate hello jogkivonatot használja az egyéni identitás rendszerleíró adatbázis-hitelesítési séma toodetermine hello eszköz identitással hitelesítheti.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-308">hello device can authenticate with your custom identity registry/authentication scheme toodetermine hello device identity that hello token service uses toocreate hello token.</span></span>
1. <span data-ttu-id="a6dbc-309">hello biztonságijogkivonat-szolgáltatás egy jogkivonatot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-309">hello token service returns a token.</span></span> <span data-ttu-id="a6dbc-310">hello jogkivonat segítségével hozhatók létre `/devices/{deviceId}` , `resourceURI`, a `deviceId` hitelesített hello eszközként.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-310">hello token is created by using `/devices/{deviceId}` as `resourceURI`, with `deviceId` as hello device being authenticated.</span></span> <span data-ttu-id="a6dbc-311">hello biztonságijogkivonat-szolgáltatás hello megosztott hozzáférési házirend tooconstruct hello jogkivonatot használja.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-311">hello token service uses hello shared access policy tooconstruct hello token.</span></span>
1. <span data-ttu-id="a6dbc-312">hello eszköz hello token közvetlenül az IoT-központ hello használja.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-312">hello device uses hello token directly with hello IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="a6dbc-313">Használhatja a .NET-osztályt hello [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] vagy Java-osztály hello [IotHubServiceSasToken] [ lnk-java-sas] toocreate egy tokent a token szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-313">You can use hello .NET class [SharedAccessSignatureBuilder][lnk-dotnet-sas] or hello Java class [IotHubServiceSasToken][lnk-java-sas] toocreate a token in your token service.</span></span>

<span data-ttu-id="a6dbc-314">hello biztonságijogkivonat-szolgáltatás hello jogkivonat lejáratáról állíthatja be, a kívánt módon működjenek.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-314">hello token service can set hello token expiration as desired.</span></span> <span data-ttu-id="a6dbc-315">Amikor hello-token érvényessége lejár, a hello IoT-központ kiszolgálója hello eszköz kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-315">When hello token expires, hello IoT hub severs hello device connection.</span></span> <span data-ttu-id="a6dbc-316">Ezt követően hello eszköz egy új jogkivonatot kell kérniük hello biztonságijogkivonat-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-316">Then, hello device must request a new token from hello token service.</span></span> <span data-ttu-id="a6dbc-317">Egy rövid lejárati időt hello eszköz és a hello biztonságijogkivonat-szolgáltatás hello terhelését növeli.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-317">A short expiry time increases hello load on both hello device and hello token service.</span></span>

<span data-ttu-id="a6dbc-318">Eszköz tooconnect tooyour hub, továbbra is hozzá kell azt az IoT-központ identitásjegyzékhez toohello – annak ellenére, hogy a jogkivonatot, és nem egy eszköz fő tooconnect hello eszköz használja.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-318">For a device tooconnect tooyour hub, you must still add it toohello IoT Hub identity registry — even though hello device is using a token and not a device key tooconnect.</span></span> <span data-ttu-id="a6dbc-319">Ezért, folytathatja a toouse eszköz hozzáférés-vezérlés engedélyezésével vagy letiltásával eszköz identitásokat a hello [identitásjegyzékhez][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="a6dbc-319">Therefore, you can continue toouse per-device access control by enabling or disabling device identities in hello [identity registry][lnk-identity-registry].</span></span> <span data-ttu-id="a6dbc-320">Ez a megközelítés azzal csökkenti az hello kockázatok a tokenek használatára hosszú lejárati idő feltüntetésével.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-320">This approach mitigates hello risks of using tokens with long expiry times.</span></span>

### <a name="comparison-with-a-custom-gateway"></a><span data-ttu-id="a6dbc-321">Egy egyéni átjáró összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="a6dbc-321">Comparison with a custom gateway</span></span>

<span data-ttu-id="a6dbc-322">hello biztonságijogkivonat-szolgáltatás a mintája, hello ajánlott módja tooimplement IoT-központ egyéni identitás rendszerleíró adatbázis-hitelesítési sémát.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-322">hello token service pattern is hello recommended way tooimplement a custom identity registry/authentication scheme with IoT Hub.</span></span> <span data-ttu-id="a6dbc-323">Ez a minta használata ajánlott, mivel az IoT-központ továbbra is toohandle legtöbb hello megoldás forgalom.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-323">This pattern is recommended because IoT Hub continues toohandle most of hello solution traffic.</span></span> <span data-ttu-id="a6dbc-324">Azonban ha hello egyéni hitelesítési séma így egymásba kapcsolódik, hello protokollal, szükség lehet egy *egyéni átjáró* tooprocess összes hello forgalmat.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-324">However, if hello custom authentication scheme is so intertwined with hello protocol, you may require a *custom gateway* tooprocess all hello traffic.</span></span> <span data-ttu-id="a6dbc-325">Ilyen eset például használ[Transport Layer Security (TLS) és előmegosztott kulccsal (PSKs)][lnk-tls-psk].</span><span class="sxs-lookup"><span data-stu-id="a6dbc-325">An example of such a scenario is using[Transport Layer Security (TLS) and pre-shared keys (PSKs)][lnk-tls-psk].</span></span> <span data-ttu-id="a6dbc-326">További információkért lásd: hello [protokoll-átjáró] [ lnk-protocols] témakör.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-326">For more information, see hello [protocol gateway][lnk-protocols] topic.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="a6dbc-327">Referencia-témaköreit:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-327">Reference topics:</span></span>

<span data-ttu-id="a6dbc-328">hello következő referencia-témakörökre biztosít ellenőrző hozzáférés tooyour IoT-központ további információt.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-328">hello following reference topics provide you with more information about controlling access tooyour IoT hub.</span></span>

## <a name="iot-hub-permissions"></a><span data-ttu-id="a6dbc-329">Az IoT-központ engedélyek</span><span class="sxs-lookup"><span data-stu-id="a6dbc-329">IoT Hub permissions</span></span>

<span data-ttu-id="a6dbc-330">hello következő táblázatban hello engedélyek toocontrol hozzáférés tooyour IoT-központ is használhatja.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-330">hello following table lists hello permissions you can use toocontrol access tooyour IoT hub.</span></span>

| <span data-ttu-id="a6dbc-331">Engedély</span><span class="sxs-lookup"><span data-stu-id="a6dbc-331">Permission</span></span> | <span data-ttu-id="a6dbc-332">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="a6dbc-332">Notes</span></span> |
| --- | --- |
| <span data-ttu-id="a6dbc-333">**RegistryRead**</span><span class="sxs-lookup"><span data-stu-id="a6dbc-333">**RegistryRead**</span></span> |<span data-ttu-id="a6dbc-334">Olvasási hozzáférés toohello identitásjegyzékhez biztosít.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-334">Grants read access toohello identity registry.</span></span> <span data-ttu-id="a6dbc-335">További információkért lásd: [identitásjegyzékhez][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="a6dbc-335">For more information, see [Identity registry][lnk-identity-registry].</span></span> <br/><span data-ttu-id="a6dbc-336">Háttér-felhőszolgáltatások használja ezt az engedélyt.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-336">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="a6dbc-337">**RegistryReadWrite**</span><span class="sxs-lookup"><span data-stu-id="a6dbc-337">**RegistryReadWrite**</span></span> |<span data-ttu-id="a6dbc-338">Biztosít olvasási és írási hozzáférés toohello identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-338">Grants read and write access toohello identity registry.</span></span> <span data-ttu-id="a6dbc-339">További információkért lásd: [identitásjegyzékhez][lnk-identity-registry].</span><span class="sxs-lookup"><span data-stu-id="a6dbc-339">For more information, see [Identity registry][lnk-identity-registry].</span></span> <br/><span data-ttu-id="a6dbc-340">Háttér-felhőszolgáltatások használja ezt az engedélyt.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-340">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="a6dbc-341">**ServiceConnect**</span><span class="sxs-lookup"><span data-stu-id="a6dbc-341">**ServiceConnect**</span></span> |<span data-ttu-id="a6dbc-342">A hozzáférési toocloud szolgáltatás irányuló kommunikációt és a végpontok ellenőrzésére.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-342">Grants access toocloud service-facing communication and monitoring endpoints.</span></span> <br/><span data-ttu-id="a6dbc-343">Felhő-eszközre küldött üzenetek és a megfelelő kézbesítési visszaigazolások lekérése hello biztosít engedély tooreceive eszközről a felhőbe üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="a6dbc-343">Grants permission tooreceive device-to-cloud messages, send cloud-to-device messages, and retrieve hello corresponding delivery acknowledgments.</span></span> <br/><span data-ttu-id="a6dbc-344">Biztosít engedély tooretrieve kézbesítési nyugtázás a fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-344">Grants permission tooretrieve delivery acknowledgements for file uploads.</span></span> <br/><span data-ttu-id="a6dbc-345">Biztosít engedély tooaccess eszköz twins tooupdate címkék kívánt tulajdonságokkal jelentett tulajdonságainak beolvasása és lekérdezéseket futtathat.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-345">Grants permission tooaccess device twins tooupdate tags and desired properties, retrieve reported properties, and run queries.</span></span> <br/><span data-ttu-id="a6dbc-346">Háttér-felhőszolgáltatások használja ezt az engedélyt.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-346">This permission is used by back-end cloud services.</span></span> |
| <span data-ttu-id="a6dbc-347">**DeviceConnect**</span><span class="sxs-lookup"><span data-stu-id="a6dbc-347">**DeviceConnect**</span></span> |<span data-ttu-id="a6dbc-348">A hozzáférési toodevice felé néző végpontok.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-348">Grants access toodevice-facing endpoints.</span></span> <br/><span data-ttu-id="a6dbc-349">Biztosít engedély toosend eszközről-a-felhőbe üzeneteket, és a felhő-eszközre küldött üzenetek fogadására.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-349">Grants permission toosend device-to-cloud messages and receive cloud-to-device messages.</span></span> <br/><span data-ttu-id="a6dbc-350">Biztosít engedély tooperform fájl feltöltése az eszközről.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-350">Grants permission tooperform file upload from a device.</span></span> <br/><span data-ttu-id="a6dbc-351">Biztosít engedély tooreceive eszköz szükséges iker tulajdonság értesítések és a frissítési eszköz iker jelentett tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-351">Grants permission tooreceive device twin desired property notifications and update device twin reported properties.</span></span> <br/><span data-ttu-id="a6dbc-352">Biztosít engedély tooperform fájlfeltöltések.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-352">Grants permission tooperform file uploads.</span></span> <br/><span data-ttu-id="a6dbc-353">Ez az engedély eszközöket használják.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-353">This permission is used by devices.</span></span> |

## <a name="additional-reference-material"></a><span data-ttu-id="a6dbc-354">További referenciaanyag</span><span class="sxs-lookup"><span data-stu-id="a6dbc-354">Additional reference material</span></span>

<span data-ttu-id="a6dbc-355">Más hello IoT Hub fejlesztői útmutató hivatkozási témaköröket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-355">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="a6dbc-356">[IoT-központok végpontjai] [ lnk-endpoints] ismerteti, hogy minden egyes IoT-központ elérhetővé teszi a futásidejű és felügyeleti műveletek különböző végpontok hello.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-356">[IoT Hub endpoints][lnk-endpoints] describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="a6dbc-357">[Sávszélesség-szabályozási és kvóták] [ lnk-quotas] hello kvóták ismerteti, és a szabályozás viselkedéseket, amelyek az IoT-központ szolgáltatás toohello alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-357">[Throttling and quotas][lnk-quotas] describes hello quotas and throttling behaviors that apply toohello IoT Hub service.</span></span>
* <span data-ttu-id="a6dbc-358">[Az Azure IoT eszköz és a szolgáltatás SDK-k] [ lnk-sdks] listák hello különböző nyelvi használhatja az eszköz és a szolgáltatás alkalmazások gondoskodnak az IoT hubbal fejlesztésekor SDK-k.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-358">[Azure IoT device and service SDKs][lnk-sdks] lists hello various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="a6dbc-359">[Az IoT-központ lekérdezési nyelv] [ lnk-query] hello lekérdezési nyelv használhatja tooretrieve IoT Hub-ből származó adataival a eszköz twins és a feladatokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-359">[IoT Hub query language][lnk-query] describes hello query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="a6dbc-360">[Az IoT Hub MQTT támogatási] [ lnk-devguide-mqtt] hello MQTT protokoll IoT-központ támogatásával kapcsolatos további információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="a6dbc-360">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a6dbc-361">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a6dbc-361">Next steps</span></span>

<span data-ttu-id="a6dbc-362">Most megtanulta, hogyan férnek hozzá a toocontrol az IoT-központot, esetleg érdekli hello IoT Hub fejlesztői útmutató témakörei a következő:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-362">Now you have learned how toocontrol access IoT Hub, you may be interested in hello following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="a6dbc-363">[Az eszköz twins toosynchronize állapotát és konfigurációkat használják][lnk-devguide-device-twins]</span><span class="sxs-lookup"><span data-stu-id="a6dbc-363">[Use device twins toosynchronize state and configurations][lnk-devguide-device-twins]</span></span>
* <span data-ttu-id="a6dbc-364">[Az eszközön közvetlen metódus][lnk-devguide-directmethods]</span><span class="sxs-lookup"><span data-stu-id="a6dbc-364">[Invoke a direct method on a device][lnk-devguide-directmethods]</span></span>
* <span data-ttu-id="a6dbc-365">[Több eszközön feladatok ütemezése][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="a6dbc-365">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="a6dbc-366">Ha szeretné tootry meg néhány ebben a cikkben ismertetett hello fogalmakat, esetleg az IoT-központ oktatóanyagok következő hello iránt érdeklődik:</span><span class="sxs-lookup"><span data-stu-id="a6dbc-366">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorials:</span></span>

* <span data-ttu-id="a6dbc-367">[Ismerkedés az Azure IoT Hub][lnk-getstarted-tutorial]</span><span class="sxs-lookup"><span data-stu-id="a6dbc-367">[Get started with Azure IoT Hub][lnk-getstarted-tutorial]</span></span>
* <span data-ttu-id="a6dbc-368">[Hogyan toosend felhőből eszközre küldött üzenetek IoT-központ][lnk-c2d-tutorial]</span><span class="sxs-lookup"><span data-stu-id="a6dbc-368">[How toosend cloud-to-device messages with IoT Hub][lnk-c2d-tutorial]</span></span>
* <span data-ttu-id="a6dbc-369">[Hogyan tooprocess IoT-központ eszközről a felhőbe üzenetek][lnk-d2c-tutorial]</span><span class="sxs-lookup"><span data-stu-id="a6dbc-369">[How tooprocess IoT Hub device-to-cloud messages][lnk-d2c-tutorial]</span></span>

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
