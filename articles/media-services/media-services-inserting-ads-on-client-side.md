---
title: "hello ügyféloldalon aaaInserting ads |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooinsert ads a hello ügyféloldali."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 65c9c747-128e-497e-afe0-3f92d2bf7972
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: e6eab4aa92918ad734db8ac3a4e7818d02ed7fe4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="inserting-ads-on-hello-client-side"></a><span data-ttu-id="d96d9-103">Hello ügyféloldalon ads beszúrása</span><span class="sxs-lookup"><span data-stu-id="d96d9-103">Inserting ads on hello client side</span></span>
<span data-ttu-id="d96d9-104">Ez a témakör ismerteti tooinsert hello ügyféloldalon ads különböző típusú.</span><span class="sxs-lookup"><span data-stu-id="d96d9-104">This topic contains information on how tooinsert various types of ads on hello client side.</span></span>

<span data-ttu-id="d96d9-105">Az élő adatfolyam-továbbítási videók lezárt feliratok és az ad támogatásával kapcsolatos további információkért lásd: [támogatott kódolt feliratok és Ad beszúrási szabványok](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).</span><span class="sxs-lookup"><span data-stu-id="d96d9-105">For information about closed captioning and ad support in Live streaming videos, see [Supported Closed Captioning and Ad Insertion Standards](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).</span></span>

> [!NOTE]
> <span data-ttu-id="d96d9-106">Az Azure Media Player jelenleg nem támogatja a hirdetések.</span><span class="sxs-lookup"><span data-stu-id="d96d9-106">Azure Media Player does not currently support Ads.</span></span>
> 
> 

## <span data-ttu-id="d96d9-107"><a id="insert_ads_into_media"></a>A Media Ads beszúrása</span><span class="sxs-lookup"><span data-stu-id="d96d9-107"><a id="insert_ads_into_media"></a>Inserting Ads into your Media</span></span>
<span data-ttu-id="d96d9-108">Az Azure Media Services támogatást nyújt a Windows Media Platform hello keresztül ad beszúrási: lejátszó-Keretrendszerekhez.</span><span class="sxs-lookup"><span data-stu-id="d96d9-108">Azure Media Services provides support for ad insertion through hello Windows Media Platform: Player Frameworks.</span></span> <span data-ttu-id="d96d9-109">Ad-támogatással rendelkező lejátszó-keretrendszerekhez Windows 8, a Silverlight, a Windows Phone 8 és az iOS-eszközök érhetők el.</span><span class="sxs-lookup"><span data-stu-id="d96d9-109">Player frameworks with ad support are available for Windows 8, Silverlight, Windows Phone 8, and iOS devices.</span></span> <span data-ttu-id="d96d9-110">Minden egyes player keretrendszer minta kódot tartalmaz, amely bemutatja, hogyan tooimplement a lejátszóalkalmazás. Nincsenek media: listájába szúrhatók ads három különböző típusú.</span><span class="sxs-lookup"><span data-stu-id="d96d9-110">Each player framework contains sample code that shows you how tooimplement a player application.There are three different kinds of ads you can insert into your media:list.</span></span>

* <span data-ttu-id="d96d9-111">**Lineáris** – teljes keret hirdetések hello fő videó felfüggesztése.</span><span class="sxs-lookup"><span data-stu-id="d96d9-111">**Linear** – full frame ads that pause hello main video.</span></span>
* <span data-ttu-id="d96d9-112">**Nem lineáris** – jelennek meg hello fő videó lejátszása hirdetések átmeneti területre, általában embléma vagy egyéb statikus kép elhelyezett hello player belül.</span><span class="sxs-lookup"><span data-stu-id="d96d9-112">**Nonlinear** – overlay ads that are displayed as hello main video is playing, usually a logo or other static image placed within hello player.</span></span>
* <span data-ttu-id="d96d9-113">**Kiegészítő** – hello player kívül megjelenített ads.</span><span class="sxs-lookup"><span data-stu-id="d96d9-113">**Companion** – ads that are displayed outside of hello player.</span></span>

<span data-ttu-id="d96d9-114">ADs hello fő videó idősorán bármikor helyezhető.</span><span class="sxs-lookup"><span data-stu-id="d96d9-114">Ads can be placed at any point in hello main video’s time line.</span></span> <span data-ttu-id="d96d9-115">Meg kell állapítani, hogy hello player amikor tooplay hello ad, és amely ads tooplay.</span><span class="sxs-lookup"><span data-stu-id="d96d9-115">You must tell hello player when tooplay hello ad and which ads tooplay.</span></span> <span data-ttu-id="d96d9-116">Ebben az esetben a szabványos XML alapú fájlok készletből: videó Ad szolgáltatás sablon (VAST), a digitális videót több Ad lista (VMAP), a Media absztrakt alkalmazás-előkészítés sablon (OSZLOPOS) és a digitális videót Player Ad felület Definition (VPAID).</span><span class="sxs-lookup"><span data-stu-id="d96d9-116">This is done using a set of standard XML-based files: Video Ad Service Template (VAST), Digital Video Multiple Ad Playlist (VMAP), Media Abstract Sequencing Template (MAST), and Digital Video Player Ad Interface Definition (VPAID).</span></span> <span data-ttu-id="d96d9-117">NAGY fájlok adja meg, milyen ads toodisplay.</span><span class="sxs-lookup"><span data-stu-id="d96d9-117">VAST files specify what ads toodisplay.</span></span> <span data-ttu-id="d96d9-118">VMAP fájlok adja meg, mikor tooplay különböző hirdetések és HATALMAS XML kódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d96d9-118">VMAP files specify when tooplay various ads and contain VAST XML.</span></span> <span data-ttu-id="d96d9-119">A fájlok OSZLOPOS egy másik módja toosequence ads is tartalmazó túlnyomó XML.</span><span class="sxs-lookup"><span data-stu-id="d96d9-119">MAST files are another way toosequence ads which also can contain VAST XML.</span></span> <span data-ttu-id="d96d9-120">VPAID fájlok meghatározása hello videólejátszó és hello ad vagy ad-kiszolgáló közötti illesztőfelületet szolgáltasson.</span><span class="sxs-lookup"><span data-stu-id="d96d9-120">VPAID files define an interface between hello video player and hello ad or ad server.</span></span>

<span data-ttu-id="d96d9-121">Minden egyes player keretrendszer eltérő módon működik, és minden egyes saját témakör tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="d96d9-121">Each player framework works differently and each will be covered in its own topic.</span></span> <span data-ttu-id="d96d9-122">Ez a témakör ismerteti, hello használt alapvető mechanizmusok tooinsert ads. Videólejátszó alkalmazások ads kérhet egy ad-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d96d9-122">This topic will describe hello basic mechanisms used tooinsert ads.Video player applications request ads from an ad server.</span></span> <span data-ttu-id="d96d9-123">hello ad kiszolgáló többféle módon válaszolhat:</span><span class="sxs-lookup"><span data-stu-id="d96d9-123">hello ad server can respond in a number of ways:</span></span>

* <span data-ttu-id="d96d9-124">Térjen vissza a túlnyomó fájl</span><span class="sxs-lookup"><span data-stu-id="d96d9-124">Return a VAST file</span></span>
* <span data-ttu-id="d96d9-125">Térjen vissza a VMAP fájlt (a beágyazott VAST)</span><span class="sxs-lookup"><span data-stu-id="d96d9-125">Return a VMAP file (with embedded VAST)</span></span>
* <span data-ttu-id="d96d9-126">Egy OSZLOPOS fájlt (a beágyazott VAST) visszaadása</span><span class="sxs-lookup"><span data-stu-id="d96d9-126">Return a MAST file (with embedded VAST)</span></span>
* <span data-ttu-id="d96d9-127">Térjen vissza a VPAID ads túlnyomó fájl</span><span class="sxs-lookup"><span data-stu-id="d96d9-127">Return a VAST file with VPAID ads</span></span>

### <a name="using-a-video-ad-service-template-vast-file"></a><span data-ttu-id="d96d9-128">Videó Ad szolgáltatás sablon (VAST) fájl használatával</span><span class="sxs-lookup"><span data-stu-id="d96d9-128">Using a Video Ad Service Template (VAST) File</span></span>
<span data-ttu-id="d96d9-129">Egy túlnyomó fájlt határozza meg, milyen ad vagy ads toodisplay.</span><span class="sxs-lookup"><span data-stu-id="d96d9-129">A VAST file specifies what ad or ads toodisplay.</span></span> <span data-ttu-id="d96d9-130">hello következő XML-kódja egy túlnyomó fájlt egy lineáris ad példát:</span><span class="sxs-lookup"><span data-stu-id="d96d9-130">hello following XML is an example of a VAST file for a linear ad:</span></span>

    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>

<span data-ttu-id="d96d9-131">hello lineáris ad hello le <**lineáris**> elemet.</span><span class="sxs-lookup"><span data-stu-id="d96d9-131">hello linear ad is described by hello <**Linear**> element.</span></span> <span data-ttu-id="d96d9-132">Hello ad hello időtartama határoz meg, nyomon követés, átkattintással, követési kattintson, és számos **MediaFile** elemek.</span><span class="sxs-lookup"><span data-stu-id="d96d9-132">It specifies hello duration of hello ad, tracking events, click through, click tracking, and a number of **MediaFile** elements.</span></span> <span data-ttu-id="d96d9-133">Nyomkövetési események hello belül vannak megadva <**TrackingEvents**> elemet, és teszi lehetővé az Active server tootrack hello ad megtekintése közben előforduló különféle események.</span><span class="sxs-lookup"><span data-stu-id="d96d9-133">Tracking events are specified within hello <**TrackingEvents**> element and allow an ad server tootrack various events that occur while viewing hello ad.</span></span> <span data-ttu-id="d96d9-134">Ebben az esetben hello start, középpont, befejeződött, és bontsa ki a események nyomon követi.</span><span class="sxs-lookup"><span data-stu-id="d96d9-134">In this case hello start, midpoint, complete, and expand events are tracked.</span></span> <span data-ttu-id="d96d9-135">hello start esemény következik be, amikor hello ad megjelenik.</span><span class="sxs-lookup"><span data-stu-id="d96d9-135">hello start event occurs when hello ad is displayed.</span></span> <span data-ttu-id="d96d9-136">hello középpont esemény következik be, legalább 50 % hello ad idősor már megtekintett.</span><span class="sxs-lookup"><span data-stu-id="d96d9-136">hello midpoint event occurs when at least 50% of hello ad’s timeline has been viewed.</span></span> <span data-ttu-id="d96d9-137">hello befejeződésének eseményét hello ad toohello end futtatásakor következik be.</span><span class="sxs-lookup"><span data-stu-id="d96d9-137">hello complete event occurs when hello ad has run toohello end.</span></span> <span data-ttu-id="d96d9-138">hello felhasználói bővíti hello videólejátszó toofull képernyő hello kibontott esemény következik be.</span><span class="sxs-lookup"><span data-stu-id="d96d9-138">hello Expand event occurs when hello user expands hello video player toofull screen.</span></span> <span data-ttu-id="d96d9-139">Clickthroughs vannak megadva, a <**Átkattintós**> elemen belül egy <**VideoClicks**> elemet, és egy URI tooa erőforrás toodisplay határozza meg, ha hello felhasználó hello ad kattint.</span><span class="sxs-lookup"><span data-stu-id="d96d9-139">Clickthroughs are specified with a <**ClickThrough**> element within a <**VideoClicks**> element and specifies a URI tooa resource toodisplay when hello user clicks on hello ad.</span></span> <span data-ttu-id="d96d9-140">ClickTracking van megadva a <**ClickTracking**> elemet, is hello <**VideoClicks**> elem és hello player toorequest követési erőforrás határozza meg, amikor hello felhasználó kattint a hello ad.hello <**MediaFile**> elemek adjon meg egy adott kódolását, az ad információt.</span><span class="sxs-lookup"><span data-stu-id="d96d9-140">ClickTracking is specified in a <**ClickTracking**> element, also within hello <**VideoClicks**> element and specifies a tracking resource for hello player toorequest when hello user clicks on hello ad.hello <**MediaFile**> elements specify information about a specific encoding of an ad.</span></span> <span data-ttu-id="d96d9-141">Ha egynél több <**MediaFile**> elem, hello videólejátszó választhat hello legjobb kódolásának hello platform.</span><span class="sxs-lookup"><span data-stu-id="d96d9-141">When there is more than one <**MediaFile**> element, hello video player can choose hello best encoding for hello platform.</span></span> 

<span data-ttu-id="d96d9-142">Lineáris ads megjeleníthető a megadott sorrendben.</span><span class="sxs-lookup"><span data-stu-id="d96d9-142">Linear ads can be displayed in a specified order.</span></span> <span data-ttu-id="d96d9-143">toodo, adja hozzá a további <Ad> elemek toohello VAST fájlt, és adja meg a hello rendelés hello feladatütemezési attribútum használatával.</span><span class="sxs-lookup"><span data-stu-id="d96d9-143">toodo this, add additional <Ad> elements toohello VAST file and specify hello order using hello sequence attribute.</span></span> <span data-ttu-id="d96d9-144">hello a következő példa ezt mutatja be:</span><span class="sxs-lookup"><span data-stu-id="d96d9-144">hello following example illustrates this:</span></span>

    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>

<span data-ttu-id="d96d9-145">Nem lineáris ads megadott egy <Creative> elemet is.</span><span class="sxs-lookup"><span data-stu-id="d96d9-145">Nonlinear ads are specified in a <Creative> element as well.</span></span> <span data-ttu-id="d96d9-146">a következő példa azt mutatja meg hello egy <Creative> elem az lineáris ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-146">hello following example shows a <Creative> element that describes a nonlinear ad.</span></span>

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>


<span data-ttu-id="d96d9-147">hello <**NonLinearAds**> elem tartalmazhat egy vagy több <**NonLinear**> elemek, amelyek leírhatja lineáris ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-147">hello <**NonLinearAds**> element can contain one or more <**NonLinear**> elements, each of which can describe a nonlinear ad.</span></span> <span data-ttu-id="d96d9-148">hello <**NonLinear**> elem hello erőforrás hello lineáris ad határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d96d9-148">hello <**NonLinear**> element specifies hello resource for hello nonlinear ad.</span></span> <span data-ttu-id="d96d9-149">hello erőforrás lehet egy <**StaticResouce**>, <**IFrameResource**>, vagy egy <**HTMLResouce**>.</span><span class="sxs-lookup"><span data-stu-id="d96d9-149">hello resource can be a <**StaticResouce**>, an <**IFrameResource**>, or an <**HTMLResouce**>.</span></span><span data-ttu-id="d96d9-150"> <**StaticResource**> nem HTML erőforrás ismerteti, és határozza meg a creativeType attribútum, amely meghatározza, hogyan hello erőforrás jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="d96d9-150"> <**StaticResource**> describes a non-HTML resource and defines a creativeType attribute that specifies how hello resource is displayed:</span></span>

<span data-ttu-id="d96d9-151">Kép/gif, a kép/jpeg, a lemezkép vagy png – hello erőforrás megjelenik egy HTML <**img**> címke.</span><span class="sxs-lookup"><span data-stu-id="d96d9-151">Image/gif, image/jpeg, image/png – hello resource is displayed in an HTML <**img**> tag.</span></span>

<span data-ttu-id="d96d9-152">Alkalmazás/x-javascript – hello erőforrás megjelenik egy HTML <**parancsfájl**> címke.</span><span class="sxs-lookup"><span data-stu-id="d96d9-152">Application/x-javascript – hello resource is displayed in an HTML <**script**> tag.</span></span>

<span data-ttu-id="d96d9-153">Alkalmazás/x-shockwave-flash – hello erőforrás Flash player jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d96d9-153">Application/x-shockwave-flash – hello resource is displayed in a Flash player.</span></span>

<span data-ttu-id="d96d9-154">**IFrameResource** egy HTML-erőforrás IFRAME megjeleníthető ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d96d9-154">**IFrameResource** describes an HTML resource that can be displayed in an IFrame.</span></span> <span data-ttu-id="d96d9-155">**HTMLResource** szúrhatók be egy weblap, HTML-kódja egy adat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d96d9-155">**HTMLResource** describes a piece of HTML code that can be inserted into a web page.</span></span> <span data-ttu-id="d96d9-156">**TrackingEvents** adja meg a nyomkövetési események és URI toorequest hello hello esemény bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="d96d9-156">**TrackingEvents** specify tracking events and hello URI toorequest when hello event occurs.</span></span> <span data-ttu-id="d96d9-157">Ez a minta hello acceptInvitation és összecsukása események követi.</span><span class="sxs-lookup"><span data-stu-id="d96d9-157">In this sample hello acceptInvitation and collapse events are tracked.</span></span> <span data-ttu-id="d96d9-158">További információ a hello **NonLinearAds** elem és a gyermekek, lásd: IAB.NET/VAST.</span><span class="sxs-lookup"><span data-stu-id="d96d9-158">For more information on hello **NonLinearAds** element and its children, see IAB.NET/VAST.</span></span> <span data-ttu-id="d96d9-159">Vegye figyelembe, hogy hello **TrackingEvents** elem a helyen belüli hello **NonLinearAds** hello helyett elem **NonLinear** elemet.</span><span class="sxs-lookup"><span data-stu-id="d96d9-159">Note that hello **TrackingEvents** element is located within hello **NonLinearAds** element rather than hello **NonLinear** element.</span></span>

<span data-ttu-id="d96d9-160">Kiegészítő ads meghatározott egy <CompanionAds> elemet.</span><span class="sxs-lookup"><span data-stu-id="d96d9-160">Companion ads are defined within a <CompanionAds> element.</span></span> <span data-ttu-id="d96d9-161">Hello <CompanionAds> elem tartalmazhat egy vagy több <Companion> elemek.</span><span class="sxs-lookup"><span data-stu-id="d96d9-161">hello <CompanionAds> element can contain one or more <Companion> elements.</span></span> <span data-ttu-id="d96d9-162">Minden egyes <Companion> elem egy kiegészítő ad ismerteti, és tartalmazhat egy <StaticResource>, <IFrameResource>, vagy <HTMLResource> amely meg van határozva a hello azonos módon, egy lineáris az ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d96d9-162">Each <Companion> element describes a companion ad and can contain a <StaticResource>, <IFrameResource>, or <HTMLResource> which are specified in hello same way as in a nonlinear ad.</span></span> <span data-ttu-id="d96d9-163">A túlnyomó is tartalmazhatnak, több kiegészítő ads, és hello lejátszóalkalmazás hello legmegfelelőbb ad toodisplay választhat.</span><span class="sxs-lookup"><span data-stu-id="d96d9-163">A VAST file can contain multiple companion ads and hello player application can choose hello most appropriate ad toodisplay.</span></span> <span data-ttu-id="d96d9-164">VAST kapcsolatos további információkért lásd: [túlnyomó 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span><span class="sxs-lookup"><span data-stu-id="d96d9-164">For more information about VAST, see [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span></span>

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a><span data-ttu-id="d96d9-165">Egy digitális videót több Ad-lista (VMAP) fájl használatával</span><span class="sxs-lookup"><span data-stu-id="d96d9-165">Using a Digital Video Multiple Ad Playlist (VMAP) File</span></span>
<span data-ttu-id="d96d9-166">Egy VMAP fájl lehetővé teszi a toospecify ad oldaltörések esetén, mennyi ideig egyes szünetek, hány ads belül szünet jeleníthető meg, és ads típusú lehet megszakítás alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d96d9-166">A VMAP file allows you toospecify when ad breaks occur, how long each break is, how many ads can be displayed within a break, and what types of ads may be displayed during a break.</span></span> <span data-ttu-id="d96d9-167">a következő egy példa VMAP fájl, amely meghatározza egy egyetlen ad break hello:</span><span class="sxs-lookup"><span data-stu-id="d96d9-167">hello following in an example VMAP file that defines a single ad break:</span></span>

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

<span data-ttu-id="d96d9-168">Egy VMAP fájl kezdődik egy <VMAP> elem, amely tartalmazza egy vagy több <AdBreak> elemek, egy ad break meghatározása.</span><span class="sxs-lookup"><span data-stu-id="d96d9-168">A VMAP file begins with a <VMAP> element that contains one or more <AdBreak> elements, each defining an ad break.</span></span> <span data-ttu-id="d96d9-169">Minden ad break határozza meg, egy break típusát, a sortörés azonosítója és a idő eltolódását.</span><span class="sxs-lookup"><span data-stu-id="d96d9-169">Each ad break specifies a break type, break ID, and time offset.</span></span> <span data-ttu-id="d96d9-170">hello breakType attribútum meghatározza során hello break lejátszható ad hello típusát: lineáris, nem lineáris, vagy megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="d96d9-170">hello breakType attribute specifies hello type of ad that can be played during hello break: linear, nonlinear, or display.</span></span> <span data-ttu-id="d96d9-171">Megjelenítési ads tooVAST kiegészítő ads hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="d96d9-171">Display ads map tooVAST companion ads.</span></span> <span data-ttu-id="d96d9-172">Egynél több ad-típus (szóközök nélkül) vesszővel elválasztott listában adható meg.</span><span class="sxs-lookup"><span data-stu-id="d96d9-172">More than one ad type can be specified in a comma (no spaces) separated list.</span></span> <span data-ttu-id="d96d9-173">hello breakID pedig hello ad azonosítója, amelyet a nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="d96d9-173">hello breakID is an optional identifier for hello ad.</span></span> <span data-ttu-id="d96d9-174">hello timeOffset határozza meg, amikor hello ad üzenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="d96d9-174">hello timeOffset specifies when hello ad should be displayed.</span></span> <span data-ttu-id="d96d9-175">Ez a következő módokon hello egyikében adható meg:</span><span class="sxs-lookup"><span data-stu-id="d96d9-175">It can be specified in one of hello following ways:</span></span>

1. <span data-ttu-id="d96d9-176">Idő óó: pp: vagy hh:mm:ss.mmm formátumban, ahol .mmm az ezredmásodperc.</span><span class="sxs-lookup"><span data-stu-id="d96d9-176">Time – in hh:mm:ss or hh:mm:ss.mmm format where .mmm is milliseconds.</span></span> <span data-ttu-id="d96d9-177">Ez az attribútum értékének hello hello idejét hello videó ütemterv toohello elejére hello ad break hello kezdetén határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d96d9-177">hello value of this attribute specifies hello time from hello beginning of hello video timeline toohello beginning of hello ad break.</span></span>
2. <span data-ttu-id="d96d9-178">Százalékos – n % formátumban, ahol n az hello ad lejátszás előtt hello videó ütemterv tooplay hello százaléka</span><span class="sxs-lookup"><span data-stu-id="d96d9-178">Percentage – in n% format where n is hello percentage of hello video timeline tooplay before playing hello ad</span></span>
3. <span data-ttu-id="d96d9-179">Kezdő és záró – Megadja, hogy egy ad üzenetnek kell megjelennie, előtt vagy után hello videó meg lett jelenítve.</span><span class="sxs-lookup"><span data-stu-id="d96d9-179">Start/End – specifies that an ad should be displayed before or after hello video has been displayed</span></span>
4. <span data-ttu-id="d96d9-180">Helyezze – hello ad oldaltörések hello időzítése ismeretlen, például az élő adatfolyam-esetén adja meg a ad oldaltörések hello sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="d96d9-180">Position – specifies hello order of ad breaks when hello timing of hello ad breaks is unknown, such as in live streaming.</span></span> <span data-ttu-id="d96d9-181">minden ad break hello sorrendjének hello #n formátumban, ahol n az 1 vagy nagyobb egész szám van megadva.</span><span class="sxs-lookup"><span data-stu-id="d96d9-181">hello order of each ad break is specified in hello #n format where n is an integer 1 or greater.</span></span> <span data-ttu-id="d96d9-182">1 fiókoldala hello ad lejátszani hello első alkalommal, 2 fiókoldala hello ad lejátszani hello második lehetőség, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="d96d9-182">1 signifies hello ad should be played at hello first opportunity, 2 signifies hello ad should be played at hello second opportunity and so on.</span></span>

<span data-ttu-id="d96d9-183">Hello belül <**AdBreak**> elem nem lehet az egyik <**AdSource**> elemet.</span><span class="sxs-lookup"><span data-stu-id="d96d9-183">Within hello <**AdBreak**> element there can be one <**AdSource**> element.</span></span> <span data-ttu-id="d96d9-184">hello <**AdSource**> elem tartalmazza-e a következő attribútumok hello:</span><span class="sxs-lookup"><span data-stu-id="d96d9-184">hello <**AdSource**> element contains hello following attributes:</span></span>

1. <span data-ttu-id="d96d9-185">Azonosító – Megadja hello ad forrás azonosítója</span><span class="sxs-lookup"><span data-stu-id="d96d9-185">Id – specifies an identifier for hello ad source</span></span>
2. <span data-ttu-id="d96d9-186">allowMultipleAds – egy logikai érték, amely meghatározza, hogy több ads is megjelenjen-e hello ad break során</span><span class="sxs-lookup"><span data-stu-id="d96d9-186">allowMultipleAds – a Boolean value that specifies whether multiple ads can be displayed during hello ad break</span></span>
3. <span data-ttu-id="d96d9-187">followRedirects – egy választható logikai érték, amely meghatározza, hogy ha hello videólejátszó kell tiszteletben átirányítja a felhasználókat egy ad választ belül</span><span class="sxs-lookup"><span data-stu-id="d96d9-187">followRedirects – an optional Boolean value that specifies if hello video player should honor redirects within an ad response</span></span>

<span data-ttu-id="d96d9-188">hello <**AdSource**> elem biztosít hello player egy beágyazott ad választ, vagy hivatkozás tooan ad választ.</span><span class="sxs-lookup"><span data-stu-id="d96d9-188">hello <**AdSource**> element provides hello player an inline ad response or a reference tooan ad response.</span></span> <span data-ttu-id="d96d9-189">A következő elemek hello egyik tartalmazhat:</span><span class="sxs-lookup"><span data-stu-id="d96d9-189">It can contain one of hello following elements:</span></span>

* <span data-ttu-id="d96d9-190"><VASTAdData>azt jelzi, hogy túlnyomó ad választ hello VMAP fájlban van beágyazva.</span><span class="sxs-lookup"><span data-stu-id="d96d9-190"><VASTAdData> indicates a VAST ad response is embedded within hello VMAP file</span></span>
* <span data-ttu-id="d96d9-191"><AdTagURI>URI, amely egy ad választ hivatkozik másik rendszerről</span><span class="sxs-lookup"><span data-stu-id="d96d9-191"><AdTagURI> a URI that references an ad response from another system</span></span>
* <span data-ttu-id="d96d9-192"><CustomAdData>– egy tetszőleges karakterlánc, adott respresents nem túlnyomó választ</span><span class="sxs-lookup"><span data-stu-id="d96d9-192"><CustomAdData> -an arbitrary string that respresents a non-VAST response</span></span>

<span data-ttu-id="d96d9-193">Ebben a példában egy beágyazott ad választ meg van adva egy <VASTAdData> elem, amely tartalmazza a túlnyomó ad választ.</span><span class="sxs-lookup"><span data-stu-id="d96d9-193">In this example an in-line ad response is specified with a <VASTAdData> element that contains a VAST ad response.</span></span> <span data-ttu-id="d96d9-194">További információ hello más elemek: [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).</span><span class="sxs-lookup"><span data-stu-id="d96d9-194">For more information about hello other elements, see [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).</span></span>

<span data-ttu-id="d96d9-195">hello <**AdBreak**> elem is tartalmazhat egy <**TrackingEvents**> elemet.</span><span class="sxs-lookup"><span data-stu-id="d96d9-195">hello <**AdBreak**> element can also contain one <**TrackingEvents**> element.</span></span> <span data-ttu-id="d96d9-196">hello <**TrackingEvents**> elem lehetővé teszi a tootrack hello kezdeti vagy záró egy ad break vagy e hiba történ hello ad sortörés.</span><span class="sxs-lookup"><span data-stu-id="d96d9-196">hello <**TrackingEvents**> element allows you tootrack hello start or end of an ad break or whether an error occurred during hello ad break.</span></span> <span data-ttu-id="d96d9-197">hello <**TrackingEvents**> elem tartalmaz egy vagy több <**követési**> elemek, amelyek mindegyike egy nyomkövetési esemény és egy követési URI adja meg.</span><span class="sxs-lookup"><span data-stu-id="d96d9-197">hello <**TrackingEvents**> element contains one or more <**Tracking**> elements, each of which specifies a tracking event and a tracking URI.</span></span> <span data-ttu-id="d96d9-198">hello lehetséges nyomkövetési események a következők:</span><span class="sxs-lookup"><span data-stu-id="d96d9-198">hello possible tracking events are:</span></span>

1. <span data-ttu-id="d96d9-199">breakStart – nyomon követi az ad-break hello kezdete</span><span class="sxs-lookup"><span data-stu-id="d96d9-199">breakStart – tracks hello beginning of an ad break</span></span>
2. <span data-ttu-id="d96d9-200">breakEnd – egy ad break hello befejezésének nyomon követése</span><span class="sxs-lookup"><span data-stu-id="d96d9-200">breakEnd – track hello completion of an ad break</span></span>
3. <span data-ttu-id="d96d9-201">Hiba – hello ad break bekövetkezett hiba követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="d96d9-201">error – tracks an error that occurred during hello ad break</span></span>

<span data-ttu-id="d96d9-202">a következő példa hello jeleníti meg, amely meghatározza a nyomkövetési események VMAP fájl</span><span class="sxs-lookup"><span data-stu-id="d96d9-202">hello following example shows a VMAP file that specifies tracking events</span></span>

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

<span data-ttu-id="d96d9-203">További információ a hello <**TrackingEvents**> elem és a gyermekek, lásd: http://iab.org/VMAP.pdf</span><span class="sxs-lookup"><span data-stu-id="d96d9-203">For more information on hello <**TrackingEvents**> element and its children, see http://iab.org/VMAP.pdf</span></span>

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a><span data-ttu-id="d96d9-204">Egy Media absztrakt alkalmazás-előkészítés sablonfájl (OSZLOPOS) használatával</span><span class="sxs-lookup"><span data-stu-id="d96d9-204">Using a Media Abstract Sequencing Template (MAST) File</span></span>
<span data-ttu-id="d96d9-205">Egy OSZLOPOS fájl lehetővé teszi toospecify eseményindítókat, amelyek meghatározzák, ha az ad jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d96d9-205">A MAST file allows you toospecify triggers that define when an ad is displayed.</span></span> <span data-ttu-id="d96d9-206">hello az alábbiakban látható egy példa egy előtti összegző ad, egy közepes összegző ad és a utáni összegző ad eseményindítók tartalmazó OSZLOPOS fájlt.</span><span class="sxs-lookup"><span data-stu-id="d96d9-206">hello following is an example MAST file that contains triggers for a pre roll ad, a mid-roll ad, and a post-roll ad.</span></span>

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' hello trigger for hello next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>



<span data-ttu-id="d96d9-207">Egy OSZLOPOS fájl kezdődik egy **OSZLOPOS** elem, amely egy **eseményindítók** elemet.</span><span class="sxs-lookup"><span data-stu-id="d96d9-207">A MAST file begins with a **MAST** element that contains one **triggers** element.</span></span> <span data-ttu-id="d96d9-208">Hello <triggers> egy vagy több elemet tartalmaz **eseményindító** határozza meg, ha az ad lejátszani.</span><span class="sxs-lookup"><span data-stu-id="d96d9-208">hello <triggers> element contains one or more **trigger** elements that define when an ad should be played.</span></span> 

<span data-ttu-id="d96d9-209">Hello **eseményindító** elem tartalmazza-e egy **startConditions** elem, amelynek adja meg, ha az ad tooplay kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="d96d9-209">hello **trigger** element contains a **startConditions** element which specify when an ad should begin tooplay.</span></span> <span data-ttu-id="d96d9-210">Hello **startConditions** egy vagy több elemet tartalmaz <condition> elemek.</span><span class="sxs-lookup"><span data-stu-id="d96d9-210">hello **startConditions** element contains one or more <condition> elements.</span></span> <span data-ttu-id="d96d9-211">Ha minden <condition> tootrue eseményindító kezdeményezett, vagy visszavonták, attól függően, hogy kiértékeli hello <condition> magában foglal egy **startConditions** vagy **endConditions** elem kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="d96d9-211">When each <condition> evaluates tootrue a trigger is initiated or revoked depending upon whether hello <condition> is contained within a **startConditions** or **endConditions** element respectively.</span></span> <span data-ttu-id="d96d9-212">Ha több <condition> elem is szerepel, azokat egy implicit vagy számít, a feltétel kiértékelése tootrue hatására hello eseményindító tooinitiate.</span><span class="sxs-lookup"><span data-stu-id="d96d9-212">When multiple <condition> elements are present, they are treated as an implicit OR, any condition evaluating tootrue will cause hello trigger tooinitiate.</span></span> <span data-ttu-id="d96d9-213"><condition>elemek egymásba ágyazható.</span><span class="sxs-lookup"><span data-stu-id="d96d9-213"><condition> elements can be nested.</span></span> <span data-ttu-id="d96d9-214">Ha gyermek <condition> elemek előre be van állítva, akkor számít egy implicit és, minden feltétel ki kell értékelnie, hogy a hello eseményindító tooinitiate tootrue.</span><span class="sxs-lookup"><span data-stu-id="d96d9-214">When child <condition> elements are preset, they are treated as an implicit AND, all conditions must evaluate tootrue for hello trigger tooinitiate.</span></span> <span data-ttu-id="d96d9-215">Hello <condition> elem tartalmazza-e a következő attribútum is definiál hello feltétel hello:</span><span class="sxs-lookup"><span data-stu-id="d96d9-215">hello <condition> element contains hello following attributes that define hello condition:</span></span> 

1. <span data-ttu-id="d96d9-216">**típus** – hello adja meg az állapot, esemény vagy tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d96d9-216">**type** – specifies hello type of condition, event or property</span></span>
2. <span data-ttu-id="d96d9-217">**név** – hello kiértékelés során használt hello tulajdonság vagy esemény toobe neve</span><span class="sxs-lookup"><span data-stu-id="d96d9-217">**name** – hello name of hello property or event toobe used during evaluation</span></span>
3. <span data-ttu-id="d96d9-218">**érték** – hello érték, amely egy tulajdonság értékelni</span><span class="sxs-lookup"><span data-stu-id="d96d9-218">**value** – hello value that a property will be evaluated against</span></span>
4. <span data-ttu-id="d96d9-219">**operátor** – hello művelet toouse kiértékelése közben: EQ (egyenlő), a NEQ (nem egyenlő), a GTR (nagyobb), a GEQ (nagyobb vagy egyenlő), a LT (kisebb), LEQ (kisebb vagy egyenlő), MOD ELEMET (Maradékos osztás)</span><span class="sxs-lookup"><span data-stu-id="d96d9-219">**operator** – hello operation toouse during evaluation: EQ (equal), NEQ (not equal), GTR (greater), GEQ (greater or equal), LT (Less than), LEQ (less than or equal), MOD (modulo)</span></span>

<span data-ttu-id="d96d9-220">**endConditions** is tartalmazhat, <condition> elemek.</span><span class="sxs-lookup"><span data-stu-id="d96d9-220">**endConditions** also contain <condition> elements.</span></span> <span data-ttu-id="d96d9-221">Ha a feltétel tootrue hello eseményindító-e reset.hello <trigger> elem is tartalmaz egy <sources> elem, amely tartalmazza egy vagy több <source> elemek.</span><span class="sxs-lookup"><span data-stu-id="d96d9-221">When a condition evaluates tootrue hello trigger is reset.hello <trigger> element also contains a <sources> element that contains one or more <source> elements.</span></span> <span data-ttu-id="d96d9-222">Hello <source> elemek hello URI toohello ad választ és ad válasz hello típusának megadása.</span><span class="sxs-lookup"><span data-stu-id="d96d9-222">hello <source> elements define hello URI toohello ad response and hello type of ad response.</span></span> <span data-ttu-id="d96d9-223">Ebben a példában egy URI tooa túlnyomó választ kap.</span><span class="sxs-lookup"><span data-stu-id="d96d9-223">In this example a URI is given tooa VAST response.</span></span> 

    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>


### <a name="using-video-player-ad-interface-definition-vpaid"></a><span data-ttu-id="d96d9-224">Videó Player Ad felületdefiníció (VPAID) használatával</span><span class="sxs-lookup"><span data-stu-id="d96d9-224">Using Video Player-Ad Interface Definition (VPAID)</span></span>
<span data-ttu-id="d96d9-225">VPAID az API-k engedélyezésének végrehajtható ad egységek toocommunicate videó lejátszóval.</span><span class="sxs-lookup"><span data-stu-id="d96d9-225">VPAID is an API for enabling executable ad units toocommunicate with a video player.</span></span> <span data-ttu-id="d96d9-226">Ez lehetővé teszi a magas interaktív ad lép.</span><span class="sxs-lookup"><span data-stu-id="d96d9-226">This allows highly interactive ad experiences.</span></span> <span data-ttu-id="d96d9-227">hello felhasználók beavatkozhatnak-hello ad-val, és hello ad válaszolhassanak tooactions hello megjelenítő venni.</span><span class="sxs-lookup"><span data-stu-id="d96d9-227">hello user can interact with hello ad and hello ad can respond tooactions taken by hello viewer.</span></span> <span data-ttu-id="d96d9-228">Például az ad gombok, amelyek lehetővé teszik a hello felhasználói tooview hello ad hosszabb verzióját vagy további információk megjelenítésére.</span><span class="sxs-lookup"><span data-stu-id="d96d9-228">For example an ad may display buttons that allow hello user tooview more information or a longer version of hello ad.</span></span> <span data-ttu-id="d96d9-229">hello videólejátszó támogatnia kell a hello VPAID API, és hello végrehajtható ad meg kell valósítania az hello API.</span><span class="sxs-lookup"><span data-stu-id="d96d9-229">hello video player must support hello VPAID API and hello executable ad must implement hello API.</span></span> <span data-ttu-id="d96d9-230">Ha egy player kéri le az Active server hello kiszolgáló az ad jelenhetnek meg, amely tartalmaz egy VPAID ad túlnyomó választ.</span><span class="sxs-lookup"><span data-stu-id="d96d9-230">When a player requests an ad from an ad server hello server may respond with a VAST response that contains a VPAID ad.</span></span>

<span data-ttu-id="d96d9-231">Egy végrehajtható ad Adobe Flash™ vagy a webböngészőben végrehajtható JavaScript futásidejű környezetben kell végrehajtani kód jön létre.</span><span class="sxs-lookup"><span data-stu-id="d96d9-231">An executable ad is created in code that must be executed in a runtime environment such as Adobe Flash™ or JavaScript that can be executed in a web browser.</span></span> <span data-ttu-id="d96d9-232">Ha egy ad-kiszolgáló egy VPAID ad tartalmazó túlnyomó választ ad vissza, hello hello hello apiFramework attribútumának értéke <MediaFile> elemnek kell lennie a "VPAID".</span><span class="sxs-lookup"><span data-stu-id="d96d9-232">When an ad server returns a VAST response containing a VPAID ad, hello value of hello apiFramework attribute in hello <MediaFile> element must be “VPAID”.</span></span> <span data-ttu-id="d96d9-233">Ez az attribútum határozza meg, hogy tartalmazott hello ad VPAID végrehajtható ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-233">This attribute specifies that hello contained ad is a VPAID executable ad.</span></span> <span data-ttu-id="d96d9-234">hello típusú attribútum értéke toohello MIME-típusát hello végrehajtható, például az "application/x-shockwave-flash" vagy "application/x-javascript".</span><span class="sxs-lookup"><span data-stu-id="d96d9-234">hello type attribute must be set toohello MIME type of hello executable, such as “application/x-shockwave-flash” or “application/x-javascript”.</span></span> <span data-ttu-id="d96d9-235">hello következő XML-részletet látható hello <MediaFile> VPAID végrehajtható ad tartalmazó túlnyomó választ elemét.</span><span class="sxs-lookup"><span data-stu-id="d96d9-235">hello following XML snippet shows hello <MediaFile> element from a VAST response containing a VPAID executable ad.</span></span> 

    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI tooexecutable ad -->
       </MediaFile>
    </MediaFiles>


<span data-ttu-id="d96d9-236">Egy végrehajtható ad hello inicializálhatók <AdParameters> hello elemet <Linear> vagy <NonLinear> elemek túlnyomó választ.</span><span class="sxs-lookup"><span data-stu-id="d96d9-236">An executable ad can be initialized using hello <AdParameters> element within hello <Linear> or <NonLinear> elements in a VAST response.</span></span> <span data-ttu-id="d96d9-237">További információ a hello <AdParameters> elem, lásd: [túlnyomó 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span><span class="sxs-lookup"><span data-stu-id="d96d9-237">For more information on hello <AdParameters> element, see [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span></span> <span data-ttu-id="d96d9-238">Hello VPAID API kapcsolatos további információkért lásd: [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).</span><span class="sxs-lookup"><span data-stu-id="d96d9-238">For more information about hello VPAID API, see [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).</span></span>

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a><span data-ttu-id="d96d9-239">A Windows vagy Windows Phone 8 Player Ad-támogatással rendelkező megvalósítása</span><span class="sxs-lookup"><span data-stu-id="d96d9-239">Implementing a Windows or Windows Phone 8 Player with Ad Support</span></span>
<span data-ttu-id="d96d9-240">Microsoft Media Platform hello: Player keretrendszer Windows 8 és Windows Phone 8-alkalmazásokra, amelyek bemutatják, hogyan videólejátszó alkalmazások használatával tooimplement hello keretrendszer gyűjteményét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d96d9-240">hello Microsoft Media Platform: Player Framework for Windows 8 and Windows Phone 8 contains a collection of sample applications that show you how tooimplement a video player application using hello framework.</span></span> <span data-ttu-id="d96d9-241">Letöltheti a hello Player keretrendszer és hello mintákat az [Player keretrendszer Windows 8 és Windows Phone 8](https://playerframework.codeplex.com).</span><span class="sxs-lookup"><span data-stu-id="d96d9-241">You can download hello Player Framework and hello samples from [Player Framework for Windows 8 and Windows Phone 8](https://playerframework.codeplex.com).</span></span>

<span data-ttu-id="d96d9-242">Hello Microsoft.PlayerFramework.Xaml.Samples megoldás megnyitásakor látni fogja a mappák hello projekten belül több.</span><span class="sxs-lookup"><span data-stu-id="d96d9-242">When you open hello Microsoft.PlayerFramework.Xaml.Samples solution you will see a number of folders within hello project.</span></span> <span data-ttu-id="d96d9-243">hello hirdetési mappa hello minta kód megfelelő toocreating egy ad-támogatással rendelkező videólejátszó tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d96d9-243">hello Advertising folder contains hello sample code relevant toocreating a video player with ad support.</span></span> <span data-ttu-id="d96d9-244">Belső hello hirdetési mappa egy XAML/cs fájljainak száma minden mely megjelenítése hogyan tooinsert ads eltérő módon.</span><span class="sxs-lookup"><span data-stu-id="d96d9-244">Inside hello Advertising folder is a number of XAML/cs files each of which show how tooinsert ads in a different way.</span></span> <span data-ttu-id="d96d9-245">a következő lista hello ismerteti:</span><span class="sxs-lookup"><span data-stu-id="d96d9-245">hello following list describes each:</span></span>

* <span data-ttu-id="d96d9-246">AdPodPage.xaml bemutatja, hogyan toodisplay ad pod.</span><span class="sxs-lookup"><span data-stu-id="d96d9-246">AdPodPage.xaml Shows how toodisplay an ad pod.</span></span>
* <span data-ttu-id="d96d9-247">AdSchedulingPage.xaml bemutatja hogyan tooschedule ads.</span><span class="sxs-lookup"><span data-stu-id="d96d9-247">AdSchedulingPage.xaml Shows how tooschedule ads.</span></span>
* <span data-ttu-id="d96d9-248">FreeWheelPage.xaml bemutatja, hogyan toouse hello FreeWheel beépülő modul tooschedule ads.</span><span class="sxs-lookup"><span data-stu-id="d96d9-248">FreeWheelPage.xaml Shows how toouse hello FreeWheel plugin tooschedule ads.</span></span>
* <span data-ttu-id="d96d9-249">MastPage.xaml bemutatja hogyan tooschedule ads OSZLOPOS fájllal.</span><span class="sxs-lookup"><span data-stu-id="d96d9-249">MastPage.xaml Shows how tooschedule ads with a MAST file.</span></span>
* <span data-ttu-id="d96d9-250">ProgrammaticAdPage.xaml bemutatja, hogyan tooprogrammatically ütemezni a videóban ads.</span><span class="sxs-lookup"><span data-stu-id="d96d9-250">ProgrammaticAdPage.xaml Shows how tooprogrammatically schedule ads into a video.</span></span>
* <span data-ttu-id="d96d9-251">ScheduleClipPage.xaml bemutatja hogyan tooschedule egy ad túlnyomó fájl nélkül.</span><span class="sxs-lookup"><span data-stu-id="d96d9-251">ScheduleClipPage.xaml Shows how tooschedule an ad without a VAST file.</span></span>
* <span data-ttu-id="d96d9-252">VastLinearCompanionPage.xaml bemutatja hogyan egy lineáris tooinsert és kiegészítő ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-252">VastLinearCompanionPage.xaml Shows how tooinsert a linear and companion ad.</span></span>
* <span data-ttu-id="d96d9-253">VastNonLinearPage.xaml bemutatja hogyan tooinsert nem lineáris ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-253">VastNonLinearPage.xaml Shows how tooinsert a non-linear ad.</span></span>
* <span data-ttu-id="d96d9-254">VmapPage.xaml bemutatja hogyan toospecify ads VMAP fájllal.</span><span class="sxs-lookup"><span data-stu-id="d96d9-254">VmapPage.xaml Shows how toospecify ads with a VMAP file.</span></span>

<span data-ttu-id="d96d9-255">Ezeket a mintákat mindegyikének használ hello Media Player hello player keretrendszer határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="d96d9-255">Each of these samples uses hello MediaPlayer class defined by hello player framework.</span></span> <span data-ttu-id="d96d9-256">A legtöbb minták beépülő modulok, amelyek különböző ad választ formátumban támogatásához használja.</span><span class="sxs-lookup"><span data-stu-id="d96d9-256">Most samples use plugins that add support for various ad response formats.</span></span> <span data-ttu-id="d96d9-257">hello ProgrammaticAdPage minta programokon keresztül kommunikál a Media Player példánya.</span><span class="sxs-lookup"><span data-stu-id="d96d9-257">hello ProgrammaticAdPage sample programmatically interacts with a MediaPlayer instance.</span></span>

### <a name="adpodpage-sample"></a><span data-ttu-id="d96d9-258">AdPodPage minta</span><span class="sxs-lookup"><span data-stu-id="d96d9-258">AdPodPage Sample</span></span>
<span data-ttu-id="d96d9-259">Ezt a mintát használ hello AdSchedulerPlugin toodefine amikor toodisplay ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-259">This sample uses hello AdSchedulerPlugin toodefine when toodisplay an ad.</span></span> <span data-ttu-id="d96d9-260">Ebben a példában egy közepes összegző hirdetmény ütemezett toobe lejátszott 5 másodpercen belül.</span><span class="sxs-lookup"><span data-stu-id="d96d9-260">In this example a mid-roll advertisement is scheduled toobe played after 5 seconds.</span></span> <span data-ttu-id="d96d9-261">hello ad pod (ads toodisplay sorrendben csoportja) egy ad-kiszolgáló által visszaadott túlnyomó fájlban van megadva.</span><span class="sxs-lookup"><span data-stu-id="d96d9-261">hello ad pod (a group of ads toodisplay in order) is specified in a VAST file returned from an ad server.</span></span> <span data-ttu-id="d96d9-262">hello megadva hello URI toohello túlnyomó fájl <RemoteAdSource> elemet.</span><span class="sxs-lookup"><span data-stu-id="d96d9-262">hello URI toohello VAST file is specified in hello <RemoteAdSource> element.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">

        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>

                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>

                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

<span data-ttu-id="d96d9-263">Hello AdSchedulerPlugin kapcsolatos további információkért lásd: [reklám a hello Player keretrendszer a Windows 8 és Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)</span><span class="sxs-lookup"><span data-stu-id="d96d9-263">For more information about hello AdSchedulerPlugin, see [Advertising in hello Player Framework on Windows 8 and Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)</span></span>

### <a name="adschedulingpage"></a><span data-ttu-id="d96d9-264">AdSchedulingPage</span><span class="sxs-lookup"><span data-stu-id="d96d9-264">AdSchedulingPage</span></span>
<span data-ttu-id="d96d9-265">Ez a minta hello AdSchedulerPlugin is használ.</span><span class="sxs-lookup"><span data-stu-id="d96d9-265">This sample also uses hello AdSchedulerPlugin.</span></span> <span data-ttu-id="d96d9-266">Három ads, egy előtti összegző ad, egy közepes összegző ad és a utáni összegző ad ütemezés.</span><span class="sxs-lookup"><span data-stu-id="d96d9-266">It schedules three ads, a pre-roll ad, a mid-roll ad, and a post-roll ad.</span></span> <span data-ttu-id="d96d9-267">hello URI toohello VAST, az egyes ad van megadva egy <RemoteAdSource> elemet.</span><span class="sxs-lookup"><span data-stu-id="d96d9-267">hello URI toohello VAST for each ad is specified in a <RemoteAdSource> element.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>

                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


### <a name="freewheelpage"></a><span data-ttu-id="d96d9-268">FreeWheelPage</span><span class="sxs-lookup"><span data-stu-id="d96d9-268">FreeWheelPage</span></span>
<span data-ttu-id="d96d9-269">Ez a minta hello FreeWheelPlugin, amely meghatározza a forrásattribútumot, amely meghatározza a pontok tooa SmartXML fájlt ad tartalmat, valamint ad ütemezési adatait megadó URI használja.</span><span class="sxs-lookup"><span data-stu-id="d96d9-269">This sample uses hello FreeWheelPlugin which specifies a Source attribute that specifies a URI that points tooa SmartXML file that specifies ad content as well as ad scheduling information.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="mastpage"></a><span data-ttu-id="d96d9-270">MastPage</span><span class="sxs-lookup"><span data-stu-id="d96d9-270">MastPage</span></span>
<span data-ttu-id="d96d9-271">Ezt a mintát használja, amely lehetővé teszi egy OSZLOPOS fájl toouse MastSchedulerPlugin hello.</span><span class="sxs-lookup"><span data-stu-id="d96d9-271">This sample uses hello MastSchedulerPlugin that allows you toouse a MAST file.</span></span> <span data-ttu-id="d96d9-272">hello forrásattribútum hello OSZLOPOS fájl hello helyét adja meg.</span><span class="sxs-lookup"><span data-stu-id="d96d9-272">hello Source attribute specifies hello location of hello MAST file.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="programmaticadpage"></a><span data-ttu-id="d96d9-273">ProgrammaticAdPage</span><span class="sxs-lookup"><span data-stu-id="d96d9-273">ProgrammaticAdPage</span></span>
<span data-ttu-id="d96d9-274">Ez a minta hello MediaPlayer programokon keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="d96d9-274">This sample programmatically interacts with hello MediaPlayer.</span></span> <span data-ttu-id="d96d9-275">hello ProgrammaticAdPage.xaml fájl hello Media Player példányosítja:</span><span class="sxs-lookup"><span data-stu-id="d96d9-275">hello ProgrammaticAdPage.xaml file instantiates hello MediaPlayer:</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

<span data-ttu-id="d96d9-276">hello ProgrammaticAdPage.xaml.cs fájl veszi fel egy TimelineMarker toospecify, amikor az ad üzenetnek kell megjelennie, majd a hello MarkerReached esemény, amely betölti a RemoteAdSource URI tooa túlnyomó fájl megadása, és majd játszik kezelőtársítást ad hozzá, létrehoz egy AdHandlerPlugin hello ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-276">hello ProgrammaticAdPage.xaml.cs file creates an AdHandlerPlugin, adds a TimelineMarker toospecify when an ad should be displayed, and then adds a handler for hello MarkerReached event which loads a RemoteAdSource specifying a URI tooa VAST file, and then plays hello ad.</span></span>

    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;

            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }

            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

### <a name="scheduleclippage"></a><span data-ttu-id="d96d9-277">ScheduleClipPage</span><span class="sxs-lookup"><span data-stu-id="d96d9-277">ScheduleClipPage</span></span>
<span data-ttu-id="d96d9-278">Ez a minta egy .wmv-fájlt, amely tartalmazza a hello ad megadásával hello AdSchedulerPlugin tooschedule egy közepes összegző ad használja.</span><span class="sxs-lookup"><span data-stu-id="d96d9-278">This sample uses hello AdSchedulerPlugin tooschedule a mid-roll ad by specifying a .wmv file that contains hello ad.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vastlinearcompanionpage"></a><span data-ttu-id="d96d9-279">VastLinearCompanionPage</span><span class="sxs-lookup"><span data-stu-id="d96d9-279">VastLinearCompanionPage</span></span>
<span data-ttu-id="d96d9-280">Ez a minta bemutatja, hogyan toouse hello AdSchedulerPlugin tooschedule egy közepes összegző lineáris ad egy kiegészítő ad-val.</span><span class="sxs-lookup"><span data-stu-id="d96d9-280">This sample illustrates how toouse hello AdSchedulerPlugin tooschedule a mid-roll linear ad with an companion ad.</span></span> <span data-ttu-id="d96d9-281">Hello <RemoteAdSource> elem hello túlnyomó fájl hello helyét adja meg.</span><span class="sxs-lookup"><span data-stu-id="d96d9-281">hello <RemoteAdSource> element specifies hello location of hello VAST file.</span></span>

    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vastlinearnonlinearpage"></a><span data-ttu-id="d96d9-282">VastLinearNonLinearPage</span><span class="sxs-lookup"><span data-stu-id="d96d9-282">VastLinearNonLinearPage</span></span>
<span data-ttu-id="d96d9-283">A példa egy lineáris és egy nem lineáris ad a hello AdSchedulerPlugin tooschedule használja.</span><span class="sxs-lookup"><span data-stu-id="d96d9-283">This sample uses hello AdSchedulerPlugin tooschedule a linear and a non-linear ad.</span></span> <span data-ttu-id="d96d9-284">hello túlnyomó fájl helye meg van adva hello <RemoteAdSource> elemet.</span><span class="sxs-lookup"><span data-stu-id="d96d9-284">hello VAST file location is specified with hello <RemoteAdSource> element.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vmappage"></a><span data-ttu-id="d96d9-285">VMAPPage</span><span class="sxs-lookup"><span data-stu-id="d96d9-285">VMAPPage</span></span>
<span data-ttu-id="d96d9-286">Ezt a mintát használ hello VmapSchedulerPlugin tooschedule ads VMAP fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="d96d9-286">This samples uses hello VmapSchedulerPlugin tooschedule ads using a VMAP file.</span></span> <span data-ttu-id="d96d9-287">hello URI toohello VMAP fájl van megadva a hello forrásattribútumának hello <VmapSchedulerPlugin> elemet.</span><span class="sxs-lookup"><span data-stu-id="d96d9-287">hello URI toohello VMAP file is specified in hello Source attribute of hello <VmapSchedulerPlugin> element.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

## <a name="implementing-an-ios-video-player-with-ad-support"></a><span data-ttu-id="d96d9-288">IOS rendszerű Ad-támogatással rendelkező videó Player megvalósítása</span><span class="sxs-lookup"><span data-stu-id="d96d9-288">Implementing an iOS Video Player with Ad Support</span></span>
<span data-ttu-id="d96d9-289">Microsoft Media Platform hello: Player keretrendszer az iOS-alkalmazásokra, amelyek bemutatják, hogyan videólejátszó alkalmazások használatával tooimplement hello keretrendszer gyűjteményét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d96d9-289">hello Microsoft Media Platform: Player Framework for iOS contains a collection of sample applications that show you how tooimplement a video player application using hello framework.</span></span> <span data-ttu-id="d96d9-290">Letöltheti a hello Player keretrendszer és hello mintákat az [Azure Media Player keretrendszer](https://github.com/Azure/azure-media-player-framework).</span><span class="sxs-lookup"><span data-stu-id="d96d9-290">You can download hello Player Framework and hello samples from [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework).</span></span> <span data-ttu-id="d96d9-291">hello github-oldalon tartalmaz egy hivatkozást tooa hello player keretrendszer és a bevezetés toohello player minta kiegészítő tudnivalókat tartalmazó Wiki: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).</span><span class="sxs-lookup"><span data-stu-id="d96d9-291">hello github page has a link tooa Wiki that contains additional information on hello player framework and an introduction toohello player sample: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).</span></span>

### <a name="scheduling-ads-with-vmap"></a><span data-ttu-id="d96d9-292">VMAP hirdetések ütemezése</span><span class="sxs-lookup"><span data-stu-id="d96d9-292">Scheduling Ads with VMAP</span></span>
<span data-ttu-id="d96d9-293">a következő példa azt mutatja meg hogyan hello tooschedule ads VMAP fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="d96d9-293">hello following example shows how tooschedule ads using a VMAP file.</span></span>

    // How tooschedule an Ad using VMAP.
    //First download hello VMAP manifest

    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using hello downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

### <a name="scheduling-ads-with-vast"></a><span data-ttu-id="d96d9-294">VAST hirdetések ütemezése</span><span class="sxs-lookup"><span data-stu-id="d96d9-294">Scheduling Ads with VAST</span></span>
<span data-ttu-id="d96d9-295">hello alábbi példa azt mutatja be hogyan tooschedule egy késői kötés túlnyomó ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-295">hello following sample shows how tooschedule a late binding VAST ad.</span></span>

    //Example:3 How tooschedule a late binding VAST ad.
    // set hello start time for hello ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify hello URI of hello VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL tooVAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

   <span data-ttu-id="d96d9-296">hello alábbi példa azt mutatja be hogyan tooschedule egy korai kötés túlnyomó ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-296">hello following sample shows how tooschedule an early binding VAST ad.</span></span>
<span data-ttu-id="d96d9-297">Példa: 4 ütemezés egy korai kötés túlnyomó ad //Download hello VAST fájlt, ha (! [ framework.adResolver downloadManifest: & jegyzék withURL: [által igényelt NSURL URLWithString: @"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[self logFrameworkError];} else {adLinearTime.startTime = 7; adLinearTime.duration = 0;</span><span class="sxs-lookup"><span data-stu-id="d96d9-297">//Example:4 Schedule an early binding VAST ad //Download hello VAST file if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) { [self logFrameworkError]; } else { adLinearTime.startTime = 7; adLinearTime.duration = 0;</span></span>

        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

<span data-ttu-id="d96d9-298">hello alábbi példa azt mutatja be hogyan tooinsert egy ad nyers Kivágás szerkesztése (RCE) használatával</span><span class="sxs-lookup"><span data-stu-id="d96d9-298">hello following sample shows how tooinsert an ad using Rough Cut Editing (RCE)</span></span>

    //Example:1 How toouse RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

<span data-ttu-id="d96d9-299">hello következő példa bemutatja, hogyan tooschedule ad pod.</span><span class="sxs-lookup"><span data-stu-id="d96d9-299">hello following example shows how tooschedule an ad pod.</span></span>

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;

    // Specify URL toocontent
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI tooad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

<span data-ttu-id="d96d9-300">a következő példa azt mutatja meg hogyan hello tooschedule a nem kapcsolódó közepes összegző ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-300">hello following example shows how tooschedule a non-sticky mid-roll ad.</span></span> <span data-ttu-id="d96d9-301">A nem kapcsolódó ad csak lejátszása után függetlenül bármilyen pozicionálási hello viewer hajt végre.</span><span class="sxs-lookup"><span data-stu-id="d96d9-301">A non-sticky ad is only played once regardless of any seeking hello viewer performs.</span></span>

    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL toocontent
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

<span data-ttu-id="d96d9-302">a következő példa azt mutatja meg hogyan hello tooschedule egy állandóságát közepes összegző ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-302">hello following example shows how tooschedule a sticky mid-roll ad.</span></span> <span data-ttu-id="d96d9-303">A kapcsolódó ad fog megjelenni minden alkalommal, amikor hello megadott hello videó idősoron pont elérésekor.</span><span class="sxs-lookup"><span data-stu-id="d96d9-303">A sticky ad will be displayed each time hello specified point on hello video timeline is reached.</span></span>

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI tooad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


<span data-ttu-id="d96d9-304">hello alábbi példa azt mutatja be hogyan tooschedule egy utáni összegző ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-304">hello following sample shows how tooschedule a post-roll ad.</span></span>

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

<span data-ttu-id="d96d9-305">hello alábbi példa azt mutatja be hogyan tooschedule egy előtti összegző ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-305">hello following sample shows how tooschedule a pre-roll ad.</span></span>

    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

<span data-ttu-id="d96d9-306">a következő minta hello bemutatja, hogyan tooschedule közepes összegző átfedő ad.</span><span class="sxs-lookup"><span data-stu-id="d96d9-306">hello following sample shows how tooschedule a mid-roll overlay ad.</span></span>

    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="d96d9-307">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="d96d9-307">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d96d9-308">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="d96d9-308">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="d96d9-309">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d96d9-309">See Also</span></span>
[<span data-ttu-id="d96d9-310">Videolejátszó alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="d96d9-310">Develop video player applications</span></span>](media-services-develop-video-players.md)

