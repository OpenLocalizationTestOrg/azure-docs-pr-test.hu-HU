---
title: "Ügyféloldali ads beszúrása |} Microsoft Docs"
description: "Ez a témakör azt ismerteti, hogyan ügyféloldali ads beszúrása."
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
ms.openlocfilehash: 52ba731f88c630830560e3cf8406ba2e9613c8a5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="inserting-ads-on-the-client-side"></a><span data-ttu-id="bf272-103">Ügyféloldali ads beszúrása</span><span class="sxs-lookup"><span data-stu-id="bf272-103">Inserting ads on the client side</span></span>
<span data-ttu-id="bf272-104">Ez a témakör az ügyféloldalon ads különböző típusú beszúrása információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bf272-104">This topic contains information on how to insert various types of ads on the client side.</span></span>

<span data-ttu-id="bf272-105">Az élő adatfolyam-továbbítási videók lezárt feliratok és az ad támogatásával kapcsolatos további információkért lásd: [támogatott kódolt feliratok és Ad beszúrási szabványok](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).</span><span class="sxs-lookup"><span data-stu-id="bf272-105">For information about closed captioning and ad support in Live streaming videos, see [Supported Closed Captioning and Ad Insertion Standards](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).</span></span>

> [!NOTE]
> <span data-ttu-id="bf272-106">Az Azure Media Player jelenleg nem támogatja a hirdetések.</span><span class="sxs-lookup"><span data-stu-id="bf272-106">Azure Media Player does not currently support Ads.</span></span>
> 
> 

## <span data-ttu-id="bf272-107"><a id="insert_ads_into_media"></a>A Media Ads beszúrása</span><span class="sxs-lookup"><span data-stu-id="bf272-107"><a id="insert_ads_into_media"></a>Inserting Ads into your Media</span></span>
<span data-ttu-id="bf272-108">Az Azure Media Services támogatást nyújt a Windows Media platformon keresztül ad beszúrási: lejátszó-Keretrendszerekhez.</span><span class="sxs-lookup"><span data-stu-id="bf272-108">Azure Media Services provides support for ad insertion through the Windows Media Platform: Player Frameworks.</span></span> <span data-ttu-id="bf272-109">Ad-támogatással rendelkező lejátszó-keretrendszerekhez Windows 8, a Silverlight, a Windows Phone 8 és az iOS-eszközök érhetők el.</span><span class="sxs-lookup"><span data-stu-id="bf272-109">Player frameworks with ad support are available for Windows 8, Silverlight, Windows Phone 8, and iOS devices.</span></span> <span data-ttu-id="bf272-110">Minden egyes player keretrendszer mintakód bemutatja, hogy a lejátszóalkalmazás megvalósításához tartalmazza. Nincsenek media: listájába szúrhatók ads három különböző típusú.</span><span class="sxs-lookup"><span data-stu-id="bf272-110">Each player framework contains sample code that shows you how to implement a player application.There are three different kinds of ads you can insert into your media:list.</span></span>

* <span data-ttu-id="bf272-111">**Lineáris** – teljes keret hirdetések felfüggeszti a fő videó.</span><span class="sxs-lookup"><span data-stu-id="bf272-111">**Linear** – full frame ads that pause the main video.</span></span>
* <span data-ttu-id="bf272-112">**Nem lineáris** – jelennek meg a fő videó lejátszása hirdetések átmeneti területre, általában embléma vagy egyéb statikus kép kerül a Windows Media player belül.</span><span class="sxs-lookup"><span data-stu-id="bf272-112">**Nonlinear** – overlay ads that are displayed as the main video is playing, usually a logo or other static image placed within the player.</span></span>
* <span data-ttu-id="bf272-113">**Kiegészítő** – kívül a Windows Media player megjelenített ads.</span><span class="sxs-lookup"><span data-stu-id="bf272-113">**Companion** – ads that are displayed outside of the player.</span></span>

<span data-ttu-id="bf272-114">A fő videó idősorán bármikor ADs helyezhető.</span><span class="sxs-lookup"><span data-stu-id="bf272-114">Ads can be placed at any point in the main video’s time line.</span></span> <span data-ttu-id="bf272-115">A Windows Media player kell arról, mikor számára, hogy az ad és számára, hogy mely hirdetések.</span><span class="sxs-lookup"><span data-stu-id="bf272-115">You must tell the player when to play the ad and which ads to play.</span></span> <span data-ttu-id="bf272-116">Ebben az esetben a szabványos XML alapú fájlok készletből: videó Ad szolgáltatás sablon (VAST), a digitális videót több Ad lista (VMAP), a Media absztrakt alkalmazás-előkészítés sablon (OSZLOPOS) és a digitális videót Player Ad felület Definition (VPAID).</span><span class="sxs-lookup"><span data-stu-id="bf272-116">This is done using a set of standard XML-based files: Video Ad Service Template (VAST), Digital Video Multiple Ad Playlist (VMAP), Media Abstract Sequencing Template (MAST), and Digital Video Player Ad Interface Definition (VPAID).</span></span> <span data-ttu-id="bf272-117">NAGY fájlok adja meg, milyen ads megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="bf272-117">VAST files specify what ads to display.</span></span> <span data-ttu-id="bf272-118">VMAP fájlok idejére különböző ads lejátszásához és HATALMAS XML kódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bf272-118">VMAP files specify when to play various ads and contain VAST XML.</span></span> <span data-ttu-id="bf272-119">A fájlok OSZLOPOS feladatütemezési ads is tartalmazó túlnyomó XML másik módja van.</span><span class="sxs-lookup"><span data-stu-id="bf272-119">MAST files are another way to sequence ads which also can contain VAST XML.</span></span> <span data-ttu-id="bf272-120">VPAID fájlok határozza meg azt a videólejátszó és az ad vagy ad-kiszolgáló közötti illesztőfelületet szolgáltasson.</span><span class="sxs-lookup"><span data-stu-id="bf272-120">VPAID files define an interface between the video player and the ad or ad server.</span></span>

<span data-ttu-id="bf272-121">Minden egyes player keretrendszer eltérő módon működik, és minden egyes saját témakör tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="bf272-121">Each player framework works differently and each will be covered in its own topic.</span></span> <span data-ttu-id="bf272-122">Ez a témakör ismerteti az alapvető módszerek segítségével szúrják be az ads. Videólejátszó alkalmazások ads kérhet egy ad-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="bf272-122">This topic will describe the basic mechanisms used to insert ads.Video player applications request ads from an ad server.</span></span> <span data-ttu-id="bf272-123">Az ad-kiszolgáló többféle módon válaszolhat:</span><span class="sxs-lookup"><span data-stu-id="bf272-123">The ad server can respond in a number of ways:</span></span>

* <span data-ttu-id="bf272-124">Térjen vissza a túlnyomó fájl</span><span class="sxs-lookup"><span data-stu-id="bf272-124">Return a VAST file</span></span>
* <span data-ttu-id="bf272-125">Térjen vissza a VMAP fájlt (a beágyazott VAST)</span><span class="sxs-lookup"><span data-stu-id="bf272-125">Return a VMAP file (with embedded VAST)</span></span>
* <span data-ttu-id="bf272-126">Egy OSZLOPOS fájlt (a beágyazott VAST) visszaadása</span><span class="sxs-lookup"><span data-stu-id="bf272-126">Return a MAST file (with embedded VAST)</span></span>
* <span data-ttu-id="bf272-127">Térjen vissza a VPAID ads túlnyomó fájl</span><span class="sxs-lookup"><span data-stu-id="bf272-127">Return a VAST file with VPAID ads</span></span>

### <a name="using-a-video-ad-service-template-vast-file"></a><span data-ttu-id="bf272-128">Videó Ad szolgáltatás sablon (VAST) fájl használatával</span><span class="sxs-lookup"><span data-stu-id="bf272-128">Using a Video Ad Service Template (VAST) File</span></span>
<span data-ttu-id="bf272-129">Egy túlnyomó fájlt határozza meg, milyen ad vagy ads megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="bf272-129">A VAST file specifies what ad or ads to display.</span></span> <span data-ttu-id="bf272-130">A következő XML-kódja egy lineáris ad túlnyomó fájl például:</span><span class="sxs-lookup"><span data-stu-id="bf272-130">The following XML is an example of a VAST file for a linear ad:</span></span>

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

<span data-ttu-id="bf272-131">A lineáris ad le a <**lineáris**> elemet.</span><span class="sxs-lookup"><span data-stu-id="bf272-131">The linear ad is described by the <**Linear**> element.</span></span> <span data-ttu-id="bf272-132">Azt adja meg az ad időtartama, nyomon követés, átkattintással, követési kattintson, és számos **MediaFile** elemek.</span><span class="sxs-lookup"><span data-stu-id="bf272-132">It specifies the duration of the ad, tracking events, click through, click tracking, and a number of **MediaFile** elements.</span></span> <span data-ttu-id="bf272-133">Nyomkövetési események belül vannak megadva a <**TrackingEvents**> elemet, és teszi lehetővé egy ad-kiszolgáló az ad megtekintése közben előforduló különféle események nyomon követéséhez.</span><span class="sxs-lookup"><span data-stu-id="bf272-133">Tracking events are specified within the <**TrackingEvents**> element and allow an ad server to track various events that occur while viewing the ad.</span></span> <span data-ttu-id="bf272-134">Ebben az esetben a kezdő középpont, befejeződött, és bontsa ki a események nyomon követi.</span><span class="sxs-lookup"><span data-stu-id="bf272-134">In this case the start, midpoint, complete, and expand events are tracked.</span></span> <span data-ttu-id="bf272-135">Az ad jelenik meg a kezdő esemény következik be.</span><span class="sxs-lookup"><span data-stu-id="bf272-135">The start event occurs when the ad is displayed.</span></span> <span data-ttu-id="bf272-136">A középpont esemény következik be, legalább 50 %-a az ad idősor már megtekintett.</span><span class="sxs-lookup"><span data-stu-id="bf272-136">The midpoint event occurs when at least 50% of the ad’s timeline has been viewed.</span></span> <span data-ttu-id="bf272-137">Az ad a befejezési futott a teljes esemény következik be.</span><span class="sxs-lookup"><span data-stu-id="bf272-137">The complete event occurs when the ad has run to the end.</span></span> <span data-ttu-id="bf272-138">A kibontott esemény következik be, amikor a felhasználó a videólejátszó bővíti a teljes képernyős.</span><span class="sxs-lookup"><span data-stu-id="bf272-138">The Expand event occurs when the user expands the video player to full screen.</span></span> <span data-ttu-id="bf272-139">Clickthroughs vannak megadva, a <**Átkattintós**> elemen belül egy <**VideoClicks**> elemet, és adja meg egy erőforrás URI-t jelenítsen meg, ha a felhasználó kattint az ad.</span><span class="sxs-lookup"><span data-stu-id="bf272-139">Clickthroughs are specified with a <**ClickThrough**> element within a <**VideoClicks**> element and specifies a URI to a resource to display when the user clicks on the ad.</span></span> <span data-ttu-id="bf272-140">ClickTracking van megadva egy <**ClickTracking**> elem, belül is a <**VideoClicks**> elemet, és adja meg a Windows Media Player kérése, amikor a felhasználó kattint az ad követési erőforrás . A <**MediaFile**> elemek adjon meg egy adott kódolását, az ad információt.</span><span class="sxs-lookup"><span data-stu-id="bf272-140">ClickTracking is specified in a <**ClickTracking**> element, also within the <**VideoClicks**> element and specifies a tracking resource for the player to request when the user clicks on the ad.The <**MediaFile**> elements specify information about a specific encoding of an ad.</span></span> <span data-ttu-id="bf272-141">Ha egynél több <**MediaFile**> elem, a videólejátszó kiválaszthatja a platform legjobb kódolást.</span><span class="sxs-lookup"><span data-stu-id="bf272-141">When there is more than one <**MediaFile**> element, the video player can choose the best encoding for the platform.</span></span> 

<span data-ttu-id="bf272-142">Lineáris ads megjeleníthető a megadott sorrendben.</span><span class="sxs-lookup"><span data-stu-id="bf272-142">Linear ads can be displayed in a specified order.</span></span> <span data-ttu-id="bf272-143">Ehhez adja hozzá további <Ad> a VAST elemek fájlt, majd adja meg a feladatütemezési attribútum használatával.</span><span class="sxs-lookup"><span data-stu-id="bf272-143">To do this, add additional <Ad> elements to the VAST file and specify the order using the sequence attribute.</span></span> <span data-ttu-id="bf272-144">A következő példa ezt mutatja be:</span><span class="sxs-lookup"><span data-stu-id="bf272-144">The following example illustrates this:</span></span>

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

<span data-ttu-id="bf272-145">Nem lineáris ads megadott egy <Creative> elemet is.</span><span class="sxs-lookup"><span data-stu-id="bf272-145">Nonlinear ads are specified in a <Creative> element as well.</span></span> <span data-ttu-id="bf272-146">Az alábbi példa mutatja egy <Creative> elem az lineáris ad.</span><span class="sxs-lookup"><span data-stu-id="bf272-146">The following example shows a <Creative> element that describes a nonlinear ad.</span></span>

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


<span data-ttu-id="bf272-147">A <**NonLinearAds**> elem tartalmazhat egy vagy több <**NonLinear**> elemek, amelyek leírhatja lineáris ad.</span><span class="sxs-lookup"><span data-stu-id="bf272-147">The <**NonLinearAds**> element can contain one or more <**NonLinear**> elements, each of which can describe a nonlinear ad.</span></span> <span data-ttu-id="bf272-148">A <**NonLinear**> elem meghatározza a lineáris ad erőforrás.</span><span class="sxs-lookup"><span data-stu-id="bf272-148">The <**NonLinear**> element specifies the resource for the nonlinear ad.</span></span> <span data-ttu-id="bf272-149">Az erőforrás lehet egy <**StaticResouce**>, <**IFrameResource**>, vagy egy <**HTMLResouce**>.</span><span class="sxs-lookup"><span data-stu-id="bf272-149">The resource can be a <**StaticResouce**>, an <**IFrameResource**>, or an <**HTMLResouce**>.</span></span><span data-ttu-id="bf272-150"> <**StaticResource**> nem HTML erőforrás ismerteti, és arról, hogyan jelenjen meg az erőforrás egy creativeType attribútum határozza meg:</span><span class="sxs-lookup"><span data-stu-id="bf272-150"> <**StaticResource**> describes a non-HTML resource and defines a creativeType attribute that specifies how the resource is displayed:</span></span>

<span data-ttu-id="bf272-151">Kép/gif, a kép/jpeg, a lemezkép vagy png – HTML jelenik meg az erőforrás <**img**> címke.</span><span class="sxs-lookup"><span data-stu-id="bf272-151">Image/gif, image/jpeg, image/png – the resource is displayed in an HTML <**img**> tag.</span></span>

<span data-ttu-id="bf272-152">Alkalmazás/x-javascript – HTML jelenik meg az erőforrás <**parancsfájl**> címke.</span><span class="sxs-lookup"><span data-stu-id="bf272-152">Application/x-javascript – the resource is displayed in an HTML <**script**> tag.</span></span>

<span data-ttu-id="bf272-153">Alkalmazás/x-shockwave-flash – az erőforrás egy Flash player jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bf272-153">Application/x-shockwave-flash – the resource is displayed in a Flash player.</span></span>

<span data-ttu-id="bf272-154">**IFrameResource** egy HTML-erőforrás IFRAME megjeleníthető ismerteti.</span><span class="sxs-lookup"><span data-stu-id="bf272-154">**IFrameResource** describes an HTML resource that can be displayed in an IFrame.</span></span> <span data-ttu-id="bf272-155">**HTMLResource** szúrhatók be egy weblap, HTML-kódja egy adat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="bf272-155">**HTMLResource** describes a piece of HTML code that can be inserted into a web page.</span></span> <span data-ttu-id="bf272-156">**TrackingEvents** adja meg a nyomkövetési események és az URI-t kérése az esemény akkor következik be.</span><span class="sxs-lookup"><span data-stu-id="bf272-156">**TrackingEvents** specify tracking events and the URI to request when the event occurs.</span></span> <span data-ttu-id="bf272-157">Ez a példa a acceptInvitation és összecsukása események követi.</span><span class="sxs-lookup"><span data-stu-id="bf272-157">In this sample the acceptInvitation and collapse events are tracked.</span></span> <span data-ttu-id="bf272-158">További információ a **NonLinearAds** elem és a gyermekek, lásd: IAB.NET/VAST.</span><span class="sxs-lookup"><span data-stu-id="bf272-158">For more information on the **NonLinearAds** element and its children, see IAB.NET/VAST.</span></span> <span data-ttu-id="bf272-159">Vegye figyelembe, hogy a **TrackingEvents** elem a helyen belüli a **NonLinearAds** elem helyett a **NonLinear** elemet.</span><span class="sxs-lookup"><span data-stu-id="bf272-159">Note that the **TrackingEvents** element is located within the **NonLinearAds** element rather than the **NonLinear** element.</span></span>

<span data-ttu-id="bf272-160">Kiegészítő ads meghatározott egy <CompanionAds> elemet.</span><span class="sxs-lookup"><span data-stu-id="bf272-160">Companion ads are defined within a <CompanionAds> element.</span></span> <span data-ttu-id="bf272-161">A <CompanionAds> elem tartalmazhat egy vagy több <Companion> elemek.</span><span class="sxs-lookup"><span data-stu-id="bf272-161">The <CompanionAds> element can contain one or more <Companion> elements.</span></span> <span data-ttu-id="bf272-162">Minden egyes <Companion> elem egy kiegészítő ad ismerteti, és tartalmazhat egy <StaticResource>, <IFrameResource>, vagy <HTMLResource> amely lineáris ad a megszokott módon vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="bf272-162">Each <Companion> element describes a companion ad and can contain a <StaticResource>, <IFrameResource>, or <HTMLResource> which are specified in the same way as in a nonlinear ad.</span></span> <span data-ttu-id="bf272-163">A túlnyomó is tartalmazhatnak, több kiegészítő ads, és a lejátszóalkalmazás kiválaszthatja a megjelenítendő legmegfelelőbb ad.</span><span class="sxs-lookup"><span data-stu-id="bf272-163">A VAST file can contain multiple companion ads and the player application can choose the most appropriate ad to display.</span></span> <span data-ttu-id="bf272-164">VAST kapcsolatos további információkért lásd: [túlnyomó 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span><span class="sxs-lookup"><span data-stu-id="bf272-164">For more information about VAST, see [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span></span>

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a><span data-ttu-id="bf272-165">Egy digitális videót több Ad-lista (VMAP) fájl használatával</span><span class="sxs-lookup"><span data-stu-id="bf272-165">Using a Digital Video Multiple Ad Playlist (VMAP) File</span></span>
<span data-ttu-id="bf272-166">Egy VMAP fájl lehetővé teszi annak megadását, amikor ad oldaltörések fordulhat elő, mennyi ideig egyes szünetek, hány ads belül szünet jeleníthető meg, és ads típusú lehet megszakítás alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bf272-166">A VMAP file allows you to specify when ad breaks occur, how long each break is, how many ads can be displayed within a break, and what types of ads may be displayed during a break.</span></span> <span data-ttu-id="bf272-167">A következő egy példa VMAP fájl, amely meghatározza egy egyetlen ad break:</span><span class="sxs-lookup"><span data-stu-id="bf272-167">The following in an example VMAP file that defines a single ad break:</span></span>

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

<span data-ttu-id="bf272-168">Egy VMAP fájl kezdődik egy <VMAP> elem, amely tartalmazza egy vagy több <AdBreak> elemek, egy ad break meghatározása.</span><span class="sxs-lookup"><span data-stu-id="bf272-168">A VMAP file begins with a <VMAP> element that contains one or more <AdBreak> elements, each defining an ad break.</span></span> <span data-ttu-id="bf272-169">Minden ad break határozza meg, egy break típusát, a sortörés azonosítója és a idő eltolódását.</span><span class="sxs-lookup"><span data-stu-id="bf272-169">Each ad break specifies a break type, break ID, and time offset.</span></span> <span data-ttu-id="bf272-170">A breakType attribútum határozza meg a során a szünet lejátszható ad: lineáris, nem lineáris, vagy megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="bf272-170">The breakType attribute specifies the type of ad that can be played during the break: linear, nonlinear, or display.</span></span> <span data-ttu-id="bf272-171">Ads térkép megjelenítése túlnyomó kiegészítő ads.</span><span class="sxs-lookup"><span data-stu-id="bf272-171">Display ads map to VAST companion ads.</span></span> <span data-ttu-id="bf272-172">Egynél több ad-típus (szóközök nélkül) vesszővel elválasztott listában adható meg.</span><span class="sxs-lookup"><span data-stu-id="bf272-172">More than one ad type can be specified in a comma (no spaces) separated list.</span></span> <span data-ttu-id="bf272-173">A breakID pedig az ad azonosítója, amelyet a nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="bf272-173">The breakID is an optional identifier for the ad.</span></span> <span data-ttu-id="bf272-174">A timeOffset határozza meg, ha az ad üzenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="bf272-174">The timeOffset specifies when the ad should be displayed.</span></span> <span data-ttu-id="bf272-175">Azt is megadhatók a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="bf272-175">It can be specified in one of the following ways:</span></span>

1. <span data-ttu-id="bf272-176">Idő óó: pp: vagy hh:mm:ss.mmm formátumban, ahol .mmm az ezredmásodperc.</span><span class="sxs-lookup"><span data-stu-id="bf272-176">Time – in hh:mm:ss or hh:mm:ss.mmm format where .mmm is milliseconds.</span></span> <span data-ttu-id="bf272-177">Ez az attribútum értékét megadja azt az időtartamot, a videó ütemterv kezdetétől a az ad-break elejére.</span><span class="sxs-lookup"><span data-stu-id="bf272-177">The value of this attribute specifies the time from the beginning of the video timeline to the beginning of the ad break.</span></span>
2. <span data-ttu-id="bf272-178">Százalékos – n % formátumban ahol n az százaléka a videó ütemterv számára, hogy az ad lejátszás előtt</span><span class="sxs-lookup"><span data-stu-id="bf272-178">Percentage – in n% format where n is the percentage of the video timeline to play before playing the ad</span></span>
3. <span data-ttu-id="bf272-179">Kezdő és záró – Megadja, hogy egy ad üzenetnek kell megjelennie, előtt vagy után a videó meg lett jelenítve.</span><span class="sxs-lookup"><span data-stu-id="bf272-179">Start/End – specifies that an ad should be displayed before or after the video has been displayed</span></span>
4. <span data-ttu-id="bf272-180">Helyezze – ad oldaltörések sorrendje határozza meg, ha az ad oldaltörések időzítése ismeretlen, például élő Stream továbbítása.</span><span class="sxs-lookup"><span data-stu-id="bf272-180">Position – specifies the order of ad breaks when the timing of the ad breaks is unknown, such as in live streaming.</span></span> <span data-ttu-id="bf272-181">Minden ad break sorrendjét a #n formátumban, ahol n az 1 vagy nagyobb egész szám van megadva.</span><span class="sxs-lookup"><span data-stu-id="bf272-181">The order of each ad break is specified in the #n format where n is an integer 1 or greater.</span></span> <span data-ttu-id="bf272-182">1 azt jelzi, hogy az ad lejátszani az első adandó 2 azt jelzi, hogy az ad lejátszani a második alkalommal és így tovább.</span><span class="sxs-lookup"><span data-stu-id="bf272-182">1 signifies the ad should be played at the first opportunity, 2 signifies the ad should be played at the second opportunity and so on.</span></span>

<span data-ttu-id="bf272-183">Belül a <**AdBreak**> elem nem lehet az egyik <**AdSource**> elemet.</span><span class="sxs-lookup"><span data-stu-id="bf272-183">Within the <**AdBreak**> element there can be one <**AdSource**> element.</span></span> <span data-ttu-id="bf272-184">A <**AdSource**> elem tartalmazza-e a következő attribútumokat:</span><span class="sxs-lookup"><span data-stu-id="bf272-184">The <**AdSource**> element contains the following attributes:</span></span>

1. <span data-ttu-id="bf272-185">Azonosító – meghatározza az ad-forrás azonosítója</span><span class="sxs-lookup"><span data-stu-id="bf272-185">Id – specifies an identifier for the ad source</span></span>
2. <span data-ttu-id="bf272-186">allowMultipleAds – egy logikai érték, amely meghatározza, hogy több ads is megjelenjen-e az ad-break során</span><span class="sxs-lookup"><span data-stu-id="bf272-186">allowMultipleAds – a Boolean value that specifies whether multiple ads can be displayed during the ad break</span></span>
3. <span data-ttu-id="bf272-187">followRedirects – egy választható logikai érték, amely meghatározza, hogy ha a videólejátszó kell tiszteletben átirányítja a felhasználókat egy ad választ belül</span><span class="sxs-lookup"><span data-stu-id="bf272-187">followRedirects – an optional Boolean value that specifies if the video player should honor redirects within an ad response</span></span>

<span data-ttu-id="bf272-188">A <**AdSource**> elem biztosít a Windows Media player egy beágyazott ad választ vagy egy ad választ mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="bf272-188">The <**AdSource**> element provides the player an inline ad response or a reference to an ad response.</span></span> <span data-ttu-id="bf272-189">Az alábbi elemek egyikét tartalmazhat:</span><span class="sxs-lookup"><span data-stu-id="bf272-189">It can contain one of the following elements:</span></span>

* <span data-ttu-id="bf272-190"><VASTAdData>azt jelzi, hogy a VMAP fájlon belüli beágyazott túlnyomó ad választ</span><span class="sxs-lookup"><span data-stu-id="bf272-190"><VASTAdData> indicates a VAST ad response is embedded within the VMAP file</span></span>
* <span data-ttu-id="bf272-191"><AdTagURI>URI, amely egy ad választ hivatkozik másik rendszerről</span><span class="sxs-lookup"><span data-stu-id="bf272-191"><AdTagURI> a URI that references an ad response from another system</span></span>
* <span data-ttu-id="bf272-192"><CustomAdData>– egy tetszőleges karakterlánc, adott respresents nem túlnyomó választ</span><span class="sxs-lookup"><span data-stu-id="bf272-192"><CustomAdData> -an arbitrary string that respresents a non-VAST response</span></span>

<span data-ttu-id="bf272-193">Ebben a példában egy beágyazott ad választ meg van adva egy <VASTAdData> elem, amely tartalmazza a túlnyomó ad választ.</span><span class="sxs-lookup"><span data-stu-id="bf272-193">In this example an in-line ad response is specified with a <VASTAdData> element that contains a VAST ad response.</span></span> <span data-ttu-id="bf272-194">Más elemeivel kapcsolatos további információkért lásd: [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).</span><span class="sxs-lookup"><span data-stu-id="bf272-194">For more information about the other elements, see [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).</span></span>

<span data-ttu-id="bf272-195">A <**AdBreak**> elem is tartalmazhat egy <**TrackingEvents**> elemet.</span><span class="sxs-lookup"><span data-stu-id="bf272-195">The <**AdBreak**> element can also contain one <**TrackingEvents**> element.</span></span> <span data-ttu-id="bf272-196">A <**TrackingEvents**> elem lehetővé teszi, hogy nyomon követheti a kezdeti vagy egy ad break vagy e hiba történ a ad break végét.</span><span class="sxs-lookup"><span data-stu-id="bf272-196">The <**TrackingEvents**> element allows you to track the start or end of an ad break or whether an error occurred during the ad break.</span></span> <span data-ttu-id="bf272-197">A <**TrackingEvents**> elem tartalmaz egy vagy több <**követési**> elemek, amelyek mindegyike egy nyomkövetési esemény és egy követési URI adja meg.</span><span class="sxs-lookup"><span data-stu-id="bf272-197">The <**TrackingEvents**> element contains one or more <**Tracking**> elements, each of which specifies a tracking event and a tracking URI.</span></span> <span data-ttu-id="bf272-198">A lehetséges nyomkövetési események állnak:</span><span class="sxs-lookup"><span data-stu-id="bf272-198">The possible tracking events are:</span></span>

1. <span data-ttu-id="bf272-199">breakStart – nyomon követi az ad-break kezdete</span><span class="sxs-lookup"><span data-stu-id="bf272-199">breakStart – tracks the beginning of an ad break</span></span>
2. <span data-ttu-id="bf272-200">breakEnd – egy ad break megvalósításának nyomon követése</span><span class="sxs-lookup"><span data-stu-id="bf272-200">breakEnd – track the completion of an ad break</span></span>
3. <span data-ttu-id="bf272-201">Hiba – nyomon követi a ad break bekövetkezett hiba</span><span class="sxs-lookup"><span data-stu-id="bf272-201">error – tracks an error that occurred during the ad break</span></span>

<span data-ttu-id="bf272-202">A következő példa bemutatja, amely meghatározza a nyomkövetési események VMAP fájl</span><span class="sxs-lookup"><span data-stu-id="bf272-202">The following example shows a VMAP file that specifies tracking events</span></span>

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

<span data-ttu-id="bf272-203">További információt a <**TrackingEvents**> elem és a gyermekek, lásd: http://iab.org/VMAP.pdf</span><span class="sxs-lookup"><span data-stu-id="bf272-203">For more information on the <**TrackingEvents**> element and its children, see http://iab.org/VMAP.pdf</span></span>

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a><span data-ttu-id="bf272-204">Egy Media absztrakt alkalmazás-előkészítés sablonfájl (OSZLOPOS) használatával</span><span class="sxs-lookup"><span data-stu-id="bf272-204">Using a Media Abstract Sequencing Template (MAST) File</span></span>
<span data-ttu-id="bf272-205">Egy OSZLOPOS fájlt adja meg, amelyek meghatározzák, hogy mikor jelenik meg az ad eseményindítók teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="bf272-205">A MAST file allows you to specify triggers that define when an ad is displayed.</span></span> <span data-ttu-id="bf272-206">Egy példa egy előtti összegző ad, egy közepes összegző ad és a utáni összegző ad eseményindítók tartalmazó OSZLOPOS fájlt a következő:</span><span class="sxs-lookup"><span data-stu-id="bf272-206">The following is an example MAST file that contains triggers for a pre roll ad, a mid-roll ad, and a post-roll ad.</span></span>

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
            <!--This 'resets' the trigger for the next clip-->
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



<span data-ttu-id="bf272-207">Egy OSZLOPOS fájl kezdődik egy **OSZLOPOS** elem, amely egy **eseményindítók** elemet.</span><span class="sxs-lookup"><span data-stu-id="bf272-207">A MAST file begins with a **MAST** element that contains one **triggers** element.</span></span> <span data-ttu-id="bf272-208">A <triggers> egy vagy több elemet tartalmaz **eseményindító** határozza meg, ha az ad lejátszani.</span><span class="sxs-lookup"><span data-stu-id="bf272-208">The <triggers> element contains one or more **trigger** elements that define when an ad should be played.</span></span> 

<span data-ttu-id="bf272-209">A **eseményindító** elem tartalmazza-e egy **startConditions** elem, amelynek adja meg, ha az ad számára, hogy kezdjen.</span><span class="sxs-lookup"><span data-stu-id="bf272-209">The **trigger** element contains a **startConditions** element which specify when an ad should begin to play.</span></span> <span data-ttu-id="bf272-210">A **startConditions** egy vagy több elemet tartalmaz <condition> elemek.</span><span class="sxs-lookup"><span data-stu-id="bf272-210">The **startConditions** element contains one or more <condition> elements.</span></span> <span data-ttu-id="bf272-211">Ha minden <condition> igaz eseményindító kezdeményezett, vagy visszavonták, attól függően, hogy a <condition> magában foglal egy **startConditions** vagy **endConditions** elem kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="bf272-211">When each <condition> evaluates to true a trigger is initiated or revoked depending upon whether the <condition> is contained within a **startConditions** or **endConditions** element respectively.</span></span> <span data-ttu-id="bf272-212">Ha több <condition> elem is szerepel, azokat egy implicit vagy számít, a feltétel kiértékelése igaz, akkor az eseményindító kezdeményezéséhez.</span><span class="sxs-lookup"><span data-stu-id="bf272-212">When multiple <condition> elements are present, they are treated as an implicit OR, any condition evaluating to true will cause the trigger to initiate.</span></span> <span data-ttu-id="bf272-213"><condition>elemek egymásba ágyazható.</span><span class="sxs-lookup"><span data-stu-id="bf272-213"><condition> elements can be nested.</span></span> <span data-ttu-id="bf272-214">Ha gyermek <condition> elemek előre be van állítva, akkor számít egy implicit és, minden feltétel ki kell értékelnie az eseményindító kezdeményezése igaz.</span><span class="sxs-lookup"><span data-stu-id="bf272-214">When child <condition> elements are preset, they are treated as an implicit AND, all conditions must evaluate to true for the trigger to initiate.</span></span> <span data-ttu-id="bf272-215">A <condition> elem tartalmazza-e a következő attribútumok, amelyek meghatározzák a következő feltételt:</span><span class="sxs-lookup"><span data-stu-id="bf272-215">The <condition> element contains the following attributes that define the condition:</span></span> 

1. <span data-ttu-id="bf272-216">**típus** – meghatározza az állapot, esemény vagy tulajdonság típusát</span><span class="sxs-lookup"><span data-stu-id="bf272-216">**type** – specifies the type of condition, event or property</span></span>
2. <span data-ttu-id="bf272-217">**név** – a következő tulajdonság vagy esemény kiértékelés során használandó neve</span><span class="sxs-lookup"><span data-stu-id="bf272-217">**name** – the name of the property or event to be used during evaluation</span></span>
3. <span data-ttu-id="bf272-218">**érték** – az értéket, amelyet a tulajdonság értékelni</span><span class="sxs-lookup"><span data-stu-id="bf272-218">**value** – the value that a property will be evaluated against</span></span>
4. <span data-ttu-id="bf272-219">**operátor** – a kiértékelés során használandó művelet: EQ (egyenlő), a NEQ (nem egyenlő), a GTR (nagyobb), a GEQ (nagyobb vagy egyenlő), a LT (kisebb), LEQ (kisebb vagy egyenlő), MOD ELEMET (Maradékos osztás)</span><span class="sxs-lookup"><span data-stu-id="bf272-219">**operator** – the operation to use during evaluation: EQ (equal), NEQ (not equal), GTR (greater), GEQ (greater or equal), LT (Less than), LEQ (less than or equal), MOD (modulo)</span></span>

<span data-ttu-id="bf272-220">**endConditions** is tartalmazhat, <condition> elemek.</span><span class="sxs-lookup"><span data-stu-id="bf272-220">**endConditions** also contain <condition> elements.</span></span> <span data-ttu-id="bf272-221">Ha a feltétel igaz az eseményindító alaphelyzetbe áll. A <trigger> elem is tartalmaz egy <sources> elem, amely tartalmazza egy vagy több <source> elemek.</span><span class="sxs-lookup"><span data-stu-id="bf272-221">When a condition evaluates to true the trigger is reset.The <trigger> element also contains a <sources> element that contains one or more <source> elements.</span></span> <span data-ttu-id="bf272-222">A <source> elemek a ad választ, és milyen típusú ad választ az URI határozza meg.</span><span class="sxs-lookup"><span data-stu-id="bf272-222">The <source> elements define the URI to the ad response and the type of ad response.</span></span> <span data-ttu-id="bf272-223">Ebben a példában egy URI túlnyomó választ kapja.</span><span class="sxs-lookup"><span data-stu-id="bf272-223">In this example a URI is given to a VAST response.</span></span> 

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


### <a name="using-video-player-ad-interface-definition-vpaid"></a><span data-ttu-id="bf272-224">Videó Player Ad felületdefiníció (VPAID) használatával</span><span class="sxs-lookup"><span data-stu-id="bf272-224">Using Video Player-Ad Interface Definition (VPAID)</span></span>
<span data-ttu-id="bf272-225">VPAID az API-k engedélyezésének végrehajtható ad egység egy videólejátszó folytatott kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="bf272-225">VPAID is an API for enabling executable ad units to communicate with a video player.</span></span> <span data-ttu-id="bf272-226">Ez lehetővé teszi a magas interaktív ad lép.</span><span class="sxs-lookup"><span data-stu-id="bf272-226">This allows highly interactive ad experiences.</span></span> <span data-ttu-id="bf272-227">A felhasználók beavatkozhatnak-e az ad-val, és az ad válaszolhassanak a megjelenítő végrehajtott műveleteket.</span><span class="sxs-lookup"><span data-stu-id="bf272-227">The user can interact with the ad and the ad can respond to actions taken by the viewer.</span></span> <span data-ttu-id="bf272-228">Például az ad megjelenítésére gombok, amelyek lehetővé teszik a felhasználó hosszabb verzióját az ad vagy további információk megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="bf272-228">For example an ad may display buttons that allow the user to view more information or a longer version of the ad.</span></span> <span data-ttu-id="bf272-229">A videólejátszó támogatnia kell a VPAID API-t, és a végrehajtható ad meg kell valósítania az API-t.</span><span class="sxs-lookup"><span data-stu-id="bf272-229">The video player must support the VPAID API and the executable ad must implement the API.</span></span> <span data-ttu-id="bf272-230">Ha egy player kísérel meg a kiszolgáló ad-kiszolgálóról ad túlnyomó választ, amely tartalmaz egy VPAID ad jelenhetnek meg.</span><span class="sxs-lookup"><span data-stu-id="bf272-230">When a player requests an ad from an ad server the server may respond with a VAST response that contains a VPAID ad.</span></span>

<span data-ttu-id="bf272-231">Egy végrehajtható ad Adobe Flash™ vagy a webböngészőben végrehajtható JavaScript futásidejű környezetben kell végrehajtani kód jön létre.</span><span class="sxs-lookup"><span data-stu-id="bf272-231">An executable ad is created in code that must be executed in a runtime environment such as Adobe Flash™ or JavaScript that can be executed in a web browser.</span></span> <span data-ttu-id="bf272-232">Egy ad-kiszolgáló egy olyan VPAID ad tartalmazó túlnyomó választ ad vissza, ha a apiFramework értékének attribútumnak a <MediaFile> elemnek kell lennie a "VPAID".</span><span class="sxs-lookup"><span data-stu-id="bf272-232">When an ad server returns a VAST response containing a VPAID ad, the value of the apiFramework attribute in the <MediaFile> element must be “VPAID”.</span></span> <span data-ttu-id="bf272-233">Ez az attribútum Megadja, hogy az abban található ad VPAID végrehajtható ad.</span><span class="sxs-lookup"><span data-stu-id="bf272-233">This attribute specifies that the contained ad is a VPAID executable ad.</span></span> <span data-ttu-id="bf272-234">Az attribútum a MIME-típusát a végrehajtható fájljához, amilyen például az "application/x-shockwave-flash" vagy "application/x-javascript" értékre kell állítani.</span><span class="sxs-lookup"><span data-stu-id="bf272-234">The type attribute must be set to the MIME type of the executable, such as “application/x-shockwave-flash” or “application/x-javascript”.</span></span> <span data-ttu-id="bf272-235">A következő XML-részlet mutatja a <MediaFile> VPAID végrehajtható ad tartalmazó túlnyomó választ elemét.</span><span class="sxs-lookup"><span data-stu-id="bf272-235">The following XML snippet shows the <MediaFile> element from a VAST response containing a VPAID executable ad.</span></span> 

    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>


<span data-ttu-id="bf272-236">Egy végrehajtható ad használatával inicializálhatók a <AdParameters> elemet a <Linear> vagy <NonLinear> elemek túlnyomó választ.</span><span class="sxs-lookup"><span data-stu-id="bf272-236">An executable ad can be initialized using the <AdParameters> element within the <Linear> or <NonLinear> elements in a VAST response.</span></span> <span data-ttu-id="bf272-237">További információ a <AdParameters> elem, lásd: [túlnyomó 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span><span class="sxs-lookup"><span data-stu-id="bf272-237">For more information on the <AdParameters> element, see [VAST 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).</span></span> <span data-ttu-id="bf272-238">A VPAID API-val kapcsolatos további információkért lásd: [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).</span><span class="sxs-lookup"><span data-stu-id="bf272-238">For more information about the VPAID API, see [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).</span></span>

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a><span data-ttu-id="bf272-239">A Windows vagy Windows Phone 8 Player Ad-támogatással rendelkező megvalósítása</span><span class="sxs-lookup"><span data-stu-id="bf272-239">Implementing a Windows or Windows Phone 8 Player with Ad Support</span></span>
<span data-ttu-id="bf272-240">A Microsoft Media Platform: Player keretrendszer Windows 8 és Windows Phone 8 tartalmaz alkalmazásokra, amelyek bemutatják a keretrendszerrel videólejátszó alkalmazások végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="bf272-240">The Microsoft Media Platform: Player Framework for Windows 8 and Windows Phone 8 contains a collection of sample applications that show you how to implement a video player application using the framework.</span></span> <span data-ttu-id="bf272-241">Letöltheti a Player keretrendszer és a minták [Player keretrendszer Windows 8 és Windows Phone 8](https://playerframework.codeplex.com).</span><span class="sxs-lookup"><span data-stu-id="bf272-241">You can download the Player Framework and the samples from [Player Framework for Windows 8 and Windows Phone 8](https://playerframework.codeplex.com).</span></span>

<span data-ttu-id="bf272-242">A Microsoft.PlayerFramework.Xaml.Samples megoldás megnyitásakor látni fogja a mappák a projekt számát.</span><span class="sxs-lookup"><span data-stu-id="bf272-242">When you open the Microsoft.PlayerFramework.Xaml.Samples solution you will see a number of folders within the project.</span></span> <span data-ttu-id="bf272-243">A hirdetés mappa létrehozása egy videólejátszó ad-támogatással rendelkező kapcsolódik mintakód tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bf272-243">The Advertising folder contains the sample code relevant to creating a video player with ad support.</span></span> <span data-ttu-id="bf272-244">A hirdetés belül az mappa az XAML/cs fájlok száma, amelyek bemutatják, hogyan ads beszúrása más módon.</span><span class="sxs-lookup"><span data-stu-id="bf272-244">Inside the Advertising folder is a number of XAML/cs files each of which show how to insert ads in a different way.</span></span> <span data-ttu-id="bf272-245">Az alábbi lista ismerteti:</span><span class="sxs-lookup"><span data-stu-id="bf272-245">The following list describes each:</span></span>

* <span data-ttu-id="bf272-246">AdPodPage.xaml bemutatja, hogyan egy ad pod megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="bf272-246">AdPodPage.xaml Shows how to display an ad pod.</span></span>
* <span data-ttu-id="bf272-247">AdSchedulingPage.xaml bemutatja, hogyan ads ütemezni.</span><span class="sxs-lookup"><span data-stu-id="bf272-247">AdSchedulingPage.xaml Shows how to schedule ads.</span></span>
* <span data-ttu-id="bf272-248">FreeWheelPage.xaml ads ütemezni a FreeWheel beépülő modul használatával jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="bf272-248">FreeWheelPage.xaml Shows how to use the FreeWheel plugin to schedule ads.</span></span>
* <span data-ttu-id="bf272-249">MastPage.xaml bemutatja, hogyan ads ütemezése egy OSZLOPOS fájllal.</span><span class="sxs-lookup"><span data-stu-id="bf272-249">MastPage.xaml Shows how to schedule ads with a MAST file.</span></span>
* <span data-ttu-id="bf272-250">ProgrammaticAdPage.xaml programozott módon ütemezhet egy videóban ads mutatja be.</span><span class="sxs-lookup"><span data-stu-id="bf272-250">ProgrammaticAdPage.xaml Shows how to programmatically schedule ads into a video.</span></span>
* <span data-ttu-id="bf272-251">ScheduleClipPage.xaml bemutatja, hogyan ütemezése egy ad túlnyomó fájl nélkül.</span><span class="sxs-lookup"><span data-stu-id="bf272-251">ScheduleClipPage.xaml Shows how to schedule an ad without a VAST file.</span></span>
* <span data-ttu-id="bf272-252">VastLinearCompanionPage.xaml beszúrása egy lineáris és kiegészítő ad jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="bf272-252">VastLinearCompanionPage.xaml Shows how to insert a linear and companion ad.</span></span>
* <span data-ttu-id="bf272-253">VastNonLinearPage.xaml bemutatja, hogyan lehet beszúrni egy nem lineáris ad.</span><span class="sxs-lookup"><span data-stu-id="bf272-253">VastNonLinearPage.xaml Shows how to insert a non-linear ad.</span></span>
* <span data-ttu-id="bf272-254">VmapPage.xaml bemutatja, hogyan adhatja meg a hirdetések VMAP fájllal.</span><span class="sxs-lookup"><span data-stu-id="bf272-254">VmapPage.xaml Shows how to specify ads with a VMAP file.</span></span>

<span data-ttu-id="bf272-255">Ezeket a mintákat mindegyikének használ a Media Player határozzák meg a player keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="bf272-255">Each of these samples uses the MediaPlayer class defined by the player framework.</span></span> <span data-ttu-id="bf272-256">A legtöbb minták beépülő modulok, amelyek különböző ad választ formátumban támogatásához használja.</span><span class="sxs-lookup"><span data-stu-id="bf272-256">Most samples use plugins that add support for various ad response formats.</span></span> <span data-ttu-id="bf272-257">A ProgrammaticAdPage minta programokon keresztül kommunikál egy MediaPlayer-példányt.</span><span class="sxs-lookup"><span data-stu-id="bf272-257">The ProgrammaticAdPage sample programmatically interacts with a MediaPlayer instance.</span></span>

### <a name="adpodpage-sample"></a><span data-ttu-id="bf272-258">AdPodPage minta</span><span class="sxs-lookup"><span data-stu-id="bf272-258">AdPodPage Sample</span></span>
<span data-ttu-id="bf272-259">Ez a minta a AdSchedulerPlugin megadására az ad megjelenítéséhez használ.</span><span class="sxs-lookup"><span data-stu-id="bf272-259">This sample uses the AdSchedulerPlugin to define when to display an ad.</span></span> <span data-ttu-id="bf272-260">Ebben a példában egy közepes összegző hirdetmény 5 másodperc után lejátszandó van ütemezve.</span><span class="sxs-lookup"><span data-stu-id="bf272-260">In this example a mid-roll advertisement is scheduled to be played after 5 seconds.</span></span> <span data-ttu-id="bf272-261">Ad fogyasztanak (ads sorrendben megjelenítendő csoportja) egy ad-kiszolgáló által visszaadott túlnyomó fájlban van megadva.</span><span class="sxs-lookup"><span data-stu-id="bf272-261">The ad pod (a group of ads to display in order) is specified in a VAST file returned from an ad server.</span></span> <span data-ttu-id="bf272-262">Az URI-t a túlnyomó fájl van megadva a <RemoteAdSource> elemet.</span><span class="sxs-lookup"><span data-stu-id="bf272-262">The URI to the VAST file is specified in the <RemoteAdSource> element.</span></span>

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

<span data-ttu-id="bf272-263">A AdSchedulerPlugin kapcsolatos további információkért lásd: [hirdetési Player keretében a Windows 8 és Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)</span><span class="sxs-lookup"><span data-stu-id="bf272-263">For more information about the AdSchedulerPlugin, see [Advertising in the Player Framework on Windows 8 and Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)</span></span>

### <a name="adschedulingpage"></a><span data-ttu-id="bf272-264">AdSchedulingPage</span><span class="sxs-lookup"><span data-stu-id="bf272-264">AdSchedulingPage</span></span>
<span data-ttu-id="bf272-265">Ez a minta a AdSchedulerPlugin is használ.</span><span class="sxs-lookup"><span data-stu-id="bf272-265">This sample also uses the AdSchedulerPlugin.</span></span> <span data-ttu-id="bf272-266">Három ads, egy előtti összegző ad, egy közepes összegző ad és a utáni összegző ad ütemezés.</span><span class="sxs-lookup"><span data-stu-id="bf272-266">It schedules three ads, a pre-roll ad, a mid-roll ad, and a post-roll ad.</span></span> <span data-ttu-id="bf272-267">Az URI-t az egyes hirdetések VAST van megadva egy <RemoteAdSource> elemet.</span><span class="sxs-lookup"><span data-stu-id="bf272-267">The URI to the VAST for each ad is specified in a <RemoteAdSource> element.</span></span>

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


### <a name="freewheelpage"></a><span data-ttu-id="bf272-268">FreeWheelPage</span><span class="sxs-lookup"><span data-stu-id="bf272-268">FreeWheelPage</span></span>
<span data-ttu-id="bf272-269">Ez a minta a FreeWheelPlugin, amely meghatározza a forrásattribútumot, amely meghatározza az URI, amely egy SmartXML fájlra mutat, amely meghatározza a tartalom ad, valamint az ütemezési információkat ad használja.</span><span class="sxs-lookup"><span data-stu-id="bf272-269">This sample uses the FreeWheelPlugin which specifies a Source attribute that specifies a URI that points to a SmartXML file that specifies ad content as well as ad scheduling information.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="mastpage"></a><span data-ttu-id="bf272-270">MastPage</span><span class="sxs-lookup"><span data-stu-id="bf272-270">MastPage</span></span>
<span data-ttu-id="bf272-271">Ez a minta a MastSchedulerPlugin, amely lehetővé teszi egy OSZLOPOS fájl használja.</span><span class="sxs-lookup"><span data-stu-id="bf272-271">This sample uses the MastSchedulerPlugin that allows you to use a MAST file.</span></span> <span data-ttu-id="bf272-272">Az adatforrás-attribútum meghatározza a OSZLOPOS fájl helyét.</span><span class="sxs-lookup"><span data-stu-id="bf272-272">The Source attribute specifies the location of the MAST file.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="programmaticadpage"></a><span data-ttu-id="bf272-273">ProgrammaticAdPage</span><span class="sxs-lookup"><span data-stu-id="bf272-273">ProgrammaticAdPage</span></span>
<span data-ttu-id="bf272-274">Ez a minta a Media Player programokon keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="bf272-274">This sample programmatically interacts with the MediaPlayer.</span></span> <span data-ttu-id="bf272-275">A ProgrammaticAdPage.xaml fájl elindítja a Media Player:</span><span class="sxs-lookup"><span data-stu-id="bf272-275">The ProgrammaticAdPage.xaml file instantiates the MediaPlayer:</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

<span data-ttu-id="bf272-276">A ProgrammaticAdPage.xaml.cs fájlt hoz létre egy AdHandlerPlugin ad meg egy TimelineMarker ad üzenetnek kell megjelennie, és betölti a RemoteAdSource túlnyomó fájlba URI megadása, és az ad majd játszik MarkerReached eseménynél kezelőtársítást ad majd hozzá.</span><span class="sxs-lookup"><span data-stu-id="bf272-276">The ProgrammaticAdPage.xaml.cs file creates an AdHandlerPlugin, adds a TimelineMarker to specify when an ad should be displayed, and then adds a handler for the MarkerReached event which loads a RemoteAdSource specifying a URI to a VAST file, and then plays the ad.</span></span>

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

### <a name="scheduleclippage"></a><span data-ttu-id="bf272-277">ScheduleClipPage</span><span class="sxs-lookup"><span data-stu-id="bf272-277">ScheduleClipPage</span></span>
<span data-ttu-id="bf272-278">Ez a minta egy közepes összegző ad ütemezése egy .wmv-fájlt, amely tartalmazza az ad megadásával a AdSchedulerPlugin használja.</span><span class="sxs-lookup"><span data-stu-id="bf272-278">This sample uses the AdSchedulerPlugin to schedule a mid-roll ad by specifying a .wmv file that contains the ad.</span></span>

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

### <a name="vastlinearcompanionpage"></a><span data-ttu-id="bf272-279">VastLinearCompanionPage</span><span class="sxs-lookup"><span data-stu-id="bf272-279">VastLinearCompanionPage</span></span>
<span data-ttu-id="bf272-280">Ez a minta bemutatja, hogyan használja a AdSchedulerPlugin ütemezése egy közepes összegző lineáris ad egy kiegészítő ad-val.</span><span class="sxs-lookup"><span data-stu-id="bf272-280">This sample illustrates how to use the AdSchedulerPlugin to schedule a mid-roll linear ad with an companion ad.</span></span> <span data-ttu-id="bf272-281">A <RemoteAdSource> elem a túlnyomó fájl helyét adja meg.</span><span class="sxs-lookup"><span data-stu-id="bf272-281">The <RemoteAdSource> element specifies the location of the VAST file.</span></span>

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

### <a name="vastlinearnonlinearpage"></a><span data-ttu-id="bf272-282">VastLinearNonLinearPage</span><span class="sxs-lookup"><span data-stu-id="bf272-282">VastLinearNonLinearPage</span></span>
<span data-ttu-id="bf272-283">Ez a minta egy lineáris ütemezése AdSchedulerPlugin és egy nem lineáris ad használja.</span><span class="sxs-lookup"><span data-stu-id="bf272-283">This sample uses the AdSchedulerPlugin to schedule a linear and a non-linear ad.</span></span> <span data-ttu-id="bf272-284">A nagy fájl helye van megadva a <RemoteAdSource> elemet.</span><span class="sxs-lookup"><span data-stu-id="bf272-284">The VAST file location is specified with the <RemoteAdSource> element.</span></span>

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

### <a name="vmappage"></a><span data-ttu-id="bf272-285">VMAPPage</span><span class="sxs-lookup"><span data-stu-id="bf272-285">VMAPPage</span></span>
<span data-ttu-id="bf272-286">A minták VMAP fájllal ads ütemezni a VmapSchedulerPlugin használja.</span><span class="sxs-lookup"><span data-stu-id="bf272-286">This samples uses the VmapSchedulerPlugin to schedule ads using a VMAP file.</span></span> <span data-ttu-id="bf272-287">Az URI-t a VMAP fájl forrásattribútumának van megadva a <VmapSchedulerPlugin> elemet.</span><span class="sxs-lookup"><span data-stu-id="bf272-287">The URI to the VMAP file is specified in the Source attribute of the <VmapSchedulerPlugin> element.</span></span>

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

## <a name="implementing-an-ios-video-player-with-ad-support"></a><span data-ttu-id="bf272-288">IOS rendszerű Ad-támogatással rendelkező videó Player megvalósítása</span><span class="sxs-lookup"><span data-stu-id="bf272-288">Implementing an iOS Video Player with Ad Support</span></span>
<span data-ttu-id="bf272-289">A Microsoft Media Platform: Player keretrendszer IOS-alkalmazásokat, amelyek bemutatják a keretrendszerrel videólejátszó alkalmazások végrehajtásához gyűjteményét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bf272-289">The Microsoft Media Platform: Player Framework for iOS contains a collection of sample applications that show you how to implement a video player application using the framework.</span></span> <span data-ttu-id="bf272-290">Letöltheti a Player keretrendszer és a minták [Azure Media Player keretrendszer](https://github.com/Azure/azure-media-player-framework).</span><span class="sxs-lookup"><span data-stu-id="bf272-290">You can download the Player Framework and the samples from [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework).</span></span> <span data-ttu-id="bf272-291">A github-oldalon tartalmaz egy hivatkozást egy Wiki player keretében további adatokat tartalmazó és a player minta bemutatása: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).</span><span class="sxs-lookup"><span data-stu-id="bf272-291">The github page has a link to a Wiki that contains additional information on the player framework and an introduction to the player sample: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).</span></span>

### <a name="scheduling-ads-with-vmap"></a><span data-ttu-id="bf272-292">VMAP hirdetések ütemezése</span><span class="sxs-lookup"><span data-stu-id="bf272-292">Scheduling Ads with VMAP</span></span>
<span data-ttu-id="bf272-293">A következő példa bemutatja, hogyan VMAP fájllal ads ütemezni.</span><span class="sxs-lookup"><span data-stu-id="bf272-293">The following example shows how to schedule ads using a VMAP file.</span></span>

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest

    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

### <a name="scheduling-ads-with-vast"></a><span data-ttu-id="bf272-294">VAST hirdetések ütemezése</span><span class="sxs-lookup"><span data-stu-id="bf272-294">Scheduling Ads with VAST</span></span>
<span data-ttu-id="bf272-295">A következő példa bemutatja, hogyan ütemezni a késői kötés túlnyomó ad.</span><span class="sxs-lookup"><span data-stu-id="bf272-295">The following sample shows how to schedule a late binding VAST ad.</span></span>

    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
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

   <span data-ttu-id="bf272-296">A következő példa bemutatja, hogyan ütemezése egy korai kötés túlnyomó ad.</span><span class="sxs-lookup"><span data-stu-id="bf272-296">The following sample shows how to schedule an early binding VAST ad.</span></span>
<span data-ttu-id="bf272-297">Példa: 4 ütemezés korai kötés túlnyomó ad-//Download a VAST fájlt, ha (! [ framework.adResolver downloadManifest: & jegyzék withURL: [által igényelt NSURL URLWithString: @"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[self logFrameworkError];} else {adLinearTime.startTime = 7; adLinearTime.duration = 0;</span><span class="sxs-lookup"><span data-stu-id="bf272-297">//Example:4 Schedule an early binding VAST ad //Download the VAST file if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) { [self logFrameworkError]; } else { adLinearTime.startTime = 7; adLinearTime.duration = 0;</span></span>

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

<span data-ttu-id="bf272-298">A következő példa bemutatja, hogyan hirdetések nyers Kivágás szerkesztése (RCE) használatával</span><span class="sxs-lookup"><span data-stu-id="bf272-298">The following sample shows how to insert an ad using Rough Cut Editing (RCE)</span></span>

    //Example:1 How to use RCE.
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

<span data-ttu-id="bf272-299">A következő példa bemutatja, hogyan egy ad pod ütemezni.</span><span class="sxs-lookup"><span data-stu-id="bf272-299">The following example shows how to schedule an ad pod.</span></span>

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;

    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
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

<span data-ttu-id="bf272-300">A következő példa bemutatja, hogyan ütemezése a nem kapcsolódó közepes összegző ad.</span><span class="sxs-lookup"><span data-stu-id="bf272-300">The following example shows how to schedule a non-sticky mid-roll ad.</span></span> <span data-ttu-id="bf272-301">A nem kapcsolódó ad csak lejátszott, miután függetlenül bármilyen keresést az használatával hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="bf272-301">A non-sticky ad is only played once regardless of any seeking the viewer performs.</span></span>

    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
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

<span data-ttu-id="bf272-302">A következő példa bemutatja, hogyan egy állandóságát közepes összegző ad ütemezni.</span><span class="sxs-lookup"><span data-stu-id="bf272-302">The following example shows how to schedule a sticky mid-roll ad.</span></span> <span data-ttu-id="bf272-303">A kapcsolódó ad fog megjelenni minden alkalommal, amikor az adott pont a videó idősoron éri el.</span><span class="sxs-lookup"><span data-stu-id="bf272-303">A sticky ad will be displayed each time the specified point on the video timeline is reached.</span></span>

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
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


<span data-ttu-id="bf272-304">A következő példa bemutatja, hogyan ütemezése egy utáni összegző ad.</span><span class="sxs-lookup"><span data-stu-id="bf272-304">The following sample shows how to schedule a post-roll ad.</span></span>

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

<span data-ttu-id="bf272-305">A következő példa bemutatja, hogyan ütemezése egy előtti összegző ad.</span><span class="sxs-lookup"><span data-stu-id="bf272-305">The following sample shows how to schedule a pre-roll ad.</span></span>

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

<span data-ttu-id="bf272-306">A következő példa bemutatja, hogyan ütemezése egy közepes összegző átfedő ad.</span><span class="sxs-lookup"><span data-stu-id="bf272-306">The following sample shows how to schedule a mid-roll overlay ad.</span></span>

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



## <a name="media-services-learning-paths"></a><span data-ttu-id="bf272-307">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="bf272-307">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="bf272-308">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="bf272-308">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="bf272-309">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="bf272-309">See Also</span></span>
[<span data-ttu-id="bf272-310">Videolejátszó alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="bf272-310">Develop video player applications</span></span>](media-services-develop-video-players.md)

