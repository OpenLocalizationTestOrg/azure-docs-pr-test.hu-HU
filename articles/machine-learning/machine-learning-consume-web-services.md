---
title: az Azure Machine Learning Web service aaaHow tooconsume |} Microsoft Docs
description: "A machine learning szolgáltatás telepítése után hello RESTFul webszolgáltatás, amelyet szeretné elérhetővé tenni a kötegelt végrehajtási szolgáltatásként vagy a valós idejű kérés-válasz szolgáltatás képes használni."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 804f8211-9437-4982-98e9-ca841b7edf56
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/02/2017
ms.author: garye
ms.openlocfilehash: 19095604169e5af1daed12c17ba66258233178bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconsume-an-azure-machine-learning-web-service"></a>Hogyan tooconsume az Azure Machine Learning Web service

Miután telepít egy Azure Machine Learning a prediktív modell webszolgáltatásként, használhatja a REST API toosend az adatok és az előrejelzés. Hello adatokat küldhet a valós idejű vagy kötegelt módban.

További információ található toocreate és központi telepítése a Machine Learning Web service Itt a Machine Learning Studio használatával:

* Hogyan toocreate egy kísérletben a Machine Learning Studióban, tekintse meg a oktatóanyagért [az első kísérlet létrehozása](machine-learning-create-experiment.md).
* További részletekért toodeploy egy webszolgáltatás-bővítmény, lásd: [egy Machine Learning webszolgáltatás-bővítmény telepítése](machine-learning-publish-a-machine-learning-web-service.md).
* További információ a gépi tanulás általában a Microsoft hello [Machine Learning dokumentációs központban](https://azure.microsoft.com/documentation/services/machine-learning/).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview"></a>Áttekintés
Hello Azure Machine Learning webszolgáltatás, a külső alkalmazás kommunikál a Machine Learning munkafolyamat pontozási modell valós időben. A Machine Learning webszolgáltatás-hívások tooan külső alkalmazás előrejelzés eredményeket ad vissza. a Machine Learning webszolgáltatás-hívások toomake, az előrejelzéshez telepítésekor létrehozott API-kulcs át. Machine Learning webszolgáltatás hello REST, olyan népszerű architektúrával kapcsolatos választási lehetőség webes projektek programozási alapul.

Az Azure Machine Learning két különböző típusú szolgáltatást tud biztosítani:

* Kérés-válasz szolgáltatás (RR-EKET) – egy kis késés, magas szinten méretezhető illesztőfelület-toohello állapot nélküli modellekhez létrehozott és telepített, a Machine Learning Studio hello biztosító szolgáltatás.
* Kötegelt végrehajtási szolgáltatás (BES) –-egy aszinkron szolgáltatást, hogy a rekordok köteg pontszámait.

További információ a Machine Learning webszolgáltatások: [egy Machine Learning webszolgáltatás-bővítmény telepítése](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Azure Machine Learning engedélyezési kulcs beszerzése
Amikor telepíti a kísérletet, API-kulcsokat hello webszolgáltatás jön létre. Hello kulcsok kérhetnek le több helyre.

### <a name="from-hello-microsoft-azure-machine-learning-web-services-portal"></a>Hello Microsoft Azure Machine Learning webszolgáltatások portálról
Jelentkezzen be toohello [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net) portálon.

tooretrieve hello API-kulcs egy új Machine Learning webszolgáltatáshoz:

1. Hello Azure Machine Learning webszolgáltatások portálon kattintson **webszolgáltatások** hello felső menüjében.
2. Kattintson a kívánt tooretrieve hello kulcs hello webszolgáltatáshoz.
3. Kattintson a felső menüben hello **felhasználás**.
4. Másolja ki és mentse a hello **elsődleges kulcs**.

tooretrieve hello API-kulcs egy klasszikus Machine Learning webszolgáltatáshoz:

1. Hello Azure Machine Learning webszolgáltatások portálon kattintson **klasszikus webszolgáltatások** hello felső menüjében.
2. Kattintson a hello webes szolgáltatás, amellyel dolgozik.
3. Kattintson a kívánt tooretrieve hello kulcs hello végpontra.
4. Kattintson a felső menüben hello **felhasználás**.
5. Másolja ki és mentse a hello **elsődleges kulcs**.

### <a name="classic-web-service"></a>Klasszikus webszolgáltatás
 A kulcs a klasszikus webszolgáltatáshoz beolvasható a Machine Learning Studio vagy a klasszikus Azure portálon hello.

#### <a name="machine-learning-studio"></a>Machine Learning Studio
1. A Machine Learning Studióban, kattintson a **WEBSZOLGÁLTATÁSOK** hello bal oldalon.
2. Kattintson egy webszolgáltatás-bővítmény. Hello **API-kulcs** a hello **IRÁNYÍTÓPULT** fülre.

#### <a name="azure-classic-portal"></a>klasszikus Azure portál
1. Kattintson a **MACHINE LEARNING** hello bal oldalon.
2. Kattintson a hello munkaterületen, ahol a webszolgáltatás is található.
3. Kattintson a **WEBSZOLGÁLTATÁSOK**.
4. Kattintson egy webszolgáltatás-bővítmény.
5. Kattintson egy végpontot. hello "API-kulcsot" hello jobb alsó, nem működik.

## <a id="connect"></a>Csatlakozás tooa Machine Learning webszolgáltatás
Machine Learning Web service bármely programozási nyelvet, amely támogatja a HTTP-kérelem-válasz tooa is elérheti. A Machine Learning webszolgáltatás súgólapja a példákban a C#, Python, és R tekintheti meg.

**Számítógép-Learning API súgó** Machine Learning API-súgó egy webszolgáltatás-bővítmény telepítésekor létrejön. Lásd: [Azure Machine Learning forgatókönyv - webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md).
Machine Learning API súgó hello webszolgáltatás előrejelzéshez részleteit tartalmazza.

1. Kattintson a hello webes szolgáltatás, amellyel dolgozik.
2. Kattintson a kívánt tooview hello API Súgóoldalát hello végpontra.
3. Kattintson a felső menüben hello **felhasználás**.
4. Kattintson a **API súgólap** hello kérés-válasz vagy kötegelt végrehajtási végpontok alatt.

**tooview a Machine Learning API súgóját egy új webszolgáltatás-bővítmény**

Az Azure Machine Learning Web Services portálra. hello:

1. Kattintson a **WEBSZOLGÁLTATÁSOK** hello felső menüben.
2. Kattintson a kívánt tooretrieve hello kulcs hello webszolgáltatáshoz.

Kattintson a **felhasználás** tooget hello URI-azonosítók hello kérelem-Reposonse és a C#, R és Python kötegelt végrehajtási szolgáltatás és a minta kódot.

Kattintson a **Swagger API** tooget hello API-k alapján dokumentációja hívása hello Swagger megadott URI-azonosítók.

### <a name="c-sample"></a>C# minta
tooconnect tooa Machine Learning webszolgáltatás, használjon egy **HttpClient** ScoreData átadásakor. ScoreData FeatureVector, egy numerikus szolgáltatások n dimenziós vektort hello ScoreData jelölő tartalmazza. API-kulcs a Machine Learning szolgáltatás toohello hitelesítést.

Machine Learning webszolgáltatás, tooconnect tooa hello **Microsoft.AspNet.WebApi.Client** NuGet-csomagot kell telepíteni.

**Telepítse a Microsoft.AspNet.WebApi.Client NuGet a Visual Studióban**

1. UCI hello letöltési adatkészlet közzététele: felnőtt 2 osztály dataset webszolgáltatáshoz.
2. Kattintson a **Tools** (Eszközök)  > **NuGet Package Manager** (NuGet-csomagkezelő) > **Package Manager Console** (Csomagkezelő konzol) elemre.
3. Válasszon **Install-Package Microsoft.AspNet.WebApi.Client**.

**toorun hello kódminta**

1. Közzététele "minta 1: dataset letöltését UCI: 2 felnőtt osztály dataset" kísérlet, a Machine Learning mintavételt hello részét.
2. Adjon meg apiKey hello kulccsal egy webszolgáltatás-bővítmény. Lásd: **Azure Machine Learning engedélyezési kulcs beszerzése** felett.
3. A kérelem URI-azonosítója hello serviceUri hozzárendelése.

### <a name="python-sample"></a>Python-minta
tooconnect tooa Machine Learning webszolgáltatás, használja a hello **urllib2** könyvtár ScoreData átadásakor. ScoreData FeatureVector, egy numerikus szolgáltatások n dimenziós vektort hello ScoreData jelölő tartalmazza. API-kulcs a Machine Learning szolgáltatás toohello hitelesítést.

**toorun hello kódminta**

1. Telepítése "minta 1: dataset letöltését UCI: 2 felnőtt osztály dataset" kísérlet, a Machine Learning mintavételt hello részét.
2. Adjon meg apiKey hello kulccsal egy webszolgáltatás-bővítmény. Lásd: hello **Azure Machine Learning engedélyezési kulcs beszerzése** közelében hello kezdete című szakaszát.
3. A kérelem URI-azonosítója hello serviceUri hozzárendelése.

