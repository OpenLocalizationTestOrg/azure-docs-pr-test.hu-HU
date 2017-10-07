---
title: "Hogyan toomanage AzureML web services API Management használata aaaLearn |} Microsoft Docs"
description: "Egy útmutató, megjelenítő, hogy miként toomanage AzureML webes szolgáltatásokat, az API Management használata."
keywords: "gépi tanulási, az api management"
services: machine-learning
documentationcenter: 
author: roalexan
manager: jhubbard
editor: 
ms.assetid: 05150ae1-5b6a-4d25-ac67-fb2f24a68e8d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2017
ms.author: roalexan
ms.openlocfilehash: 6e764fbfd71be6cc908a1c8d3d8889969fc651a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="learn-how-toomanage-azureml-web-services-using-api-management"></a>Ismerje meg, hogy miként toomanage AzureML webes szolgáltatásokat, az API Management használata
## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan tooquickly az API Management toomanage az AzureML webszolgáltatások első lépéseiben.

## <a name="what-is-azure-api-management"></a>Mi az Azure API Management?
Az Azure API-kezelés nem az Azure-szolgáltatások, amely lehetővé teszi a REST API-végpontokat kezelheti a felhasználói hozzáférés-szabályozás és figyelési irányítópult meghatározásával. Kattintson a [Itt](https://azure.microsoft.com/services/api-management/) Azure API Management leírását. Kattintson a [Itt](../api-management/api-management-get-started.md) az útmutató, hogyan tooget el az Azure API Management szolgáltatással. Ez más útmutató, ez az útmutató alapján, minden olyan további témaköreit, beleértve az értesítési konfigurációk, a árképzési szint, a válasz kezelése, a felhasználói hitelesítést, termékek, a fejlesztői előfizetésekhez és a használati dashboarding létrehozása.

## <a name="what-is-azureml"></a>Mi az az AzureML?
Az AzureML az Azure-szolgáltatások a machine Learning szolgáltatáshoz, amely lehetővé teszi tooeasily build, telepítheti, és speciális elemzési megoldások megosztása. Kattintson a [Itt](https://azure.microsoft.com/services/machine-learning/) AzureML leírását.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az útmutatóban szüksége:

* Egy Azure-fiók. Ha nem rendelkezik Azure-fiókot, kattintson a [Itt](https://azure.microsoft.com/pricing/free-trial/) kapcsolatos részletes tudnivalókért toocreate egy ingyenes próbafiókot.
* Az AzureML-fiók. Ha nincs AzureML fiókja, kattintson a [Itt](https://studio.azureml.net/) kapcsolatos részletes tudnivalókért toocreate egy ingyenes próbafiókot.
* hello munkaterületen, a szolgáltatás és a api_key egy webes szolgáltatásként telepített AzureML kísérleti fázisú funkciókat. Kattintson a [Itt](machine-learning-create-experiment.md) talál részletes információt hogyan toocreate az AzureML kipróbálásához. Kattintson a [Itt](machine-learning-publish-a-machine-learning-web-service.md) talál részletes információt hogyan toodeploy az AzureML webszolgáltatásként kipróbálásához. Alternatív megoldásként a függelék rendelkezik hogyan toocreate és tesztelése egy egyszerű AzureML kísérletezhet, és telepítse azt egy webszolgáltatás vonatkozó utasításokat.

## <a name="create-an-api-management-instance"></a>API Management-példány létrehozása
Az alábbiakban hello API Management toomanage használatának lépései a AzureML webes szolgáltatás. Először létre kell hoznia egy szolgáltatáspéldány. Jelentkezzen be toohello [klasszikus portál](https://manage.windowsazure.com/) kattintson **új** > **alkalmazásszolgáltatások** > **API Management**  >  **Létrehozása**.

![példány létrehozása](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Adjon meg egy egyedi **URL-cím**. Ez az útmutató használ **demoazureml** – valami más kell toochoose. Válassza ki a kívánt hello **előfizetés** és **régió** a szolgáltatáspéldány. Miután kiválasztotta a kívánt beállításokat, kattintson a hello Tovább gombra.

![Hozzon létre-szolgáltatás-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Adjon meg egy értéket hello **szervezetnév**. Ez az útmutató használ **demoazureml** – valami más kell toochoose. Adja meg az e-mail cím hello **rendszergazdai e-mail** mező. Ez az e-mail cím az API Management rendszer hello értesítésekhez használatos.

![Hozzon létre-szolgáltatás-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Kattintson a szolgáltatáspéldány a hello jelölőnégyzetet toocreate. *Egy új szolgáltatás toobe létrehozott toothirty percig foglal*.

## <a name="create-hello-api"></a>Hello API létrehozása
Hello szolgáltatáspéldány létrehozása után a következő lépésben hello toocreate hello API. Az API egy ügyfélalkalmazásokból meghívható műveletkészletből áll. API-műveleteket a proxy tooexisting webes szolgáltatások. Ez az útmutató a meglévő AzureML RR-EKET és BES webszolgáltatások proxy toohello API-k hoz létre.

API-k létrehozása és konfigurált portálról hello API publisher, amely hello klasszikus Azure portálon keresztül érhető el. tooreach hello publisher portál, válassza ki a szolgáltatáspéldányt.

![szolgáltatáspéldány kiválasztása](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Kattintson a **kezelése** a klasszikus Azure portál hello az API Management szolgáltatás.

![szolgáltatás kezelése](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

Kattintson a **API-k** a hello **API Management** hello maradt, és válassza a menü **hozzáadása API**.

![API-felügyeleti-menü](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Típus **AzureML bemutató API** , hello **webes API-név**. Típus **https://ussouthcentral.services.azureml.net** , hello **webszolgáltatás URL-címe**. Típus **azureml-bemutató** , hello **webes API URL-címe utótag**. Ellenőrizze **HTTPS** , hello **webes API URL-címe** séma. Válassza ki **alapszintű** , **termékek**. Ha befejezte, kattintson a **mentése** toocreate hello API.

![Adja hozzá-új-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

## <a name="add-hello-operations"></a>Hello műveletek hozzáadása
Kattintson a **hozzáadási művelet** tooadd műveletek toothis API.

![Adja hozzá művelet](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

Hello **új művelet** ablak jelenik meg, és hello **aláírás** alapértelmezett kiválasztása lapon.

## <a name="add-rrs-operation"></a>RR-EKET művelet hozzáadása
Először létre kell hoznia a hello szolgáltatást az AzureML RR-EKET művelete. Válassza ki **POST** , hello **HTTP-műveletet**. Típus **/workspaces/ {munkaterület} /services/ {szolgáltatás} / execute? api-version = {apiversion} & részletei = {részletek}** hello, **URL-cím sablon**. Típus **RRS hajtható végre** , hello **megjelenített név**.

![Adja hozzá-rrs-művelet-aláírása](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Kattintson a **válaszok** > **hozzáadása** hello bal, és válassza a **200 OK**. Kattintson a **mentése** toosave ezt a műveletet.

![Adja hozzá-rrs-művelet-válasz](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

## <a name="add-bes-operations"></a>Adja hozzá a BES műveletek
Képernyőfelvételek nincsenek hello BES műveletek, mivel ezek nagyon hasonló toothose hello RR-EKET művelet hozzáadásához.

### <a name="submit-but-not-start-a-batch-execution-job"></a>A kötegelt végrehajtási feladatok elküldése (de indul el)
Kattintson a **hozzáadási művelet** tooadd hello AzureML BES művelet toohello API. Válassza ki **POST** a hello **HTTP-műveletet**. Típus **/workspaces/ {munkaterület} /services/ {szolgáltatás} / feladatok? api-version = {apiversion}** a hello **URL-cím sablon**. Típus **BES nyújt** a hello **megjelenített név**. Kattintson a **válaszok** > **hozzáadása** hello bal, és válassza a **200 OK**. Kattintson a **mentése** toosave ezt a műveletet.

### <a name="start-a-batch-execution-job"></a>Kötegelt végrehajtási feladat indítása
Kattintson a **hozzáadási művelet** tooadd hello AzureML BES művelet toohello API. Válassza ki **POST** a hello **HTTP-műveletet**. Típus **/workspaces/ {munkaterület} {szolgáltatás} /services/ /jobs/ {jobid} / start? api-version = {apiversion}** a hello **URL-cím sablon**. Típus **BES Start** a hello **megjelenített név**. Kattintson a **válaszok** > **hozzáadása** hello bal, és válassza a **200 OK**. Kattintson a **mentése** toosave ezt a műveletet.

### <a name="get-hello-status-or-result-of-a-batch-execution-job"></a>Szerezze be a hello állapot vagy a kötegelt végrehajtási feladat eredménye
Kattintson a **hozzáadási művelet** tooadd hello AzureML BES művelet toohello API. Válassza ki **beolvasása** a hello **HTTP-műveletet**. Típus **/workspaces/ {munkaterület} {szolgáltatás} /services/ /jobs/ {jobid}? api-version = {apiversion}** a hello **URL-cím sablon**. Típus **BES állapot** a hello **megjelenített név**. Kattintson a **válaszok** > **hozzáadása** hello bal, és válassza a **200 OK**. Kattintson a **mentése** toosave ezt a műveletet.

### <a name="delete-a-batch-execution-job"></a>A kötegelt végrehajtási feladatok törlése
Kattintson a **hozzáadási művelet** tooadd hello AzureML BES művelet toohello API. Válassza ki **törlése** a hello **HTTP-műveletet**. Típus **/workspaces/ {munkaterület} {szolgáltatás} /services/ /jobs/ {jobid}? api-version = {apiversion}** a hello **URL-cím sablon**. Típus **BES törlése** a hello **megjelenített név**. Kattintson a **válaszok** > **hozzáadása** hello bal, és válassza a **200 OK**. Kattintson a **mentése** toosave ezt a műveletet.

## <a name="call-an-operation-from-hello-developer-portal"></a>Egy művelet hívja hello fejlesztői portálján
Műveletek hívható közvetlenül a hello fejlesztői portálján, amely egy kényelmes módszert arra tooview biztosít, és tesztelje az API-k hello működésére. Ez az útmutató lépésben fel fogja hívni hello **RRS hajtható végre** toohello hozzáadott metódus **AzureML bemutató API**. Kattintson a **fejlesztői portálján** hello hello menüből a hello klasszikus portál jobb felső.

![fejlesztői-portál](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

Kattintson a **API-k** hello felső menüben, majd kattintson a **AzureML bemutató API** toosee hello műveletek érhető el.

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Válassza ki **RRS hajtható végre** hello a művelethez. Kattintson a **kipróbálás**.

![Próbálja meg-it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

A kérelemben szereplő paraméterek, írja be a **munkaterület**, **szolgáltatás**, **2.0** a hello **apiversion**, és **true**a hello **részletek**. Megtalálhatja a **munkaterület** és **szolgáltatás** hello AzureML webes szolgáltatás irányítópultján (lásd: **hello webszolgáltatás tesztelése** függelék).

Kérelemfejléc, kattintson **Hozzáadás fejléc** és írja be **Content-Type** és **az application/json**, majd kattintson a **Hozzáadás fejléc** és írja be **engedélyezési** és **tulajdonosi <YOUR AZUREML SERVICE API-KEY>** . Megtalálhatja a **api-kulcs** hello AzureML webes szolgáltatás irányítópultján (lásd: **hello webszolgáltatás tesztelése** függelék).

Típus **{"Bemenetek": {"input1": {"ColumnNames": "Értékek" ["Col2"]: [["Ez az a jó naponta"]]}}, "GlobalParameters": {}}** hello kérelemtörzset a.

![az azureml-bemutató-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Kattintson a **küldése**.

![Küldés](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

Után egy művelet indítása, hello fejlesztői portál megjeleníti-e a hello **a kért URL-cím** hello háttér-szolgáltatásból hello **válaszállapot**, hello **válaszfejlécek**, és bármely **válasz tartalom**.

![válasz-állapot](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

## <a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>A függelék – létrehozása és tesztelése egy egyszerű AzureML webszolgáltatás
### <a name="creating-hello-experiment"></a>Hello kísérlet létrehozása
Az alábbiakban egy egyszerű AzureML kísérlet létrehozását és telepítését webszolgáltatásként hello lépéseit. webes szolgáltatás vesz hello bemenetként a tetszőleges szöveget, és adja vissza egész számok-ként funkciókat. Példa:

| Szöveg | Kivonatolt szöveg |
| --- | --- |
| Ez az jó naponta |1 1 2 2 0 2 0 1 |

Először egy böngészőt, nyissa meg azt: [https://studio.azureml.net/](https://studio.azureml.net/) , és írja be a hitelesítő adatok toolog a. Ezután hozzon létre egy új üres kísérlet.

![Keresés – kísérlet-sablonok](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Nevezze át túl**SimpleFeatureHashingExperiment**. Bontsa ki a **mentett adatkészletek** , és húzza **könyv értékelést az Amazon** alakzatot a kísérlet során.

![egyszerű-szolgáltatás-kivonatolás-kísérlet](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Bontsa ki a **Data Transformation** és **adatkezelési** , és húzza **Select Columns in Dataset** alakzatot a kísérlet során. Csatlakozás **könyv értékelést az Amazon** túl**Select Columns in Dataset**.

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

Kattintson a **Select Columns in Dataset** majd **Oszlopválasztás** válassza **Col2**. Kattintson a pipa tooapply hello ezeket a módosításokat.

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Bontsa ki a **Szövegelemzések** , és húzza **Szolgáltatáskivonatolás** alakzatot hello kísérlet. Csatlakozás **Select Columns in Dataset** túl**Szolgáltatáskivonatolás**.

![Connect--projektoszlopok](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Típus **3** a hello **bitsize kivonatoláshoz**. Ezzel létrehoz 8 (23) oszlopot.

![kivonatoló bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

Ezen a ponton érdemes lehet tooclick **futtatása** tootest hello kísérlet.

![Futtatás](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

### <a name="create-a-web-service"></a>Webszolgáltatás létrehozása
Most hozzon létre egy webszolgáltatás-bővítmény. Bontsa ki a **webszolgáltatás** , és húzza **bemeneti** alakzatot a kísérlet során. Csatlakozás **bemeneti** túl**Szolgáltatáskivonatolás**. Húzással is **kimeneti** alakzatot a kísérlet során. Csatlakozás **kimeneti** túl**Szolgáltatáskivonatolás**.

![kimeneti-az-szolgáltatás-kivonatolás](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Kattintson a **webszolgáltatás**.

![közzététel-webszolgáltatás](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Kattintson a **Igen** toopublish hello kísérlet.

![Igen – közzététele](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

### <a name="test-hello-web-service"></a>Teszt hello webszolgáltatás
Az AzureML webszolgáltatás RSS (kérelem/válasz szolgáltatás) és a BES (kötegelt végrehajtási szolgáltatás) végpontok áll. RSS szinkron végrehajtásra van. BES aszinkron feladat végrehajtása szolgál. a webes szolgáltatás hello minta Python forrás alábbi tootest, esetleg toodownload és az Azure SDK telepítése hello Python (lásd: [hogyan tooinstall Python](../python-how-to-install.md)).

Konfigurálnia kell hello **munkaterület**, **szolgáltatás**, és **api_key** hello minta forrás alábbi kísérletbe. Található hello munkaterületet, és a szolgáltatás lehet **kérelem/válasz** vagy **kötegelt végrehajtási** hello webes szolgáltatás irányítópultján a kísérleti fázisú funkciókat.

![Keresés – munkaterület-és-szolgáltatás](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

Hello található **api_key** hello webes szolgáltatás irányítópultján kísérletbe kattintva.

![Keresés – api-kulcs](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

#### <a name="test-rrs-endpoint"></a>Teszt RR-EKET végpont
##### <a name="test-button"></a>Teszt gomb
Egy egyszerű módot tootest hello RRS-végpont esetében tooclick **teszt** hello webes szolgáltatás irányítópultján.

![Teszt](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

Típus **Ez az jó naponta** a **col2**. Kattintson a pipa hello.

![Adja meg adatok](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Látni fogja hasonlót

![minta-kimenet](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

##### <a name="sample-code"></a>Mintakód
Egy másik módja tootest a RR-EKET az Ügyfélkód származik. Ha **kérelem/válasz** hello irányítópult és görgetési toohello alsó, jelennek meg példakód C#, Python, és R. Látni fogja emellett hello RR-EKET kérelem, beleértve a hello kérelem URI, fejlécek és fő hello szintaxisát.

Ez az útmutató bemutatja egy működő Python példa. Szüksége lesz a toomodify azt hello **munkaterület**, **szolgáltatás**, és **api_key** a kísérlet.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("hello request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

#### <a name="test-bes-endpoint"></a>Teszt BES végpont
Kattintson a **kötegelt végrehajtás** hello irányítópult és görgetési toohello alján. Mintakód látni fogja a C#, Python, és R. Látni fogja emellett hello szintaxisát hello BES kérelmek toosubmit egy feladatot, elindíthat egy feladatot, hello állapot vagy a feladat eredményeinek lekérése és törli a feladatot.

Ez az útmutató bemutatja egy működő Python példa. Toomodify van szüksége a hello **munkaterület**, **szolgáltatás**, és **api_key** a kísérlet. Továbbá szüksége toomodify hello **tárfióknév**, **tárfiók kulcsa**, és **storage-tároló nevének**. Végül, szüksége lesz toomodify hello helyét hello **bemeneti fájl** és hello hello helyének **kimeneti fájl**.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH hello API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH hello LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH hello LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("hello request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading hello result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written toohello file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("hello results for " + outputName + " are available at hello following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "hello results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading hello input tooblob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded hello input tooblob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting hello job...")
    # submit hello job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove hello enclosing double-quotes
    print("Job ID: " + job_id)
    # start hello job
    print("Starting hello job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking hello job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in hello follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
