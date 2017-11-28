---
title: "aaaHow toouse Blitline kép feldolgozásra - Azure szolgáltatás útmutató"
description: "Ismerje meg, hogyan toouse hello Blitline szolgáltatást az Azure-alkalmazások tooprocess képek."
services: 
documentationcenter: .net
author: blitline-dev
manager: jason@blitline.com
editor: jason@blitline.com
ms.assetid: 6c711248-0e52-4895-ba9e-8395628de924
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2014
ms.author: support@blitline.com
ms.openlocfilehash: 328fd177e25f45f29f8ad8e142d02b46017a858e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blitline-with-azure-and-azure-storage"></a><span data-ttu-id="1765e-103">Hogyan toouse Blitline Azure és az Azure tárolással</span><span class="sxs-lookup"><span data-stu-id="1765e-103">How toouse Blitline with Azure and Azure Storage</span></span>
<span data-ttu-id="1765e-104">Ez az útmutató alapján meghatározható, hogyan tooaccess Blitline szolgáltatásokat, és hogyan toosubmit feladatok tooBlitline.</span><span class="sxs-lookup"><span data-stu-id="1765e-104">This guide will explain how tooaccess Blitline services and how toosubmit jobs tooBlitline.</span></span>

## <a name="what-is-blitline"></a><span data-ttu-id="1765e-105">Mi az a Blitline?</span><span class="sxs-lookup"><span data-stu-id="1765e-105">What is Blitline?</span></span>
<span data-ttu-id="1765e-106">Blitline a felhő alapú lemezkép feldolgozási szolgáltatás, amely egy azon részét, hogy toobuild volna áron hello ár vállalati szintű kép feldolgozását, saját magának.</span><span class="sxs-lookup"><span data-stu-id="1765e-106">Blitline is a cloud-based image processing service that provides enterprise level image processing at a fraction of hello price that it would cost toobuild it yourself.</span></span>

<span data-ttu-id="1765e-107">hello tény, hogy lemezkép nincs feldolgozva többször, általában az újonnan létrehozott minden webhely hello alapoktól.</span><span class="sxs-lookup"><span data-stu-id="1765e-107">hello fact is that image processing has been done over and over again, usually rebuilt from hello ground up for each and every website.</span></span> <span data-ttu-id="1765e-108">A Microsoft vegye figyelembe, ez mivel azt létrehozott őket egy millió alkalommal túl.</span><span class="sxs-lookup"><span data-stu-id="1765e-108">We realize this because we’ve built them a million times too.</span></span> <span data-ttu-id="1765e-109">Egy nap döntöttünk, hogy lehet, hogy a rendszer idő azt csak erre mindenki számára.</span><span class="sxs-lookup"><span data-stu-id="1765e-109">One day we decided that perhaps it‘s time we just do it for everyone.</span></span> <span data-ttu-id="1765e-110">Tudjuk, hogyan toodo azt, akkor gyors és hatékony, és mentse mindenki működni a hello addig toodo.</span><span class="sxs-lookup"><span data-stu-id="1765e-110">We know how toodo it, toodo it fast and efficiently, and save everyone work in hello meantime.</span></span>

<span data-ttu-id="1765e-111">További információkért lásd: [http://www.blitline.com](http://www.blitline.com).</span><span class="sxs-lookup"><span data-stu-id="1765e-111">For more information, see [http://www.blitline.com](http://www.blitline.com).</span></span>

## <a name="what-blitline-is-not"></a><span data-ttu-id="1765e-112">Mi Blitline nincs...</span><span class="sxs-lookup"><span data-stu-id="1765e-112">What Blitline is NOT...</span></span>
<span data-ttu-id="1765e-113">tooclarify mi Blitline a hasznos, ez gyakran könnyebb tooidentify Blitline nem mire soron előtt.</span><span class="sxs-lookup"><span data-stu-id="1765e-113">tooclarify what Blitline is useful for, it is often easier tooidentify what Blitline does NOT do before moving forward.</span></span>

* <span data-ttu-id="1765e-114">Blitline nincs HTML widgeteket tooupload képek.</span><span class="sxs-lookup"><span data-stu-id="1765e-114">Blitline does NOT have HTML widgets tooupload images.</span></span> <span data-ttu-id="1765e-115">Lemezképeket nyilvánosan vagy Blitline tooreach érhető el a korlátozott engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1765e-115">You must have images available publicly or with restricted permissions available for Blitline tooreach.</span></span>
* <span data-ttu-id="1765e-116">Blitline nem végez feldolgozást, például a Aviary.com élő kép</span><span class="sxs-lookup"><span data-stu-id="1765e-116">Blitline does NOT do live image processing, like Aviary.com</span></span>
* <span data-ttu-id="1765e-117">Blitline nem fogadja el a képek feltöltését, közvetlenül nem lehet leküldéssel terjeszteni a képek tooBlitline.</span><span class="sxs-lookup"><span data-stu-id="1765e-117">Blitline does NOT accept image uploads, you cannot push your images tooBlitline directly.</span></span> <span data-ttu-id="1765e-118">Kell küldje le őket tooAzure tároló vagy más helyen Blitline támogatja, és majd kérje meg a Blitline, ahol toogo megkapja őket.</span><span class="sxs-lookup"><span data-stu-id="1765e-118">You must push them tooAzure Storage or other places Blitline supports and then tell Blitline where toogo get them.</span></span>
* <span data-ttu-id="1765e-119">Blitline nagymértékben párhuzamos, és nem végez semmilyen szinkron feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="1765e-119">Blitline is massively parallel and does NOT do any synchronous processing.</span></span> <span data-ttu-id="1765e-120">Ezért meg kell biztosítják a postback_url, és azt segítségével megállapíthatja, hogy ha igazolnia befejezése feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="1765e-120">Meaning you must give us a postback_url and we can tell you when we are done processing.</span></span>

## <a name="create-a-blitline-account"></a><span data-ttu-id="1765e-121">Blitline-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="1765e-121">Create a Blitline account</span></span>
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-toocreate-a-blitline-job"></a><span data-ttu-id="1765e-122">Hogyan toocreate Blitline feladat</span><span class="sxs-lookup"><span data-stu-id="1765e-122">How toocreate a Blitline job</span></span>
<span data-ttu-id="1765e-123">Blitline JSON-toodefine hello műveleteket, amelyeket a lemezkép tootake használja.</span><span class="sxs-lookup"><span data-stu-id="1765e-123">Blitline uses JSON toodefine hello actions you want tootake on an image.</span></span> <span data-ttu-id="1765e-124">Ez a JSON néhány egyszerű mezők tevődik össze.</span><span class="sxs-lookup"><span data-stu-id="1765e-124">This JSON is composed of a few simple fields.</span></span>

<span data-ttu-id="1765e-125">hello legegyszerűbb például a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="1765e-125">hello simplest example is as follows:</span></span>

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

<span data-ttu-id="1765e-126">Itt a "src" lemezkép kezeléséért JSON tudunk "... boys.jpeg" és a lemezkép too240x140 majd átméretezése.</span><span class="sxs-lookup"><span data-stu-id="1765e-126">Here we have JSON that will take a "src" image "...boys.jpeg" and then resize that image too240x140.</span></span>

<span data-ttu-id="1765e-127">hello Alkalmazásazonosító valami megtalálja a **KAPCSOLATINFORMÁCIÓ** vagy **kezelése** Azure fülre.</span><span class="sxs-lookup"><span data-stu-id="1765e-127">hello Application ID is something you can find in your **CONNECTION INFO** or **MANAGE** tab on Azure.</span></span> <span data-ttu-id="1765e-128">A titkos azonosítója, amely lehetővé teszi a Blitline toorun feladatok is.</span><span class="sxs-lookup"><span data-stu-id="1765e-128">It is your secret identifier that allows you toorun jobs on Blitline.</span></span>

<span data-ttu-id="1765e-129">"Mentés" paraméter hello hol szeretné tooput hello kép után azt feldolgozta azonosítja.</span><span class="sxs-lookup"><span data-stu-id="1765e-129">hello "save" parameter identifies information about where you want tooput hello image once we have processed it.</span></span> <span data-ttu-id="1765e-130">A trivial esetben még nem meghatározott egyet.</span><span class="sxs-lookup"><span data-stu-id="1765e-130">In this trivial case, we haven't defined one.</span></span> <span data-ttu-id="1765e-131">Ha nincs megadva helykód Blitline fogja tárolni, helyileg (és ideiglenesen) egy egyedi felhő helyen.</span><span class="sxs-lookup"><span data-stu-id="1765e-131">If no location is defined Blitline will store it locally (and temporarily) at a unique cloud location.</span></span> <span data-ttu-id="1765e-132">A hely, ahonnan hello JSON Blitline által visszaadott, amikor hello Blitline képes tooget lesz.</span><span class="sxs-lookup"><span data-stu-id="1765e-132">You will be able tooget that location from hello JSON returned by Blitline when you make hello Blitline.</span></span> <span data-ttu-id="1765e-133">hello "lemezképpel" azonosító szükség esetén és tooyou mentésekor tooidentify az adott képet.</span><span class="sxs-lookup"><span data-stu-id="1765e-133">hello "image" identifier is required and is returned tooyou when tooidentify this particular saved image.</span></span>

<span data-ttu-id="1765e-134">További információ a hello található *funkciók* támogatjuk itt: <http://www.blitline.com/docs/functions></span><span class="sxs-lookup"><span data-stu-id="1765e-134">You can find more information about hello *functions* we support here: <http://www.blitline.com/docs/functions></span></span>

<span data-ttu-id="1765e-135">Is találhat hello kapcsolatos feladat beállítások itt: <http://www.blitline.com/docs/api></span><span class="sxs-lookup"><span data-stu-id="1765e-135">You can also find documentation about hello job options here: <http://www.blitline.com/docs/api></span></span>

<span data-ttu-id="1765e-136">Ha elvégezte a JSON toodo van szüksége **POST** azt túl`http://api.blitline.com/job`</span><span class="sxs-lookup"><span data-stu-id="1765e-136">Once you have your JSON all you need toodo is **POST** it too`http://api.blitline.com/job`</span></span>

<span data-ttu-id="1765e-137">Az alábbihoz hasonló JSON vissza jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="1765e-137">You will get JSON back that looks something like this:</span></span>

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


<span data-ttu-id="1765e-138">Igen, akkor Blitline megkapta a kérelmet, azt állította a feldolgozási sorban, és befejezését követően érhető el lesz-e a lemezkép hello: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_azonosítója /CK3f0xBF_2bV6wf7gEZE8w.jpg**</span><span class="sxs-lookup"><span data-stu-id="1765e-138">This tells you that Blitline has recieved your request, it has put it in a processing queue, and when it has completed hello image will be available at: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**</span></span>

## <a name="how-toosave-an-image-tooyour-azure-storage-account"></a><span data-ttu-id="1765e-139">Hogyan toosave egy kép tooyour Azure Storage-fiókban</span><span class="sxs-lookup"><span data-stu-id="1765e-139">How toosave an image tooyour Azure Storage account</span></span>
<span data-ttu-id="1765e-140">Ha egy Azure Storage-fiókot, könnyedén lehet Blitline leküldéses feldolgozott hello képek az Azure-tárolóba.</span><span class="sxs-lookup"><span data-stu-id="1765e-140">If you have an Azure Storage account, you can easily have Blitline push hello processed images into your Azure container.</span></span> <span data-ttu-id="1765e-141">Egy "azure_destination" hozzáadásával meghatározása a hello helyét és a Blitline toopush engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="1765e-141">By adding an "azure_destination" you define hello location and permissions for Blitline toopush to.</span></span>

<span data-ttu-id="1765e-142">Például:</span><span class="sxs-lookup"><span data-stu-id="1765e-142">Here is an example:</span></span>

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


<span data-ttu-id="1765e-143">Hello CAPITALIZED értékeket a saját kitöltésével elküldheti a JSON toohttp://api.blitline.com/job és hello "src" kép életlenítés szűrő dolgoz fel, és majd leküldött tooyou Azure cél.</span><span class="sxs-lookup"><span data-stu-id="1765e-143">By filling in hello CAPITALIZED values with your own, you can submit this JSON toohttp://api.blitline.com/job and hello "src" image will be processed with a blur filter and then pushed tooyou Azure destination.</span></span>

### <a name="please-note"></a><span data-ttu-id="1765e-144">Megjegyzés:</span><span class="sxs-lookup"><span data-stu-id="1765e-144">Please note:</span></span>
<span data-ttu-id="1765e-145">hello SAS hello teljes SAS URL-cím, beleértve a célfájl hello hello fájlnevet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="1765e-145">hello SAS must contain hello entire SAS url, including hello filename of hello destination file.</span></span>

<span data-ttu-id="1765e-146">Példa:</span><span class="sxs-lookup"><span data-stu-id="1765e-146">Example:</span></span>

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


<span data-ttu-id="1765e-147">Is olvasható Blitline tartozó Azure Storage docs legújabb kiadása hello [Itt](http://www.blitline.com/docs/azure_storage).</span><span class="sxs-lookup"><span data-stu-id="1765e-147">You can also read hello latest edition of Blitline's Azure Storage docs [here](http://www.blitline.com/docs/azure_storage).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1765e-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1765e-148">Next Steps</span></span>
<span data-ttu-id="1765e-149">Látogasson el az egyéb funkciókkal kapcsolatos blitline.com tooread:</span><span class="sxs-lookup"><span data-stu-id="1765e-149">Visit blitline.com tooread about all our other features:</span></span>

* <span data-ttu-id="1765e-150">Blitline API-végpont Docs <http://www.blitline.com/docs/api></span><span class="sxs-lookup"><span data-stu-id="1765e-150">Blitline API Endpoint Docs <http://www.blitline.com/docs/api></span></span>
* <span data-ttu-id="1765e-151">Blitline API-függvények <http://www.blitline.com/docs/functions></span><span class="sxs-lookup"><span data-stu-id="1765e-151">Blitline API Functions <http://www.blitline.com/docs/functions></span></span>
* <span data-ttu-id="1765e-152">Blitline API példák <http://www.blitline.com/docs/examples></span><span class="sxs-lookup"><span data-stu-id="1765e-152">Blitline API Examples <http://www.blitline.com/docs/examples></span></span>
* <span data-ttu-id="1765e-153">Harmadik része a Nuget könyvtár <http://nuget.org/packages/Blitline.Net></span><span class="sxs-lookup"><span data-stu-id="1765e-153">Third Part Nuget Library <http://nuget.org/packages/Blitline.Net></span></span>

