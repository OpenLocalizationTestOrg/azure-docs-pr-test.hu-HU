---
title: "aaaCreate Hadoop-fürtök Azure REST API - Azure használatával |} Microsoft Docs"
description: Ismerje meg, hogyan toocreate HDInsight clusters szerint Azure Resource Manager sablonok toohello Azure REST API-t.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 98be5893-2c6f-4dfa-95ec-d4d8b5b7dcb5
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/10/2017
ms.author: larryfr
ms.openlocfilehash: 87b585e5084eccdc3d7c57483deabb4ad6e32597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-clusters-using-hello-azure-rest-api"></a>Hello Azure REST API használatával Hadoop-fürtök létrehozása

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Ismerje meg, hogyan toocreate egy HDInsight fürt Azure Resource Manager-sablonnal, és hello Azure REST API-t.

hello Azure REST API lehetővé teszi a hello Azure platformon, beleértve az új erőforrások, például a HDInsight-fürtök létrehozása hello üzemeltetett szolgáltatások tooperform felügyeleti műveleteihez.

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!NOTE]
> Ez a dokumentum használatát hello lépései hello [curl (https://curl.haxx.se/)](https://curl.haxx.se/) segédprogram toocommunicate a hello Azure REST API-t.

## <a name="create-a-template"></a>Sablon létrehozása

Az Azure Resource Manager-sablonok leíró JSON-dokumentumok egy **erőforráscsoport** és minden-erőforrásokat (például a HDInsight.) Ez a sablon-alapú módszer lehetővé teszi egy sablon szükséges a HDInsight toodefine hello erőforrások.

hello következő JSON-dokumentumhoz hello egyesülés sablon és a paraméterek fájlok [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), ami létrehoz egy Linux-alapú a jelszó toosecure hello SSH-felhasználói fiókhoz használó fürt.

   ```json
   {
       "properties": {
           "template": {
               "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
               "contentVersion": "1.0.0.0",
               "parameters": {
                   "clusterType": {
                       "type": "string",
                       "allowedValues": ["hadoop",
                       "hbase",
                       "storm",
                       "spark"],
                       "metadata": {
                           "description": "hello type of hello HDInsight cluster toocreate."
                       }
                   },
                   "clusterName": {
                       "type": "string",
                       "metadata": {
                           "description": "hello name of hello HDInsight cluster toocreate."
                       }
                   },
                   "clusterLoginUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used toosubmit jobs toohello cluster and toolog into cluster dashboards."
                       }
                   },
                   "clusterLoginPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "sshUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used tooremotely access hello cluster."
                       }
                   },
                   "sshPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "clusterStorageAccountName": {
                       "type": "string",
                       "metadata": {
                           "description": "hello name of hello storage account toobe created and be used as hello cluster's storage."
                       }
                   },
                   "clusterWorkerNodeCount": {
                       "type": "int",
                       "defaultValue": 4,
                       "metadata": {
                           "description": "hello number of nodes in hello HDInsight cluster."
                       }
                   }
               },
               "variables": {
                   "defaultApiVersion": "2015-05-01-preview",
                   "clusterApiVersion": "2015-03-01-preview"
               },
               "resources": [{
                   "name": "[parameters('clusterStorageAccountName')]",
                   "type": "Microsoft.Storage/storageAccounts",
                   "location": "[resourceGroup().location]",
                   "apiVersion": "[variables('defaultApiVersion')]",
                   "dependsOn": [],
                   "tags": {

                   },
                   "properties": {
                       "accountType": "Standard_LRS"
                   }
               },
               {
                   "name": "[parameters('clusterName')]",
                   "type": "Microsoft.HDInsight/clusters",
                   "location": "[resourceGroup().location]",
                   "apiVersion": "[variables('clusterApiVersion')]",
                   "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                   "tags": {

                   },
                   "properties": {
                       "clusterVersion": "3.5",
                       "osType": "Linux",
                       "clusterDefinition": {
                           "kind": "[parameters('clusterType')]",
                           "configurations": {
                               "gateway": {
                                   "restAuthCredential.isEnabled": true,
                                   "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                   "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                               }
                           }
                       },
                       "storageProfile": {
                           "storageaccounts": [{
                               "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                               "isDefault": true,
                               "container": "[parameters('clusterName')]",
                               "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                           }]
                       },
                       "computeProfile": {
                           "roles": [{
                               "name": "headnode",
                               "targetInstanceCount": "2",
                               "hardwareProfile": {
                                   "vmSize": "Standard_D3"
                               },
                               "osProfile": {
                                   "linuxOperatingSystemProfile": {
                                       "username": "[parameters('sshUserName')]",
                                       "password": "[parameters('sshPassword')]"
                                   }
                               }
                           },
                           {
                               "name": "workernode",
                               "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                               "hardwareProfile": {
                                   "vmSize": "Standard_D3"
                               },
                               "osProfile": {
                                   "linuxOperatingSystemProfile": {
                                       "username": "[parameters('sshUserName')]",
                                       "password": "[parameters('sshPassword')]"
                                   }
                               }
                           }]
                       }
                   }
               }],
               "outputs": {
                   "cluster": {
                       "type": "object",
                       "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                   }
               }
           },
           "mode": "incremental",
           "Parameters": {
               "clusterName": {
                   "value": "newclustername"
               },
               "clusterType": {
                   "value": "hadoop"
               },
               "clusterStorageAccountName": {
                   "value": "newstoragename"
               },
               "clusterLoginUserName": {
                   "value": "admin"
               },
               "clusterLoginPassword": {
                   "value": "changeme"
               },
               "sshUserName": {
                   "value": "sshuser"
               },
               "sshPassword": {
                   "value": "changeme"
               }
           }
       }
   }
   ```

Ebben a példában a jelen dokumentumban leírt lépések hello használatos. Cserélje le a hello példa *értékek* a hello **paraméterek** szakasz a fürt hello értékekkel.

> [!IMPORTANT]
> hello sablon hello alapértelmezett számú munkavégző csomópontokhoz (4) a HDInsight-fürtök használ. Ha több mint 32 munkavégző csomópontot, majd ki kell választania egy átjárócsomóponttal mérete legalább 8 maggal és 14 GB RAM-mal.
>
> A csomópont-méretek és a társuló költségeket további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="log-in-tooyour-azure-subscription"></a>Jelentkezzen be tooyour Azure-előfizetés

Részletes ismertetését lásd: hello lépésekkel [Ismerkedés az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) , és csatlakozzon a feliratkozás tooyour hello `az login` parancsot.

## <a name="create-a-service-principal"></a>Egyszerű szolgáltatás létrehozása

> [!NOTE]
> Ezeket a lépéseket egy rövidített verzióját hello *egyszerű szolgáltatásnév létrehozása jelszóval* hello szakasza [egyszerű szolgáltatás használata az Azure parancssori felület toocreate tooaccess erőforrások](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) dokumentum. Ezeket a lépéseket, amely használt tooauthenticate toohello Azure REST API szolgáltatásnevet létrehozni.

1. A parancssorból használja a következő parancs toolist hello Azure-előfizetését.

   ```bash
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    Hello listában válassza ki a kívánt toouse, és jegyezze fel a hello hello előfizetés **ELŐFIZETÉS_AZONOSÍTÓJA** és __Tenant_ID__ oszlopok. Ezeket az értékeket menteni.

2. A következő parancs toocreate egy alkalmazás az Azure Active Directoryban hello használata.

   ```bash
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    Cserélje le a hello hello értékeinek `--display-name`, `--homepage`, és `--identifier-uris` saját értékekkel. Adja meg a jelszót hello új Active Directory-kereséshez.

   > [!NOTE]
   > Hello `--home-page` és `--identifier-uris` értékek nem kell tooreference hello üzemeltetett tényleges weblap internet. Egyedi URI legyen.

   hello Ez a parancs által visszaadott érték: hello __Alkalmazásazonosító__ hello új alkalmazásként. Mentse ezt az értéket.

3. Használjon hello következő parancsot a toocreate hello használata egyszerű szolgáltatás **Alkalmazásazonosító**.

   ```bash
   az ad sp create --id <App ID> --query 'objectId'
   ```

     hello Ez a parancs által visszaadott érték: hello __Objektumazonosító__. Mentse ezt az értéket.

4. Rendelje hozzá a hello **tulajdonos** hello használata egyszerű szerepkör toohello **Objektumazonosító** érték. Használjon hello **előfizetés-azonosító** korábban beszerzett.

   ```bash
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a>Egy hitelesítési token beszerzése

A következő parancs tooretrieve egy hitelesítési jogkivonatot hello használata:

```bash
curl -X "POST" "https://login.microsoftonline.com/$TENANTID/oauth2/token" \
-H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=$APPID" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "client_secret=$PASSWORD" \
--data-urlencode "resource=https://management.azure.com/"
```

Állítsa be `$TENANTID`, `$APPID`, és `$PASSWORD` toohello értékek szerezte be, vagy a korábban használt.

Ha a kérelem sikeres, 200 adatsorozat választ kapnak, és hello adott válasz törzsének egy JSON-dokumentum tartalmaz.

hello JSON-dokumentum, a kérelem által visszaadott elemet tartalmaz nevű **access_token**. hello értékének **access_token** használt tooauthentication kérelmek toohello REST API-t.

```json
{
    "token_type":"Bearer",
    "expires_in":"3599",
    "expires_on":"1463409994",
    "not_before":"1463406094",
    "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
}
```

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

A következő toocreate erőforráscsoport hello használata.

* Állítsa be `$SUBSCRIPTIONID` hello egyszerű szolgáltatás létrehozása során kapott toohello előfizetés-azonosító.
* Állítsa be `$ACCESSTOKEN` hello előző lépésben kapott toohello hozzáférési jogkivonat.
* Cserélje le `DATACENTERLOCATION` hello data Center kívánja toocreate hello erőforráscsoport és erőforrások, a. Például "déli középső Régiójában".
* Állítsa be `$RESOURCEGROUPNAME` kívánja az adott csoport toouse toohello nevét:

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME?api-version=2015-01-01" \
    -H "Authorization: Bearer $ACCESSTOKEN" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DATACENTERLOCATION"
}'
```

Ha a kérelem sikeres, 200 adatsorozat választ kapnak, és hello adott válasz törzsének hello-csoport információit tartalmazó JSON-dokumentum tartalmaz. Hello `"provisioningState"` elem értéke `"Succeeded"`.

## <a name="create-a-deployment"></a>Hozzon létre telepítést

A következő parancs toodeploy hello sablon toohello erőforráscsoport hello használata.

* Állítsa be `$DEPLOYMENTNAME` kívánja a telepítés toouse toohello nevét.

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json" \
-d "{set your body string toohello template and parameters}"
```

> [!NOTE]
> Hello sablon tooa fájlt mentette, ha használható a következő parancs helyett hello `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Ha a kérelem sikeres, 200 adatsorozat választ kapnak, és hello adott válasz törzsének hello központi telepítési művelettel kapcsolatos adatokat tartalmazó JSON-dokumentum tartalmaz.

> [!IMPORTANT]
> hello telepítési elküldte, de még nem fejeződött be. Több percig, általában körülbelül 15 hello telepítési toocomplete is igénybe vehet.

## <a name="check-hello-status-of-a-deployment"></a>A központi telepítés hello állapotának ellenőrzése

hello központi telepítés, a következő parancs használata hello toocheck hello állapotát:

```bash
curl -X "GET" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json"
```

Ez a parancs visszaadja a hello központi telepítési művelettel kapcsolatos adatokat tartalmazó JSON-dokumentumból. Hello `"provisioningState"` elem tartalmazza-e hello hello központi telepítésének állapotát tartalmazza. Ha ez az elem értéke `"Succeeded"`, majd hello központi telepítés sikeresen befejeződött.

## <a name="troubleshoot"></a>Hibaelhárítás

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Következő lépések

Most, hogy sikeresen létrehozott egy HDInsight-fürtre, használja a következő toolearn hogyan hello toowork a fürthöz.

### <a name="hadoop-clusters"></a>Hadoop-fürtök

* [A Hive használata a HDInsightban](hdinsight-use-hive.md)
* [A Pig használata a HDInsightban](hdinsight-use-pig.md)
* [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-fürtökkel

* [Az a HDInsight HBase első lépései](hdinsight-hbase-tutorial-get-started-linux.md)
* [A HDInsight HBase Java-alkalmazások fejlesztése](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-fürtök

* [Java-topológiák fejlesztése hdinsight alatt futó Storm](hdinsight-storm-develop-java-topology.md)
* [A HDInsight alatt futó Storm Python-összetevőket használnak](hdinsight-storm-develop-python-topology.md)
* [Telepítheti és figyelheti a HDInsight alatt futó Storm topológiák](hdinsight-storm-deploy-monitor-topology-linux.md)
