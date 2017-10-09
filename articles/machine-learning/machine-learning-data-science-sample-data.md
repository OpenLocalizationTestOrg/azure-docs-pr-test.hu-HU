---
title: "aaaSample adatokat az Azure blob-tárolók, SQL Server és a Hive táblák |} Microsoft Docs"
description: "Tooexplore adatok tárolásának módját a különböző Azure enviromnents."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 80a9dfae-e3a6-4cfb-aecc-5701cfc7e39d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 5a5295b59fa02f91da680fc7495a92ca135e26c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="f3ed7-103"><a name="heading"></a>Az Azure blob-tárolókban, SQL Server-adatokat, és a Hive táblák</span><span class="sxs-lookup"><span data-stu-id="f3ed7-103"><a name="heading"></a>Sample data in Azure blob containers, SQL Server, and Hive tables</span></span>
<span data-ttu-id="f3ed7-104">Ez a dokumentum hivatkozik, amely lefedi tootopics hogyan toosample három különböző Azure helyek egyikén tárolt adatokat:</span><span class="sxs-lookup"><span data-stu-id="f3ed7-104">This document links tootopics that covers how toosample data that is stored in one of three different Azure locations:</span></span>

* <span data-ttu-id="f3ed7-105">**Az Azure blob-tároló adatok** van mintát az programozott módon letöltheti, és majd mintavételi minta Python kóddal.</span><span class="sxs-lookup"><span data-stu-id="f3ed7-105">**Azure blob container data** is sampled by downloading it programmatically and then sampling it with sample Python code.</span></span>
* <span data-ttu-id="f3ed7-106">**SQL Server-adatok** mintát venni, SQL és hello Python programozási nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="f3ed7-106">**SQL Server data** is sampled using both SQL and hello Python Programming Language.</span></span> 
* <span data-ttu-id="f3ed7-107">**Hive tábla adatai** Hive-lekérdezésekkel van mintát.</span><span class="sxs-lookup"><span data-stu-id="f3ed7-107">**Hive table data** is sampled using Hive queries.</span></span>

<span data-ttu-id="f3ed7-108">hello következő **menü** toohello leíró témakörök hivatkozásait hogyan toosample adatokat az egyes az Azure storage környezetben.</span><span class="sxs-lookup"><span data-stu-id="f3ed7-108">hello following **menu** links toohello topics that describe how toosample data from each of these Azure storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="f3ed7-109">Ez a mintavételi feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="f3ed7-109">This sampling task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

<span data-ttu-id="f3ed7-110">**Miért érdemes az adatokat?**</span><span class="sxs-lookup"><span data-stu-id="f3ed7-110">**Why sample data?**</span></span>

<span data-ttu-id="f3ed7-111">Ha azt tervezi, hogy tooanalyze hello adatkészlet túl nagy, a rendszer általában egy jó ötlet toodown-minta hello adatok tooreduce azt tooa kisebb, de reprezentatív és könnyebben kezelhető méretét.</span><span class="sxs-lookup"><span data-stu-id="f3ed7-111">If hello dataset you plan tooanalyze is large, it's usually a good idea toodown-sample hello data tooreduce it tooa smaller but representative and more manageable size.</span></span> <span data-ttu-id="f3ed7-112">Ez lehetővé teszi az adatok ismertetése, feltárása és a szolgáltatás mérnöki csapathoz.</span><span class="sxs-lookup"><span data-stu-id="f3ed7-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="f3ed7-113">Hello Cortana Analytics folyamat a szerepköre tooenable gyors prototípusának hello adatfeldolgozási funkciók és a gépi tanulási modellek.</span><span class="sxs-lookup"><span data-stu-id="f3ed7-113">Its role in hello Cortana Analytics Process is tooenable fast prototyping of hello data processing functions and machine learning models.</span></span>

