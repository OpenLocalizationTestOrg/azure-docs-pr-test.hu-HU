---
title: "az Azure CLI Azure alkalmazás aaaCreate identitást |} Microsoft Docs"
description: "Ismerteti, hogyan szabályozza a toouse Azure CLI toocreate egy Azure Active Directory-alkalmazás és szolgáltatás egyszerű, és azt keresztül férnek hozzá tooresources szerepkörön alapuló hozzáférés biztosítása. Azt illusztrálja, hogyan tooauthenticate alkalmazás jelszóval vagy tanúsítvánnyal."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c224a189-dd28-4801-b3e3-26991b0eb24d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 0d693ec801d4f4d6c24ec420580776e73014b325
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-cli-toocreate-a-service-principal-tooaccess-resources"></a>A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure CLI toocreate

Ha egy alkalmazás vagy parancsfájlt, amelyet a tooaccess erőforrások, hello alkalmazás identitás beállítása, és hitelesíteni hello alkalmazást a saját hitelesítő adatokkal. Ezzel az identitással egyszerű szolgáltatás néven ismert. Ez a megközelítés lehetővé teszi:

* Engedélyek hozzárendelése saját engedélyek eltérő toohello identitását. Ezek az engedélyek általában korlátozott tooexactly milyen hello alkalmazásnak kell toodo.
* A tanúsítvány használata a hitelesítéshez, egy felügyelet nélküli parancsfájl végrehajtása közben.

Ez a cikk bemutatja, hogyan toouse [Azure CLI 1.0](../cli-install-nodejs.md) tooset be az alkalmazás toorun a saját hitelesítő adatait és identitás. Telepítse a legújabb változatát hello [Azure CLI 1.0](../cli-install-nodejs.md) meg arról, hogy a környezet megfelel a cikkben szereplő példák hello toomake.

## <a name="required-permissions"></a>Szükséges engedélyek
toocomplete Ez a témakör megfelelő engedélyekkel kell rendelkeznie az Azure Active Directory és az Azure-előfizetésében is. Pontosabban tudja toocreate hello Azure Active Directory az alkalmazásban, és hello szolgáltatás egyszerű tooa szerepkör hozzárendelése. 

hello legegyszerűbb módja toocheck hello portálon keresztül van a fiók rendelkezik-e megfelelő engedélyekkel. Lásd [Szükséges jogosultságok ellenőrzése a portálon](resource-group-create-service-principal-portal.md#required-permissions).

A következő lépésben tooa szakasz bármelyik [jelszó](#create-service-principal-with-password) vagy [tanúsítvány](#create-service-principal-with-certificate) hitelesítés.

## <a name="create-service-principal-with-password"></a>Egyszerű szolgáltatásnév létrehozása jelszóval
Ebben a szakaszban hajtsa végre a hello lépéseket toocreate hello jelszóval AD-alkalmazást, és rendelje hozzá a hello olvasó szerepkört toohello egyszerű.

1. Jelentkezzen be tooyour fiókjával.
   
   ```azurecli
   azure login
   ```
2. toocreate app identitást, hello alkalmazás hello nevét és jelszavát, adja meg, ahogy az a következő parancs hello:
     
   ```azurecli
   azure ad sp create -n exampleapp -p {your-password}
   ```
     
   hello új egyszerű szolgáltatásnév adja vissza. Az objektumazonosító hello van szükség, ha a jogosultságokat. az egyszerű szolgáltatásnevek hello felsorolt hello guid van szükség, ha van bejelentkezve. Ez a guid egy hello hello app ID ugyanazt az értéket. A hello alkalmazásokat, az értéket nem hivatkozott tooas hello `Client ID`. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating application exampleapp
     / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
     data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
     data:    Display Name:            exampleapp
     data:    Service Principal Names:
     data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
     data:                             https://www.contoso.org/example
     info:    ad sp create command OK
   ```

3. Adja meg a hello szolgáltatás egyszerű engedélyeket az előfizetéshez. A jelen példában adja hozzá hello szolgáltatás egyszerű toohello olvasó szerepkört, amely ad engedélyt tooread hello előfizetésben található összes erőforrást. Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md). Hello objectid paraméter adja meg, amely hello alkalmazás létrehozásakor használt objektumazonosító hello. A parancs futtatása előtt engedélyeznie kell a némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directoryban. Ha manuálisan futtassa a következő parancsokat, általában elegendő idő eltelte feladatok között. Egy parancsfájlban, hozzá kell adnia egy lépés toosleep hello parancsok között (például `sleep 15`). Ha egy hiba azzal hello egyszerű hello könyvtárban nem létezik, akkor futtassa újra a hello parancsot.
   
   ```azurecli
   azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   ```
   
Ennyi az egész! Az AD-alkalmazást és egy egyszerű szolgáltatást be vannak állítva. hello következő szakasz bemutatja, hogyan toolog hello be a hitelesítő adatok Azure parancssori felületen keresztül. Ha toouse hello hitelesítő kódot alkalmazásában, nem kell toocontinue az ebben a témakörben. Toohello is ugorhat [mintaalkalmazást](#sample-applications) példák a jelentkezhetnek be az alkalmazás azonosítóját és jelszavát. 

### <a name="provide-credentials-through-azure-cli"></a>Adjon meg hitelesítő adatokat az Azure parancssori felület használatával
Most van szüksége a toolog hello alkalmazás tooperform műveletként.

1. Amikor jelentkezik be, és egy egyszerű szolgáltatást, az AD-alkalmazás kell tooprovide hello bérlőazonosító hello könyvtár. A bérlő az Azure Active Directory példánya. hello-bérlőazonosító tooretrieve jelenleg hitelesített előfizetési, használja:
   
   ```azurecli
   azure account show
   ```
   
   Amely adja vissza:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
     Tooget hello bérlői azonosító egy másik előfizetés szükséges, ha használja a következő parancs hello:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Ha naplózás tooretrieve hello ügyfél azonosító toouse van szüksége, használja:
   
   ```azurecli
   azure ad sp show -c exampleapp --json
   ```
   
     hello érték toouse való bejelentkezéskor hello egyszerű szolgáltatásnevek felsorolt hello guid.
   
   ```azurecli
   [
     {
       "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "7132aca4-1bdb-4238-ad81-996ff91d8db4"
       ]
     }
   ]
   ```
3. Jelentkezzen be egyszerű hello szolgáltatás.
   
   ```azurecli
   azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   ```
   
    Hello jelszó megadását kéri. Hello AD-alkalmazás létrehozásakor megadott hello jelszót megadnia.
   
   ```azurecli
   info:    Executing command login
   Password: ********
   ```

Most már hitelesítve mint hello szolgáltatás egyszerű létrehozott egyszerű hello szolgáltatást.

Azt is megteheti hívhat meg hello parancssori toolog a többi műveletek. Hello hitelesítési választ, a hozzáférési jogkivonat hello használható egyéb műveletek kérheti le. Például egy hello hozzáférési jogkivonat beolvasása REST műveleteinek figyelőn, lásd: [generálása egy hozzáférési jogkivonat](resource-manager-rest-api.md#generating-an-access-token).

## <a name="create-service-principal-with-certificate"></a>A tanúsítvány egyszerű szolgáltatás létrehozása
Ebben a szakaszban hello lépéseket hajtsa végre:

* Önaláírt tanúsítvány létrehozása
* hello tanúsítványt, és egy egyszerű hello szolgáltatást hello AD-alkalmazás létrehozása
* hello olvasó szerepkört toohello egyszerű hozzárendelése

Ezek a lépések toocomplete, rendelkeznie kell [OpenSSL](http://www.openssl.org/) telepítve.

1. Hozzon létre egy önaláírt tanúsítványt.
   
   ```
   openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
   ```

2. előző lépésben létrehozott két fájlt - privkey.pem és cert.pem hello. Hello nyilvános és titkos kulcsok egyesítése egyetlen fájlt.

   ```
   cat privkey.pem cert.pem > examplecert.pem
   ```

3. Nyissa meg hello **examplecert.pem** fájlt, és keressen hello hosszú karaktersorozatot, mely közötti **---BEGIN CERTIFICATE---** és **---vége tanúsítvány---**. Másolja a hello tanúsítványának adatait. Ezek az adatok továbbítsa paraméterként hello egyszerű szolgáltatás létrehozása során.

4. Jelentkezzen be tooyour fiókjával.

   ```azurecli
   azure login
   ```
5. toocreate hello szolgáltatás egyszerű adja hello hello app és hello Tanúsítványadatok, ahogy az a következő parancs hello:
     
   ```azurecli
   azure ad sp create -n exampleapp --cert-value {certificate data}
   ```
     
   hello új egyszerű szolgáltatásnév adja vissza. Az objektumazonosító hello van szükség, ha a jogosultságokat. az egyszerű szolgáltatásnevek hello felsorolt hello guid van szükség, ha van bejelentkezve. Ez a guid egy hello hello app ID ugyanazt az értéket. A hello alkalmazásokat az értéket nem hivatkozott tooas hello ügyfél-azonosítót. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
     data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
     data:    Display Name:     exampleapp
     data:    Service Principal Names:
     data:                      4fd39843-c338-417d-b549-a545f584a745
     data:                      https://www.contoso.org/example
     info:    ad sp create command OK
   ```
6. Adja meg a hello szolgáltatás egyszerű engedélyeket az előfizetéshez. A jelen példában adja hozzá hello szolgáltatás egyszerű toohello olvasó szerepkört, amely ad engedélyt tooread hello előfizetésben található összes erőforrást. Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md). Hello objectid paraméter adja meg, amely hello alkalmazás létrehozásakor használt objektumazonosító hello. A parancs futtatása előtt engedélyeznie kell a némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directoryban. Ha manuálisan futtassa a következő parancsokat, általában elegendő idő eltelte feladatok között. Egy parancsfájlban, hozzá kell adnia egy lépés toosleep hello parancsok között (például `sleep 15`). Ha egy hiba azzal hello egyszerű hello könyvtárban nem létezik, akkor futtassa újra a hello parancsot.
   
   ```azurecli
   azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   ```
  
### <a name="provide-certificate-through-automated-azure-cli-script"></a>Adja meg a tanúsítvány automatikus Azure CLI-parancsfájl használatával
Most van szüksége a toolog hello alkalmazás tooperform műveletként.

1. Amikor jelentkezik be, és egy egyszerű szolgáltatást, az AD-alkalmazás kell tooprovide hello bérlőazonosító hello könyvtár. A bérlő az Azure Active Directory példánya. hello-bérlőazonosító tooretrieve jelenleg hitelesített előfizetési, használja:
   
   ```azurecli
   azure account show
   ```
   
   Amely adja vissza:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
   Tooget hello bérlői azonosító egy másik előfizetés szükséges, ha használja a következő parancs hello:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. tooretrieve hello tanúsítvány ujjlenyomatát, és távolítsa el a felesleges karaktereket, használja:
   
   ```
   openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   ```
   
   Amely értékét adja vissza egy ujjlenyomat hasonló:
   
   ```
   30996D9CE48A0B6E0CD49DBB9A48059BF9355851
   ```
3. Ha naplózás tooretrieve hello ügyfél azonosító toouse van szüksége, használja:
   
   ```azurecli
   azure ad sp show -c exampleapp
   ```
   
   hello érték toouse való bejelentkezéskor hello egyszerű szolgáltatásnevek felsorolt hello guid.
     
   ```azurecli
   [
     {
       "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "4fd39843-c338-417d-b549-a545f584a745",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "4fd39843-c338-417d-b549-a545f584a745"
       ]
     }
   ]
   ```
4. Jelentkezzen be egyszerű hello szolgáltatás.
   
   ```azurecli
   azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}
   ```

Most már hitelesítve hello Azure Active Directory-alkalmazást, az egyszerű hello szolgáltatást.

## <a name="change-credentials"></a>Hitelesítő adatok módosítása

AD-alkalmazás, vagy a biztonsági sérülés vagy a hitelesítő adatok érvényessége miatt toochange hello hitelesítő adatait használja `azure ad app set`.

a jelszó toochange használja:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --password p@ssword
```

egy tanúsítvány érték toochange használja:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --cert-value {certificate data}
```

## <a name="debug"></a>Hibakeresés

Egy egyszerű szolgáltatás létrehozásakor a következő hibák hello merülhetnek fel:

* **"Authentication_Unauthorized"** vagy **"előfizetést hello a környezetben található."** – Ezt a hibaüzenetet látja, ha a fiók nem rendelkezik hello [szükséges engedélyek](#required-permissions) a hello Azure Active Directory tooregister egy alkalmazást. Általában ezt a hibát látva az Azure Active Directoryban csak rendszergazda felhasználók regisztrálhatják az alkalmazásokat, és a fiók nincs a rendszergazda segítségét. Kérje a rendszergazda tooeither rendelje hozzá, akkor tooan rendszergazdai szerepkör, illetve tooenable felhasználók tooregister alkalmazások.

* A fiók **"Nincs engedély tooperform művelet"Microsoft.Authorization/roleAssignments/write"hatókörben"/Subscriptions/ {guid}"."**  -Ezt a hibaüzenetet látja, ha a fiók nem rendelkezik elegendő engedélyekkel tooassign egy szerepkör tooan identitást. Kérje meg az előfizetés rendszergazdája tooadd meg tooUser hozzáférési rendszergazdai szerepkört.

## <a name="sample-applications"></a>Mintaalkalmazások
Különböző platformokon keresztül hello alkalmazásként naplózás kapcsolatos információkért lásd:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Következő lépések
* Az alkalmazás integrálása az Azure erőforrások kezeléséhez részletes lépéseiért lásd: [– fejlesztői útmutató tooauthorization hello Azure Resource Manager API-val rendelkező](resource-manager-api-authentication.md).
* tooget tanúsítványok és az Azure parancssori felület használatával kapcsolatos további tudnivalókat lásd a [Tanúsítványalapú hitelesítés az Azure Szolgáltatásnevekről Linux parancssorból](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
* Az elérhető műveleteket, lehet megadott vagy toousers megtagadva listájáért lásd: [Azure Resource Manager erőforrás-szolgáltató műveletek](../active-directory/role-based-access-control-resource-provider-operations.md).
