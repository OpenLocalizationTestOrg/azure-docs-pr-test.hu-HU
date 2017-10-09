---
title: "aaaCustomize HDInsight-fürtök Parancsfájlműveletek - Azure használatával |} Microsoft Docs"
description: "Adja hozzá a tooLinux-alapú HDInsight-fürtök Parancsfájlműveletek segítségével egyéni összetevőket. A Parancsfájlműveletek olyan Bash parancsfájlok használt toocustomize hello fürtkonfiguráció vagy adja hozzá a további szolgáltatások és segédprogramok Hue, Solr vagy R."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48e85f53-87c1-474f-b767-ca772238cc13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: ff22680a8a50b21985f6941f1edaf1dcf863d13f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-linux-based-hdinsight-clusters-using-script-action"></a>Parancsfájlművelet Linux-alapú HDInsight-fürtök testreszabása

A HDInsight lehetővé nevű konfigurációs beállítás **parancsfájlművelet** , amely elindítja az egyéni parancsfájlok, amelyek hello fürt testreszabása. Ezek a parancsfájlok használt tooinstall további összetevőket, és konfigurációs beállításokat módosítaná. A Parancsfájlműveletek használható során vagy a fürt létrehozása után.

> [!IMPORTANT]
> Linux-alapú HDInsight-fürtök hello képességét toouse Parancsfájlműveletek egy már futó fürt csak érhető el.
>
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Parancsfájlműveletek HDInsight alkalmazásként is közzétett toohello Azure piactéren. Ebben a dokumentumban hello példák némelyike bemutatják, hogyan telepítse a PowerShell és a .NET SDK hello parancsfájl művelet parancsokkal HDInsight-alkalmazások. További információk a HDInsight-alkalmazások: [hello Azure piactér közzé HDInsight-alkalmazások](hdinsight-apps-publish-applications.md).

## <a name="permissions"></a>Engedélyek

Ha egy tartományhoz csatlakozó HDInsight-fürtöt használ, nincsenek két Ambari szükséges engedélyek hello fürt integrált parancsfájl-műveletek használata esetén:

* **AMBARI. Futtatás\_egyéni\_parancs**: hello Ambari rendszergazdai szerepkör rendelkezik ezzel az engedéllyel, alapértelmezés szerint.
* **A FÜRT. Futtatás\_egyéni\_parancs**: mindkét hello HDInsight fürt rendszergazdája és Ambari rendszergazda alapértelmezés szerint rendelkezik ilyen engedéllyel.

Tartományhoz csatlakozó HDInsight engedélyeket munkáról bővebben lásd: [tartományhoz HDInsight-fürtök kezelése](hdinsight-domain-joined-manage.md).

## <a name="access-control"></a>Hozzáférés-vezérlés

Ha nem hello az Azure-előfizetéshez, rendszergazdai vagy tulajdonosa, a fióknak rendelkeznie kell legalább **közreműködő** hozzáférés toohello tartalmazó erőforráscsoportot hello HDInsight-fürthöz.

Emellett ha hoz létre HDInsight-fürtöt, valaki legalább **közreműködő** hozzáférés toohello Azure-előfizetéssel kell már korábban regisztrált HDInsight hello szolgáltatója. A szolgáltató regisztrációja történik, ha egy felhasználó közreműködői hozzáférés toohello előfizetéssel hoz létre hello erőforrás először hello az előfizetésben. Az erőforrás létrehozása nélkül is elvégezhető, ha [ a REST segítségével regisztrál egy szolgáltatót](https://msdn.microsoft.com/library/azure/dn790548.aspx).

A access management munkáról bővebben lásd a következő dokumentumok hello:

* [Hozzáférés-kezelés hello Azure-portálon az első lépések](../active-directory/role-based-access-control-what-is.md)
* [Szerepkör hozzárendelések toomanage hozzáférés tooyour Azure-előfizetés erőforrásainak használatához](../active-directory/role-based-access-control-configure.md)

## <a name="understanding-script-actions"></a>A Parancsfájlműveletek ismertetése

A parancsfájlművelet egyszerűen egy Bash parancsfájlok mutató URI-t biztosító, de paramétereit. hello parancsfájl hello HDInsight-fürt csomópontján fut. hello a következők jellemzők és Parancsfájlműveletek funkcióit.

* URI, amely a HDInsight-fürt hello elérhető kell tárolni. Az alábbiakban hello lehetséges tárolókba:

    * Egy **Azure Data Lake Store** hello HDInsight-fürt által elérhető fiók. Azure Data Lake Store és a HDInsight együttes használatáról további információért lásd: [HDInsight-fürtök létrehozása a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

        A Data Lake Store-ban tárolt parancsfájl használata esetén hello URI-formátum van `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.

        > [!NOTE]
        > hello szolgáltatás egyszerű HDInsight használ tooaccess Data Lake Store toohello parancsfájl olvasási hozzáféréssel kell rendelkeznie.

    * A blob egy **Azure Storage-fiók** , hogy-e vagy hello elsődleges vagy további tárfiók hello HDInsight-fürthöz. HDInsight kapnak hozzáférést tooboth az ilyen típusú tárfiókok a fürt létrehozása során.

    * Egy nyilvános fájlmegosztási szolgáltatással, például az Azure Blob, a GitHub, a onedrive vállalati verzió, a Dropbox, a stb.

        Például URI, lásd: hello [parancsfájl művelet parancsfájlpéldákat](#example-script-action-scripts) szakasz.

        > [!WARNING]
        > Csak a HDInsight támogatja __általános célú__ Azure Storage-fiókokat. Jelenleg nem támogatja hello __Blob-tároló__ fiók típusa.

* Túl korlátozható**futhat az egyes csomóponttípusok**, például átjárócsomópontokkal vagy munkavégző csomópontokhoz.

  > [!NOTE]
  > HDInsight prémium használata esetén adja meg, hogy hello parancsfájl hello élcsomópont kell használni.

* Lehet **megőrzött** vagy **alkalmi**.

    **Megőrzött** parancsfájlok fürtöket alkalmazott tooworker csomópontok hozzáadott toohello hello parancsfájl futtatása után. Például, amikor hello fürt vertikális felskálázásával.

    Egy megőrzött parancsfájl is előfordulhat, hogy alkalmazza a módosításokat tooanother csomóponttípus, például egy átjárócsomóponttal.

  > [!IMPORTANT]
  > A megőrzött Parancsfájlműveletek egy egyedi névvel kell rendelkeznie.

    **Az ad hoc** parancsfájlok nem maradnak meg. Nincsenek alkalmazott tooworker csomópontok hozzáadott toohello fürt után hello parancsfájl lefutott rendelkezik. Ezt követően előléptetheti az alkalmi parancsfájl tooa megőrzött parancsfájl vagy egy megőrzött parancsfájl tooan alkalmi parancsfájl fokozni.

  > [!IMPORTANT]
  > Fürt létrehozása során használt Parancsfájlműveletek automatikusan megmaradnak.
  >
  > Parancsfájlok, amelyek nem sikertelen tárolt, még akkor is, ha kifejezetten Megadja, hogy azok kell lennie.

* Fogadhat **paraméterek** használt hello parancsfájl végrehajtása közben.
* Futtassa a **gyökér szintű jogosultságokkal** hello fürtcsomópontokon.
* Hello keresztül használható **Azure-portálon**, **Azure PowerShell**, **Azure CLI**, vagy **HDInsight .NET SDK**

hello fürt megtart lett futtatott összes parancsfájl előzményeit. hello előzmények akkor hasznos, ha egy parancsfájl toofind hello azonosítója a(z) előléptetés vagy a lefokozás műveletek van szüksége.

> [!IMPORTANT]
> Nincs nincs automatikus módon tooundo hello egy parancsfájl műveletet hajtott végre. Manuálisan fordított hello módosításokat, vagy adjon meg egy parancsfájlt, amely visszavonja őket.


### <a name="script-action-in-hello-cluster-creation-process"></a>Hello fürtlétrehozás parancsfájlművelet

Fürt létrehozása során használt Parancsfájlműveletek némileg eltérnek a műveletek egy meglévő fürthöz futtatott parancsfájl:

* hello parancsfájl **automatikusan megőrzött**.
* A **hiba** hello parancsfájl hello fürt létrehozási folyamat toofail azt okozhatja.

hello alábbi ábrán látható parancsfájl hello létrehozási folyamata során kerül végrehajtásra:

![A HDInsight fürt testreszabási és fürt létrehozása során szakaszai][img-hdi-cluster-states]

hello parancsfájl fut, amíg a HDInsight konfigurálása. Ebben a szakaszban hello parancsfájl futtatása párhuzamosan összes hello hello fürt és a legfelső szintű jogosultságokkal fut hello csomóponton megadott csomópontok.

> [!NOTE]
> Mivel hello parancsfájl fut gyökér szintű jogosultsággal rendelkező hello fürtcsomópontokon, például a szolgáltatások, beleértve a Hadoop-kapcsolatos szolgáltatások indítása és leállítása műveleteket hajthat végre. Ha leállítja a szolgáltatások, bizonyosodjon meg, hogy hello Ambari szolgáltatás és más Hadoop kapcsolatos szolgáltatások működik, és mielőtt hello parancsfájl befejezése után történik. Ezek a szolgáltatások szükségesek toosuccessfully hello állapotát és hello fürt állapotának meghatározásához azt létrehozása közben.


Fürt létrehozása során is használhat több Parancsfájlműveletek egyszerre. Ezek a parancsfájlok hívják meg volt hello sorrendben.

> [!IMPORTANT]
> Parancsfájlműveletek 60 perc, vagy időtúllépés belül kell végrehajtani. Fürt telepítése során, hello parancsfájl egyidejűleg más beállítás és konfiguráció folyamatok fut. Erőforrások, például a CPU-idő vagy a hálózati sávszélesség erőforrásokért való versengés hello parancsfájl tootake hosszabb toofinish azonban nem a fejlesztési környezetet, mint okozhat.
>
> toominimize hello alkalommal vesz toorun hello parancsfájl, feladatokat, mint a letöltés és forrásból származó alkalmazások fordítása elkerülése érdekében. Előre lefordítani az alkalmazásokat, és az Azure Storage hello bináris tárolja.


### <a name="script-action-on-a-running-cluster"></a>Egy futó fürtön parancsfájlművelet

Egy hiba történt egy parancsfájlt a fürt létrehozása során használt műveletek egy már futó fürtön futtatott parancsfájl eltérően nem automatikusan okoz hello fürtállapot toochange tooa nem sikerült. A parancsfájl befejeztét követően hello fürt tooa "fut" állapotba kell visszaadnia.

> [!IMPORTANT]
> Akkor is, ha hello fürt "fut" állapotú, hello sikertelen parancsfájlt is megszakítják dolgot. A parancsfájl például hello fürt számára szükséges fájlok sikerült törölni.
>
> Parancsfájlok műveletek futtassa legfelső szintű jogosultságokkal, ezért meg kell győződnie arról, hogy tudomásul veszi a parancsfájl funkciója tooyour fürt alkalmazása előtt.

Egy parancsfájl tooa fürt alkalmazásakor hello fürt állapota megváltozik-e toofrom **futtató** túl**elfogadott**, majd **HDInsight konfigurációs**, és végül biztonsági túl**Futtató** sikeres parancsfájlok. hello parancsfájl állapot hello parancsfájlművelet-előzmény van bejelentkezve, és ezen információk toodetermine használhatja, hogy hello parancsfájl sikeres vagy sikertelen volt. Például hello `Get-AzureRmHDInsightScriptActionHistory` PowerShell-parancsmag használt tooview hello állapotát egy parancsfájlt is lehet. Információk a következő szöveg hasonló toohello adja vissza:

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!NOTE]
> Ha módosította hello fürt (rendszergazda) felhasználói jelszó hello fürt létrehozása után, műveletek lefutott-e a fürt parancsfájl sikertelen lehet. Ha a megőrzött Parancsfájlműveletek adott cél munkavégző csomópontokhoz, ezek a parancsfájlok nem tud hello fürt méretezni.

## <a name="example-script-action-scripts"></a>Példa parancsfájlművelet parancsfájlok

Parancsfájl művelet parancsfájlok keresztül hello segédeszközök a következő használható:

* Azure Portal
* Azure PowerShell
* Azure CLI
* HDInsight .NET SDK

HDInsight összetevők követően a HDInsight-fürtökön parancsfájlok tooinstall hello biztosítja:

| Név | Szkript |
| --- | --- |
| **Egy Azure Storage-fiók hozzáadása** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxaddstorageaccountv01/Add-Storage-Account-v01.SH. Lásd: [Hozzáadás további tárhely tooan HDInsight-fürt](hdinsight-hadoop-add-storage.md). |
| **Hue telepítése** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-hue-uber-v02.SH. Lásd: [telepítése és használata a HDInsight Hue-fürtök](hdinsight-hadoop-hue-linux.md). |
| **Presto telepítése** |https://RAW.githubusercontent.com/hdinsight/presto-hdinsight/Master/installpresto.SH. Lásd: [telepítése és használata Presto a HDInsight-fürtök](hdinsight-hadoop-install-presto.md). |
| **Solr telepítése** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsolrconfigactionv01/solr-Installer-v01.SH. Lásd: [telepítése és használata Solr a HDInsight-fürtök](hdinsight-hadoop-solr-install-linux.md). |
| **Giraph telepítése** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxgiraphconfigactionv01/giraph-Installer-v01.SH. Lásd: [telepítése és használata Giraph a HDInsight-fürtök](hdinsight-hadoop-giraph-install-linux.md). |
| **Hive-könyvtárakhoz előzetes betöltése** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsetupcustomhivelibsv01/Setup-customhivelibs-v01.SH. Lásd: [kódtárak hozzáadása Hive HDInsight-fürtök](hdinsight-hadoop-add-hive-libraries.md). |
| **Mono telepítése vagy frissítése** | https://hdiconfigactions.BLOB.Core.Windows.NET/Install-mono/Install-mono.bash. Lásd: [telepítése vagy frissítése a HDInsight monó](hdinsight-hadoop-install-mono.md). |

## <a name="use-a-script-action-during-cluster-creation"></a>Egy parancsfájlművelettel fürt létrehozása során

Ez a szakasz hello különböző módokon használhatja a Parancsfájlműveletek létrehozása a HDInsight-fürtök esetén a példákat.

### <a name="use-a-script-action-during-cluster-creation-from-hello-azure-portal"></a>Egy parancsfájlművelettel hello Azure-portálon a fürt létrehozása során

1. Indítsa el a fürt létrehozása a részben ismertetett módon [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md). Állítsa le a hello elérésekor __fürt összefoglaló__ szakasz.

2. A hello __fürt összefoglaló__ szakaszban, jelölje be hello __szerkesztése__ hivatkozás a __speciális beállítások__.

    ![Speciális beállítások hivatkozása](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. A hello __speciális beállítások__ szakaszban jelölje be __parancsfájl-műveletek__. A hello __parancsfájl-műveletek__ szakaszban jelölje be __+ Submit újdonságai__

    ![Egy új parancsfájlművelet](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. Használjon hello __válassza ki a parancsprogramot__ bejegyzés tooselect egy előre elkészített parancsfájlt. toouse egyéni parancsfájl, válassza ki __egyéni__ és adja meg a hello __neve__ és __Bash parancsfájlok URI__ a parancsfájlt.

    ![A parancsprogramok hozzáadásához hello válassza parancsfájl formátumban](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    a következő táblázat hello hello űrlap hello elemeket ismerteti:

    | Tulajdonság | Érték |
    | --- | --- |
    | Válassza ki a parancsprogramot | toouse a saját parancsfájl, jelölje be __egyéni__. Ellenkező esetben válasszon egyet a megadott hello parancsfájlok. |
    | Név |Adja meg a hello parancsfájlművelet nevét. |
    | Bash parancsfájlok URI |Hello URI toohello parancsfájl megadásához, amely meghívott toocustomize hello fürt. |
    | HEAD/munkavégző/Zookeeper |Adja meg a hello csomópontok (**Head**, **munkavégző**, vagy **ZooKeeper**) mely hello testreszabási a parancsfájl futtatása. |
    | Paraméterek |Adja meg a hello paraméterek hello parancsfájl által szükség esetén. |

    Használjon hello __parancsfájlműveletet__ bejegyzés tooensure, hogy a parancsfájl hello skálázás műveletek során alkalmazzák.

5. Válassza ki __létrehozása__ toosave hello parancsfájl. Ezután __+ új küldési__ tooadd parancsfájlt.

    ![Több parancsprogram-műveletek](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    Amikor elkészült a parancsfájlok hozzáadását, használja a hello __kiválasztása__ gombra, és ezután hello __tovább__ gomb tooreturn toohello __fürt összefoglaló__ szakasz.

3. toocreate hello fürt, jelölje be __létrehozása__ a hello __fürt összefoglaló__ kijelölés.

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a>Egy parancsfájlművelettel Azure Resource Manager-sablonok alapján

Ebben a szakaszban hello példák bemutatják, hogyan toouse parancsfájl-műveletek Azure Resource Manager-sablonok.

#### <a name="before-you-begin"></a>Előkészületek

* A munkaállomás toorun HDInsight Powershell-parancsmagok konfigurálásával kapcsolatos további információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).
* Útmutatást toocreate sablonjainak használatáról [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).
* Ha nem korábban használt Azure PowerShell a Resource Manager, lásd: [az Azure PowerShell használata Azure Resource Managerrel](../azure-resource-manager/powershell-azure-resource-manager.md).

#### <a name="create-clusters-using-script-action"></a>Parancsfájlművelet-fürtök létrehozása

1. A sablon tooa helye a számítógépen a következő hello másolja. Ez a sablon telepítése Giraph hello headnodes és munkavégző hello fürt csomópontja. Ha a hello JSON-sablon esetében érvényes is ellenőrizheti. Illessze be a sablon a tartalom [JSONLint](http://jsonlint.com/), online JSON-érvényesítési segédprogramot.

            {
            "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "clusterLocation": {
                    "type": "string",
                    "defaultValue": "West US",
                    "allowedValues": [ "West US" ]
                },
                "clusterName": {
                    "type": "string"
                },
                "clusterUserName": {
                    "type": "string",
                    "defaultValue": "admin"
                },
                "clusterUserPassword": {
                    "type": "securestring"
                },
                "sshUserName": {
                    "type": "string",
                    "defaultValue": "username"
                },
                "sshPassword": {
                    "type": "securestring"
                },
                "clusterStorageAccountName": {
                    "type": "string"
                },
                "clusterStorageAccountResourceGroup": {
                    "type": "string"
                },
                "clusterStorageType": {
                    "type": "string",
                    "defaultValue": "Standard_LRS",
                    "allowedValues": [
                        "Standard_LRS",
                        "Standard_GRS",
                        "Standard_ZRS"
                    ]
                },
                "clusterStorageAccountContainer": {
                    "type": "string"
                },
                "clusterHeadNodeCount": {
                    "type": "int",
                    "defaultValue": 1
                },
                "clusterWorkerNodeCount": {
                    "type": "int",
                    "defaultValue": 2
                }
            },
            "variables": {
            },
            "resources": [
                {
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-05-01-preview",
                    "dependsOn": [ ],
                    "tags": { },
                    "properties": {
                        "accountType": "[parameters('clusterStorageType')]"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-03-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Storage/storageAccounts/', parameters('clusterStorageAccountName'))]"
                    ],
                    "tags": { },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "hadoop",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterUserPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [
                                {
                                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                    "isDefault": true,
                                    "container": "[parameters('clusterStorageAccountContainer')]",
                                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                                }
                            ]
                        },
                        "computeProfile": {
                            "roles": [
                                {
                                    "name": "headnode",
                                    "targetInstanceCount": "[parameters('clusterHeadNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installGiraph",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                },
                                {
                                    "name": "workernode",
                                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installR",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                }
                            ]
                        }
                    }
                }
            ],
            "outputs": {
                "cluster":{
                    "type" : "object",
                    "value" : "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                }
            }
        }
2. Indítsa el az Azure PowerShell, és jelentkezzen be tooyour Azure-fiók. Miután megadta a hitelesítő adatait, a hello parancs visszaadja a fiókja adatait.

        Add-AzureRmAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...
3. Ha több előfizetéssel, adja meg az előfizetés-azonosító hello kívánja toouse központi telepítéshez.

        Select-AzureRmSubscription -SubscriptionID <YourSubscriptionId>

    > [!NOTE]
    > Használhat `Get-AzureRmSubscription` tooget, beleértve az előfizetés-azonosító hello minden egyes a fiókjához társított előfizetéseket listáját.

4. Ha nem rendelkezik egy meglévő erőforráscsoportot, hozzon létre egy erőforráscsoportot. Adja meg a hello erőforráscsoportot és helyet, a megoldáshoz szükséges hello nevét. Új erőforráscsoport hello összegzését adja vissza.

        New-AzureRmResourceGroup -Name myresourcegroup -Location "West US"

        ResourceGroupName : myresourcegroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. az erőforráscsoport, futtassa a hello telepítésének toocreate **New-AzureRmResourceGroupDeployment** parancsot, és adja meg a szükséges paraméterek hello. hello paraméternek számít a hello a következő adatokat:

    * A központi telepítés nevét
    * az erőforráscsoport neve hello
    * hello elérési útja vagy URL-cím létrehozott toohello sablont.

  Ha a sablon bármely paramétereket igényel, át kell adnia ezeket a paramétereket. Hello parancsfájl művelet tooinstall R hello fürtön ebben az esetben nincs szükség a paramétereket.

        New-AzureRmResourceGroupDeployment -Name mydeployment -ResourceGroupName myresourcegroup -TemplateFile <PathOrLinkToTemplate>

    Hello sablon hello paraméterek felszólító tooprovide értékei lesznek.

1. Hello erőforráscsoportban van telepítve, hello telepítés összegzését jelenik meg.

          DeploymentName    : mydeployment
          ResourceGroupName : myresourcegroup
          ProvisioningState : Succeeded
          Timestamp         : 8/14/2017 7:00:27 PM
          Mode              : Incremental
          ...

2. Ha nem sikerül a telepítés, a következő parancsmagok tooget információ hello hibák hello is használhatja.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName myresourcegroup -ProvisioningState Failed

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a>Egy parancsfájlművelettel Azure PowerShell a fürt létrehozása során

Ebben a szakaszban hello használjuk [Add-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) parancsmag tooinvoke parancsfájlokat parancsfájlművelet toocustomize fürt használatával. A folytatás előtt győződjön meg arról, hogy telepítette és konfigurálta az Azure PowerShell. A munkaállomás toorun HDInsight PowerShell-parancsmagok konfigurálásával kapcsolatos további információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).

hello alábbi parancsfájl bemutatja, hogyan tooapply egy parancsfájlművelet, amikor a PowerShell használatával fürtöt hoz létre:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

Hello fürt létrehozása előtt több percet is igénybe vehet.

### <a name="use-a-script-action-during-cluster-creation-from-hello-hdinsight-net-sdk"></a>Egy parancsfájlművelettel hello HDInsight .NET SDK a fürt létrehozása során

hello HDInsight .NET SDK biztosít, amely megkönnyíti a .NET-alkalmazás és a HDInsight együttes könnyebb toowork klienskódtárak segítségével. Kódminta, lásd: [létrehozása Linux-alapú fürtökön a Hdinsightban az hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).

## <a name="apply-a-script-action-tooa-running-cluster"></a>A fürt futtató parancsfájlművelet tooa alkalmazása

Ebben a szakaszban megtudhatja, hogyan tooapply parancsfájl-műveletek tooa rendszerű fürt.

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-azure-portal"></a>A fürt futtatja hello Azure-portálon parancsfájlművelet tooa alkalmazása

1. A hello [Azure-portálon](https://portal.azure.com), válassza ki a HDInsight-fürthöz.

2. Hello HDInsight fürt áttekintése, válassza ki hello **Parancsfájlműveletek** csempére.

    ![Parancsfájl műveletek csempe](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > Igény szerint kiválaszthatja **összes beállítás** , és válassza **Parancsfájlműveletek** a hello beállítások szakaszban.

3. A Parancsfájlműveletek szakasz hello hello felső részén jelölje ki **új küldési**.

    ![A parancsfájl tooa futtató fürt hozzáadása](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. Használjon hello __válassza ki a parancsprogramot__ bejegyzés tooselect egy előre elkészített parancsfájlt. toouse egyéni parancsfájl, válassza ki __egyéni__ és adja meg a hello __neve__ és __Bash parancsfájlok URI__ a parancsfájlt.

    ![A parancsprogramok hozzáadásához hello válassza parancsfájl formátumban](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    a következő táblázat hello hello űrlap hello elemeket ismerteti:

    | Tulajdonság | Érték |
    | --- | --- |
    | Válassza ki a parancsprogramot | toouse a saját parancsfájl, jelölje be __egyéni__. Ellenkező esetben válassza ki a megadott parancsfájlt. |
    | Név |Adja meg a hello parancsfájlművelet nevét. |
    | Bash parancsfájlok URI |Hello URI toohello parancsfájl megadásához, amely meghívott toocustomize hello fürt. |
    | HEAD/munkavégző/Zookeeper |Adja meg a hello csomópontok (**Head**, **munkavégző**, vagy **ZooKeeper**) mely hello testreszabási a parancsfájl futtatása. |
    | Paraméterek |Adja meg a hello paraméterek hello parancsfájl által szükség esetén. |

    Használjon hello __parancsfájlműveletet__ bejegyzés toomake meg arról, hogy hello parancsfájl skálázás műveletek során alkalmazzák.

5. Végül, használja a hello **létrehozása** gomb tooapply hello parancsfájl toohello fürt.

### <a name="apply-a-script-action-tooa-running-cluster-from-azure-powershell"></a>A fürt futtatja az Azure PowerShell parancsfájlművelet tooa alkalmazása

A folytatás előtt győződjön meg arról, hogy telepítette és konfigurálta az Azure PowerShell. A munkaállomás toorun HDInsight PowerShell-parancsmagok konfigurálásával kapcsolatos további információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).

hello következő példa bemutatja, hogyan tooapply parancsfájl művelet tooa működő fürthöz:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

Hello művelet után információk hasonló toohello a következő szöveg jelenik meg:

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-azure-cli"></a>A fürt hello Azure CLI-ről fut parancsfájlművelet tooa alkalmazása

A folytatás előtt győződjön meg arról, hogy telepítette és konfigurálta a hello Azure CLI. További információkért lásd: [telepítés hello Azure CLI](../cli-install-nodejs.md).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. tooswitch tooAzure Resource Manager módra a következő parancsot a következő hello parancssori hello használata:

        azure config mode arm

2. A következő tooauthenticate tooyour Azure-előfizetés hello használata.

        azure login

3. A következő parancs tooapply egy fürtben futó parancsfájl művelet tooa hello használata

        azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>

    Ha nincs megadva ez a parancs paramétereit, kéri azokat. Ha a megadott parancsfájl hello `-u` fogad el paramétert, megadhatja őket hello segítségével `-p` paraméter.

    Érvénytelen csomópont típusok `headnode`, `workernode`, és `zookeeper`. Ha hello parancsfájl alkalmazott toomultiple csomóponttípusok, adja meg a elválasztott hello típusokat egy ";". Például: `-n headnode;workernode`.

    toopersist hello parancsfájl, vegye fel a hello `--persistOnSuccess`. Akkor is is megmaradnak hello parancsfájl később használatával `azure hdinsight script-action persisted set`.

    Amint hello feladat befejeződik, a következő szöveg kimeneti hasonló toohello kapni:

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-tooa-running-cluster-using-rest-api"></a>A futó REST API használatával fürt parancsfájlművelet tooa alkalmazása

Lásd: [Parancsfájlműveletek futtatása a futó fürt](https://msdn.microsoft.com/library/azure/mt668441.aspx).

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-hdinsight-net-sdk"></a>A fürt hello HDInsight .NET SDK-ről fut parancsfájlművelet tooa alkalmazása

Például egy hello .NET SDK tooapply parancsfájlok tooa fürt használatával, lásd: [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

## <a name="view-history-promote-and-demote-script-actions"></a>Előzményeinek megtekintése, előléptetése és lefokozása Parancsfájlműveletek

### <a name="using-hello-azure-portal"></a>Hello Azure-portál használatával

1. A hello [Azure-portálon](https://portal.azure.com), válassza ki a HDInsight-fürthöz.

2. Hello HDInsight fürt áttekintése, válassza ki hello **Parancsfájlműveletek** csempére.

    ![Parancsfájl műveletek csempe](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > Igény szerint kiválaszthatja **összes beállítás** , és válassza **Parancsfájlműveletek** a hello beállítások szakaszban.

4. Ehhez a fürthöz parancsfájlok előzményeit hello Parancsfájlműveletek szakaszban látható. Ezen információk közé tartozik a megőrzött parancsfájlok listáját. A hello alábbi képernyőképen látható megtekintheti, hogy ezen a fürtön futtatott parancsfájl lett Solr hello. képernyőfelvétel a hello nem szerepelnek a megőrzött parancsfájlokat.

    ![Parancsfájl-műveletek szakasz](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. Hello tulajdonságok szakaszának ehhez a parancsprogramhoz parancsfájl kijelölésével hello előzményeit jeleníti meg. A hello felső az üdvözlő képernyőt futtassa újra a hello parancsfájl, vagy előléptetheti.

    ![Parancsfájl műveletek tulajdonságai](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. Is használhatja a hello **...**  toohello sarkában hello Parancsfájlműveletek szakasz tooperform műveletek bejegyzéseket.

    ![Parancsfájl-műveletek... kihasználtsága](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="using-azure-powershell"></a>Az Azure PowerShell használata

| Használja a hello következő... | túl... |
| --- | --- |
| Get-AzureRmHDInsightPersistedScriptAction |A megőrzött Parancsfájlműveletek információk beolvasása |
| Get-AzureRmHDInsightScriptActionHistory |Parancsfájl műveletek alkalmazása toohello fürt, vagy egy adott parancsfájl részletes előzményeit beolvasása |
| Set-AzureRmHDInsightPersistedScriptAction |Az ad hoc elősegíthető parancsfájl művelet tooa megőrzött parancsfájl művelet |
| Remove-AzureRmHDInsightPersistedScriptAction |Úgy fokozza le egy megőrzött parancsfájl művelet tooan alkalmi művelet |

> [!IMPORTANT]
> Használatával `Remove-AzureRmHDInsightPersistedScriptAction` nem vonja vissza a parancsfájl által végrehajtott hello műveleteket. Ez a parancsmag csak eltávolítja hello megőrzött jelzőt.

a példaként megadott parancsfájlt a következő hello hello parancsmagok toopromote segítségével bemutatja, majd egy parancsfájl fokozni.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="using-hello-azure-cli"></a>Hello Azure parancssori felület használatával

| Használja a hello következő... | túl... |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |A megőrzött Parancsfájlműveletek listájának beolvasása |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |Egy adott megőrzött parancsfájlművelet információk beolvasása |
| `azure hdinsight script-action history list <clustername>` |Parancsfájl műveletek alkalmazása toohello fürt előzményeit beolvasása |
| `azure hdinsight script-action history show <clustername> <scriptname>` |Egy adott parancsfájlművelet információk beolvasása |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |Az ad hoc elősegíthető parancsfájl művelet tooa megőrzött parancsfájl művelet |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |Úgy fokozza le egy megőrzött parancsfájl művelet tooan alkalmi művelet |

> [!IMPORTANT]
> Használatával `azure hdinsight script-action persisted delete` nem vonja vissza a parancsfájl által végrehajtott hello műveleteket. Ez a parancsmag csak eltávolítja hello megőrzött jelzőt.

### <a name="using-hello-hdinsight-net-sdk"></a>Hello HDInsight .NET SDK használatával

Például egy hello .NET SDK tooretrieve parancsfájl előzmények fürtök használata, lépteti elő vagy parancsfájlok fokozni, lásd: [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

> [!NOTE]
> Ez a példa is mutatja be, hogyan egy HDInsight alkalmazást, amely tooinstall hello .NET SDK-val.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>A HDInsight-fürtökön használt nyílt forráskódú szoftvereket támogatása

a Microsoft Azure HDInsight szolgáltatás hello egy nyílt forráskódú technológiák formátumú-e körül Hadoop ökoszisztémájának használja. A Microsoft Azure támogatási általános szintű nyílt forráskódú technológiák nyújt. További információkért lásd: hello **támogatási hatókör** hello szakasza [Azure támogatás – gyakori kérdések webhely](https://azure.microsoft.com/support/faq/). hello HDInsight-szolgáltatás egy további szintű támogatást biztosít a beépített összetevők.

Nyílt forráskódú összetevők hello HDInsight szolgáltatás elérhető két típusa van:

* **A beépített összetevők** -ezeket az összetevőket a HDInsight-fürtök előre telepített, és alapfunkciókat hello fürt adja meg. YARN ResourceManager, hello Hive query language (HiveQL) és hello Mahout könyvtár például toothis kategóriába tartozik. A kiszolgálófürt-összetevők teljes listáját a érhető [What's new in HDInsight által biztosított hello Hadoop-fürt verziók](hdinsight-component-versioning.md).
* **Egyéni összetevők** -hello fürt felhasználóként, telepítése vagy használata előtt a munkaterhelésnek valamelyik összetevő a hello közösségi vagy Ön által létrehozott.

> [!WARNING]
> HDInsight-fürt hello összetevői teljes mértékben támogatottak. Microsoft Support tooisolate segít, és a kapcsolódó toothese összetevők problémák megoldásához.
>
> Egyéni összetevők kap minden üzleti szempontból ésszerű támogatási toohelp meg toofurther hello problémával kapcsolatos hibaelhárítás elősegítéséhez. A Microsoft támogatási képes tooresolve hello probléma vagy azok kérheti tooengage elérhető csatorna a hello nyílt forráskódú technológiák részletes segítséget, hogy a technológiát találhatók. Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).

HDInsight-szolgáltatás hello toouse egyéni összetevők számos lehetőséget biztosít. hello azonos szintű támogatást vonatkoznak, függetlenül attól, hogyan összetevőt használja, és hello fürtön telepítve. hello alábbi lista ismerteti azokat a hello leggyakoribb módszereket, hogy használható-e az egyéni összetevőkben HDInsight-fürtök:

1. Feladat elküldése - Hadoop és egyéb feladatok végrehajtása, vagy használjon egyéni összetevők elküldött toohello tárolófürt is lehet.

2. Fürt testreszabási - fürt létrehozása során megadhatja további beállításokat és egyéni hello fürtcsomópontokon telepített összetevőket.

3. Minták – a népszerű egyéni összetevők, a Microsoft és a mások által biztosított mintákat ezeket az összetevőket hogyan használható a hello HDInsight-fürtökön. Ezeket a mintákat támogatás nélkül van megadva.

## <a name="troubleshooting"></a>Hibaelhárítás

Ambari webes felhasználói felület tooview által naplózott információ Parancsfájlműveletek is használhatja. Hello parancsfájlt a fürt létrehozása során nem sikerül, ha hello naplókat is rendelkezésre állnak a hello hello-fürthöz tartozó alapértelmezett tárfiók. Ebben a szakaszban megtudhatja hogyan tooretrieve hello naplózza, mindkét ezen beállítások használatával.

### <a name="using-hello-ambari-web-ui"></a>Hello Ambari webes felhasználói felület használatával

1. A böngészőben nyissa meg a toohttps://CLUSTERNAME.azurehdinsight.net. CLUSTERNAME cserélje le a HDInsight fürt hello neve.

    Amikor a rendszer kéri, adja meg (rendszergazda) hello admin fióknevet és jelszót hello fürt. Előfordulhat, hogy tooreenter hello rendszergazdai hitelesítő adatokat egy webes űrlapon.

2. Hello sorából hello oldal hello tetején, válassza ki a hello **ops** bejegyzés. Hello fürtön Ambari keresztül végrehajtott jelenlegi és korábbi műveletek listája jelenik meg.

    ![Ambari webes felhasználói felület sávon kicserélhet kijelölt ops](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. Keresés hello bejegyzéseket **futtatása\_customscriptaction** a hello **műveletek** oszlop. Ezek a bejegyzések hello Parancsfájlműveletek futásakor jönnek létre.

    ![Képernyőkép a műveletek](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    tooview hello STDOUT és az STDERR kimeneti, jelöljön ki hello run\customscriptaction bejegyzést, és hello található hivatkozásokon keresztül részletekbe menően tárhatják. A kimenet jön létre, amikor hello parancsfájl fut, és hasznos információt tartalmazhat.

### <a name="access-logs-from-hello-default-storage-account"></a>Az alapértelmezett tárfiók hello belépési naplók

Hello fürt létrehozása tooa parancsfájl műveleti hiba miatt nem sikerül, ha hello naplók elérhetők hello fürt tárfiók.

* hello tárolási érhetők el naplók a `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.

    ![Képernyőkép a műveletek](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    Ebben a könyvtárban, a hello naplók vannak rendszerezve külön-külön headnode, workernode és zookeeper csomópontok. Néhány példa:

    * **Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`

    * **Munkavégző csomópont** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`

    * **Zookeeper csomópont** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`

* Az stdout és az stderr hello megfelelő gazdagép toohello tárfiók feltöltve. Van egy **kimeneti -\*.txt** és **hibák -\*.txt** parancsfájl műveleteket. hello kimeneti-*.txt fájlt kapott hello gazdagépen futó hello parancsfájl hello URI információt tartalmaz. Példa:

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* Lehetséges, hogy meg ismételten hozzon létre egy parancsfájl művelet fürtöt hello ugyanazzal a névvel. Ilyen esetben különböztetheti meg egymástól a naplók megfelelő hello hello dátum mappa neve alapján. Például hello mappaszerkezet különböző időpontokban létrehozott fürt (sajátfürt) jelenik meg, a következő naplóbejegyzések hasonló toohello:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Ha egy parancsfájl művelet fürtöt hoz létre hello azonos név a hello azonos nap, hello egyedi előtag tooidentify hello kapcsolatos fontos naplófájlokat is használhatja.

* 12:00 -kor (éjfél) mellett a fürt létrehozása, akkor lehetséges, hogy hello naplófájlok span két napon keresztül. Ilyen esetben két külön dátummal mappát hello láthat ugyanabban a fürtben.

* Feltöltése naplózási fájlok toohello alapértelmezett tároló too5 perc, különösen olyan nagyméretű fürtök is eltarthat. Ezért ha azt szeretné, hogy tooaccess hello naplókat, törölni kell, nem azonnal hello fürt Ha egy parancsfájl művelet sikertelen.

### <a name="ambari-watchdog"></a>Ambari figyelő

> [!WARNING]
> Ne változtassa meg a Linux-alapú HDInsight-fürtök az Ambari figyelő (hdinsightwatchdog) hello hello jelszavát. Változó hello jelszót a fiókhoz tartozó hello képességét toorun új Parancsfájlműveletek hello HDInsight-fürt megszakítja.

### <a name="cant-import-name-blobservice"></a>BlobService neve nem lehet importálni.

__A jelenség__: hello parancsfájl művelet sikertelen lesz. Hiba a következő szöveg hasonló toohello Ambari hello művelet megtekintésekor jelenik meg:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__OK__: Ez a hiba akkor fordul elő, ha hello HDInsight-fürt részét képező hello Python Azure Storage ügyfelet frissít. A HDInsight az Azure Storage ügyfél 0.20.0 vár.

__Megoldási__: tooresolve hibára, manuálisan a tooeach fürt csomópont használatával csatlakozzon `ssh` és a következő parancs tooreinstall hello megfelelő tárolási ügyfélverzió hello használata:

```
sudo pip install azure-storage==0.20.0
```

Az SSH csatlakozó toohello fürtön információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="history-doesnt-show-scripts-used-during-cluster-creation"></a>Előzmények nem jelenik meg a fürt létrehozása során használt parancsfájlok

Ha a fürt létrehozása előtt 2016. március 15-én nem jelenhet meg a parancsfájlművelet előzményeinek bejegyzése. 2016. március 15-én után átméretezésekor hello fürt hello parancsfájlok használata a fürt létrehozása során jelennek meg előzmények, a rendszer alkalmazza azokat hello fürt hello részeként toonew csomópontja átméretezése műveletet.

Ez alól két kivétel van:

* Ha a fürt 2015. szeptember 1. előtt létrehozott. Ez a dátum esetén a Parancsfájlműveletek lett bevezetve. Ez a dátum előtt létrehozott fürthöz sikerült nem használta a Parancsfájlműveletek a fürt létrehozásához.

* Ha több Parancsfájlműveletek fürt létrehozása során használt, és azonos több parancsfájl nevet, vagy hello hello használt azonos nevet, ugyanilyen URI, de több parancsfájl különböző paramétereit. Ezekben az esetekben hello a következő hiba jelenhet meg:

    Nem lehet műveleteket új parancsfájl lefutott, tooconflicting parancsfájl nevét a meglévő parancsfájlok miatt ezen a fürtön. Fürt elérhető parancsfájl nevét kell létrehozni minden egyedi lehet. Meglévő parancsfájlok megbízhatóságához ablakhoz.

## <a name="next-steps"></a>Következő lépések

* [A HDInsight parancsfájlművelet-parancsfájlok fejlesztése](hdinsight-hadoop-script-actions-linux.md)
* [Telepítheti és használhatja a HDInsight-fürtök Solr](hdinsight-hadoop-solr-install-linux.md)
* [Telepítheti és használhatja a HDInsight-fürtök Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [További tárhely tooan HDInsight-fürt hozzáadása](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Fürt létrehozása során szakaszból"
