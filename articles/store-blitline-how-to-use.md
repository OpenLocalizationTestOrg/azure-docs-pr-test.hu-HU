---
title: "Hogyan Blitline használandó kép feldolgozása - Azure szolgáltatás útmutató"
description: "Megtudhatja, hogyan használhatja az Azure alkalmazáshoz tartozó képek feldolgozásához Blitline szolgáltatást."
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
ms.openlocfilehash: 1d90599e028b3407a513b04b878e3aefc39928a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a><span data-ttu-id="0258f-103">Az Azure és az Azure Storage Blitline használata</span><span class="sxs-lookup"><span data-stu-id="0258f-103">How to use Blitline with Azure and Azure Storage</span></span>
<span data-ttu-id="0258f-104">Ez az útmutató alapján meghatározható, Blitline szolgáltatások eléréséhez és az Blitline feladatok elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="0258f-104">This guide will explain how to access Blitline services and how to submit jobs to Blitline.</span></span>

## <a name="what-is-blitline"></a><span data-ttu-id="0258f-105">Mi az a Blitline?</span><span class="sxs-lookup"><span data-stu-id="0258f-105">What is Blitline?</span></span>
<span data-ttu-id="0258f-106">Blitline a felhő alapú lemezkép feldolgozási szolgáltatás, amely biztosít a vállalati szintű kép feldolgozását építheti fel saját maga szeretné költség ár töredéke alatt.</span><span class="sxs-lookup"><span data-stu-id="0258f-106">Blitline is a cloud-based image processing service that provides enterprise level image processing at a fraction of the price that it would cost to build it yourself.</span></span>

<span data-ttu-id="0258f-107">Az a tény, hogy lemezkép nincs feldolgozva többször, általában az újonnan létrehozott minden webhely az alapoktól.</span><span class="sxs-lookup"><span data-stu-id="0258f-107">The fact is that image processing has been done over and over again, usually rebuilt from the ground up for each and every website.</span></span> <span data-ttu-id="0258f-108">A Microsoft vegye figyelembe, ez mivel azt létrehozott őket egy millió alkalommal túl.</span><span class="sxs-lookup"><span data-stu-id="0258f-108">We realize this because we’ve built them a million times too.</span></span> <span data-ttu-id="0258f-109">Egy nap döntöttünk, hogy lehet, hogy a rendszer idő azt csak erre mindenki számára.</span><span class="sxs-lookup"><span data-stu-id="0258f-109">One day we decided that perhaps it‘s time we just do it for everyone.</span></span> <span data-ttu-id="0258f-110">Tudjuk, hogyan kell tenni, akkor gyors és hatékony, és időközben mentése mindenki munkahelyi.</span><span class="sxs-lookup"><span data-stu-id="0258f-110">We know how to do it, to do it fast and efficiently, and save everyone work in the meantime.</span></span>

<span data-ttu-id="0258f-111">További információkért lásd: [http://www.blitline.com](http://www.blitline.com).</span><span class="sxs-lookup"><span data-stu-id="0258f-111">For more information, see [http://www.blitline.com](http://www.blitline.com).</span></span>

## <a name="what-blitline-is-not"></a><span data-ttu-id="0258f-112">Mi Blitline nincs...</span><span class="sxs-lookup"><span data-stu-id="0258f-112">What Blitline is NOT...</span></span>
<span data-ttu-id="0258f-113">Elmagyarázza, mit Blitline hasznos, ha, nem gyakran könnyebben azonosíthatja a Blitline nem mire soron előtt.</span><span class="sxs-lookup"><span data-stu-id="0258f-113">To clarify what Blitline is useful for, it is often easier to identify what Blitline does NOT do before moving forward.</span></span>

* <span data-ttu-id="0258f-114">Blitline nincs HTML widgeteket, hogy a képek feltöltése.</span><span class="sxs-lookup"><span data-stu-id="0258f-114">Blitline does NOT have HTML widgets to upload images.</span></span> <span data-ttu-id="0258f-115">Lemezképeket nyilvánosan vagy az elérni kívánt Blitline érhető el a korlátozott engedélyekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="0258f-115">You must have images available publicly or with restricted permissions available for Blitline to reach.</span></span>
* <span data-ttu-id="0258f-116">Blitline nem végez feldolgozást, például a Aviary.com élő kép</span><span class="sxs-lookup"><span data-stu-id="0258f-116">Blitline does NOT do live image processing, like Aviary.com</span></span>
* <span data-ttu-id="0258f-117">Blitline nem fogadja el a képek feltöltését, nem lehet leküldéssel terjeszteni a képek a Blitline közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="0258f-117">Blitline does NOT accept image uploads, you cannot push your images to Blitline directly.</span></span> <span data-ttu-id="0258f-118">Kell küldje le őket az Azure Storage vagy más helyen Blitline támogatja, és azokat hol Blitline beállítják.</span><span class="sxs-lookup"><span data-stu-id="0258f-118">You must push them to Azure Storage or other places Blitline supports and then tell Blitline where to go get them.</span></span>
* <span data-ttu-id="0258f-119">Blitline nagymértékben párhuzamos, és nem végez semmilyen szinkron feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="0258f-119">Blitline is massively parallel and does NOT do any synchronous processing.</span></span> <span data-ttu-id="0258f-120">Ezért meg kell biztosítják a postback_url, és azt segítségével megállapíthatja, hogy ha igazolnia befejezése feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="0258f-120">Meaning you must give us a postback_url and we can tell you when we are done processing.</span></span>

## <a name="create-a-blitline-account"></a><span data-ttu-id="0258f-121">Blitline-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="0258f-121">Create a Blitline account</span></span>
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a><span data-ttu-id="0258f-122">Egy Blitline feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="0258f-122">How to create a Blitline job</span></span>
<span data-ttu-id="0258f-123">Blitline JSON használja a műveletek végrehajtása a lemezkép kívánt adhatók meg.</span><span class="sxs-lookup"><span data-stu-id="0258f-123">Blitline uses JSON to define the actions you want to take on an image.</span></span> <span data-ttu-id="0258f-124">Ez a JSON néhány egyszerű mezők tevődik össze.</span><span class="sxs-lookup"><span data-stu-id="0258f-124">This JSON is composed of a few simple fields.</span></span>

<span data-ttu-id="0258f-125">A legegyszerűbb például a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="0258f-125">The simplest example is as follows:</span></span>

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

<span data-ttu-id="0258f-126">Itt a "src" lemezkép kezeléséért JSON tudunk "... boys.jpeg", majd a majd módosítsa az adott 240 x 140 lemezképet.</span><span class="sxs-lookup"><span data-stu-id="0258f-126">Here we have JSON that will take a "src" image "...boys.jpeg" and then resize that image to 240x140.</span></span>

<span data-ttu-id="0258f-127">Alkalmazásazonosító valami megtalálja a **KAPCSOLATINFORMÁCIÓ** vagy **kezelése** Azure fülre.</span><span class="sxs-lookup"><span data-stu-id="0258f-127">The Application ID is something you can find in your **CONNECTION INFO** or **MANAGE** tab on Azure.</span></span> <span data-ttu-id="0258f-128">A titkos azonosítója, amely lehetővé teszi a Blitline feladatok futtatása is.</span><span class="sxs-lookup"><span data-stu-id="0258f-128">It is your secret identifier that allows you to run jobs on Blitline.</span></span>

<span data-ttu-id="0258f-129">A "Mentés" paraméter azonosítja, ahol el szeretné helyezni a kép, ha azt feldolgozta kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="0258f-129">The "save" parameter identifies information about where you want to put the image once we have processed it.</span></span> <span data-ttu-id="0258f-130">A trivial esetben még nem meghatározott egyet.</span><span class="sxs-lookup"><span data-stu-id="0258f-130">In this trivial case, we haven't defined one.</span></span> <span data-ttu-id="0258f-131">Ha nincs megadva helykód Blitline fogja tárolni, helyileg (és ideiglenesen) egy egyedi felhő helyen.</span><span class="sxs-lookup"><span data-stu-id="0258f-131">If no location is defined Blitline will store it locally (and temporarily) at a unique cloud location.</span></span> <span data-ttu-id="0258f-132">Erre a helyre beszerezni a JSON Blitline által visszaadott, amikor a Blitline lehet.</span><span class="sxs-lookup"><span data-stu-id="0258f-132">You will be able to get that location from the JSON returned by Blitline when you make the Blitline.</span></span> <span data-ttu-id="0258f-133">A "lemezképpel" azonosító szükséges, ezért Önnek mentésekor azonosításához az adott képet ad vissza.</span><span class="sxs-lookup"><span data-stu-id="0258f-133">The "image" identifier is required and is returned to you when to identify this particular saved image.</span></span>

<span data-ttu-id="0258f-134">További információt talál arról a *funkciók* támogatjuk itt: <http://www.blitline.com/docs/functions></span><span class="sxs-lookup"><span data-stu-id="0258f-134">You can find more information about the *functions* we support here: <http://www.blitline.com/docs/functions></span></span>

<span data-ttu-id="0258f-135">A feladat beállítások itt dokumentációjában talál: <http://www.blitline.com/docs/api></span><span class="sxs-lookup"><span data-stu-id="0258f-135">You can also find documentation about the job options here: <http://www.blitline.com/docs/api></span></span>

<span data-ttu-id="0258f-136">Ha elvégezte a JSON csak annyit kell tennie az **POST** úgy, hogy`http://api.blitline.com/job`</span><span class="sxs-lookup"><span data-stu-id="0258f-136">Once you have your JSON all you need to do is **POST** it to `http://api.blitline.com/job`</span></span>

<span data-ttu-id="0258f-137">Az alábbihoz hasonló JSON vissza jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="0258f-137">You will get JSON back that looks something like this:</span></span>

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


<span data-ttu-id="0258f-138">Igen, akkor Blitline megkapta a kérelmet, azt állította a feldolgozási sorban, és befejezését követően a kép lesz elérhető legyen a következőn: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_azonosítója / CK3f0xBF_2bV6wf7gEZE8w.jpg**</span><span class="sxs-lookup"><span data-stu-id="0258f-138">This tells you that Blitline has recieved your request, it has put it in a processing queue, and when it has completed the image will be available at: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**</span></span>

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a><span data-ttu-id="0258f-139">A képfájl mentése az Azure Storage-fiókhoz</span><span class="sxs-lookup"><span data-stu-id="0258f-139">How to save an image to your Azure Storage account</span></span>
<span data-ttu-id="0258f-140">Ha egy Azure Storage-fiókot, könnyedén lehet Blitline a feldolgozott képek leküldéses az Azure-tárolóba.</span><span class="sxs-lookup"><span data-stu-id="0258f-140">If you have an Azure Storage account, you can easily have Blitline push the processed images into your Azure container.</span></span> <span data-ttu-id="0258f-141">Egy "azure_destination" hozzáadásával határozza meg a hely és a Blitline, amelyekkel a.</span><span class="sxs-lookup"><span data-stu-id="0258f-141">By adding an "azure_destination" you define the location and permissions for Blitline to push to.</span></span>

<span data-ttu-id="0258f-142">Például:</span><span class="sxs-lookup"><span data-stu-id="0258f-142">Here is an example:</span></span>

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


<span data-ttu-id="0258f-143">A saját CAPITALIZED értékek kitöltésével elküldheti a http://api.blitline.com/job a JSON és a "src" kép rendszer dolgozza fel a szűrőéhez, majd majd leküldött Azure célhelyre.</span><span class="sxs-lookup"><span data-stu-id="0258f-143">By filling in the CAPITALIZED values with your own, you can submit this JSON to http://api.blitline.com/job and the "src" image will be processed with a blur filter and then pushed to you Azure destination.</span></span>

### <a name="please-note"></a><span data-ttu-id="0258f-144">Megjegyzés:</span><span class="sxs-lookup"><span data-stu-id="0258f-144">Please note:</span></span>
<span data-ttu-id="0258f-145">A biztonsági Társítások teljes SAS URL-címét, beleértve a fájlnevet a célfájl kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="0258f-145">The SAS must contain the entire SAS url, including the filename of the destination file.</span></span>

<span data-ttu-id="0258f-146">Példa:</span><span class="sxs-lookup"><span data-stu-id="0258f-146">Example:</span></span>

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


<span data-ttu-id="0258f-147">A legújabb kiadása Blitline tartozó Azure Storage docs is olvasható [Itt](http://www.blitline.com/docs/azure_storage).</span><span class="sxs-lookup"><span data-stu-id="0258f-147">You can also read the latest edition of Blitline's Azure Storage docs [here](http://www.blitline.com/docs/azure_storage).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0258f-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0258f-148">Next Steps</span></span>
<span data-ttu-id="0258f-149">Látogasson el az egyéb funkciókkal kapcsolatos olvasni blitline.com:</span><span class="sxs-lookup"><span data-stu-id="0258f-149">Visit blitline.com to read about all our other features:</span></span>

* <span data-ttu-id="0258f-150">Blitline API-végpont Docs <http://www.blitline.com/docs/api></span><span class="sxs-lookup"><span data-stu-id="0258f-150">Blitline API Endpoint Docs <http://www.blitline.com/docs/api></span></span>
* <span data-ttu-id="0258f-151">Blitline API-függvények <http://www.blitline.com/docs/functions></span><span class="sxs-lookup"><span data-stu-id="0258f-151">Blitline API Functions <http://www.blitline.com/docs/functions></span></span>
* <span data-ttu-id="0258f-152">Blitline API példák <http://www.blitline.com/docs/examples></span><span class="sxs-lookup"><span data-stu-id="0258f-152">Blitline API Examples <http://www.blitline.com/docs/examples></span></span>
* <span data-ttu-id="0258f-153">Harmadik része a Nuget könyvtár <http://nuget.org/packages/Blitline.Net></span><span class="sxs-lookup"><span data-stu-id="0258f-153">Third Part Nuget Library <http://nuget.org/packages/Blitline.Net></span></span>

