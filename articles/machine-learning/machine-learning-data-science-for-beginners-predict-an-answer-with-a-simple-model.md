---
title: "egyszerű regressziós modell - Azure Machine Learning választ aaaPredict |} Microsoft Docs"
description: "Hogyan toocreate egy egyszerű regressziós modell toopredict Adattudomány videó 4 kezdőknek ár. Egy lineáris regressziós cél adatokat tartalmazza."
keywords: "a modell, egyszerű modell, árának előrejelzése, egyszerű regressziós modell létrehozása"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: a28f1fab-e2d8-4663-aa7d-ca3530c8b525
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: d4270c2237c33b7e898b78a08b292bc9d62e49ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="predict-an-answer-with-a-simple-model"></a><span data-ttu-id="f3327-105">Válasz előrejelzése egy egyszerű modell segítségével</span><span class="sxs-lookup"><span data-stu-id="f3327-105">Predict an answer with a simple model</span></span>
## <a name="video-4-data-science-for-beginners-series"></a><span data-ttu-id="f3327-106">4. Videó: Adattudomány kezdők sorozat</span><span class="sxs-lookup"><span data-stu-id="f3327-106">Video 4: Data Science for Beginners series</span></span>
<span data-ttu-id="f3327-107">Ismerje meg, hogyan toocreate egy egyszerű regressziós modell toopredict hello egy rombusz a videó 4 kezdőknek Adattudomány árát.</span><span class="sxs-lookup"><span data-stu-id="f3327-107">Learn how toocreate a simple regression model toopredict hello price of a diamond in Data Science for Beginners video 4.</span></span> <span data-ttu-id="f3327-108">Azt fogja megrajzolásához egy regressziós modell cél adatokkal.</span><span class="sxs-lookup"><span data-stu-id="f3327-108">We'll draw a regression model with target data.</span></span>

<span data-ttu-id="f3327-109">tooget hello hatékonyabb működtetését hello adatsorozat, tekintse meg azokat.</span><span class="sxs-lookup"><span data-stu-id="f3327-109">tooget hello most out of hello series, watch them all.</span></span> <span data-ttu-id="f3327-110">[Nyissa meg a videók toohello listája](#other-videos-in-this-series)
</span><span class="sxs-lookup"><span data-stu-id="f3327-110">[Go toohello list of videos](#other-videos-in-this-series)
</span></span><br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-predict-an-answer-with-a-simple-model/player]
>
>

## <a name="other-videos-in-this-series"></a><span data-ttu-id="f3327-111">A sorozat többi videók</span><span class="sxs-lookup"><span data-stu-id="f3327-111">Other videos in this series</span></span>
<span data-ttu-id="f3327-112">*Adattudomány kezdőknek* van egy gyors bevezetés toodata tudományos az öt rövid videók.</span><span class="sxs-lookup"><span data-stu-id="f3327-112">*Data Science for Beginners* is a quick introduction toodata science in five short videos.</span></span>

* <span data-ttu-id="f3327-113">1. Videó: [hello 5 kérdésekre adatok tudományos válaszok](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 perc 14 másodperc)*</span><span class="sxs-lookup"><span data-stu-id="f3327-113">Video 1: [hello 5 questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*</span></span>
* <span data-ttu-id="f3327-114">2. Videó: [adattudomány készen áll az adatok?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md)</span><span class="sxs-lookup"><span data-stu-id="f3327-114">Video 2: [Is your data ready for data science?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md)</span></span> <span data-ttu-id="f3327-115">*(4 perc 56 másodperc)*</span><span class="sxs-lookup"><span data-stu-id="f3327-115">*(4 min 56 sec)*</span></span>
* <span data-ttu-id="f3327-116">3. Videó: [tegyen fel kérdést a válasz adatokkal](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 perc 17 másodperc)*</span><span class="sxs-lookup"><span data-stu-id="f3327-116">Video 3: [Ask a question you can answer with data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*</span></span>
* <span data-ttu-id="f3327-117">4. Videó: Egyszerű modell választ előrejelzése</span><span class="sxs-lookup"><span data-stu-id="f3327-117">Video 4: Predict an answer with a simple model</span></span>
* <span data-ttu-id="f3327-118">5. Videó: [mások munkahelyi toodo adattudomány másolása](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 perc 18 másodperc)*</span><span class="sxs-lookup"><span data-stu-id="f3327-118">Video 5: [Copy other people's work toodo data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*</span></span>

## <a name="transcript-predict-an-answer-with-a-simple-model"></a><span data-ttu-id="f3327-119">A Beszélgetés szövegének: Egyszerű modell választ előrejelzése</span><span class="sxs-lookup"><span data-stu-id="f3327-119">Transcript: Predict an answer with a simple model</span></span>
<span data-ttu-id="f3327-120">Üdvözli a negyedik videó hello "Adatok tudományos a kezdők" Adatsorozat toohello.</span><span class="sxs-lookup"><span data-stu-id="f3327-120">Welcome toohello fourth video in hello "Data Science for Beginners" series.</span></span> <span data-ttu-id="f3327-121">Ezt a telepítést azt bemutatjuk egy egyszerű modell létrehozása, ellenőrizze az előrejelzéshez.</span><span class="sxs-lookup"><span data-stu-id="f3327-121">In this one, we'll build a simple model and make a prediction.</span></span>

<span data-ttu-id="f3327-122">A *modell* egy egyszerűsített szövegegység az adatok körül forog.</span><span class="sxs-lookup"><span data-stu-id="f3327-122">A *model* is a simplified story about our data.</span></span> <span data-ttu-id="f3327-123">I fogjuk I jelenti.</span><span class="sxs-lookup"><span data-stu-id="f3327-123">I'll show you what I mean.</span></span>

## <a name="collect-relevant-accurate-connected-enough-data"></a><span data-ttu-id="f3327-124">Megfelelő, a pontos, csatlakoztatott, elég adatok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="f3327-124">Collect relevant, accurate, connected, enough data</span></span>
<span data-ttu-id="f3327-125">Tegyük fel például, egy rombusz kívánt tooshop.</span><span class="sxs-lookup"><span data-stu-id="f3327-125">Say I want tooshop for a diamond.</span></span> <span data-ttu-id="f3327-126">Az egy 1.35 beszúrási rombusz beállítással nőivarú toomy nagyszülő tartozó gyűrű van, és tooget egy meghatározni, hogy a rendszer mennyibe kívánt.</span><span class="sxs-lookup"><span data-stu-id="f3327-126">I have a ring that belonged toomy grandmother with a setting for a 1.35 carat diamond, and I want tooget an idea of how much it will cost.</span></span> <span data-ttu-id="f3327-127">Szeretnék egy Jegyzettömb és toll hello ékszereket tárolóba, és szeretnék írja le az összes hello rombusz – hello esetben és akkor karát mennyi mérjük hello ár.</span><span class="sxs-lookup"><span data-stu-id="f3327-127">I take a notepad and pen into hello jewelry store, and I write down hello price of all of hello diamonds in hello case and how much they weigh in carats.</span></span> <span data-ttu-id="f3327-128">Hello első rombusz - fennállt 1.01 karát és $7,366 kezdve.</span><span class="sxs-lookup"><span data-stu-id="f3327-128">Starting with hello first diamond - it's 1.01 carats and $7,366.</span></span>

<span data-ttu-id="f3327-129">Most keresztül halad, és tegye meg ezt az összes hello más rombusz – hello tároló.</span><span class="sxs-lookup"><span data-stu-id="f3327-129">Now I go through and do this for all hello other diamonds in hello store.</span></span>

![Oszlopok rombusz adatok](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

<span data-ttu-id="f3327-131">Figyelje meg, hogy a kiválasztott két oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f3327-131">Notice that our list has two columns.</span></span> <span data-ttu-id="f3327-132">Mindegyik oszlopnak van egy másik attribútum - a súlyozás karát és ár - és minden egyes sorára egyetlen rombusz jelölő egyetlen adatpont.</span><span class="sxs-lookup"><span data-stu-id="f3327-132">Each column has a different attribute - weight in carats and price - and each row is a single data point that represents a single diamond.</span></span>

<span data-ttu-id="f3327-133">Ténylegesen létrehoztunk Önnek egy kis adatkészlet ide - tábla.</span><span class="sxs-lookup"><span data-stu-id="f3327-133">We've actually created a small data set here - a table.</span></span> <span data-ttu-id="f3327-134">Figyelje meg, hogy megfelel-e a minőségi feltételek:</span><span class="sxs-lookup"><span data-stu-id="f3327-134">Notice that it meets our criteria for quality:</span></span>

* <span data-ttu-id="f3327-135">hello adatok **vonatkozó** -súly mindenképpen kapcsolódó tooprice</span><span class="sxs-lookup"><span data-stu-id="f3327-135">hello data is **relevant** - weight is definitely related tooprice</span></span>
* <span data-ttu-id="f3327-136">Rendelkezik **pontos** -azt ellenőrizni hello árak, amelyek azt írja le</span><span class="sxs-lookup"><span data-stu-id="f3327-136">It's **accurate** - we double-checked hello prices that we write down</span></span>
* <span data-ttu-id="f3327-137">Rendelkezik **csatlakoztatott** -nincsenek ezeket az oszlopokat az üres szóközök nélkül</span><span class="sxs-lookup"><span data-stu-id="f3327-137">It's **connected** - there are no blank spaces in either of these columns</span></span>
* <span data-ttu-id="f3327-138">És mivel megtanulhatja, rendelkezik **elég** adatok tooanswer a kérdés</span><span class="sxs-lookup"><span data-stu-id="f3327-138">And, as we'll see, it's **enough** data tooanswer our question</span></span>

## <a name="ask-a-sharp-question"></a><span data-ttu-id="f3327-139">Éles kérdés</span><span class="sxs-lookup"><span data-stu-id="f3327-139">Ask a sharp question</span></span>
<span data-ttu-id="f3327-140">Most azt fogja jelent a kérdés éles úgy: "mennyi lesz költség toobuy egy 1.35 beszúrási rombusz?"</span><span class="sxs-lookup"><span data-stu-id="f3327-140">Now we'll pose our question in a sharp way: "How much will it cost toobuy a 1.35 carat diamond?"</span></span>

<span data-ttu-id="f3327-141">A lista nem rendelkezik egy 1.35 beszúrási rombusz, hogy lesz toouse hello többi részének az adatok tooget egy válasz toohello kérdést.</span><span class="sxs-lookup"><span data-stu-id="f3327-141">Our list doesn't have a 1.35 carat diamond in it, so we'll have toouse hello rest of our data tooget an answer toohello question.</span></span>

## <a name="plot-hello-existing-data"></a><span data-ttu-id="f3327-142">Hello meglévő adatok ábrázolása</span><span class="sxs-lookup"><span data-stu-id="f3327-142">Plot hello existing data</span></span>
<span data-ttu-id="f3327-143">hello elsőként azt kell végeznie az rajzolása vízszintes számát, nevű tengely toochart hello súlyt.</span><span class="sxs-lookup"><span data-stu-id="f3327-143">hello first thing we'll do is draw a horizontal number line, called an axis, toochart hello weights.</span></span> <span data-ttu-id="f3327-144">hello hello súlyok tartománya 0 too2, így azt fogja rajzolása, amely hozzá van rendelve, amely a tartomány és az egyes fele beszúrási ticks helyezze.</span><span class="sxs-lookup"><span data-stu-id="f3327-144">hello range of hello weights is 0 too2, so we'll draw a line that covers that range and put ticks for each half carat.</span></span>

<span data-ttu-id="f3327-145">Lesz ezután azt egy függőleges tengely toorecord hello ár megrajzolásához, és csatlakoztassa toohello súly vízszintes tengely.</span><span class="sxs-lookup"><span data-stu-id="f3327-145">Next we'll draw a vertical axis toorecord hello price and connect it toohello horizontal weight axis.</span></span> <span data-ttu-id="f3327-146">Ezzel egység dolláros lesz.</span><span class="sxs-lookup"><span data-stu-id="f3327-146">This will be in units of dollars.</span></span> <span data-ttu-id="f3327-147">Most van koordináta tengelyek készlete.</span><span class="sxs-lookup"><span data-stu-id="f3327-147">Now we have a set of coordinate axes.</span></span>

![Súly és az ár](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

<span data-ttu-id="f3327-149">A Microsoft következővel folyamatos tootake ezeket az adatokat, és kapcsolja be a *pont rajzot*.</span><span class="sxs-lookup"><span data-stu-id="f3327-149">We're going tootake this data now and turn it into a *scatter plot*.</span></span> <span data-ttu-id="f3327-150">Ez a kiváló módja toovisualize numerikus adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="f3327-150">This is a great way toovisualize numerical data sets.</span></span>

<span data-ttu-id="f3327-151">Az első adatpont hello azt eyeball 1.01 karát mérve.</span><span class="sxs-lookup"><span data-stu-id="f3327-151">For hello first data point, we eyeball a vertical line at 1.01 carats.</span></span> <span data-ttu-id="f3327-152">Ezt követően: $7,366 vízszintes vonal eyeball azt.</span><span class="sxs-lookup"><span data-stu-id="f3327-152">Then, we eyeball a horizontal line at $7,366.</span></span> <span data-ttu-id="f3327-153">Megfelelnek-e, ha azt egy pont megrajzolásához.</span><span class="sxs-lookup"><span data-stu-id="f3327-153">Where they meet, we draw a dot.</span></span> <span data-ttu-id="f3327-154">Az első rombusz jelképez.</span><span class="sxs-lookup"><span data-stu-id="f3327-154">This represents our first diamond.</span></span>

<span data-ttu-id="f3327-155">Most azt halad át minden rombusz ezen a listán, és hello ugyanaz.</span><span class="sxs-lookup"><span data-stu-id="f3327-155">Now we go through each diamond on this list and do hello same thing.</span></span> <span data-ttu-id="f3327-156">A rendszer keresztül, ha ez az azt lekérése: pontokból álló, lemezcsoport, minden egyes rombusz egyet.</span><span class="sxs-lookup"><span data-stu-id="f3327-156">When we're through, this is what we get: a bunch of dots, one for each diamond.</span></span>

![Pontdiagram rajzot](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-hello-model-through-hello-data-points"></a><span data-ttu-id="f3327-158">Hello modell hello adatpontokra megrajzolásához</span><span class="sxs-lookup"><span data-stu-id="f3327-158">Draw hello model through hello data points</span></span>
<span data-ttu-id="f3327-159">Most hello pontok és squint tekinti meg, ha hello gyűjtemény néz fat, intelligens sor.</span><span class="sxs-lookup"><span data-stu-id="f3327-159">Now if you look at hello dots and squint, hello collection looks like a fat, fuzzy line.</span></span> <span data-ttu-id="f3327-160">Azt a jelölő igénybe, és egy egyenes vonalat rajta.</span><span class="sxs-lookup"><span data-stu-id="f3327-160">We can take our marker and draw a straight line through it.</span></span>

<span data-ttu-id="f3327-161">Egy sor rajzolási, által létrehozott egy *modell*.</span><span class="sxs-lookup"><span data-stu-id="f3327-161">By drawing a line, we created a *model*.</span></span> <span data-ttu-id="f3327-162">Használjon hello valós véve, és azt simplistic Rajzfilmes verziójának.</span><span class="sxs-lookup"><span data-stu-id="f3327-162">Think of this as taking hello real world and making a simplistic cartoon version of it.</span></span> <span data-ttu-id="f3327-163">Most hello Rajzfilmes megfelelő – hello sor összes hello adatpontok nem használható.</span><span class="sxs-lookup"><span data-stu-id="f3327-163">Now hello cartoon is wrong - hello line doesn't go through all hello data points.</span></span> <span data-ttu-id="f3327-164">Ez azonban hasznos egyszerűsítését.</span><span class="sxs-lookup"><span data-stu-id="f3327-164">But, it's a useful simplification.</span></span>

![Lineáris regressziós egyenes](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

<span data-ttu-id="f3327-166">hello tényt, hogy minden hello pontokból nem végighaladnia pontosan hello sor az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="f3327-166">hello fact that all hello dots don't go exactly through hello line is OK.</span></span> <span data-ttu-id="f3327-167">Adatszakértőkön azt ismertetik, ennek kimondásával, hogy van-e hello modell -, amely hello sor - és minden pont van néhány *zaj* vagy *variancia* társítva.</span><span class="sxs-lookup"><span data-stu-id="f3327-167">Data scientists explain this by saying that there's hello model - that's hello line - and then each dot has some *noise* or *variance* associated with it.</span></span> <span data-ttu-id="f3327-168">Az alapul szolgáló tökéletes kapcsolat hello van, és nincs majd hello kövecses, valós globális zaj és bizonytalanság hozzáadó.</span><span class="sxs-lookup"><span data-stu-id="f3327-168">There's hello underlying perfect relationship, and then there's hello gritty, real world that adds noise and uncertainty.</span></span>

<span data-ttu-id="f3327-169">Mivel tooanswer hello kérdés próbált *mennyi?* ezt nevezik a *regressziós*.</span><span class="sxs-lookup"><span data-stu-id="f3327-169">Because we're trying tooanswer hello question *How much?* this is called a *regression*.</span></span> <span data-ttu-id="f3327-170">Mivel használunk egyenes, és van egy *lineáris regressziós*.</span><span class="sxs-lookup"><span data-stu-id="f3327-170">And because we're using a straight line, it's a *linear regression*.</span></span>

## <a name="use-hello-model-toofind-hello-answer"></a><span data-ttu-id="f3327-171">Hello modell toofind hello válaszfájl használata</span><span class="sxs-lookup"><span data-stu-id="f3327-171">Use hello model toofind hello answer</span></span>
<span data-ttu-id="f3327-172">A modell van, és megkérjük, a kérdés: mennyi lesz a 1.35 beszúrási rombusz költség?</span><span class="sxs-lookup"><span data-stu-id="f3327-172">Now we have a model and we ask it our question: How much will a 1.35 carat diamond cost?</span></span>

<span data-ttu-id="f3327-173">tooanswer a kérdést, szem 1.35 karát azt és egy függőleges vonalat.</span><span class="sxs-lookup"><span data-stu-id="f3327-173">tooanswer our question, we eyeball 1.35 carats and draw a vertical line.</span></span> <span data-ttu-id="f3327-174">Az áthalad hello modell sor, ha azt egy vízszintes vonal toohello dollár tengely eyeball.</span><span class="sxs-lookup"><span data-stu-id="f3327-174">Where it crosses hello model line, we eyeball a horizontal line toohello dollar axis.</span></span> <span data-ttu-id="f3327-175">Találatok száma a jobb: 10 000-re.</span><span class="sxs-lookup"><span data-stu-id="f3327-175">It hits right at 10,000.</span></span> <span data-ttu-id="f3327-176">Elterjedése!</span><span class="sxs-lookup"><span data-stu-id="f3327-176">Boom!</span></span> <span data-ttu-id="f3327-177">Ez az hello válasz: egy 1.35 beszúrási rombusz körülbelül 10 000 $ költségek.</span><span class="sxs-lookup"><span data-stu-id="f3327-177">That's hello answer: A 1.35 carat diamond costs about $10,000.</span></span>

![Található hello válasz hello modell](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a><span data-ttu-id="f3327-179">Hozzon létre egy megbízhatósági intervallum</span><span class="sxs-lookup"><span data-stu-id="f3327-179">Create a confidence interval</span></span>
<span data-ttu-id="f3327-180">Hogyan pontos a előrejelzése természetes toowonder.</span><span class="sxs-lookup"><span data-stu-id="f3327-180">It's natural toowonder how precise this prediction is.</span></span> <span data-ttu-id="f3327-181">Hasznos tooknow e hello 1.35 beszúrási rombusz túl rendkívül szoros lesznek $ 10 000-re vagy magasabb vagy alacsonyabb nagy.</span><span class="sxs-lookup"><span data-stu-id="f3327-181">It's useful tooknow whether hello 1.35 carat diamond will be very close too$10,000, or a lot higher or lower.</span></span> <span data-ttu-id="f3327-182">toofigure a, most körül, amely tartalmazza a legtöbb hello pontokból hello regressziós egyenes boríték megrajzolásához.</span><span class="sxs-lookup"><span data-stu-id="f3327-182">toofigure this out, let's draw an envelope around hello regression line that includes most of hello dots.</span></span> <span data-ttu-id="f3327-183">A boríték nevezik a *megbízhatósági*: a rendszer közérthető bizonyos abban, hogy az árak esik-e a boríték, mert az elmúlt hello többsége a őket.</span><span class="sxs-lookup"><span data-stu-id="f3327-183">This envelope is called our *confidence interval*: We're pretty confident that prices fall within this envelope, because in hello past most of them have.</span></span> <span data-ttu-id="f3327-184">Ha hello 1.35 beszúrási sor áthalad hello teteje és alja hello borítékot azt is tárolt két vízszintesen több sort.</span><span class="sxs-lookup"><span data-stu-id="f3327-184">We can draw two more horizontal lines from where hello 1.35 carat line crosses hello top and hello bottom of that envelope.</span></span>

![Megbízhatósági intervallum](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

<span data-ttu-id="f3327-186">Most azt is ismertetés az vetett bizalmat időköz: magabiztosan azt mondani, hogy egy 1.35 beszúrási rombusz hello ára készül $ 10 000 -, de lehet akár csupán $8000 és lehet, mint $ 12 000.</span><span class="sxs-lookup"><span data-stu-id="f3327-186">Now we can say something about our confidence interval:  We can say confidently that hello price of a 1.35 carat diamond is about $10,000 - but it might be as low as $8,000 and it might be as high as $12,000.</span></span>

## <a name="were-done-with-no-math-or-computers"></a><span data-ttu-id="f3327-187">A Microsoft nem használja, nem matematikai vagy számítógépek</span><span class="sxs-lookup"><span data-stu-id="f3327-187">We're done, with no math or computers</span></span>
<span data-ttu-id="f3327-188">Azt adta milyen adatszakértőkön beolvasása fizetett toodo, és azt adta azt csak fel:</span><span class="sxs-lookup"><span data-stu-id="f3327-188">We did what data scientists get paid toodo, and we did it just by drawing:</span></span>

* <span data-ttu-id="f3327-189">Azt kéri, a kérdés, hogy azt az választ sikerült adatokkal</span><span class="sxs-lookup"><span data-stu-id="f3327-189">We asked a question that we could answer with data</span></span>
* <span data-ttu-id="f3327-190">Azt a beépített egy *modell* használatával *lineáris regressziós*</span><span class="sxs-lookup"><span data-stu-id="f3327-190">We built a *model* using *linear regression*</span></span>
* <span data-ttu-id="f3327-191">A Microsoft tett egy *előrejelzés*, kész, de egy *megbízhatósági intervallum*</span><span class="sxs-lookup"><span data-stu-id="f3327-191">We made a *prediction*, complete with a *confidence interval*</span></span>

<span data-ttu-id="f3327-192">Matematikai vagy számítógépek toodo nem használjuk, és azt.</span><span class="sxs-lookup"><span data-stu-id="f3327-192">And we didn't use math or computers toodo it.</span></span>

<span data-ttu-id="f3327-193">Ha további információt a Microsoft volt, mostantól például...</span><span class="sxs-lookup"><span data-stu-id="f3327-193">Now if we'd had more information, like...</span></span>

* <span data-ttu-id="f3327-194">hello hello rombusz a Kivágás</span><span class="sxs-lookup"><span data-stu-id="f3327-194">hello cut of hello diamond</span></span>
* <span data-ttu-id="f3327-195">szín változata (milyen pontossággal hello rombusz fehér toobeing)</span><span class="sxs-lookup"><span data-stu-id="f3327-195">color variations (how close hello diamond is toobeing white)</span></span>
* <span data-ttu-id="f3327-196">a hello rombusz befoglalások hello száma</span><span class="sxs-lookup"><span data-stu-id="f3327-196">hello number of inclusions in hello diamond</span></span>

<span data-ttu-id="f3327-197">.. helyőrzőkre azt kellett volna több oszlopot.</span><span class="sxs-lookup"><span data-stu-id="f3327-197">...then we would have had more columns.</span></span> <span data-ttu-id="f3327-198">Ebben az esetben a matematikai lesz hasznos.</span><span class="sxs-lookup"><span data-stu-id="f3327-198">In that case, math becomes helpful.</span></span> <span data-ttu-id="f3327-199">Ha több mint két oszlop, papíron rögzített toodraw pont.</span><span class="sxs-lookup"><span data-stu-id="f3327-199">If you have more than two columns, it's hard toodraw dots on paper.</span></span> <span data-ttu-id="f3327-200">hello matematikai lehetővé teszi, hogy a sor vagy vezérlősík tooyour adatok nagyon szépen.</span><span class="sxs-lookup"><span data-stu-id="f3327-200">hello math lets you fit that line or that plane tooyour data very nicely.</span></span>

<span data-ttu-id="f3327-201">Is ha helyett csak néhány olyan gyémántok, le kellett két példányban vagy két millió, majd munka teheti sokkal gyorsabb, a számítógép.</span><span class="sxs-lookup"><span data-stu-id="f3327-201">Also, if instead of just a handful of diamonds, we had two thousand or two million, then you can do that work much faster with a computer.</span></span>

<span data-ttu-id="f3327-202">Napjainkban túlmutatnak tárgyalt hogyan toodo lineáris regresszióját, és azt szeretné az adatok előrejelzéshez tenni.</span><span class="sxs-lookup"><span data-stu-id="f3327-202">Today, we've talked about how toodo linear regression, and we made a prediction using data.</span></span>

<span data-ttu-id="f3327-203">Hello ki, hogy toocheck kell az "Adatok tudományos a kezdők" a Microsoft Azure Machine Learning más videók.</span><span class="sxs-lookup"><span data-stu-id="f3327-203">Be sure toocheck out hello other videos in "Data Science for Beginners" from Microsoft Azure Machine Learning.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3327-204">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3327-204">Next steps</span></span>
* [<span data-ttu-id="f3327-205">Próbálja meg egy első adatok tudományos kísérletben a Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="f3327-205">Try a first data science experiment with Machine Learning Studio</span></span>](machine-learning-create-experiment.md)
* [<span data-ttu-id="f3327-206">Egy bevezető tooMachine beszerzése a Microsoft Azure tanulási</span><span class="sxs-lookup"><span data-stu-id="f3327-206">Get an introduction tooMachine Learning on Microsoft Azure</span></span>](machine-learning-what-is-machine-learning.md)
