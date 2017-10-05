---
title: "Az Azure blob-tárolókban, SQL Server-adatokat, és a Hive táblák |} Microsoft Docs"
description: "Hogyan különböző Azure enviromnents tárolt adatokba."
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
ms.openlocfilehash: 0683be564a88ef54882e8d882d196851ecac243d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="7e4ab-103"><a name="heading"></a>Az Azure blob-tárolókban, SQL Server-adatokat, és a Hive táblák</span><span class="sxs-lookup"><span data-stu-id="7e4ab-103"><a name="heading"></a>Sample data in Azure blob containers, SQL Server, and Hive tables</span></span>
<span data-ttu-id="7e4ab-104">Ez a dokumentum témakörökre mutató hivatkozásokat tartalmaz, amely bemutatja, hogyan adhat az három különböző Azure helyek egyikén tárolt adatokat:</span><span class="sxs-lookup"><span data-stu-id="7e4ab-104">This document links to topics that covers how to sample data that is stored in one of three different Azure locations:</span></span>

* <span data-ttu-id="7e4ab-105">**Az Azure blob-tároló adatok** van mintát az programozott módon letöltheti, és majd mintavételi minta Python kóddal.</span><span class="sxs-lookup"><span data-stu-id="7e4ab-105">**Azure blob container data** is sampled by downloading it programmatically and then sampling it with sample Python code.</span></span>
* <span data-ttu-id="7e4ab-106">**SQL Server-adatok** mintát venni, SQL és a Python programozási nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="7e4ab-106">**SQL Server data** is sampled using both SQL and the Python Programming Language.</span></span> 
* <span data-ttu-id="7e4ab-107">**Hive tábla adatai** Hive-lekérdezésekkel van mintát.</span><span class="sxs-lookup"><span data-stu-id="7e4ab-107">**Hive table data** is sampled using Hive queries.</span></span>

<span data-ttu-id="7e4ab-108">A következő **menü** az adatokat az Azure storage környezetben minden egyes a leíró témakörök hivatkozásait.</span><span class="sxs-lookup"><span data-stu-id="7e4ab-108">The following **menu** links to the topics that describe how to sample data from each of these Azure storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="7e4ab-109">Ez a mintavételi feladat Ez a lépés a [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="7e4ab-109">This sampling task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

<span data-ttu-id="7e4ab-110">**Miért érdemes az adatokat?**</span><span class="sxs-lookup"><span data-stu-id="7e4ab-110">**Why sample data?**</span></span>

<span data-ttu-id="7e4ab-111">Ha azt tervezi, hogy elemezheti az adatkészlet túl nagy, akkor általában down kétmintás az adatokat, hogy az kisebb, de reprezentatív és könnyebben kezelhető méretű jó ötlet.</span><span class="sxs-lookup"><span data-stu-id="7e4ab-111">If the dataset you plan to analyze is large, it's usually a good idea to down-sample the data to reduce it to a smaller but representative and more manageable size.</span></span> <span data-ttu-id="7e4ab-112">Ez lehetővé teszi az adatok ismertetése, feltárása és a szolgáltatás mérnöki csapathoz.</span><span class="sxs-lookup"><span data-stu-id="7e4ab-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="7e4ab-113">A szerepkör a Cortana Analytics folyamat ahhoz, hogy az adatok feldolgozása funkciók és a gépi tanulási modellek gyors prototípusának.</span><span class="sxs-lookup"><span data-stu-id="7e4ab-113">Its role in the Cortana Analytics Process is to enable fast prototyping of the data processing functions and machine learning models.</span></span>

