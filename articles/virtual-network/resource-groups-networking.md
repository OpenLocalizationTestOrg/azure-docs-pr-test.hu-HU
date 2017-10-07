---
title: "Erőforrás-szolgáltató áttekintés aaaNetwork |} Microsoft Docs"
description: "További tudnivalók: hello új hálózati erőforrás-szolgáltató az Azure Resource Manager"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 79bf09da-4809-45cb-8d21-705616ef24dc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 81b8f51fe8ee180d8f7885c6e04eb953904d7be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="network-resource-provider"></a>Hálózatierőforrás-szolgáltató
Egy megerősítő kell az aktuális üzleti sikeres, a lehetősége toobuild hello és nagy méretű hálózati alkalmazások kezelése a gyors, rugalmas, biztonságos és ismételhető módon. Az Azure Resource Manager lehetővé teszi, hogy Ön toocreate ilyen alkalmazások, mint az erőforrások erőforráscsoportok egyetlen gyűjtemény. Ilyen erőforrások kezelése különböző erőforrás-szolgáltató az erőforrás-kezelő használatával.

Az Azure Resource Manager támaszkodik a különböző erőforrás szolgáltatók tooprovide tooyour erőforrások eléréséhez. Nincsenek három fő erőforrás-szolgáltatókon: hálózati, tárolási és számítási. Ez a dokumentum ismerteti a hello jellemzőit és a hálózatierőforrás-szolgáltatónál, hello előnyei többek között:

* **Metaadatok** – információk tooresources címkék használatával adhat hozzá. Ezekkel a címkékkel használt tootrack erőforrás-használat lehet erőforráscsoport-sablonok és előfizetések között.
* **Hatékonyabb vezérlését a hálózati** - hálózati erőforrások lazán, és részletesebb módon szabályozhatja. Ez azt jelenti, hogy nagyobb rugalmasságot biztosítanak a hello hálózati erőforrások kezelése.
* **Gyorsabb konfigurációs** -hálózati erőforrások vannak lazán, mert hozhat létre és levezényelni a hálózati erőforrások párhuzamosan. Ez jelentősen lecsökkentette konfigurációs idő.
* **Szerepköralapú hozzáférés-vezérlés** -RBAC biztosít az alapértelmezett szerepköröket, meghatározott biztonsági hatókörrel, a biztonságos felügyeleti egyéni szerepkörök hozzáadása tooallowing hello létrehozása.
* **Egyszerűbb kezelés és a telepítés** -könnyebb toodeploy, és kezelheti az alkalmazásokat, mivel is létrehozhat egész alkalmazáscsoportokat erőforráscsoport erőforrásainak egyetlen gyűjteményeként. És gyorsabb toodeploy, mivel, egyszerűen adja meg a sablon JSON-adattartalmat telepítése.
* **Gyors testreszabási** -stílusú deklaratív sablonok tooenable megismételhető és gyors központi telepítés testreszabási is használhatja.
* **A testreszabáshoz megismételhető** -stílusú deklaratív sablonok tooenable megismételhető és gyors központi telepítés testreszabási is használhatja.
* **Felügyeleti felületek** -az erőforrások is használja a következő illesztők toomanage hello valamelyikét:
  * REST-alapú API
  * PowerShell
  * .NET SDK
  * Node.JS SDK
  * Java SDK
  * Azure CLI
  * Betekintő portál
  * Erőforrás-kezelő sablon nyelve

## <a name="network-resources"></a>Hálózati erőforrások
Most függetlenül is felügyelhetők hálózati erőforrások, így ahelyett hogy azokat egyetlen számítási erőforrás (virtuális gép) kezelhetők. Ez biztosítja, hogy nagyobb fokú rugalmasságot és agilitást az erőforráscsoport egy összetett és nagy méretű méretezési infrastruktúra létrehozása.

Egy üzembe helyezési minta egy többrétegű alkalmazást magában foglaló konceptuális ábrázolása az alábbi táblázat mutatja. Az egyes erőforrások jelenik meg, például a hálózati adapterek, a nyilvános IP-címek és a virtuális gépek, egymástól függetlenül is felügyelhetők.

![Hálózati erőforrás-modellje](./media/resource-groups-networking/Figure2.png)

Minden erőforrás azokat egy közös tulajdonságokat, és az egyéni tulajdonság. hello közös tulajdonságai a következők:

| Tulajdonság | Leírás | Példaértékek |
| --- | --- | --- |
| **név** |Egyedi erőforrás neve. Minden erőforrás saját vonatkozó elnevezési korlátozás rendelkezik. |PIP01, VM01, NIC01 |
| **hely** |Az Azure-régió, mely hello erőforrás található |westus, eastus |
| **azonosítója** |Egyedi alapú URI-azonosító |következő<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP |

Ellenőrizheti az alábbi hello szakaszaiban található forrásanyagok hello egyes tulajdonságait.

[!INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[!INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[!INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[!INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[!INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[!INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[!INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[!INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[!INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[!INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Felügyeleti felületek
Az Azure-hálózati erőforrások különböző felületek segítségével kezelheti. Ebben a dokumentumban tárgyaljuk ezeket felületek fonókábel: REST API-t, és a sablonokat.

### <a name="rest-api"></a>REST API
A korábban említett csatolókat, beleértve a REST API-t, a .NET SDK, Node.JS SDK, Java SDK, PowerShell, CLI, Azure Portal és sablonok számos hálózati erőforrások is felügyelhetők.

hello Rest API toohello HTTP 1.1 protokoll specifikációja felel meg. hello általános URI szerkezete hello API alatt jelenik meg:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

És a kapcsos zárójelek hello paraméterek határoz meg a következő elemek hello:

* **előfizetés-azonosító** -Azure-előfizetése azonosítóját.
* **erőforrás-szolgáltató-névtér** -használt hello szolgáltató névterét. hello hello hálózati erőforrás-szolgáltató értéke *Microsoft.Network*.
* **terület-neve** -hello Azure-régiót neve

hello következő HTTP-metódus támogatottak, ha így hívások toohello REST API:

* **PUT** - használt toocreate egy adott típusú erőforrást egy erőforrás-tulajdonság módosítására, vagy módosítsa a erőforrások közötti társítást.
* **ELSŐ** -használt tooretrieve adatok kiosztott erőforrás.
* **Törlés** -toodelete meglévő erőforrás használja.

Hello kérelem és a válasz felel meg a tooa JSON-adattartalom formátuma. További részletekért lásd: [Azure erőforrás-kezelési API-k](https://msdn.microsoft.com/library/azure/dn948464.aspx).

### <a name="resource-manager-template-language"></a>Erőforrás-kezelő sablon nyelve
Továbbá toomanaging erőforrások imperatively (API-k vagy SDK) keresztül, a is használja a deklaratív programozási stílus toobuild és hálózati erőforrások kezelése hello Resource Manager sablon nyelv használatával.

A minta egy sablon megjelenítése alább –

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

hello sablon elsősorban JSON-leírásuk hello erőforrások és a paraméterek keresztül beszúrta hello példány értéke. az alábbi példa hello használt toocreate 2 alhálózatok virtuális hálózat lehet.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Lehetősége van hello biztosító hello paraméterértékek manuálisan, egy sablon használatakor, vagy egy paraméterfájl is használhat. hello az alábbi példában használt a fenti hello sablonnal paraméter értékek toobe lehetséges készlete:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


hello fő előnyei-sablonokkal a következők:

* Egy összetett infrastruktúra deklaratív Style erőforráscsoportban is létrehozható. hello hello erőforrások, például az függőségi felügyeleti létrehozásának orchestration, kezelését a kezelő által.
* hello infrastruktúra is létrehozható egy ismételhető módon különböző régiókban és régión belül paraméterek egyszerűen módosításával.
* hello deklaratív stílus tooshorter átfutási idő hello sablonok létrehozása és terítésével hello infrastruktúra vezet.

A minta-sablonok, lásd: [Azure gyors üzembe helyezési sablonokat](https://github.com/Azure/azure-quickstart-templates).

A Resource Manager sablon nyelvi hello további információkért lásd: [Azure Resource Manager sablon nyelvi](../azure-resource-manager/resource-group-authoring-templates.md).

a fenti hello mintasablon hello virtuális hálózati és alhálózati erőforrásokat használ. Nincsenek más hálózati erőforrásokhoz, használhatja az alábbiak szerint:

### <a name="using-a-template"></a>Egy sablon használatával
Szolgáltatások tooAzure sablonból powershellel, AzureCLI, vagy a Githubról egy kattintson toodeploy végrehajtásával telepítheti. a Githubon, sablonból toodeploy szolgáltatások hello lépésekkel hajtható végre:

1. Nyissa meg a hello template3 fájlt a Githubról. Tegyük fel, nyissa meg a [, két alhálózattal a virtuális hálózati](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Kattintson a **tooAzure telepítése**, majd jelentkezzen be a toohello Azure-portálon a hitelesítő adataival.
3. Ellenőrizze a hello sablont, és kattintson **mentése**.
4. Kattintson a **paraméterek szerkesztése** és adjon meg egy helyet, például a *USA nyugati régiója*, hello hálózatok és alhálózatok.
5. Szükség esetén módosítsa a hello **CÍMELŐTAGJA** és **SUBNETPREFIX** paramétereket, és kattintson **OK**.
6. Kattintson a **válasszon egy erőforráscsoportot** majd kattintson a kívánt tooadd hello hálózatok és alhálózatok hello erőforráscsoportot. Alternatív megoldásként létrehozhat egy új erőforráscsoportot kattintva **, vagy hozzon létre új**.
7. Kattintson a **Create** (Létrehozás) gombra. Figyelje meg, csempe megjelenítése hello **kiépítés sablon-üzembehelyezés**. Miután hello üzembe helyezés hasonló tooone az alábbi a képernyő jelenik meg.

![Sablon üzembe helyezési minta](./media/resource-groups-networking/Figure6.png)

## <a name="next-steps"></a>Következő lépések
[Az Azure Resource Manager sablon nyelve](../azure-resource-manager/resource-group-authoring-templates.md)

[Az Azure hálózatkezelés – általánosan használt sablon](https://github.com/Azure/azure-quickstart-templates)

[Az Azure Resource Manager és klasszikus üzembe helyezési összehasonlítása](../azure-resource-manager/resource-manager-deployment-model.md)

[Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md)

