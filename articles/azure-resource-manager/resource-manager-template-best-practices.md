---
title: "aaaBest eljárások létrehozásához a Resource Manager-sablonok |} Microsoft Docs"
description: "Az Azure Resource Manager-sablonok egyszerűsítésére irányelveket."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 31b10deb-0183-47ce-a5ba-6d0ff2ae8ab3
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: tomfitz
ms.openlocfilehash: ec9bbe218c4f2c6a92ca44b5e9c9c71029e22151
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a>Ajánlott eljárások az Azure Resource Manager-sablonok létrehozása
Iránymutatás segítséget nyújtanak az Azure Resource Manager-sablonok, amelyek megbízható és könnyű toouse létrehozásához. hello irányelvek csak javaslatok. Nincsenek követelmények, kivéve, ha az áttelepítés előtt feljegyzett. Adott esetben előfordulhat, hogy egy hello egyik változata a következő módszerekkel vagy példák.

## <a name="resource-names"></a>Erőforrások neve
Általában három típusú erőforrásnevek a Resource Manager használata:

* Erőforrás neve, amely egyedinek kell lennie.
* Erőforrás neve nem egyedi toobe szükséges, de úgy dönt, hogy a nevet, amely segítségével azonosíthatja környezetben alapuló erőforrás tooprovide.
* Erőforrás neve, amely lehet általános.

 Erőforrás neve vonatkozó megkötésekkel kapcsolatos további információkért lásd: [Azure-erőforrások elnevezési szabályai ajánlott](../guidance/guidance-naming-conventions.md).

### <a name="unique-resource-names"></a>Egyedi erőforrás neve
Egy adat-hozzáférési végponttal rendelkezik erőforrás típussal egyedi erőforrás nevét kell megadnia. Néhány gyakori erőforrástípusok esetében, amelyek egyedi nevet kell megadni a következők:

* Az Azure Storage<sup>1</sup> 
* Web Apps funkció az Azure App Service-ben
* SQL Server
* Azure Key Vault
* Azure Redis Cache
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Az Azure HDInsight

<sup>1</sup> tárfiókneveket is kisbetűnek kell lennie, 24 karakter vagy kevesebb, és nem rendelkezik a kötőjel.

Egy paramétert az erőforrás nevét adja meg, ha hello erőforrás telepítésekor meg kell adnia egy egyedi nevet. Másik lehetőségként létrehozhat hello használó változó [uniqueString()](resource-group-template-functions-string.md#uniquestring) toogenerate nevét működik. 

Azt is előfordulhat, hogy szeretné tooadd előtag vagy utótag toohello **uniqueString** eredménye. Módosítása hello egyedi név segítségével további hello erőforrástípus neve alapján hello könnyebb azonosításához. Például egy egyedi nevet a tárfiók hozhat létre a következő változó hello segítségével:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Az azonosításhoz erőforrásnevek
Egyes erőforrástípusok esetében érdemes tooname, de a nevek nem rendelkezik egyedi toobe. Ezen erőforrás esetében adja meg a nevet, amely azonosítja az erőforrás-környezet hello és a hello erőforrástípus. Adjon meg egy leíró nevet, hogy könnyebb legyen azonosítani hello erőforrás tartozó erőforrások listáját. Ha toouse különböző központi telepítésénél a különböző erőforrás nevét, a paraméter használható hello nevét:

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "hello name of hello VM toocreate."
        }
    }
}
```

Toopass egy nevet a telepítés során nem szükséges, ha egy változót is használhatja: 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

A kódolt érték is használható:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
}
```

### <a name="generic-resource-names"></a>Általános erőforrás neve
Erőforrás esetében, amelyek többnyire keresztül érhető el egy másik erőforráscsoportban általános neve nem változtatható hello sablonban is használhatja. Például beállíthatja egy szabványos, általános nevet tűzfalszabályok SQL-kiszolgálón:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="parameters"></a>Paraméterek
hello alábbi információ segítségével esetleg megállapítható paraméterek használata:

* Minimalizálása érdekében a paraméterek használatával. Amikor csak lehetséges, használjon egy változó vagy konstansérték. Paraméterek csak ezek helyzetekben használhatja:
   
   * A beállítások, amelyet az toouse változatait tooenvironment (SKU, méret, kapacitás) szerint.
   * Erőforrás nevét, amelyet az toospecify könnyen azonosításához.
   * Az értékeket, hogy a gyakran használt toocomplete más feladatokat (például egy rendszergazda felhasználójának neve).
   * A titkos kulcsokat (például a jelszavak).
   * hello száma vagy értékek toouse erőforrástípus több példánya létrehozásakor tömbje.
* -És nagybetűhasználattal teve a paraméterek nevei.
* Adjon meg egy leírást, minden paraméter hello metaadatokban:

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "hello type of hello new storage account created toostore hello VM disks."
           }
       }
   }
   ```

* Adja meg (kivéve a jelszavak és SSH-kulcsok) paraméterek alapértelmezett értéke:
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "hello type of hello new storage account created toostore hello VM disks."
            }
        }
   }
   ```

* Használjon **SecureString** a jelszavak és a titkos kulcsok: 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "hello value of hello secret toostore in hello vault."
           }
       }
   }
   ```

* Amikor csak lehetséges, ne használjon egy paraméter toospecify helyre. Ehelyett használja a hello **hely** hello erőforráscsoport tulajdonság. Hello segítségével **resourceGroup () .location** az erőforrások kifejezését erőforrások hello sablon telepítése hello hello erőforráscsoport és ugyanazon a helyen:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         ...
     }
   ]
   ```
   
   Ha egy erőforrás típusa támogatott helyek csak korlátozott számú, érdemes toospecify hello sablonban közvetlenül egy érvényes helyet. Használata esetén egy **hely** paraméter, a lehető legnagyobb mértékben paraméterérték megosztása a hello valószínűleg toobe erőforrásokhoz ugyanazon a helyen. Ez minimalizálja a hello szám, ahányszor a felhasználó felkérést kap tooprovide helyére vonatkozó információkat.
* Kerülje a paraméter vagy változó hello API-verzió erőforrástípus. Erőforrás-tulajdonságok és értékek alapján verziószáma változhat. A kód szerkesztése az IntelliSense nem határozható meg, hogy megfelelő sémát hello hello API-verzió beállítása tooa paraméter vagy változó. Ehelyett kódolnia hello hello sablonban API-verzió.

## <a name="variables"></a>Változók
hello alábbi információ segítségével esetleg megállapítható változók használata:

* Változók használata, toouse csak egyszer kell a sablonban lévő érték. Ha az érték csak egyszer legyen használva, kódolt értéket a sablon könnyebb tooread lehetővé teszi.
* Nem használhat hello [hivatkozás](resource-group-template-functions-resource.md#reference) hello függvényt **változók** hello sablon szakasza. Hello **hivatkozás** függvény hello erőforrás futásidejű állapot az értékét osztályból származik. Azonban a változók feloldva hello kezdeti hello sablon elemzése során. Értékek, amelyek hello kell összeállítani **hivatkozás** függvény közvetlenül a hello **erőforrások** vagy **kimenete** hello sablon szakasza.
* Az erőforrás nevét, amely egyedinek kell lennie, a változókat tartalmazhat [erőforrásnevek](#resource-names).
* Változók összetett objektumokba csoportosíthatja. Használjon hello **variable.subentry** formázása tooreference egy összetett objektum közötti értéket. Változók segítségével nyomon követheti a kapcsolódó változók. Ez növeli a hello sablon olvashatóság érdekében is. Íme egy példa:
   
   ```json
   "variables": {
       "storage": {
           "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
           "type": "Standard_LRS"
       }
   },
   "resources": [
     {
         "type": "Microsoft.Storage/storageAccounts",
         "name": "[variables('storage').name]",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "sku": {
             "name": "[variables('storage').type]"
         },
         ...
     }
   ]
   ```
   
   > [!NOTE]
   > Egy összetett objektum nem tartalmazhat egy kifejezés, amely egy összetett objektumot egy értéket a hivatkozásokat. Külön változó erre a célra.
   > 
   > 
   
     Speciális összetett objektumok használatával változókként, tekintse meg a [megosztani az Azure Resource Manager sablonokban állapot](best-practices-resource-manager-state.md).

## <a name="resources"></a>Erőforrások
hello alábbi információ segítségével esetleg megállapítható erőforrások használatakor:

* toohelp más közreműködők megértéséhez hello erőforrás hello célját, adja meg **megjegyzések** hello sablonban az egyes erőforrások:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used toostore hello VM disks.",
         ...
     }
   ]
   ```

* Címkék tooadd metaadatok tooresources is használhatja. Az erőforrások metaadatok tooadd adatait használja. Például hozzáadhat metaadatok toorecord számlázási tudnivalókról erőforrás. További információkért lásd: [Using címkéket tooorganize az Azure-erőforrások](resource-group-using-tags.md).
* Ha egy *nyilvános végpontot* a sablonban (például egy Azure Blob storage nyilvános végpontot), *do nem rögzített kód* névtér hello. Használjon hello **hivatkozás** függvény toodynamically hello névtér beolvasása. Ez a megközelítés toodeploy hello sablon toodifferent nyilvános névtér-környezetekben hello végpont hello sablonban manuális módosítása nélkül is használhatja. Állítsa be a hello API-verzió toohello hello tárfiókot használja a sablon azonos verziójú:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Ha az hello tárfiók hello ugyanazt a sablont, amely hoz létre, nem kell toospecify hello szolgáltatójának névtere amikor hello erőforrás hivatkozik. Ez az hello egyszerűsített szintaxist:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Ha a sablont más értékek, amelyek a konfigurált toouse nyilvános névtér, ezek helyett tooreflect hello azonos **hivatkozás** függvény. Például beállíthatja a hello **storageUri** hello virtuális gép diagnosztikai profiljának tulajdonsága:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Meglévő tárfiókot, amely egy másik erőforráscsoportban található is hivatkozhat:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Rendelje hozzá a nyilvános IP címek tooa virtuális gép csak akkor, ha egy alkalmazás írja elő. tooconnect tooa virtuális gép (VM) hibakereséshez vagy felügyeleti vagy felügyeleti célokra, használja a bejövő NAT-szabályok, a virtuális hálózati átjáró vagy egy jumpbox.
   
     Csatlakozás toovirtual gépek kapcsolatos további információkért lásd:
   
   * [Futtassa a virtuális gépek Azure-ban N szintű architektúrája](../guidance/guidance-compute-n-tier-vm.md)
   * [A WinRM-hozzáférés beállítása az Azure Resource Manager virtuális gépekhez](../virtual-machines/windows/winrm.md)
   * [Külső hozzáférés tooyour VM engedélyezése hello Azure-portál használatával](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [Külső hozzáférés tooyour VM engedélyezése a PowerShell használatával](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Külső hozzáférés tooyour Linux virtuális gép engedélyezése az Azure parancssori felület használatával](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* Hello **domainNameLabel** nyilvános IP-címekhez tulajdonságnak egyedinek kell lennie. Hello **domainNameLabel** értéket kell 3 és 63 karakter között lehet, és kövesse a reguláris kifejezés által meghatározott hello szabályok: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Mivel hello **uniqueString** függvény karakterláncot hoz létre, amely 13 karakterig, hello **dnsPrefixString** paraméter korlátozott too50 karakterek:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "hello DNS label for hello public IP address. It must be lowercase. It should match hello following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* A jelszó tooa egyéni parancsprogramok futtatására szolgáló bővítmény hozzáadásakor használja hello **commandToExecute** hello tulajdonság **protectedSettings** tulajdonság:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > tooensure, amelyek a titkos kulcsok titkosított, amikor azok paraméterek tooVMs és bővítmények át lettek adva, használja a hello **protectedSettings** hello vonatkozó bővítmények tulajdonság.
   > 
   > 

## <a name="outputs"></a>kimenetek
Ha egy sablon toocreate nyilvános IP-címeket használ, egy **kimenete** szakasz adatait hello IP-cím és hello teljesen minősített tartománynevét (FQDN) adja vissza. A telepítés utáni kimeneti értékek tooeasily lekérése adatait nyilvános IP-címek és a teljes tartománynevek is használhatja. Ha hello erőforrás hivatkozik, amellyel toocreate hello API verzióját használja azt: 

```json
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-vs-nested-templates"></a>Egy sablon és a beágyazott sablonok
toodeploy a megoldás több beágyazott sablonok használhatja ugyanazt a sablont vagy a fő sablont. Beágyazott sablonok olyan közös speciális forgatókönyvekhez. Egy beágyazott sablon ad meg a következő előnyöket hello használata:

* A célként megadott összetevők megoldás is lebontva.
* Eltérő fő sablonok beágyazott sablonok is felhasználhatja.

Ha úgy dönt, hogy a beágyazott toouse sablonok, a következő irányelveket hello segítségével a sablon tervezési szabványosítására. Ezeket az irányelveket alapuló [tervezése során az Azure Resource Manager-sablonok minták](best-practices-resource-manager-design-templates.md). Azt javasoljuk, amely rendelkezik a következő sablonok hello:

* **Fő sablon** (azuredeploy.json). Ezzel az hello bemeneti paraméterek.
* **Megosztott erőforrások sablon**. Használjon toodeploy megosztott erőforrásokat, amelyek más erőforrások (például: virtuális hálózat és a rendelkezésre állási készlet). Használjon hello **dependsOn** kifejezés tooensure, hogy ez a sablon olyan sablon előtt van-e telepíteni.
* **Nem kötelező erőforrások sablon**. Használjon tooconditionally erőforrások (például jumpbox) paraméter alapján telepítheti.
* **Tag erőforrások sablon**. Minden belül egy alkalmazás réteget példánytípust saját konfigurációval rendelkezik. A réteg belül különböző példány típust határoznak meg. (Például hello első példányát egy fürtöt hoz létre, és a további példányt toohello meglévő fürt adja hozzá.) Minden példány rendelkezik saját központi telepítési sablont.
* **Parancsfájlok**. Széles körben újrafelhasználható parancsfájlok alkalmazhatók minden példány típusa (például inicializálása és formázása további lemezek). Egy adott testreszabási célra létrehozott egyéni parancsfájlok eltérőek, attól függően hello sablonpéldány típusúvá.

![Beágyazott sablon](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

További információkért lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).

## <a name="conditionally-link-toonested-templates"></a>Feltételes hivatkozás toonested sablonok
A paraméter tooconditionally hivatkozás toonested sablonokat használhat. hello paraméter hello URI hello sablon része lesz:

```json
"parameters": {
    "newOrExisting": {
        "type": "String",
        "allowedValues": [
            "new",
            "existing"
        ]
    }
},
"variables": {
    "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
},
"resources": [
    {
        "apiVersion": "2015-01-01",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "[variables('templatelink')]",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
            }
        }
    }
]
```

## <a name="template-format"></a>Sablon formátumban
Egy jó gyakorlat toopass a sablon egy JSON-érvényesítő keresztül. Egy érvényesítő segítségével távolítsa el a felesleges vessző, zárójelek és zárójelek, üzembe helyezése során hibát okozhat. Próbálja [JSONLint](http://jsonlint.com/) vagy a kedvenc linter csomag szerkesztésével környezet (Visual Studio Code, Atom, Sublime Text, Visual Studio).

Egyúttal egy jó ötlet tooformat a JSON jobb olvashatóság érdekében. A helyi szerkesztő JSON formázó csomag használható. A Visual Studio tooformat hello dokumentum, nyomja le az **Ctrl + K, Ctrl + D**. A Visual Studio Code, nyomja le a **Alt + Shift + F**. Ha a helyi szerkesztő nem formázza hello dokumentum, egy [online formázó](https://www.bing.com/search?q=json+formatter).

## <a name="next-steps"></a>Következő lépések
* A megoldás a virtuális gépek újratervezni a útmutatóért lásd: [futtassa egy Windows virtuális Gépet az Azure-ban](../guidance/guidance-compute-single-vm.md) és [Linux virtuális gép futtatása az Azure-ban](../guidance/guidance-compute-single-vm-linux.md).
* A storage-fiók beállításával kapcsolatos útmutatásért lásd: [Azure Storage teljesítményére és méretezhetőségére ellenőrzőlista](../storage/common/storage-performance-checklist.md).
* arról, hogyan vállalati erőforrás-kezelő tooeffectively toolearn-előfizetések kezelése című [Azure enterprise scaffold: részletes utasításokkal megadott előfizetés irányítás](resource-manager-subscription-governance.md).

