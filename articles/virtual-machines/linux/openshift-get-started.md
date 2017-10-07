---
title: "aaaDeploy OpenShift származási tooAzure |} Microsoft Docs"
description: "Ismerje meg, hogy toodeploy OpenShift származási tooAzure virtuális gépek."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jbinder
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: jbinder
ms.openlocfilehash: a67450c46da41134a5f6c669a9e54e14773ac5b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-openshift-origin-tooazure-virtual-machines"></a>OpenShift származási tooAzure virtuális gépek telepítése 

[OpenShift származási](https://www.openshift.org/) egy nyílt forráskódú tároló platform épülő [Kubernetes](https://kubernetes.io/). Egyszerűbbé teszi a központi telepítését, a méretezés és a több-bérlős alkalmazásokhoz működő hello folyamat. 

Ez az útmutató ismerteti, hogyan toodeploy OpenShift forrása az Azure virtuális gépek használatával hello Azure CLI és az Azure Resource Manager-sablonok. Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * A KeyVault toomanage hello OpenShift fürthöz SSH-kulcsok létrehozása.
> * OpenShift-fürt üzembe helyezése Azure virtuális gépeken. 
> * Telepítse és konfigurálja a hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello fürt.
> * Hello OpenShift telepítés testreszabásához.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

A gyors üzembe helyezési szükséges hello Azure CLI 2.0.8 verzió vagy újabb. toofind hello verzió, futtassa `az --version`. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure 
Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat, vagy kattintson **kipróbálás** toouse felhő rendszerhéj.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. 

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.

```azurecli 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-key-vault"></a>Kulcstartó létrehozása
Hozzon létre egy KeyVault toostore hello hello fürthöz SSH-kulcsok hello [az keyvault létrehozása](/cli/azure/keyvault#create) parancsot.  

```azurecli 
az keyvault create --resource-group myResourceGroup --name myKeyVault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>SSH-kulcs létrehozása 
SSH-kulcs szükséges toosecure hozzáférés toohello OpenShift forrás fürt. Hozzon létre egy SSH-kulcspár hello segítségével `ssh-keygen` parancsot. 
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> hello hoz létre SSH-kulcspárral nem lehet egy hozzáférési kódot.

További információk a Windows rendszeren az SSH-kulcsok [hogyan toocreate SSH-kulcsok Windows](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-ssh-private-key-in-key-vault"></a>Key Vault titkos SSH-kulcs tárolása
hello OpenShift telepítési toohello OpenShift fő létrehozott toosecure hozzáférés hello SSH-kulcsot használ. tooenable hello telepítési toosecurely hello SSH-kulcs beolvasása, hello kulcs tárolása a Key Vault hello a következő parancs használatával.

# <a name="enabled-for-template-deployment"></a>Sablon-üzembehelyezés engedélyezve
```azurecli
az keyvault secret set --vault-name KeyVaultName --name OpenShiftKey --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a>Egyszerű szolgáltatás létrehozása 
OpenShift kommunikál az Azure-ban, a felhasználónév és jelszó vagy egy egyszerű szolgáltatást. Egy Azure szolgáltatás egyszerű egy biztonsági azonosító, amely alkalmazások, szolgáltatások és automatizálási eszközökkel, például a OpenShift használható. Szabályozza, és adja meg hello engedélyek, mivel toowhat műveletek hello szolgáltatás egyszerű hajthat végre az Azure-ban. tooimprove biztonsági keresztül csak a felhasználónév és jelszó megadása, ez a példa létrehoz egy alapszintű service egyszerű.

Az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) és kimeneti hello hitelesítő adatait, amelyet a OpenShift:

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {strong password} \
          --scopes $(az group show --name myResourceGroup --query id)
```
Jegyezze fel a hello parancs által visszaadott hello appId tulajdonság.
```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {strong password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Ne hozzon létre nem biztonságos jelszót.  Kövesse az alábbi cikkben ismertetett útmutatást: [Az Azure AD-jelszavakra vonatkozó szabályok és korlátozások](/azure/active-directory/active-directory-passwords-policy).

A szolgáltatás rendszerbiztonsági tagok további információkért lásd: [hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

## <a name="deploy-hello-openshift-origin-template"></a>Hello OpenShift eredeti sablon telepítése
Ezután telepítése Azure Resource Manager-sablonnal OpenShift forrása. 

> [!NOTE] 
> hello következő parancshoz szükséges az CLI 2.0.8 vagy újabb. Ellenőrizheti az CLI hello hello verziójával `az --version` parancsot. tooupdate hello CLI verzió, lásd: [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).

Használjon hello `appId` hello szolgáltatás egyszerű hello korábban létrehozott értéket `aadClientId` paraméter.

```azurecli 
az group deployment create --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --params \ 
        openshiftMasterPublicIpDnsLabel=myopenshiftmaster \
        infraLbPublicIpDnsLabel=myopenshiftlb \
        openshiftPassword=Pass@word!
        sshPublicKey=~/.ssh/openshift_rsa.pub \
        keyVaultResourceGroup=myResourceGroup \
        keyVaultName=myKeyVault \
        keyVaultSecret=OpenShiftKey \
        aadClientId={appId} \
        aadClientSecret={strong password} 
```
hello telepítési too20 perc toocomplete is tarthat. hello hello OpenShift konzol URL-CÍMÉT és DNS-neve hello főkiszolgáló OpenShift nyomtatott toohello Terminálszolgáltatások hello telepítés befejezéséről.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ocpadmin@myopenshiftmaster.cloudapp.azure.com"
}
```
## <a name="connect-toohello-openshift-cluster"></a>Csatlakoztassa toohello OpenShift fürtöt
Hello központi telepítés befejezése után csatlakoztassa toohello OpenShift konzolt hello segítségével hello böngészővel `OpenShift Console Uri`. Másik lehetőségként toohello OpenShift fő hello a következő parancs használatával is elérheti.

```bash
$ ssh ocpadmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása
Ha már nincs szükség, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, OpenShift fürt és az összes kapcsolódó erőforrások parancsot.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Következő lépések

Az ezen oktatóanyag, megismert hogyan:
> [!div class="checklist"]
> * A KeyVault toomanage hello OpenShift fürthöz SSH-kulcsok létrehozása.
> * OpenShift-fürt üzembe helyezése Azure virtuális gépeken. 
> * Telepítse és konfigurálja a hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello fürt.

Most, hogy OpenShift forrás fürt telepítése. Hogyan követheti OpenShift oktatóanyagok toolearn toodeploy az első és használati hello OpenShift eszközök. Lásd: [OpenShift származási első lépések](https://docs.openshift.org/latest/getting_started/index.html) tooget elindult. 
