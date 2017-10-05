---
title: "Az Azure Data Lake Analytics U-SQL kognitív képességek segítségével |} Microsoft Docs"
description: "Az eszközintelligencia kognitív képességek használata U-SQL-ben"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: sukvg
editor: cgronlun
ms.assetid: 019c1d53-4e61-4cad-9b2c-7a60307cbe19
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: f77329f9838d6e824afa7234de90f62257a004de
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-get-started-with-the-cognitive-capabilities-of-u-sql"></a><span data-ttu-id="5da79-103">Oktatóanyag: Ismerkedés a U-SQL kognitív lehetőségeinek</span><span class="sxs-lookup"><span data-stu-id="5da79-103">Tutorial: Get started with the Cognitive capabilities of U-SQL</span></span>

<span data-ttu-id="5da79-104">A fejlesztők kognitív képességet biztosít a U-SQL helyezze az eszközintelligencia a big Data típusú adatok programok telepítése és használata.</span><span class="sxs-lookup"><span data-stu-id="5da79-104">Cognitive capabilities for U-SQL enable developers to use put intelligence in their big data programs.</span></span> <span data-ttu-id="5da79-105">A teljes folyamat egyszerű:</span><span class="sxs-lookup"><span data-stu-id="5da79-105">The overall process in simple:</span></span>

* <span data-ttu-id="5da79-106">A referencia szerelvény utasítás használható a U-SQL parancsfájl kognitív funkciók lehetővé tétele</span><span class="sxs-lookup"><span data-stu-id="5da79-106">Use the REFERENCE ASSEMBLY statement to enable the cognitive features for the U-SQL Script</span></span>
* <span data-ttu-id="5da79-107">A folyamat során a rendszer kognitív lehetőségek hívása</span><span class="sxs-lookup"><span data-stu-id="5da79-107">Call the PROCESS operation to use the Cognitive capabilities</span></span> 

## <a name="imaging-scenarios"></a><span data-ttu-id="5da79-108">Képkezelő forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="5da79-108">Imaging scenarios</span></span>

### <a name="example-image-tagging"></a><span data-ttu-id="5da79-109">Példa: Kép címkézése</span><span class="sxs-lookup"><span data-stu-id="5da79-109">Example: Image tagging</span></span>

<span data-ttu-id="5da79-110">A következő példa bemutatja a lemezkép-készítési képességek objektumok azonosíthatók a képek egy végpontok közötti használatát.</span><span class="sxs-lookup"><span data-stu-id="5da79-110">The following example shows an end-to-end use of the imaging capabilities to detect objects in images.</span></span>

    REFERENCE ASSEMBLY ImageCommon;
    REFERENCE ASSEMBLY FaceSdk;
    REFERENCE ASSEMBLY ImageEmotion;
    REFERENCE ASSEMBLY ImageTagging;
    REFERENCE ASSEMBLY ImageOcr;

    @imgs =
        EXTRACT FileName string, ImgData byte[]
        FROM @"/images/{FileName:*}.jpg"
        USING new Cognition.Vision.ImageExtractor();

    // Extract the number of objects on each image and tag them 
    @objects =
        PROCESS @imgs 
        PRODUCE FileName,
                NumObjects int,
                Tags string
        READONLY FileName
        USING new Cognition.Vision.ImageTagger();


### <a name="extract-emotions-from-human-faces"></a><span data-ttu-id="5da79-111">Érzelmek kinyerése emberi felületei</span><span class="sxs-lookup"><span data-stu-id="5da79-111">Extract emotions from human faces</span></span> 

    @emotions =
        PROCESS @imgs
        PRODUCE FileName string,
                NumFaces int,
                Emotion string
        READONLY FileName
        USING new Cognition.Vision.EmotionAnalyzer();

### <a name="estimate-age-and-gender-for-human-faces"></a><span data-ttu-id="5da79-112">Becsült kora és az emberi lapok nemét</span><span class="sxs-lookup"><span data-stu-id="5da79-112">Estimate age and gender for human faces</span></span>

    @faces = 
            PROCESS @imgs
            PRODUCE FileName,
                    NumFaces int,
                    FaceAge string,
                    FaceGender string
            READONLY FileName
            USING new Cognition.Vision.FaceDetector();

### <a name="detect-text-in-images-ocr"></a><span data-ttu-id="5da79-113">Szöveg képek (OCR) észlelése</span><span class="sxs-lookup"><span data-stu-id="5da79-113">Detect text in Images (OCR)</span></span>

    @ocrs =
            PROCESS @imgs
            PRODUCE FileName,
                    Text string
            READONLY FileName
            USING new Cognition.Vision.OcrExtractor();

## <a name="text-scenarios"></a><span data-ttu-id="5da79-114">Szöveg forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="5da79-114">Text scenarios</span></span>

### <a name="input-data"></a><span data-ttu-id="5da79-115">A bemeneti adatok</span><span class="sxs-lookup"><span data-stu-id="5da79-115">Input data</span></span>

<span data-ttu-id="5da79-116">Tegyük fel, amely "A War és a nyugodt maradhat" áll bemeneti tudunk Leo Tolstoy által.</span><span class="sxs-lookup"><span data-stu-id="5da79-116">Assume that we have an input that consists of “War and Peace” by Leo Tolstoy.</span></span>

    REFERENCE ASSEMBLY [TextCommon];
    REFERENCE ASSEMBLY [TextSentiment];
    REFERENCE ASSEMBLY [TextKeyPhrase];

    @WarAndPeace =
        EXTRACT No int,
                Year string,
                Book string,
                Chapter string,
                Text string
        FROM @"/usqlext/samples/cognition/war_and_peace.csv"
        USING Extractors.Csv();

### <a name="extract-key-phrases-for-each-paragraph"></a><span data-ttu-id="5da79-117">Bontsa ki az egyes bekezdések legfontosabb kifejezések</span><span class="sxs-lookup"><span data-stu-id="5da79-117">Extract key phrases for each paragraph</span></span>

    @keyphrase =
        PROCESS @WarAndPeace
        PRODUCE No,
                Year,
                Book,
                Chapter,
                Text,
                KeyPhrase string
        READONLY No,
                Year,
                Book,
                Chapter,
                Text
        USING new Cognition.Text.KeyPhraseExtractor();

    // Tokenize the key phrases.
    @kpsplits =
        SELECT No,
            Year,
            Book,
            Chapter,
            Text,
            T.KeyPhrase
        FROM @keyphrase
            CROSS APPLY
                new Cognition.Text.Splitter("KeyPhrase") AS T(KeyPhrase);
    
### <a name="perform-sentiment-analysis-on-each-paragraph"></a><span data-ttu-id="5da79-118">Minden bekezdéséhez véleményeket elemzést</span><span class="sxs-lookup"><span data-stu-id="5da79-118">Perform sentiment analysis on each paragraph</span></span>

    @sentiment =
        PROCESS @WarAndPeace
        PRODUCE No,
                Year,
                Book,
                Chapter,
                Text,
                Sentiment string,
                Conf double
        READONLY No,
                Year,
                Book,
                Chapter,
                Text
        USING new Cognition.Text.SentimentAnalyzer(true);

