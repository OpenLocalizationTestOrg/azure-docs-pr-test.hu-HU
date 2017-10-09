---
title: "aaaService egyszerű Azure Kubernetes fürthöz |} Microsoft Docs"
description: "Kubernetes-fürthöz tartozó Azure Active Directory egyszerű szolgáltatás létrehozása és felügyelete az Azure Container Service-ben"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 7a01624c5ac3fa717dbcbd570e05ceb4d917c53a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-an-azure-ad-service-principal-for-a-kubernetes-cluster-in-container-service"></a>Kubernetes-fürthöz tartozó Azure AD egyszerű szolgáltatás beállítása a Container Service-ben


Az Azure Tárolószolgáltatásban, Kubernetes fürt szükséges egy [Azure Active Directory szolgáltatás egyszerű](../../active-directory/develop/active-directory-application-objects.md) toointeract Azure API-khoz. hello szolgáltatás egyszerű szükséges toodynamically kezelheti az erőforrásokat például [felhasználó által definiált útvonalak](../../virtual-network/virtual-networks-udr-overview.md) és hello [réteg 4 Azure Load Balancer](../../load-balancer/load-balancer-overview.md). 


Ez a cikk ismerteti a különböző beállítások tooset egy szolgáltatás egyszerű a Kubernetes fürt számára. Például, ha telepítve, és állítsa be hello [Azure CLI 2.0](/cli/azure/install-az-cli2), futtathatja hello [ `az acs create` ](/cli/azure/acs#create) parancs toocreate hello Kubernetes fürt és hello egyszerű szolgáltatásnév: hello ugyanannyi időt vesz igénybe.


## <a name="requirements-for-hello-service-principal"></a>Hello szolgáltatás egyszerű követelményei

Használhat egy meglévő Azure AD szolgáltatás egyszerű, hogy megfelel-e hello követelményeknek, vagy hozzon létre egy újat.

* **Hatókör**: hello erőforráscsoport hello előfizetésben toodeploy hello Kubernetes fürt, vagy (kevésbé korlátozottan) hello előfizetés fel toodeploy hello fürt.

* **Szerepkör**: **Közreműködő**

* **Titkos ügyfélkulcs**: egy jelszónak kell lennie. Jelenleg nem használhat egyszerű szolgáltatás beállítást tanúsítvány hitelesítéshez.

> [!IMPORTANT] 
> egy egyszerű szolgáltatásnév toocreate, rendelkeznie kell engedélyek tooregister az alkalmazás az Azure AD-bérlő és a tooassign hello alkalmazás tooa szerepkör az előfizetésben. hello szükséges engedélyek, ha toosee [hello Portal beadása](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions). 
>

## <a name="option-1-create-a-service-principal-in-azure-ad"></a>1. lehetőség: Egyszerű szolgáltatás létrehozása az Azure AD-ban

Ha az Azure AD szolgáltatás egyszerű toocreate Kubernetes fürt központi telepítése előtt, az Azure számos módszert biztosít. 

a következő Példaparancsok hello mutatja be toodo hello a [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md). Másik lehetőségként létrehozhat egy szolgáltatás egyszerű használatával [Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), hello [portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md), vagy más módszerrel.

```azurecli
az login

az account set --subscription "mySubscriptionID"

az group create -n "myResourceGroupName" -l "westus"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID/resourceGroups/myResourceGroupName"
```

(Itt látható kivont) hasonló toohello következő kimenete:

![Egyszerű szolgáltatás létrehozása](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

A kijelölt vannak hello **ügyfél-azonosító** (`appId`) és hello **ügyfélkulcs** (`password`) az fürtöt tartalmazó környezetben használt service egyszerű paraméterekként.


### <a name="specify-service-principal-when-creating-hello-kubernetes-cluster"></a>Adja meg a szolgáltatás egyszerű hello Kubernetes fürt létrehozásakor

Adja meg a hello **ügyfél-azonosító** (más néven hello `appId`, az Alkalmazásazonosító) és **ügyfélkulcs** (`password`) egy meglévő egyszerű szolgáltatásnév paraméter hello létrehozásakor a Kubernetes fürt. Ellenőrizze, hogy hello szolgáltatás egyszerű kezdve ez a cikk hello: hello követelményeknek megfelelő.

Ezek a paraméterek hello Kubernetes fürtjéhez hello telepítésekor megadhatja [Azure parancssori felület (CLI) 2.0-s](container-service-kubernetes-walkthrough.md), [Azure-portálon](../dcos-swarm/container-service-deployment.md), vagy más módszerrel.

>[!TIP] 
>Hello megadásakor **ügyfél-azonosító**, lehet, hogy toouse hello `appId`, nem hello `ObjectId`, hello szolgáltatás rendszerbiztonsági tag.
>

hello alábbi példában látható egyirányú toopass hello Azure CLI 2.0 hello paramétereket. Ez a példa hello [Kubernetes gyorsindítási sablonon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).

1. [Töltse le](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) hello sablon paraméterfájl `azuredeploy.parameters.json` a Githubról.

2. toospecify hello szolgáltatás egyszerű, adja meg az értékeket `servicePrincipalClientId` és `servicePrincipalClientSecret` hello fájlban. (Szükség tooprovide a saját értékeit `dnsNamePrefix` és `sshRSAPublicKey`. Ez utóbbi hello hello SSH nyilvános kulcs tooaccess hello fürt.) Hello fájl mentéséhez.

    ![Az egyszerű szolgáltatás paramétereinek továbbítása](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. A következő parancsot, használatával futtatási hello `--parameters` tooset hello elérési toohello azuredeploy.parameters.json fájlt. Ez a parancs telepíti egy erőforráscsoportban hoz létre a hívott hello fürt `myResourceGroup` hello USA nyugati régiója régióban.

    ```azurecli
    az login

    az account set --subscription "mySubscriptionID"

    az group create --name "myResourceGroup" --location "westus" 
    
    az group deployment create -g "myResourceGroup" --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" --parameters @azuredeploy.parameters.json
    ```


## <a name="option-2-generate-a-service-principal-when-creating-hello-cluster-with-az-acs-create"></a>2. lehetőség: Egy egyszerű szolgáltatás létrehozása, ha hello fürt létrehozása`az acs create`

Ha futtatja a hello [ `az acs create` ](/cli/azure/acs#create) parancs toocreate Kubernetes fürt hello telepítette, hello beállítás toogenerate szolgáltatásnevet automatikusan.

Hasonlóan mint más Kubernetes-fürt létrehozási lehetőségek esetében, az `az acs create` futtatásakor megadhatja egy meglévő egyszerű szolgáltatás paramétereit. Azonban ha kihagyja ezeket a paramétereket, hello Azure CLI hoz létre egy automatikusan használja a Tárolószolgáltatás. Ez akkor történik meg transzparens módon hello üzembe helyezése során. 

hello következő parancs egy Kubernetes fürtöt hoz létre, és SSH-kulcsok és a szolgáltatás egyszerű hitelesítő adatait:

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

> [!IMPORTANT]
> Ha a fiók nem rendelkezik hello Azure AD és az előfizetés engedélyek toocreate egy egyszerű szolgáltatást, hello parancs hibát generál hasonló túl`Insufficient privileges toocomplete hello operation.`
> 

## <a name="additional-considerations"></a>Néhány fontos megjegyzés

* Ha nincs engedélyek toocreate egyszerű szolgáltatás az előfizetéshez, szükség lehet a tooask az Azure AD vagy az előfizetés rendszergazdája tooassign hello szükséges engedélyekkel, vagy kérje az Azure Tárolószolgáltatás egy szolgáltatás egyszerű toouse. 

* a Kubernetes egyszerű hello szolgáltatás hello fürtkonfiguráció része. Azonban nem használható hello identitás toodeploy hello fürt.

* Minden egyszerű szolgáltatás társítva van egy Azure AD-alkalmazáshoz. hello szolgáltatás egyszerű Kubernetes fürt társítható bármilyen érvényes Azure AD-alkalmazás neve (például: `https://www.contoso.org/example`). hello alkalmazás URL-címe hello valós végpont nem rendelkezik a toobe.

* Ha megadó hello szolgáltatás egyszerű **ügyfél-azonosító**, használhatja a hello hello értékének `appId` (amint ez a cikk) vagy hello tartozó egyszerű szolgáltatásnév `name` (például`https://www.contoso.org/example`).

* Hello master és a virtuális gépek ügynök hello Kubernetes fürt hello fájl /etc/kubernetes/azure.json hello szolgáltatás egyszerű hitelesítő adatait tárolja.

* Amikor az hello `az acs create` toogenerate hello szolgáltatás egyszerű parancs automatikusan, hello szolgáltatás egyszerű hitelesítő adatait írt toohello fájl ~/.azure/acsServicePrincipal.json hello gépen használt toorun hello parancsot. 

* Hello használata esetén `az acs create` toogenerate hello szolgáltatás egyszerű parancs automatikusan, egyszerű hello szolgáltatást is képes hitelesíteni az egy [Azure tároló beállításjegyzék](../../container-registry/container-registry-intro.md) hello létrehozott azonos előfizetéssel.




## <a name="next-steps"></a>Következő lépések

* [Ismerkedés a Kubernetes-szel](container-service-kubernetes-walkthrough.md) a tárolószolgáltatás-fürtben.

* tootroubleshoot hello szolgáltatás egyszerű Kubernetes, lásd: hello [ACS motor dokumentáció](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).


