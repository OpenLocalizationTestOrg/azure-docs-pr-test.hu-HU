---
title: "az Azure Machine Learning webszolgáltatások Import/Export adatok aaaUsing |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse adatok importálása és exportálása adatok modulok toosend hello és egy webszolgáltatás-bővítmény fogadhat adatokat."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: 3a7ac351-ebd3-43a1-8c5d-18223903d08e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 176380259b15cb338ede61c7f28ba2296b35dd52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Az Adatok importálása és az Adatok exportálása modult használó Azure ML webszolgáltatások üzembe helyezése

Amikor létrehoz egy prediktív kísérletté, általában hozzáadhat egy webszolgáltatás bemenete és a kimeneti. Hello kísérlet telepítésekor fogyasztók küldhet és fogadhat adatokat hello webszolgáltatás hello bemenetekhez és kimenetekhez keresztül. Egyes alkalmazások a fogyasztói adatoktól adatcsatornáról érhető el vagy már található egy külső adatforrásból, például az Azure Blob Storage tárolóban. Ebben az esetben nincs szükségük olvasása és írása az adatokat, és a webszolgáltatás bemenetei és kimenetei. Ugyanakkor, ehelyett hello kötegelt végrehajtási szolgáltatás (BES) tooread adatokat használja az adatok importálása modullal hello adatforrásból és pontozási eredményei tooa különböző adatok helye az adatok exportálása modullal hello írni.

hello adatok importálása és exportálása adatok modulok, lehet olvasni és toovarious az adatok írása például egy webes URL-Címéhez HTTP, a Hive-lekérdezések, egy Azure SQL-adatbázis, Azure Table storage, Azure Blob Storage tárolóban, a adatcsatorna révén helyét adja meg, vagy egy helyi SQL-adatbázis.

Ez a témakör használ hello "minta 5: vonat, tesztelési, Evaluate bináris osztályozási: felnőtt Dataset" sample, és azt feltételezi, hogy hello dataset már be van töltve nevű censusdata Azure SQL-táblába.

## <a name="create-hello-training-experiment"></a>Hello képzési kísérlet létrehozása
Hello megnyitásakor "minta 5: vonat, tesztelési, Evaluate bináris osztályozási: felnőtt Dataset" mintát azt használja hello minta bináris osztályozási felnőtt nyilvántartásba bevétel adatkészlet. És hello vásznon a hello kísérlet a következő kép hasonló toohello fog kinézni:

![Kezdeti konfigurációs hello kísérlet.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)

hello Azure SQL-tábla tooread hello adatait:

1. Hello dataset modul törlése.
2. Hello összetevők keresési mezőbe írja be az importálási.
3. Hello eredménylistában hozzá egy *és adatokat importálhat* modul toohello kísérlet vászonra.
4. Csatlakozás hello kimenete *és adatokat importálhat* modul hello bemeneti hello *Clean Missing Data* modul.
5. A Tulajdonságok panelen, jelölje ki **Azure SQL Database** a hello **adatforrás** legördülő menüből.
6. A hello **adatbázis-kiszolgáló neve**, **adatbázisnév**, **felhasználónév**, és **jelszó** mezők, adja meg a megfelelő információkat hello az adatbázis.
7. Hello adatbázis-lekérdezés mezőjébe írja be a következő lekérdezés hello.
   
     Válassza ki a [kora]
   
        [workclass],
        [fnlwgt],
        [education],
        [education-num],
        [marital-status],
        [occupation],
        [relationship],
        [race],
        [sex],
        [capital-gain],
        [capital-loss],
        [hours-per-week],
        [native-country],
        [income]
     a dbo.censusdata;
8. A kísérletvászonra hello hello alján kattintson **futtatása**.

## <a name="create-hello-predictive-experiment"></a>Hello prediktív kísérlet létrehozása
Ezután hello prediktív kísérletté, ahol a webszolgáltatás telepítése beállítása.

1. A kísérletvászonra hello hello alján kattintson **webes szolgáltatások beállítása** válassza **prediktív webszolgáltatás [ajánlott]**.
2. Távolítsa el a hello *webszolgáltatás bemenetét* és *webes szolgáltatás kimeneti modulok* hello prediktív kísérlet. 
3. Hello összetevők keresési mezőbe írja be az exportálás.
4. Hello eredménylistában hozzá egy *adatok exportálása* modul toohello kísérlet vászonra.
5. Csatlakozás hello kimenete *Score Model* modul hello bemeneti hello *adatok exportálása* modul. 
6. A Tulajdonságok panelen, jelölje ki **Azure SQL Database** hello adatok cél legördülő.
7. A hello **adatbázis-kiszolgáló neve**, **adatbázisnév**, **kiszolgáló felhasználói fiók nevét**, és **kiszolgáló felhasználói fiók jelszavát** mezőkben adja meg az adatbázis hello megfelelő adatokat.
8. A hello **vesszővel tagolt listája mentett oszlopok toobe** mezőbe írja be a pontozott címkék.
9. A hello **tábla neve adatmező**, írja be a dbo. ScoredLabels. Hello tábla nem létezik, ha létrejön hello kísérlet futtatása vagy hello webszolgáltatás nevezik.
10. A hello **vesszővel tagolt listája datatable oszlopok** mezőbe írja be a ScoredLabels.

Egy alkalmazás írásakor, hogy a hívások hello végső webszolgáltatás, érdemes lehet toospecify egy másik bemeneti lekérdezés vagy a cél táblázatban futási időben. tooconfigure ezek bemenetekhez és kimenetekhez, használja a hello webszolgáltatás-paramétereket szolgáltatás tooset hello *és adatokat importálhat* modul *adatforrás* tulajdonság és hello *adatok exportálása* mód adatok cél tulajdonság.  A webszolgáltatás-paramétereket további információkért lásd: hello [AzureML webszolgáltatás-paramétereket bejegyzés](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) hello Cortana Intelligence és a Machine Learning Blog.

hello importálási lekérdezés és hello céltábla tooconfigure hello webszolgáltatás-paramétereket:

1. Hello tulajdonságok panelen az hello *és adatokat importálhat* modul, hello hello ikonra kattintson jobb oldalán hello felső **adatbázis-lekérdezés** mezőben, majd válassza ki **webszolgáltatási paraméter beállítása**.
2. Hello tulajdonságok panelen az hello *adatok exportálása* modul, hello hello ikonra kattintson jobb oldalán hello felső **adatok táblanév** mezőben, majd válassza ki **webszolgáltatási paraméter beállítása**.
3. Hello hello alján *adatok exportálása* modul tulajdonságok paneljén, hello **webszolgáltatás-paramétereket** szakaszt, kattintson az adatbázis-lekérdezés és lekérdezés nevezni.
4. Kattintson a **adatok táblanév** és adjon neki **tábla**.

Amikor elkészült, a kísérlet a következő kép hasonló toohello kell kinéznie:

![Kísérlet végső megjelenését.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Most már telepítheti hello kísérlet webszolgáltatásként.

## <a name="deploy-hello-web-service"></a>Hello webszolgáltatás telepítése
Tooeither telepíthet egy klasszikus vagy az új webszolgáltatás-bővítmény.

### <a name="deploy-a-classic-web-service"></a>Klasszikus webes szolgáltatás telepítése
Klasszikus webszolgáltatásként toodeploy, és hozzon létre egy alkalmazás tooconsume azt:

1. Hello kísérletvászonra hello alsó részén kattintson a Futtatás gombra.
2. Futtatás hello befejezése után kattintson **webes szolgáltatás telepítése** válassza **webes szolgáltatás telepítése [klasszikus]**.
3. Hello webes szolgáltatás irányítópultját keresse meg az API-kulcs. Másolja ki és mentse toouse később.
4. A hello **alapértelmezett végpont** table, kattintson a hello **kötegelt végrehajtási** hivatkozás tooopen hello API Súgóoldalát.
5. A Visual Studio, hozzon létre egy C# konzolalkalmazást: **új** > **projekt** > **Visual C#** > **Windows Klasszikus asztal** > **Konzolalkalmazásból (.NET-keretrendszer)**.
6. Hello API Súgóoldalát, megtalálhatja hello **mintakód** alján hello hello szakasz.
7. Másolja és hello C# mintakód illessze be a Program.cs fájlt, és távolítsa el az összes hivatkozás toohello blob-tároló.
8. Hello hello értékét *apiKey* változó korábban mentett hello API-kulccsal.
9. Keresse meg a hello kérelem nyilatkozat és frissítés hello értékei toohello átadott webszolgáltatás-paramétereket *és adatokat importálhat* és *adatok exportálása* modulok. Ebben az esetben akkor hello eredeti lekérdezéssel, de egy új táblázat nevének meghatározása.
   
        var request = new BatchExecutionRequest() 
        {           
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. Hello alkalmazás futtatásához. 

Futtatás hello befejezésekor egy új hozzá pontozási eredményei hello tartalmazó toohello adatbázis.

### <a name="deploy-a-new-web-service"></a>Egy új webszolgáltatás-bővítmény telepítése

> [!NOTE] 
> toodeploy egy új webszolgáltatás-bővítmény, megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich meg hello webes szolgáltatás telepítéséhez. További információkért lásd: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md). 

egy új webszolgáltatás toodeploy, és hozzon létre egy alkalmazás tooconsume azt:

1. A kísérletvászonra hello hello alján kattintson **futtatása**.
2. Futtatás hello befejezése után kattintson **webes szolgáltatás telepítése** válassza **[Új] webes szolgáltatás telepítése**.
3. Hello telepítése kísérlet lapon adja meg egy nevet a webszolgáltatás, és válassza ki a tarifacsomagot, majd kattintson **telepítés**.
4. A hello **gyors üzembe helyezés** kattintson **felhasználás**.
5. A hello **mintakód** kattintson **kötegelt**.
6. A Visual Studio, hozzon létre egy C# konzolalkalmazást: **új** > **projekt** > **Visual C#** > **Windows Klasszikus asztal** > **Konzolalkalmazásból (.NET-keretrendszer)**.
7. Másolással illessze be a C# mintakód hello a Program.cs fájlba.
8. Hello hello értékét *apiKey* változó, hello **elsődleges kulcs** hello található **alapvető fogyasztási adatai** szakasz.
9. Keresse meg a hello *scoreRequest* toohello átadott webszolgáltatás-paramétereket nyilatkozat és a frissítés hello értékei *és adatokat importálhat* és *adatok exportálása* modulok. Ebben az esetben akkor hello eredeti lekérdezéssel, de egy új táblázat nevének meghatározása.
   
        var scoreRequest = new
        {       
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };
10. Hello alkalmazás futtatásához. 

