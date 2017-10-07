---
title: "az Azure-erőforrások aaaSet deploymentorder |} Microsoft Docs"
description: "Ismerteti, hogyan tooset egy erőforrást, erőforrástól függ másik során telepítési tooensure erőforrásai hello megfelelő sorrendben legyenek telepítve."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 34ebaf1e-480c-4b4d-9bf6-251bd3f8f2cf
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: tomfitz
ms.openlocfilehash: 2f658f4c85236966c46b34a65aafb8426c92806c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="define-hello-order-for-deploying-resources-in-azure-resource-manager-templates"></a>Hello ahhoz, hogy az Azure Resource Manager sablonokban erőforrásokat üzembe helyezi meghatározása
A megadott erőforrás lehet más erőforrások, amelyek már léteznie kell hello erőforrás van telepítve. Például egy SQL server léteznie kell egy SQL-adatbázis toodeploy megkísérlése előtt. Megadhatja a kapcsolat hello függenek, egy erőforrás megjelölésével egyéb erőforrásokat. Megadhatja a hello függőség **dependsOn** elem, vagy hello segítségével **hivatkozás** függvény. 

Erőforrás-kezelő erőforrások közti függőségeket hello kiértékeli, és telepíti azokat a függő sorrendben. Ha nincsenek függő erőforrások, erőforrás-kezelő telepíti azokat párhuzamosan. Elegendő toodefine függőségek hello üzembe helyezett erőforrások ugyanazt a sablont. 

## <a name="dependson"></a>dependsOn
A sablonon belül hello dependsOn elem lehetővé teszi toodefine egy erőforrást, egy vagy több erőforrást egy függ. Az érték lehet erőforrásnevek vesszővel tagolt listája. 

hello következő példa bemutatja a virtuálisgép-méretezési csoport, amely egy adott terheléselosztóhoz, a virtuális hálózati és a több tárfiókot létrehozó hurkot függ. Ezeket az erőforrásokat a következő példa hello nem láthatók, de tooexist máshol hello sablonban kell.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "[variables('namingInfix')]",
  "location": "[variables('location')]",
  "apiVersion": "2016-03-30",
  "tags": {
    "displayName": "VMScaleSet"
  },
  "dependsOn": [
    "[variables('loadBalancerName')]",
    "[variables('virtualNetworkName')]",
    "storageLoop",
  ],
  ...
}
```

A fenti példa hello, egy függőségi szerepel-e a másolási ciklust nevű keresztül létrehozott hello erőforrások **storageLoop**. Egy vonatkozó példáért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).

Függőségek meghatározásakor hello erőforrás szolgáltató névtér és az erőforrás típusa tooavoid kétértelműség is megadhat. Például tooclarify egy terheléselosztó és a virtuális hálózat, amelyek azonos nevek egyéb erőforrásokat, a következő használatát hello hello formázása:

```json
"dependsOn": [
  "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
  "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
]
``` 

Ferde toouse dependsOn toomap kapcsolatok lehet, ha az erőforrások között, de napjainkban fontos toounderstand miért végzett azt. Például, milyen erőforrásokat kötik, toodocument dependsOn nincs hello megfelelő módszert. Nem lehet lekérdezni, mely erőforrásokhoz a telepítés utáni hello dependsOn elemben definiált. DependsOn tulajdonság használatával, potenciálisan hatással van az telepítési idő mert erőforrás-kezelő nem telepíti a függőségi viszonyban párhuzamos két erőforrásokat. erőforrások toodocument kapcsolatai inkább [erőforrás linking](/rest/api/resources/resourcelinks).

## <a name="child-resources"></a>Gyermek-erőforrások
hello erőforrások tulajdonság teszi lehetővé, amelyek a múltbeli kapcsolódó toohello erőforrás toospecify gyermekerőforrásait. Gyermek erőforrások csak meghatározott öt szintnél mélyebb lehet. Fontos toonote, amely egy implicit függőség nem hoz létre egy gyermek és hello szülő típusú erőforrást között. Gyermek-erőforrás toobe hello szülő erőforrás után kell hello, hello dependsOn tulajdonság függőséget explicit módon meg kell adni. 

Minden szülő erőforrás csak bizonyos típusú erőforrások gyermekerőforrásait fogad el. hello elfogadott erőforrástípusok esetében meg van határozva a hello [sablonséma](https://github.com/Azure/azure-resource-manager-schemas) hello szülő erőforrás. gyermek erőforrástípus neve hello tartalmaz hello neve hello szülő erőforrástípusra, például **Microsoft.Web/sites/config** és **Microsoft.Web/sites/extensions** mindkét hello agyermek-erőforrás **Microsoft.Web/sites**.

a következő példa hello jeleníti meg, egy SQL server és SQL-adatbázis. Figyelje meg, hogy egy explicit függőség hello SQL database és SQL server között van-e definiálva annak ellenére, hogy hello adatbázis hello server gyermeke.

```json
"resources": [
  {
    "name": "[variables('sqlserverName')]",
    "type": "Microsoft.Sql/servers",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "SqlServer"
    },
    "apiVersion": "2014-04-01-preview",
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "name": "[parameters('databaseName')]",
        "type": "databases",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "Database"
        },
        "apiVersion": "2014-04-01-preview",
        "dependsOn": [
          "[variables('sqlserverName')]"
        ],
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
        }
      }
    ]
  }
]
```

## <a name="reference-function"></a>hivatkozás függvény
Hello [függvényre](resource-group-template-functions-resource.md#reference) lehetővé teszi egy kifejezés tooderive más JSON név-érték párok vagy futásidejű erőforrások az értékét. Hivatkozási kifejezések implicit módon deklarálja, hogy egy erőforrást egy másik függ. hello általános formátuma:

```json
reference('resourceName').propertyPath
```

A következő példa hello a CDN-végpontok explicit módon függ hello CDN-profilt, és a implicit módon függ a webes alkalmazás.

```json
{
    "name": "[variables('endpointName')]",
    "type": "endpoints",
    "location": "[resourceGroup().location]",
    "apiVersion": "2016-04-02",
    "dependsOn": [
            "[variables('profileName')]"
    ],
    "properties": {
        "originHostHeader": "[reference(variables('webAppName')).hostNames[0]]",
        ...
    }
```

Ezen elem vagy a hello dependsOn elem toospecify függőségeket is használhat, de nem kell mindkét toouse hello az azonos függő erőforrást. Amikor csak lehetséges, használjon egy implicit hivatkozási tooavoid szükségtelen-függőség felvétele.

több, lásd: toolearn [függvényre](resource-group-template-functions-resource.md#reference).

## <a name="recommendations-for-setting-dependencies"></a>Javaslatok függőségek beállítása

Amikor eldönti, milyen függőségek tooset, használja a következő irányelveket hello:

* A lehető legkevesebb függőségek beállítása.
* Állítsa be a gyermek-erőforrás a szülő erőforrás függ.
* Használjon hello **hivatkozás** tooset tooshare tulajdonság szükséges erőforrások közötti implicit függőségek működik. Ne vegyen fel egy explicit függőség (**dependsOn**) Ha már megadta egy implicit függőség. Ez a megközelítés hello kockázatot szükségtelen függőségek csökkenti. 
* Amikor az erőforrás nem lehet a függőség beállítása **létrehozott** nélkül egy másik erőforrás funkciói. Ne állítson be egy függőséget hello erőforrások csak interaktív telepítés után.
* Lehetővé függőségek cascade explicit módon beállítás nélkül. Például a virtuális gép virtuális hálózati adapteren, valamint hello virtuális hálózati adapter függ a virtuális hálózat és a nyilvános IP-címeket. Ezért hello virtuális gép telepített összes három erőforrások, de nincs explicit módon beállítva az hello virtuális gép összes három erőforrástól függ. Ez a megközelítés hello függőségi sorrend tisztázza, így könnyebben toochange hello sablon később.
* Ha a telepítés előtt értéket lehet meghatározni, próbálja hello erőforrás függőség nélküli telepítése. Például ha egy konfigurációs értéke egy másik erőforrás hello nevét, nem szükség lehet egy függőséget. Ez az útmutató nem mindig működik, mert egyes erőforrásokat hello hello létezésének ellenőrzése egyéb erőforrásokat. Ha hibaüzenetet kap, hozzáadjon egy függőséget. 

Erőforrás-kezelő körkörös függőségi viszony azonosítja a sablon érvényesítése során. Ha hibaüzenet jelenik meg, amely meghatározza, hogy, hogy létezik-e körkörös függőséget, a sablon toosee kiértékelheti, hogy bármely függőségek esetén nem szükséges, és távolíthatja el. Ha függőségek nem segít, elkerülhető körkörös függőségi viszony, néhány telepítési művelet áthelyezése gyermekszintű erőforrása, amely után hello erőforrásokat, amelyek hello körkörös függőségi viszonyban vannak telepítve. Tegyük fel például, két olyan virtuális gépet telepít, de a tulajdonságok az egyes más toohello hivatkozó meg kell adni. A sorrend hello telepíthetné őket:

1. vm1
2. vm2 virtuális gépnek
3. Vm1 kiterjesztés vm1 és vm2 virtuális gépnek függ. hello bővítmény, amely azt lekérése vm2 virtuális gépnek vm1 értékek beállítása.
4. Bővítmény vm2 virtuális gépnek a vm1 és vm2 virtuális gépnek függ. hello bővítmény értékek beállítása, amely azt lekérése vm1 vm2 virtuális gépnek.

Felmérése hello telepítési sorrendje: és függőségi hibák feloldására vonatkozó információkért lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="next-steps"></a>Következő lépések
* a telepítés során függőségek hibaelhárítással kapcsolatos toolearn lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).
* toolearn, Azure Resource Manager-sablonok létrehozásával kapcsolatban lásd: [sablonok készítése](resource-group-authoring-templates.md). 
* Hello elérhető funkciók egy listáját lásd: [sablonfüggvényei](resource-group-template-functions.md).

