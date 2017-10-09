---
title: "aaaRetrain Machine Learning modellek szoftveres átképezése |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprogrammatically újratanítása a modell és a frissítés hello webes toouse hello újonnan betanított modell az Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: 7ae4f977-e6bf-4d04-9dde-28a66ce7b664
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raymondl;garye;v-donglo
ms.openlocfilehash: edbb64c08f7d9edf3c76e23e0cc7e14c0125d697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-machine-learning-models-programmatically"></a>Azure Machine Learning-modellek szoftveres átképezése
Ez a forgatókönyv megtudhatja, hogyan tooprogrammatically működik az Azure Machine Learning webszolgáltatás C# és hello Machine Learning kötegelt végrehajtási szolgáltatás használatával.

Hello modell rendelkezik retrained, a következő forgatókönyvek hello megjelenni hogyan tooupdate hello modell a prediktív webszolgáltatás:

* Ha telepítette a klasszikus webszolgáltatás hello Machine Learning webszolgáltatások portálon, lásd: [egy klasszikus webszolgáltatás újratanítása](machine-learning-retrain-a-classic-web-service.md). 
* Ha egy új webszolgáltatás-bővítmény központi telepítése, lásd: [egy új webszolgáltatás-bővítmény hello Machine Learning Management-parancsmagok használatával újratanítása](machine-learning-retrain-new-web-service-using-powershell.md).

Folyamat átképezési hello áttekintését lásd: [a gépi tanulási modellek újratanítása](machine-learning-retrain-machine-learning-model.md).

Ha azt szeretné toostart, mint a létező új Azure Resource Manager-alapú webszolgáltatás, lásd: [meglévő prediktív webszolgáltatás újratanítása](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Hozzon létre egy tanítási kísérletet
Ehhez a példához használandó "minta 5: vonat, tesztelési, Evaluate bináris osztályozási: felnőtt Dataset" hello Microsoft Azure Machine Learning minták. 

toocreate hello kísérlet:

1. Jelentkezzen be Azure Machine Learning Studio tooMicrosoft. 
2. A hello jobb alsó sarkában hello irányítópultot, kattintson **új**.
3. Hello Microsoft Samples jelölje ki a minta 5.
4. toorename hello kísérlet hello tetején hello kísérletvászonra, válassza ki a hello kísérlet neve "minta 5: vonat, tesztelési, Evaluate bináris osztályozási: felnőtt Dataset".
5. Típus nyilvántartásba modell.
6. A kísérletvászonra hello hello alján kattintson **futtatása**.
7. Kattintson a **Set Up webszolgáltatás** válassza **webszolgáltatás Átképezési**. 

hello következő hello kezdeti kísérlet jeleníti meg.
   
   ![Kezdeti kísérlet.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>A prediktív kísérletté létrehozása és egy webszolgáltatás közzététele
Ezután hozzon létre egy Predicative kísérletet.

1. A kísérletvászonra hello hello alján kattintson **webes szolgáltatások beállítása** válassza **prediktív webszolgáltatás**. Ez a modell betanítását hello modell menti, és webes szolgáltatás bemeneti és kimeneti modulok hozzáadása. 
2. Kattintson a **Run** (Futtatás) parancsra. 
3. Miután hello kísérlet futása befejeződött, kattintson **webes szolgáltatás telepítése [klasszikus]** vagy **[Új] webes szolgáltatás telepítése**.

> [!NOTE] 
> toodeploy egy új webszolgáltatás-bővítmény, megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich meg hello webes szolgáltatás telepítéséhez. További információ: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md). 

## <a name="deploy-hello-training-experiment-as-a-training-web-service"></a>Hello tanítási kísérletet egy képzési webszolgáltatás telepítése
tooretrain hello betanított modell, ha már telepítette a hello tanítási kísérletet, amelyet létrehozott Retraining webszolgáltatásként. Ez a webszolgáltatás kell egy *webes szolgáltatás kimeneti* modul csatlakoztatott toohello  *[tanítási modell] [ train-model]*  modul, új toobe képes tooproduce betanított modellek.

1. tooreturn toohello tanítási kísérletet, hello kísérletek ikonra hello bal oldali ablaktáblán, majd kattintson a nyilvántartásba modell nevű hello kísérlet.  
2. Hello keresési kísérlet elemek keresési mezőbe írja be a webes szolgáltatás. 
3. Húzza a *webszolgáltatás bemenetét* alakzatot hello modul vászonra kísérletek kifejlesztéséhez, és csatlakozzon a kimeneti toohello *Clean Missing Data* modul.  Ez biztosítja, hogy a megőrzési adatfeldolgozás hello azonos módon, mint az eredeti betanítási adata.
4. Húzza a két *webszolgáltatás kimeneti* alakzatot hello modulok kísérlet vászonra. Csatlakozás hello hello kimenete *tanítási modell* modul tooone és hello kimenete hello *modell kiértékelése* más modul toohello. a webes szolgáltatás kimeneti hello **tanítási modell** biztosítanak számunkra hello új betanított modell. csatolt túl kimeneti hello**modell kiértékelése** adja vissza, hogy a modul kimenetneve, vagyis hello teljesítménymérési eredményeket.
5. Kattintson a **Run** (Futtatás) parancsra. 

Ezután egy webszolgáltatás, amely létrehozza a modell betanítását és modell kiértékelésének eredménye hello tanítási kísérletet kell telepítenie. Ez a következő műveletek készletét függenek e dolgozik egy klasszikus webszolgáltatás-bővítmény vagy egy új webszolgáltatás-bővítmény tooaccomplish.  

**Klasszikus webszolgáltatás**

Hello kísérletvászonra hello alsó részén, kattintson a **webes szolgáltatások beállítása** válassza **webes szolgáltatás telepítése [klasszikus]**. hello webszolgáltatás **irányítópult** jelenik meg, amely az API-kulcs és hello API hello súgólap a kötegelt végrehajtás. Csak a kötegelt végrehajtási módszer hello betanított modellek létrehozásához használható.

**Új webszolgáltatás**

A kísérletvászonra hello hello alján kattintson **webes szolgáltatások beállítása** válassza **[Új] webes szolgáltatás telepítése**. hello webes szolgáltatás Azure Machine Learning webszolgáltatások portál megnyitja toohello telepítés webszolgáltatás oldalát. Adjon meg egy nevet a webszolgáltatáshoz és támogatási csomag kiválasztása, majd kattintson **telepítés**. Csak hello kötegelt végrehajtási módszer használható betanított modellek létrehozása

Mindkét esetben kísérlet tartalmaz befejezett futtatását, miután hello eredményül kapott munkafolyamat kell hasonlítania:

![Eredményül kapott munkafolyamat futtatása után.][4]



## <a name="retrain-hello-model-with-new-data-using-bes"></a>Hello modell újratanítása BES használatával új adatokkal
Ebben a példában a C# toocreate hello átképezési alkalmazás használ. Is használhatja hello Python vagy R minta kód tooaccomplish ezt a feladatot.

toocall hello Átképezési API-kat:

1. Hozzon létre egy C# konzolalkalmazást a Visual Studio: **új** > **projekt** > **Visual C#** > **klasszikus Windows asztal** > **Konzolalkalmazás (.NET-keretrendszer)**.
2. Jelentkezzen be toohello a Machine Learning webszolgáltatás portálon.
3. Ha egy klasszikus webszolgáltatás dolgozik, kattintson a **klasszikus webszolgáltatások**.
   1. Kattintson a hello webszolgáltatás dolgozik.
   2. Kattintson a hello alapértelmezett végpont.
   3. Kattintson a **felhasználásához**.
   4. Hello hello alján **felhasználás** lap hello **mintakód** kattintson **kötegelt**.
   5. Az eljárás 5 toostep továbbra is.
4. Ha egy új webszolgáltatás-bővítmény dolgozik, kattintson a **webszolgáltatások**.
   1. Kattintson a hello webszolgáltatás dolgozik.
   2. Kattintson a **felhasználásához**.
   3. Hello felhasználás lap hello hello alján **mintakód** kattintson **kötegelt**.
5. Hello minta C#-kódban a kötegelt végrehajtás másolása és beillesztése hello Program.cs fájlhoz a meggyőződött arról, hogy hello névtér nem módosulnak.

Adja hozzá a hello Microsoft.AspNet.WebApi.Client Nuget-csomagot a hello megjegyzések. tooadd hello hivatkozás tooMicrosoft.WindowsAzure.Storage.dll, először meg kell tooinstall hello ügyféloldali kódtár a Microsoft Azure tárolási szolgáltatásokhoz. További információkért lásd: [Windows tárolószolgáltatások](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-hello-apikey-declaration"></a>Hello apikey nyilatkozat frissítése
Keresse meg a hello **apikey** nyilatkozatot.

    const string apiKey = "abc123"; // Replace this with hello API key for hello web service

A hello **alapvető fogyasztási adatai** hello szakasza **felhasználás** lapon hello elsődleges kulcs keresse meg és másolja azt toohello **apikey** nyilatkozatot.

### <a name="update-hello-azure-storage-information"></a>Hello Azure Storage-adatainak módosítása
hello BES mintakód feltölt egy fájlt egy helyi meghajtó (például "C:\temp\CensusIpnput.csv") tooAzure tárolási, folyamatokat engedélyez, és írja a hello eredmények hátsó tooAzure tárolási.  

tooaccomplish ebben a feladatban le kell kérni a tárfiók hello klasszikus Azure portálon, és a megfelelő értékek hello kódban hello frissítse hello Tárfiók neve, a kulcs és a tároló adatait. 

1. Jelentkezzen be a klasszikus Azure portálon toohello.
2. Kattintson a bal oldali oszlopban hello **tárolási**.
3. Hello listában tárfiókok jelölje be egy toostore hello retrained modell.
4. Hello a hello lap alján, kattintson **elérési kulcsok kezelése**.
5. Másolja ki és mentse a hello **elsődleges elérési kulcsot** és Bezárás hello párbeszédpanel. 
6. Hello hello oldal tetején, kattintson a **tárolók**.
7. Jelöljön ki egy meglévő tárolót, vagy hozzon létre egy újat, és mentse hello nevét.

Keresse meg a hello *StorageAccountName*, *StorageAccountKey*, és *StorageContainerName* deklarációkat és mentette a hello frissítés hello Azure-portálon.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

Akkor is biztosítania kell hello bemeneti fájl elérhető hello hello kódban megadott helyen. 

### <a name="specify-hello-output-location"></a>Hello kimeneti helyének megadása
A kérelem hasznos hello hello kimeneti hely megadásakor hello megadott hello fájl kiterjesztése *RelativeLocation* ilearner kell megadni. 

Tekintse meg a következő példa hello:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with hello location you would like toouse for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> a kimeneti helyek hello nevének azokat, a forgatókönyv alapuló hello hello webes szolgáltatás kimeneti modulok hozzá hello eltérhet. A tanítási kísérletet két kimenettel állít be, mert hello eredmények tartalmazzák a Tartózkodásihely-adatok tárolási helyezni őket.  
> 
> 

![Kimeneti átképezési][6]

4. ábra: A kimeneti Átképezési.

## <a name="evaluate-hello-retraining-results"></a>Hello Átképezési eredmények értékelése
Hello alkalmazás futtatásakor hello kimeneti hello URL-címet tartalmazza, és a SAS-token szükséges tooaccess hello kiértékelésének eredménye.

Megtekintheti a hello teljesítménymérési eredményeket hello retrained modell hello kombinálásával *BaseLocation*, *RelativeLocation*, és *SasBlobToken* hello kimeneti eredmények a *output2* (ahogy az az előző kimeneti kép átképezési hello szerepel), és be hello teljes URL-CÍMÉT a böngésző címsorában hello.  

Hello eredmények toodetermine akkor megvizsgálja, hogy hello újonnan betanított modell is elegendő egy meglévő tooreplace hello hajt végre.

Másolás hello *BaseLocation*, *RelativeLocation*, és *SasBlobToken* hello kimeneti eredmények, használhatja őket hello átképezési folyamat során.

## <a name="next-steps"></a>Következő lépések
Ha telepítette a hello prediktív webszolgáltatás kattintva **webes szolgáltatás telepítése [klasszikus]**, lásd: [egy klasszikus webszolgáltatás újratanítása](machine-learning-retrain-a-classic-web-service.md).

Ha telepítette a hello prediktív webszolgáltatás kattintva **[Új] webes szolgáltatás telepítése**, lásd: [egy új webszolgáltatás-bővítmény hello Machine Learning Management-parancsmagok használatával újratanítása](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using hello Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
