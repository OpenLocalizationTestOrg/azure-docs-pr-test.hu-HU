---
title: "Azure IoT Hub MQTT támogatási aaaUnderstand |} Microsoft Docs"
description: "Fejlesztői útmutató - tooan IoT Hub eszköz felé néző végpont használatával csatlakozó eszközök hello MQTT protokoll támogatása. Hello Azure IoT eszközoldali SDK-k beépített MQTT támogatásával kapcsolatos információkat tartalmazza."
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
ms.openlocfilehash: e461687963138987acdf1f4e0e34c453744ea191
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="communicate-with-your-iot-hub-using-hello-mqtt-protocol"></a><span data-ttu-id="ac110-104">Az IoT hub hello MQTT protokoll használatával folytatott kommunikációhoz</span><span class="sxs-lookup"><span data-stu-id="ac110-104">Communicate with your IoT hub using hello MQTT protocol</span></span>

<span data-ttu-id="ac110-105">Az IoT-központ lehetővé teszi, hogy a eszközök toocommunicate hello IoT Hub eszköz végpontokon hello segítségével [MQTT v3.1.1] [ lnk-mqtt-org] protokoll-as port 8883 vagy MQTT v3.1.1 WebSocket protokoll 443-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="ac110-105">IoT Hub enables devices toocommunicate with hello IoT Hub device endpoints using hello [MQTT v3.1.1][lnk-mqtt-org] protocol on port 8883 or MQTT v3.1.1 over WebSocket protocol on port 443.</span></span> <span data-ttu-id="ac110-106">Az IoT-központ szükséges összes eszköz kommunikációs toobe a TLS/SSL használatával biztonságossá (emiatt az IoT-központ nem támogatja a nem biztonságos kapcsolatok 1883 porton keresztül).</span><span class="sxs-lookup"><span data-stu-id="ac110-106">IoT Hub requires all device communication toobe secured using TLS/SSL (hence, IoT Hub doesn’t support non-secure connections over port 1883).</span></span>

## <a name="connecting-tooiot-hub"></a><span data-ttu-id="ac110-107">TooIoT központi csatlakozás</span><span class="sxs-lookup"><span data-stu-id="ac110-107">Connecting tooIoT Hub</span></span>

<span data-ttu-id="ac110-108">Eszközök bármelyikével hello MQTT protokoll tooconnect tooan IoT-központ hello hello tárak segítségével [Azure IoT SDK-k] [ lnk-device-sdks] vagy közvetlenül hello MQTT protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="ac110-108">A device can use hello MQTT protocol tooconnect tooan IoT hub either by using hello libraries in hello [Azure IoT SDKs][lnk-device-sdks] or by using hello MQTT protocol directly.</span></span>

## <a name="using-hello-device-sdks"></a><span data-ttu-id="ac110-109">Hello eszköz SDK-k használata</span><span class="sxs-lookup"><span data-stu-id="ac110-109">Using hello device SDKs</span></span>

<span data-ttu-id="ac110-110">[Eszközoldali SDK-k] [ lnk-device-sdks] érhetők el, hogy támogatási hello MQTT protokoll Java, Node.js, C, C# és Python.</span><span class="sxs-lookup"><span data-stu-id="ac110-110">[Device SDKs][lnk-device-sdks] that support hello MQTT protocol are available for Java, Node.js, C, C#, and Python.</span></span> <span data-ttu-id="ac110-111">hello eszköz SDK-k használata hello szabványos IoT-központ kapcsolati karakterlánc tooestablish kapcsolat tooan IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="ac110-111">hello device SDKs use hello standard IoT Hub connection string tooestablish a connection tooan IoT hub.</span></span> <span data-ttu-id="ac110-112">toouse hello MQTT protokoll hello ügyfél protokoll paramétert kell beállítani túl**MQTT**.</span><span class="sxs-lookup"><span data-stu-id="ac110-112">toouse hello MQTT protocol, hello client protocol parameter must be set too**MQTT**.</span></span> <span data-ttu-id="ac110-113">Alapértelmezés szerint hello eszköz SDK-k kapcsolódni az IoT-központ tooan hello **CleanSession** jelző beállítva túl**0** és **QoS 1** hello IoT hubbal üzenet exchange-hez.</span><span class="sxs-lookup"><span data-stu-id="ac110-113">By default, hello device SDKs connect tooan IoT Hub with hello **CleanSession** flag set too**0** and use **QoS 1** for message exchange with hello IoT hub.</span></span>

<span data-ttu-id="ac110-114">Ha egy eszköz csatlakoztatott tooan IoT-központot, hello eszköz SDK-k biztosítanak, amelyek lehetővé teszik az eszköz toosend köszönőüzenetei tooand az IoT-központ érkező üzenetek fogadására módszerek.</span><span class="sxs-lookup"><span data-stu-id="ac110-114">When a device is connected tooan IoT hub, hello device SDKs provide methods that enable hello device toosend messages tooand receive messages from an IoT hub.</span></span>

<span data-ttu-id="ac110-115">hello következő tábla tartalmaz hivatkozásokat toocode minták egyes nyelv támogatott, és adja meg a hello paraméter toouse tooestablish egy kapcsolat tooIoT Hub hello MQTT protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="ac110-115">hello following table contains links toocode samples for each supported language and specifies hello parameter toouse tooestablish a connection tooIoT Hub using hello MQTT protocol.</span></span>

| <span data-ttu-id="ac110-116">Nyelv</span><span class="sxs-lookup"><span data-stu-id="ac110-116">Language</span></span> | <span data-ttu-id="ac110-117">Protokoll paraméter</span><span class="sxs-lookup"><span data-stu-id="ac110-117">Protocol parameter</span></span> |
| --- | --- |
| <span data-ttu-id="ac110-118">[NODE.js][lnk-sample-node]</span><span class="sxs-lookup"><span data-stu-id="ac110-118">[Node.js][lnk-sample-node]</span></span> |<span data-ttu-id="ac110-119">Azure-iot-eszközök – mqtt</span><span class="sxs-lookup"><span data-stu-id="ac110-119">azure-iot-device-mqtt</span></span> |
| <span data-ttu-id="ac110-120">[Java][lnk-sample-java]</span><span class="sxs-lookup"><span data-stu-id="ac110-120">[Java][lnk-sample-java]</span></span> |<span data-ttu-id="ac110-121">IotHubClientProtocol.MQTT</span><span class="sxs-lookup"><span data-stu-id="ac110-121">IotHubClientProtocol.MQTT</span></span> |
| <span data-ttu-id="ac110-122">[C][lnk-sample-c]</span><span class="sxs-lookup"><span data-stu-id="ac110-122">[C][lnk-sample-c]</span></span> |<span data-ttu-id="ac110-123">MQTT_Protocol</span><span class="sxs-lookup"><span data-stu-id="ac110-123">MQTT_Protocol</span></span> |
| <span data-ttu-id="ac110-124">[C#][lnk-sample-csharp]</span><span class="sxs-lookup"><span data-stu-id="ac110-124">[C#][lnk-sample-csharp]</span></span> |<span data-ttu-id="ac110-125">TransportType.Mqtt</span><span class="sxs-lookup"><span data-stu-id="ac110-125">TransportType.Mqtt</span></span> |
| <span data-ttu-id="ac110-126">[Python][lnk-sample-python]</span><span class="sxs-lookup"><span data-stu-id="ac110-126">[Python][lnk-sample-python]</span></span> |<span data-ttu-id="ac110-127">IoTHubTransportProvider.MQTT</span><span class="sxs-lookup"><span data-stu-id="ac110-127">IoTHubTransportProvider.MQTT</span></span> |

### <a name="migrating-a-device-app-from-amqp-toomqtt"></a><span data-ttu-id="ac110-128">Egy eszköz alkalmazásának AMQP tooMQTT áttelepítése</span><span class="sxs-lookup"><span data-stu-id="ac110-128">Migrating a device app from AMQP tooMQTT</span></span>

<span data-ttu-id="ac110-129">Hello használata [eszköz SDK-k][lnk-device-sdks], AMQP használatával való váltás tooMQTT módosítani kell hello protokoll paraméter hello ügyfél inicializálása ahogy korábban is hangsúlyoztuk.</span><span class="sxs-lookup"><span data-stu-id="ac110-129">If you are using hello [device SDKs][lnk-device-sdks], switching from using AMQP tooMQTT requires changing hello protocol parameter in hello client initialization as stated previously.</span></span>

<span data-ttu-id="ac110-130">Annak során, győződjön meg arról, hogy toocheck hello a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="ac110-130">When doing so, make sure toocheck hello following items:</span></span>

* <span data-ttu-id="ac110-131">AMQP hibáit a sok feltételek MQTT hello kapcsolat leállítása közben.</span><span class="sxs-lookup"><span data-stu-id="ac110-131">AMQP returns errors for many conditions, while MQTT terminates hello connection.</span></span> <span data-ttu-id="ac110-132">A kivételkezelő logikát is eredményeképpen bizonyos változásokat igényelhet.</span><span class="sxs-lookup"><span data-stu-id="ac110-132">As a result your exception handling logic might require some changes.</span></span>
* <span data-ttu-id="ac110-133">MQTT nem támogatja a hello *elutasítása* műveletek fogadásakor [felhő-eszközre küldött üzenetek][lnk-messaging].</span><span class="sxs-lookup"><span data-stu-id="ac110-133">MQTT does not support hello *reject* operations when receiving [cloud-to-device messages][lnk-messaging].</span></span> <span data-ttu-id="ac110-134">Ha a háttér-tooreceive hello eszközalkalmazás válaszára kell, érdemes lehet [módszerek közvetlen][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="ac110-134">If your back-end app needs tooreceive a response from hello device app, consider using [direct methods][lnk-methods].</span></span>

## <a name="using-hello-mqtt-protocol-directly"></a><span data-ttu-id="ac110-135">Hello MQTT protokoll segítségével közvetlenül</span><span class="sxs-lookup"><span data-stu-id="ac110-135">Using hello MQTT protocol directly</span></span>
<span data-ttu-id="ac110-136">Ha egy eszköz nem használható hello eszköz SDK-k, továbbra is kapcsolódhatnak toohello nyilvános eszköz végpontok hello MQTT protokollal a port 8883.</span><span class="sxs-lookup"><span data-stu-id="ac110-136">If a device cannot use hello device SDKs, it can still connect toohello public device endpoints using hello MQTT protocol on port 8883.</span></span> <span data-ttu-id="ac110-137">A hello **CONNECT** csomag hello eszközt kell használnia a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="ac110-137">In hello **CONNECT** packet hello device should use hello following values:</span></span>

* <span data-ttu-id="ac110-138">A hello **ClientId** mezőben használja a hello **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="ac110-138">For hello **ClientId** field, use hello **deviceId**.</span></span>
* <span data-ttu-id="ac110-139">A hello **felhasználónév** mezőben `{iothubhostname}/{device_id}/api-version=2016-11-14`, ahol {iothubhostname} van hello hello IoT hub teljes CName.</span><span class="sxs-lookup"><span data-stu-id="ac110-139">For hello **Username** field, use `{iothubhostname}/{device_id}/api-version=2016-11-14`, where {iothubhostname} is hello full CName of hello IoT hub.</span></span>

    <span data-ttu-id="ac110-140">Például hello neve az IoT hub akkor **contoso.azure-devices.net** , és ha az eszköz nevére hello **MyDevice01**, teljes hello **felhasználónév** mezőt kell tartalmaznia. `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.</span><span class="sxs-lookup"><span data-stu-id="ac110-140">For example, if hello name of your IoT hub is **contoso.azure-devices.net** and if hello name of your device is **MyDevice01**, hello full **Username** field should contain `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.</span></span>
* <span data-ttu-id="ac110-141">A hello **jelszó** mezőben egy SAS-jogkivonatot használja.</span><span class="sxs-lookup"><span data-stu-id="ac110-141">For hello **Password** field, use a SAS token.</span></span> <span data-ttu-id="ac110-142">SAS-jogkivonat neve hello hello formátuma megegyezik hello hello HTTP és a AMQP protokollok esetében:</span><span class="sxs-lookup"><span data-stu-id="ac110-142">hello format of hello SAS token is hello same as for both hello HTTP and AMQP protocols:</span></span><br/><span data-ttu-id="ac110-143">`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.</span><span class="sxs-lookup"><span data-stu-id="ac110-143">`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.</span></span>

    <span data-ttu-id="ac110-144">További információ a SAS-tokenje toogenerate, a hello eszköz című szakaszában talál [IoT-központ használatával biztonsági jogkivonatokat][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="ac110-144">For more information about how toogenerate SAS tokens, see hello device section of [Using IoT Hub security tokens][lnk-sas-tokens].</span></span>

    <span data-ttu-id="ac110-145">Vizsgálatakor is használhatja hello [eszköz explorer] [ lnk-device-explorer] eszköz tooquickly a SAS-token másolja és illessze be a saját kód létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="ac110-145">When testing, you can also use hello [device explorer][lnk-device-explorer] tool tooquickly generate a SAS token that you can copy and paste into your own code:</span></span>

  1. <span data-ttu-id="ac110-146">Nyissa meg toohello **felügyeleti** lapján **eszköz Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ac110-146">Go toohello **Management** tab in **Device Explorer**.</span></span>
  2. <span data-ttu-id="ac110-147">Kattintson a **SAS-Token** (jobb felső).</span><span class="sxs-lookup"><span data-stu-id="ac110-147">Click **SAS Token** (top right).</span></span>
  3. <span data-ttu-id="ac110-148">A **SASTokenForm**, jelölje ki az eszköz hello **DeviceID** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="ac110-148">On **SASTokenForm**, select your device in hello **DeviceID** drop down.</span></span> <span data-ttu-id="ac110-149">Állítsa be a **TTL**.</span><span class="sxs-lookup"><span data-stu-id="ac110-149">Set your **TTL**.</span></span>
  4. <span data-ttu-id="ac110-150">Kattintson a **Generate** toocreate a jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="ac110-150">Click **Generate** toocreate your token.</span></span>

     <span data-ttu-id="ac110-151">hello generált SAS-jogkivonat van ez a struktúra: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span><span class="sxs-lookup"><span data-stu-id="ac110-151">hello SAS token that's generated has this structure: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span></span>

     <span data-ttu-id="ac110-152">a token toouse, hello részét hello **jelszó** mező tooconnect MQTT használatával van: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span><span class="sxs-lookup"><span data-stu-id="ac110-152">hello part of this token toouse as hello **Password** field tooconnect using MQTT is: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span></span>

<span data-ttu-id="ac110-153">MQTT csatlakozni, és válassza le a csomagok, IoT-központ kibocsát egy eseménnyel hello **Operations figyelés** csatorna további információkkal, melyek segíthetnek tootroubleshoot kapcsolódási problémák.</span><span class="sxs-lookup"><span data-stu-id="ac110-153">For MQTT connect and disconnect packets, IoT Hub issues an event on hello **Operations Monitoring** channel with additional information that can help you tootroubleshoot connectivity issues.</span></span>

### <a name="sending-device-to-cloud-messages"></a><span data-ttu-id="ac110-154">Eszköz-felhő üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="ac110-154">Sending device-to-cloud messages</span></span>

<span data-ttu-id="ac110-155">Sikeres csatlakozás után a egy eszköz küldhet üzeneteket tooIoT központ használatával `devices/{device_id}/messages/events/` vagy `devices/{device_id}/messages/events/{property_bag}` , egy **témakör**.</span><span class="sxs-lookup"><span data-stu-id="ac110-155">After making a successful connection, a device can send messages tooIoT Hub using `devices/{device_id}/messages/events/` or `devices/{device_id}/messages/events/{property_bag}` as a **Topic Name**.</span></span> <span data-ttu-id="ac110-156">Hello `{property_bag}` elem lehetővé teszi, hogy az eszköz toosend köszönőüzenetei további tulajdonságokkal url-kódolású formátumban.</span><span class="sxs-lookup"><span data-stu-id="ac110-156">hello `{property_bag}` element enables hello device toosend messages with additional properties in a url-encoded format.</span></span> <span data-ttu-id="ac110-157">Példa:</span><span class="sxs-lookup"><span data-stu-id="ac110-157">For example:</span></span>

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> <span data-ttu-id="ac110-158">Ez `{property_bag}` elem által használt hello ugyanazon kódolás hello HTTP protokoll a lekérdezési karakterláncok esetében.</span><span class="sxs-lookup"><span data-stu-id="ac110-158">This `{property_bag}` element uses hello same encoding as for query strings in hello HTTP protocol.</span></span>
>
>

<span data-ttu-id="ac110-159">hello eszköz alkalmazást is használhatja `devices/{device_id}/messages/events/{property_bag}` hello, **lesz a témakör neve** toodefine *üzenetek fog* toobe telemetriai üzenetként továbbítja.</span><span class="sxs-lookup"><span data-stu-id="ac110-159">hello device app can also use `devices/{device_id}/messages/events/{property_bag}` as hello **Will topic name** toodefine *Will messages* toobe forwarded as a telemetry message.</span></span>

- <span data-ttu-id="ac110-160">Az IoT-központ nem támogatja a QoS 2 üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="ac110-160">IoT Hub does not support QoS 2 messages.</span></span> <span data-ttu-id="ac110-161">Ha egy eszköz alkalmazás tesz közzé egy üzenetet, amelyben **QoS 2**, IoT-központ hello hálózati kapcsolat bezárása után.</span><span class="sxs-lookup"><span data-stu-id="ac110-161">If a device app publishes a message with **QoS 2**, IoT Hub closes hello network connection.</span></span>
- <span data-ttu-id="ac110-162">Az IoT-központ nem maradnak megőrzése üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="ac110-162">IoT Hub does not persist Retain messages.</span></span> <span data-ttu-id="ac110-163">Ha egy eszköz küldi hello üzenet **megőrzése** jelző too1 beállítva, az IoT-központ hozzáadja hello **x-opt-megőrzése** alkalmazás tulajdonság toohello üzenet.</span><span class="sxs-lookup"><span data-stu-id="ac110-163">If a device sends a message with hello **RETAIN** flag set too1, IoT Hub adds hello **x-opt-retain** application property toohello message.</span></span> <span data-ttu-id="ac110-164">Ebben az esetben helyett tárolásakor hello tartsa meg az üzenetet, IoT-központ toohello háttér app továbbítja azokat.</span><span class="sxs-lookup"><span data-stu-id="ac110-164">In this case, instead of persisting hello retain message, IoT Hub passes it toohello backend app.</span></span>
- <span data-ttu-id="ac110-165">Az IoT-központ csak eszközönként egy aktív MQTT kapcsolatot támogat.</span><span class="sxs-lookup"><span data-stu-id="ac110-165">IoT Hub only supports one active MQTT connection per device.</span></span> <span data-ttu-id="ac110-166">Egy új MQTT kapcsolathoz nevében hello ugyanazon Eszközazonosítót hatására az IoT-központ toodrop hello meglévő kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ac110-166">Any new MQTT connection on behalf of hello same device ID causes IoT Hub toodrop hello existing connection.</span></span>

<span data-ttu-id="ac110-167">További információkért lásd: [fejlesztői útmutató üzenetküldési][lnk-messaging].</span><span class="sxs-lookup"><span data-stu-id="ac110-167">For more information, see [Messaging developer's guide][lnk-messaging].</span></span>

### <a name="receiving-cloud-to-device-messages"></a><span data-ttu-id="ac110-168">Felhő-eszközre küldött üzenetek fogadása</span><span class="sxs-lookup"><span data-stu-id="ac110-168">Receiving cloud-to-device messages</span></span>

<span data-ttu-id="ac110-169">az IoT-központ tooreceive üzenetek, egy eszközt kell előfizetés `devices/{device_id}/messages/devicebound/#` , egy **témakör szűrő**.</span><span class="sxs-lookup"><span data-stu-id="ac110-169">tooreceive messages from IoT Hub, a device should subscribe using `devices/{device_id}/messages/devicebound/#` as a **Topic Filter**.</span></span> <span data-ttu-id="ac110-170">több szintű helyettesítő hello  **#**  hello a témakör szűrővel csak tooallow hello tooreceive további eszköztulajdonságok hello témakör neve.</span><span class="sxs-lookup"><span data-stu-id="ac110-170">hello multi-level wildcard **#** in hello Topic Filter is used only tooallow hello device tooreceive additional properties in hello topic name.</span></span> <span data-ttu-id="ac110-171">Az IoT-központ nem teszi lehetővé a hello hello használata  **#**  vagy **?**</span><span class="sxs-lookup"><span data-stu-id="ac110-171">IoT Hub does not allow hello usage of hello **#** or **?**</span></span> <span data-ttu-id="ac110-172">helyettesítő karakterek alárendelt témakörök szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="ac110-172">wildcards for filtering of sub-topics.</span></span> <span data-ttu-id="ac110-173">Mivel az IoT-központ nem egy általános célú pub-sub üzenetkezelési broker, csak a támogatott hello részletes ismertetését lásd a témakör nevét és a témakör szűrők.</span><span class="sxs-lookup"><span data-stu-id="ac110-173">Since IoT Hub is not a general purpose pub-sub messaging broker, it only supports hello documented topic names and topic filters.</span></span>

<span data-ttu-id="ac110-174">hello eszköz nem kap minden üzenet IoT-központot, amíg nem tooits eszközspecifikus végpont hello által képviselt sikeresen kér le `devices/{device_id}/messages/devicebound/#` témakör szűrő.</span><span class="sxs-lookup"><span data-stu-id="ac110-174">hello device does not receive any messages from IoT Hub, until it has successfully subscribed tooits device-specific endpoint, represented by hello `devices/{device_id}/messages/devicebound/#` topic filter.</span></span> <span data-ttu-id="ac110-175">Sikeres előfizetés létrejötte után hello eszköz elindítja a tooit elküldött hello előfizetés hello idő után csak felhőalapú eszközre üzenetek fogadására.</span><span class="sxs-lookup"><span data-stu-id="ac110-175">After successful subscription has been established, hello device starts receiving only cloud-to-device messages that have been sent tooit after hello time of hello subscription.</span></span> <span data-ttu-id="ac110-176">Ha hello eszköz csatlakozik az **CleanSession** jelző beállítva túl**0**, különböző munkamenetek között hello előfizetés megőrződjenek.</span><span class="sxs-lookup"><span data-stu-id="ac110-176">If hello device connects with **CleanSession** flag set too**0**, hello subscription is persisted across different sessions.</span></span> <span data-ttu-id="ac110-177">A következő csatlakozásakor ebben az esetben hello **CleanSession 0** hello eszköz kapja, függőben lévő üzenetek elküldött tooit közben le lett választva.</span><span class="sxs-lookup"><span data-stu-id="ac110-177">In this case, hello next time it connects with **CleanSession 0** hello device receives outstanding messages that have been sent tooit while it was disconnected.</span></span> <span data-ttu-id="ac110-178">Ha hello eszköz által használt **CleanSession** jelző beállítva túl**1** azonban azt nem bármely üzenetek fogadása az IoT-központ mindaddig, amíg az eszköz-végpont tooits feliratkozva.</span><span class="sxs-lookup"><span data-stu-id="ac110-178">If hello device uses **CleanSession** flag set too**1** though, it does not receive any messages from IoT Hub until it subscribes tooits device-endpoint.</span></span>

<span data-ttu-id="ac110-179">Az IoT-központ lekéri az üzeneteket a hello **témakör** `devices/{device_id}/messages/devicebound/`, vagy `devices/{device_id}/messages/devicebound/{property_bag}` bármely üzenettulajdonságok esetén.</span><span class="sxs-lookup"><span data-stu-id="ac110-179">IoT Hub delivers messages with hello **Topic Name** `devices/{device_id}/messages/devicebound/`, or `devices/{device_id}/messages/devicebound/{property_bag}` if there are any message properties.</span></span> <span data-ttu-id="ac110-180">`{property_bag}`url-kódolású kulcs/érték párok az üzenet tulajdonságait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ac110-180">`{property_bag}` contains url-encoded key/value pairs of message properties.</span></span> <span data-ttu-id="ac110-181">Csak alkalmazáshoz és a felhasználó állítható be rendszer tulajdonságai (például **messageId** vagy **correlationId**) hello tulajdonságcsomag szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="ac110-181">Only application properties and user-settable system properties (such as **messageId** or **correlationId**) are included in hello property bag.</span></span> <span data-ttu-id="ac110-182">Rendszer tulajdonságnevek rendelkezik hello előtag  **$** , alkalmazástulajdonságok hello eredeti tulajdonságnév használata nincs előtagja.</span><span class="sxs-lookup"><span data-stu-id="ac110-182">System property names have hello prefix **$**, application properties use hello original property name with no prefix.</span></span>

<span data-ttu-id="ac110-183">Ha egy eszköz alkalmazásának előfizet tooa témakör **QoS 2**, IoT-központ biztosít maximális QoS szintjének 1 hello **SUBACK** csomagot.</span><span class="sxs-lookup"><span data-stu-id="ac110-183">When a device app subscribes tooa topic with **QoS 2**, IoT Hub grants maximum QoS level 1 in hello **SUBACK** packet.</span></span> <span data-ttu-id="ac110-184">Ezt követően az IoT-központ üzenetek toohello eszközt QoS 1 nyújt.</span><span class="sxs-lookup"><span data-stu-id="ac110-184">After that, IoT Hub delivers messages toohello device using QoS 1.</span></span>

### <a name="retrieving-a-device-twins-properties"></a><span data-ttu-id="ac110-185">Egy eszköz iker tulajdonságainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="ac110-185">Retrieving a device twin's properties</span></span>

<span data-ttu-id="ac110-186">Először egy eszköz túl előfizet`$iothub/twin/res/#`, tooreceive hello művelet válaszokat.</span><span class="sxs-lookup"><span data-stu-id="ac110-186">First, a device subscribes too`$iothub/twin/res/#`, tooreceive hello operation's responses.</span></span> <span data-ttu-id="ac110-187">Ezután elküldi egy üres üzenet tootopic `$iothub/twin/GET/?$rid={request id}`, megadott értékkel **kérelemazonosító**. hello szolgáltatás majd egy iker eszközadatok hello témakör tartalmazó válaszüzenetet küld vissza `$iothub/twin/res/{status}/?$rid={request id}`, használatával hello azonos  **Kérelemazonosító** hello kérés.</span><span class="sxs-lookup"><span data-stu-id="ac110-187">Then, it sends an empty message tootopic `$iothub/twin/GET/?$rid={request id}`, with a populated value for **request id**. hello service then sends a response message containing hello device twin data on topic `$iothub/twin/res/{status}/?$rid={request id}`, using hello same **request id** as hello request.</span></span>

<span data-ttu-id="ac110-188">Kérelemazonosító tetszőleges érvényes érték, egy üzenet tulajdonságérték lehet megfelelően [IoT Hub fejlesztői útmutató üzenetküldési][lnk-messaging], és állapota egy egész számként van hitelesítve.</span><span class="sxs-lookup"><span data-stu-id="ac110-188">Request id can be any valid value for a message property value, as per [IoT Hub messaging developer's guide][lnk-messaging], and status is validated as an integer.</span></span>
<span data-ttu-id="ac110-189">hello adott válasz törzsének hello eszköz iker hello tulajdonságok szakasza tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="ac110-189">hello response body contains hello properties section of hello device twin:</span></span>

<span data-ttu-id="ac110-190">hello törzs hello identitás bejegyzés korlátozott toohello "Tulajdonságok" tag, például:</span><span class="sxs-lookup"><span data-stu-id="ac110-190">hello body of hello identity registry entry limited toohello “properties” member, for example:</span></span>

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

<span data-ttu-id="ac110-191">hello lehetséges állapota kódok a következők:</span><span class="sxs-lookup"><span data-stu-id="ac110-191">hello possible status codes are:</span></span>

|<span data-ttu-id="ac110-192">status</span><span class="sxs-lookup"><span data-stu-id="ac110-192">Status</span></span> | <span data-ttu-id="ac110-193">Leírás</span><span class="sxs-lookup"><span data-stu-id="ac110-193">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ac110-194">200</span><span class="sxs-lookup"><span data-stu-id="ac110-194">200</span></span> | <span data-ttu-id="ac110-195">Sikeres</span><span class="sxs-lookup"><span data-stu-id="ac110-195">Success</span></span> |
| <span data-ttu-id="ac110-196">429</span><span class="sxs-lookup"><span data-stu-id="ac110-196">429</span></span> | <span data-ttu-id="ac110-197">Túl sok kérelmek (halmozódni), mint / [IoT Hub-szabályozás][lnk-quotas]</span><span class="sxs-lookup"><span data-stu-id="ac110-197">Too many requests (throttled), as per [IoT Hub throttling][lnk-quotas]</span></span> |
| <span data-ttu-id="ac110-198">5**</span><span class="sxs-lookup"><span data-stu-id="ac110-198">5**</span></span> | <span data-ttu-id="ac110-199">Kiszolgáló hibák</span><span class="sxs-lookup"><span data-stu-id="ac110-199">Server errors</span></span> |

<span data-ttu-id="ac110-200">További információkért lásd: [eszköz twins fejlesztői útmutató][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="ac110-200">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="update-device-twins-reported-properties"></a><span data-ttu-id="ac110-201">Eszköz iker jelentett tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="ac110-201">Update device twin's reported properties</span></span>

<span data-ttu-id="ac110-202">hello következő feladatütemezési ismerteti, hogy egy eszköz frissíti hello jelentett hello eszköz iker az IoT Hub-tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="ac110-202">hello following sequence describes how a device updates hello reported properties in hello device twin in IoT Hub:</span></span>

1. <span data-ttu-id="ac110-203">Egy eszköz először elő kell fizetnie toohello `$iothub/twin/res/#` IoT-központ témakör tooreceive hello művelet válaszát.</span><span class="sxs-lookup"><span data-stu-id="ac110-203">A device must first subscribe toohello `$iothub/twin/res/#` topic tooreceive hello operation's responses from IoT Hub.</span></span>

1. <span data-ttu-id="ac110-204">Egy eszköz, amely tartalmazza a hello eszköz iker frissítés toohello üzenet küldése `$iothub/twin/PATCH/properties/reported/?$rid={request id}` témakör.</span><span class="sxs-lookup"><span data-stu-id="ac110-204">A device sends a message that contains hello device twin update toohello `$iothub/twin/PATCH/properties/reported/?$rid={request id}` topic.</span></span> <span data-ttu-id="ac110-205">Ez az üzenet tartalmaz egy **kérelemazonosító** érték.</span><span class="sxs-lookup"><span data-stu-id="ac110-205">This message includes a **request id** value.</span></span>

1. <span data-ttu-id="ac110-206">hello szolgáltatást, majd elküldi a válaszüzenetet, amely tartalmazza az új ETag érték hello hello jelentett tulajdonsággyűjteményében témakör `$iothub/twin/res/{status}/?$rid={request id}`.</span><span class="sxs-lookup"><span data-stu-id="ac110-206">hello service then sends a response message that contains hello new ETag value for hello reported properties collection on topic `$iothub/twin/res/{status}/?$rid={request id}`.</span></span> <span data-ttu-id="ac110-207">A válaszüzenet által használt azonos hello **kérelemazonosító** hello kérés.</span><span class="sxs-lookup"><span data-stu-id="ac110-207">This response message uses hello same **request id** as hello request.</span></span>

<span data-ttu-id="ac110-208">hello kérelem üzenettörzs egy JSON-dokumentumában, amely új értékek biztosít a jelentett tulajdonságokat (más tulajdonság vagy metaadatai nem módosíthatók) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ac110-208">hello request message body contains a JSON document, which provides new values for reported properties (no other property or metadata can be modified).</span></span>
<span data-ttu-id="ac110-209">Minden tagjának hello JSON-dokumentum frissíti, vagy adja hozzá a megfelelő tag hello hello eszköz iker dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="ac110-209">Each member in hello JSON document updates or add hello corresponding member in hello device twin’s document.</span></span> <span data-ttu-id="ac110-210">Egy tag beállítása túl`null`, törléseket hello objektumot tartalmazó hello tagja.</span><span class="sxs-lookup"><span data-stu-id="ac110-210">A member set too`null`, deletes hello member from hello containing object.</span></span> <span data-ttu-id="ac110-211">Példa:</span><span class="sxs-lookup"><span data-stu-id="ac110-211">For example:</span></span>

        {
            "telemetrySendFrequency": "35m",
            "batteryLevel": 60
        }

<span data-ttu-id="ac110-212">hello lehetséges állapota kódok a következők:</span><span class="sxs-lookup"><span data-stu-id="ac110-212">hello possible status codes are:</span></span>

|<span data-ttu-id="ac110-213">status</span><span class="sxs-lookup"><span data-stu-id="ac110-213">Status</span></span> | <span data-ttu-id="ac110-214">Leírás</span><span class="sxs-lookup"><span data-stu-id="ac110-214">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="ac110-215">200</span><span class="sxs-lookup"><span data-stu-id="ac110-215">200</span></span> | <span data-ttu-id="ac110-216">Sikeres</span><span class="sxs-lookup"><span data-stu-id="ac110-216">Success</span></span> |
| <span data-ttu-id="ac110-217">400</span><span class="sxs-lookup"><span data-stu-id="ac110-217">400</span></span> | <span data-ttu-id="ac110-218">Hibás kérés.</span><span class="sxs-lookup"><span data-stu-id="ac110-218">Bad Request.</span></span> <span data-ttu-id="ac110-219">Nem megfelelően formázott JSON-ban</span><span class="sxs-lookup"><span data-stu-id="ac110-219">Malformed JSON</span></span> |
| <span data-ttu-id="ac110-220">429</span><span class="sxs-lookup"><span data-stu-id="ac110-220">429</span></span> | <span data-ttu-id="ac110-221">Túl sok kérelmek (halmozódni), mint / [IoT Hub-szabályozás][lnk-quotas]</span><span class="sxs-lookup"><span data-stu-id="ac110-221">Too many requests (throttled), as per [IoT Hub throttling][lnk-quotas]</span></span> |
| <span data-ttu-id="ac110-222">5**</span><span class="sxs-lookup"><span data-stu-id="ac110-222">5**</span></span> | <span data-ttu-id="ac110-223">Kiszolgáló hibák</span><span class="sxs-lookup"><span data-stu-id="ac110-223">Server errors</span></span> |

<span data-ttu-id="ac110-224">További információkért lásd: [eszköz twins fejlesztői útmutató][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="ac110-224">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="receiving-desired-properties-update-notifications"></a><span data-ttu-id="ac110-225">A fogadó kívánt tulajdonságokkal vonatkozó frissítési értesítések</span><span class="sxs-lookup"><span data-stu-id="ac110-225">Receiving desired properties update notifications</span></span>

<span data-ttu-id="ac110-226">Ha egy eszköz csatlakozik, IoT-központ küld-e az értesítések toohello témakör `$iothub/twin/PATCH/properties/desired/?$version={new version}`, tartalmazó hello hello megoldás háttérrendszerének által végzett hello frissítés tartalmát.</span><span class="sxs-lookup"><span data-stu-id="ac110-226">When a device is connected, IoT Hub sends notifications toohello topic `$iothub/twin/PATCH/properties/desired/?$version={new version}`, which contain hello content of hello update performed by hello solution back end.</span></span> <span data-ttu-id="ac110-227">Példa:</span><span class="sxs-lookup"><span data-stu-id="ac110-227">For example:</span></span>

        {
            "telemetrySendFrequency": "5m",
            "route": null
        }

<span data-ttu-id="ac110-228">Tulajdonság frissítései, mint `null` érték azt jelenti, hogy a hello JSON objektum tag törlése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="ac110-228">As for property updates, `null` values means that hello JSON object member is being deleted.</span></span>


> [!IMPORTANT] 
> <span data-ttu-id="ac110-229">Az IoT-központ állít elő változási értesítéseket csak akkor, ha a csatlakoztatott eszközök is.</span><span class="sxs-lookup"><span data-stu-id="ac110-229">IoT Hub generates change notifications only when devices are connected.</span></span> <span data-ttu-id="ac110-230">Győződjön meg arról, hogy tooimplement hello [eszköz újracsatlakozás folyamat] [ lnk-devguide-twin-reconnection] tookeep hello szükséges tulajdonságok szinkronizálja az IoT Hub és hello eszköz alkalmazás között.</span><span class="sxs-lookup"><span data-stu-id="ac110-230">Make sure tooimplement hello [device reconnection flow][lnk-devguide-twin-reconnection] tookeep hello desired properties synchronized between IoT Hub and hello device app.</span></span>

<span data-ttu-id="ac110-231">További információkért lásd: [eszköz twins fejlesztői útmutató][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="ac110-231">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="respond-tooa-direct-method"></a><span data-ttu-id="ac110-232">Válaszoljon tooa közvetlen módszer</span><span class="sxs-lookup"><span data-stu-id="ac110-232">Respond tooa direct method</span></span>

<span data-ttu-id="ac110-233">Először egy eszköznek toosubscribe túl`$iothub/methods/POST/#`.</span><span class="sxs-lookup"><span data-stu-id="ac110-233">First, a device has toosubscribe too`$iothub/methods/POST/#`.</span></span> <span data-ttu-id="ac110-234">Az IoT-központ küld metódus kérelmek toohello témakör `$iothub/methods/POST/{method name}/?$rid={request id}`, vagy egy érvényes JSON-adatokat, vagy egy üres szövegtörzzsel.</span><span class="sxs-lookup"><span data-stu-id="ac110-234">IoT Hub sends method requests toohello topic `$iothub/methods/POST/{method name}/?$rid={request id}`, with either a valid JSON or an empty body.</span></span>

<span data-ttu-id="ac110-235">toorespond, hello eszköz üzenetet küld a egy érvényes JSON vagy üres szövegtörzzsel toohello témakör `$iothub/methods/res/{status}/?$rid={request id}`, ahol **kérelemazonosító** rendelkezik egy hello kérelemüzenetet, a hello toomatch és **állapot** toobe rendelkezik egy egész számot .</span><span class="sxs-lookup"><span data-stu-id="ac110-235">toorespond, hello device sends a message with a valid JSON or empty body toohello topic `$iothub/methods/res/{status}/?$rid={request id}`, where **request id** has toomatch hello one in hello request message, and **status** has toobe an integer.</span></span>

<span data-ttu-id="ac110-236">További információkért lásd: [közvetlen módszer fejlesztői útmutató][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="ac110-236">For more information, see [Direct method developer's guide][lnk-methods].</span></span>

### <a name="additional-considerations"></a><span data-ttu-id="ac110-237">Néhány fontos megjegyzés</span><span class="sxs-lookup"><span data-stu-id="ac110-237">Additional considerations</span></span>

<span data-ttu-id="ac110-238">Végső szempont, mint ha toocustomize hello MQTT protokoll viselkedését hello felhő oldalon tekintse át hello [Azure IoT protokoll-átjáró] [ lnk-azure-protocol-gateway] , amely lehetővé teszi egy nagy teljesítményű toodeploy egyéni protokoll-átjáró, amely közvetlenül az IoT-központ felületek.</span><span class="sxs-lookup"><span data-stu-id="ac110-238">As a final consideration, if you need toocustomize hello MQTT protocol behavior on hello cloud side, you should review hello [Azure IoT protocol gateway][lnk-azure-protocol-gateway] that enables you toodeploy a high-performance custom protocol gateway that interfaces directly with IoT Hub.</span></span> <span data-ttu-id="ac110-239">hello Azure IoT protokoll-átjáró lehetővé teszi, hogy toocustomize hello eszköz protokoll tooaccommodate brownfield MQTT rendszerbe állításához vagy egyéb egyéni protokollok.</span><span class="sxs-lookup"><span data-stu-id="ac110-239">hello Azure IoT protocol gateway enables you toocustomize hello device protocol tooaccommodate brownfield MQTT deployments or other custom protocols.</span></span> <span data-ttu-id="ac110-240">Ezt a módszert igényel, azonban, hogy futtatja, és egy egyéni protokoll-átjáró működik.</span><span class="sxs-lookup"><span data-stu-id="ac110-240">This approach does require, however, that you run and operate a custom protocol gateway.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac110-241">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac110-241">Next steps</span></span>

<span data-ttu-id="ac110-242">toolearn hello MQTT protokoll, bővebben lásd: hello [MQTT dokumentáció][lnk-mqtt-docs].</span><span class="sxs-lookup"><span data-stu-id="ac110-242">toolearn more about hello MQTT protocol, see hello [MQTT documentation][lnk-mqtt-docs].</span></span>

<span data-ttu-id="ac110-243">További információ az IoT-központ telepítésének tervezése toolearn lásd:</span><span class="sxs-lookup"><span data-stu-id="ac110-243">toolearn more about planning your IoT Hub deployment, see:</span></span>

* <span data-ttu-id="ac110-244">[Az IoT-eszközök katalógus Azure Certified][lnk-devices]</span><span class="sxs-lookup"><span data-stu-id="ac110-244">[Azure Certified for IoT device catalog][lnk-devices]</span></span>
* <span data-ttu-id="ac110-245">[Támogatja a további protokollok][lnk-protocols]</span><span class="sxs-lookup"><span data-stu-id="ac110-245">[Support additional protocols][lnk-protocols]</span></span>
* <span data-ttu-id="ac110-246">[Az Event Hubs összehasonlítása][lnk-compare]</span><span class="sxs-lookup"><span data-stu-id="ac110-246">[Compare with Event Hubs][lnk-compare]</span></span>
* <span data-ttu-id="ac110-247">[Méretezés, a magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási][lnk-scaling]</span><span class="sxs-lookup"><span data-stu-id="ac110-247">[Scaling, HA, and DR][lnk-scaling]</span></span>

<span data-ttu-id="ac110-248">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="ac110-248">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="ac110-249">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="ac110-249">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="ac110-250">[Egy eszköz szimulálva Azure IoT oldala][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="ac110-250">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

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
