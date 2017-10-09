---
title: "aaaAzure Resource Manager sablon struktúra és szintaxis |} Microsoft Docs"
description: "Hello struktúra és az Azure Resource Manager-sablonok deklaratív JSON-szintaxis használatával tulajdonságait ismerteti."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2017
ms.author: tomfitz
ms.openlocfilehash: b0709852f8777c91cc1704d6bca16257a017d515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-hello-structure-and-syntax-of-azure-resource-manager-templates"></a>Hello struktúra és az Azure Resource Manager-sablonok szintaxisát ismertetése
Ez a témakör ismerteti az Azure Resource Manager sablon hello szerkezete. Azt mutatja be a különböző szakaszaiban hello egy sablon és hello elérhető tulajdonságok köre szakaszt. hello sablon JSON és, hogy tooconstruct értékeket is használhat az üzembe helyezéshez kifejezések áll. A sablonok létrehozásának részletes oktatóanyaga, lásd: [az első Azure Resource Manager-sablon létrehozása](resource-manager-create-first-template.md).

## <a name="template-format"></a>Sablon formátumban
A legegyszerűbb szerkezetét egy sablon tartalmazza a következő elemek hello:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| Elem neve | Szükséges | Leírás |
|:--- |:--- |:--- |
| $schema |Igen |Hello JSON séma-fájl helyét, amely leírja a hello hello sablon nyelvi verzióját. A fenti példa hello látható hello URL-címet használja. |
| contentVersion |Igen |Hello sablon (például 1.0.0.0) verzióját. Bármely érték biztosíthat ehhez az elemhez. Erőforrások hello sablonnal való telepítésekor, az érték lehet használt toomake arra, hogy hello a megfelelő sablon használja. |
| paraméterek |Nem |Értékek által biztosított, ha a központi telepítés végrehajtása toocustomize erőforrások telepítése. |
| változók |Nem |Hello toosimplify sablon sablonnyelvi kifejezéseket JSON szilánkok használt értékek. |
| Erőforrások |Igen |Telepített vagy frissített erőforráscsoportban erőforrástípusok esetében. |
| kimenetek |Nem |A telepítés utáni visszaadott értékek. |

Minden elem beállítható tulajdonságokat tartalmaz. a következő példa hello sablon hello teljes szintaxisát tartalmazza:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  
        "<parameter-name>" : {
            "type" : "<type-of-parameter-value>",
            "defaultValue": "<default-value-of-parameter>",
            "allowedValues": [ "<array-of-allowed-values>" ],
            "minValue": <minimum-value-for-int>,
            "maxValue": <maximum-value-for-int>,
            "minLength": <minimum-length-for-string-or-array>,
            "maxLength": <maximum-length-for-string-or-array-parameters>,
            "metadata": {
                "description": "<description-of-hello parameter>" 
            }
        }
    },
    "variables": {  
        "<variable-name>": "<variable-value>",
        "<variable-name>": { 
            <variable-complex-type-value> 
        }
    },
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

Azt vizsgálja meg a témakör későbbi részében részletesebben hello sablon hello szakaszait.

## <a name="expressions-and-functions"></a>Kifejezések és funkciók
hello alapszintű hello sablon szintaxisa JSON. Kifejezések és a funkciók kiterjesztése hello JSON értékek belül hello sablon érhető el.  Kifejezések íródtak belül JSON szövegkonstansok amelynek első és utolsó karaktere hello zárójelek közé: `[` és `]`, illetve. hello érték hello kifejezés kiértékelése hello sablon telepítésekor. Megírva egy szöveges karakterlánc, miközben hello eredménye hello kifejezés kiértékelése lehet eltérő típusú JSON, például a tömb vagy egész, attól függően, hogy hello tényleges kifejezést.  szövegkonstansnak indítsa el a zárójel toohave `[`, de nem rendelkezik az kifejezésként értelmezi, a felesleges zárójel toostart hello karakterlánc hozzáadása `[[`.

Általában akkor használják kifejezések funkciók tooperform műveletekkel hello telepítési konfigurálásához. Csak a például a JavaScript, függvényhívások-ként formázott `functionName(arg1,arg2,arg3)`. Tulajdonságok hello pont és a [index] operátorok használatával hivatkozik.

hello a következő példa bemutatja, hogyan több működik térített toouse értékeket:

```json
"variables": {
    "location": "[resourceGroup().location]",
    "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
    "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

Hello a sablon funkciók teljes listáját, lásd: [Azure Resource Manager sablonfüggvényei](resource-group-template-functions.md). 

## <a name="parameters"></a>Paraméterek
Hello paraméterek szakaszban hello sablon megadhatja, mely értékei telepítésekor szövegszerkesztőben hello erőforrásokat. A paraméterértékek lehetővé teszik a toocustomize hello telepítési egy adott környezetben (például a fejlesztői, tesztelési és éles) is lefednek értékek megadásával. Nincs tooprovide paramétereket a sablonban, de a paraméterek nélkül a sablon mindig központilag hello ugyanazokat az erőforrásokat a hello azonos nevek, helyek és -tulajdonságokat.

A következő struktúra hello definiálhatja a paramétereket:

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-hello parameter>" 
        }
    }
}
```

| Elem neve | Szükséges | Leírás |
|:--- |:--- |:--- |
| parameterName |Igen |Hello paraméter neve. Egy érvényes JavaScript-azonosítónak kell lennie. |
| type |Igen |Hello paraméter értékének típusa. Engedélyezett típusokkal hello listáját lásd a táblázat után. |
| DefaultValue érték |Nem |Hello paraméter, ha nincs érték megadva hello paraméter alapértelmezett értéke. |
| Storageaccount_accounttype |Nem |Engedélyezett értékek hello paraméter toomake, hogy hello jobb oldali értéktengely rendelkezik tömbje. |
| A MinValue |Nem |hello minimális int típusú paraméterekhez, ez az érték értéke között lehet. |
| MaxValue |Nem |hello maximális int típusú paraméterekhez, ez az érték értéke között lehet. |
| a minLength |Nem |hello minimális string, secureString és array típusú paraméterekhez, ez az érték hossza határokat is beleértve. |
| MaxLength |Nem |hello maximális string, secureString és array típusú paraméterekhez, ez az érték hossza határokat is beleértve. |
| leírás |Nem |Hello paraméter, amely leírása toousers hello portálon jelenik meg. |

hello engedélyezett típusokkal és értékek a következők:

* **karakterlánc**
* **secureString**
* **int**
* **logikai érték**
* **objektum** 
* **secureObject**
* **a tömb**

toospecify választható, mert egy paraméter adja meg a DefaultValue érték (üres is lehet). 

A paraméter neve a sablon megfelelő hello parancs toodeploy hello sablon egyik paraméterének ad meg, ha nincs megadta hello értékek lehetséges egyértelműek. Erőforrás-kezelő hello utótag hozzáadásával oldja fel a félreértések **FromTemplate** toohello sablonparaméter. Például, ha nevű paraméter **ResourceGroupName** a sablonban ütközik hello **ResourceGroupName** hello paraméterének [ Új AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) parancsmag. A telepítés során áll felszólító tooprovide értéket **ResourceGroupNameFromTemplate**. Ez zavart ne általában a nem azonos nevet telepítési műveleteihez használt paraméterekként hello paramétereket.

> [!NOTE]
> Jelszavak, kulcsokat és más titkos adatokat kell használni a hello **secureString** típusa. Ha JSON-objektum átadni a bizalmas adatokat, használja a hello **secureObject** típusa. Sablonparaméterek secureString vagy secureObject típusú erőforrások telepítése után nem lehet olvasni. 
> 
> Például hello hello üzembe helyezési előzményeket a következő bejegyzés mutatja egy karakterláncot és objektumot, de nem a secureString és secureObject hello értékkel.
>
> ![központi telepítés értékek megjelenítése](./media/resource-group-authoring-templates/show-parameters.png)  
>

a következő példa azt mutatja meg hogyan hello toodefine paraméterek:

```json
"parameters": {
    "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
    },
    "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
    },
    "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
    },
    "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
    }
}
```

Hogyan tooinput hello paraméter értékei üzembe helyezése során, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md). 

## <a name="variables"></a>Változók
Hello a változók szakaszban, a sablon egész érték, amely használható hozhat létre. Nem kell toodefine változók, de azok gyakran leegyszerűsítheti a sablon csökkentésével összetett kifejezések.

A következő struktúra hello adja meg a változókat:

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    }
}
```

a következő példa azt mutatja meg hogyan hello toodefine paraméter két érték közül összeállított változó:

```json
"variables": {
    "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
}
```

hello következő példában egy változó, amely egy összetett JSON-típus, és a változókat, amelyek más változók össze:

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
"variables": {
    "environmentSettings": {
        "test": {
            "instancesSize": "Small",
            "instancesCount": 1
        },
        "prod": {
            "instancesSize": "Large",
            "instancesCount": 4
        }
    },
    "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
    "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
    "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
}
```

## <a name="resources"></a>Erőforrások
Hello erőforrások szakaszában vannak telepítve, vagy frissített hello erőforrások határozza meg. Ez a szakasz kérheti le bonyolult, mert ismernie kell a hello meg kell adnia tett tooprovide hello megfelelő értékeket. Tekintse meg a hello erőforrás-specifikus értékeket (apiVersion, típusa és tulajdonságait), hogy kell-e tooset [meghatározhatja az erőforrásokat az Azure Resource Manager sablonokban](/azure/templates/). 

Erőforrások a következő struktúra hello határozhat meg:

```json
"resources": [
  {
      "condition": "<boolean-value-whether-to-deploy>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": "<number-of-iterations>",
          "mode": "<serial-or-parallel>",
          "batchSize": "<number-to-deploy-serially>"
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Elem neve | Szükséges | Leírás |
|:--- |:--- |:--- |
| Az állapot | Nem | Logikai érték, amely azt jelzi, hogy telepítve van-e a hello erőforrás. |
| apiVersion |Igen |Hello REST API toouse hello erőforrás létrehozásához verzióját. |
| type |Igen |Hello erőforrás típusát. Az értéket nem hello névtér hello erőforrás-szolgáltató és hello erőforrástípus kombinációja (például **Microsoft.Storage/storageAccounts**). |
| név |Igen |Hello erőforrás neve. hello neve URI összetevő korlátozások RFC3986 definiált kell követnie. Emellett Azure-szolgáltatások hello erőforrás neve toooutside felek visszaállítását érvényesítése hello neve toomake rá, hogy ez nem egy kísérlet toospoof másik identitást. |
| location |Változó |A megadott erőforrás hello támogatott földrajzi-helyét. Kiválaszthatja hello elérhető helyek valamelyikén, de általában teszi logika toopick tartalmazó Bezárás tooyour felhasználók. Általában is érdemes tooplace erőforrások adatcserét a hello azonos régióban. A legtöbb erőforrás szükséges egy helyre, de néhány típust (például egy szerepkör-hozzárendelés) igényel egy helyre. Lásd: [erőforrás hely beállítása az Azure Resource Manager sablonokban](resource-manager-template-location.md). |
| tags |Nem |Hello erőforrás társított címkékkel. Lásd: [címkével olyan erőforrásokat az Azure Resource Manager sablonokban](resource-manager-template-tags.md). |
| Megjegyzések |Nem |A Megjegyzések a hello erőforrások a sablonban dokumentálása |
| Másolás |Nem |Ha egynél több példány van szükség, hello erőforrások toocreate száma. hello alapértelmezett módja párhuzamos. Adja meg a soros módban, ha nem szeretné, hogy az összes vagy erőforrások toodeploy hello: hello ugyanannyi időt vesz igénybe. További információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Nem |Ehhez az erőforráshoz központi telepítése előtt telepíteni kell erőforrások. Erőforrás-kezelő erőforrások közti függőségeket hello kiértékeli, és telepíti azokat a megfelelő sorrendben hello. Ha nincsenek függő erőforrások, párhuzamos központi telepítés. hello érték lehet egy vesszővel elválasztott lista erőforrás nevét vagy egyedi erőforrás-azonosítók. Ez a sablon üzembe helyezett erőforrások csak felsorolása Erőforrások, amelyek nincsenek meghatározva a sablonban már léteznie kell. Kerülje a szükségtelen függőségek hozzáadásával még a központi telepítés lassú, és hozzon létre körkörös függőségi viszony. A beállítás függőségek útmutatást lásd: [függőségek meghatározása az Azure Resource Manager-sablonok](resource-group-define-dependencies.md). |
| properties |Nem |Erőforrás-specifikus konfigurációs beállításokat. hello tulajdonságok értékeinek hello vannak hello azonos hello REST API művelet (PUT metódust) toocreate hello erőforrás hello kérés törzsében meg hello értékként. A másolási tömb toocreate tulajdonság több példányát is megadhatja. További információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md). |
| Erőforrások |Nem |Múltbeli hello erőforrástól függő gyermekszintű erőforrása. Csak olyan típusú erőforrások hello séma hello szülő erőforrás által számukra engedélyezett. hello teljesen minősített típusú hello gyermek erőforrás tartalmaz hello szülő erőforrástípusra, például **Microsoft.Web/sites/extensions**. Hello szülő erőforrás függőség nem utal. Függőséget explicit módon meg kell adni. |

hello források szakaszában hello erőforrások toodeploy tömbjét tartalmazza. Az egyes erőforrások belül is meghatározhat gyermekerőforrásait tömbjét. Ezért a források szakaszában eredményezhet. a struktúra, például:

```json
"resources": [
  {
      "name": "resourceA",
  },
  {
      "name": "resourceB",
      "resources": [
        {
            "name": "firstChildResourceB",
        },
        {   
            "name": "secondChildResourceB",
        }
      ]
  },
  {
      "name": "resourceC",
  }
]
```      

Gyermek erőforrások meghatározása kapcsolatos további információkért lásd: [Resource Manager-sablon nevét és típusát gyermek erőforrás beállított](resource-manager-template-child-resource.md).

Hello **feltétel** elem határozza meg, hogy telepítve van-e a hello erőforrás. hello érték ehhez az elemhez oldja fel a tootrue vagy HAMIS eredményt ad. Például toospecify hogy telepít egy új tárfiókot, a rendszer használni:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Például egy új vagy meglévő erőforrást használ, tekintse meg a [új vagy meglévő feltétel sablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).

toospecify egy virtuális gép telepítve van a jelszóval vagy SSH-kulcsban, hogy adjon meg hello virtuális gép két verziója a sablon és -felhasználási **feltétel** toodifferentiate használat. A paraméter meghatározza, hogy mely a forgatókönyv toodeploy továbbítja.

```json
{
    "condition": "[equals(parameters('passwordOrSshKey'),'password')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'password')]",
    "properties": {
        "osProfile": {
            "computerName": "[variables('vmName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
        },
        ...
    },
    ...
},
{
    "condition": "[equals(parameters('passwordOrSshKey'),'sshKey')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'ssh')]",
    "properties": {
        "osProfile": {
            "linuxConfiguration": {
                "disablePasswordAuthentication": "true",
                "ssh": {
                    "publicKeys": [
                        {
                            "path": "[variables('sshKeyPath')]",
                            "keyData": "[parameters('adminSshKey')]"
                        }
                    ]
                }
            }
        },
        ...
    },
    ...
}
``` 

Például a jelszóval vagy SSH-kulcs toodeploy virtuális gép, [felhasználónév és SSH feltétel sablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="outputs"></a>kimenetek
Hello kimenetek szakaszban adja meg központi telepítéséből a visszaadott érték. Például visszaadhatja hello URI tooaccess üzembe helyezett erőforrás.

hello alábbi példa bemutatja egy kimeneti definíciója hello szerkezete:

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| Elem neve | Szükséges | Leírás |
|:--- |:--- |:--- |
| outputName |Igen |Hello kimeneti érték nevét. Egy érvényes JavaScript-azonosítónak kell lennie. |
| type |Igen |Hello kimeneti érték típusa. Kimeneti értékek hello azonos sablon bemeneti paraméterként típusokat támogatja. |
| érték |Igen |Amely értékeli ki, és kimeneti értéket adja vissza a sablonnyelvi kifejezés. |

hello következő példa bemutatja a hello kimenetek szakaszban visszaadott érték.

```json
"outputs": {
    "siteUri" : {
        "type" : "string",
        "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
    }
}
```

A kimeneti használatáról további információk: [állapotát az Azure Resource Manager sablonokban megosztása](best-practices-resource-manager-state.md).

## <a name="template-limits"></a>Sablon korlátok

Hello méretének korlátozása a a sablon too1 MB, és minden paraméter fájl too64 KB. hello 1 MB-os korlát vonatkozik toohello végső állapot hello sablon, miután kiterjedt ismétlődő erőforrás-definíciókban és változók és a paraméterek értékeit. 

Emellett korlátozva:

* 256 paraméterek
* 256 változók
* 800 erőforrások (például a példányszám)
* 64 kimeneti értékek
* egy sablon kifejezés 24,576 karakter

Néhány sablon korlátot azért lépheti túl a beágyazott sablon használatával. További információkért lásd: [kapcsolt sablonok használata az Azure-erőforrások telepítésekor](resource-group-linked-templates.md). tooreduce hello száma paramétereket, változók vagy kimenetek, kombinálható több értéket egy objektumot. További információkért lásd: [paraméterekként objektumok](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Következő lépések
* tooview teljes sablonok számos különböző típusú megoldások, lásd: hello [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/).
* Használhatja a sablonon belül a hello functions szolgáltatással kapcsolatos részletekért lásd: [Azure Resource Manager Sablonfüggvényei](resource-group-template-functions.md).
* toocombine több sablon telepítése során lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).
* Szükség lehet toouse belül egy másik erőforráscsoportban található erőforráshoz. Ez a forgatókönyv nem közös, ha a storage-fiókok vagy több erőforrás csoporttal megosztott virtuális hálózatok. További információkért lásd: hello [resourceId függvény](resource-group-template-functions-resource.md#resourceid).
