---
title: "Csatlakozás egy Machine Learning webszolgáltatás |} Microsoft Docs"
description: "A C# vagy Python az Azure Machine Learning Web engedélykulcs szolgáltatáshoz kapcsolódni."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 59b07bab-b60f-48c4-a385-a162e50ec7c2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: garye
ROBOTS: NOINDEX
redirect_url: machine-learning-consume-web-services
redirect_document_id: TRUE
ms.openlocfilehash: 0fc6c7e921b18eb14a95fb737d8fb5ab5cc7e687
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-an-azure-machine-learning-web-service"></a>Kapcsolódás az Azure Machine Learning webszolgáltatásokhoz
Az Azure Machine Learning-fejlesztők számára a webszolgáltatási API a bemeneti adatokból előrejelzéseket készítsen a valós idejű vagy kötegelt módban. Azure Machine Learning Studio használatával előrejelzéseket létrehozása és telepítése az Azure Machine Learning Web service.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Megtudhatja, hogyan hozhat létre és telepíthet egy Machine Learning webszolgáltatás Machine Learning Studio használatával kapcsolatos:

* A kísérlet létrehozása a Machine Learning Studio oktatóanyag, lásd: [az első kísérlet létrehozása](machine-learning-create-experiment.md).
* Egy webszolgáltatás-bővítmény telepítésével, lásd: [egy Machine Learning webszolgáltatás-bővítmény telepítése](machine-learning-publish-a-machine-learning-web-service.md).
* További információ a gépi tanulás általában, látogasson el a [Machine Learning dokumentációs központban](https://azure.microsoft.com/documentation/services/machine-learning/).

## <a name="azure-machine-learning-web-service"></a>Az Azure Machine Learning webszolgáltatás
Az Azure Machine Learning Web Service külső alkalmazás kommunikál a Machine Learning munkafolyamat pontozási modell valós időben. A Machine Learning webszolgáltatás-hívás a külső alkalmazás előrejelzés eredményeket ad vissza. Ahhoz, hogy egy Machine Learning webszolgáltatás hívja, át előrejelzéshez telepítésekor létrehozott API-kulcs. A Machine Learning webszolgáltatás REST, olyan népszerű architektúrával kapcsolatos választási lehetőség webes projektek programozási alapul.

Az Azure Machine Learning két különböző típusú szolgáltatást tud biztosítani:

* Kérés-válasz szolgáltatás (RR-EKET) – egy kis késés, az állapot nélküli modellekhez létrehozott és telepített felületet biztosít a Machine Learning Studio a magas szinten méretezhető szolgáltatás.
* Kötegelt végrehajtási szolgáltatás (BES) –-egy aszinkron szolgáltatást, hogy a rekordok köteg pontszámait.

További információ a Machine Learning webszolgáltatások: [egy Machine Learning webszolgáltatás-bővítmény telepítése](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Azure Machine Learning engedélyezési kulcs beszerzése
Amikor telepíti a kísérletet, API-kulcsokat jönnek létre a webszolgáltatás. A kulcsok kérhetnek le több helyre.

### <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>A Microsoft Azure Machine Learning webszolgáltatások portálról
Jelentkezzen be a [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net) portálon.

Egy új Machine Learning webszolgáltatáshoz API-kulcs beolvasása:

1. Az Azure Machine Learning webszolgáltatások portálon kattintson **webszolgáltatások** a felső menüben.
2. Kattintson a webszolgáltatás legyen a kulcs beolvasása.
3. Kattintson a felső menüben **felhasználás**.
4. Másolja ki és mentse a **elsődleges kulcs**.

Az API-kulcs a klasszikus Machine Learning webszolgáltatás beolvasása:

1. Az Azure Machine Learning webszolgáltatások portálon kattintson **klasszikus webszolgáltatások** a felső menüben.
2. Kattintson a webes szolgáltatás, amellyel dolgozik.
3. Kattintson a végpont legyen a kulcs beolvasása.
4. Kattintson a felső menüben **felhasználás**.
5. Másolja ki és mentse a **elsődleges kulcs**.

### <a name="classic-web-service"></a>Klasszikus webszolgáltatás
 A kulcs a klasszikus webszolgáltatáshoz beolvasható a Machine Learning Studio vagy a klasszikus Azure portálon.

#### <a name="machine-learning-studio"></a>Machine Learning Studio
1. A Machine Learning Studióban, kattintson a **WEBSZOLGÁLTATÁSOK** a bal oldalon.
2. Kattintson egy webszolgáltatás-bővítmény. A **API-kulcs** megtalálható a **IRÁNYÍTÓPULT** fülre.

#### <a name="azure-classic-portal"></a>klasszikus Azure portál
1. Kattintson a **MACHINE LEARNING** a bal oldalon.
2. Kattintson a munkaterületen, ahol a webszolgáltatás.
3. Kattintson a **WEBSZOLGÁLTATÁSOK**.
4. Kattintson egy webszolgáltatás-bővítmény.
5. Kattintson egy végpontot. A "API-kulcsot" nem működik az alsó sarokban.

## <a id="connect"></a>Csatlakozás egy Machine Learning webszolgáltatás-bővítmény
A Machine Learning Web service bármely programozási nyelvet, amely támogatja a HTTP-kérelem-válasz kapcsolódhat. A Machine Learning webszolgáltatás súgólapja a példákban a C#, Python, és R tekintheti meg.

**Számítógép-Learning API súgó** Machine Learning API-súgó egy webszolgáltatás-bővítmény telepítésekor létrejön. Lásd: [Azure Machine Learning forgatókönyv - webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md).
A Machine Learning API súgó webszolgáltatás előrejelzéshez részleteit tartalmazza.

1. Kattintson a webes szolgáltatás, amellyel dolgozik.
2. Kattintson a végpont, amelynek meg szeretné tekinteni a API Súgóoldalát.
3. Kattintson a felső menüben **felhasználás**.
4. Kattintson a **API súgólap** alatt a kérelem / válasz vagy kötegelt végrehajtási végpontok.

**Az új webszolgáltatás nézet Machine Learning API segítségével**

Az Azure gépi tanulási webportál szolgáltatások:

1. Kattintson a **WEBSZOLGÁLTATÁSOK** a felső menüben.
2. Kattintson a webszolgáltatás legyen a kulcs beolvasása.

Kattintson a **felhasználás** az URI-azonosítók lekérése a kérelem-Reposonse és kötegelt végrehajtási szolgáltatás és a minta kód a C#, R és Python.

Kattintson a **Swagger API** megszerezni a Swagger-alapú dokumentáció számára az API-k a megadott URI-k hívását.

### <a name="c-sample"></a>C# minta
Csatlakozás egy Machine Learning webszolgáltatáshoz, használjon egy **HttpClient** ScoreData átadásakor. ScoreData FeatureVector, egy numerikus szolgáltatások n dimenziós vektort a ScoreData jelölő tartalmazza. A hitelesítést a Machine Learning szolgáltatás API-kulcs.

Csatlakozás egy Machine Learning webszolgáltatáshoz, a **Microsoft.AspNet.WebApi.Client** NuGet-csomagot kell telepíteni.

**Telepítse a Microsoft.AspNet.WebApi.Client NuGet a Visual Studióban**

1. A letöltési UCI adatkészlet közzététele: felnőtt 2 osztály dataset webszolgáltatáshoz.
2. Kattintson a **Tools** (Eszközök)  > **NuGet Package Manager** (NuGet-csomagkezelő) > **Package Manager Console** (Csomagkezelő konzol) elemre.
3. Válasszon **Install-Package Microsoft.AspNet.WebApi.Client**.

**A kód a minta futtatásához**

1. Közzététele "1. példa: dataset letöltését UCI: felnőtt 2 osztály adatkészlet" kísérlet, a Machine Learning minta gyűjtemény része.
2. Rendelje hozzá a kulccsal apiKey webszolgáltatásból. Lásd: **Azure Machine Learning engedélyezési kulcs beszerzése** felett.
3. A kérelem URI-azonosítójú serviceUri hozzárendelése.

### <a name="python-sample"></a>Python-minta
Csatlakozás egy Machine Learning webszolgáltatáshoz, használja a **urllib2** könyvtár ScoreData átadásakor. ScoreData FeatureVector, egy numerikus szolgáltatások n dimenziós vektort a ScoreData jelölő tartalmazza. A hitelesítést a Machine Learning szolgáltatás API-kulcs.

**A kód a minta futtatásához**

1. Telepítése "minta 1: dataset letöltését UCI: 2 felnőtt osztály dataset" kísérlet, a Machine Learning minta gyűjtemény része.
2. Rendelje hozzá a kulccsal apiKey webszolgáltatásból. Tekintse meg a **Azure Machine Learning engedélyezési kulcs beszerzése** elején című szakaszát.
3. A kérelem URI-azonosítójú serviceUri hozzárendelése.

