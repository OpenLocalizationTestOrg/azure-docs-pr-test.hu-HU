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
# <a name="create-hadoop-clusters-using-hello-azure-rest-api"></a><span data-ttu-id="371c8-103">Hello Azure REST API használatával Hadoop-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="371c8-103">Create Hadoop clusters using hello Azure REST API</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="371c8-104">Ismerje meg, hogyan toocreate egy HDInsight fürt Azure Resource Manager-sablonnal, és hello Azure REST API-t.</span><span class="sxs-lookup"><span data-stu-id="371c8-104">Learn how toocreate an HDInsight cluster using an Azure Resource Manager template and hello Azure REST API.</span></span>

<span data-ttu-id="371c8-105">hello Azure REST API lehetővé teszi a hello Azure platformon, beleértve az új erőforrások, például a HDInsight-fürtök létrehozása hello üzemeltetett szolgáltatások tooperform felügyeleti műveleteihez.</span><span class="sxs-lookup"><span data-stu-id="371c8-105">hello Azure REST API allows you tooperform management operations on services hosted in hello Azure platform, including hello creation of new resources such as HDInsight clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="371c8-106">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="371c8-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="371c8-107">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="371c8-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

> [!NOTE]
> <span data-ttu-id="371c8-108">Ez a dokumentum használatát hello lépései hello [curl (https://curl.haxx.se/)](https://curl.haxx.se/) segédprogram toocommunicate a hello Azure REST API-t.</span><span class="sxs-lookup"><span data-stu-id="371c8-108">hello steps in this document use hello [curl (https://curl.haxx.se/)](https://curl.haxx.se/) utility toocommunicate with hello Azure REST API.</span></span>

## <a name="create-a-template"></a><span data-ttu-id="371c8-109">Sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="371c8-109">Create a template</span></span>

<span data-ttu-id="371c8-110">Az Azure Resource Manager-sablonok leíró JSON-dokumentumok egy **erőforráscsoport** és minden-erőforrásokat (például a HDInsight.) Ez a sablon-alapú módszer lehetővé teszi egy sablon szükséges a HDInsight toodefine hello erőforrások.</span><span class="sxs-lookup"><span data-stu-id="371c8-110">Azure Resource Manager templates are JSON documents that describe a **resource group** and all resources in it (such as HDInsight.) This template-based approach allows you toodefine hello resources that you need for HDInsight in one template.</span></span>

<span data-ttu-id="371c8-111">hello következő JSON-dokumentumhoz hello egyesülés sablon és a paraméterek fájlok [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), ami létrehoz egy Linux-alapú a jelszó toosecure hello SSH-felhasználói fiókhoz használó fürt.</span><span class="sxs-lookup"><span data-stu-id="371c8-111">hello following JSON document is a merger of hello template and parameters files from [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), which creates a Linux-based cluster using a password toosecure hello SSH user account.</span></span>

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

<span data-ttu-id="371c8-112">Ebben a példában a jelen dokumentumban leírt lépések hello használatos.</span><span class="sxs-lookup"><span data-stu-id="371c8-112">This example is used in hello steps in this document.</span></span> <span data-ttu-id="371c8-113">Cserélje le a hello példa *értékek* a hello **paraméterek** szakasz a fürt hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="371c8-113">Replace hello example *values* in hello **Parameters** section with hello values for your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="371c8-114">hello sablon hello alapértelmezett számú munkavégző csomópontokhoz (4) a HDInsight-fürtök használ.</span><span class="sxs-lookup"><span data-stu-id="371c8-114">hello template uses hello default number of worker nodes (4) for an HDInsight cluster.</span></span> <span data-ttu-id="371c8-115">Ha több mint 32 munkavégző csomópontot, majd ki kell választania egy átjárócsomóponttal mérete legalább 8 maggal és 14 GB RAM-mal.</span><span class="sxs-lookup"><span data-stu-id="371c8-115">If you plan on more than 32 worker nodes, then you must select a head node size with at least 8 cores and 14 GB ram.</span></span>
>
> <span data-ttu-id="371c8-116">A csomópont-méretek és a társuló költségeket további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="371c8-116">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

## <a name="log-in-tooyour-azure-subscription"></a><span data-ttu-id="371c8-117">Jelentkezzen be tooyour Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="371c8-117">Log in tooyour Azure subscription</span></span>

<span data-ttu-id="371c8-118">Részletes ismertetését lásd: hello lépésekkel [Ismerkedés az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) , és csatlakozzon a feliratkozás tooyour hello `az login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="371c8-118">Follow hello steps documented in [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) and connect tooyour subscription using hello `az login` command.</span></span>

## <a name="create-a-service-principal"></a><span data-ttu-id="371c8-119">Egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="371c8-119">Create a service principal</span></span>

> [!NOTE]
> <span data-ttu-id="371c8-120">Ezeket a lépéseket egy rövidített verzióját hello *egyszerű szolgáltatásnév létrehozása jelszóval* hello szakasza [egyszerű szolgáltatás használata az Azure parancssori felület toocreate tooaccess erőforrások](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="371c8-120">These steps are an abridged version of hello *Create service principal with password* section of hello [Use Azure CLI toocreate a service principal tooaccess resources](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) document.</span></span> <span data-ttu-id="371c8-121">Ezeket a lépéseket, amely használt tooauthenticate toohello Azure REST API szolgáltatásnevet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="371c8-121">These steps create a service principal that is used tooauthenticate toohello Azure REST API.</span></span>

1. <span data-ttu-id="371c8-122">A parancssorból használja a következő parancs toolist hello Azure-előfizetését.</span><span class="sxs-lookup"><span data-stu-id="371c8-122">From a command line, use hello following command toolist your Azure subscriptions.</span></span>

   ```bash
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    <span data-ttu-id="371c8-123">Hello listában válassza ki a kívánt toouse, és jegyezze fel a hello hello előfizetés **ELŐFIZETÉS_AZONOSÍTÓJA** és __Tenant_ID__ oszlopok.</span><span class="sxs-lookup"><span data-stu-id="371c8-123">In hello list, select hello subscription that you want toouse and note hello **Subscription_ID** and __Tenant_ID__ columns.</span></span> <span data-ttu-id="371c8-124">Ezeket az értékeket menteni.</span><span class="sxs-lookup"><span data-stu-id="371c8-124">Save these values.</span></span>

2. <span data-ttu-id="371c8-125">A következő parancs toocreate egy alkalmazás az Azure Active Directoryban hello használata.</span><span class="sxs-lookup"><span data-stu-id="371c8-125">Use hello following command toocreate an application in Azure Active Directory.</span></span>

   ```bash
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    <span data-ttu-id="371c8-126">Cserélje le a hello hello értékeinek `--display-name`, `--homepage`, és `--identifier-uris` saját értékekkel.</span><span class="sxs-lookup"><span data-stu-id="371c8-126">Replace hello values for hello `--display-name`, `--homepage`, and `--identifier-uris` with your own values.</span></span> <span data-ttu-id="371c8-127">Adja meg a jelszót hello új Active Directory-kereséshez.</span><span class="sxs-lookup"><span data-stu-id="371c8-127">Provide a password for hello new Active Directory entry.</span></span>

   > [!NOTE]
   > <span data-ttu-id="371c8-128">Hello `--home-page` és `--identifier-uris` értékek nem kell tooreference hello üzemeltetett tényleges weblap internet.</span><span class="sxs-lookup"><span data-stu-id="371c8-128">hello `--home-page` and `--identifier-uris` values don't need tooreference an actual web page hosted on hello internet.</span></span> <span data-ttu-id="371c8-129">Egyedi URI legyen.</span><span class="sxs-lookup"><span data-stu-id="371c8-129">They must be unique URIs.</span></span>

   <span data-ttu-id="371c8-130">hello Ez a parancs által visszaadott érték: hello __Alkalmazásazonosító__ hello új alkalmazásként.</span><span class="sxs-lookup"><span data-stu-id="371c8-130">hello value returned from this command is hello __App ID__ for hello new application.</span></span> <span data-ttu-id="371c8-131">Mentse ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="371c8-131">Save this value.</span></span>

3. <span data-ttu-id="371c8-132">Használjon hello következő parancsot a toocreate hello használata egyszerű szolgáltatás **Alkalmazásazonosító**.</span><span class="sxs-lookup"><span data-stu-id="371c8-132">Use hello following command toocreate a service principal using hello **App ID**.</span></span>

   ```bash
   az ad sp create --id <App ID> --query 'objectId'
   ```

     <span data-ttu-id="371c8-133">hello Ez a parancs által visszaadott érték: hello __Objektumazonosító__.</span><span class="sxs-lookup"><span data-stu-id="371c8-133">hello value returned from this command is hello __Object ID__.</span></span> <span data-ttu-id="371c8-134">Mentse ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="371c8-134">Save this value.</span></span>

4. <span data-ttu-id="371c8-135">Rendelje hozzá a hello **tulajdonos** hello használata egyszerű szerepkör toohello **Objektumazonosító** érték.</span><span class="sxs-lookup"><span data-stu-id="371c8-135">Assign hello **Owner** role toohello service principal using hello **Object ID** value.</span></span> <span data-ttu-id="371c8-136">Használjon hello **előfizetés-azonosító** korábban beszerzett.</span><span class="sxs-lookup"><span data-stu-id="371c8-136">Use hello **subscription ID** you obtained earlier.</span></span>

   ```bash
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a><span data-ttu-id="371c8-137">Egy hitelesítési token beszerzése</span><span class="sxs-lookup"><span data-stu-id="371c8-137">Get an authentication token</span></span>

<span data-ttu-id="371c8-138">A következő parancs tooretrieve egy hitelesítési jogkivonatot hello használata:</span><span class="sxs-lookup"><span data-stu-id="371c8-138">Use hello following command tooretrieve an authentication token:</span></span>

```bash
curl -X "POST" "https://login.microsoftonline.com/$TENANTID/oauth2/token" \
-H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=$APPID" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "client_secret=$PASSWORD" \
--data-urlencode "resource=https://management.azure.com/"
```

<span data-ttu-id="371c8-139">Állítsa be `$TENANTID`, `$APPID`, és `$PASSWORD` toohello értékek szerezte be, vagy a korábban használt.</span><span class="sxs-lookup"><span data-stu-id="371c8-139">Set `$TENANTID`, `$APPID`, and `$PASSWORD` toohello values obtained or used previously.</span></span>

<span data-ttu-id="371c8-140">Ha a kérelem sikeres, 200 adatsorozat választ kapnak, és hello adott válasz törzsének egy JSON-dokumentum tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="371c8-140">If this request is successful, you receive a 200 series response and hello response body contains a JSON document.</span></span>

<span data-ttu-id="371c8-141">hello JSON-dokumentum, a kérelem által visszaadott elemet tartalmaz nevű **access_token**.</span><span class="sxs-lookup"><span data-stu-id="371c8-141">hello JSON document returned by this request contains an element named **access_token**.</span></span> <span data-ttu-id="371c8-142">hello értékének **access_token** használt tooauthentication kérelmek toohello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="371c8-142">hello value of **access_token** is used tooauthentication requests toohello REST API.</span></span>

```json
{
    "token_type":"Bearer",
    "expires_in":"3599",
    "expires_on":"1463409994",
    "not_before":"1463406094",
    "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
}
```

## <a name="create-a-resource-group"></a><span data-ttu-id="371c8-143">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="371c8-143">Create a resource group</span></span>

<span data-ttu-id="371c8-144">A következő toocreate erőforráscsoport hello használata.</span><span class="sxs-lookup"><span data-stu-id="371c8-144">Use hello following toocreate a resource group.</span></span>

* <span data-ttu-id="371c8-145">Állítsa be `$SUBSCRIPTIONID` hello egyszerű szolgáltatás létrehozása során kapott toohello előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="371c8-145">Set `$SUBSCRIPTIONID` toohello subscription ID received while creating hello service principal.</span></span>
* <span data-ttu-id="371c8-146">Állítsa be `$ACCESSTOKEN` hello előző lépésben kapott toohello hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="371c8-146">Set `$ACCESSTOKEN` toohello access token received in hello previous step.</span></span>
* <span data-ttu-id="371c8-147">Cserélje le `DATACENTERLOCATION` hello data Center kívánja toocreate hello erőforráscsoport és erőforrások, a.</span><span class="sxs-lookup"><span data-stu-id="371c8-147">Replace `DATACENTERLOCATION` with hello data center you wish toocreate hello resource group, and resources, in.</span></span> <span data-ttu-id="371c8-148">Például "déli középső Régiójában".</span><span class="sxs-lookup"><span data-stu-id="371c8-148">For example, 'South Central US'.</span></span>
* <span data-ttu-id="371c8-149">Állítsa be `$RESOURCEGROUPNAME` kívánja az adott csoport toouse toohello nevét:</span><span class="sxs-lookup"><span data-stu-id="371c8-149">Set `$RESOURCEGROUPNAME` toohello name you wish toouse for this group:</span></span>

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME?api-version=2015-01-01" \
    -H "Authorization: Bearer $ACCESSTOKEN" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DATACENTERLOCATION"
}'
```

<span data-ttu-id="371c8-150">Ha a kérelem sikeres, 200 adatsorozat választ kapnak, és hello adott válasz törzsének hello-csoport információit tartalmazó JSON-dokumentum tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="371c8-150">If this request is successful, you receive a 200 series response and hello response body contains a JSON document containing information about hello group.</span></span> <span data-ttu-id="371c8-151">Hello `"provisioningState"` elem értéke `"Succeeded"`.</span><span class="sxs-lookup"><span data-stu-id="371c8-151">hello `"provisioningState"` element contains a value of `"Succeeded"`.</span></span>

## <a name="create-a-deployment"></a><span data-ttu-id="371c8-152">Hozzon létre telepítést</span><span class="sxs-lookup"><span data-stu-id="371c8-152">Create a deployment</span></span>

<span data-ttu-id="371c8-153">A következő parancs toodeploy hello sablon toohello erőforráscsoport hello használata.</span><span class="sxs-lookup"><span data-stu-id="371c8-153">Use hello following command toodeploy hello template toohello resource group.</span></span>

* <span data-ttu-id="371c8-154">Állítsa be `$DEPLOYMENTNAME` kívánja a telepítés toouse toohello nevét.</span><span class="sxs-lookup"><span data-stu-id="371c8-154">Set `$DEPLOYMENTNAME` toohello name you wish toouse for this deployment.</span></span>

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json" \
-d "{set your body string toohello template and parameters}"
```

> [!NOTE]
> <span data-ttu-id="371c8-155">Hello sablon tooa fájlt mentette, ha használható a következő parancs helyett hello `-d "{ template and parameters}"`:</span><span class="sxs-lookup"><span data-stu-id="371c8-155">If you saved hello template tooa file, you can use hello following command instead of `-d "{ template and parameters}"`:</span></span>
>
> `--data-binary "@/path/to/file.json"`

<span data-ttu-id="371c8-156">Ha a kérelem sikeres, 200 adatsorozat választ kapnak, és hello adott válasz törzsének hello központi telepítési művelettel kapcsolatos adatokat tartalmazó JSON-dokumentum tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="371c8-156">If this request is successful, you receive a 200 series response and hello response body contains a JSON document containing information about hello deployment operation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="371c8-157">hello telepítési elküldte, de még nem fejeződött be.</span><span class="sxs-lookup"><span data-stu-id="371c8-157">hello deployment has been submitted, but has not completed.</span></span> <span data-ttu-id="371c8-158">Több percig, általában körülbelül 15 hello telepítési toocomplete is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="371c8-158">It can take several minutes, usually around 15, for hello deployment toocomplete.</span></span>

## <a name="check-hello-status-of-a-deployment"></a><span data-ttu-id="371c8-159">A központi telepítés hello állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="371c8-159">Check hello status of a deployment</span></span>

<span data-ttu-id="371c8-160">hello központi telepítés, a következő parancs használata hello toocheck hello állapotát:</span><span class="sxs-lookup"><span data-stu-id="371c8-160">toocheck hello status of hello deployment, use hello following command:</span></span>

```bash
curl -X "GET" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json"
```

<span data-ttu-id="371c8-161">Ez a parancs visszaadja a hello központi telepítési művelettel kapcsolatos adatokat tartalmazó JSON-dokumentumból.</span><span class="sxs-lookup"><span data-stu-id="371c8-161">This command returns a JSON document containing information about hello deployment operation.</span></span> <span data-ttu-id="371c8-162">Hello `"provisioningState"` elem tartalmazza-e hello hello központi telepítésének állapotát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="371c8-162">hello `"provisioningState"` element contains hello status of hello deployment.</span></span> <span data-ttu-id="371c8-163">Ha ez az elem értéke `"Succeeded"`, majd hello központi telepítés sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="371c8-163">If this element contains a value of `"Succeeded"`, then hello deployment has completed successfully.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="371c8-164">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="371c8-164">Troubleshoot</span></span>

<span data-ttu-id="371c8-165">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="371c8-165">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="371c8-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="371c8-166">Next steps</span></span>

<span data-ttu-id="371c8-167">Most, hogy sikeresen létrehozott egy HDInsight-fürtre, használja a következő toolearn hogyan hello toowork a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="371c8-167">Now that you have successfully created an HDInsight cluster, use hello following toolearn how toowork with your cluster.</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="371c8-168">Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="371c8-168">Hadoop clusters</span></span>

* [<span data-ttu-id="371c8-169">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="371c8-169">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="371c8-170">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="371c8-170">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="371c8-171">Use MapReduce with HDInsight</span><span class="sxs-lookup"><span data-stu-id="371c8-171">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="371c8-172">HBase-fürtökkel</span><span class="sxs-lookup"><span data-stu-id="371c8-172">HBase clusters</span></span>

* [<span data-ttu-id="371c8-173">Az a HDInsight HBase első lépései</span><span class="sxs-lookup"><span data-stu-id="371c8-173">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="371c8-174">A HDInsight HBase Java-alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="371c8-174">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="371c8-175">Storm-fürtök</span><span class="sxs-lookup"><span data-stu-id="371c8-175">Storm clusters</span></span>

* [<span data-ttu-id="371c8-176">Java-topológiák fejlesztése hdinsight alatt futó Storm</span><span class="sxs-lookup"><span data-stu-id="371c8-176">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="371c8-177">A HDInsight alatt futó Storm Python-összetevőket használnak</span><span class="sxs-lookup"><span data-stu-id="371c8-177">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="371c8-178">Telepítheti és figyelheti a HDInsight alatt futó Storm topológiák</span><span class="sxs-lookup"><span data-stu-id="371c8-178">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)
