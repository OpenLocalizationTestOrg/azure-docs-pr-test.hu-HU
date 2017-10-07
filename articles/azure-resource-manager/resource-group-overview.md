---
title: "aaaAzure Resource Manager áttekintése |} Microsoft Docs"
description: "Ismerteti, hogyan toouse Azure Resource Manager üzembe helyezését, felügyeleti és az Azure-erőforrások hozzáférés-vezérlés."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 76df7de1-1d3b-436e-9b44-e1b3766b3961
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: tomfitz
ms.openlocfilehash: a44fccd96d722c006224145d71cc44292255debf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-overview"></a>Az Azure Resource Manager áttekintése
az alkalmazás hello infrastruktúrája általában számos összetevőből áll – például egy virtuális gépből, tárfiókot, és virtuális hálózati vagy egy webalkalmazást, adatbázis, adatbázis-kiszolgáló és 3. fél szolgáltatások épül fel. Ezeket az összetevőket nem külön entitásokként látja, hanem egyetlen entitás kapcsolódó és egymással összefüggő részeiként. Szeretné, hogy toodeploy, kezelheti és figyelheti azokat csoportként. Az Azure Resource Manager lehetővé teszi a megoldás hello erőforrásokat toowork csoportként. Telepíteni, frissíteni vagy hello összes erőforrást törli a megoldás egyetlen, koordinált műveletben. A telepítéshez egy sablon használatos, amely különböző, például tesztelési, átmeneti és üzemi környezetben is képes működni. A Resource Manager biztosít biztonsági, naplózási és címkézési szolgáltatásokat toohelp, kezelheti az erőforrásokat a telepítés utáni. 

## <a name="terminology"></a>Terminológia
Ha új tooAzure erőforrás-kezelő, van néhány nem feltétlenül ismeri a feltételeket.

* **erőforrás** – Egy olyan kezelhető elem, amely az Azure-on keresztül érhető el. Általános erőforrás például a következő: virtuális gép, tárfiók, webalkalmazás, adatbázis, virtuális hálózat, de számos további fajtája is létezik.
* **erőforráscsoport** – Egy olyan tároló, amely egy Azure-megoldáshoz kapcsolódó erőforrásokat tárol. hello erőforráscsoport összes hello erőforrások hello megoldás, és csak azokat az erőforrásokat, amelyet az egy csoportként toomanage tartalmazhatnak. Úgy dönt, hogy hogyan tooallocate erőforrások tooresource csoportok alapján hasznossá hello a legtöbb, a szervezet számára legjobb. Lásd: [erőforráscsoportok](#resource-groups).
* **erőforrás-szolgáltató** -egy szolgáltatás, amely megadja az erőforrások hello telepítheti és kezelheti a Resource Manageren keresztül. Mindegyik erőforrás-szolgáltató telepített hello erőforrásaival kapcsolatos műveletek kínál. Néhány általános erőforrás-szolgáltatók a következők: Microsoft.Compute, hello virtuálisgép-erőforrás értendők, Microsoft.Storage, hello tárolási fiók erőforrás értendők és Microsoft.Web, amely megadja az erőforrások kapcsolódó tooweb alkalmazásokat. Lásd: [erőforrás-szolgáltatók](#resource-providers).
* **Resource Manager-sablon** -A JavaScript Object Notation (JSON) fájl, amely egy vagy több erőforrások toodeploy tooa erőforrás csoportját határozza meg. Hello telepített erőforrások közötti hello függőségeket is meghatározza. hello sablon használt toodeploy hello erőforrások csak következetesen és ismételten. Lásd: [sablonalapú telepítés](#template-deployment).
* **deklaratív szintaxisán** -szintaxis, amely lehetővé teszi állapot "Íme I kíván toocreate" anélkül, hogy a parancsok toocreate programozási toowrite hello feladatütemezési azt. hello Resource Manager-sablonok deklaratív szintaxisán példája. Hello fájlban hello infrastruktúra toodeploy tooAzure hello tulajdonságait határozza meg. 

## <a name="hello-benefits-of-using-resource-manager"></a>Erőforrás-kezelő használatával hello előnyei
A Resource Manager számos előnyt kínál:

* Telepíthetik, felügyelhetik és erőforrások hello figyeli, hogy a megoldás egy csoportot, hanem erőforrások különálló kezelése.
* Ismételten telepítheti a megoldást hello fejlesztési életciklus során, és lehet abban, hogy az erőforrások telepítése konzisztens lesz.
* Az infrastruktúrát szkriptek helyett deklaratív sablonok segítségével is kezelheti.
* Megadhatja, hogy hello függőségek között erőforrásokat, hogy azok hello megfelelő sorrendben legyenek telepítve.
* Hozzáférés-vezérlési tooall szolgáltatásokat alkalmazhat az erőforráscsoportban, mivel a szerepköralapú hozzáférés-vezérlést (RBAC) natív módon integrálva hello felügyeleti platform.
* Címkékkel láthatja tooresources toologically rendszerezése összes hello erőforrást az előfizetésében.
* Tisztázhatja a szervezete erőforrások csoportjának költségeinek megtekintésével megosztása hello azonos címkével.  

Erőforrás-kezelő egy új módon toodeploy biztosít, és a megoldások kezelése. Ha követte hello korábbi telepítési modellt és toolearn hello változásokról, tekintse meg [Understanding Resource Manager üzembe helyezési és a klasszikus üzembe helyezési](resource-manager-deployment-model.md).

## <a name="consistent-management-layer"></a>Konzisztens felügyeleti réteg
A Resource Manager biztosít egy konzisztens felügyeleti réteg hello Azure PowerShell, az Azure parancssori felület, Azure-portálon, REST API és fejlesztői eszközök segítségével elvégezhető feladatokhoz. Minden hello eszközök forráskészletből műveletek. Érdemes használni, és nélkül használhatják azokat azonos értelemben zavart hello eszközöket használhatja. 

hello következő kép bemutatja hogyan összes hello eszközök interakciót hello azonos Azure Resource Manager API-val. hello API kérelmek toohello erőforrás-kezelő szolgáltatás, amely hitelesíti és engedélyezi a hello kérelmek adja át. Erőforrás-kezelő ezután küldi hello kérelmek toohello megfelelő erőforrás-szolgáltatók.

![A Resource Manager kérelmi modellje](./media/resource-group-overview/consistent-management-layer.png)

## <a name="guidance"></a>Útmutatás
hello következő javaslatok segítségével teljes körű kihasználása erőforrás-kezelő a megoldásaival való munka során.

1. Adja meg, és az infrastruktúra hello a a Resource Manager-sablonok deklaratív szintaxisán keresztül, imperatív parancsok helyett központi telepítéséhez.
2. Adja meg az összes telepítési és konfigurációs lépést hello sablon. Nem szükséges manuális lépéseket megadnia a megoldás beállításához.
3. Imperatív parancsok toomanage futtassa az erőforrások, például toostart vagy leállíthat egy alkalmazást vagy gépet.
4. Erőforrások elrendezése hello az azonos életciklussal erőforráscsoportban. Címkék segítségével tetszés szerint rendezheti az erőforrásokat.

A sablonokra vonatkozó javaslatokat talál a [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md) (Az Azure Resource Manager-sablonok létrehozásának ajánlott eljárásai) című cikkben.

A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).

## <a name="resource-groups"></a>Erőforráscsoportok
Az erőforráscsoport meghatározásakor, van néhány fontos tényezőt tooconsider:

1. A csoportban található összes hello erőforrások megosztása kell hello azonos életciklussal. Egyszerre fogja őket telepíteni, frissíteni és törölni. Ha egy erőforrásnak, például egy adatbázis-kiszolgálónak különböző fejlesztési ciklusban a tooexist van szüksége az egy másik erőforráscsoportban kell lennie.
2. Az egyes erőforrások csak egy erőforráscsoportban létezhetnek.
3. Adja hozzá, vagy egy erőforrás tooa erőforráscsoport bármikor eltávolíthatja.
4. Az erőforrásokat áthelyezheti az egyik csoport tooanother erőforráscsoportból. További információkért lásd: [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](resource-group-move-resources.md).
5. Az erőforráscsoportok tartalmazhatnak olyan erőforrásokat, amelyek különböző régiókban találhatók.
6. Erőforráscsoport lehet tooscope hozzáférés-vezérlés felügyeleti műveletekhez használják.
7. Egy erőforrás más erőforráscsoportok erőforrásaival is interakcióba tud lépni. Kommunikáció esetén közös hello két erőforrásokat kapcsolódó, de nem ugyanazt a bejövő hello azonos életciklussal (például webalkalmazások tooa adatbázis kapcsolódás).

Erőforráscsoport létrehozásakor tooprovide egy helyre kell erőforráscsoporthoz. Most felmerülhet Önben a kérdés, hogy „Miért van szüksége egy erőforráscsoportnak helyre? És, ha hello erőforrások lehet különböző helyek mint hello erőforráscsoport, miért nem hello erőforráscsoport helye mindegy minden?" hello erőforráscsoport hello erőforrások metaadatokat tárol. Ezért amikor hello erőforrásnak a helyét adja meg, meg, hogy a metaadatokat tároló. Megfelelőségi okokból, szükség lehet a tooensure, amely az adott tárolja az adatokat.

## <a name="resource-providers"></a>Erőforrás-szolgáltatók
Mindegyik erőforrás-szolgáltató erőforrás-készleteket és műveleteket biztosít az Azure-szolgáltatásokkal való együttműködéshez. Például, ha azt szeretné, toostore kulcsok és titkos, akivel együttműködik hello **Microsoft.KeyVault** erőforrás-szolgáltató. Az erőforrás-szolgáltató nevű erőforrástípust biztosít **tárolók** hello kulcstároló létrehozásához. 

erőforrástípus neve hello hello formátumban van: **{erőforrás-szolgáltató} / {erőforrástípus-}**. Például hello kulcstároló típus **Microsoft.KeyVault/vaults**.

Első lépések az erőforrások telepítésére, mielőtt ettől kezdve hello elérhető erőforrás-szolgáltatók ismeretét. Tudatában hello nevek erőforrás szolgáltatók és erőforrások segítségével meghatározhatja az erőforrások kívánt toodeploy tooAzure. Is kell tooknow hello érvényes helyekre és API-verziók minden erőforrástípus. További információkért lásd az [erőforrás-szolgáltatókat és a típusaikat](resource-manager-supported-services.md) ismertető cikket.

## <a name="template-deployment"></a>Sablonalapú telepítés
A Resource Manager létrehozhat egy sablont (JSON formátumban), amely meghatározza a hello infrastruktúráját és konfigurációját az Azure-megoldás. A sablonok segítségével a megoldás a teljes életciklusa során ismételten üzembe helyezhető, és az erőforrások üzembe helyezése biztosan konzisztens lesz. Amikor létrehoz egy megoldást hello portálról, hello automatikusan tartalmaz egy központi telepítési sablont. Nem rendelkezik toocreate a teljesen új sablont, mert a megoldás hello sablon kezdődnie, és testre szabhatja, toomeet igény szerinti. Meglévő erőforráscsoport sablonjának exportálása hello hello erőforráscsoport aktuális állapotát, vagy az adott példány használt hello sablon megtekintése kérheti le. Megtekintés, hello [exportált sablonok](resource-manager-export-template.md) egy hasznos toolearn hello sablon szintaxissal kapcsolatos van.

toolearn hello sablont, és hogyan hozható létre, hello formátuma kapcsolatban lásd: [az első Azure Resource Manager-sablon létrehozása](resource-manager-create-first-template.md). tooview hello JSON-szintaxis erőforrások esetében, tekintse meg [meghatározhatja az erőforrásokat az Azure Resource Manager sablonokban](/azure/templates/).

Erőforrás-kezelő hello sablont, például más kérelmet dolgoz fel (lásd: hello kép [konzisztens felügyeleti réteg](#consistent-management-layer)). Hello sablon elemez, és konvertálja a szintaxist hello megfelelő erőforrás-szolgáltató REST API-műveleteket. Például ha erőforrás-kezelő megkapja a sablon az erőforrás-definíció következő hello:

```json
"resources": [
  {
    "apiVersion": "2016-01-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "mystorageaccount",
    "location": "westus",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {
    }
  }
]
```

Átalakítja hello definition toohello következő REST API-művelet, amely toohello Microsoft.Storage erőforrás-szolgáltató zajlik:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/mystorageaccount?api-version=2016-01-01
REQUEST BODY
{
  "location": "westus",
  "properties": {
  }
  "sku": {
    "name": "Standard_LRS"
  },   
  "kind": "Storage"
}
```

Hogyan adható meg sablonok és -erőforráscsoportok működik teljesen tooyou és toomanage módját a megoldás. Például telepítheti a három réteg alkalmazást keresztül ugyanazt a sablont tooa egyetlen erőforráscsoportként működnek.

![háromrétegű sablon](./media/resource-group-overview/3-tier-template.png)

De nincs toodefine a teljes infrastruktúrát egyetlen sablonban. Gyakran így logika toodivide célzott konkrét, Célspecifikus sablonok a telepítési követelményeket. Ezeket a sablonokat egyszerűen újból felhasználhatja különböző megoldásokhoz. a megoldás toodeploy, létrehozhat egy fő sablont, amely összekapcsolja az összes szükséges hello sablonok. hello következő kép bemutatja, hogyan toodeploy tartalmazó szülő sablon három három réteg megoldást beágyazott sablonok.

![beágyazott rétegsablon](./media/resource-group-overview/nested-tiers-template.png)

Ha envision a rétegek, hogy külön életciklusának, telepítheti a három réteg tooseparate erőforráscsoportok. Értesítés hello erőforrások továbbra is történhet a többi erőforráscsoport csatolt tooresources.

![rétegsablon](./media/resource-group-overview/tier-templates.png)

A sablonok tervezésével kapcsolatos további javaslatokért lásd: [Minták Azure Resource Manager-sablonok tervezéséhez](best-practices-resource-manager-design-templates.md). A beágyazott sablonokkal kapcsolatos további információkért lásd: [Kapcsolt sablonok használata az Azure Resource Manager eszközben](resource-group-linked-templates.md).

Az Azure Resource Manager tooensure erőforrások jönnek létre a megfelelő sorrendben hello függőségek elemzésével. Ha egy erőforrás egy másik erőforráshoz tartozó értéket használ fel (például egy virtuális gép, amely egy tárfiókot igényel a lemezekhez), akkor beállíthat egy függőséget. További információ: [Függőségek meghatározása az Azure Resource Manager sablonokban](resource-group-define-dependencies.md).

Hello sablon frissítések toohello infrastruktúra is használható. Például erőforrás tooyour megoldás hozzáadása, és hozzáadhat konfigurációs szabályokat a már telepített hello erőforrások. Ha hello sablont létrehoznia egy erőforrást, de az erőforrás már létezik, az Azure Resource Manager egy új eszköz létrehozása helyett frissítést hajt végre. Az Azure Resource Manager frissítések hello meglévő eszköz toohello azonos, mert állapot mintha új lenne.  

Resource Manager bővítményeket forgatókönyvek biztosít, amely nem része a hello beállítása adott szoftver telepítése például további műveletek szükségesek. Ha már használ valamilyen konfigurációfelügyeleti szolgáltatást, mint a DSC, Chef vagy Puppet, bővítmények segítségével folytathatja a munkát az adott szolgáltatással. További információ a virtuális gépi bővítményekről: [A virtuális gépi bővítmények és funkcióik áttekintése](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Végezetül hello sablon hello az alkalmazás forráskódjának részévé válik. Elhelyezheti a forráskódraktárban tooyour, és frissítheti az alkalmazás továbbfejlesztésekor. Visual Studio alkalmazással hello sablon szerkesztése

Az sablon megadása után készen toodeploy hello erőforrások tooAzure áll. Hello parancsok toodeploy hello erőforrások lásd:

* [Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure PowerShell-lel](resource-group-template-deploy.md)
* [Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure parancssori felületével](resource-group-template-deploy-cli.md)
* [Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure Portallal](resource-group-template-deploy-portal.md)
* [Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure Manager REST API-val](resource-group-template-deploy-rest.md)

## <a name="tags"></a>Címkék
A Resource Manager biztosít egy címkézési funkciót, amely lehetővé teszi toocategorize erőforrások tooyour felügyeleti vagy számlázási követelményeinek megfelelően. Címkék használata, ha összetett erőforráscsoport-sablonok és erőforrások több tartalmaz, és kell toovisualize hello módon hello legtöbb logika tooyou az eszközöket. Például elláthat címkével olyan erőforrásokat, amelyek hasonló szerepet töltenek be a szervezet vagy toohello tartozik részleghez. Nélkül hozhat létre a címkék, a szervezet felhasználói több erőforrást, amely lehet, hogy nehéz toolater azonosításához és kezeléséhez. Például előfordulhat, hogy kívánja toodelete egy adott projekt összes hello erőforrást. Ezeket az erőforrásokat nem hello projekt címkével rendelkeznek, azok megkereséséről toomanually lesz. Címkézés lehet egy módja a felesleges költségek tooreduce az előfizetésben. 

Az erőforrásoknak nem kell a hello tooreside azonos erőforrás csoport tooshare egy címkét. Létrehozhat saját címke besorolás tooensure, hogy a szervezete összes felhasználója közös címkék helyett használjon felhasználók ne használjanak véletlenül némileg eltérő címkéket (például "részleg" helyett "osztály").

a következő példa hello címkével tooa virtuális gép jeleníti meg.

```json
"resources": [    
  {
    "type": "Microsoft.Compute/virtualMachines",
    "apiVersion": "2015-06-15",
    "name": "SimpleWindowsVM",
    "location": "[resourceGroup().location]",
    "tags": {
        "costCenter": "Finance"
    },
    ...
  }
]
```

tooretrieve összes hello erőforrás címke értékkel, használja a következő PowerShell-parancsmag hello:

```powershell
Find-AzureRmResource -TagName costCenter -TagValue Finance
```

Vagy hello Azure CLI 2.0 parancs a következő:

```azurecli
az resource list --tag costCenter=Finance
```

Címkézett erőforrásoknak hello Azure-portálon is megtekintheti.

Hello [használati jelentés](../billing/billing-understand-your-bill.md) számára az előfizetés tartalmazza címke neveit és értékeit, ami lehetővé teszi a címkék alapján toobreak kimenő költségek. Címkékkel kapcsolatos további információkért lásd: [Using címkéket tooorganize az Azure-erőforrások](resource-group-using-tags.md).

## <a name="access-control"></a>Hozzáférés-vezérlés
Erőforrás-kezelő lehetővé teszi toocontrol rendelkező hozzáférés toospecific műveletek a szervezet számára. Natív módon integrálja a szerepköralapú hozzáférés-vezérlést (RBAC) hello kezelési platformra, és alkalmazza a hozzáférés-vezérlési tooall szolgáltatásokat az erőforráscsoportban. 

Nincsenek két fő alapelv toounderstand szerepköralapú hozzáférés-vezérlés az használatakor:

* Szerepkör-definíciók – egy engedélycsoportot írnak le, és számos hozzárendelésnél használhatók.
* Szerepkör-hozzárendelések – egy definíciót társítanak egy identitáshoz (felhasználóhoz vagy csoporthoz) egy adott hatókörre vonatkozóan (előfizetés, erőforráscsoport vagy erőforrás). hello hozzárendelés alacsonyabb hatókörök örökli.

Felhasználók toopre által definiált platform- és erőforrás-specifikus szerepköröket is hozzáadhat. Például hello olvasó, amely lehetővé teszi a felhasználók tooview erőforrások nevű előre meghatározott szerepkör előnyeit, de nem tudnák azokat módosítani. A szervezet hozzáférés toohello olvasó szerepkört az ilyen típusú igénylő felhasználókat ad hozzá, és alkalmazza a hello szerepkör toohello előfizetés, az erőforráscsoportot, vagy az erőforrás.

Azure biztosít a következő négy platform szerepkörök hello:

1. Tulajdonos, aki mindent kezelhet, beleértve a hozzáférést is
2. Közreműködő, aki a hozzáférés kivételével mindent kezelhet
3. Olvasó, aki mindent megtekinthet, de módosításokat nem hajthat végre
4. Felhasználói hozzáférés adminisztrátora - kezelhető a felhasználói hozzáférés tooAzure erőforrások

Az Azure számos erőforrás-specifikus szerepkört is biztosít. Ilyenek például a következők:

1. Virtuális gép közreműködő - virtuális gépek kezeléséhez, de nem adnak toothem eléréséhez, és nem tudja kezelni a hello virtuális hálózati vagy tárolási fiók toowhich vannak csatlakoztatva
2. Hálózat közreműködő - az összes hálózati erőforrások kezeléséhez, de nem ad meg hozzáférési toothem
3. Tárolási fiók közreműködői - storage-fiókok kezeléséhez, de nem ad meg hozzáférési toothem
4. SQL Server közreműködője – felügyelheti az SQL-kiszolgálókat és -adatbázisokat, de nem kezelheti a biztonsággal kapcsolatos házirendjeiket
5. Webhely közreműködői - webhelyek kezeléséhez, de nem hello webes tervek toowhich vannak csatlakoztatva

Hello teljes listája a szerepkörök és az engedélyezett műveletek [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md). A szerepköralapú hozzáférés-vezérléssel kapcsolatos további információk: [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md). 

Bizonyos esetekben érdemes toorun kód vagy parancsfájlt, amely hozzáfér az erőforrások, de nem szeretné, hogy toorun e egy felhasználó hitelesítő adatait. Ehelyett szeretné, hogy toocreate szolgáltatás egyszerű neve hello alkalmazás identitást, és rendelje hozzá a megfelelő szerepkört hello hello egyszerű szolgáltatásnév. Erőforrás-kezelő lehetővé teszi a toocreate hello alkalmazás hitelesítő adatait, és a programozott módon hitelesítéshez hello alkalmazás. toolearn szolgáltatásnevekről, létrehozásával kapcsolatban lásd a következő témaköröket:

* [A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure PowerShell toocreate](resource-group-authenticate-service-principal.md)
* [A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure CLI toocreate](resource-group-authenticate-service-principal-cli.md)
* [Portál toocreate Azure Active Directory-alkalmazás és az erőforrások eléréséhez egyszerű szolgáltatás használata](resource-group-create-service-principal-portal.md)

Kifejezetten zárolhatja a kritikus erőforrásokat tooprevent felhasználók törölhessék vagy módosíthassák azokat. További információ: [Erőforrások zárolása az Azure Resource Manager eszközzel](resource-group-lock-resources.md).

## <a name="activity-logs"></a>Tevékenységnaplók
A Resource Manager naplózza az erőforrásokat létrehozó, módosító és törlő műveleteket. Hello tevékenység naplók toofind hibaelhárítása során hiba vagy toomonitor hogyan a szervezet egy felhasználó módosította a következő erőforrás. toosee hello naplókat, válassza ki **tevékenységi naplóit** a hello **beállítások** erőforráscsoport panel. Számos különböző értékeket, beleértve a felhasználó által kezdeményezett hello művelettől szerint szűrheti hello naplókat. Hello tevékenységi naplóit használatával kapcsolatos információkért lásd: [tevékenység megtekintése naplózza toomanage Azure erőforrások](resource-group-audit.md).

## <a name="customized-policies"></a>Testreszabott házirendek
Erőforrás-kezelő lehetővé teszi az erőforrások kezelése testreszabott toocreate házirendeket. házirendek hello típusok között lehetnek különböző forgatókönyveket. Kényszerítheti egy adott elnevezési konvenció használatát az erőforrásokon, korlátozhatja a telepíthető példányok és erőforrások típusát, illetve korlátozhatja azokat az adott típusú erőforrás tárolásához használható régiókat. Erőforrások tooorganize számlázási címkeérték osztályai lehet szükség. Házirendek létrehozása toohelp csökkentheti a költségeket, és biztosítja az egységességet az előfizetésében. 

A házirendeket a JSON használatával határozhatja meg, majd alkalmazhatja őket az előfizetésében vagy az erőforráscsoportban. Házirendek eltérnek a szerepköralapú hozzáférés-vezérlés, mivel azok alkalmazott tooresource típusok.

a következő példa hello egy házirendet, amely címke konzisztencia biztosítja, hogy minden erőforrások közé tartoznak a costCenter címke megadásával jeleníti meg.

```json
{
  "if": {
    "not" : {
      "field" : "tags",
      "containsKey" : "costCenter"
    }
  },
  "then" : {
    "effect" : "deny"
  }
}
```

Rengeteg típusú házirendet hozhat létre. További információkért lásd: [vezérlési hozzáférési és használati feltételei toomanage erőforrások](resource-manager-policy.md).

## <a name="sdks"></a>SDK-k
Az Azure SDK-k több nyelven és többféle platformon elérhetőek. A nyelvi implementációk mindegyike elérhető az ökoszisztéma-csomagkezelőn és a GitHubon keresztül.

Itt találhatóak az Open Source SDK-adattáraink. Szívesen vesszük a visszajelzéseket, a hibabejelentéseket és a lekérési kérelmeket.

* [Azure SDK for .NET](https://github.com/Azure/azure-sdk-for-net)
* [Javához készült Azure felügyeleti könyvtárak](https://github.com/Azure/azure-sdk-for-java)
* [Node.js-hez készült Azure SDK](https://github.com/Azure/azure-sdk-for-node)
* [PHP-hoz készült Azure SDK](https://github.com/Azure/azure-sdk-for-php)
* [Pythonhoz készült Azure SDK](https://github.com/Azure/azure-sdk-for-python)
* [Rubyhoz készült Azure SDK](https://github.com/Azure/azure-sdk-for-ruby)

További információ arról, hogyan használhatók ezek a nyelvek a saját erőforrásaival:

* [Azure .NET-fejlesztőknek](/dotnet/azure/?view=azure-dotnet)
* [Azure Java-fejlesztőknek](/java/azure/)
* [Azure Node.js-fejlesztőknek](/nodejs/azure/)
* [Azure Python-fejlesztőknek](/python/azure/)

> [!NOTE]
> Ha hello SDK hello szükséges funkciók nem biztosít, Ön is meghívhatja toohello [Azure REST API](https://docs.microsoft.com/rest/api/resources/) közvetlenül.
> 
> 

## <a name="next-steps"></a>Következő lépések
* Egy egyszerű bemutatása tooworking sablonok, lásd: [Azure Resource Manager-sablonok exportálása létező erőforrásokból](resource-manager-export-template.md).
* A sablonok létrehozásának részletes ismertetése: [Az első Azure Resource Manager-sablon létrehozása](resource-manager-create-first-template.md).
* toounderstand hello funkciók is használhatja a sablont, lásd: [sablonfüggvények](resource-group-template-functions.md)
* A Visual Studio és a Resource Manager együttes használatával kapcsolatos információ: [Azure erőforráscsoport-sablonok létrehozása és telepítése a Visual Studio alkalmazással](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

Ismertető videó az áttekintésről:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure-Documentation-Shorts/Azure-Resource-Manager-Overview/player]


[powershellref]: https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.2.0/azurerm.resources
