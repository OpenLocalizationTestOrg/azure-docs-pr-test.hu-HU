---
title: "aaaMultiple IP-címek az Azure virtuális gépek - sablon |} Microsoft Docs"
description: "Ismerje meg, hogyan tooassign több IP-címek tooa virtuális gépet az Azure Resource Manager-sablon használatával."
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/08/2016
ms.author: jdial
ms.openlocfilehash: e7660257b2d5c7da4b8b86771abe51a2c5012fa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-an-azure-resource-manager-template"></a>Több IP-cím hozzárendelése toovirtual gépeket Azure Resource Manager-sablonnal

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Ez a cikk azt ismerteti, hogyan toocreate egy virtuális gép (VM) keresztül hello Azure Resource Manager telepítési modell Resource Manager-sablon használatával. Több nyilvános és magánhálózati IP-cím nem rendelhető hozzá toohello azonos hálózati hello klasszikus telepítési modell használatával egy virtuális gép telepítésekor. További információk az Azure üzembe helyezési modellel, olvassa el a hello toolearn [üzembe helyezési modellel megértéséhez](../resource-manager-deployment-model.md) cikk.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name="template-description"></a>Sablon leírása

A sablonok telepítésével lehetővé teszi a tooquickly, és következetesen hozzon létre Azure-erőforrások különféle konfigurációs értékeket. Olvasási hello [Resource Manager sablonokhoz](../azure-resource-manager/resource-manager-template-walkthrough.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a következő cikket: Ha még nem ismeri a Azure Resource Manager-sablonok. Hello [több IP-címekkel rendelkező olyan virtuális gép telepítéséhez](https://azure.microsoft.com/resources/templates/101-vm-multiple-ipconfig) sablon használata ebben a cikkben.

<a name="resources"></a>Központi telepítési hello-sablon létrehozza a következő erőforrások hello:

|Erőforrás|Név|Leírás|
|---|---|---|
|Hálózati illesztő|*myNic1*|hello hello forgatókönyv című cikkben ismertetett három IP-konfigurációk létrehozása és hozzárendelése toothis hálózati adaptert.|
|Nyilvános IP-cím erőforrás|2 jönnek létre: *myPublicIP* és *myPublicIP2*|Ezeket az erőforrásokat statikus nyilvános IP-címek és rendelt toohello *IPConfig-1* és *IPConfig-2* hello forgatókönyvben bemutatott IP-konfigurációk.|
|VM|*myVM1*|Egy szabványos DS3 virtuális gép.|
|Virtuális hálózat|*myVNet1*|Egy virtuális hálózatot egyetlen alhálózattal nevű *mySubnet*.|
|Tárfiók|Egyedi toohello központi telepítés|A storage-fiók.|

<a name="parameters"></a>Hello sablon telepítésekor meg kell adnia a következő paraméterek hello értékeit:

|Név|Leírás|
|---|---|
|adminUsername|Rendszergazda felhasználóneve. hello felhasználónévnek meg kell felelnie [Azure username követelmények](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|
|adminPassword|Rendszergazdai jelszó hello jelszavát meg kell felelnie [Azure jelszavakra vonatkozó követelmények](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
|dnsLabelPrefix|PublicIPAddressName1 DNS-nevét. hello DNS-név hello nyilvános IP-címtartományból toohello VM tooone fel. hello nevének egyedinek kell lennie belül hello Azure régióban (hely) hoz létre a virtuális gép hello.|
|dnsLabelPrefix1|PublicIPAddressName2 DNS-nevét. hello DNS-név hello nyilvános IP-címtartományból toohello VM tooone fel. hello nevének egyedinek kell lennie belül hello Azure régióban (hely) hoz létre a virtuális gép hello.|
|OSVersion|hello Windows/Linux hello virtuális gép verziója. hello operációs rendszer megadott kiválasztott windowsos/Linuxos verzióját hello teljesen javított képe.|
|imagePublisher|hello Windows/Linux kép közzétevőjének hello kijelölt virtuális gép.|
|imageOffer|hello Windows/Linux kép hello kijelölt virtuális gép.|

Egyes hello sablon hello forrásokkal több alapértelmezett beállításokkal van konfigurálva. Ezeket a beállításokat, vagy a következő módszerek hello keresztül tekintheti meg:

- **Hello sablon megtekintése a Githubon:** Ha jártas a sablonok, megtekintheti a hello beállítások belül hello [sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json).
- **Telepítése után hello beállítások megtekintéséhez:** Ha még nem ismeri a sablonok, a következő szakaszok hello egyikében lépéseket követve hello sablon üzembe helyezése, és nézze meg a telepítés utáni hello-beállítások.

Hello Azure-portálon, a PowerShell vagy a hello Azure parancssori felület (CLI) toodeploy hello sablon használható. Összes módszert előállításához hello ugyanazt az eredményt. toodeploy hello sablon, teljes hello lépéseit a következő szakaszok hello egyikét:

## <a name="deploy-using-hello-azure-portal"></a>Szabályzatsablonokat hello Azure-portálon

toodeploy hello sablon használatával hello Azure-portálon teljes hello a következő lépéseket:

1. Ha szükséges, módosítsa a hello sablont. hello sablon telepíti hello erőforrások és hello felsorolt beállítások [erőforrások](#resources) című szakaszát. sablonokkal kapcsolatos további toolearn és hogyan tooauthor őket, olvassa el hello [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-network%2ftoc.json)cikk.
2. Hello sablon üzembe helyezése hello a következő módszerek egyikével:
    - **Jelölje be hello sablon hello portálon:** teljes hello hello szükséges lépések [egyéni sablont az erőforrások telepítése](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-resources-from-custom-template) cikk. Válassza ki a hello korábban létrehozott sablont nevű *101-vm – több-ipconfig*.
    - **Közvetlenül:** kattintson a következő gombra tooopen hello sablon közvetlenül a hello portál hello:<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-vm-multiple-ipconfig%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

Függetlenül hello módszert választja, szüksége lesz toosupply értékek hello [paraméterek](#parameters) ebben a cikkben korábban felsorolt. Hello VM telepítése után csatlakoztassa a virtuális gép toohello és hello titkos IP-címek toohello operációs rendszere hello elvégzésével telepített hello szükséges lépések hozzáadása [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát. Hello nyilvános IP-címek toohello operációs rendszere nem adja hozzá.

## <a name="deploy-using-powershell"></a>Telepítheti a PowerShell használatával

toodeploy hello sablon PowerShell, a következő lépéseket teljes hello használata:

1. Telepítés hello sablon hello lépések végrehajtásával a hello [a PowerShell-lel egy sablon telepítésének](../azure-resource-manager/resource-group-template-deploy-cli.md) cikk. a cikk hello többféle módon a sablonok telepítésével. Ha úgy dönt, hogy hello segítségével toodeploy `-TemplateUri parameter`, hello URI esetében ez a sablon *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Ha úgy dönt, hogy hello segítségével toodeploy `-TemplateFile` paraméter, a Másolás hello tartalmát hello [sablonfájl](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) a Githubból a számítógépen egy új fájlba. Ha szükséges, módosítsa a sablon tartalmát hello. hello sablon telepíti hello erőforrások és hello felsorolt beállítások [erőforrások](#resources) című szakaszát. sablonokkal kapcsolatos további toolearn és hogyan tooauthor őket, olvassa el hello [Azure Resource Manager-sablonok készítése ](../azure-resource-manager/resource-group-authoring-templates.md)cikk.

    Függetlenül hello lehetőséget toodeploy hello sablont választja, meg kell adnia hello paraméter értékei hello felsorolt [paraméterek](#parameters) című szakaszát. Ha úgy dönt, hogy a paraméterek fájllal toosupply paraméterek, hello hello tartalom másolása [paraméterfájl](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) a Githubból a számítógépen egy új fájlba. Hello fájl hello értékeit módosíthatja. Hello hello értékként létrehozott fájlokat használjon hello `-TemplateParameterFile` paraméter.

    érvényes értékek toodetermine hello OSVersion, ImagePublisher és imageOffer paraméterek, teljes hello szükséges lépések hello [keresse meg és jelölje be a Windows virtuális gép képek cikk](../virtual-machines/windows/cli-ps-findimage.md) cikk.

    >[!TIP]
    >Ha nem biztos abban, hogy rendelkezésre áll-e egy dnslabelprefix, adja meg a hello `Test-AzureRmDnsAvailability -DomainNameLabel <name-you-want-to-use> -Location <location>` parancs toofind ki. Hello parancs fog visszaadni esetén érhető el, `True`.

2. Hello VM telepítése után csatlakoztassa a virtuális gép toohello és hello titkos IP-címek toohello operációs rendszere hello elvégzésével telepített hello szükséges lépések hozzáadása [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát. Hello nyilvános IP-címek toohello operációs rendszere nem adja hozzá.

## <a name="deploy-using-hello-azure-cli"></a>Telepítheti a hello Azure parancssori felület használatával

toodeploy hello sablon Azure CLI 1.0-s teljes hello lépések hello használata:

1. Telepítés hello sablon hello lépések végrehajtásával a hello [hello Azure parancssori felület a sablon telepítése](../azure-resource-manager/resource-group-template-deploy-cli.md) cikk. a cikk hello hello sablon telepítéséhez több beállítások. Ha úgy dönt, hogy hello segítségével toodeploy `--template-uri` (-f), hello URI esetében ez a sablon *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Ha úgy dönt, hogy hello segítségével toodeploy `--template-file` (-f) paraméter, a Másolás hello tartalmát hello [sablonfájl](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) a Githubból a számítógépen egy új fájlba. Ha szükséges, módosítsa a sablon tartalmát hello. hello sablon telepíti hello erőforrások és hello felsorolt beállítások [erőforrások](#resources) című szakaszát. sablonokkal kapcsolatos további toolearn és hogyan tooauthor őket, olvassa el hello [Azure Resource Manager-sablonok készítése ](../azure-resource-manager/resource-group-authoring-templates.md)cikk.

    Függetlenül hello lehetőséget toodeploy hello sablont választja, meg kell adnia hello paraméter értékei hello felsorolt [paraméterek](#parameters) című szakaszát. Ha úgy dönt, hogy a paraméterek fájllal toosupply paraméterek, hello hello tartalom másolása [paraméterfájl](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) a Githubból a számítógépen egy új fájlba. Hello fájl hello értékeit módosíthatja. Hello hello értékként létrehozott fájlokat használjon hello `--parameters-file` (-e) paraméter.

    érvényes értékek toodetermine hello OSVersion, ImagePublisher és imageOffer paraméterek, teljes hello szükséges lépések hello [keresse meg és jelölje be a Windows virtuális gép képek cikk](../virtual-machines/windows/cli-ps-findimage.md) cikk.

2. Hello VM telepítése után csatlakoztassa a virtuális gép toohello és hello titkos IP-címek toohello operációs rendszere hello elvégzésével telepített hello szükséges lépések hozzáadása [hozzáadása IP-címek tooa virtuális gép operációs rendszer](#os-config) című szakaszát. Hello nyilvános IP-címek toohello operációs rendszere nem adja hozzá.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
