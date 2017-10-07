---
title: "aaaMultiple bemeneti fájlok és a prémium szintű kódolás - Azure összetevő tulajdonságai |} Microsoft Docs"
description: "Ez a témakör azt ismerteti, hogyan toouse setRuntimeProperties toouse több bemeneti fájl, és adja át egyéni adatok toohello Media Encoder prémium munkafolyamat media processzor."
services: media-services
documentationcenter: 
author: xpouyat
manager: cfowler
editor: 
ms.assetid: 7fb35bdd-9891-4401-a65b-ef3cc8190e8a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: xpouyat;anilmur;juliako
ms.openlocfilehash: e14d10fbf9669e0b88e5ba1c519f1ba5e0bafdd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a><span data-ttu-id="15684-103">A prémium szintű kódolás több bemeneti fájlok és összetevő tulajdonságai használja</span><span class="sxs-lookup"><span data-stu-id="15684-103">Using multiple input files and component properties with Premium Encoder</span></span>
## <a name="overview"></a><span data-ttu-id="15684-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="15684-104">Overview</span></span>
<span data-ttu-id="15684-105">Forgatókönyv, ahol szükség lehet a toocustomize összetevő tulajdonságai, adja meg a klip lista XML-tartalom, vagy több bemeneti fájlok küldése hello kötegazonosítójú feladat elküldése **Media Encoder prémium munkafolyamat** media processzor.</span><span class="sxs-lookup"><span data-stu-id="15684-105">There are scenarios in which you might need toocustomize component properties, specify Clip List XML content, or send multiple input files when you submit a task with hello **Media Encoder Premium Workflow** media processor.</span></span> <span data-ttu-id="15684-106">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="15684-106">Some examples are:</span></span>

* <span data-ttu-id="15684-107">A videó szöveg felirataként és hello szöveges érték (például a jelenlegi dátum hello) beállítása az egyes bemeneti videó futásidőben.</span><span class="sxs-lookup"><span data-stu-id="15684-107">Overlaying text on video and setting hello text value (for example, hello current date) at runtime for each input video.</span></span>
* <span data-ttu-id="15684-108">Testreszabás hello klip lista XML (toospecify egy vagy több forrás-fájlok, vagy anélkül díszítésre stb.).</span><span class="sxs-lookup"><span data-stu-id="15684-108">Customizing hello Clip List XML (toospecify one or several source files, with or without trimming, etc.).</span></span>
* <span data-ttu-id="15684-109">Egy embléma felirataként hello bemeneti videóhoz, miközben hello videó.</span><span class="sxs-lookup"><span data-stu-id="15684-109">Overlaying a logo image on hello input video while hello video is encoded.</span></span>
* <span data-ttu-id="15684-110">Több hang nyelvi kódolást.</span><span class="sxs-lookup"><span data-stu-id="15684-110">Multiple audio language encoding.</span></span>

<span data-ttu-id="15684-111">toolet hello **Media Encoder prémium munkafolyamat** tudja, hogy néhány tulajdonság hello munkafolyamat módosítani hello feladat létrehozásakor, vagy több bemeneti fájlok küldése telepítette, toouse tartalmazó konfigurációs karakterlánc  **setRuntimeProperties** és/vagy **transcodeSource**.</span><span class="sxs-lookup"><span data-stu-id="15684-111">toolet hello **Media Encoder Premium Workflow** know that you are changing some properties in hello workflow when you create hello task or send multiple input files, you have toouse a configuration string that contains **setRuntimeProperties** and/or **transcodeSource**.</span></span> <span data-ttu-id="15684-112">Ez a témakör azt ismerteti, hogyan toouse őket.</span><span class="sxs-lookup"><span data-stu-id="15684-112">This topic explains how toouse them.</span></span>

## <a name="configuration-string-syntax"></a><span data-ttu-id="15684-113">Konfigurációs karakterlánc-formátum:</span><span class="sxs-lookup"><span data-stu-id="15684-113">Configuration string syntax</span></span>
<span data-ttu-id="15684-114">hello konfigurációs karakterlánc tooset a kódolási feladat hello használja az XML-dokumentum, amely a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="15684-114">hello configuration string tooset in hello encoding task uses an XML document that looks like this:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<transcodeRequest>
  <transcodeSource>
  </transcodeSource>
  <setRuntimeProperties>
    <property propertyPath="Media File Input/filename" value="VideoFileName.mp4" />
  </setRuntimeProperties>
</transcodeRequest>
```

<span data-ttu-id="15684-115">hello következő hello C# kóddal, amely hello XML konfigurációs fájlból olvassa be, frissítse hello jobb videó fájlnév, és továbbítja azokat a feladatok toohello feladat:</span><span class="sxs-lookup"><span data-stu-id="15684-115">hello following is hello C# code that reads hello XML configuration from a file, update it with hello right video filename and passes it toohello task in a job:</span></span>

```c#
string premiumConfiguration = ReadAllText(@"D:\home\site\wwwroot\Presets\SetRuntime.xml").Replace("VideoFileName", myVideoFileName);

// Declare a new job.
IJob job = _context.Jobs.Create("Premium Workflow encoding job");

// Get a media processor reference, and pass tooit hello name of hello 
// processor toouse for hello specific task.
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

// Create a task with hello encoding details, using a string preset.
ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                              processor,
                              premiumConfiguration,
                              TaskOptions.None);

// Specify hello input assets
task.InputAssets.Add(workflow); // workflow asset
task.InputAssets.Add(video); // video asset with multiple files

// Add an output asset toocontain hello results of hello job. 
// This output is specified as AssetCreationOptions.None, which 
// means hello output asset is not encrypted. 
task.OutputAssets.AddNew("Output asset", AssetCreationOptions.None);
```

## <a name="customizing-component-properties"></a><span data-ttu-id="15684-116">Összetevő tulajdonságai testreszabása</span><span class="sxs-lookup"><span data-stu-id="15684-116">Customizing component properties</span></span>
### <a name="property-with-a-simple-value"></a><span data-ttu-id="15684-117">Egyszerű értéke</span><span class="sxs-lookup"><span data-stu-id="15684-117">Property with a simple value</span></span>
<span data-ttu-id="15684-118">Bizonyos esetekben hasznos toocustomize együtt hello munkafolyamat fájl, amelyet hajtja végre a Media Encoder prémium munkafolyamat toobe összetevő tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="15684-118">In some cases, it is useful toocustomize a component property together with hello workflow file that is going toobe executed by Media Encoder Premium Workflow.</span></span>

<span data-ttu-id="15684-119">Tegyük fel, a munkafolyamat átfedések szöveg a videók, és a következő hello szöveg (például a jelenlegi dátum hello) toobe set kellene futásidőben.</span><span class="sxs-lookup"><span data-stu-id="15684-119">Suppose you designed a workflow that overlays text on your videos, and hello text (for example, hello current date) is supposed toobe set at runtime.</span></span> <span data-ttu-id="15684-120">Ehhez hello szöveg toobe hello text tulajdonságához hello átfedő összetevő hello új értéket állítja be a kódolási feladat hello küldésével.</span><span class="sxs-lookup"><span data-stu-id="15684-120">You can do this by sending hello text toobe set as hello new value for hello text property of hello overlay component from hello encoding task.</span></span> <span data-ttu-id="15684-121">A mechanizmus toochange egyéb tulajdonságok használhatók az összetevő hello munkafolyamatban (például hello pozícióját vagy hello felirat színét, hello sávszélességű hello AVC kódoló, stb.).</span><span class="sxs-lookup"><span data-stu-id="15684-121">You can use this mechanism toochange other properties of a component in hello workflow (such as hello position or color of hello overlay, hello bitrate of hello AVC encoder, etc.).</span></span>

<span data-ttu-id="15684-122">**setRuntimeProperties** használt toooverride hello munkafolyamat hello összetevői tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="15684-122">**setRuntimeProperties** is used toooverride a property in hello components of hello workflow.</span></span>

<span data-ttu-id="15684-123">Példa:</span><span class="sxs-lookup"><span data-stu-id="15684-123">Example:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
      <property propertyPath="Optional Text Overlay/Text tooImage Converter/text" value="Today is Friday hello 13th of May, 2016"/>
  </setRuntimeProperties>
</transcodeRequest>
```

### <a name="property-with-an-xml-value"></a><span data-ttu-id="15684-124">Az XML-értéke tulajdonság</span><span class="sxs-lookup"><span data-stu-id="15684-124">Property with an XML value</span></span>
<span data-ttu-id="15684-125">tooset vár egy XML-érték tulajdonság használatával beágyazására `<![CDATA[ and ]]>`.</span><span class="sxs-lookup"><span data-stu-id="15684-125">tooset a property that expects an XML value, encapsulate by using `<![CDATA[ and ]]>`.</span></span>

<span data-ttu-id="15684-126">Példa:</span><span class="sxs-lookup"><span data-stu-id="15684-126">Example:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

> [!NOTE]
> <span data-ttu-id="15684-127">Győződjön meg arról, hogy nem tooput kocsivissza csak után térjen vissza `<![CDATA[`.</span><span class="sxs-lookup"><span data-stu-id="15684-127">Make sure not tooput a carriage return just after `<![CDATA[`.</span></span>

### <a name="propertypath-value"></a><span data-ttu-id="15684-128">a propertyPath érték</span><span class="sxs-lookup"><span data-stu-id="15684-128">propertyPath value</span></span>
<span data-ttu-id="15684-129">Az előző példákban hello hello propertyPath lett "/ Media File bemeneti/filename" vagy "/ inactiveTimeout" vagy "clipListXml".</span><span class="sxs-lookup"><span data-stu-id="15684-129">In hello previous examples, hello propertyPath was "/Media File Input/filename" or "/inactiveTimeout" or "clipListXml".</span></span>
<span data-ttu-id="15684-130">Ez az általában hello hello összetevő neve, majd hello tulajdonság hello nevét.</span><span class="sxs-lookup"><span data-stu-id="15684-130">This is, in general, hello name of hello component, then hello name of hello property.</span></span> <span data-ttu-id="15684-131">hello elérési utat is van több vagy kevesebb, mint "/ primarySourceFile" (mert hello tulajdonság hello gyökerében hello munkafolyamat) vagy "/ videó feldolgozási/kép átfedő/átlátszatlanság" (mert átfedő hello csoportban).</span><span class="sxs-lookup"><span data-stu-id="15684-131">hello path can have more or fewer levels, like "/primarySourceFile" (because hello property is at hello root of hello workflow) or "/Video Processing/Graphic Overlay/Opacity" (because hello Overlay is in a group).</span></span>    

<span data-ttu-id="15684-132">toocheck hello elérési útját és a tulajdonság nevét, használja hello akciógombra kattinthat, amely közvetlenül mellett minden egyes tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="15684-132">toocheck hello path and property name, use hello action button that is immediately beside each property.</span></span> <span data-ttu-id="15684-133">A művelet gombra kattintva, és válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="15684-133">You can click this action button and select **Edit**.</span></span> <span data-ttu-id="15684-134">Ez azt mutatja majd, hello tényleges név: hello tulajdonság, és azonnal felette, hello névtér.</span><span class="sxs-lookup"><span data-stu-id="15684-134">This will show you hello actual name of hello property, and immediately above it, hello namespace.</span></span>

![A művelet/szerkesztése](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Tulajdonság](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a><span data-ttu-id="15684-137">Több bemeneti fájl</span><span class="sxs-lookup"><span data-stu-id="15684-137">Multiple input files</span></span>
<span data-ttu-id="15684-138">Egyes feladatokat, hogy elküldését toohello **Media Encoder prémium munkafolyamat** két eszközök igényel:</span><span class="sxs-lookup"><span data-stu-id="15684-138">Each task that you submit toohello **Media Encoder Premium Workflow** requires two assets:</span></span>

* <span data-ttu-id="15684-139">hello először még egy *munkafolyamat eszköz* , amely a munkafolyamat-fájlt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="15684-139">hello first one is a *Workflow Asset* that contains a workflow file.</span></span> <span data-ttu-id="15684-140">Hello segítségével megtervezheti a munkafolyamat-fájlok [munkafolyamat-Tervező](media-services-workflow-designer.md).</span><span class="sxs-lookup"><span data-stu-id="15684-140">You can design workflow files by using hello [Workflow Designer](media-services-workflow-designer.md).</span></span>
* <span data-ttu-id="15684-141">hello második van egy *Media eszköz* hello media (oka) t, amelyet az tooencode tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="15684-141">hello second one is a *Media Asset* that contains hello media file(s) that you want tooencode.</span></span>

<span data-ttu-id="15684-142">Ha több adathordozó fájlok toohello küldjük **Media Encoder prémium munkafolyamat** kódoló, hello a következő korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="15684-142">When you're sending multiple media files toohello **Media Encoder Premium Workflow** encoder, hello following constraints apply:</span></span>

* <span data-ttu-id="15684-143">Fájlok kell lennie minden hello media hello azonos *Media eszköz*.</span><span class="sxs-lookup"><span data-stu-id="15684-143">All hello media files must be in hello same *Media Asset*.</span></span> <span data-ttu-id="15684-144">Több adathordozó eszközök használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="15684-144">Using multiple Media Assets is not supported.</span></span>
* <span data-ttu-id="15684-145">Meg kell adnia hello elsődleges fájl a Media eszköz (ideális esetben ez a hello kódoló hello fő videofájl kapcsolatba tooprocess).</span><span class="sxs-lookup"><span data-stu-id="15684-145">You must set hello primary file in this Media Asset (ideally, this is hello main video file that hello encoder is asked tooprocess).</span></span>
* <span data-ttu-id="15684-146">Hello tartalmazó toopass szükséges konfigurációs adatok **setRuntimeProperties** és/vagy **transcodeSource** elem toohello processzor.</span><span class="sxs-lookup"><span data-stu-id="15684-146">It is necessary toopass configuration data that includes hello **setRuntimeProperties** and/or **transcodeSource** element toohello processor.</span></span>
  * <span data-ttu-id="15684-147">**setRuntimeProperties** használt toooverride hello filename tulajdonságban vagy a hello összetevők hello munkafolyamat egy másik tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="15684-147">**setRuntimeProperties** is used toooverride hello filename property or another property in hello components of hello workflow.</span></span>
  * <span data-ttu-id="15684-148">**transcodeSource** használt toospecify hello klip lista XML-tartalom.</span><span class="sxs-lookup"><span data-stu-id="15684-148">**transcodeSource** is used toospecify hello Clip List XML content.</span></span>

<span data-ttu-id="15684-149">Hello munkafolyamat kapcsolatok:</span><span class="sxs-lookup"><span data-stu-id="15684-149">Connections in hello workflow:</span></span>

* <span data-ttu-id="15684-150">Ha egy vagy több adathordozó fájl bemeneti összetevőket használnak, és tervezze meg toouse **setRuntimeProperties** toospecify hello fájl nevét, majd hello elsődleges fájl összetevő PIN-kód toothem csatlakoztatásának mellőzése.</span><span class="sxs-lookup"><span data-stu-id="15684-150">If you use one or several Media File Input components and plan toouse **setRuntimeProperties** toospecify hello file name, then do not connect hello primary file component pin toothem.</span></span> <span data-ttu-id="15684-151">Győződjön meg arról, hogy nincs-e hello elsődleges fájl objektum és hello Media fájl Input(s) közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="15684-151">Make sure that there is no connection between hello primary file object and hello Media File Input(s).</span></span>
* <span data-ttu-id="15684-152">Ha jobban szeret toouse klip lista XML és egy forrás-adathordozó összetevőt, majd csatlakozhat mindkét együtt.</span><span class="sxs-lookup"><span data-stu-id="15684-152">If you prefer toouse Clip List XML and one Media Source component, then you can connect both together.</span></span>

![Nincs elsődleges forrásfájl tooMedia fájl bemeneti közötti kapcsolat](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

<span data-ttu-id="15684-154">*Nincs kapcsolat hello elsődleges fájl tooMedia fájl bemeneti elemét/elemeit az setRuntimeProperties tooset hello filename tulajdonságban használatakor.*</span><span class="sxs-lookup"><span data-stu-id="15684-154">*There is no connection from hello primary file tooMedia File Input component(s) if you use setRuntimeProperties tooset hello filename property.*</span></span>

![Klip lista XML tooClip forráslista közötti kapcsolat](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

<span data-ttu-id="15684-156">*Forrás klip lista XML tooMedia csatlakozhat, és transcodeSource használja.*</span><span class="sxs-lookup"><span data-stu-id="15684-156">*You can connect Clip List XML tooMedia Source and use transcodeSource.*</span></span>

### <a name="clip-list-xml-customization"></a><span data-ttu-id="15684-157">Lista XML testreszabási levágása</span><span class="sxs-lookup"><span data-stu-id="15684-157">Clip List XML customization</span></span>
<span data-ttu-id="15684-158">Megadhat hello klip lista XML hello munkafolyamat futásidőben használatával **transcodeSource** hello konfigurációban karakterlánc XML.</span><span class="sxs-lookup"><span data-stu-id="15684-158">You can specify hello Clip List XML in hello workflow at runtime by using **transcodeSource** in hello configuration string XML.</span></span> <span data-ttu-id="15684-159">Ehhez a hello klip lista XML PIN-kód csatlakoztatott toobe toohello forrás-adathordozó összetevő hello munkafolyamatban.</span><span class="sxs-lookup"><span data-stu-id="15684-159">This requires hello Clip List XML pin toobe connected toohello Media Source component in hello workflow.</span></span>

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <transcodeSource>
      <clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
      </clipList>
    </transcodeSource>
    <setRuntimeProperties>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

<span data-ttu-id="15684-160">Ha azt szeretné, hogy ez a tulajdonság tooname hello kimeneti fájlok "Kifejezések" segítségével toospecify /primarySourceFile toouse, akkor javasoljuk, hogy átadja hello klip lista XML-tulajdonságként *után* hello /primarySourceFile tulajdonság, tooavoid hello rendelkező klip lista felülbírálnia hello /primarySourceFile beállítást.</span><span class="sxs-lookup"><span data-stu-id="15684-160">If you want toospecify /primarySourceFile toouse this property tooname hello output files by using 'Expressions', then we recommend passing hello Clip List XML as a property *after* hello /primarySourceFile property, tooavoid having hello Clip List be overridden by hello /primarySourceFile setting.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

<span data-ttu-id="15684-161">A további keret pontos tisztítás:</span><span class="sxs-lookup"><span data-stu-id="15684-161">With additional frame-accurate trimming:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

## <a name="example-1--overlay-an-image-on-top-of-hello-video"></a><span data-ttu-id="15684-162">1. példa: Átfedő fölött videó hello kép</span><span class="sxs-lookup"><span data-stu-id="15684-162">Example 1 : Overlay an image on top of hello video</span></span>

### <a name="presentation"></a><span data-ttu-id="15684-163">Bemutató</span><span class="sxs-lookup"><span data-stu-id="15684-163">Presentation</span></span>
<span data-ttu-id="15684-164">Tekintse meg egy példát kívánt toooverlay egy embléma hello bemeneti videóhoz közben hello videó.</span><span class="sxs-lookup"><span data-stu-id="15684-164">Consider an example in which you want toooverlay a logo image on hello input video while hello video is encoded.</span></span> <span data-ttu-id="15684-165">Ebben a példában hello bemeneti videó "Microsoft_HoloLens_Possibilities_816p24.mp4" nevű és hello embléma neve "logo.png".</span><span class="sxs-lookup"><span data-stu-id="15684-165">In this example, hello input video is named "Microsoft_HoloLens_Possibilities_816p24.mp4" and hello logo is named "logo.png".</span></span> <span data-ttu-id="15684-166">Végre kell hajtania az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="15684-166">You should perform hello following steps:</span></span>

* <span data-ttu-id="15684-167">Hozzon létre egy munkafolyamat-eszközt a hello munkafolyamat fájlt (lásd a következő példa hello).</span><span class="sxs-lookup"><span data-stu-id="15684-167">Create a Workflow Asset with hello workflow file (see hello following example).</span></span>
* <span data-ttu-id="15684-168">Hozzon létre egy adathordozó eszköz, amely két fájlt tartalmaz: MyInputVideo.mp4, az elsődleges fájl- és MyLogo.png hello.</span><span class="sxs-lookup"><span data-stu-id="15684-168">Create a Media Asset, which contains two files: MyInputVideo.mp4 as hello primary file and MyLogo.png.</span></span>
* <span data-ttu-id="15684-169">Egy feladat toohello Media Encoder prémium munkafolyamat media processzor, a fenti hello bemeneti eszközök küldése, és adja meg a következő konfigurációs karakterlánc hello.</span><span class="sxs-lookup"><span data-stu-id="15684-169">Send a task toohello Media Encoder Premium Workflow media processor with hello above input assets and specify hello following configuration string.</span></span>

<span data-ttu-id="15684-170">A konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="15684-170">Configuration:</span></span>

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

<span data-ttu-id="15684-171">Hello a fenti példában a hello videofájl hello neve küldött toohello Media fájl bemeneti összetevő és hello primarySourceFile tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="15684-171">In hello example above, hello name of hello video file is sent toohello Media File Input component and hello primarySourceFile property.</span></span> <span data-ttu-id="15684-172">hello embléma fájl neve hello tooanother Media fájl bemeneti csatlakoztatott toohello grafikus átfedő összetevő által küldött.</span><span class="sxs-lookup"><span data-stu-id="15684-172">hello name of hello logo file is sent tooanother Media File Input that is connected toohello graphic overlay component.</span></span>

> [!NOTE]
> <span data-ttu-id="15684-173">hello videó fájlnév küldött toohello primarySourceFile tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="15684-173">hello video file name is sent toohello primarySourceFile property.</span></span> <span data-ttu-id="15684-174">Ennek az oka hello toouse hello munkafolyamat készítéséhez hello megfelelő kimeneti fájl nevét kifejezésekkel, például a tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="15684-174">hello reason for this is toouse this property in hello workflow for building hello correct output file name using Expressions, for example.</span></span>

### <a name="step-by-step-workflow-creation"></a><span data-ttu-id="15684-175">A munkafolyamat részletes létrehozása</span><span class="sxs-lookup"><span data-stu-id="15684-175">Step-by-step workflow creation</span></span>
<span data-ttu-id="15684-176">Az alábbiakban hello lépéseket toocreate olyan munkafolyamatot, amely két fájlt fogadja bemeneti adatként: videó és lemezképet.</span><span class="sxs-lookup"><span data-stu-id="15684-176">Here are hello steps toocreate a workflow that takes two files as input: a video and an image.</span></span> <span data-ttu-id="15684-177">Akkor lesz átfedő hello kép videó hello felett.</span><span class="sxs-lookup"><span data-stu-id="15684-177">It will overlay hello image on top of hello video.</span></span>

<span data-ttu-id="15684-178">Nyissa meg **munkafolyamat-Tervező** válassza **fájl** > **új munkaterület** > **átkódolására tervezetének**.</span><span class="sxs-lookup"><span data-stu-id="15684-178">Open **Workflow Designer** and select **File** > **New Workspace** > **Transcode Blueprint**.</span></span>

<span data-ttu-id="15684-179">Új munkafolyamat hello három elemeit tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="15684-179">hello new workflow shows three elements:</span></span>

* <span data-ttu-id="15684-180">Elsődleges forrásfájl</span><span class="sxs-lookup"><span data-stu-id="15684-180">Primary Source File</span></span>
* <span data-ttu-id="15684-181">XML klip listázása</span><span class="sxs-lookup"><span data-stu-id="15684-181">Clip List XML</span></span>
* <span data-ttu-id="15684-182">Kimeneti fájl vagy eszköz</span><span class="sxs-lookup"><span data-stu-id="15684-182">Output File/Asset</span></span>  

![Új kódolási munkafolyamat](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

<span data-ttu-id="15684-184">*Új kódolási munkafolyamat*</span><span class="sxs-lookup"><span data-stu-id="15684-184">*New Encoding Workflow*</span></span>

<span data-ttu-id="15684-185">Rendelés tooaccept hello bemeneti adathordozófájlba első lépésként adja hozzá az adathordozó fájl bemeneti összetevőt.</span><span class="sxs-lookup"><span data-stu-id="15684-185">In order tooaccept hello input media file, start with adding a Media File Input component.</span></span> <span data-ttu-id="15684-186">tooadd összetevő toohello munkafolyamat keressen hello tárház keresési mezőbe, majd szükséges hello bejegyzés húzza hello Tervező ablak.</span><span class="sxs-lookup"><span data-stu-id="15684-186">tooadd a component toohello workflow, look for it in hello Repository search box and drag hello desired entry onto hello designer pane.</span></span>

<span data-ttu-id="15684-187">Ezután adja hozzá a munkafolyamat tervezéséhez használt hello videofájl toobe.</span><span class="sxs-lookup"><span data-stu-id="15684-187">Next, add hello video file toobe used for designing your workflow.</span></span> <span data-ttu-id="15684-188">toodo tehát hello háttér ablaktábla munkafolyamat-tervezőben kattintson, és keresse meg hello elsődleges forrásfájl tulajdonság a hello tulajdonság jobb oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="15684-188">toodo so, click hello background pane in Workflow Designer and look for hello Primary Source File property on hello right-hand property pane.</span></span> <span data-ttu-id="15684-189">Hello mappa ikonra, és válassza ki a megfelelő videofájl hello.</span><span class="sxs-lookup"><span data-stu-id="15684-189">Click hello folder icon and select hello appropriate video file.</span></span>

![Elsődleges fájl forrás](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

<span data-ttu-id="15684-191">*Elsődleges fájl forrás*</span><span class="sxs-lookup"><span data-stu-id="15684-191">*Primary File Source*</span></span>

<span data-ttu-id="15684-192">Adja meg a következő hello videofájl hello Media fájl bemeneti összetevő.</span><span class="sxs-lookup"><span data-stu-id="15684-192">Next, specify hello video file in hello Media File Input component.</span></span>   

![A médiafájl bemeneti forrása](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

<span data-ttu-id="15684-194">*A médiafájl bemeneti forrása*</span><span class="sxs-lookup"><span data-stu-id="15684-194">*Media File Input Source*</span></span>

<span data-ttu-id="15684-195">Amint ez történik, hello Media fájl bemeneti összetevő hello fájl vizsgálata, és a kimeneti PIN-kódok tooreflect hello fájl, amely megvizsgálja az feltöltése.</span><span class="sxs-lookup"><span data-stu-id="15684-195">As soon as this is done, hello Media File Input component will inspect hello file and populate its output pins tooreflect hello file that it inspected.</span></span>

<span data-ttu-id="15684-196">hello következő lépés egy "Videó adatok típusa Frissítőjének" toospecify hello szín terület tooRec.709 tooadd.</span><span class="sxs-lookup"><span data-stu-id="15684-196">hello next step is tooadd a "Video Data Type Updater" toospecify hello color space tooRec.709.</span></span> <span data-ttu-id="15684-197">Adja hozzá a "videó konverter" tooData elrendezés/típusának beállított = síkbeli konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="15684-197">Add a "Video Format Converter" that is set tooData Layout/Layout type = Configurable Planar.</span></span> <span data-ttu-id="15684-198">A művelet konvertálja hello video-adatfolyamot tooa formátum hello átfedő összetevő forrásaként lehet tenni.</span><span class="sxs-lookup"><span data-stu-id="15684-198">This will convert hello video stream tooa format that can be taken as a source of hello overlay component.</span></span>

![videó típusú frissítési adatok és konverter](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

<span data-ttu-id="15684-200">*Videó adatok típusa Frissítőjének és konverter*</span><span class="sxs-lookup"><span data-stu-id="15684-200">*Video Data Type Updater and Format Converter*</span></span>

![Típusának konfigurálható síkbeli =](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

<span data-ttu-id="15684-202">*Elrendezés típus síkbeli konfigurálható:*</span><span class="sxs-lookup"><span data-stu-id="15684-202">*Layout type is Configurable Planar*</span></span>

<span data-ttu-id="15684-203">A következő videó átfedő összetevőt, és csatlakozzon a hello (tömörítetlen) video PIN-kód toohello (tömörítetlen) video PIN-kód hello media fájl bemeneti.</span><span class="sxs-lookup"><span data-stu-id="15684-203">Next, add a Video Overlay component and connect hello (uncompressed) video pin toohello (uncompressed) video pin of hello media file input.</span></span>

<span data-ttu-id="15684-204">Adja hozzá egy másik Media fájl bemeneti (tooload hello embléma fájl), kattintson a ezt az összetevőt, és nevezze át túl "Media fájl bemeneti embléma", és válassza a hello fájltulajdonság lemezkép (.png fájl például).</span><span class="sxs-lookup"><span data-stu-id="15684-204">Add another Media File Input (tooload hello logo file), click on this component and rename it too"Media File Input Logo", and select an image (a .png file for example) in hello file property.</span></span> <span data-ttu-id="15684-205">Csatlakozás hello átfedő hello tömörítetlen lemezkép tömörített kép PIN-kód toohello PIN kódját.</span><span class="sxs-lookup"><span data-stu-id="15684-205">Connect hello Uncompressed image pin toohello Uncompressed image pin of hello overlay.</span></span>

![Összetevő- és képfájlok forrásfájlt átfedő](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

<span data-ttu-id="15684-207">*Összetevő- és képfájlok forrásfájlt átfedő*</span><span class="sxs-lookup"><span data-stu-id="15684-207">*Overlay component and image file source*</span></span>

<span data-ttu-id="15684-208">Ha azt szeretné, hogy toomodify hello pozíciója a hello videó hello embléma (például érdemes tooposition 10 százalékát hello felső ki a bal oldali hello videó sarkában), törölje a jelet hello "Manuális bevitel" jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="15684-208">If you want toomodify hello position of hello logo on hello video (for example, you might want tooposition it at 10 percent off of hello top left corner of hello video), clear hello "Manual Input" check box.</span></span> <span data-ttu-id="15684-209">Ez végezhető el, mert a média fájl bemeneti tooprovide hello embléma fájl toohello átfedő összetevőt használ.</span><span class="sxs-lookup"><span data-stu-id="15684-209">You can do this because you are using a Media File Input tooprovide hello logo file toohello overlay component.</span></span>

![Átfedő pozíciója](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

<span data-ttu-id="15684-211">*Átfedő pozíciója*</span><span class="sxs-lookup"><span data-stu-id="15684-211">*Overlay position*</span></span>

<span data-ttu-id="15684-212">tooencode hello video-adatfolyamot tooH.264, vegye fel a hello AVC Videókódoló és AAC kódoló összetevők toohello Tervező felületére.</span><span class="sxs-lookup"><span data-stu-id="15684-212">tooencode hello video stream tooH.264, add hello AVC Video Encoder and AAC encoder components toohello designer surface.</span></span> <span data-ttu-id="15684-213">Csatlakozás hello PIN-kód.</span><span class="sxs-lookup"><span data-stu-id="15684-213">Connect hello pins.</span></span>
<span data-ttu-id="15684-214">Hello AAC kódoló beállítását, és hang formátum konverziós/készlet kiválasztása: 2.0 (L, R).</span><span class="sxs-lookup"><span data-stu-id="15684-214">Set up hello AAC encoder and select Audio Format Conversion/Preset : 2.0 (L, R).</span></span>

![Hang- és kódolók](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

<span data-ttu-id="15684-216">*Hang- és kódolók*</span><span class="sxs-lookup"><span data-stu-id="15684-216">*Audio and Video Encoders*</span></span>

<span data-ttu-id="15684-217">Ezután adja hozzá a hello **ISO Mpeg-4 Multiplexer** és **kimeneti fájl** összetevők és csatlakoztassa hello PIN-kód.</span><span class="sxs-lookup"><span data-stu-id="15684-217">Now add hello **ISO Mpeg-4 Multiplexer** and **File Output** components and connect hello pins as shown.</span></span>

![MP4 multiplexer és a kimeneti fájl](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

<span data-ttu-id="15684-219">*MP4 multiplexer és a kimeneti fájl*</span><span class="sxs-lookup"><span data-stu-id="15684-219">*MP4 multiplexer and file output*</span></span>

<span data-ttu-id="15684-220">Meg kell tooset hello hello kimeneti fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="15684-220">You need tooset hello name for hello output file.</span></span> <span data-ttu-id="15684-221">Kattintson a hello **kimeneti fájl** hello fájl összetevő és a Szerkesztés hello kifejezése:</span><span class="sxs-lookup"><span data-stu-id="15684-221">Click hello **File Output** component and edit hello expression for hello file:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Kimeneti fájlnév](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

<span data-ttu-id="15684-223">*Kimeneti fájlnév*</span><span class="sxs-lookup"><span data-stu-id="15684-223">*File output name*</span></span>

<span data-ttu-id="15684-224">Hello munkafolyamat futtatása helyileg toocheck, hogy fut-e megfelelően.</span><span class="sxs-lookup"><span data-stu-id="15684-224">You can run hello workflow locally toocheck that it is running correctly.</span></span>

<span data-ttu-id="15684-225">Miután a Befejezés után futtatható Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="15684-225">After it finishes, you can run it in Azure Media Services.</span></span>

<span data-ttu-id="15684-226">Először készítsen egy eszköz két fájlokat az Azure Media Services: hello videofájl és hello embléma.</span><span class="sxs-lookup"><span data-stu-id="15684-226">First, prepare an asset in Azure Media Services with two files in it: hello video file and hello logo.</span></span> <span data-ttu-id="15684-227">Ehhez hello .NET vagy a REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="15684-227">You can do this by using hello .NET or REST API.</span></span> <span data-ttu-id="15684-228">Is ehhez hello Azure-portál használatával vagy [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).</span><span class="sxs-lookup"><span data-stu-id="15684-228">You can also do this by using hello Azure portal or [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).</span></span>

<span data-ttu-id="15684-229">Az oktatóanyag bemutatja, hogyan AMSE toomanage eszközöket.</span><span class="sxs-lookup"><span data-stu-id="15684-229">This tutorial shows you how toomanage assets with AMSE.</span></span> <span data-ttu-id="15684-230">Két módon tooadd fájlok tooan eszköz van:</span><span class="sxs-lookup"><span data-stu-id="15684-230">There are two ways tooadd files tooan asset:</span></span>

* <span data-ttu-id="15684-231">Hozzon létre egy helyi mappát, másolja hello két fájlokat, és áthúzása hello mappa toohello **eszköz** fülre.</span><span class="sxs-lookup"><span data-stu-id="15684-231">Create a local folder, copy hello two files in it, and drag and drop hello folder toohello **Asset** tab.</span></span>
* <span data-ttu-id="15684-232">Eszközként hello videofájl feltöltése hello eszköz információk megjelenítése, nyissa meg toohello fájlok fülre, és töltse fel egy további fájlt (embléma).</span><span class="sxs-lookup"><span data-stu-id="15684-232">Upload hello video file as an asset, display hello asset information, go toohello files tab, and upload an additional file (logo).</span></span>

> [!NOTE]
> <span data-ttu-id="15684-233">Győződjön meg arról, hogy tooset elsődleges fájl hello eszközt (hello fő videofájl).</span><span class="sxs-lookup"><span data-stu-id="15684-233">Make sure tooset a primary file in hello asset (hello main video file).</span></span>

![Az AMSE eszköz fájlok](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

<span data-ttu-id="15684-235">*Az AMSE eszköz fájlok*</span><span class="sxs-lookup"><span data-stu-id="15684-235">*Asset files in AMSE*</span></span>

<span data-ttu-id="15684-236">Válassza ki a hello eszközt, és válassza ki a tooencode a prémium szintű kódolás.</span><span class="sxs-lookup"><span data-stu-id="15684-236">Select hello asset and choose tooencode it with Premium Encoder.</span></span> <span data-ttu-id="15684-237">Töltse fel a hello munkafolyamat, és válassza ki azt.</span><span class="sxs-lookup"><span data-stu-id="15684-237">Upload hello workflow and select it.</span></span>

<span data-ttu-id="15684-238">Hello toohello gomb toopass adatfeldolgozó kattintson, és adja hozzá a következő XML-tooset hello futásidejű tulajdonságok hello:</span><span class="sxs-lookup"><span data-stu-id="15684-238">Click hello button toopass data toohello processor, and add hello following XML tooset hello runtime properties:</span></span>

![Prémium szintű kódolás az AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

<span data-ttu-id="15684-240">*Prémium szintű kódolás az AMSE*</span><span class="sxs-lookup"><span data-stu-id="15684-240">*Premium Encoder in AMSE*</span></span>

<span data-ttu-id="15684-241">Majd illessze be a következő XML-adatok hello.</span><span class="sxs-lookup"><span data-stu-id="15684-241">Then, paste hello following XML data.</span></span> <span data-ttu-id="15684-242">Hello videofájl toospecify hello neve hello Media fájl bemeneti és primarySourceFile is szükség van.</span><span class="sxs-lookup"><span data-stu-id="15684-242">You need toospecify hello name of hello video file for both hello Media File Input and primarySourceFile.</span></span> <span data-ttu-id="15684-243">Adja meg hello hello embléma hello fájlnév túl.</span><span class="sxs-lookup"><span data-stu-id="15684-243">Specify hello name of hello file name for hello logo too.</span></span>

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

<span data-ttu-id="15684-245">*setRuntimeProperties*</span><span class="sxs-lookup"><span data-stu-id="15684-245">*setRuntimeProperties*</span></span>

<span data-ttu-id="15684-246">Ha hello .NET SDK toocreate használja, és hello feladat futtatása az XML-adatok toobe átadott hello konfigurációs karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="15684-246">If you use hello .NET SDK toocreate and run hello task, this XML data has toobe passed as hello configuration string.</span></span>

```c#
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

<span data-ttu-id="15684-247">Hello feladat befejezése után a hello MP4-fájlt a hello kimeneti eszköz megjeleníti hello átfedő!</span><span class="sxs-lookup"><span data-stu-id="15684-247">After hello job is complete, hello MP4 file in hello output asset displays hello overlay!</span></span>

![A videó hello átfedő](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

<span data-ttu-id="15684-249">*A videó hello átfedő*</span><span class="sxs-lookup"><span data-stu-id="15684-249">*Overlay on hello video*</span></span>

<span data-ttu-id="15684-250">Letöltheti a minta-munkafolyamat hello [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).</span><span class="sxs-lookup"><span data-stu-id="15684-250">You can download hello sample workflow from [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).</span></span>

## <a name="example-2--multiple-audio-language-encoding"></a><span data-ttu-id="15684-251">2. példa: Több nyelvi hang kódolás</span><span class="sxs-lookup"><span data-stu-id="15684-251">Example 2 : Multiple audio language encoding</span></span>

<span data-ttu-id="15684-252">Példa kódolási workfkow érhető el több hang nyelvi [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).</span><span class="sxs-lookup"><span data-stu-id="15684-252">An example of multiple audio language encoding workfkow is available in [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).</span></span>

<span data-ttu-id="15684-253">Ez a mappa tartalmaz egy minta-munkafolyamat egy MXF fájl tooa multi MP4 fájlok eszköz több zeneszámok használt tooencode lesz.</span><span class="sxs-lookup"><span data-stu-id="15684-253">This folder contains a sample workflow which can be used tooencode a MXF file tooa multi MP4 files asset with multiple audio tracks.</span></span>

<span data-ttu-id="15684-254">Ez a munkafolyamat azt feltételezi, hogy hello MXF fájl tartalmaz egy hang követése; hello további zeneszámok (WAV vagy MP4...) külön hang fájlokként kell átadni.</span><span class="sxs-lookup"><span data-stu-id="15684-254">This workflow assumes that hello MXF file contains one audio track ; hello additional audio tracks should be passed as seperate audio files (WAV or MP4...).</span></span>

<span data-ttu-id="15684-255">tooencode, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="15684-255">tooencode, please follow these steps:</span></span>

* <span data-ttu-id="15684-256">Hozzon létre egy Media Services eszközt a hello MXF fájl- és hello hang fájlok (0 too18 hang).</span><span class="sxs-lookup"><span data-stu-id="15684-256">Create a Media Services asset with hello MXF file and hello Audio files (0 too18 audio files).</span></span>
* <span data-ttu-id="15684-257">Győződjön meg arról, hogy hello MXF fájl elsődleges fájl be van állítva.</span><span class="sxs-lookup"><span data-stu-id="15684-257">Make sure that hello MXF file is set as a primary file.</span></span>
* <span data-ttu-id="15684-258">Hozzon létre egy feladat- és a prémium szintű munkafolyamat kódolás processzor hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="15684-258">Create a job and a task using hello Premium Workflow Encoder processor.</span></span> <span data-ttu-id="15684-259">A megadott (MultiMP4-1080p-19audio-v1.workflow) hello munkafolyamat használja.</span><span class="sxs-lookup"><span data-stu-id="15684-259">Use hello workflow provided (MultiMP4-1080p-19audio-v1.workflow).</span></span>
* <span data-ttu-id="15684-260">(Ha használja az Azure Media Services Explorer, használata hello "XML-adatok toohello munkafolyamata átadni" gombot), adja át hello setruntime.xml adatok toohello feladat.</span><span class="sxs-lookup"><span data-stu-id="15684-260">Pass hello setruntime.xml data toohello task (if you use Azure Media Services Explorer, use hello “pass xml data toohello workflow” button).</span></span>
  * <span data-ttu-id="15684-261">Frissítse a hello XML-adatok toospecify hello megfelelő fájl nevét és nyelvek címkék.</span><span class="sxs-lookup"><span data-stu-id="15684-261">Please update hello XML data toospecify hello correct file names and languages tags.</span></span>
  * <span data-ttu-id="15684-262">hello munkafolyamat nevű hang 1 tooAudio 18 hang részből áll.</span><span class="sxs-lookup"><span data-stu-id="15684-262">hello workflow has audio components named Audio 1 tooAudio 18.</span></span>
  * <span data-ttu-id="15684-263">RFC5646 nyelvcímkének hello támogatott.</span><span class="sxs-lookup"><span data-stu-id="15684-263">RFC5646 is supported for hello language tag.</span></span>

```xml
<?xml version="1.0" encoding="utf-16"?>
<transcodeRequest>
  <setRuntimeProperties>
    <property propertyPath="Media File Input Video/filename" value="MainVideo.mxf" />
    <property propertyPath="Language/language_code" value="en" />
    <property propertyPath="/primarySourceFile" value="MainVideo.mxf" />
    <property propertyPath="Audio 1/Media File Input/filename" value="french-audio.wav" />
    <property propertyPath="Audio 1/Language/language_code" value="fr" />
    <property propertyPath="Audio 2/Media File Input/filename" value="german-audio.wav" />
    <property propertyPath="Audio 2/Language/language_code" value="de" />
    <property propertyPath="Audio 3/Media File Input/filename" value="japanese-audio.wav" />
    <property propertyPath="Audio 3/Language/language_code" value="ja" />
  </setRuntimeProperties>
</transcodeRequest>
```

* <span data-ttu-id="15684-264">hello kódolású eszköz több nyelv zeneszámok fogja tartalmazni, és ezek nyomon követi az Azure Media Player választható kell lennie.</span><span class="sxs-lookup"><span data-stu-id="15684-264">hello encoded asset will contain multi language audio tracks and these tracks should be selectable in Azure Media Player.</span></span>

## <a name="see-also"></a><span data-ttu-id="15684-265">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="15684-265">See also</span></span>
* [<span data-ttu-id="15684-266">Prémium szintű kódolás az Azure Media Services bemutatása</span><span class="sxs-lookup"><span data-stu-id="15684-266">Introducing Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [<span data-ttu-id="15684-267">Hogyan prémium szintű Azure Media Services kódolási toouse</span><span class="sxs-lookup"><span data-stu-id="15684-267">How toouse Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [<span data-ttu-id="15684-268">Az Azure Media Services kódolási igény tartalom</span><span class="sxs-lookup"><span data-stu-id="15684-268">Encoding on-demand content with Azure Media Services</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)
* [<span data-ttu-id="15684-269">Media Encoder prémium szintű munkafolyamat-formátumok és kodekek</span><span class="sxs-lookup"><span data-stu-id="15684-269">Media Encoder Premium Workflow formats and codecs</span></span>](media-services-premium-workflow-encoder-formats.md)
* [<span data-ttu-id="15684-270">Minta munkafolyamat-fájlok</span><span class="sxs-lookup"><span data-stu-id="15684-270">Sample workflow files</span></span>](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)
* [<span data-ttu-id="15684-271">Azure Media Services Explorer eszköz</span><span class="sxs-lookup"><span data-stu-id="15684-271">Azure Media Services Explorer tool</span></span>](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a><span data-ttu-id="15684-272">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="15684-272">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="15684-273">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="15684-273">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
