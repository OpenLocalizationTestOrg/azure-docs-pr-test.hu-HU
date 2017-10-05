---
title: "Azure IoT Hub MQTT támogatás ismertetése |} Microsoft Docs"
description: "Fejlesztői útmutató - eszközök csatlakoztatása az IoT Hub eszköz felé néző végpont a MQTT protokollal támogatása. Az Azure IoT-eszközök SDK-k MQTT támogatja a beépített kapcsolatos adatokat tartalmaz."
services: iot-hub
documentationcenter: .net
author: kdotchkoff
manager: timlt
editor: 
ms.assetid: 1d71c27c-b466-4a40-b95b-d6550cf85144
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: kdotchko
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eab70c1aa9c49a137c2ac1012449d57915fb0d27
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="communicate-with-your-iot-hub-using-the-mqtt-protocol"></a><span data-ttu-id="3dbe4-104">Az IoT hub a MQTT protokoll segítségével kommunikálnak.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-104">Communicate with your IoT hub using the MQTT protocol</span></span>

<span data-ttu-id="3dbe4-105">Az IoT-központ lehetővé teszi, hogy az eszköz kommunikáljon az IoT Hub eszköz végpontjaitól az [MQTT v3.1.1] [ lnk-mqtt-org] protokoll-as port 8883 vagy MQTT v3.1.1 WebSocket protokoll 443-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-105">IoT Hub enables devices to communicate with the IoT Hub device endpoints using the [MQTT v3.1.1][lnk-mqtt-org] protocol on port 8883 or MQTT v3.1.1 over WebSocket protocol on port 443.</span></span> <span data-ttu-id="3dbe4-106">Az IoT-központ szükséges összes eszköz kommunikáció biztonságát, a TLS/SSL használatára (emiatt az IoT-központ nem támogatja a nem biztonságos kapcsolatok 1883 porton keresztül).</span><span class="sxs-lookup"><span data-stu-id="3dbe4-106">IoT Hub requires all device communication to be secured using TLS/SSL (hence, IoT Hub doesn’t support non-secure connections over port 1883).</span></span>

## <a name="connecting-to-iot-hub"></a><span data-ttu-id="3dbe4-107">Kapcsolódás az IoT-központ</span><span class="sxs-lookup"><span data-stu-id="3dbe4-107">Connecting to IoT Hub</span></span>

<span data-ttu-id="3dbe4-108">Egy eszköz a MQTT protokoll használható az IoT-központ keresztül a tárak csatlakozni a [Azure IoT SDK-k] [ lnk-device-sdks] vagy a MQTT használatával protokoll közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-108">A device can use the MQTT protocol to connect to an IoT hub either by using the libraries in the [Azure IoT SDKs][lnk-device-sdks] or by using the MQTT protocol directly.</span></span>

## <a name="using-the-device-sdks"></a><span data-ttu-id="3dbe4-109">Az SDK-k eszközt</span><span class="sxs-lookup"><span data-stu-id="3dbe4-109">Using the device SDKs</span></span>

<span data-ttu-id="3dbe4-110">[Eszközoldali SDK-k] [ lnk-device-sdks] támogató a MQTT protokoll érhetők el a Java, Node.js, C, C# és Python.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-110">[Device SDKs][lnk-device-sdks] that support the MQTT protocol are available for Java, Node.js, C, C#, and Python.</span></span> <span data-ttu-id="3dbe4-111">Az eszköz SDK-k a szabványos IoT-központ kapcsolati karakterlánc használatával kapcsolatot létrehozni az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-111">The device SDKs use the standard IoT Hub connection string to establish a connection to an IoT hub.</span></span> <span data-ttu-id="3dbe4-112">A MQTT protokoll használatához az ügyfél protokoll paraméter értékre kell állítani **MQTT**.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-112">To use the MQTT protocol, the client protocol parameter must be set to **MQTT**.</span></span> <span data-ttu-id="3dbe4-113">Alapértelmezés szerint az eszköz SDK-k csatlakozzon egy IoT hubot a **CleanSession** jelző beállítása **0** és **QoS 1** és az IoT hub üzenet exchange-hez.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-113">By default, the device SDKs connect to an IoT Hub with the **CleanSession** flag set to **0** and use **QoS 1** for message exchange with the IoT hub.</span></span>

<span data-ttu-id="3dbe4-114">Ha egy eszköz csatlakozik az IoT-központ, az eszköz SDK-k, amelyek lehetővé teszik az eszköz üzenetek küldését, és az IoT-központ érkező üzenetek fogadására módszerek adja meg.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-114">When a device is connected to an IoT hub, the device SDKs provide methods that enable the device to send messages to and receive messages from an IoT hub.</span></span>

<span data-ttu-id="3dbe4-115">A következő táblázat minden támogatott nyelven mintakódok mutató hivatkozásokat tartalmaz, és adja meg a paraméter a kapcsolatot a MQTT protokollal IoT-központ használatával.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-115">The following table contains links to code samples for each supported language and specifies the parameter to use to establish a connection to IoT Hub using the MQTT protocol.</span></span>

| <span data-ttu-id="3dbe4-116">Nyelv</span><span class="sxs-lookup"><span data-stu-id="3dbe4-116">Language</span></span> | <span data-ttu-id="3dbe4-117">Protokoll paraméter</span><span class="sxs-lookup"><span data-stu-id="3dbe4-117">Protocol parameter</span></span> |
| --- | --- |
| <span data-ttu-id="3dbe4-118">[NODE.js][lnk-sample-node]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-118">[Node.js][lnk-sample-node]</span></span> |<span data-ttu-id="3dbe4-119">Azure-iot-eszközök – mqtt</span><span class="sxs-lookup"><span data-stu-id="3dbe4-119">azure-iot-device-mqtt</span></span> |
| <span data-ttu-id="3dbe4-120">[Java][lnk-sample-java]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-120">[Java][lnk-sample-java]</span></span> |<span data-ttu-id="3dbe4-121">IotHubClientProtocol.MQTT</span><span class="sxs-lookup"><span data-stu-id="3dbe4-121">IotHubClientProtocol.MQTT</span></span> |
| <span data-ttu-id="3dbe4-122">[C][lnk-sample-c]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-122">[C][lnk-sample-c]</span></span> |<span data-ttu-id="3dbe4-123">MQTT_Protocol</span><span class="sxs-lookup"><span data-stu-id="3dbe4-123">MQTT_Protocol</span></span> |
| <span data-ttu-id="3dbe4-124">[C#][lnk-sample-csharp]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-124">[C#][lnk-sample-csharp]</span></span> |<span data-ttu-id="3dbe4-125">TransportType.Mqtt</span><span class="sxs-lookup"><span data-stu-id="3dbe4-125">TransportType.Mqtt</span></span> |
| <span data-ttu-id="3dbe4-126">[Python][lnk-sample-python]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-126">[Python][lnk-sample-python]</span></span> |<span data-ttu-id="3dbe4-127">IoTHubTransportProvider.MQTT</span><span class="sxs-lookup"><span data-stu-id="3dbe4-127">IoTHubTransportProvider.MQTT</span></span> |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a><span data-ttu-id="3dbe4-128">Egy eszköz alkalmazásának áttelepítése AMQP MQTT</span><span class="sxs-lookup"><span data-stu-id="3dbe4-128">Migrating a device app from AMQP to MQTT</span></span>

<span data-ttu-id="3dbe4-129">Ha használja a [eszköz SDK-k][lnk-device-sdks], az ügyfél inicializálása a protokoll paraméter módosítani, ahogy korábban is hangsúlyoztuk AMQP MQTT történő használatával való váltás kell.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-129">If you are using the [device SDKs][lnk-device-sdks], switching from using AMQP to MQTT requires changing the protocol parameter in the client initialization as stated previously.</span></span>

<span data-ttu-id="3dbe4-130">Amikor megtenné, ellenőrizze, hogy a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-130">When doing so, make sure to check the following items:</span></span>

* <span data-ttu-id="3dbe4-131">AMQP hibáit sok feltételeket, amíg a MQTT megszakítja a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-131">AMQP returns errors for many conditions, while MQTT terminates the connection.</span></span> <span data-ttu-id="3dbe4-132">A kivételkezelő logikát is eredményeképpen bizonyos változásokat igényelhet.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-132">As a result your exception handling logic might require some changes.</span></span>
* <span data-ttu-id="3dbe4-133">MQTT nem támogatja a *elutasítása* műveletek fogadásakor [felhő-eszközre küldött üzenetek][lnk-messaging].</span><span class="sxs-lookup"><span data-stu-id="3dbe4-133">MQTT does not support the *reject* operations when receiving [cloud-to-device messages][lnk-messaging].</span></span> <span data-ttu-id="3dbe4-134">Ha a háttér-kell az eszköz alkalmazás kapott választ, érdemes lehet [módszerek közvetlen][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="3dbe4-134">If your back-end app needs to receive a response from the device app, consider using [direct methods][lnk-methods].</span></span>

## <a name="using-the-mqtt-protocol-directly"></a><span data-ttu-id="3dbe4-135">Közvetlenül a MQTT protokollal</span><span class="sxs-lookup"><span data-stu-id="3dbe4-135">Using the MQTT protocol directly</span></span>
<span data-ttu-id="3dbe4-136">Ha egy eszköz nem tudja használni az eszköz SDK-k, hogy továbbra is kapcsolódni tud a nyilvános eszköz végpontok a MQTT protokollal 8883 porton.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-136">If a device cannot use the device SDKs, it can still connect to the public device endpoints using the MQTT protocol on port 8883.</span></span> <span data-ttu-id="3dbe4-137">Az a **CONNECT** csomag az eszközt használja a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-137">In the **CONNECT** packet the device should use the following values:</span></span>

* <span data-ttu-id="3dbe4-138">Az a **ClientId** mezőben használja a **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-138">For the **ClientId** field, use the **deviceId**.</span></span>
* <span data-ttu-id="3dbe4-139">Az a **felhasználónév** mezőben `{iothubhostname}/{device_id}/api-version=2016-11-14`, ahol a {iothubhostname} az IoT hub teljes CNAME-adatait.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-139">For the **Username** field, use `{iothubhostname}/{device_id}/api-version=2016-11-14`, where {iothubhostname} is the full CName of the IoT hub.</span></span>

    <span data-ttu-id="3dbe4-140">Például, ha az IoT hub neve **contoso.azure-devices.net** , és ha az eszköz nevére **MyDevice01**, a teljes **felhasználónév** mező kelltartalmaznia.`contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-140">For example, if the name of your IoT hub is **contoso.azure-devices.net** and if the name of your device is **MyDevice01**, the full **Username** field should contain `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.</span></span>
* <span data-ttu-id="3dbe4-141">Az a **jelszó** mezőben egy SAS-jogkivonatot használja.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-141">For the **Password** field, use a SAS token.</span></span> <span data-ttu-id="3dbe4-142">A SAS-jogkivonat formátuma ugyanaz, mint a HTTP és a AMQP protokoll:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-142">The format of the SAS token is the same as for both the HTTP and AMQP protocols:</span></span><br/><span data-ttu-id="3dbe4-143">`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-143">`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.</span></span>

    <span data-ttu-id="3dbe4-144">SAS-tokenje kapcsolatos további információkért tekintse meg az eszköz részében [IoT-központ használatával biztonsági jogkivonatokat][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="3dbe4-144">For more information about how to generate SAS tokens, see the device section of [Using IoT Hub security tokens][lnk-sas-tokens].</span></span>

    <span data-ttu-id="3dbe4-145">Ha tesztelni, használhatja a [eszköz explorer] [ lnk-device-explorer] eszköz segítségével gyorsan egy SAS-jogkivonatot másolja és illessze be a saját kódot:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-145">When testing, you can also use the [device explorer][lnk-device-explorer] tool to quickly generate a SAS token that you can copy and paste into your own code:</span></span>

  1. <span data-ttu-id="3dbe4-146">Lépjen a **felügyeleti** lapján **eszköz Explorer**.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-146">Go to the **Management** tab in **Device Explorer**.</span></span>
  2. <span data-ttu-id="3dbe4-147">Kattintson a **SAS-Token** (jobb felső).</span><span class="sxs-lookup"><span data-stu-id="3dbe4-147">Click **SAS Token** (top right).</span></span>
  3. <span data-ttu-id="3dbe4-148">A **SASTokenForm**, válassza ki az eszközt a **DeviceID** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-148">On **SASTokenForm**, select your device in the **DeviceID** drop down.</span></span> <span data-ttu-id="3dbe4-149">Állítsa be a **TTL**.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-149">Set your **TTL**.</span></span>
  4. <span data-ttu-id="3dbe4-150">Kattintson a **Generate** a jogkivonat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-150">Click **Generate** to create your token.</span></span>

     <span data-ttu-id="3dbe4-151">A SAS-jogkivonat, amely akkor jön létre, ez a struktúra rendelkezik: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-151">The SAS token that's generated has this structure: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span></span>

     <span data-ttu-id="3dbe4-152">Ez a token kívánja használni, mint a részét a **jelszó** MQTT segítségével szeretne csatlakozni a mező: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-152">The part of this token to use as the **Password** field to connect using MQTT is: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span></span>

<span data-ttu-id="3dbe4-153">MQTT csatlakozni, és válassza le a csomagok, IoT-központot egy eseményt állít ki a a **Operations figyelés** további információkkal, melyek segíthetnek csatlakozási problémák csatorna.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-153">For MQTT connect and disconnect packets, IoT Hub issues an event on the **Operations Monitoring** channel with additional information that can help you to troubleshoot connectivity issues.</span></span>

### <a name="sending-device-to-cloud-messages"></a><span data-ttu-id="3dbe4-154">Eszköz-felhő üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="3dbe4-154">Sending device-to-cloud messages</span></span>

<span data-ttu-id="3dbe4-155">Sikeres csatlakozás után eszköz üzeneteket küldhetnek az IoT-központ használatával `devices/{device_id}/messages/events/` vagy `devices/{device_id}/messages/events/{property_bag}` , egy **témakör**.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-155">After making a successful connection, a device can send messages to IoT Hub using `devices/{device_id}/messages/events/` or `devices/{device_id}/messages/events/{property_bag}` as a **Topic Name**.</span></span> <span data-ttu-id="3dbe4-156">A `{property_bag}` elem lehetővé teszi, hogy az eszköz további tulajdonságokkal rendelkező üzenetek küldése egy url-ként kódolt formában.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-156">The `{property_bag}` element enables the device to send messages with additional properties in a url-encoded format.</span></span> <span data-ttu-id="3dbe4-157">Példa:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-157">For example:</span></span>

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> <span data-ttu-id="3dbe4-158">Ez `{property_bag}` elem használja, mint a HTTP protokoll a lekérdezési karakterláncok ugyanazon kódolás.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-158">This `{property_bag}` element uses the same encoding as for query strings in the HTTP protocol.</span></span>
>
>

<span data-ttu-id="3dbe4-159">Az eszköz alkalmazást is használhatja `devices/{device_id}/messages/events/{property_bag}` , a **lesz a témakör neve** meghatározásához *üzenetek fog* telemetriai üzenetként továbbítását.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-159">The device app can also use `devices/{device_id}/messages/events/{property_bag}` as the **Will topic name** to define *Will messages* to be forwarded as a telemetry message.</span></span>

- <span data-ttu-id="3dbe4-160">Az IoT-központ nem támogatja a QoS 2 üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-160">IoT Hub does not support QoS 2 messages.</span></span> <span data-ttu-id="3dbe4-161">Ha egy eszköz alkalmazás tesz közzé egy üzenetet, amelyben **QoS 2**, IoT-központ bezárása után a hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-161">If a device app publishes a message with **QoS 2**, IoT Hub closes the network connection.</span></span>
- <span data-ttu-id="3dbe4-162">Az IoT-központ nem maradnak megőrzése üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-162">IoT Hub does not persist Retain messages.</span></span> <span data-ttu-id="3dbe4-163">Ha egy eszköz küld egy üzenetet, amelyben a **megőrzése** jelző értéke 1, az IoT-központ hozzáadja a **x-opt-megőrzése** az üzenetek alkalmazás tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-163">If a device sends a message with the **RETAIN** flag set to 1, IoT Hub adds the **x-opt-retain** application property to the message.</span></span> <span data-ttu-id="3dbe4-164">Ebben az esetben helyett a megőrzése üzenet megőrzése, IoT-központ számára továbbítja azokat a háttér-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-164">In this case, instead of persisting the retain message, IoT Hub passes it to the backend app.</span></span>
- <span data-ttu-id="3dbe4-165">Az IoT-központ csak eszközönként egy aktív MQTT kapcsolatot támogat.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-165">IoT Hub only supports one active MQTT connection per device.</span></span> <span data-ttu-id="3dbe4-166">Egy új MQTT kapcsolathoz nevében az ugyanazon Eszközazonosítót hatására az IoT-központ, a meglévő kapcsolat megszakad.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-166">Any new MQTT connection on behalf of the same device ID causes IoT Hub to drop the existing connection.</span></span>

<span data-ttu-id="3dbe4-167">További információkért lásd: [fejlesztői útmutató üzenetküldési][lnk-messaging].</span><span class="sxs-lookup"><span data-stu-id="3dbe4-167">For more information, see [Messaging developer's guide][lnk-messaging].</span></span>

### <a name="receiving-cloud-to-device-messages"></a><span data-ttu-id="3dbe4-168">Felhő-eszközre küldött üzenetek fogadása</span><span class="sxs-lookup"><span data-stu-id="3dbe4-168">Receiving cloud-to-device messages</span></span>

<span data-ttu-id="3dbe4-169">Üzenetek fogadása az IoT-központ, egy eszköz használatával ajánlatos `devices/{device_id}/messages/devicebound/#` , egy **témakör szűrő**.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-169">To receive messages from IoT Hub, a device should subscribe using `devices/{device_id}/messages/devicebound/#` as a **Topic Filter**.</span></span> <span data-ttu-id="3dbe4-170">A több szintű helyettesítő  **#**  a témakör szűrő használatával csak az eszköz további tulajdonságok kap, a témakör neve.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-170">The multi-level wildcard **#** in the Topic Filter is used only to allow the device to receive additional properties in the topic name.</span></span> <span data-ttu-id="3dbe4-171">Az IoT-központ nem teszi lehetővé az használatát, a  **#**  vagy **?**</span><span class="sxs-lookup"><span data-stu-id="3dbe4-171">IoT Hub does not allow the usage of the **#** or **?**</span></span> <span data-ttu-id="3dbe4-172">helyettesítő karakterek alárendelt témakörök szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-172">wildcards for filtering of sub-topics.</span></span> <span data-ttu-id="3dbe4-173">Mivel az IoT-központ nem egy általános célú pub-sub üzenetkezelési broker, csak a támogatott dokumentált témakör nevét és a témakör szűrők.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-173">Since IoT Hub is not a general purpose pub-sub messaging broker, it only supports the documented topic names and topic filters.</span></span>

<span data-ttu-id="3dbe4-174">Az eszköz nem fogadhat üzeneteket az IoT-központot, amíg sikeresen feliratkozott a eszközspecifikus végpontra, által képviselt a `devices/{device_id}/messages/devicebound/#` témakör szűrő.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-174">The device does not receive any messages from IoT Hub, until it has successfully subscribed to its device-specific endpoint, represented by the `devices/{device_id}/messages/devicebound/#` topic filter.</span></span> <span data-ttu-id="3dbe4-175">Sikeres előfizetés létrehozása után az eszköz elindítja a fogadásra csak felhőalapú eszközre elküldött hozzá az előfizetés az időpont után.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-175">After successful subscription has been established, the device starts receiving only cloud-to-device messages that have been sent to it after the time of the subscription.</span></span> <span data-ttu-id="3dbe4-176">Ha az eszköz csatlakozik az **CleanSession** jelző beállítása **0**, az előfizetés megőrződjenek különböző munkamenetek között.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-176">If the device connects with **CleanSession** flag set to **0**, the subscription is persisted across different sessions.</span></span> <span data-ttu-id="3dbe4-177">Ebben az esetben, amikor legközelebb kapcsolódik a **CleanSession 0** az eszköz megkaphatná a függőben lévő forráshoz lett rá elküldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-177">In this case, the next time it connects with **CleanSession 0** the device receives outstanding messages that have been sent to it while it was disconnected.</span></span> <span data-ttu-id="3dbe4-178">Ha az eszköz által használt **CleanSession** jelző beállítása **1** azonban azt nem bármely üzenetek fogadása az IoT-központ mindaddig, amíg az eszköz-végpont feliratkozva.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-178">If the device uses **CleanSession** flag set to **1** though, it does not receive any messages from IoT Hub until it subscribes to its device-endpoint.</span></span>

<span data-ttu-id="3dbe4-179">Az IoT-központ lekéri az üzeneteket a **témakör** `devices/{device_id}/messages/devicebound/`, vagy `devices/{device_id}/messages/devicebound/{property_bag}` bármely üzenettulajdonságok esetén.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-179">IoT Hub delivers messages with the **Topic Name** `devices/{device_id}/messages/devicebound/`, or `devices/{device_id}/messages/devicebound/{property_bag}` if there are any message properties.</span></span> <span data-ttu-id="3dbe4-180">`{property_bag}`url-kódolású kulcs/érték párok az üzenet tulajdonságait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-180">`{property_bag}` contains url-encoded key/value pairs of message properties.</span></span> <span data-ttu-id="3dbe4-181">Csak alkalmazáshoz és a felhasználó állítható be rendszer tulajdonságai (például **messageId** vagy **correlationId**) a tulajdonságcsomag szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-181">Only application properties and user-settable system properties (such as **messageId** or **correlationId**) are included in the property bag.</span></span> <span data-ttu-id="3dbe4-182">Rendszer tulajdonságnevek előtaggal van ellátva  **$** , alkalmazástulajdonságok használja az eredeti tulajdonság nevét nincs előtagja.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-182">System property names have the prefix **$**, application properties use the original property name with no prefix.</span></span>

<span data-ttu-id="3dbe4-183">Ha egy eszköz alkalmazás előfizet egy témakör **QoS 2**, IoT-központ maximális QoS szintjének 1 biztosít a **SUBACK** csomagot.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-183">When a device app subscribes to a topic with **QoS 2**, IoT Hub grants maximum QoS level 1 in the **SUBACK** packet.</span></span> <span data-ttu-id="3dbe4-184">Ezt követően az IoT-központ kézbesíti üzenetek a QoS-1-eszközt.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-184">After that, IoT Hub delivers messages to the device using QoS 1.</span></span>

### <a name="retrieving-a-device-twins-properties"></a><span data-ttu-id="3dbe4-185">Egy eszköz iker tulajdonságainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="3dbe4-185">Retrieving a device twin's properties</span></span>

<span data-ttu-id="3dbe4-186">Először egy eszköz előfizet `$iothub/twin/res/#`, a művelet válaszok fogadására.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-186">First, a device subscribes to `$iothub/twin/res/#`, to receive the operation's responses.</span></span> <span data-ttu-id="3dbe4-187">Ezt követően egy üres üzenetet küld a témakör `$iothub/twin/GET/?$rid={request id}`, ki van töltve értékkel **kérelemazonosító**.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-187">Then, it sends an empty message to topic `$iothub/twin/GET/?$rid={request id}`, with a populated value for **request id**.</span></span> <span data-ttu-id="3dbe4-188">A szolgáltatás majd a témakör az eszköz iker adatokat tartalmazó válaszüzenetet küld vissza `$iothub/twin/res/{status}/?$rid={request id}`, azonos **kérelemazonosító** kérés.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-188">The service then sends a response message containing the device twin data on topic `$iothub/twin/res/{status}/?$rid={request id}`, using the same **request id** as the request.</span></span>

<span data-ttu-id="3dbe4-189">Kérelemazonosító tetszőleges érvényes érték, egy üzenet tulajdonságérték lehet megfelelően [IoT Hub fejlesztői útmutató üzenetküldési][lnk-messaging], és állapota egy egész számként van hitelesítve.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-189">Request id can be any valid value for a message property value, as per [IoT Hub messaging developer's guide][lnk-messaging], and status is validated as an integer.</span></span>
<span data-ttu-id="3dbe4-190">Az adott válasz törzsének az eszköz iker tulajdonságok szakasza tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-190">The response body contains the properties section of the device twin:</span></span>

<span data-ttu-id="3dbe4-191">A szervezet a identitás bejegyzés például a "Tulajdonságok" tag korlátozódik:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-191">The body of the identity registry entry limited to the “properties” member, for example:</span></span>

        {
            "properties": {
                "desired": {
                    "telemetrySendFrequency": "5m",
                    "$version": 12
                },
                "reported": {
                    "telemetrySendFrequency": "5m",
                    "batteryLevel": 55,
                    "$version": 123
                }
            }
        }

<span data-ttu-id="3dbe4-192">A lehetséges állapota kódok a következők:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-192">The possible status codes are:</span></span>

|<span data-ttu-id="3dbe4-193">status</span><span class="sxs-lookup"><span data-stu-id="3dbe4-193">Status</span></span> | <span data-ttu-id="3dbe4-194">Leírás</span><span class="sxs-lookup"><span data-stu-id="3dbe4-194">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="3dbe4-195">200</span><span class="sxs-lookup"><span data-stu-id="3dbe4-195">200</span></span> | <span data-ttu-id="3dbe4-196">Sikeres</span><span class="sxs-lookup"><span data-stu-id="3dbe4-196">Success</span></span> |
| <span data-ttu-id="3dbe4-197">429</span><span class="sxs-lookup"><span data-stu-id="3dbe4-197">429</span></span> | <span data-ttu-id="3dbe4-198">Túl sok kérelmek (halmozódni), mint / [IoT Hub-szabályozás][lnk-quotas]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-198">Too many requests (throttled), as per [IoT Hub throttling][lnk-quotas]</span></span> |
| <span data-ttu-id="3dbe4-199">5**</span><span class="sxs-lookup"><span data-stu-id="3dbe4-199">5**</span></span> | <span data-ttu-id="3dbe4-200">Kiszolgáló hibák</span><span class="sxs-lookup"><span data-stu-id="3dbe4-200">Server errors</span></span> |

<span data-ttu-id="3dbe4-201">További információkért lásd: [eszköz twins fejlesztői útmutató][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="3dbe4-201">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="update-device-twins-reported-properties"></a><span data-ttu-id="3dbe4-202">Eszköz iker jelentett tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="3dbe4-202">Update device twin's reported properties</span></span>

<span data-ttu-id="3dbe4-203">Az alábbi sorrendben ismerteti, hogy egy eszköz frissíti az eszköz a két az IoT hubon jelentett tulajdonságait:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-203">The following sequence describes how a device updates the reported properties in the device twin in IoT Hub:</span></span>

1. <span data-ttu-id="3dbe4-204">Egy eszköz először elő kell a `$iothub/twin/res/#` úgy, hogy a művelet válaszok fogadjon IoT-központ témakör.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-204">A device must first subscribe to the `$iothub/twin/res/#` topic to receive the operation's responses from IoT Hub.</span></span>

1. <span data-ttu-id="3dbe4-205">Egy eszköz, amely tartalmazza az eszköz iker frissítés üzenetet küld a `$iothub/twin/PATCH/properties/reported/?$rid={request id}` témakör.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-205">A device sends a message that contains the device twin update to the `$iothub/twin/PATCH/properties/reported/?$rid={request id}` topic.</span></span> <span data-ttu-id="3dbe4-206">Ez az üzenet tartalmaz egy **kérelemazonosító** érték.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-206">This message includes a **request id** value.</span></span>

1. <span data-ttu-id="3dbe4-207">A szolgáltatás ezután elküldi a témakör a jelentett tulajdonsággyűjteményében új ETag értékét tartalmazó válaszüzenetet `$iothub/twin/res/{status}/?$rid={request id}`.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-207">The service then sends a response message that contains the new ETag value for the reported properties collection on topic `$iothub/twin/res/{status}/?$rid={request id}`.</span></span> <span data-ttu-id="3dbe4-208">A válaszüzenet használ azonos **kérelemazonosító** kérés.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-208">This response message uses the same **request id** as the request.</span></span>

<span data-ttu-id="3dbe4-209">A kérelem üzenettörzs egy JSON-dokumentumában, amely új értékek biztosít a jelentett tulajdonságokat (más tulajdonság vagy metaadatai nem módosíthatók) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-209">The request message body contains a JSON document, which provides new values for reported properties (no other property or metadata can be modified).</span></span>
<span data-ttu-id="3dbe4-210">A JSON-dokumentum minden tagjának frissíti, vagy adja hozzá a megfelelő tag a két eszköz dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-210">Each member in the JSON document updates or add the corresponding member in the device twin’s document.</span></span> <span data-ttu-id="3dbe4-211">Egy tag értéke `null`, törli a tagot a tartalmazó objektum.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-211">A member set to `null`, deletes the member from the containing object.</span></span> <span data-ttu-id="3dbe4-212">Példa:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-212">For example:</span></span>

        {
            "telemetrySendFrequency": "35m",
            "batteryLevel": 60
        }

<span data-ttu-id="3dbe4-213">A lehetséges állapota kódok a következők:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-213">The possible status codes are:</span></span>

|<span data-ttu-id="3dbe4-214">status</span><span class="sxs-lookup"><span data-stu-id="3dbe4-214">Status</span></span> | <span data-ttu-id="3dbe4-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="3dbe4-215">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="3dbe4-216">200</span><span class="sxs-lookup"><span data-stu-id="3dbe4-216">200</span></span> | <span data-ttu-id="3dbe4-217">Sikeres</span><span class="sxs-lookup"><span data-stu-id="3dbe4-217">Success</span></span> |
| <span data-ttu-id="3dbe4-218">400</span><span class="sxs-lookup"><span data-stu-id="3dbe4-218">400</span></span> | <span data-ttu-id="3dbe4-219">Hibás kérés.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-219">Bad Request.</span></span> <span data-ttu-id="3dbe4-220">Nem megfelelően formázott JSON-ban</span><span class="sxs-lookup"><span data-stu-id="3dbe4-220">Malformed JSON</span></span> |
| <span data-ttu-id="3dbe4-221">429</span><span class="sxs-lookup"><span data-stu-id="3dbe4-221">429</span></span> | <span data-ttu-id="3dbe4-222">Túl sok kérelmek (halmozódni), mint / [IoT Hub-szabályozás][lnk-quotas]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-222">Too many requests (throttled), as per [IoT Hub throttling][lnk-quotas]</span></span> |
| <span data-ttu-id="3dbe4-223">5**</span><span class="sxs-lookup"><span data-stu-id="3dbe4-223">5**</span></span> | <span data-ttu-id="3dbe4-224">Kiszolgáló hibák</span><span class="sxs-lookup"><span data-stu-id="3dbe4-224">Server errors</span></span> |

<span data-ttu-id="3dbe4-225">További információkért lásd: [eszköz twins fejlesztői útmutató][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="3dbe4-225">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="receiving-desired-properties-update-notifications"></a><span data-ttu-id="3dbe4-226">A fogadó kívánt tulajdonságokkal vonatkozó frissítési értesítések</span><span class="sxs-lookup"><span data-stu-id="3dbe4-226">Receiving desired properties update notifications</span></span>

<span data-ttu-id="3dbe4-227">Ha egy eszköz csatlakoztatása az IoT-központ értesítéseket küld a témakör `$iothub/twin/PATCH/properties/desired/?$version={new version}`, tartalmazó végzi el a megoldás háttérrendszeréhez frissítés tartalmát.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-227">When a device is connected, IoT Hub sends notifications to the topic `$iothub/twin/PATCH/properties/desired/?$version={new version}`, which contain the content of the update performed by the solution back end.</span></span> <span data-ttu-id="3dbe4-228">Példa:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-228">For example:</span></span>

        {
            "telemetrySendFrequency": "5m",
            "route": null
        }

<span data-ttu-id="3dbe4-229">Tulajdonság frissítései, mint `null` érték azt jelenti, hogy a JSON-objektum tag törlése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-229">As for property updates, `null` values means that the JSON object member is being deleted.</span></span>


> [!IMPORTANT] 
> <span data-ttu-id="3dbe4-230">Az IoT-központ állít elő változási értesítéseket csak akkor, ha a csatlakoztatott eszközök is.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-230">IoT Hub generates change notifications only when devices are connected.</span></span> <span data-ttu-id="3dbe4-231">Ügyeljen arra, hogy implementálja a [eszköz újracsatlakozás folyamat] [ lnk-devguide-twin-reconnection] tartani a kívánt tulajdonságokkal szinkronizálja az IoT-központ és az eszköz alkalmazás között.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-231">Make sure to implement the [device reconnection flow][lnk-devguide-twin-reconnection] to keep the desired properties synchronized between IoT Hub and the device app.</span></span>

<span data-ttu-id="3dbe4-232">További információkért lásd: [eszköz twins fejlesztői útmutató][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="3dbe4-232">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="respond-to-a-direct-method"></a><span data-ttu-id="3dbe4-233">A közvetlen módszer válaszolni</span><span class="sxs-lookup"><span data-stu-id="3dbe4-233">Respond to a direct method</span></span>

<span data-ttu-id="3dbe4-234">Először egy eszközhöz tartozik előfizetés `$iothub/methods/POST/#`.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-234">First, a device has to subscribe to `$iothub/methods/POST/#`.</span></span> <span data-ttu-id="3dbe4-235">Az IoT-központ metódus kérelmek küldése a következő témakörben `$iothub/methods/POST/{method name}/?$rid={request id}`, vagy egy érvényes JSON-adatokat, vagy egy üres szövegtörzzsel.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-235">IoT Hub sends method requests to the topic `$iothub/methods/POST/{method name}/?$rid={request id}`, with either a valid JSON or an empty body.</span></span>

<span data-ttu-id="3dbe4-236">Válaszol, az eszközre érvényes JSON vagy üres szövegtörzzsel üzenetet küld a témakör `$iothub/methods/res/{status}/?$rid={request id}`, ahol **kérelemazonosító** meg kell egyeznie a kérelemüzenetet, a és **állapot** egész számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-236">To respond, the device sends a message with a valid JSON or empty body to the topic `$iothub/methods/res/{status}/?$rid={request id}`, where **request id** has to match the one in the request message, and **status** has to be an integer.</span></span>

<span data-ttu-id="3dbe4-237">További információkért lásd: [közvetlen módszer fejlesztői útmutató][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="3dbe4-237">For more information, see [Direct method developer's guide][lnk-methods].</span></span>

### <a name="additional-considerations"></a><span data-ttu-id="3dbe4-238">Néhány fontos megjegyzés</span><span class="sxs-lookup"><span data-stu-id="3dbe4-238">Additional considerations</span></span>

<span data-ttu-id="3dbe4-239">Végső szempont, mint ha testre szeretné szabni a MQTT protokoll viselkedését a felhő oldalon tekintse át a [Azure IoT protokoll-átjáró] [ lnk-azure-protocol-gateway] , amely lehetővé teszi egy nagy teljesítményű egyéni telepítése felületek, közvetlenül az IoT Hub protokoll-átjáró.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-239">As a final consideration, if you need to customize the MQTT protocol behavior on the cloud side, you should review the [Azure IoT protocol gateway][lnk-azure-protocol-gateway] that enables you to deploy a high-performance custom protocol gateway that interfaces directly with IoT Hub.</span></span> <span data-ttu-id="3dbe4-240">Az Azure IoT protokoll átjáró lehetővé teszi az eszköz olyan brownfield MQTT központi telepítések protokoll(ok) más egyéni testreszabását.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-240">The Azure IoT protocol gateway enables you to customize the device protocol to accommodate brownfield MQTT deployments or other custom protocols.</span></span> <span data-ttu-id="3dbe4-241">Ezt a módszert igényel, azonban, hogy futtatja, és egy egyéni protokoll-átjáró működik.</span><span class="sxs-lookup"><span data-stu-id="3dbe4-241">This approach does require, however, that you run and operate a custom protocol gateway.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3dbe4-242">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3dbe4-242">Next steps</span></span>

<span data-ttu-id="3dbe4-243">A MQTT protokoll kapcsolatos további tudnivalókért tekintse meg a [MQTT dokumentáció][lnk-mqtt-docs].</span><span class="sxs-lookup"><span data-stu-id="3dbe4-243">To learn more about the MQTT protocol, see the [MQTT documentation][lnk-mqtt-docs].</span></span>

<span data-ttu-id="3dbe4-244">Az IoT-központ telepítésének tervezése kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-244">To learn more about planning your IoT Hub deployment, see:</span></span>

* <span data-ttu-id="3dbe4-245">[Az IoT-eszközök katalógus Azure Certified][lnk-devices]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-245">[Azure Certified for IoT device catalog][lnk-devices]</span></span>
* <span data-ttu-id="3dbe4-246">[Támogatja a további protokollok][lnk-protocols]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-246">[Support additional protocols][lnk-protocols]</span></span>
* <span data-ttu-id="3dbe4-247">[Az Event Hubs összehasonlítása][lnk-compare]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-247">[Compare with Event Hubs][lnk-compare]</span></span>
* <span data-ttu-id="3dbe4-248">[Méretezés, a magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási][lnk-scaling]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-248">[Scaling, HA, and DR][lnk-scaling]</span></span>

<span data-ttu-id="3dbe4-249">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="3dbe4-249">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="3dbe4-250">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-250">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="3dbe4-251">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="3dbe4-251">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-sas-tokens]: iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-twin-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-devguide-twin]: iot-hub-devguide-device-twins.md
