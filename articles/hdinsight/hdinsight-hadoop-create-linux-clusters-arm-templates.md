---
title: aaaCreate Hadoop-sablonokkal - Azure HDInsight clusters |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate fürtöket a HDInsight Resource Manager-sablonok segítségével"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00a80dea-011f-44f0-92a4-25d09db9d996
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: jgao
ms.openlocfilehash: 92a6c1d888e401a11537dba34f188245ac17f448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a>Hadoop-fürtök létrehozása a Hdinsightban Resource Manager-sablonok használatával
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Ebből a cikkből megismerheti többféleképpen toocreate Azure HDInsight-fürtök az Azure Resource Manager-sablonok. További információkért lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](../azure-resource-manager/resource-group-template-deploy.md). toolearn más fürt létrehozása eszközöket és szolgáltatásokat, kattintson a hello lapon választó hello legfelső lap vagy a további részletekért lásd a [Fürtlétrehozási módszerekhez](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).

## <a name="prerequisites"></a>Előfeltételek
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Ez a cikk toofollow hello utasításait, szüksége lesz:

* Egy [Azure-előfizetés](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Az Azure PowerShell és/vagy Azure CLI-t.

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="resource-manager-templates"></a>Resource Manager-sablonok
A Resource Manager-sablon lehetővé teszi az alkalmazás egyetlen, koordinált műveletben a következő egyszerű toocreate hello:
* A HDInsight-fürtök és a tőle függő erőforrások (például hello alapértelmezett tárfiók)
* További erőforrások (például az Azure SQL Database toouse Apache Sqoop)

Hello sablonban hello alkalmazáshoz szükséges erőforrások hello határozza meg. Telepítési paraméterek tooinput értékeket különböző környezetekben is megadni. hello sablon JSON és, hogy a központi telepítés tooconstruct értéket használ kifejezések áll.

HDInsight sablon minták található [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/?term=hdinsight). Használja a platformok közötti [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) a hello [erőforrás-kezelő bővítmény](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) vagy szöveges szerkesztő toosave hello sablont egy fájlba a munkaállomáson. Megtudhatja, hogyan toocall hello más módszerekkel sablont.

Erőforrás-kezelő sablonokkal kapcsolatos további információkért tekintse meg a következő cikkek hello:

* [Szerző Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-authoring-templates.md)
* [Alkalmazás üzembe helyezése az Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a>Sablonok készítése

Hello Azure-portál használatával konfigurálja a fürt összes hello tulajdonságait, és mentse hello sablon üzembe helyezése előtt. Ezután újra felhasználhatja hello sablont.

**toogenerate sablon hello Azure-portál használatával**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **új** hello bal oldali menüben kattintson **Eszközintelligencia + analitika**, és kattintson a **HDInsight**.
3. Hajtsa végre a hello utasításokat tooenter tulajdonságait. Használhatja bármelyik hello **Gyorslétrehozás** vagy hello **egyéni** lehetőséget.
4. A hello **összegzés** lapra, majd **töltse le a sablon és a paraméterek**:

    ![HDInsight Hadoop létrehozása a fürt Resource Manager sablon letöltése](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download.png)

    Hello sablonfájl paraméterfájl és kód használt minták toodeploy hello sablon listájának megtekintéséhez:

    ![HDInsight Hadoop-fürt létrehozása Resource Manager sablon letöltési beállítások](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download-options.png)

    Itt hello sablon letöltése, mentse tooyour Sablonkönyvtár vagy hello sablon üzembe helyezése.

    tooaccess egy sablont a könyvtárban kattintson **további szolgáltatások** hello bal oldali menüben, majd kattintson a **sablonok** (alatt hello **más** kategória).

    > [!Note]
    > hello sablon és a paraméterek fájl együtt kell használni. Ellenkező esetben előfordulhat, hogy eredményt el nem várt. Például az alapértelmezett hello **clusterKind** tulajdonság értéke mindig **hadoop**, annak ellenére, hogy milyen, adja meg a hello sablon letöltése előtt.



## <a name="deploy-with-powershell"></a>Üzembe helyezés a PowerShell-lel

Ez az eljárás egy Hadoop-fürt hdinsightban hoz létre.

1. Hello JSON-fájl mentése hello [függelék](#appx-a-arm-template) tooyour munkaállomásra. Hello PowerShell-parancsfájlt, hello fájl neve: `C:\HDITutorials-ARM\hdinsight-arm-template.json`.
2. Ha szükséges, állítsa a hello paramétereket és változókat.
3. Hello sablon futtassa a következő PowerShell-parancsfájl hello használatával:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>"
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and variables
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect tooAzure
        ####################################
        #region - Connect tooAzure subscription
        Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and hello dependent storage account
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

    PowerShell parancsfájl hello csak hello fürt nevét konfigurálja. hello tárfiók neve nem változtatható hello sablonban. Biztosan felszólító tooenter hello fürt felhasználói jelszavát. (hello alapértelmezett felhasználónév az **admin**.) Biztosan is felszólító tooenter hello SSH felhasználói jelszavát. (alapértelmezett SSH-felhasználónév hello **sshuser**.)  

További információkért lásd: [telepítés a következő PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).

## <a name="deploy-with-cli"></a>A parancssori felület telepítése
a következő minta hello Azure parancssori felület (CLI) használja. A fürt és a függő tárfiókot és a tároló létrehoz egy Resource Manager-sablon meghívásával:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"

Rákérdezéses tooenter áll:
* hello fürt neve.
* hello fürt felhasználói jelszavát. (hello alapértelmezett felhasználónév az **admin**.)
* hello SSH felhasználói jelszavát. (alapértelmezett SSH-felhasználónév hello **sshuser**.)

a következő kód hello beágyazott paramétereket tartalmazza:

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-hello-rest-api"></a>Hello REST API-t üzembe helyezéséhez
Lásd: [Deploy a REST API hello](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="deploy-with-visual-studio"></a>Üzembe helyezés a Visual Studióval
 Visual Studio toocreate egy erőforráscsoport-projekt használja, és telepítse azt tooAzure hello felhasználói felületen keresztül. A projekt hello típusú erőforrások tooinclude lehetőséget választja. Ezeket az erőforrásokat automatikusan toohello Resource Manager-sablon. hello projekt PowerShell parancsfájl toodeploy hello sablont is tartalmaz.

Egy bevezető toousing Visual Studio az erőforráscsoportokhoz, lásd: [létrehozása és telepítése a Visual Studio használatával Azure erőforráscsoport-sablonok a](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="troubleshoot"></a>Hibaelhárítás

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta rendelkezik számos módon toocreate HDInsight-fürtöt. toolearn több, tekintse meg a következő cikkek hello:

* Például egy keresztül hello .NET ügyféloldali kódtár erőforrásokat üzembe helyezi, lásd: [erőforrások telepíteni a .NET-kódtárakra és egy sablon](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Részletes példa az alkalmazások központi telepítése, lásd: [kiépítése és mikroszolgáltatások kiszámítható módon tudja az Azure-ban telepítheti](../app-service-web/app-service-deploy-complex-application-predictably.md).
* A megoldás toodifferent környezetei telepítésével kapcsolatos útmutatásért lásd: [a Microsoft Azure-ban fejlesztési és tesztkörnyezetek](../solution-dev-test-environments.md).
* toolearn hello szakaszai hello Azure Resource Manager-sablon, lásd: [sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).
* Az Azure Resource Manager-sablonokban használható hello függvények listáját lásd: [sablonfüggvényei](../azure-resource-manager/resource-group-template-functions.md).

## <a name="appendix-resource-manager-template-toocreate-a-hadoop-cluster"></a>A függelék: Resource Manager sablon toocreate Hadoop-fürthöz
hello következő Azure Resource Manager sablonnal hoz létre egy Linux-alapú Hadoop-fürt hello függő Azure storage-fiók.

> [!NOTE]
> Ez a minta Hive metaadattárhoz és az Oozie metaadattárhoz konfigurációs információkat tartalmazza. Távolítsa el a hello szakaszban, vagy hello szakasz hello sablon használata előtt konfigurálnia.
>
>

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "hello name of hello HDInsight cluster toocreate."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
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
        "defaultValue": "sshuser",
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
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "hello location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "hello type of hello HDInsight cluster toocreate."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "hello number of nodes in hello HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
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
                }
            ]
            }
        }
        }
    ],
    "outputs": {
        "cluster": {
        "type": "object",
        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
        }
    }
    }

## <a name="appendix-resource-manager-template-toocreate-a-spark-cluster"></a>A függelék: Resource Manager sablon toocreate Spark-fürt

Ez a témakör a Resource Manager-sablon használható toocreate egy HDInsight Spark-fürt. Ez a sablon tartalmazza konfigurációi `spark-defaults` és `spark-thrift-sparkconf` (az 1.6-os Spark-fürtök) és `spark2-defaults` és `spark2-thrift-sparkconf` (a Spark 2-fürtök). Ezenkívül toothis, HDInsight számítja ki, és beállítja konfigurációk például `spark.executor.instances`, `spark.executor.memory`, és `spark.executor.cores` hello fürt mérete alapján. 

Bármely egy paraméter értéke a szakasz hello sablonba részeként, HDInsight nélkül kiszámításához, hello más paramétereket az hello ugyanabban a szakaszban. Például paraméter `spark.executor.instances` hello van `spark-defaults` konfigurációs. Egy másik paraméter értéke (például `spark.yarn.exector.memoryOverhead`) a hello `spark-defaults` konfigurációs, HDInsight nélkül kiszámításához, hello `spark.executor.instances` paramétert is.

    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "hello name of hello HDInsight cluster toocreate."
            }
        },
        "clusterLoginUserName": {
            "type": "string",
            "defaultValue": "admin",
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
        "location": {
            "type": "string",
            "defaultValue": "southcentralus",
            "metadata": {
                "description": "hello location where all azure resources will be deployed."
            }
        },
        "clusterVersion": {
            "type": "string",
            "defaultValue": "3.5",
            "metadata": {
                "description": "HDInsight cluster version."
            }
        },
        "clusterWorkerNodeCount": {
            "type": "int",
            "defaultValue": 4,
            "metadata": {
                "description": "hello number of nodes in hello HDInsight cluster."
            }
        },
        "clusterKind": {
            "type": "string",
            "defaultValue": "SPARK",
            "metadata": {
                "description": "hello type of hello HDInsight cluster toocreate."
            }
        },
        "sshUserName": {
            "type": "string",
            "defaultValue": "sshuser",
            "metadata": {
                "description": "These credentials can be used tooremotely access hello cluster."
            }
        },
        "sshPassword": {
            "type": "securestring",
            "metadata": {
                "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        }
    },
    "variables": {
        "defaultApiVersion": "2017-06-01",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
    {
            "apiVersion": "2015-03-01-preview",
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "dependsOn": [],
            "properties": {
                "clusterVersion": "[parameters('clusterVersion')]",
                "osType": "Linux",
                "tier": "standard",
                "clusterDefinition": {
                    "kind": "[parameters('clusterKind')]",
                    "configurations": {
                        "gateway": {
                            "restAuthCredential.isEnabled": true,
                            "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                            "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                        },
                        "spark-defaults": {
                            "spark.executor.cores": "2"
                        },
                        "spark-thrift-sparkconf": {
                            "spark.yarn.executor.memoryOverhead": "896"
                        }
                    }
                },
                "storageProfile": {
                    "storageaccounts": [
                        {
                            "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                            "isDefault": true,
                            "container": "[parameters('clusterName')]",
                            "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                        }
                    ]
                },
                "computeProfile": {
                    "roles": [
                        {
                            "name": "headnode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 2,
                            "hardwareProfile": {
                                "vmSize": "Standard_D12"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                }
                            },
                            "virtualNetworkProfile": null,
                            "scriptActions": []
                        },
                        {
                            "name": "workernode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 4,
                            "hardwareProfile": {
                                "vmSize": "Standard_D4"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                    }
                                },
                                "virtualNetworkProfile": null,
                                "scriptActions": []
                            }
                        ]
                    }
                }
            }
        ]
    }
