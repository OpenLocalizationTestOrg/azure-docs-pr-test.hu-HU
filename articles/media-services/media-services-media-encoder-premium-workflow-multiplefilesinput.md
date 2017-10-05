---
title: "Több bemeneti fájlok és a prémium szintű kódolás - Azure összetevő tulajdonságai |} Microsoft Docs"
description: "Ez a témakör azt ismerteti, hogyan setRuntimeProperties segítségével több bemeneti fájlt használja, és egyéni adatok továbbítását a Media Encoder prémium munkafolyamat media processzor."
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
ms.openlocfilehash: df1ee5089a0af6ffce1431b658843fcb34a66ce5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a><span data-ttu-id="8e82f-103">A prémium szintű kódolás több bemeneti fájlok és összetevő tulajdonságai használja</span><span class="sxs-lookup"><span data-stu-id="8e82f-103">Using multiple input files and component properties with Premium Encoder</span></span>
## <a name="overview"></a><span data-ttu-id="8e82f-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8e82f-104">Overview</span></span>
<span data-ttu-id="8e82f-105">Forgatókönyv, ahol előfordulhat, hogy testreszabásához szükséges összetevő tulajdonságai, adja meg a klip lista XML-tartalom, vagy több bemeneti fájl küld, ha a feladat elküldése a **Media Encoder prémium munkafolyamat** media processzor.</span><span class="sxs-lookup"><span data-stu-id="8e82f-105">There are scenarios in which you might need to customize component properties, specify Clip List XML content, or send multiple input files when you submit a task with the **Media Encoder Premium Workflow** media processor.</span></span> <span data-ttu-id="8e82f-106">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="8e82f-106">Some examples are:</span></span>

* <span data-ttu-id="8e82f-107">Szöveg felirataként videó-és a szöveges értéket (például az aktuális dátum) beállítása az egyes bemeneti videó futásidőben.</span><span class="sxs-lookup"><span data-stu-id="8e82f-107">Overlaying text on video and setting the text value (for example, the current date) at runtime for each input video.</span></span>
* <span data-ttu-id="8e82f-108">Testreszabása az klip lista XML-(adjon meg egy vagy több forrásfájlokat, függetlenül a tisztítás, stb.).</span><span class="sxs-lookup"><span data-stu-id="8e82f-108">Customizing the Clip List XML (to specify one or several source files, with or without trimming, etc.).</span></span>
* <span data-ttu-id="8e82f-109">A bemeneti videóhoz a egy embléma felirataként, amíg a videó.</span><span class="sxs-lookup"><span data-stu-id="8e82f-109">Overlaying a logo image on the input video while the video is encoded.</span></span>
* <span data-ttu-id="8e82f-110">Több hang nyelvi kódolást.</span><span class="sxs-lookup"><span data-stu-id="8e82f-110">Multiple audio language encoding.</span></span>

<span data-ttu-id="8e82f-111">Ahhoz, hogy a **Media Encoder prémium munkafolyamat** tudja, hogy néhány tulajdonság a munkafolyamat több bemeneti fájlküldésre vagy a feladat létrehozásakor módosítani, kell használni, amely tartalmazza a konfigurációs karakterlánc **setRuntimeProperties** és/vagy **transcodeSource**.</span><span class="sxs-lookup"><span data-stu-id="8e82f-111">To let the **Media Encoder Premium Workflow** know that you are changing some properties in the workflow when you create the task or send multiple input files, you have to use a configuration string that contains **setRuntimeProperties** and/or **transcodeSource**.</span></span> <span data-ttu-id="8e82f-112">Ez a témakör azt ismerteti, hogyan is használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="8e82f-112">This topic explains how to use them.</span></span>

## <a name="configuration-string-syntax"></a><span data-ttu-id="8e82f-113">Konfigurációs karakterlánc-formátum:</span><span class="sxs-lookup"><span data-stu-id="8e82f-113">Configuration string syntax</span></span>
<span data-ttu-id="8e82f-114">A konfigurációs karakterláncot a kódolási feladat használja az XML-dokumentum, amely a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="8e82f-114">The configuration string to set in the encoding task uses an XML document that looks like this:</span></span>

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

<span data-ttu-id="8e82f-115">A következő egy C#-kódban, amely fájlból olvassa be az XML-konfiguráció, a jobb oldali videó fájlnév frissíti, és átadja a feladatot a feladatok:</span><span class="sxs-lookup"><span data-stu-id="8e82f-115">The following is the C# code that reads the XML configuration from a file, update it with the right video filename and passes it to the task in a job:</span></span>

```c#
string premiumConfiguration = ReadAllText(@"D:\home\site\wwwroot\Presets\SetRuntime.xml").Replace("VideoFileName", myVideoFileName);

// Declare a new job.
IJob job = _context.Jobs.Create("Premium Workflow encoding job");

// Get a media processor reference, and pass to it the name of the 
// processor to use for the specific task.
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

// Create a task with the encoding details, using a string preset.
ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                              processor,
                              premiumConfiguration,
                              TaskOptions.None);

// Specify the input assets
task.InputAssets.Add(workflow); // workflow asset
task.InputAssets.Add(video); // video asset with multiple files

// Add an output asset to contain the results of the job. 
// This output is specified as AssetCreationOptions.None, which 
// means the output asset is not encrypted. 
task.OutputAssets.AddNew("Output asset", AssetCreationOptions.None);
```

## <a name="customizing-component-properties"></a><span data-ttu-id="8e82f-116">Összetevő tulajdonságai testreszabása</span><span class="sxs-lookup"><span data-stu-id="8e82f-116">Customizing component properties</span></span>
### <a name="property-with-a-simple-value"></a><span data-ttu-id="8e82f-117">Egyszerű értéke</span><span class="sxs-lookup"><span data-stu-id="8e82f-117">Property with a simple value</span></span>
<span data-ttu-id="8e82f-118">Bizonyos esetekben célszerű egy összetevő tulajdonság, és a munkafolyamat-fájlt, amelyet szeretne hajthatják végre a Media Encoder prémium munkafolyamat testreszabása.</span><span class="sxs-lookup"><span data-stu-id="8e82f-118">In some cases, it is useful to customize a component property together with the workflow file that is going to be executed by Media Encoder Premium Workflow.</span></span>

<span data-ttu-id="8e82f-119">Tegyük fel, akkor a munkafolyamat átfedések szöveg a videók, és a következő futtatási állítható be a szöveget (például az aktuális dátum) kellene.</span><span class="sxs-lookup"><span data-stu-id="8e82f-119">Suppose you designed a workflow that overlays text on your videos, and the text (for example, the current date) is supposed to be set at runtime.</span></span> <span data-ttu-id="8e82f-120">Ehhez a kódolási feladat a állítható be az új értéket az átmeneti területre összetevő text tulajdonságához szöveg küldésével.</span><span class="sxs-lookup"><span data-stu-id="8e82f-120">You can do this by sending the text to be set as the new value for the text property of the overlay component from the encoding task.</span></span> <span data-ttu-id="8e82f-121">A mechanizmus segítségével más egy összetevő (például pozícióját vagy az átmeneti területre színe, az átviteli AVC kódoló, stb.) a munkafolyamat tulajdonságainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="8e82f-121">You can use this mechanism to change other properties of a component in the workflow (such as the position or color of the overlay, the bitrate of the AVC encoder, etc.).</span></span>

<span data-ttu-id="8e82f-122">**setRuntimeProperties** felül a munkafolyamat-összetevők tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8e82f-122">**setRuntimeProperties** is used to override a property in the components of the workflow.</span></span>

<span data-ttu-id="8e82f-123">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e82f-123">Example:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
      <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
  </setRuntimeProperties>
</transcodeRequest>
```

### <a name="property-with-an-xml-value"></a><span data-ttu-id="8e82f-124">Az XML-értéke tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8e82f-124">Property with an XML value</span></span>
<span data-ttu-id="8e82f-125">Olyan tulajdonságon, amely vár egy XML-érték beállításához foglalják magukban a `<![CDATA[ and ]]>`.</span><span class="sxs-lookup"><span data-stu-id="8e82f-125">To set a property that expects an XML value, encapsulate by using `<![CDATA[ and ]]>`.</span></span>

<span data-ttu-id="8e82f-126">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e82f-126">Example:</span></span>

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
> <span data-ttu-id="8e82f-127">Ügyeljen arra, hogy nem kocsivissza visszatérési put után csak `<![CDATA[`.</span><span class="sxs-lookup"><span data-stu-id="8e82f-127">Make sure not to put a carriage return just after `<![CDATA[`.</span></span>

### <a name="propertypath-value"></a><span data-ttu-id="8e82f-128">a propertyPath érték</span><span class="sxs-lookup"><span data-stu-id="8e82f-128">propertyPath value</span></span>
<span data-ttu-id="8e82f-129">A fenti példákban a propertyPath lett "/ Media File bemeneti/fájlnév" vagy "/ inactiveTimeout" vagy "clipListXml".</span><span class="sxs-lookup"><span data-stu-id="8e82f-129">In the previous examples, the propertyPath was "/Media File Input/filename" or "/inactiveTimeout" or "clipListXml".</span></span>
<span data-ttu-id="8e82f-130">Ez az általános, az összetevő neve, majd a tulajdonságnevet kell megadni.</span><span class="sxs-lookup"><span data-stu-id="8e82f-130">This is, in general, the name of the component, then the name of the property.</span></span> <span data-ttu-id="8e82f-131">Az elérési út is van több vagy kevesebb, mint "/ primarySourceFile" (mert a tulajdonság a munkafolyamat gyökerében) vagy "/ videó feldolgozási/kép átfedő/átlátszatlanság" (mivel az átmeneti területre csoportban).</span><span class="sxs-lookup"><span data-stu-id="8e82f-131">The path can have more or fewer levels, like "/primarySourceFile" (because the property is at the root of the workflow) or "/Video Processing/Graphic Overlay/Opacity" (because the Overlay is in a group).</span></span>    

<span data-ttu-id="8e82f-132">Ellenőrizze az elérési út és a tulajdonság nevét, használja az akciógombra kattinthat, amely közvetlenül mellett minden egyes tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="8e82f-132">To check the path and property name, use the action button that is immediately beside each property.</span></span> <span data-ttu-id="8e82f-133">A művelet gombra kattintva, és válassza ki **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="8e82f-133">You can click this action button and select **Edit**.</span></span> <span data-ttu-id="8e82f-134">Ez azt mutatja majd, a tényleges nevét, a tulajdonság, és azonnal felette, a névtér.</span><span class="sxs-lookup"><span data-stu-id="8e82f-134">This will show you the actual name of the property, and immediately above it, the namespace.</span></span>

![A művelet/szerkesztése](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Tulajdonság](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a><span data-ttu-id="8e82f-137">Több bemeneti fájl</span><span class="sxs-lookup"><span data-stu-id="8e82f-137">Multiple input files</span></span>
<span data-ttu-id="8e82f-138">Minden tevékenység elküldött a **Media Encoder prémium munkafolyamat** két eszközök igényel:</span><span class="sxs-lookup"><span data-stu-id="8e82f-138">Each task that you submit to the **Media Encoder Premium Workflow** requires two assets:</span></span>

* <span data-ttu-id="8e82f-139">Az első egy egy *munkafolyamat eszköz* , amely a munkafolyamat-fájlt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8e82f-139">The first one is a *Workflow Asset* that contains a workflow file.</span></span> <span data-ttu-id="8e82f-140">Munkafolyamat-fájlok segítségével megtervezheti a [munkafolyamat-Tervező](media-services-workflow-designer.md).</span><span class="sxs-lookup"><span data-stu-id="8e82f-140">You can design workflow files by using the [Workflow Designer](media-services-workflow-designer.md).</span></span>
* <span data-ttu-id="8e82f-141">A második érték van egy *Media eszköz* , amely tartalmazza a media (oka) t, amelyet szeretne kódolni.</span><span class="sxs-lookup"><span data-stu-id="8e82f-141">The second one is a *Media Asset* that contains the media file(s) that you want to encode.</span></span>

<span data-ttu-id="8e82f-142">Ha több médiafájlok küldjük a **Media Encoder prémium munkafolyamat** kódoló, a következő korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="8e82f-142">When you're sending multiple media files to the **Media Encoder Premium Workflow** encoder, the following constraints apply:</span></span>

* <span data-ttu-id="8e82f-143">A médiafájlokat kell lennie ugyanazon *Media eszköz*.</span><span class="sxs-lookup"><span data-stu-id="8e82f-143">All the media files must be in the same *Media Asset*.</span></span> <span data-ttu-id="8e82f-144">Több adathordozó eszközök használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="8e82f-144">Using multiple Media Assets is not supported.</span></span>
* <span data-ttu-id="8e82f-145">Meg kell adnia az elsődleges fájlnak a Media eszköz (ideális esetben azt a fő videofájl, amely a kódoló kapcsolatba kell feldolgozni).</span><span class="sxs-lookup"><span data-stu-id="8e82f-145">You must set the primary file in this Media Asset (ideally, this is the main video file that the encoder is asked to process).</span></span>
* <span data-ttu-id="8e82f-146">Szükséges konfigurációs adatok, amely tartalmazza a **setRuntimeProperties** és/vagy **transcodeSource** elemben, amely a processzor.</span><span class="sxs-lookup"><span data-stu-id="8e82f-146">It is necessary to pass configuration data that includes the **setRuntimeProperties** and/or **transcodeSource** element to the processor.</span></span>
  * <span data-ttu-id="8e82f-147">**setRuntimeProperties** felül a filename tulajdonságban vagy az összetevők a munkafolyamat egy másik tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="8e82f-147">**setRuntimeProperties** is used to override the filename property or another property in the components of the workflow.</span></span>
  * <span data-ttu-id="8e82f-148">**transcodeSource** klip lista XML-tartalom szolgál.</span><span class="sxs-lookup"><span data-stu-id="8e82f-148">**transcodeSource** is used to specify the Clip List XML content.</span></span>

<span data-ttu-id="8e82f-149">A munkafolyamat kapcsolatok:</span><span class="sxs-lookup"><span data-stu-id="8e82f-149">Connections in the workflow:</span></span>

* <span data-ttu-id="8e82f-150">Ha egy vagy több adathordozó fájl bemeneti összetevőket használnak, és tervezi használni az **setRuntimeProperties** adja meg a fájlnevet, majd nem csatlakozzon az elsődleges fájl összetevő PIN-kód őket.</span><span class="sxs-lookup"><span data-stu-id="8e82f-150">If you use one or several Media File Input components and plan to use **setRuntimeProperties** to specify the file name, then do not connect the primary file component pin to them.</span></span> <span data-ttu-id="8e82f-151">Győződjön meg arról, hogy nincs-e az elsődleges fájl objektum és az adathordozó fájl Input(s) közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="8e82f-151">Make sure that there is no connection between the primary file object and the Media File Input(s).</span></span>
* <span data-ttu-id="8e82f-152">Ha szeretné használni a klip lista XML és egy forrás-adathordozó összetevőt, majd összekapcsolhatja mindkét együtt.</span><span class="sxs-lookup"><span data-stu-id="8e82f-152">If you prefer to use Clip List XML and one Media Source component, then you can connect both together.</span></span>

![Nincs kapcsolat elsődleges forrásfájlból Media fájl bemeneti](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

<span data-ttu-id="8e82f-154">*Nincs Media fájl bemeneti elemét/elemeit az elsődleges fájlnak a kapcsolat beállítása a filename tulajdonságban setRuntimeProperties használatakor.*</span><span class="sxs-lookup"><span data-stu-id="8e82f-154">*There is no connection from the primary file to Media File Input component(s) if you use setRuntimeProperties to set the filename property.*</span></span>

![XML-forráslista levágása klip listából kapcsolat](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

<span data-ttu-id="8e82f-156">*Forrás-adathordozó klip lista XML csatlakozni, és transcodeSource használja.*</span><span class="sxs-lookup"><span data-stu-id="8e82f-156">*You can connect Clip List XML to Media Source and use transcodeSource.*</span></span>

### <a name="clip-list-xml-customization"></a><span data-ttu-id="8e82f-157">Lista XML testreszabási levágása</span><span class="sxs-lookup"><span data-stu-id="8e82f-157">Clip List XML customization</span></span>
<span data-ttu-id="8e82f-158">Megadhatja a klip lista XML futásidőben a munkafolyamatban használatával **transcodeSource** konfigurációjában karakterlánc XML.</span><span class="sxs-lookup"><span data-stu-id="8e82f-158">You can specify the Clip List XML in the workflow at runtime by using **transcodeSource** in the configuration string XML.</span></span> <span data-ttu-id="8e82f-159">Ehhez a klip lista XML PIN-kódot kell csatlakoztatni a forrás-adathordozó összetevő a munkafolyamatban.</span><span class="sxs-lookup"><span data-stu-id="8e82f-159">This requires the Clip List XML pin to be connected to the Media Source component in the workflow.</span></span>

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

<span data-ttu-id="8e82f-160">Ha meg szeretné határozni /primarySourceFile használni ezt a tulajdonságot "Kifejezések" használatával, a kimeneti fájlok nevét, akkor javasoljuk, hogy átadja a klip lista XML-tulajdonságként *után* a /primarySourceFile tulajdonság kívánja kerülni a /primarySourceFile beállításával bírálható klip listájában.</span><span class="sxs-lookup"><span data-stu-id="8e82f-160">If you want to specify /primarySourceFile to use this property to name the output files by using 'Expressions', then we recommend passing the Clip List XML as a property *after* the /primarySourceFile property, to avoid having the Clip List be overridden by the /primarySourceFile setting.</span></span>

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

<span data-ttu-id="8e82f-161">A további keret pontos tisztítás:</span><span class="sxs-lookup"><span data-stu-id="8e82f-161">With additional frame-accurate trimming:</span></span>

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

## <a name="example-1--overlay-an-image-on-top-of-the-video"></a><span data-ttu-id="8e82f-162">1. példa: Átfedő fölött a videó kép</span><span class="sxs-lookup"><span data-stu-id="8e82f-162">Example 1 : Overlay an image on top of the video</span></span>

### <a name="presentation"></a><span data-ttu-id="8e82f-163">Bemutató</span><span class="sxs-lookup"><span data-stu-id="8e82f-163">Presentation</span></span>
<span data-ttu-id="8e82f-164">Vegye figyelembe például szeretné egy embléma lemezképét a bemeneti videóhoz átfedő, amíg a videó.</span><span class="sxs-lookup"><span data-stu-id="8e82f-164">Consider an example in which you want to overlay a logo image on the input video while the video is encoded.</span></span> <span data-ttu-id="8e82f-165">Ebben a példában a bemeneti videó "Microsoft_HoloLens_Possibilities_816p24.mp4" nevű, és az embléma neve "logo.png".</span><span class="sxs-lookup"><span data-stu-id="8e82f-165">In this example, the input video is named "Microsoft_HoloLens_Possibilities_816p24.mp4" and the logo is named "logo.png".</span></span> <span data-ttu-id="8e82f-166">Az alábbi lépéseket kell végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="8e82f-166">You should perform the following steps:</span></span>

* <span data-ttu-id="8e82f-167">Hozzon létre egy munkafolyamat-eszközt a a munkafolyamat-fájlt (lásd a következő példát).</span><span class="sxs-lookup"><span data-stu-id="8e82f-167">Create a Workflow Asset with the workflow file (see the following example).</span></span>
* <span data-ttu-id="8e82f-168">Hozzon létre egy adathordozó eszköz, amely két fájlt tartalmaz: az elsődleges fájl és MyLogo.png MyInputVideo.mp4.</span><span class="sxs-lookup"><span data-stu-id="8e82f-168">Create a Media Asset, which contains two files: MyInputVideo.mp4 as the primary file and MyLogo.png.</span></span>
* <span data-ttu-id="8e82f-169">A feladat elküldése a Media Encoder prémium munkafolyamat media processzor, a fenti bemeneti eszközök, és adja meg a következő konfigurációs karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8e82f-169">Send a task to the Media Encoder Premium Workflow media processor with the above input assets and specify the following configuration string.</span></span>

<span data-ttu-id="8e82f-170">A konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="8e82f-170">Configuration:</span></span>

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

<span data-ttu-id="8e82f-171">A fenti példában a videó fájl neve elküldi az adathordozó fájl bemeneti összetevő és a primarySourceFile tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8e82f-171">In the example above, the name of the video file is sent to the Media File Input component and the primarySourceFile property.</span></span> <span data-ttu-id="8e82f-172">Az embléma fájl nevét, amely a grafikus átfedő összetevő kapcsolódik egy másik Media fájl bemeneti zajlik.</span><span class="sxs-lookup"><span data-stu-id="8e82f-172">The name of the logo file is sent to another Media File Input that is connected to the graphic overlay component.</span></span>

> [!NOTE]
> <span data-ttu-id="8e82f-173">A videó fájlnév nem jut hozzá a primarySourceFile tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8e82f-173">The video file name is sent to the primarySourceFile property.</span></span> <span data-ttu-id="8e82f-174">A hiba okát is használják ezt a tulajdonságot a munkafolyamat készítéséhez a megfelelő kimeneti fájl nevét kifejezésekkel, például.</span><span class="sxs-lookup"><span data-stu-id="8e82f-174">The reason for this is to use this property in the workflow for building the correct output file name using Expressions, for example.</span></span>

### <a name="step-by-step-workflow-creation"></a><span data-ttu-id="8e82f-175">A munkafolyamat részletes létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e82f-175">Step-by-step workflow creation</span></span>
<span data-ttu-id="8e82f-176">Az alábbiakban olyan munkafolyamatot, amely két fájlt fogadja bemeneti adatként létrehozásának lépései: videó és lemezképet.</span><span class="sxs-lookup"><span data-stu-id="8e82f-176">Here are the steps to create a workflow that takes two files as input: a video and an image.</span></span> <span data-ttu-id="8e82f-177">A kép fölött a videó azt fogja átfedő.</span><span class="sxs-lookup"><span data-stu-id="8e82f-177">It will overlay the image on top of the video.</span></span>

<span data-ttu-id="8e82f-178">Nyissa meg **munkafolyamat-Tervező** válassza **fájl** > **új munkaterület** > **átkódolására tervezetének**.</span><span class="sxs-lookup"><span data-stu-id="8e82f-178">Open **Workflow Designer** and select **File** > **New Workspace** > **Transcode Blueprint**.</span></span>

<span data-ttu-id="8e82f-179">Az új munkafolyamat három elemeit tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8e82f-179">The new workflow shows three elements:</span></span>

* <span data-ttu-id="8e82f-180">Elsődleges forrásfájl</span><span class="sxs-lookup"><span data-stu-id="8e82f-180">Primary Source File</span></span>
* <span data-ttu-id="8e82f-181">XML klip listázása</span><span class="sxs-lookup"><span data-stu-id="8e82f-181">Clip List XML</span></span>
* <span data-ttu-id="8e82f-182">Kimeneti fájl vagy eszköz</span><span class="sxs-lookup"><span data-stu-id="8e82f-182">Output File/Asset</span></span>  

![Új kódolási munkafolyamat](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

<span data-ttu-id="8e82f-184">*Új kódolási munkafolyamat*</span><span class="sxs-lookup"><span data-stu-id="8e82f-184">*New Encoding Workflow*</span></span>

<span data-ttu-id="8e82f-185">Ahhoz, hogy fogadja el a bemeneti médiafájl, első lépésként adja hozzá az adathordozó fájl bemeneti összetevőt.</span><span class="sxs-lookup"><span data-stu-id="8e82f-185">In order to accept the input media file, start with adding a Media File Input component.</span></span> <span data-ttu-id="8e82f-186">Vegyen fel összetevőt a munkafolyamathoz, keresse meg azt a tárház keresési mezőbe, és a kívánt bejegyzés húzza a Tervező ablak.</span><span class="sxs-lookup"><span data-stu-id="8e82f-186">To add a component to the workflow, look for it in the Repository search box and drag the desired entry onto the designer pane.</span></span>

<span data-ttu-id="8e82f-187">Ezután adja hozzá a video-fájl a munkafolyamat tervezéséhez.</span><span class="sxs-lookup"><span data-stu-id="8e82f-187">Next, add the video file to be used for designing your workflow.</span></span> <span data-ttu-id="8e82f-188">Ehhez az szükséges, kattintson a háttér panelre munkafolyamat-tervezővel, és keresse meg az elsődleges forrásfájl tulajdonság, a jobb oldali tulajdonság panelen.</span><span class="sxs-lookup"><span data-stu-id="8e82f-188">To do so, click the background pane in Workflow Designer and look for the Primary Source File property on the right-hand property pane.</span></span> <span data-ttu-id="8e82f-189">A mappa ikonra, és válassza ki a megfelelő video-fájlt.</span><span class="sxs-lookup"><span data-stu-id="8e82f-189">Click the folder icon and select the appropriate video file.</span></span>

![Elsődleges fájl forrás](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

<span data-ttu-id="8e82f-191">*Elsődleges fájl forrás*</span><span class="sxs-lookup"><span data-stu-id="8e82f-191">*Primary File Source*</span></span>

<span data-ttu-id="8e82f-192">Ezt követően adja meg a videofájl a Media fájl bemeneti összetevő.</span><span class="sxs-lookup"><span data-stu-id="8e82f-192">Next, specify the video file in the Media File Input component.</span></span>   

![A médiafájl bemeneti forrása](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

<span data-ttu-id="8e82f-194">*A médiafájl bemeneti forrása*</span><span class="sxs-lookup"><span data-stu-id="8e82f-194">*Media File Input Source*</span></span>

<span data-ttu-id="8e82f-195">Amint ez történik, az adathordozó fájl bemeneti összetevő vizsgálhatja meg a fájl, és a kimeneti PIN-kód megfelelően a fájlt, amely megvizsgálja az feltöltése.</span><span class="sxs-lookup"><span data-stu-id="8e82f-195">As soon as this is done, the Media File Input component will inspect the file and populate its output pins to reflect the file that it inspected.</span></span>

<span data-ttu-id="8e82f-196">A következő lépés hozzáadása egy "videó adatok típusa Frissítőjének" adhatja meg a Rec.709 szín terület.</span><span class="sxs-lookup"><span data-stu-id="8e82f-196">The next step is to add a "Video Data Type Updater" to specify the color space to Rec.709.</span></span> <span data-ttu-id="8e82f-197">Adja hozzá a "videó konverter" adatok elrendezés/elrendezés típusra van állítva = síkbeli konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="8e82f-197">Add a "Video Format Converter" that is set to Data Layout/Layout type = Configurable Planar.</span></span> <span data-ttu-id="8e82f-198">Ez a video-adatfolyamot átalakítása átvihető az átmeneti területre összetevő forrásaként formátumú.</span><span class="sxs-lookup"><span data-stu-id="8e82f-198">This will convert the video stream to a format that can be taken as a source of the overlay component.</span></span>

![videó típusú frissítési adatok és konverter](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

<span data-ttu-id="8e82f-200">*Videó adatok típusa Frissítőjének és konverter*</span><span class="sxs-lookup"><span data-stu-id="8e82f-200">*Video Data Type Updater and Format Converter*</span></span>

![Típusának konfigurálható síkbeli =](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

<span data-ttu-id="8e82f-202">*Elrendezés típus síkbeli konfigurálható:*</span><span class="sxs-lookup"><span data-stu-id="8e82f-202">*Layout type is Configurable Planar*</span></span>

<span data-ttu-id="8e82f-203">A következő videó átfedő összetevő hozzáadása, és a (tömörítetlen) video PIN-kód csatlakozzon a media fájl bemeneti (tömörítetlen) video PIN kódját.</span><span class="sxs-lookup"><span data-stu-id="8e82f-203">Next, add a Video Overlay component and connect the (uncompressed) video pin to the (uncompressed) video pin of the media file input.</span></span>

<span data-ttu-id="8e82f-204">Egy másik Media fájl bemenete (betölteni az embléma fájlt), vegye fel ezt az összetevőt parancsára, és nevezze át a "Media fájl bemeneti embléma", és jelölje ki a képfájl (például .png fájl) a fájl tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8e82f-204">Add another Media File Input (to load the logo file), click on this component and rename it to "Media File Input Logo", and select an image (a .png file for example) in the file property.</span></span> <span data-ttu-id="8e82f-205">A tömörített kép PIN-kód csatlakozni a tömörített kép PIN-kódot az átfedés.</span><span class="sxs-lookup"><span data-stu-id="8e82f-205">Connect the Uncompressed image pin to the Uncompressed image pin of the overlay.</span></span>

![Összetevő- és képfájlok forrásfájlt átfedő](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

<span data-ttu-id="8e82f-207">*Összetevő- és képfájlok forrásfájlt átfedő*</span><span class="sxs-lookup"><span data-stu-id="8e82f-207">*Overlay component and image file source*</span></span>

<span data-ttu-id="8e82f-208">Ha a pozíciót a videót a módosítani kívánt (például érdemes helyezze el a 10 százaléka ki a videó bal felső sarkában), törölje a jelet az "Manuális bevitel" jelölőnégyzetből.</span><span class="sxs-lookup"><span data-stu-id="8e82f-208">If you want to modify the position of the logo on the video (for example, you might want to position it at 10 percent off of the top left corner of the video), clear the "Manual Input" check box.</span></span> <span data-ttu-id="8e82f-209">Ez végezhető el, mert egy Media fájl bemeneti segítségével adja meg az átmeneti területre összetevőre embléma fájlját.</span><span class="sxs-lookup"><span data-stu-id="8e82f-209">You can do this because you are using a Media File Input to provide the logo file to the overlay component.</span></span>

![Átfedő pozíciója](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

<span data-ttu-id="8e82f-211">*Átfedő pozíciója*</span><span class="sxs-lookup"><span data-stu-id="8e82f-211">*Overlay position*</span></span>

<span data-ttu-id="8e82f-212">A video-adatfolyamot H.264 kódolására, adja hozzá a AVC Videókódoló és AAC kódoló összetevőket a Tervező felületére.</span><span class="sxs-lookup"><span data-stu-id="8e82f-212">To encode the video stream to H.264, add the AVC Video Encoder and AAC encoder components to the designer surface.</span></span> <span data-ttu-id="8e82f-213">Csatlakoztassa a PIN-kód.</span><span class="sxs-lookup"><span data-stu-id="8e82f-213">Connect the pins.</span></span>
<span data-ttu-id="8e82f-214">Állítsa be a AAC kódoló, és válassza ki a hang formátum konverziós/készlet: 2.0 (L, R).</span><span class="sxs-lookup"><span data-stu-id="8e82f-214">Set up the AAC encoder and select Audio Format Conversion/Preset : 2.0 (L, R).</span></span>

![Hang- és kódolók](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

<span data-ttu-id="8e82f-216">*Hang- és kódolók*</span><span class="sxs-lookup"><span data-stu-id="8e82f-216">*Audio and Video Encoders*</span></span>

<span data-ttu-id="8e82f-217">Ezután adja hozzá a **ISO Mpeg-4 Multiplexer** és **kimeneti fájl** összetevők és csatlakoztassa a PIN-kód.</span><span class="sxs-lookup"><span data-stu-id="8e82f-217">Now add the **ISO Mpeg-4 Multiplexer** and **File Output** components and connect the pins as shown.</span></span>

![MP4 multiplexer és a kimeneti fájl](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

<span data-ttu-id="8e82f-219">*MP4 multiplexer és a kimeneti fájl*</span><span class="sxs-lookup"><span data-stu-id="8e82f-219">*MP4 multiplexer and file output*</span></span>

<span data-ttu-id="8e82f-220">Állítsa be a kimeneti fájl nevét kell.</span><span class="sxs-lookup"><span data-stu-id="8e82f-220">You need to set the name for the output file.</span></span> <span data-ttu-id="8e82f-221">Kattintson a **kimeneti fájl** összetevő és a kifejezést a fájl szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="8e82f-221">Click the **File Output** component and edit the expression for the file:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Kimeneti fájlnév](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

<span data-ttu-id="8e82f-223">*Kimeneti fájlnév*</span><span class="sxs-lookup"><span data-stu-id="8e82f-223">*File output name*</span></span>

<span data-ttu-id="8e82f-224">A munkafolyamat győződjön meg arról, hogy megfelelően fut-e el helyileg is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="8e82f-224">You can run the workflow locally to check that it is running correctly.</span></span>

<span data-ttu-id="8e82f-225">Miután a Befejezés után futtatható Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="8e82f-225">After it finishes, you can run it in Azure Media Services.</span></span>

<span data-ttu-id="8e82f-226">Először készítsen egy eszköz két fájlokat az Azure Media Services: a videofájl és az emblémát.</span><span class="sxs-lookup"><span data-stu-id="8e82f-226">First, prepare an asset in Azure Media Services with two files in it: the video file and the logo.</span></span> <span data-ttu-id="8e82f-227">Ehhez a .NET vagy a REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="8e82f-227">You can do this by using the .NET or REST API.</span></span> <span data-ttu-id="8e82f-228">Is ehhez az Azure portál használatával vagy [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).</span><span class="sxs-lookup"><span data-stu-id="8e82f-228">You can also do this by using the Azure portal or [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).</span></span>

<span data-ttu-id="8e82f-229">Ez az oktatóanyag bemutatja, hogyan AMSE rendelkező eszközök kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="8e82f-229">This tutorial shows you how to manage assets with AMSE.</span></span> <span data-ttu-id="8e82f-230">Fájlok hozzáadása egy eszköz két módja van:</span><span class="sxs-lookup"><span data-stu-id="8e82f-230">There are two ways to add files to an asset:</span></span>

* <span data-ttu-id="8e82f-231">Hozzon létre egy helyi mappát, másolja a két fájlt, és húzással a mappát a **eszköz** fülre.</span><span class="sxs-lookup"><span data-stu-id="8e82f-231">Create a local folder, copy the two files in it, and drag and drop the folder to the **Asset** tab.</span></span>
* <span data-ttu-id="8e82f-232">Eszközként a videofájl feltöltése, az eszköz adatait jeleníti meg, lépjen a fájlok lapra és töltse fel egy további fájlt (embléma).</span><span class="sxs-lookup"><span data-stu-id="8e82f-232">Upload the video file as an asset, display the asset information, go to the files tab, and upload an additional file (logo).</span></span>

> [!NOTE]
> <span data-ttu-id="8e82f-233">Feltétlenül állítson be egy elsődleges fájl az adategységben (a fő videofájl).</span><span class="sxs-lookup"><span data-stu-id="8e82f-233">Make sure to set a primary file in the asset (the main video file).</span></span>

![Az AMSE eszköz fájlok](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

<span data-ttu-id="8e82f-235">*Az AMSE eszköz fájlok*</span><span class="sxs-lookup"><span data-stu-id="8e82f-235">*Asset files in AMSE*</span></span>

<span data-ttu-id="8e82f-236">Válassza ki az objektumot, és válassza a prémium szintű kódolás kódolása.</span><span class="sxs-lookup"><span data-stu-id="8e82f-236">Select the asset and choose to encode it with Premium Encoder.</span></span> <span data-ttu-id="8e82f-237">Töltse fel a munkafolyamat, és válassza ki azt.</span><span class="sxs-lookup"><span data-stu-id="8e82f-237">Upload the workflow and select it.</span></span>

<span data-ttu-id="8e82f-238">A gombra kattintva adatokat adnak át a processzor, és adja hozzá a következő XML futásidejű tulajdonságainak beállításához:</span><span class="sxs-lookup"><span data-stu-id="8e82f-238">Click the button to pass data to the processor, and add the following XML to set the runtime properties:</span></span>

![Prémium szintű kódolás az AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

<span data-ttu-id="8e82f-240">*Prémium szintű kódolás az AMSE*</span><span class="sxs-lookup"><span data-stu-id="8e82f-240">*Premium Encoder in AMSE*</span></span>

<span data-ttu-id="8e82f-241">Majd illessze be a következő XML-adataiban.</span><span class="sxs-lookup"><span data-stu-id="8e82f-241">Then, paste the following XML data.</span></span> <span data-ttu-id="8e82f-242">Meg kell adnia a videó fájl nevét az adathordozó fájl bemeneti és a primarySourceFile.</span><span class="sxs-lookup"><span data-stu-id="8e82f-242">You need to specify the name of the video file for both the Media File Input and primarySourceFile.</span></span> <span data-ttu-id="8e82f-243">Adja meg a fájl neve az embléma túl.</span><span class="sxs-lookup"><span data-stu-id="8e82f-243">Specify the name of the file name for the logo too.</span></span>

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

<span data-ttu-id="8e82f-245">*setRuntimeProperties*</span><span class="sxs-lookup"><span data-stu-id="8e82f-245">*setRuntimeProperties*</span></span>

<span data-ttu-id="8e82f-246">Ha a .NET SDK használatával hozzon létre, és futtatni a feladatot, az XML-adatok ki lesznek átadva, a konfigurációs karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="8e82f-246">If you use the .NET SDK to create and run the task, this XML data has to be passed as the configuration string.</span></span>

```c#
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

<span data-ttu-id="8e82f-247">A feladat befejezése után a kimeneti adategységen MP4 fájlban megjeleníti az átmeneti területre!</span><span class="sxs-lookup"><span data-stu-id="8e82f-247">After the job is complete, the MP4 file in the output asset displays the overlay!</span></span>

![A videó az átfedő](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

<span data-ttu-id="8e82f-249">*A videó az átfedő*</span><span class="sxs-lookup"><span data-stu-id="8e82f-249">*Overlay on the video*</span></span>

<span data-ttu-id="8e82f-250">Letöltheti a minta-munkafolyamat [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).</span><span class="sxs-lookup"><span data-stu-id="8e82f-250">You can download the sample workflow from [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).</span></span>

## <a name="example-2--multiple-audio-language-encoding"></a><span data-ttu-id="8e82f-251">2. példa: Több nyelvi hang kódolás</span><span class="sxs-lookup"><span data-stu-id="8e82f-251">Example 2 : Multiple audio language encoding</span></span>

<span data-ttu-id="8e82f-252">Példa kódolási workfkow érhető el több hang nyelvi [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).</span><span class="sxs-lookup"><span data-stu-id="8e82f-252">An example of multiple audio language encoding workfkow is available in [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).</span></span>

<span data-ttu-id="8e82f-253">Ez a mappa tartalmaz egy minta-munkafolyamat egy többszörös MP4 fájlok eszköz több zeneszámok MXF fájlt kódolni használható.</span><span class="sxs-lookup"><span data-stu-id="8e82f-253">This folder contains a sample workflow which can be used to encode a MXF file to a multi MP4 files asset with multiple audio tracks.</span></span>

<span data-ttu-id="8e82f-254">A munkafolyamat azt feltételezi, hogy a MXF fájl tartalmazza-e egy hang követése; a további zeneszámok (WAV vagy MP4...) külön hang fájlokként kell átadni.</span><span class="sxs-lookup"><span data-stu-id="8e82f-254">This workflow assumes that the MXF file contains one audio track ; the additional audio tracks should be passed as seperate audio files (WAV or MP4...).</span></span>

<span data-ttu-id="8e82f-255">Kódolására, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="8e82f-255">To encode, please follow these steps:</span></span>

* <span data-ttu-id="8e82f-256">Hozzon létre egy Media Services eszköz a MXF, és a hang fájlokat (0 – 18 hangfájlok).</span><span class="sxs-lookup"><span data-stu-id="8e82f-256">Create a Media Services asset with the MXF file and the Audio files (0 to 18 audio files).</span></span>
* <span data-ttu-id="8e82f-257">Győződjön meg arról, hogy a MXF fájl be van állítva az elsődleges fájl.</span><span class="sxs-lookup"><span data-stu-id="8e82f-257">Make sure that the MXF file is set as a primary file.</span></span>
* <span data-ttu-id="8e82f-258">Hozzon létre egy feladatot, és egy feladatot, a prémium szintű munkafolyamat kódolás processzor használatával.</span><span class="sxs-lookup"><span data-stu-id="8e82f-258">Create a job and a task using the Premium Workflow Encoder processor.</span></span> <span data-ttu-id="8e82f-259">A megadott munkafolyamat (MultiMP4-1080p-19audio-v1.workflow) használja.</span><span class="sxs-lookup"><span data-stu-id="8e82f-259">Use the workflow provided (MultiMP4-1080p-19audio-v1.workflow).</span></span>
* <span data-ttu-id="8e82f-260">A setruntime.xml adatokat adnak át a feladat (Azure Media Services Explorer használatakor használja az "XML-adatok átadása a munkafolyamat" gombot).</span><span class="sxs-lookup"><span data-stu-id="8e82f-260">Pass the setruntime.xml data to the task (if you use Azure Media Services Explorer, use the “pass xml data to the workflow” button).</span></span>
  * <span data-ttu-id="8e82f-261">Frissítse a helyes nevek és nyelvek címkék adhatók meg az XML-adatok.</span><span class="sxs-lookup"><span data-stu-id="8e82f-261">Please update the XML data to specify the correct file names and languages tags.</span></span>
  * <span data-ttu-id="8e82f-262">A munkafolyamat hang 18-ra hang 1 nevű hang részből áll.</span><span class="sxs-lookup"><span data-stu-id="8e82f-262">The workflow has audio components named Audio 1 to Audio 18.</span></span>
  * <span data-ttu-id="8e82f-263">A nyelvcímkének RFC5646 esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="8e82f-263">RFC5646 is supported for the language tag.</span></span>

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

* <span data-ttu-id="8e82f-264">A kódolt objektumhoz több nyelv zeneszámok fogja tartalmazni, és ezek nyomon követi az Azure Media Player választható kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8e82f-264">The encoded asset will contain multi language audio tracks and these tracks should be selectable in Azure Media Player.</span></span>

## <a name="see-also"></a><span data-ttu-id="8e82f-265">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="8e82f-265">See also</span></span>
* [<span data-ttu-id="8e82f-266">Prémium szintű kódolás az Azure Media Services bemutatása</span><span class="sxs-lookup"><span data-stu-id="8e82f-266">Introducing Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [<span data-ttu-id="8e82f-267">Azure Media Services használata a prémium szintű kódolás</span><span class="sxs-lookup"><span data-stu-id="8e82f-267">How to use Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [<span data-ttu-id="8e82f-268">Az Azure Media Services kódolási igény tartalom</span><span class="sxs-lookup"><span data-stu-id="8e82f-268">Encoding on-demand content with Azure Media Services</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)
* [<span data-ttu-id="8e82f-269">Media Encoder prémium szintű munkafolyamat-formátumok és kodekek</span><span class="sxs-lookup"><span data-stu-id="8e82f-269">Media Encoder Premium Workflow formats and codecs</span></span>](media-services-premium-workflow-encoder-formats.md)
* [<span data-ttu-id="8e82f-270">Minta munkafolyamat-fájlok</span><span class="sxs-lookup"><span data-stu-id="8e82f-270">Sample workflow files</span></span>](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)
* [<span data-ttu-id="8e82f-271">Azure Media Services Explorer eszköz</span><span class="sxs-lookup"><span data-stu-id="8e82f-271">Azure Media Services Explorer tool</span></span>](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a><span data-ttu-id="8e82f-272">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="8e82f-272">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="8e82f-273">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="8e82f-273">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
