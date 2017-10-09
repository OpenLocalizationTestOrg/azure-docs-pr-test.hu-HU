---
title: "aaaMedia szolgáltatások PlayReady licenc sablon – áttekintés"
description: "Ez a témakör áttekintést a PlayReady-licencsablon tooconfigure PlayReady-licencek használt."
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
ms.openlocfilehash: 5a5ba930c56f70038db204681486ebc4308199fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="media-services-playready-license-template-overview"></a><span data-ttu-id="75a8b-103">Media Services PlayReady licenc sablon – áttekintés</span><span class="sxs-lookup"><span data-stu-id="75a8b-103">Media Services PlayReady license template overview</span></span>
<span data-ttu-id="75a8b-104">Az Azure Media Services most már tartalmaz egy szolgáltatás, amelynek segítségével a Microsoft PlayReady-licencek.</span><span class="sxs-lookup"><span data-stu-id="75a8b-104">Azure Media Services now provides a service for delivering Microsoft PlayReady licenses.</span></span> <span data-ttu-id="75a8b-105">Amikor hello végfelhasználói player (például Silverlight) tooplay a PlayReady védett tartalmakat, egy kérelem elküldött toohello licenc kézbesítési szolgáltatás tooobtain licencet.</span><span class="sxs-lookup"><span data-stu-id="75a8b-105">When hello end user player (for example, Silverlight) tries tooplay your PlayReady protected content, a request is sent toohello license delivery service tooobtain a license.</span></span> <span data-ttu-id="75a8b-106">Hello licencszolgáltatás hello kérelmet jóváhagyja, ha hello licenc, amely elküldött toohello ügyfél kapcsolatos, és képes lehet használt toodecrypt és play hello megadott tartalom.</span><span class="sxs-lookup"><span data-stu-id="75a8b-106">If hello license service approves hello request, it issues hello license which is sent toohello client and can be used toodecrypt and play hello specified content.</span></span>

<span data-ttu-id="75a8b-107">Media Services Ezenfelül API-k, amelyek lehetővé teszik a PlayReady-licencek konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="75a8b-107">Media Services also provides APIs that let you configure your PlayReady licenses.</span></span> <span data-ttu-id="75a8b-108">Licencek hello jogokat tartalmaznak, és korlátozások, amelyet a hello PlayReady DRM futásidejű tooenforce amikor egy felhasználó megpróbál tooplayback a védett tartalmak.</span><span class="sxs-lookup"><span data-stu-id="75a8b-108">Licenses contain hello rights and restrictions that you want for hello PlayReady DRM runtime tooenforce when a user is trying tooplayback protected content.</span></span>
<span data-ttu-id="75a8b-109">Az alábbiakban néhány példa a PlayReady licenckorlátozások megadása:</span><span class="sxs-lookup"><span data-stu-id="75a8b-109">Below are some examples of PlayReady license restrictions that you can specify:</span></span>

* <span data-ttu-id="75a8b-110">hello DateTime mely hello licenc nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="75a8b-110">hello DateTime from which hello license is valid.</span></span>
* <span data-ttu-id="75a8b-111">hello hello licenc lejártakor DateTime típusú érték.</span><span class="sxs-lookup"><span data-stu-id="75a8b-111">hello DateTime value when hello license expires.</span></span> 
* <span data-ttu-id="75a8b-112">A hello licenc toobe hello ügyfélen az állandó tároló mentve.</span><span class="sxs-lookup"><span data-stu-id="75a8b-112">For hello license toobe saved in persistent storage on hello client.</span></span> <span data-ttu-id="75a8b-113">Állandó licenc általánosan használt tooallow offline lejátszás hello tartalom.</span><span class="sxs-lookup"><span data-stu-id="75a8b-113">Persistent licenses are typically used tooallow offline playback of hello content.</span></span>
* <span data-ttu-id="75a8b-114">hello minimális biztonsági szintjét, hogy egy player meg kell adni tooplay a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="75a8b-114">hello minimum security level that a player must have tooplay your content.</span></span> 
* <span data-ttu-id="75a8b-115">hello kimeneti védelmi szint hello kimeneti vezérlők audio\video tartalom.</span><span class="sxs-lookup"><span data-stu-id="75a8b-115">hello output protection level for hello output controls for audio\video content.</span></span> 
* <span data-ttu-id="75a8b-116">További információkért lásd: hello kimeneti vezérlők szakasz (3.5) a hello [PlayReady megfelelőségi szabályok](https://www.microsoft.com/playready/licensing/compliance/) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="75a8b-116">For more information, see hello Output Controls section (3.5) in hello [PlayReady Compliance Rules](https://www.microsoft.com/playready/licensing/compliance/) document.</span></span>

> [!NOTE]
> <span data-ttu-id="75a8b-117">Jelenleg csak is beállíthatja hello PlayRight hello PlayReady-licenc (Ez a jogosultság szükség).</span><span class="sxs-lookup"><span data-stu-id="75a8b-117">Currently, you can only configure hello PlayRight of hello PlayReady license (this right is required).</span></span> <span data-ttu-id="75a8b-118">hello PlayRight ad hello ügyfél hello képességét tooplayback hello tartalmat.</span><span class="sxs-lookup"><span data-stu-id="75a8b-118">hello PlayRight gives hello client hello ability tooplayback hello content.</span></span> <span data-ttu-id="75a8b-119">hello PlayRight is lehetővé teszi korlátozások adott tooplayback konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="75a8b-119">hello PlayRight also allows configuring restrictions specific tooplayback.</span></span> <span data-ttu-id="75a8b-120">További információkért lásd: [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).</span><span class="sxs-lookup"><span data-stu-id="75a8b-120">For more information, see [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).</span></span>
> 
> 

<span data-ttu-id="75a8b-121">tooconfigure PlayReady-licencek Media Services használatával, konfigurálnia kell a hello Media Services PlayReady licenc sablonja.</span><span class="sxs-lookup"><span data-stu-id="75a8b-121">tooconfigure PlayReady licenses using Media Services, you must configure hello Media Services PlayReady license template.</span></span> <span data-ttu-id="75a8b-122">hello sablon az XML-Fájlban van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="75a8b-122">hello template is defined in XML.</span></span>

<span data-ttu-id="75a8b-123">hello következő példa bemutatja hello legegyszerűbb (és általános) sablont, amely egy adatfolyam-továbbítási alaplicenc konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="75a8b-123">hello following example shows hello simplest (and most common) template that configures a basic streaming license.</span></span> <span data-ttu-id="75a8b-124">A licenccel rendelkező az ügyfelek képesek tooplayback lenne a PlayReady védett tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="75a8b-124">With this license, your clients would be able tooplayback your PlayReady protected content.</span></span>

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

<span data-ttu-id="75a8b-125">hello XML toohello PlayReady licenc sablon XML-séma hello PlayReady licencsablon XML-séma szakaszban meghatározott megfelel.</span><span class="sxs-lookup"><span data-stu-id="75a8b-125">hello XML conforms toohello PlayReady license template XML schema defined in hello PlayReady license template XML schema section.</span></span>

<span data-ttu-id="75a8b-126">A Media Services is meghatároz egy .NET-osztályok használt tooserialized és hello XML a deszerializált tooand ellenőrizhető.</span><span class="sxs-lookup"><span data-stu-id="75a8b-126">Media Services also defines a set of .NET classes that could be used tooserialized and deserialized tooand from hello XML.</span></span> <span data-ttu-id="75a8b-127">A fő osztályok ismertetését lásd: [Media Services .NET-osztályok](media-services-playready-license-template-overview.md#classes).</span><span class="sxs-lookup"><span data-stu-id="75a8b-127">For description of main classes, see [Media Services .NET classes](media-services-playready-license-template-overview.md#classes).</span></span> <span data-ttu-id="75a8b-128">a rendszer a használt tooconfigure licenc sablonok.</span><span class="sxs-lookup"><span data-stu-id="75a8b-128">that are used tooconfigure license templates.</span></span>

<span data-ttu-id="75a8b-129">Használja a .NET-végpontok közötti példa tooconfigure hello PlayReady licencsablon osztályok, lásd: [használatával dinamikus PlayReady-titkosítás és Licenctovábbítási szolgáltatása](media-services-protect-with-drm.md).</span><span class="sxs-lookup"><span data-stu-id="75a8b-129">For an end-to-end example that uses .NET classes tooconfigure hello PlayReady license template, see [Using PlayReady Dynamic Encryption and License Delivery Service](media-services-protect-with-drm.md).</span></span>

## <span data-ttu-id="75a8b-130"><a id="classes"></a>Media Services .NET-osztályok, amelyek használt tooconfigure licenc sablonok</span><span class="sxs-lookup"><span data-stu-id="75a8b-130"><a id="classes"></a>Media Services .NET classes that are used tooconfigure license templates</span></span>
<span data-ttu-id="75a8b-131">hello az alábbiakban hello fő .NET osztályokat használt tooconfigure Media Services PlayReady licenc sablonok.</span><span class="sxs-lookup"><span data-stu-id="75a8b-131">hello following are hello main .NET classes are used tooconfigure Media Services PlayReady license templates.</span></span> <span data-ttu-id="75a8b-132">Ezeket az osztályokat leképezése definiált toohello típusok [PlayReady licenc sablon XML-séma](media-services-playready-license-template-overview.md#schema).</span><span class="sxs-lookup"><span data-stu-id="75a8b-132">These classes map toohello types defined in [PlayReady license template XML schema](media-services-playready-license-template-overview.md#schema).</span></span>

<span data-ttu-id="75a8b-133">Hello [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) osztály használt tooserialize és tooand sablonból hello Media Services licenc XML deszerializálása.</span><span class="sxs-lookup"><span data-stu-id="75a8b-133">hello [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) class is used tooserialize and deserialize tooand from hello Media Services license template XML.</span></span>

### <a name="playreadylicenseresponsetemplate"></a><span data-ttu-id="75a8b-134">PlayReadyLicenseResponseTemplate</span><span class="sxs-lookup"><span data-stu-id="75a8b-134">PlayReadyLicenseResponseTemplate</span></span>
<span data-ttu-id="75a8b-135">[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) -Ez az osztály az hello választ küldött vissza toohello végfelhasználói hello sablon jelenti.</span><span class="sxs-lookup"><span data-stu-id="75a8b-135">[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) - This class represents hello template for hello response sent back toohello end user.</span></span> <span data-ttu-id="75a8b-136">Egyéni adatok karakterlánc hello licenckiszolgáló hello alkalmazás (hasznos lehet az egyéni alkalmazás logika), valamint egy vagy több licenc sablonok listájának között egy mezőt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="75a8b-136">It contains a field for a custom data string between hello license server and hello application (may be useful for custom app logic) as well as a list of one or more license templates.</span></span>

<span data-ttu-id="75a8b-137">Ez az hello "legfelső szintű" osztály hello sablon hierarchiában.</span><span class="sxs-lookup"><span data-stu-id="75a8b-137">This is hello “top level” class in hello template hierarchy.</span></span> <span data-ttu-id="75a8b-138">Amely hello válasz sablon licenc sablonok listáját tartalmazza, és hello licenc-sablonjai tartalmazzák a (közvetlenül vagy közvetetten) összes hello hello sablon adatok toobe szerializált alkotó más osztályok.</span><span class="sxs-lookup"><span data-stu-id="75a8b-138">Meaning that hello response template includes a list of license templates and hello license templates include (directly or indirectly) all of hello other classes that make up hello template data toobe serialized.</span></span>

### <a name="playreadylicensetemplate"></a><span data-ttu-id="75a8b-139">PlayReadyLicenseTemplate</span><span class="sxs-lookup"><span data-stu-id="75a8b-139">PlayReadyLicenseTemplate</span></span>
<span data-ttu-id="75a8b-140">[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) -hello osztály jelöli egy licencsablon a PlayReady-licencek toobe toohello végfelhasználók számára visszaadott létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="75a8b-140">[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) - hello class represents a license template for creating PlayReady licenses toobe returned toohello end users.</span></span> <span data-ttu-id="75a8b-141">A tartalomkulcs hello hello licenc hello adatokat tartalmaz, és bármely jogokat vagy korlátozásokat toobe kikényszeríti a hello PlayReady DRM futásidejű hello tartalomkulcsot használatakor.</span><span class="sxs-lookup"><span data-stu-id="75a8b-141">It contains hello data on hello content key in hello license and any rights or restrictions toobe enforced by hello PlayReady DRM runtime when using hello content key.</span></span>

### <span data-ttu-id="75a8b-142"><a id="PlayReadyPlayRight"></a>PlayReadyPlayRight</span><span class="sxs-lookup"><span data-stu-id="75a8b-142"><a id="PlayReadyPlayRight"></a>PlayReadyPlayRight</span></span>
<span data-ttu-id="75a8b-143">[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) -Ez az osztály a PlayReady-licenc PlayRight hello jelenti.</span><span class="sxs-lookup"><span data-stu-id="75a8b-143">[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) - This class represents hello PlayRight of a PlayReady license.</span></span> <span data-ttu-id="75a8b-144">Tartalom tulajdonos toohello nulla hello, vagy további korlátozásokat konfigurált hello licenccel, valamint hello (a lejátszás adott házirend) maga PlayRight hello felhasználói hello képességét tooplayback ad.</span><span class="sxs-lookup"><span data-stu-id="75a8b-144">It grants hello user hello ability tooplayback hello content subject toohello zero or more restrictions configured in hello license and on hello PlayRight itself (for playback specific policy).</span></span> <span data-ttu-id="75a8b-145">Vezérelheti lejátszható hello tartalom keresztül kimenetek hello típusú kimeneti korlátozások és kell kell bevezetni a megadott kimeneti használatakor korlátozások toodo nagy részét a hello PlayRight hello házirend tartozik.</span><span class="sxs-lookup"><span data-stu-id="75a8b-145">Much of hello policy on hello PlayRight has toodo with output restrictions which control hello types of outputs that hello content can be played over and any restrictions that must be put in place when using a given output.</span></span> <span data-ttu-id="75a8b-146">Például ha hello DigitalVideoOnlyContentRestriction engedélyezve van, majd hello DRM futásidejű csak akkor engedélyezi hello videó toobe (analóg videó kimenetek szakember toopass hello tartalom) digitális kimenetek felett jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="75a8b-146">For example, if hello DigitalVideoOnlyContentRestriction is enabled, then hello DRM runtime will only allow hello video toobe displayed over digital outputs (analog video outputs won’t be allowed toopass hello content).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75a8b-147">Ezek a típusok korlátozások nagyon hatékonyak lehetnek, de hello felhasználói élmény is hatással lehet.</span><span class="sxs-lookup"><span data-stu-id="75a8b-147">These types of restrictions can be very powerful but can also affect hello consumer experience.</span></span> <span data-ttu-id="75a8b-148">Ha hello kimeneti védelmet túl szigorú vannak konfigurálva, előfordulhat, hogy hello tartalmát az egyes ügyfelek játsszák le.</span><span class="sxs-lookup"><span data-stu-id="75a8b-148">If hello output protections are configured too restrictive, hello content might be unplayable on some clients.</span></span> <span data-ttu-id="75a8b-149">További információkért lásd: hello [PlayReady megfelelőségi szabályok](https://www.microsoft.com/playready/licensing/compliance/) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="75a8b-149">For more information, see hello [PlayReady Compliance Rules](https://www.microsoft.com/playready/licensing/compliance/) document.</span></span>
> 
> 

<span data-ttu-id="75a8b-150">Milyen védelmi szintek támogatja a Silverlight példát lásd: [kimeneti védelmet Silverlight támogatása](http://go.microsoft.com/fwlink/?LinkId=617318).</span><span class="sxs-lookup"><span data-stu-id="75a8b-150">For an example of what protection levels Silverlight supports, see: [Silverlight support for output protections](http://go.microsoft.com/fwlink/?LinkId=617318).</span></span>

## <span data-ttu-id="75a8b-151"><a id="schema"></a>PlayReady licenc sablon XML-séma</span><span class="sxs-lookup"><span data-stu-id="75a8b-151"><a id="schema"></a>PlayReady license template XML schema</span></span>
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



## <a name="media-services-learning-paths"></a><span data-ttu-id="75a8b-152">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="75a8b-152">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="75a8b-153">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="75a8b-153">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

