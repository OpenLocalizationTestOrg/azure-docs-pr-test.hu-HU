---
title: "aaaPass összetett értékek tartománya: Azure-sablonok |} Microsoft Docs"
description: "Azt mutatja be ajánlott megközelítés használatos összetett objektumok tooshare állapot adatait az Azure Resource Manager-sablonok és a csatolt sablonok használatával."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fd2f5e2d-d56d-4e01-a57d-34f3eaead4a9
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: tomfitz
ms.openlocfilehash: 72df1dee351446cea6ce15269e6db288b1f1db79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="share-state-tooand-from-azure-resource-manager-templates"></a>Megosztás állapot tooand Azure Resource Manager-sablonok alapján
Ez a témakör gyakorlati tanácsok a kezelése és megosztása a sablonokon belüli állapotát jeleníti meg. hello paraméterek és változók ebben a témakörben szereplő példák hello típusú objektumok definiálhat tooconveniently rendezheti a telepítési követelményeket. Ezekben a példákban a saját objektumok, amelyek a környezetben jelentéssel bírnak valósíthat meg.

Ez a témakör egy nagyobb tanulmány részét képezi. Töltse le a teljes papír tooread hello [globális osztály Resource Manager sablonok szempontok és eljárások bizonyítása](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

## <a name="provide-standard-configuration-settings"></a>Adja meg a szabványos konfigurációs beállítások
Helyett a sablont, amely a teljes és számos változata kínál, egy közös mintát a kijelölt ismert konfigurációk tooprovide. Érvényben a felhasználók kiválaszthatják például védőfal kis, közepes és nagy szabványos pólóra méretét. Más pólóra méretű például a termék ajánlatokat, community edition vagy enterprise edition. Más esetekben lehet munkaterhelés-specifikus konfigurációk egy technológia – például a térkép csökkentése vagy a nem sql.

Összetett objektumok, amelyek tartalmazzák a gyűjtött adatokat, más néven "tulajdonságcsomagok" változók létrehozása és a sablonban lévő adatok toodrive hello erőforrás nyilatkozatot használva. Ezt a módszert biztosít az ügyfelek különböző méretű előre konfigurált megfelelő, ismert konfigurációk. Ismert konfigurációk nélkül hello sablon felhasználók kell fürt méretezése a platform erőforrás korlátozó saját, tényező határozza meg, és tegye matematikai tooidentify hello eredő storage-fiókok és egyéb erőforrások particionálás (toocluster mérete miatt és erőforrás-korlátozások). Továbbá toomaking hello ügyfél hatékonyabb működését, néhány ismert konfigurációk egyszerűbb toosupport és sűrűség magasabb szintű fájlmegosztásba nyújt segítséget.

a következő példa azt mutatja meg hogyan hello toodefine változókat, amelyek tartalmazzák a gyűjtött adatokat jelképező összetett objektumra. hello gyűjtemények megadása a virtuális gép méretét, a hálózati beállításokat, az operációs rendszeri beállítások és a rendelkezésre állási beállítások használt értékek.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Figyelje meg, hogy hello **tshirtSize** változó összefűzi egy paraméteren keresztül megadott hello Póló mérete (**kis**, **Közepes**, **nagy**) toohello szöveg **tshirtSize**. A változó tooretrieve hello társított összetett objektumot változó használata pólóra méret.

Ezek a változók később hello sablonban majd hivatkozhat. hello képességét tooreference nevű-változók és azok tulajdonságaival egyszerűbbé teszi a hello sablonok szintaxisát, és teszi, hogy könnyen toounderstand környezetben. a következő példa hello egy erőforrás toodeploy hello objektumok előzőleg bemutatott tooset értékek használatával határozza meg. Hello Virtuálisgép-méretet lekérésével hello értékét állítsa például `variables('tshirtSize').vmSize` hello érték során hello lemez méretét veszi át a `variables('tshirtSize').diskSize`. Emellett a csatolt sablon hello értékkel van beállítva a hello URI `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-tooa-template"></a>Hozzáférési állapot tooa sablon
Megosztott állapot paramétereknek, hogy közvetlenül a telepítés során a sablonba.

a következő tábla listák általánosan használt paraméterek a sablonok hello.

| Név | Érték | Leírás |
| --- | --- | --- |
| location |Az Azure-régiók korlátozott listájából karakterlánc |hello hely, ahol hello erőforrások telepítése. |
| storageAccountNamePrefix |Karakterlánc |Hello Tárfiókot, ahol hello virtuális gép lemezeinek kerülnek egyedi DNS-neve |
| Tartománynév |Karakterlánc |Hello nyilvánosan elérhető jumpbox VM hello formátumban tartománynevet: **{tartománynév}. { hely}.cloudapp.com** például: **mydomainname.westus.cloudapp.azure.com** |
| adminUsername |Karakterlánc |Virtuális gépek hello tartozó felhasználónév |
| adminPassword |Karakterlánc |Virtuális gépek hello jelszavát |
| tshirtSize |A korlátozott listájából karakterlánc kínált pólóra méretek |hello nevű méretezési egység méretének tooprovision. Például "Kicsi", "közepes", "Nagy" |
| virtualNetworkName |Karakterlánc |Hello fogyasztói hello virtuális hálózat neve toouse szeretne. |
| enableJumpbox |Korlátozott listájából (engedélyezett vagy letiltott) karakterlánc |Paraméter, amely azonosítja az e tooenable egy jumpbox hello környezethez. Értékek: "engedélyezve", "Letiltva" |

Hello **tshirtSize** hello előző szakaszban használt paraméter típusúként van definiálva:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of hello MongoDB deployment"
        }
      }
    }


## <a name="pass-state-toolinked-templates"></a>Továbbítsa állapot toolinked sablonok
Ha toolinked sablonok, gyakran statikus vegyesen használ, és létrehozott változókat.

### <a name="static-variables"></a>Statikus változó
Statikus változók értékei gyakran használt tooprovide alap, például a használt URL-, egy sablon egész.

A következő sablon cikkből hello `templateBaseUrl` hello gyökerét hello sablon meghatározza a Githubon. hello következő sor összeállít egy új változót `sharedTemplateUrl` , amely összefűzi hello alap URL-cím elé hello ismert hello megosztott erőforrások sablon nevét. Adott sor alatt egy összetett objektum változó akkor használt toostore pólóra méretétől, melynél hello alap URL-cím összefűzött toohello ismert konfiguráció sablon helyére, és a rendszer a hello `vmTemplate` tulajdonság.

hello ezt a módszert előnye, hogy ha hello sablon helye megváltozik, csak kell toochange hello statikus változó az egyik helyen, amely továbbítja azokat a teljes kapcsolódó hello sablonok.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>Létrehozott változók
Továbbá toostatic változók több változók generálása dinamikusan történik. Ez a szakasz azonosítja a legelterjedtebb fajtáit hello közös létrehozott változókat.

#### <a name="tshirtsize"></a>tshirtSize
A fenti hello példákban a létrehozott változó tisztában.

#### <a name="networksettings"></a>networkSettings
A kapacitás, a funkció vagy a végpont hatókörön belüli megoldássablonban, hello kapcsolt sablonok általában létrehozása meglévő erőforrások hálózaton. Egy egyszerű megközelítése toouse egy összetett objektum toostore hálózati beállításokat, és adja meg azokat toolinked sablonok.

Hálózati beállítások kommunikáció például alább látható.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings
A csatolt sablonok létrejött erőforrásokat gyakran kerülnek egy rendelkezésre állási csoportot. A következő példa hello hello rendelkezésre állási készlet neve van megadva, és is hello tartalék tartományt, és tartományi száma toouse frissítéséhez.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Ha több rendelkezésre állási csoportok (például egy fő csomópontok) és egy másik adatok csomópontok előtagjaként is használhatja a nevét, adjon meg több rendelkezésre állási csoportok, vagy hello modell létrehozásához egy adott pólóra méretű változót korábban bemutatott kövesse.

#### <a name="storagesettings"></a>storageSettings
Csatolt sablonok gyakran megosztott tárolási részleteit. Az alábbi hello példában egy *storageSettings* objektum ismerteti hello tárolási fiók és a tároló nevét.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings
A csatolt sablonokkal szükség lehet az toopass operációs rendszer beállításait toovarious csomópontok típusú különböző ismert konfigurációs típusok között. Egy összetett objektumot egy egyszerűen toostore és megosztási operációsrendszer-információkat, és megkönnyíti a könnyebb toosupport több operációs rendszer lehetőség központi telepítéshez.

hello alábbi példa bemutatja egy objektumot *osSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings
A létrehozott változó *machineSettings* core változók a virtuális gép létrehozása vegyesen tartalmazó összetett objektum. hello változók közé tartoznak a rendszergazdai felhasználónevet és jelszót, hello virtuális gép nevét egy előtagot és egy operációs rendszer lemezképének mutató hivatkozás.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Vegye figyelembe, hogy *osImageReference* lekéri hello hello értékeinek *osSettings* hello fő sablonban definiált változó. Ez azt jelenti, hogy könnyen módosíthatja a virtuális gépek hello operációs rendszer – teljes egészében vagy sablon felhasználóinak hello beállítás alapján.

#### <a name="vmscripts"></a>vmScripts
Hello *vmScripts* objektumra a hello parancsfájlok toodownload kapcsolatos részleteket tartalmaz, és egy Virtuálisgép-példány, beleértve a külső és belső hivatkozások hajt végre. Kívülről való hivatkozások hello infrastruktúra is.
Belső hivatkozások hello telepített szoftvereket és a konfigurációs tartalmazza.

Hello használata *scriptsToDownload* tulajdonság toolist hello parancsfájlok toodownload toohello virtuális gép. Ez az objektum is tartalmaz hivatkozásokat toocommand-sor argumentumok különféle műveletek. Ilyen műveletek közé tartoznak, hello alapértelmezett telepítés minden egyes csomóponton, egy telepítés, amely minden csomópont telepítése után fut és esetlegesen a sablonban megadott adott tooa további parancsfájlok végrehajtása.

Ebben a példában a MongoDB, amely egy soron toodeliver magas rendelkezésre állást igényel használt sablon toodeploy származik. Hello *arbiterNodeInstallCommand* túl hozzá lett adva*vmScripts* tooinstall hello soron.

hello változók szakaszban, ahol található hello változókat, amelyek meghatározzák a hello adott szöveg tooexecute hello parancsfájl hello megfelelő értékekkel.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Egy sablonból visszatérési állapota
Nem csak egy sablonba át adatokat, is megosztás hátsó toohello hívó sablon. A hello **kimenete** szakasz csatolt sablon, megadhatja a kulcs/érték párok, amelyeket a hello Forrássablon lehet használni.

hello következő példa bemutatja, hogyan toopass hello magánhálózati IP-cím egy csatolt sablon hozott létre.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

Hello fő sablont, belül használható hello szintaxisa a következő adatokat:

    "[reference('master-node').outputs.masterip.value]"

Ebben a kifejezésben hello kimenetek szakasz vagy hello források szakaszában hello fő sablont is használhatja. Hello kifejezés nem használható hello változók szakaszban, mert hello futásidejű állapot támaszkodnak. tooreturn hello fő sablont, használja ezt az értéket:

    "outputs": {
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }

Hello használatának példája adja kimenetként, egy virtuális géphez csatolt sablon tooreturn adatlemezek szakaszában, a következő témakörben: [hozzon létre egy virtuális gép több adatlemezek](resource-group-create-multiple.md).

## <a name="define-authentication-settings-for-virtual-machine"></a>Virtuális gép hitelesítési beállításainak megadása
Hello használható konfigurációs beállítások toospecify hello hitelesítési beállításait a virtuális gépek korábban bemutatott minta. Hello típusú hitelesítés sikeres paramétere hoz létre.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

Hozzáadhat hello különböző hitelesítési típust és egy változó toostore, mely a hello paraméter értékének hello alapján központi telepítéshez használt változókat.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Hello virtuális gép definiálásakor meg hello **osProfile** létrehozott toohello változó.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Következő lépések
* hello sablon hello szakaszai toolearn lásd [Azure Resource Manager sablonok készítése](resource-group-authoring-templates.md)
* a sablonon belül elérhető toosee hello funkciók lásd [Azure Resource Manager Sablonfüggvényei](resource-group-template-functions.md)
