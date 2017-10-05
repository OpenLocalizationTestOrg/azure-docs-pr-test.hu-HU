---
title: "Media Services PlayReady licenc sablon – áttekintés"
description: "Ez a témakör áttekintést nyújt a PlayReady licenc sablonból, amely a PlayReady-licencek konfigurálásához használt."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: fddce5d0-1278-478f-ae05-9b985c748731
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: be19f616e36916655390cd05e738e93c08dcdf68
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="media-services-playready-license-template-overview"></a><span data-ttu-id="bdbb8-103">Media Services PlayReady licenc sablon – áttekintés</span><span class="sxs-lookup"><span data-stu-id="bdbb8-103">Media Services PlayReady license template overview</span></span>
<span data-ttu-id="bdbb8-104">Az Azure Media Services most már tartalmaz egy szolgáltatás, amelynek segítségével a Microsoft PlayReady-licencek.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-104">Azure Media Services now provides a service for delivering Microsoft PlayReady licenses.</span></span> <span data-ttu-id="bdbb8-105">Ha a végfelhasználó player (például Silverlight) próbál PlayReady védett tartalom lejátszása, a licenc beszerzésével kérelmet küldött a licenctovábbítási szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-105">When the end user player (for example, Silverlight) tries to play your PlayReady protected content, a request is sent to the license delivery service to obtain a license.</span></span> <span data-ttu-id="bdbb8-106">Ha a szolgáltatás a kérelem jóváhagyása, a licenc, amely az ügyfélnek küldött és fejti vissza és a megadott tartalom lejátszásához használható ad ki.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-106">If the license service approves the request, it issues the license which is sent to the client and can be used to decrypt and play the specified content.</span></span>

<span data-ttu-id="bdbb8-107">Media Services Ezenfelül API-k, amelyek lehetővé teszik a PlayReady-licencek konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-107">Media Services also provides APIs that let you configure your PlayReady licenses.</span></span> <span data-ttu-id="bdbb8-108">Licencek tartalmazzák a jogokat és korlátozásokat, amelyeket szeretne a PlayReady DRM futási időben t a lejátszás védett tartalmat próbál egy felhasználó.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-108">Licenses contain the rights and restrictions that you want for the PlayReady DRM runtime to enforce when a user is trying to playback protected content.</span></span>
<span data-ttu-id="bdbb8-109">Az alábbiakban néhány példa a PlayReady licenckorlátozások megadása:</span><span class="sxs-lookup"><span data-stu-id="bdbb8-109">Below are some examples of PlayReady license restrictions that you can specify:</span></span>

* <span data-ttu-id="bdbb8-110">A dátumot/időt, amelyből a licenc érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-110">The DateTime from which the license is valid.</span></span>
* <span data-ttu-id="bdbb8-111">A DateTime típusú érték, ha a licenc lejárati.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-111">The DateTime value when the license expires.</span></span> 
* <span data-ttu-id="bdbb8-112">Az állandó tároló menteni az ügyfélen a licenc.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-112">For the license to be saved in persistent storage on the client.</span></span> <span data-ttu-id="bdbb8-113">Állandó licencek engedélyezése a tartalom lejátszását offline általában használhatók.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-113">Persistent licenses are typically used to allow offline playback of the content.</span></span>
* <span data-ttu-id="bdbb8-114">A minimális biztonsági szintet, hogy az egy player számára, hogy a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-114">The minimum security level that a player must have to play your content.</span></span> 
* <span data-ttu-id="bdbb8-115">A kimeneti védelmi szint a kimeneti vezérlők audio\video tartalom.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-115">The output protection level for the output controls for audio\video content.</span></span> 
* <span data-ttu-id="bdbb8-116">További információk: a kimeneti vezérlők szakasz (3.5) az a [PlayReady megfelelőségi szabályok](https://www.microsoft.com/playready/licensing/compliance/) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-116">For more information, see the Output Controls section (3.5) in the [PlayReady Compliance Rules](https://www.microsoft.com/playready/licensing/compliance/) document.</span></span>

> [!NOTE]
> <span data-ttu-id="bdbb8-117">Jelenleg csak is beállíthatja a PlayRight a PlayReady-licenc (Ez a jogosultság szükség).</span><span class="sxs-lookup"><span data-stu-id="bdbb8-117">Currently, you can only configure the PlayRight of the PlayReady license (this right is required).</span></span> <span data-ttu-id="bdbb8-118">A PlayRight lehetővé teszi az ügyfél a tartalmak lejátszásához.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-118">The PlayRight gives the client the ability to playback the content.</span></span> <span data-ttu-id="bdbb8-119">A PlayRight is lehetővé teszi a lejátszás vonatkozó korlátozások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-119">The PlayRight also allows configuring restrictions specific to playback.</span></span> <span data-ttu-id="bdbb8-120">További információkért lásd: [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).</span><span class="sxs-lookup"><span data-stu-id="bdbb8-120">For more information, see [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).</span></span>
> 
> 

<span data-ttu-id="bdbb8-121">PlayReady-licencek Media Services használatával konfigurálja, konfigurálnia kell a Media Services PlayReady licenc sablonja.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-121">To configure PlayReady licenses using Media Services, you must configure the Media Services PlayReady license template.</span></span> <span data-ttu-id="bdbb8-122">A sablon az XML-Fájlban van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-122">The template is defined in XML.</span></span>

<span data-ttu-id="bdbb8-123">A következő példa bemutatja a legegyszerűbb (és általános) sablont, amely egy adatfolyam-továbbítási alaplicenc konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-123">The following example shows the simplest (and most common) template that configures a basic streaming license.</span></span> <span data-ttu-id="bdbb8-124">A licenccel rendelkező az ügyfelek számára a lejátszás képes lenne a PlayReady védett tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-124">With this license, your clients would be able to playback your PlayReady protected content.</span></span>

    <?xml version="1.0" encoding="utf-8"?>
    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" 
                                      xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <PlayRight />
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

<span data-ttu-id="bdbb8-125">Az XML-kód megfelel-e a PlayReady licenc sablon XML-séma a PlayReady-licencsablon XML-séma szakaszban meghatározott.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-125">The XML conforms to the PlayReady license template XML schema defined in the PlayReady license template XML schema section.</span></span>

<span data-ttu-id="bdbb8-126">A Media Services is meghatároz egy .NET-osztályok használható szerializált és irányuló és onnan az XML deszerializálása.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-126">Media Services also defines a set of .NET classes that could be used to serialized and deserialized to and from the XML.</span></span> <span data-ttu-id="bdbb8-127">A fő osztályok ismertetését lásd: [Media Services .NET-osztályok](media-services-playready-license-template-overview.md#classes).</span><span class="sxs-lookup"><span data-stu-id="bdbb8-127">For description of main classes, see [Media Services .NET classes](media-services-playready-license-template-overview.md#classes).</span></span> <span data-ttu-id="bdbb8-128">licenc sablonok konfigurálásához használt.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-128">that are used to configure license templates.</span></span>

<span data-ttu-id="bdbb8-129">Végpontok közötti például, hogy a .NET-osztályokat használja a PlayReady licencsablon konfigurálása, [használatával dinamikus PlayReady-titkosítás és Licenctovábbítási szolgáltatása](media-services-protect-with-drm.md).</span><span class="sxs-lookup"><span data-stu-id="bdbb8-129">For an end-to-end example that uses .NET classes to configure the PlayReady license template, see [Using PlayReady Dynamic Encryption and License Delivery Service](media-services-protect-with-drm.md).</span></span>

## <span data-ttu-id="bdbb8-130"><a id="classes"></a>Media Services .NET-osztályok használandó licenc sablonok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bdbb8-130"><a id="classes"></a>Media Services .NET classes that are used to configure license templates</span></span>
<span data-ttu-id="bdbb8-131">Az alábbi táblázat a fő .NET-osztályok Media Services PlayReady licenc sablonok konfigurálására szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-131">The following are the main .NET classes are used to configure Media Services PlayReady license templates.</span></span> <span data-ttu-id="bdbb8-132">Ezeket az osztályokat a megadott típus beolvasása leképezése [PlayReady licenc sablon XML-séma](media-services-playready-license-template-overview.md#schema).</span><span class="sxs-lookup"><span data-stu-id="bdbb8-132">These classes map to the types defined in [PlayReady license template XML schema](media-services-playready-license-template-overview.md#schema).</span></span>

<span data-ttu-id="bdbb8-133">A [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) osztály szerializálása és deszerializálása irányuló és onnan a Media Services licencsablon XML szolgál.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-133">The [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) class is used to serialize and deserialize to and from the Media Services license template XML.</span></span>

### <a name="playreadylicenseresponsetemplate"></a><span data-ttu-id="bdbb8-134">PlayReadyLicenseResponseTemplate</span><span class="sxs-lookup"><span data-stu-id="bdbb8-134">PlayReadyLicenseResponseTemplate</span></span>
<span data-ttu-id="bdbb8-135">[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) -Ez az osztály a sablon a választ küld vissza a végfelhasználó jelenti.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-135">[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) - This class represents the template for the response sent back to the end user.</span></span> <span data-ttu-id="bdbb8-136">Egy egyéni karakterláncot kell megadnia a licenckiszolgáló és az alkalmazás (hasznos lehet az egyéni alkalmazás logika), valamint egy vagy több licenc sablonok listájának közötti egy mezőt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-136">It contains a field for a custom data string between the license server and the application (may be useful for custom app logic) as well as a list of one or more license templates.</span></span>

<span data-ttu-id="bdbb8-137">Ez az "legfelső szintű" a sablon hierarchiában.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-137">This is the “top level” class in the template hierarchy.</span></span> <span data-ttu-id="bdbb8-138">Ami azt jelenti, hogy a válasz sablon licenc sablonok listáját tartalmazza, és a licenc-sablonjai tartalmazzák a (közvetlenül vagy közvetetten) a sablon adatokat alkotó szerializálandó más osztályok.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-138">Meaning that the response template includes a list of license templates and the license templates include (directly or indirectly) all of the other classes that make up the template data to be serialized.</span></span>

### <a name="playreadylicensetemplate"></a><span data-ttu-id="bdbb8-139">PlayReadyLicenseTemplate</span><span class="sxs-lookup"><span data-stu-id="bdbb8-139">PlayReadyLicenseTemplate</span></span>
<span data-ttu-id="bdbb8-140">[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) -az osztály a végfelhasználók számára a visszaadandó PlayReady-licencek létrehozására szolgáló licenc sablont jelöli.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-140">[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) - The class represents a license template for creating PlayReady licenses to be returned to the end users.</span></span> <span data-ttu-id="bdbb8-141">A tartalomkulcsot a licenc és bármely jogokat és korlátozásokat a tartalomkulcs használatakor a PlayReady DRM-futtatókörnyezet kikényszerítendő az adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-141">It contains the data on the content key in the license and any rights or restrictions to be enforced by the PlayReady DRM runtime when using the content key.</span></span>

### <span data-ttu-id="bdbb8-142"><a id="PlayReadyPlayRight"></a>PlayReadyPlayRight</span><span class="sxs-lookup"><span data-stu-id="bdbb8-142"><a id="PlayReadyPlayRight"></a>PlayReadyPlayRight</span></span>
<span data-ttu-id="bdbb8-143">[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) -Ez az osztály a PlayReady-licenc PlayRight jelenti.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-143">[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) - This class represents the PlayRight of a PlayReady license.</span></span> <span data-ttu-id="bdbb8-144">Ez a felhasználó számára lehetőséget ad a lejátszás a tartalmat a konfigurált, a licenc, valamint magának a PlayRight (a lejátszás adott házirend) nulla vagy több korlátozások érvényesek.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-144">It grants the user the ability to playback the content subject to the zero or more restrictions configured in the license and on the PlayRight itself (for playback specific policy).</span></span> <span data-ttu-id="bdbb8-145">Nagy részét a házirendet a PlayRight a van elvégezni a segítségével vezérelheti, amelyet a tartalom még keresztül kimeneti korlátozások és korlátozások kell kell bevezetni a megadott kimeneti használatakor.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-145">Much of the policy on the PlayRight has to do with output restrictions which control the types of outputs that the content can be played over and any restrictions that must be put in place when using a given output.</span></span> <span data-ttu-id="bdbb8-146">Például ha a DigitalVideoOnlyContentRestriction engedélyezve van, majd a DRM-futtatókörnyezet csak lehetővé teszi (analóg videó kimenetek való kapcsolódásuk nem engedélyezett a tartalmat továbbítani) digitális kimenetek felett megjelenő videó.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-146">For example, if the DigitalVideoOnlyContentRestriction is enabled, then the DRM runtime will only allow the video to be displayed over digital outputs (analog video outputs won’t be allowed to pass the content).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bdbb8-147">Ezek a típusok korlátozások nagyon hatékonyak lehetnek, de is hatással lehet a felhasználói élmény.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-147">These types of restrictions can be very powerful but can also affect the consumer experience.</span></span> <span data-ttu-id="bdbb8-148">Ha a kimeneti védelmet túl szigorú vannak konfigurálva, előfordulhat, hogy a tartalmat az egyes ügyfelek játsszák le.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-148">If the output protections are configured too restrictive, the content might be unplayable on some clients.</span></span> <span data-ttu-id="bdbb8-149">További információkért lásd: a [PlayReady megfelelőségi szabályok](https://www.microsoft.com/playready/licensing/compliance/) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="bdbb8-149">For more information, see the [PlayReady Compliance Rules](https://www.microsoft.com/playready/licensing/compliance/) document.</span></span>
> 
> 

<span data-ttu-id="bdbb8-150">Milyen védelmi szintek támogatja a Silverlight példát lásd: [kimeneti védelmet Silverlight támogatása](http://go.microsoft.com/fwlink/?LinkId=617318).</span><span class="sxs-lookup"><span data-stu-id="bdbb8-150">For an example of what protection levels Silverlight supports, see: [Silverlight support for output protections](http://go.microsoft.com/fwlink/?LinkId=617318).</span></span>

## <span data-ttu-id="bdbb8-151"><a id="schema"></a>PlayReady licenc sablon XML-séma</span><span class="sxs-lookup"><span data-stu-id="bdbb8-151"><a id="schema"></a>PlayReady license template XML schema</span></span>
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:ser="http://schemas.microsoft.com/2003/10/Serialization/" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:import namespace="http://schemas.microsoft.com/2003/10/Serialization/" />
      <xs:complexType name="AgcAndColorStripeRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction" />
      <xs:simpleType name="ContentType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Unspecified" />
          <xs:enumeration value="UltravioletDownload" />
          <xs:enumeration value="UltravioletStreaming" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="ContentType" nillable="true" type="tns:ContentType" />
      <xs:complexType name="ExplicitAnalogTelevisionRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="BestEffort" type="xs:boolean" />
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ExplicitAnalogTelevisionRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction" />
      <xs:complexType name="PlayReadyContentKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="PlayReadyContentKey" nillable="true" type="tns:PlayReadyContentKey" />
      <xs:complexType name="ContentEncryptionKeyFromHeader">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence />
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromHeader" nillable="true" type="tns:ContentEncryptionKeyFromHeader" />
      <xs:complexType name="ContentEncryptionKeyFromKeyIdentifier">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence>
              <xs:element minOccurs="0" name="KeyIdentifier" type="ser:guid" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromKeyIdentifier" nillable="true" type="tns:ContentEncryptionKeyFromKeyIdentifier" />
      <xs:complexType name="PlayReadyLicenseResponseTemplate">
        <xs:sequence>
          <xs:element name="LicenseTemplates" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
          <xs:element minOccurs="0" name="ResponseCustomData" nillable="true" type="xs:string">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseResponseTemplate" nillable="true" type="tns:PlayReadyLicenseResponseTemplate" />
      <xs:complexType name="ArrayOfPlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfPlayReadyLicenseTemplate" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
      <xs:complexType name="PlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AllowTestDevices" type="xs:boolean" />
          <xs:element minOccurs="0" name="BeginDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element name="ContentKey" nillable="true" type="tns:PlayReadyContentKey" />
          <xs:element minOccurs="0" name="ContentType" type="tns:ContentType">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExpirationDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="GracePeriod" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="LicenseType" type="tns:PlayReadyLicenseType" />
          <xs:element minOccurs="0" name="PlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
      <xs:simpleType name="PlayReadyLicenseType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Nonpersistent" />
          <xs:enumeration value="Persistent" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="PlayReadyLicenseType" nillable="true" type="tns:PlayReadyLicenseType" />
      <xs:complexType name="PlayReadyPlayRight">
        <xs:sequence>
          <xs:element minOccurs="0" name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AllowPassingVideoContentToUnknownOutput" type="tns:UnknownOutputPassingOption">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AnalogVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="DigitalVideoOnlyContentRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExplicitAnalogTelevisionOutputRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="FirstPlayExpiration" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComponentVideoRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComputerMonitorRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyPlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
      <xs:simpleType name="UnknownOutputPassingOption">
        <xs:restriction base="xs:string">
          <xs:enumeration value="NotAllowed" />
          <xs:enumeration value="Allowed" />
          <xs:enumeration value="AllowedWithVideoConstriction" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="UnknownOutputPassingOption" nillable="true" type="tns:UnknownOutputPassingOption" />
      <xs:complexType name="ScmsRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction" />
    </xs:schema>



## <a name="media-services-learning-paths"></a><span data-ttu-id="bdbb8-152">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="bdbb8-152">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="bdbb8-153">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="bdbb8-153">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

