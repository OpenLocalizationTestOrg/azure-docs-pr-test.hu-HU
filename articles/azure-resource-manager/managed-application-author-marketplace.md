---
title: "aaaAzure hello piactér-alkalmazások felügyelete |} Microsoft Docs"
description: "Ismerteti az Azure által felügyelt alkalmazások hello piactéren keresztül elérhető."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: b3cdf3f1fccdd47db699e4892ae8bce35118bfd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-managed-applications-in-hello-marketplace"></a>Azure által felügyelt alkalmazások hello piactér

 MSPs ISV-k és rendszerintegrátorok (SIs) használható Azure által felügyelt alkalmazások toooffer megoldások tooall Azure piactér ügyfeleiknek. Az ilyen megoldások hello karbantartási és a terhelés karbantartásának ügyfelek csökkentése. Közzétevők biztosíthatja az infrastruktúra- és szoftver hello piactéren keresztül. Ezek csatolhat a szolgáltatások és alkalmazások működési támogatását toomanaged. További információkért lásd: [felügyelt használatát áttekintő cikkben](managed-application-overview.md).

Ez a cikk azt ismerteti, hogyan egy MSP, ISV vagy SI közzétenni egy alkalmazást toohello piactér és toocustomers körben elérhetővé tegye.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Kezelt alkalmazás közzététele előfeltételei

A piactér hello toolisting Előfeltételek:

* Műszaki

    *  Hello alapszintű struktúrát és az Azure Resource Manager-sablonok szintaxisát kapcsolatos információkért lásd: [Azure Resource Manager-sablonok](resource-group-authoring-templates.md).
    *  tooview teljes sablon megoldások, lásd: [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/en-us/documentation/templates/) vagy hello [gyorsindítási sablonon tárház](https://github.com/azure/azure-quickstart-templates).
    *  További információ a hogyan toocreate hello felületet az ügyfelek, akik hello piactéren keresztül az alkalmazás központi telepítése: [hozzon létre egy felhasználói felületet csomagdefiníciós fájl](managed-application-createuidefinition-overview.md).

* A felhasználóknak (üzleti követelmények)

    *   A vállalat vagy a helyi leányvállalatnál olyan országban, ahol értékesítési hello piactér által támogatott kell lennie.
    *   A termék licenccel kell rendelkezniük a oly módon, amely kompatibilis a számlázási modellt hello piactér támogat.
    *   Ön a felelős a technikai támogatási szolgálathoz elérhető toocustomers minden üzleti szempontból ésszerű módon. hello támogatási ingyenes, a fizetős, és közösségi keresztül támogatja.
    *   Ön a felelős a szoftver és a harmadik féltől származó szoftverek függőségeit.
    *   Meg kell adnia, amely megfelel az ajánlat toobe felsorolt tartalom hello piactér és hello Azure-portálon.
    *   Meg kell erősítenie hello Azure piactér részvételét házirendek és a közzétevő megállapodás toohello feltételeit.
    *   El kell fogadnia a használati feltételeket hello, a Microsoft adatvédelmi nyilatkozatát és a Microsoft Azure hitelesített Program szerződés toocomply.

## <a name="create-a-new-azure-application-offer"></a>Hozzon létre egy új Azure-alkalmazásokban ajánlatot

Megfelel a hello Előfeltételek, után készen áll a toocreate még a felügyelt alkalmazási ajánlat. Vegyünk egy ajánlatot és egy SKU gyors áttekintést.

### <a name="offer"></a>Ajánlat

hello ajánlat a kezelt alkalmazás tooa osztály egy közzétételi ajánlat termék felel meg. Ha egy új típusú megoldás/alkalmazás, amelyet az toomake hello piactéren elérhető, állíthat be, mint egy új ajánlat. Az ajánlat termékváltozatok gyűjteménye. Minden ajánlat hello piactér saját entitás jelenik meg.

### <a name="sku"></a>SKU

Egy termékváltozata hello legkisebb purchasable egysége ajánlatot. A Termékváltozat hello belül is használhatja ugyanazt a termék osztály (ajánlat) toodifferentiate között:

* Támogatott különböző szolgáltatások.
* E hello ajánlat felügyelt vagy nem felügyelt.
* Számlázási modellt a támogatottak.

A Termékváltozat hello szülő ajánlat hello piactér alatt jelenik meg. A saját purchasable entitás hello Azure-portálon jelenik meg.

### <a name="set-up-an-offer"></a>Az ajánlat beállítása

1. Jelentkezzen be toohello [Cloud Partner portálra](https://cloudpartner.azure.com/).

2. Hello bal oldali hello navigációs ablaktábláján válassza ki **+ új ajánlat** > **Azure alkalmazások**.

    ![Új ajánlat](./media/managed-application-author-marketplace/newOffer.png)

3. Töltse ki az hello hello megjelenő hello űrlapok **szerkesztő** nézet. A piros csillaggal (*) megjelölve kötelező mezőket.

    ![Az ajánlat beállításait](./media/managed-application-author-marketplace/newOffer_OfferSettings.png)

    Négy fő űrlap használt toocreate kezelt alkalmazás:

    a. Az ajánlat beállításait

    b. Termékváltozat

    c. Piactér

    d. Támogatás

Ezek az űrlapok a következő részekben hello részletesebben ismerteti.

## <a name="offer-settings-form"></a>Az ajánlat beállítások képernyő
Az alapvető űrlap toospecify hello ajánlat beállításait használja.

1. Adja meg a hello **beállításokat kínálnak** űrlap. hello különböző mezők a következők:

    a. **Ajánlat azonosítója**: Ez az egyedi azonosító a közzétevő profilon belül hello ajánlat azonosítja. Ezt az Azonosítót látható termék URL-címek, Resource Manager-sablonok, és számlázási jelenti. Csak összeállítható kisbetűs alfanumerikus karaktereket és kötőjelet (-). hello azonosítója nem végződhet kötőjellel. Korlátozva tooa legfeljebb 50 karakter hosszú lehet. Miután ajánlatot élő kerül, ez a mező zárolva van.

    b. **Gyártó Azonosítóját**: legördülő lista toochoose hello publisher profilt ezt az ajánlatot a toopublish szeretné. Miután ajánlatot élő kerül, ez a mező zárolva van.

    c. **Név**: ezt a megjelenítési nevet az előfizetéshez hello piactér és a hello portálon jelenik meg. Rendelkezhet egy legfeljebb 50 karakter hosszú lehet. A termék márkáját egy felismerhető nevet tartalmaz. A vállalat nevét itt nem tartalmaznak, kivéve, ha ezt forgalmazott hogyan. Ha ez az ajánlat még marketing a saját webhelyén, győződjön meg arról, hogy hello név pontosan hogyan webhelye jelenik meg.

2. Válassza ki **mentése** toosave az előrehaladást. 

## <a name="skus-form"></a>SKU képernyő
hello következő lépésre tooadd termékváltozatok az előfizetéshez.

1. Válassza ki **termékváltozatok** > **új SKU**. 

    ![Válassza ki az új Termékváltozat](./media/managed-application-author-marketplace/newOffer_skus.png)

2. Adjon meg egy **SKU-ID**. A SKU-ID hello SKU ajánlatot belül egyedi azonosítója. Ezt az Azonosítót látható termék URL-címek, Resource Manager-sablonok, és számlázási jelenti. Csak összeállítható kisbetűs alfanumerikus karaktereket és kötőjelet (-). hello azonosítója nem végződhet kötőjellel, és korlátozott tooa legfeljebb 50 karakter hosszú lehet. Miután ajánlatot élő kerül, ez a mező zárolva van. Az ajánlat belül több SKU lehet. A Termékváltozat van szüksége az egyes lemezképek toopublish tervezi.

3. Töltse ki a hello **SKU részletek** hello képernyőn a következő szakaszt:

    ![Adja meg az új Termékváltozat](./media/managed-application-author-marketplace/newOffer_newsku.png)

    Töltse ki a következő mezők hello:
    
    a. **Cím**: Adjon meg egy címet a Termékváltozat. Ez a cím jelenik meg ehhez az elemhez hello gyűjteménye.

    b. **Összegző**: Adjon meg egy rövid összefoglaló a termékváltozat. Ez a szöveg hello cím alatt jelenik meg.

    c. **Leírás**: hello SKU kapcsolatos részletes leírását adja meg.

    d. **SKU típusú**: hello két érték engedélyezett **kezelt alkalmazás** és **Solution Templates**. Ebben az esetben válassza a **kezelt alkalmazás**.

4. Töltse ki a hello **csomag részletei** hello képernyőn a következő szakaszt:

    ![Csomag](./media/managed-application-author-marketplace/newOffer_newsku_package.png)

    Töltse ki a következő mezők hello:

    a. **Aktuális verzió**: Adjon meg egy verzió hello csomag feltöltése. Hello formátumban kell `{number}.{number}.{number}{number}`.

    b. **Válassza ki a csomagfájl**: Ez a csomag tartalmaz hello tömörített fájlok .zip fájlba a következő:
    * **applianceMainTemplate.json**: hello központi telepítési sablon fájl toodeploy hello megoldás/alkalmazás által használt. További információ központi telepítési sablon fájlok toocreate, lásd: [az első Azure Resource Manager-sablon létrehozása](resource-manager-create-first-template.md).
    * **appliancecreateUIDefinition.json**: hello Azure portál toogenerate hello felhasználói felület által használt tooprovision a megoldás/alkalmazás használják ezt a fájlt. További információkért lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
    * **mainTemplate.json**: Ez a sablon fájl csak hello Microsoft.Solution/appliances erőforrást tartalmaz. hello mainTemplate fájl tartalmazza az alábbi tulajdonságokkal hello:

        *  **milyen**: használata **piactér** hello piactér a kezelt alkalmazások.
        *  **ManagedResourceGroupId**: Ez az erőforráscsoport hello ügyfél-előfizetést a, ahol applianceMainTemplate.json definiált összes hello erőforrások telepítése.
        *  **PublisherPackageId**: Ez a karakterlánc egyedi azonosítására szolgál a hello csomag. Adjon meg értéket hello hello formátuma `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.

Hello beszerzése **kínálnak azonosító** és **gyártó Azonosítóját** a portálon, közzétételi hello kép a következő ábrán hello:

![Ajánlat azonosítója](./media/managed-application-author-marketplace/UniqueString_pubid_offerid.png)
        
Szerezze be a hello **SKU-ID**, ahogy az a következő kép hello:

![TERMÉKVÁLTOZAT-AZONOSÍTÓ](./media/managed-application-author-marketplace/UniqueString_skuid.png)
        
Hello csomag beszerzése **verzió**, ahogy az a következő kép hello:

![Csomag verziója](./media/managed-application-author-marketplace/UniqueString_packageversion.png)
    
  Az előző példák hello alapján, hello értékének **PublisherPackageId** van `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.

  A minta mainTemplate.json:

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountNamePrefix": {
        "type": "string",
        "metadata": {
          "description": "Specify hello name of hello storage account"
        }
      },
      "storageAccountType": {
        "type": "string"
      }
    },
    "variables": {
      "managedResourceGroup": "[concat(resourceGroup().id,uniquestring(resourceGroup().id))]"
    },
    "resources": [{
      "type": "Microsoft.Solutions/appliances",
      "apiVersion": "2016-09-01-preview",
      "name": "[concat(parameters('storageAccountNamePrefix'), '-', 'managed')]",
      "location": "[resourceGroup().location]",
      "kind": "marketplace",
      "properties": {
        "managedResourceGroupId": "[variables('managedResourceGroup')]",
        "PublisherPackageId":"azureappliancetest.ravmanagedapptest.ravpreviewmanagedsku.1.0.0",
        "parameters": {
          "storageAccountName": {
            "value": "[parameters('storageAccountNamePrefix')]"
          },
          "storageAccountType": {
            "value": "[parameters('storageAccountType')]"
          }
        }
      }
    }],
    "outputs": {

    }
  }
  ```

A csomagnak tartalmaznia kell minden olyan beágyazott sablon vagy parancsfájlokat, amelyek a szükséges toosuccessfully telepíteni ezt az alkalmazást. hello mainTemplate.json applianceMainTemplate.json és applianceCreateUIDefinition.json fájlokat kell lennie: hello gyökérmappájába.

* **Engedélyek**: Ez a tulajdonság határozza meg, hogy az ügyfelek előfizetések toohello erőforrások elérését és hello szintjét, illetve jogosultak. hello publisher használható toomanage hello alkalmazás hello ügyfél nevében.
* **PrincipalId**: Ez a tulajdonság akkor hello Azure Active Directory (Azure AD) azonosítója egy felhasználó, a felhasználói csoport vagy az alkalmazás, amely rendelkezik megfelelő engedélyekkel hello erőforrásainak hello ügyfél-előfizetést. Szerepkör-definíció hello hello engedélyeket ismerteti. 
* **Szerepkör-definíció**: Ez a tulajdonság akkor hello beépített szerepköralapú hozzáférés-vezérlést (RBAC) szerepkörök az Azure AD által biztosított listáját. Kiválaszthatja a hello szerepkört, amely leginkább megfelelő toouse toomanage hello erőforrások hello ügyfél nevében.

Több engedélyeket adhat hozzá. Javasoljuk, hogy hozzon létre az Active Directory-felhasználók csoportnak, és adja meg az Azonosítót a **PrincipalId**. Ezzel a módszerrel adhat hozzá további felhasználókat toohello felhasználói csoport hello kell tooupdate hello SKU nélkül.

Az RBAC kapcsolatos további információkért lásd: [megismerheti az RBAC a hello Azure-portálon](../active-directory/role-based-access-control-what-is.md).

## <a name="marketplace-form"></a>Piactér képernyő

hello piactér űrlap kér meg hello mezők [Azure piactér](https://azuremarketplace.microsoft.com) a hello [Azure-portálon](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Előzetes előfizetés-azonosítók

Írjon be egy Azure-előfizetés azonosítók hello ajánlat által elérhető, hogy közzététele után. A fehér felsorolt előfizetések tootest megtekintett hello ajánlat előtt használható live. Hello partnerportálon too100 előfizetések listája üres állíthat össze.

### <a name="suggested-categories"></a>Javasolt kategóriák

Válassza ki az ajánlatot is legjobb társítható hello listából toofive kategóriák létrehozása. Ezen kategóriák vannak használt toomap az ajánlat toohello termékkategóriák hello elérhető [Azure piactér](https://azuremarketplace.microsoft.com) és hello [Azure-portálon](https://portal.azure.com/).

#### <a name="azure-marketplace"></a>Azure Piactér

a felügyelt alkalmazás hello összegzése a következő mezők hello jeleníti meg:

![Piactér-összefoglaló](./media/managed-application-author-marketplace/publishvm10.png)

Hello **áttekintése** a felügyelt alkalmazás jeleníti meg a következő mezők hello lapján:

![Piactér áttekintése](./media/managed-application-author-marketplace/publishvm11.png)

Hello **tervek + árazás** a felügyelt alkalmazás jeleníti meg a következő mezők hello lapján:

![Piactér tervek](./media/managed-application-author-marketplace/publishvm15.png)

#### <a name="azure-portal"></a>Azure Portal

a felügyelt alkalmazás hello összegzése a következő mezők hello jeleníti meg:

![A portál összefoglaló](./media/managed-application-author-marketplace/publishvm12.png)

a felügyelt alkalmazás hello áttekintése a következő mezők hello jeleníti meg:

![Portál áttekintése](./media/managed-application-author-marketplace/publishvm13.png)

#### <a name="logo-guidelines"></a>Emblémáinak használatáról

Kövesse a bármely hello Cloud Partner portálra a feltöltött embléma:

*   hello Azure tervezési egy egyszerű színpaletta rendelkezik. Hello száma elsődleges és másodlagos színek az embléma korlátozza.
*   hello téma hello portál színeket fehér és fekete. Nem használja ezeket a színeket hello háttérszínnel az embléma. Használjon, amely lehetővé teszi az embléma jól láthatóan elhelyezett hello portálon színt. Azt javasoljuk, hogy egyszerű alapszínek. *Átlátszó háttérrel használatakor győződjön meg arról, hogy hello embléma és a szöveg nem fehér, fekete, vagy a kék.*
*   Ne használjon a háttér átmenetének hello embléma a.
*   Ne tegyen szöveg hello embléma, még a vállalat vagy a márka neve. hello megjelenését és működését az embléma legyen egyszerű és átmenetek elkerülése érdekében.
*   Ellenőrizze, hogy hello embléma nincs archiválva a felhőbe.

#### <a name="hero-logo"></a>Hero-embléma

hello hero embléma nem kötelező megadni. hello publisher nem tooupload hero embléma választhat. Miután hello hero ikon töltheti fel, nem lehet törölni. Ugyanakkor a hello partner hello piactér irányelvek hero ikonok kell követnie.

Kövesse a hello hero embléma ikon:

*   hello publisher megjelenített név, a hello terv jogcímre és a hosszú összefoglalás hello ajánlat fehér jelennek meg. Ezért ne használjon világos színű hello háttér hello hero ikon. Egy fekete, fehér vagy átlátszó háttér hero ikonok nem engedélyezett.
*   Hello ajánlat szerepel, miután hello publisher megjelenítésére neve, hello terv cím, hello ajánlat hosszú összefoglalás hello **létrehozása** gomb programozott módon beágyazott hello hero embléma belül. Ezért ne adjon meg szöveget hello hero embléma tervezése közben. Meghagyása üres területet hello jobb mivel hello szöveg szerepel programozott módon, hogy a hely. hello üres hello szöveg kell lennie a jobb oldali hello 415 x 100 képpont. Hello balról 370 képpont ellensúlyozza.

    ![Hero embléma – példa](./media/managed-application-author-marketplace/publishvm14.png)

## <a name="support-form"></a>Támogatja az űrlap

Töltse ki a hello **támogatja** űrlap-támogatással rendelkező kapcsolatba lép a vállalat. Ezek az információk előfordulhat, hogy mérnöki, névjegyek és ügyfél-támogatási kapcsolattartók.

## <a name="publish-an-offer"></a>Ajánlat közzététele

Adja meg az összes hello szakaszok, után válassza ki **közzététel** toostart hello folyamat, amely lehetővé teszi a ajánlat elérhető toocustomers.

## <a name="next-steps"></a>Következő lépések

* Egy bevezető toomanaged alkalmazások, lásd: [felügyelt használatát áttekintő cikkben](managed-application-overview.md).
* További információ a piactér hello a kezelt alkalmazás felhasználása: [felhasználásához Azure felügyelt alkalmazások a piactér hello](managed-application-consume-marketplace.md).
* További információ a szolgáltatáskatalógus kezelt alkalmazás közzététele: [létrehozása és a szolgáltatáskatalógus kezelt alkalmazás közzététele](managed-application-publishing.md).
* További információ a szolgáltatási katalógus által felügyelt alkalmazások felhasználása: [felhasználását a szolgáltatási katalógus által felügyelt alkalmazások](managed-application-consumption.md).
