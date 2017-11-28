---
title: "Készen állnak adatai az elemzésre? Az adatok kiértékelése - Azure Machine Learning |} Microsoft Docs"
description: "Ismerje meg, hogy az adatok készen álljon a adattudomány 4 feltételeit. Videó 2 kezdőknek Adattudomány rendelkezik konkrét, meghatározott példák segítséget az alapszintű adatok kiértékelése."
keywords: "a vonatkozó adatok adatok kiértékelése, adatok, adatok feltételek, készen áll a adatok előkészítése"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: d502062c-da70-4b21-9054-0bfd9902612e
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: c4a8bc11aec2f71796f589c0af54cc92253e5180
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="is-your-data-ready-for-data-science"></a><span data-ttu-id="d564a-106">Készen állnak adatai az elemzésre?</span><span class="sxs-lookup"><span data-stu-id="d564a-106">Is your data ready for data science?</span></span>
## <a name="video-2-data-science-for-beginners-series"></a><span data-ttu-id="d564a-107">2. Videó: Adattudomány kezdők sorozat</span><span class="sxs-lookup"><span data-stu-id="d564a-107">Video 2: Data Science for Beginners series</span></span>
<span data-ttu-id="d564a-108">Útmutató az adatokat, hogy ellenőrizze, hogy megfelel-e egyszerű feltételt, készen áll a adattudomány kiértékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="d564a-108">Learn how to evaluate your data to make sure it meets basic criteria to be ready for data science.</span></span>

<span data-ttu-id="d564a-109">Ahhoz, hogy minél hatékonyabb működtetését az adatsorozat, tekintse meg azokat.</span><span class="sxs-lookup"><span data-stu-id="d564a-109">To get the most out of the series, watch them all.</span></span> <span data-ttu-id="d564a-110">[Ugrás a videók listája](#other-videos-in-this-series)
</span><span class="sxs-lookup"><span data-stu-id="d564a-110">[Go to the list of videos](#other-videos-in-this-series)
</span></span><br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a><span data-ttu-id="d564a-111">A sorozat többi videók</span><span class="sxs-lookup"><span data-stu-id="d564a-111">Other videos in this series</span></span>
<span data-ttu-id="d564a-112">*Adattudomány kezdőknek* van egy gyors Bevezetés a adattudomány az öt rövid videók.</span><span class="sxs-lookup"><span data-stu-id="d564a-112">*Data Science for Beginners* is a quick introduction to data science in five short videos.</span></span>

* <span data-ttu-id="d564a-113">1. Videó: [az 5 kérdésekre adatok tudományos válaszok](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 perc 14 másodperc)*</span><span class="sxs-lookup"><span data-stu-id="d564a-113">Video 1: [The 5 questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec)*</span></span>
* <span data-ttu-id="d564a-114">2. Videó: Az adatok készen áll a adattudomány?</span><span class="sxs-lookup"><span data-stu-id="d564a-114">Video 2: Is your data ready for data science?</span></span>
* <span data-ttu-id="d564a-115">3. Videó: [tegyen fel kérdést a válasz adatokkal](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 perc 17 másodperc)*</span><span class="sxs-lookup"><span data-stu-id="d564a-115">Video 3: [Ask a question you can answer with data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*</span></span>
* <span data-ttu-id="d564a-116">4. Videó: [választ egyszerű modell előrejelzése](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 perc 42 másodperc)*</span><span class="sxs-lookup"><span data-stu-id="d564a-116">Video 4: [Predict an answer with a simple model](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*</span></span>
* <span data-ttu-id="d564a-117">5. Videó: [mások munkahelyi adattudomány ehhez másolja](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 perc 18 másodperc)*</span><span class="sxs-lookup"><span data-stu-id="d564a-117">Video 5: [Copy other people's work to do data science](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*</span></span>

## <a name="transcript-is-your-data-ready-for-data-science"></a><span data-ttu-id="d564a-118">A Beszélgetés szövegének: Az adatok készen áll a adattudomány?</span><span class="sxs-lookup"><span data-stu-id="d564a-118">Transcript: Is your data ready for data science?</span></span>
<span data-ttu-id="d564a-119">Üdvözli a "Az adatok készen áll a adattudomány?"</span><span class="sxs-lookup"><span data-stu-id="d564a-119">Welcome to "Is your data ready for data science?"</span></span> <span data-ttu-id="d564a-120">a második videó az adatsorozat *Adattudomány kezdőknek*.</span><span class="sxs-lookup"><span data-stu-id="d564a-120">the second video in the series *Data Science for Beginners*.</span></span>  

<span data-ttu-id="d564a-121">Előtt adattudomány adhat meg a kívánt válaszokat, akkor adjon neki néhány kiváló minőségű raw anyagok történő együttműködésre.</span><span class="sxs-lookup"><span data-stu-id="d564a-121">Before data science can give you the answers you want, you have to give it some high-quality raw materials to work with.</span></span> <span data-ttu-id="d564a-122">Csakúgy, mint egy pizzaszósz, annál jobb a kiindulási pont, annál jobb a végső termék összetevők elvégzése.</span><span class="sxs-lookup"><span data-stu-id="d564a-122">Just like making a pizza, the better the ingredients you start with, the better the final product.</span></span> 

## <a name="criteria-for-data"></a><span data-ttu-id="d564a-123">Adatok feltételeit</span><span class="sxs-lookup"><span data-stu-id="d564a-123">Criteria for data</span></span>
<span data-ttu-id="d564a-124">Igen adattudomány, ha nincsenek egyes lekéréses együtt szükséges összetevők.</span><span class="sxs-lookup"><span data-stu-id="d564a-124">So, in the case of data science, there are some ingredients that we need to pull together.</span></span>

<span data-ttu-id="d564a-125">Igazolnia kell, amely adatokat:</span><span class="sxs-lookup"><span data-stu-id="d564a-125">We need data that is:</span></span>

* <span data-ttu-id="d564a-126">Megfelelő</span><span class="sxs-lookup"><span data-stu-id="d564a-126">Relevant</span></span>
* <span data-ttu-id="d564a-127">Csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="d564a-127">Connected</span></span>
* <span data-ttu-id="d564a-128">A pontos</span><span class="sxs-lookup"><span data-stu-id="d564a-128">Accurate</span></span>
* <span data-ttu-id="d564a-129">Elegendő a használata</span><span class="sxs-lookup"><span data-stu-id="d564a-129">Enough to work with</span></span>

## <a name="is-your-data-relevant"></a><span data-ttu-id="d564a-130">Fontos az adatokat?</span><span class="sxs-lookup"><span data-stu-id="d564a-130">Is your data relevant?</span></span>
<span data-ttu-id="d564a-131">Ezért az első összetevő - ellenőriznünk kell a szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="d564a-131">So the first ingredient - we need data that's relevant.</span></span>

![A vonatkozó adatokat, és irreleváns adatok - adatok kiértékelése](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

<span data-ttu-id="d564a-133">Tekintse meg a tábla a bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="d564a-133">Look at the table on the left.</span></span> <span data-ttu-id="d564a-134">Azt kívül Boston sávok hét személyek teljesül, a vér alkohol szintjükre, az utolsó játékban piros Sox batting átlagos és a legközelebbi kényelmi tárolóban tej ára mérni.</span><span class="sxs-lookup"><span data-stu-id="d564a-134">We met seven people outside of Boston bars, measured their blood alcohol level, the Red Sox batting average in their last game, and the price of milk in the nearest convenience store.</span></span>

<span data-ttu-id="d564a-135">Ezek az összes tökéletesen jogos adatai.</span><span class="sxs-lookup"><span data-stu-id="d564a-135">This is all perfectly legitimate data.</span></span> <span data-ttu-id="d564a-136">Az egyetlen hiba, nincs jelentősége.</span><span class="sxs-lookup"><span data-stu-id="d564a-136">It’s only fault is that it isn’t relevant.</span></span> <span data-ttu-id="d564a-137">Ezek a számok között nem nyilvánvaló kapcsolat áll fenn.</span><span class="sxs-lookup"><span data-stu-id="d564a-137">There's no obvious relationship between these numbers.</span></span> <span data-ttu-id="d564a-138">I akitől tej és a piros Sox batting átlagos aktuális ára, ha nincs semmilyen módon nem lehetett kitalálni a saját vér alkohol tartalom.</span><span class="sxs-lookup"><span data-stu-id="d564a-138">If I gave you the current price of milk and the Red Sox batting average, there's no way you could guess my blood alcohol content.</span></span>

<span data-ttu-id="d564a-139">Most tekintse meg a tábla a jobb oldalon.</span><span class="sxs-lookup"><span data-stu-id="d564a-139">Now look at the table on the right.</span></span> <span data-ttu-id="d564a-140">Most azt mérni minden egyes személy háttértár body és számítanak azok volna italok száma.</span><span class="sxs-lookup"><span data-stu-id="d564a-140">This time we measured each person’s body mass and counted the number of drinks they’ve had.</span></span>  <span data-ttu-id="d564a-141">Minden egyes sorára szereplő számok most kapcsolódik egymáshoz.</span><span class="sxs-lookup"><span data-stu-id="d564a-141">The numbers in each row are now relevant to each other.</span></span> <span data-ttu-id="d564a-142">Ha I akitől a szervezet háttértár és a már korábban Margaritas számát, akkor teheti a saját vér cikkekből alkohol tartalom.</span><span class="sxs-lookup"><span data-stu-id="d564a-142">If I gave you my body mass and the number of Margaritas I've had, you could make a guess at my blood alcohol content.</span></span>

## <a name="do-you-have-connected-data"></a><span data-ttu-id="d564a-143">Tegye csatlakozott adatokat?</span><span class="sxs-lookup"><span data-stu-id="d564a-143">Do you have connected data?</span></span>
<span data-ttu-id="d564a-144">A következő összetevő az csatlakoztatott adatai.</span><span class="sxs-lookup"><span data-stu-id="d564a-144">The next ingredient is connected data.</span></span>

![Csatlakoztatott vagy leválasztott adat - adatok feltételek, készen áll az adatok](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

<span data-ttu-id="d564a-146">Íme néhány hamburgers minőségének vonatkozó adatokat: hőmérséklet patty súly és minősítési újság helyi étele a rács.</span><span class="sxs-lookup"><span data-stu-id="d564a-146">Here is some relevant data on the quality of hamburgers: grill temperature, patty weight, and rating in the local food magazine.</span></span> <span data-ttu-id="d564a-147">De figyelje meg, a bal oldali tábla hiányosságait.</span><span class="sxs-lookup"><span data-stu-id="d564a-147">But notice the gaps in the table on the left.</span></span>

<span data-ttu-id="d564a-148">A legtöbb adatkészletek hiányzik néhány érték.</span><span class="sxs-lookup"><span data-stu-id="d564a-148">Most data sets are missing some values.</span></span> <span data-ttu-id="d564a-149">Gyakori megoldás, hogy rendelkezik az ehhez hasonló lyuk és módon kerülő őket.</span><span class="sxs-lookup"><span data-stu-id="d564a-149">It's common to have holes like this and there are ways to work around them.</span></span> <span data-ttu-id="d564a-150">De ha túl sok hiányzik, az adatok elkezdi Svájc közötti sajt tűnik.</span><span class="sxs-lookup"><span data-stu-id="d564a-150">But if there's too much missing, your data begins to look like Swiss cheese.</span></span>

<span data-ttu-id="d564a-151">A tábla a bal oldali tekinti meg, ha nincs, sok hiányzó adatot, hogy nehezen rács hőmérséklet és patty súly közötti kapcsolat bármilyen kapja meg.</span><span class="sxs-lookup"><span data-stu-id="d564a-151">If you look at the table on the left, there's so much missing data, it's hard to come up with any kind of relationship between grill temperature and patty weight.</span></span> <span data-ttu-id="d564a-152">Íme egy leválasztott adatok.</span><span class="sxs-lookup"><span data-stu-id="d564a-152">This is an example of disconnected data.</span></span>

<span data-ttu-id="d564a-153">A jobb oldali tábla azonban megtelt, és kész - csatlakoztatott adatok példát.</span><span class="sxs-lookup"><span data-stu-id="d564a-153">The table on the right, though, is full and complete - an example of connected data.</span></span>

## <a name="is-your-data-accurate"></a><span data-ttu-id="d564a-154">Az adatok pontos van?</span><span class="sxs-lookup"><span data-stu-id="d564a-154">Is your data accurate?</span></span>
<span data-ttu-id="d564a-155">A következő igazolnia kell összetevője pontosságát.</span><span class="sxs-lookup"><span data-stu-id="d564a-155">The next ingredient we need is accuracy.</span></span> <span data-ttu-id="d564a-156">Az alábbiakban a nyilak találat szeretnénk négy tárolók.</span><span class="sxs-lookup"><span data-stu-id="d564a-156">Here are four targets that we’d like to hit with arrows.</span></span>

![Hibás adatokat - adatok feltételek és pontos adatok](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

<span data-ttu-id="d564a-158">Tekintse meg a cél a jobb felső részén.</span><span class="sxs-lookup"><span data-stu-id="d564a-158">Look at the target in the upper right.</span></span> <span data-ttu-id="d564a-159">Az elem-macskaszem körül jobbra szoros csoportosítása van.</span><span class="sxs-lookup"><span data-stu-id="d564a-159">We’ve got a tight grouping right around the bullseye.</span></span> <span data-ttu-id="d564a-160">Természetesen ez pontos.</span><span class="sxs-lookup"><span data-stu-id="d564a-160">That, of course, is accurate.</span></span> <span data-ttu-id="d564a-161">Furcsa viselkedése adattudomány nyelvén, a teljesítmény a cél jobb alá is figyelembe veszi a pontos.</span><span class="sxs-lookup"><span data-stu-id="d564a-161">Oddly, in the language of data science, our performance on the target right below it is also considered accurate.</span></span>

<span data-ttu-id="d564a-162">Ha ezen nyilak közepére meg leképezni, mutatunk be, hogy a rendszer nagyon közel az elem-macskaszem.</span><span class="sxs-lookup"><span data-stu-id="d564a-162">If you were to map out the center of these arrows, you'd see that it's very close to the bullseye.</span></span> <span data-ttu-id="d564a-163">A nyilak helyezkednek el minden, a cél körül, nem pontos most minősül, de azok még része a elem-macskaszem, így pontos most számít.</span><span class="sxs-lookup"><span data-stu-id="d564a-163">The arrows are spread out all around the target, so they're considered imprecise, but they're centered around the bullseye, so they're considered accurate.</span></span>

<span data-ttu-id="d564a-164">Most tekintse meg a bal felső cél.</span><span class="sxs-lookup"><span data-stu-id="d564a-164">Now look at the upper-left target.</span></span> <span data-ttu-id="d564a-165">Itt a nyilak találati nagyon közel együtt szoros csoportosítása.</span><span class="sxs-lookup"><span data-stu-id="d564a-165">Here our arrows hit very close together, a tight grouping.</span></span> <span data-ttu-id="d564a-166">Pontos fontosságúak, de mivel a központ ki az elem-macskaszem módon pontatlan fontosságúak.</span><span class="sxs-lookup"><span data-stu-id="d564a-166">They're precise, but they're inaccurate because the center is way off the bullseye.</span></span> <span data-ttu-id="d564a-167">És a természetesen a nyilak a bal alsó célzott pontatlan, mind a nem pontos.</span><span class="sxs-lookup"><span data-stu-id="d564a-167">And, of course, the arrows in the bottom-left target are both inaccurate and imprecise.</span></span> <span data-ttu-id="d564a-168">Ez archer további eljárás szükséges.</span><span class="sxs-lookup"><span data-stu-id="d564a-168">This archer needs more practice.</span></span>

## <a name="do-you-have-enough-data-to-work-with"></a><span data-ttu-id="d564a-169">Elegendő adatokra van?</span><span class="sxs-lookup"><span data-stu-id="d564a-169">Do you have enough data to work with?</span></span>
<span data-ttu-id="d564a-170">Végezetül összetevő #4 - igazolnia kell a megfelelő adatokat.</span><span class="sxs-lookup"><span data-stu-id="d564a-170">Finally, ingredient #4 - we need to have enough data.</span></span>

![Elegendő vonatkozó adatok elemzési célú van?](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

<span data-ttu-id="d564a-173">Gondolja, hogy egy ecset stroke elem található a kifestési a tábla minden egyes adatpont.</span><span class="sxs-lookup"><span data-stu-id="d564a-173">Think of each data point in your table as being a brush stroke in a painting.</span></span> <span data-ttu-id="d564a-174">Ha csak néhány őket, lehet, hogy a kifestési közérthető intelligens - nehéz mondja el, mi.</span><span class="sxs-lookup"><span data-stu-id="d564a-174">If you have only a few of them, the painting can be pretty fuzzy - it's hard to tell what it is.</span></span>

<span data-ttu-id="d564a-175">Néhány további ecset vonások való hozzáadásakor a kifestési elindítja az beszerzése kissé tisztább biztosít.</span><span class="sxs-lookup"><span data-stu-id="d564a-175">If you add some more brush strokes, then your painting starts to get a little sharper.</span></span>

<span data-ttu-id="d564a-176">Ha alig elég vonások, láthatja, éppen elegendő néhány széleskörű vonatkozó döntések meghozatalában.</span><span class="sxs-lookup"><span data-stu-id="d564a-176">When you have barely enough strokes, you can see just enough to make some broad decisions.</span></span> <span data-ttu-id="d564a-177">Ennyi az egész valahol előfordulhat, hogy kívánt látogasson el?</span><span class="sxs-lookup"><span data-stu-id="d564a-177">Is it somewhere I might want to visit?</span></span> <span data-ttu-id="d564a-178">A jelek világos, amely a következőképpen néz tiszta vízjel – Igen, ahol fogom szabadságon van.</span><span class="sxs-lookup"><span data-stu-id="d564a-178">It looks bright, that looks like clean water – yes, that’s where I’m going on vacation.</span></span>

<span data-ttu-id="d564a-179">További adatok hozzáadása a kép tisztább válik, és döntéseket lehet részletesebb.</span><span class="sxs-lookup"><span data-stu-id="d564a-179">As you add more data, the picture becomes clearer and you can make more detailed decisions.</span></span> <span data-ttu-id="d564a-180">Most is megtekinthetik a bal oldali bank a három szállodák.</span><span class="sxs-lookup"><span data-stu-id="d564a-180">Now I can look at the three hotels on the left bank.</span></span> <span data-ttu-id="d564a-181">Tudja, valóban tetszik az az előtérben egy architekturális funkcióit.</span><span class="sxs-lookup"><span data-stu-id="d564a-181">You know, I really like the architectural features of the one in the foreground.</span></span> <span data-ttu-id="d564a-182">I marad, a harmadik újraindítják.</span><span class="sxs-lookup"><span data-stu-id="d564a-182">I’ll stay there, on the third floor.</span></span>

<span data-ttu-id="d564a-183">A megfelelő, csatlakoztatott, pontos adatokat, és ahhoz, hogy rendelkezik minden olyan összetevő, igazolnia kell a tegye néhány kiváló minőségű adattudomány.</span><span class="sxs-lookup"><span data-stu-id="d564a-183">With data that's relevant, connected, accurate, and enough, we have all the ingredients we need to do some high-quality data science.</span></span>

<span data-ttu-id="d564a-184">Ügyeljen arra, hogy tekintse meg a többi négy videók *Adattudomány kezdőknek* a Microsoft Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d564a-184">Be sure to check out the other four videos in *Data Science for Beginners* from Microsoft Azure Machine Learning.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d564a-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d564a-185">Next steps</span></span>
* [<span data-ttu-id="d564a-186">Próbálja meg egy első adatok tudományos kísérletben a Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="d564a-186">Try a first data science experiment with Machine Learning Studio</span></span>](machine-learning-create-experiment.md)
* [<span data-ttu-id="d564a-187">A Microsoft Azure Machine Learning bemutatása beolvasása</span><span class="sxs-lookup"><span data-stu-id="d564a-187">Get an introduction to Machine Learning on Microsoft Azure</span></span>](machine-learning-what-is-machine-learning.md)
