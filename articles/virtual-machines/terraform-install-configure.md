---
title: "aaaInstall és Terraform tooprovision virtuális gépek és egyéb infrastruktúra konfigurálása az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall Terraform toocreate Azure konfigurálása és erőforrások"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.openlocfilehash: 803f51a6f5357417b96264ba713791408f9935b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-terraform-tooprovision-vms-and-other-infrastructure-into-azure"></a>Telepítse és konfigurálja a Terraform tooprovision virtuális gépek és egyéb infrastruktúrával az Azure 
Ez a cikk ismerteti a hello szükséges lépéseket tooinstall, és Terraform tooprovision erőforrások, például a virtuális gépek konfigurálása az Azure. Megtudhatja, hogyan toocreate és -felhasználási Azure hitelesítő adatok tooenable Terraform tooprovision felhőalapú erőforrások biztonságos elérését.

HashiCorp Terraform egy egyszerűen toodefine biztosít, és telepítheti a felhőalapú infrastruktúra HashiCorp konfigurációs nyelvi (Hardverkompatibilitási) nevű egyéni templating nyelv használatával. Az egyéni nyelv [toowrite egyszerűen és könnyen toounderstand](terraform-create-complete-vm.md). Továbbá használatával hello `terraform plan` parancs hello módosítások tooyour infrastruktúra jelenítheti meg előtt véglegesítse azokat. Kövesse a lépéseket toostart Terraform használata Azure.

## <a name="install-terraform"></a>Terraform telepítése
tooinstall Terraform, [letöltése](https://www.terraform.io/downloads.html) hello csomag megfelelő, az operációs rendszer egy önálló telepítés könyvtárba. hello egy egyetlen végrehajtható fájlt, amelynek meg kell is meg a globális elérési utat tartalmazó. Az utasításokat hogyan tooset hello Linux és Mac, elérési út túl lépjen[a weblap](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux). Az utasítások hogyan tooset hello elérési út a Windows, a go túl[a weblap](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows). tooverify a telepítés, hello `terraform` parancsot. Az elérhető Terraform listáját output típusúként kell megjelennie.

A következő lépésben tooallow Terraform hozzáférés tooyour Azure-előfizetés tooperform infrastruktúra kiépítése.

## <a name="set-up-terraform-access-tooazure"></a>Terraform hozzáférés tooAzure beállítása
Azure Active Directory (Azure AD) toocreate két entitása szüksége tooenable Terraform tooprovision erőforrások az Azure: egy Azure AD-alkalmazást és az Azure AD szolgáltatás egyszerű. Ezt követően ezeket az entitásokat azonosítók használhatja a Terraform parancsfájlokat. Egy egyszerű egy helyi példányát egy globális Azure AD-alkalmazást. Egy egyszerű szolgáltatás lehetővé teszi, hogy a vezérlő tooglobal erőforrások részletes helyi eléréséhez.

Számos módon toocreate van egy Azure AD-alkalmazást és az Azure AD szolgáltatás egyszerű. hello legegyszerűbben és leggyorsabban ma eltérő módon toouse Azure CLI 2.0, amely [, töltse le és telepítse a Windows, Linux és Mac](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). PowerShell vagy Azure CLI 1.0 toocreate hello szükséges biztonsági infrastruktúra is használhatja. hello utasításokban mutatja be az Azure-Terraform tooconfigure összes ezek a módszerek használatával.

### <a name="use-azure-cli-20-for-windows-linux-or-mac-users"></a>Azure CLI 2.0 használja (a Windows, Linux és Mac-felhasználók számára) 
Töltse le és telepítse hello után [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), jelentkezzen be tooadminister az Azure-előfizetéshez hello a következő parancs beírásával:

```
az login
```

>[!NOTE]
>Hello Kína, a Németországi Azure vagy az Azure Government felhők használja, ha szüksége van-e toofirst hello Azure CLI toowork konfigurálja, hogy a felhő. Ehhez futtassa a következő hello:

```
az cloud set --name AzureChinaCloud|AzureGermanCloud|AzureUSGovernment
```

Ha több Azure-előfizetéssel rendelkezik, azok adatai hello által visszaadott `az login` parancsot. Set hello `SUBSCRIPTION_ID` környezeti változó toohold hello értékének hello visszaadott `id` hello előfizetésből mező kívánt toouse. 

Állítsa be, amelyet az toouse ehhez a munkamenethez hello előfizetés.

```
az account set --subscription="${SUBSCRIPTION_ID}"
```

Hello fiók tooget hello előfizetés-azonosító lekérdezési, és a bérlői azonosító érték.

```
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Következő lépésként hozzon létre külön hitelesítő adatokat Terraform.

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

Visszaadja a appId, a jelszót, a sp_name és a bérlői. Jegyezze fel a hello appId és a jelszót.

tooconfirm a hitelesítő adatait (egyszerű szolgáltatásneve), nyissa meg az új rendszerhéjat, és futtassa a következő parancsok hello. Helyettesítő hello visszaadott sp_name, jelszót és bérlői értékeit:

```
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

### <a name="use-powershell-for-windows-users"></a>Használja a Powershellt (Windows-felhasználó) 
egy Windows toouse toowrite számítógéphez, és hajtsa végre a Terraform parancsfájlok és toouse PowerShell konfigurációs feladatok, a gép beállítása hello megfelelő PowerShell-eszközökkel. 

1. PowerShell-eszközök hello lépéseit követve telepítse [telepítse és konfigurálja az Azure Powershellt](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps). 

2. Letöltés és végrehajtás hello [azure-setup.ps1 parancsfájl](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) hello PowerShell-konzolon.

3. toorun hello azure-setup.ps1 parancsfájlban, töltse le, és hello végrehajtható `./azure-setup.ps1 setup` parancs hello PowerShell-konzolon. Majd jelentkezzen be rendszergazdai jogosultságokkal rendelkező Azure-előfizetés tooyour.

4. Adjon meg egy alkalmazásnevet (tetszőleges karakterlánc, kötelező) Ha a rendszer kéri. Ha szükséges adjon meg egy erős jelszót, amikor a rendszer kéri. Ha nem ad meg egy jelszót, egy erős jelszót hozza létre az Ön .NET biztonsági könyvtárak segítségével.

### <a name="use-azure-cli-10-for-linux-or-mac-users"></a>Azure CLI 1.0 használata (a Linux- vagy Mac-felhasználók számára)
tooget használatába Terraform Linux-gépek vagy a Mac számítógépekhez az Azure CLI 1.0-s telepítése hello megfelelő szalagtárak a számítógépen.  

1. Azure xPlat parancssori felület teszttelepítése hello lépéseit követve telepítse [Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli). 

2. Töltse le és telepítse a JSON processzor hello utasításait követve [jq letöltése](https://stedolan.github.io/jq/download/).

3. Letöltés és végrehajtás hello [azure-setup.sh parancsfájl](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash parancsfájl hello konzolról.

4. toorun hello azure-setup.sh parancsfájlban, töltse le, és hello végrehajtható `./azure-setup setup` hello konzol parancsot. Majd jelentkezzen be rendszergazdai jogosultságokkal rendelkező Azure-előfizetés tooyour.
 
5. Adjon meg egy alkalmazásnevet (tetszőleges karakterlánc, kötelező) Ha a rendszer kéri. Ha szükséges adjon meg egy erős jelszót, amikor a rendszer kéri. Ha nem ad meg egy jelszót, egy erős jelszót hozza létre az Ön .NET biztonsági könyvtárak segítségével.

Minden hello korábbi parancsfájlok hozzon létre egy Azure AD-alkalmazást és a szolgáltatás egyszerű. egyszerű hello szolgáltatás lekérdezi a közreműködői vagy a tulajdonos szintű hozzáférés hello az előfizetésben. Hozzáférést biztosít magas szintű hello, mert mindig a parancsfájlok által generált hello biztonsági információk védelme érdekében. Jegyezze fel az összes négy adatra biztonsági a parancsfájlok által biztosított: appId, jelszó ELŐFIZETÉS_AZONOSÍTÓJA és tenant_id.

## <a name="set-environment-variables"></a>Környezeti változók beállítása
Létrehozott, és konfigurálja az Azure AD szolgáltatás egyszerű, toolet kell Terraform tudja hello Bérlőazonosító, előfizetés-azonosító, ügyfél-Azonosítót és titkos toouse ügyfél. Ezt megteheti a Terraform parancsfájlokban ágyaz be ezeket az értékeket a [létrehozása alapvető infrastruktúra Terraform használatával](terraform-create-complete-vm.md). Másik lehetőségként beállíthatja a következő környezeti változók hello (és így elkerülhető a véletlenül ellenőrzés, vagy a hitelesítő adatok megosztása):

- ARM_SUBSCRIPTION_ID
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- ARM_TENANT_ID

Ez a minta rendszerhéj parancsfájl tooset ezeket a változókat használhatja:

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

Emellett Terraform vagy kínai, vagy Azure Government az Azure vagy a Németországi Azure használja, ha szüksége tooset hello környezeti változó megfelelően.

## <a name="next-steps"></a>Következő lépések
Ezzel Terraform telepített és konfigurált Azure hitelesítő adataival, hogy elindíthatja az Azure-előfizetéséhez olyan infrastruktúra üzembe. A következő megtudhatja, hogyan túl[hozzon létre infrastruktúra Terraform](terraform-create-complete-vm.md).
