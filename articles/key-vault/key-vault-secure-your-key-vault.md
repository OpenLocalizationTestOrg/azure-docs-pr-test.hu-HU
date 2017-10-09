---
title: "aaaSecure a kulcsot tároló |} Microsoft Docs"
description: "Kulcstartó-hozzáférési engedélyek kezelése tárolók, kulcsok és titkos kulcsok kezeléséhez. A kulcstároló, és hogyan toosecure a kulcsot tároló hitelesítési és engedélyezési modell"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: 84f5fc18142a1ad89babbd11f4f65eca105afc32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-your-key-vault"></a>Kulcstartó védelme
Az Azure Key Vault egy felhőszolgáltatás, mely a titkosítási kulcsokat és titkos kulcsokat (pl. tanúsítványok, kapcsolati karakterláncok, jelszavak) védi az Ön felhőalkalmazásainál. Mivel ezek az adatok bizalmas és a kritikus fontosságú üzleti, toosecure hozzáférés tooyour kulcstárolójának szeretné, hogy csak engedélyezett alkalmazások és a felhasználók kulcstároló. Ez a cikk kulcstároló hozzáférési modell áttekintése, hitelesítési és engedélyezési ismerteti, és ismerteti, hogyan toosecure hozzáférés tookey a felhőalapú alkalmazásokhoz, például a tároló.

## <a name="overview"></a>Áttekintés
Hozzáférés tooa kulcstároló két külön-felületek segítségével lehet szabályozni: felügyeleti vezérlősík és a vezérlősík adatokat. Mindkét síkok megfelelő hitelesítési és engedélyezési szükség egy hívó (a felhasználó vagy egy alkalmazás) kérheti le a hozzáférési tookey tároló előtt. Hitelesítési hello hívó identitását hello hoz létre, amíg engedélyezési határozza meg, hogy milyen műveletek hello hívó tooperform.

Hitelesítésre mind a felügyeleti sík, mind az adatsík az Azure Active Directoryt használja. Engedélyezésre a felügyeleti sík szerepköralapú hozzáférés-vezérlést (RBAC) használ, míg az adatsík kulcstartó-hozzáférési házirendet.

Hello témák rövid áttekintését a következő:

[Hitelesítés az Azure Active Directoryval](#authentication-using-azure-active-directory) -ebből a témakörből megtudhatja, hogyan hívó hitelesítse magát Azure Active Directory tooaccess kulcstároló felügyeleti vezérlősík és adatok vezérlősík keresztül. 

[Felügyeleti sík és adatsík](#management-plane-and-data-plane) – A felügyeleti sík és az adatsík a kulcstartó-hozzáféréséhez használt két hozzáférési síkot jelenti. Minden hozzáférési sík meghatározott műveleteket támogat. Ez a szakasz ismerteti a hello hozzáférés végpontok, műveletek támogatott, és minden vezérlősík vezérlő módszerének eléréséhez. 

[Felügyeleti vezérlősík hozzáférés-vezérlés](#management-plane-access-control) – ebben a szakaszban megnézzük, így a hozzáférés toomanagement vezérlősík műveletek szerepköralapú hozzáférés-vezérlés használatával.

[Adatok vezérlősík hozzáférés-vezérlés](#data-plane-access-control) – Ez a szakasz ismerteti, hogyan toouse kulcstároló hozzáférési házirend toocontrol adatok sík hozzáférést.

[Példa](#example) – Ez a példa bemutatja, hogyan toosetup hozzáférés a kulcstartót tooallow három különböző csoportjai (biztonsági csoport, a fejlesztők/operátorok és auditor) tooperform meghatározott feladatok toodevelop, kezelni és megfigyelni a kérelmet az Azure-ban .

## <a name="authentication-using-azure-active-directory"></a>Hitelesítés az Azure Active Directory használatával
Azure-előfizetés hozzon létre egy kulcstartót esetén automatikusan társított hello előfizetés Azure Active Directory-bérlő. Összes hívó (a felhasználók és az alkalmazások) kell lennie a bérlő tooaccess regisztrált ezen a kulcstartón. Egy alkalmazás vagy a felhasználó Azure Active Directory tooaccess kulcstároló kell hitelesíteni. Ez vonatkozik tooboth felügyeleti vezérlősík és adatok vezérlősík hozzáférést. Mindkét esetben kétféle módon férhet hozzá az alkalmazás a kulcstartóhoz:

* **felhasználó + alkalmazás-hozzáférés** – ez általában olyan alkalmazások esetén történik, amelyek bejelentkezett felhasználó nevében férnek hozzá a kulcstartóhoz. Ilyen típusú hozzáférés például az Azure PowerShell vagy az Azure Portal. Toogrant hozzáférés toousers két módja van: egyirányú toogrant hozzáférés toousers, melyet bármely alkalmazásból kulcstároló eléréséhez, és más módon hello toogrant a felhasználói hozzáférés tookey tároló csak akkor, amikor egy adott alkalmazás (hivatkozott tooas összetett identitás) használnak. 
* **csak alkalmazás hozzáférés** – az alkalmazások, hogy démon szolgáltatások futtatása stb hello alkalmazás azonosítóját kapnak a feladatok a háttérben hozzáférési toohello kulcs tárolóban.

Mindkét típusú alkalmazások hello alkalmazás Azure Active Directoryval bármely hello használatával hitelesíti [támogatott hitelesítési módszerek](../active-directory/active-directory-authentication-scenarios.md) , és beszerzi a jogkivonatot. Használt hitelesítési módszert hello alkalmazás típusától függ. Majd hello alkalmazás használja a jogkivonatot, és elküldi a REST API-kérés tookey tárolóban. Felügyeleti vezérlősík hozzáférés hello esetén kérelmek Azure Resource Manager-végpont rendszer továbbít. Adatok vezérlősík elérésekor hello alkalmazások közvetlenül tooa kulcstároló végpont kiszolgálóhoz. További információt olvashat hello [egész hitelesítési folyamat](../active-directory/active-directory-protocols-oauth-code.md). 

hello erőforrás neve, amelynek hello alkalmazás kér egy token eltér attól függően, hogy hello alkalmazás felügyeleti vezérlősík vagy adatok vezérlősík elérése. Ezért hello erőforrás nevének megadása vagy felügyeleti vezérlősík vagy adatok vezérlősík végpont hello táblázatban egy későbbi szakasz ismerteti, attól függően, hogy hello Azure környezetben.

A hitelesítési tooboth egy egyetlen mechanizmussal rendelkezik felügyelete és az adatok vezérlősík rendelkezik a saját előnyei:

* A szervezetek központilag szabályozhatja a szervezetek hozzáférés tooall kulcstárolójának
* Ha egy felhasználó kilép, azonnal megszakad a hozzáférési tooall kulcstárolójának hello szervezet
* A szervezetek szabhatja testre a hitelesítés az Azure Active Directoryban (például, amely lehetővé teszi a többtényezős hitelesítést nagyobb biztonság) hello-beállítások segítségével

## <a name="management-plane-and-data-plane"></a>Felügyeleti sík és adatsík
Az Azure Key Vault az Azure Resource Manager-alapú üzemi modellen keresztül elérhető Azure-szolgáltatás. Kulcstartó létrehozásakor Ön virtuális tárolóhoz jut, és benne egyéb objektumokat – például kulcsokat, titkos kulcsokat és tanúsítványokat – hozhat létre. Ezután a kulcstartót használó felügyeleti vezérlősík és vezérlősík tooperform adott műveletei érhető el. Felügyeleti vezérlősík objektumfelület használt toomanage a kulcsot tároló magát, például a létrehozása, törlése, kulcstároló tulajdonságainak frissítése és az adatok vezérlősík hozzáférési házirendek beállítása. Adatok vezérlősík adapter használt tooadd, törlése, módosítása, és hello kulcsokat, a titkos kulcsok és a tanúsítványok a key vaultban tárolt használja.

hello vezérlősík és az adatok vezérlősík felületek (lásd a táblázatot) különböző végpontokon keresztül érhetők el. hello táblázat második oszlopában hello ismerteti ezeket a végpontokat különböző Azure-környezetek hello DNS-neveit. hello harmadik oszlop hello műveletek hajthatók végre a minden egyes hozzáférés vezérlősík ismerteti. Minden hozzáférési síknak megvan a saját hozzáférés-vezérlési mechanizmusa is: a felügyeleti sík hozzáférés-vezérlése az Azure Resource Manager szerepköralapú hozzáférés-vezérlése (RBAC) használatával van beállítva, míg az adatsík hozzáférés-vezérlésének beállítása a kulcstartó hozzáférési házirendjének használatával történik.

| Hozzáférési sík | Hozzáférés végpontjai | Műveletek | Hozzáférés-vezérlési mechanizmus |
| --- | --- | --- | --- |
| Felügyeleti sík |**Globálisan:**<br> management.azure.com:443<br><br> **Azure China:**<br> management.chinacloudapi.cn:443<br><br> **Amerikai Egyesült Államok kormánya által használt Azure:**<br> management.usgovcloudapi.net:443<br><br> **Azure Germany:**<br> management.microsoftazure.de:443 |Kulcstartó létrehozása/olvasása/frissítése/törlése <br> Kulcstartó hozzáférési házirendjeinek beállítása<br>Címkék beállítása kulcstartóhoz |Az Azure Resource Manager szerepköralapú hozzáférés-vezérlése (RBAC) |
| Adatsík |**Globálisan:**<br> &lt;tároló-neve&gt;.vault.azure.net:443<br><br> **Azure China:**<br> &lt;tároló-neve&gt;.vault.azure.cn:443<br><br> **Amerikai Egyesült Államok kormánya által használt Azure:**<br> &lt;tároló-neve&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Germany:**<br> &lt;tároló-neve&gt;.vault.microsoftazure.de:443 |Kulcsok: Visszafejtés, Titkosítás, UnwrapKey, WrapKey, Ellenőrzés, Aláírás, Beolvasás, Listázás, Frissítés, Létrehozás, Importálás, Törlés, Biztonsági mentés, Visszaállítás<br><br> Titkos kulcsok: Beolvasás, Listázás, Beállítás, Törlés |Kulcstartó hozzáférési házirendje |

hello felügyeleti vezérlősík és az adatok vezérlősík hozzáférés-vezérlést egymástól függetlenül működnek. Például ha toogrant kulcstároló egy alkalmazás-hozzáférési toouse kulcs, csak kell toogrant adatok vezérlősík hozzáférési engedélyeik kulcstároló hozzáférési szabályzatainak, és nem felügyeleti vezérlősík hozzáférés szükséges ehhez az alkalmazáshoz. És ezzel szemben, ha tudni szeretné egy felhasználó toobe tooread tulajdonságok és címkék tároló, de nem rendelkeznek hozzáféréssel tookeys, titkos kulcsokat, vagy tanúsítványok, is meg lehet adni a felhasználó, "read" hozzáférés RBAC és egyetlen hozzáférési toodata vezérlősík szükség.

## <a name="management-plane-access-control"></a>Felügyeleti sík hozzáférés-vezérlése
hello felügyeleti vezérlősík maga hello kulcstároló-műveletek áll. Például létrehozhat vagy törölhet egy kulcstartót. Előfizetésben megkaphatja a tárolók listáját. Kulcstartó tulajdonságait (például Termékváltozat, a címkék) beolvasása, és állítsa be a kulcstároló hozzáférési házirendeket, amelyek vezérlik hello felhasználók és a kulcsok és titkos hello a key vaultban elérhető alkalmazásokat. A felügyeleti sík hozzáférés-vezérlése RBAC használatával történik. Lásd: hello kulcstároló felügyeleti vezérlősík hello tábla előző szakaszban végezhető műveletek teljes listájának. 

### <a name="role-based-access-control-rbac"></a>Szerepköralapú hozzáférés-vezérlés (RBAC)
Minden Azure-előfizetés Azure Active Directoryval rendelkezik. Felhasználók, csoportok és alkalmazások a könyvtárból toomanage az Azure-erőforrások hello Azure-előfizetés hello Azure Resource Manager üzembe helyezési modellben használó engedélyezhetők. Ez a hozzáférés-vezérlés típus hivatkozott tooas szerepköralapú hozzáférés-vezérlést (RBAC). toomanage ez fér hozzá, használhatja a hello [Azure-portálon](https://portal.azure.com/), hello [Azure CLI-eszközei](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), vagy hello [Azure Resource Manager REST API-k](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Hello Azure Resource Manager modellt a kulcstartót alkalmazásban létrehozott egy erőforrás csoport és a vezérlés hozzáférési toohello felügyeleti vezérlősík ezen a kulcstartón az Azure Active Directory használatával. Például biztosíthat a felhasználók vagy a csoport lehetőséget toomanage kulcstárolójának egy adott erőforráscsoportban.

Hozzáférés toousers, csoportok és alkalmazások egy adott hatókörhöz megadásához, megfelelő RBAC-szerepkörök hozzárendelése. Például toogrant tooa felhasználói toomanage kulcstárolójának rendelne egy előre meghatározott szerepkör "key vault közreműködői" toothis felhasználó egy adott hatókörhöz érhető el. hello hatókör ebben az esetben lenne előfizetés, egy erőforráscsoportot, vagy csak adott kulcstároló. Egy előfizetés szintjén szerepkörrel tooall erőforráscsoport-sablonok és erőforrások adott előfizetésen belül érvényes. Egy erőforrás-csoportok szintjén szerepkörrel tooall erőforrások az erőforráscsoport vonatkozik. Egy adott erőforráshoz hozzárendelt szerepkör csak akkor toothat erőforrás. Számos előre meghatározott szerepkör (lásd: [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md)), és ha hello előre definiált szerepkörök nem igényei saját szerepköröket is meghatározhat.

> [!IMPORTANT]
> Vegye figyelembe, hogy ha egy felhasználó közreműködői engedélyekkel (RBAC) tooa kulcstároló felügyeleti vezérlősík, ő biztosíthat közte hozzáférés toodata vezérlősík, kulcstároló hozzáférési házirendet, amely szabályozza a hozzáférést toodata vezérlősík beállításával. Ezért ajánlott tootightly vezérlő rendelkező "Közreműködői" hozzáférési tooyour kulcstárolójának tooensure csak arra jogosult személyek eléréséhez, és a kulcstárolók, a kulcsokat, a titkos kulcsok és a tanúsítványok kezeléséhez.
> 
> 

## <a name="data-plane-access-control"></a>Adatsík-hozzáférés vezérlése
hello kulcstároló adatok vezérlősík-műveletek hatása a kulcstároló, például a kulcsokat, a titkos kulcsok és a tanúsítványok hello objektumok áll.  Olyan, kulcsokkal kapcsolatos műveleteket foglal magába, mint a kulcsok létrehozása, importálása, frissítése, listázása, biztonsági mentése és visszaállítása, valamint kriptográfiai műveleteket, mint például a kulcsok aláírása, ellenőrzése, titkosítása, visszafejtése, be- és kicsomagolása, a kulcsok címkéinek és egyéb attribútumainak beállítása. Hasonlóképpen, titkos kulcsok esetén magába foglalja a beolvasást, beállítást, listázást és törlést.

Az adatsík-hozzáférés a kulcstartó hozzáférési házirendjeinek beállításán keresztül biztosítható. A felhasználó, csoport vagy egy alkalmazás felügyeleti vezérlősík vonatkozó egy kulcstartót toobe képes tooset hozzáférési házirendek adott kulcstároló-közreműködői jogokat (RBAC) kell rendelkeznie. Egy felhasználó, csoport vagy alkalmazás lehet adni a kulcsok vagy titkos kulcsok a kulcstároló hozzáférési tooperform műveleteket. Kulcstároló-támogatás too16 hozzáférés házirend-bejegyzések kulcstároló fel. Hozzon létre egy Azure Active Directory biztonsági csoportot, és adja hozzá a felhasználók toothat csoport toogrant adatok vezérlősík hozzáférés tooseveral felhasználók tooa kulcstároló.

### <a name="key-vault-access-policies"></a>Kulcstartó-hozzáférési házirendek
Kulcstároló hozzáférési szabályzatainak külön-külön adjon engedélyeket tookeys, a titkos kulcsok és a tanúsítványok. Például adhat egy felhasználói hívóbetűk tooonly, de nincs engedélye a titkos kulcsok. Azonban engedélyek tooaccess kulcsok vagy titkos kulcsokat vagy tanúsítványokat is hello tároló szintjén. Másként megfogalmazva, a kulcstartó-hozzáférési házirend nem támogatja az objektumszintű engedélyeket. Használhat [Azure-portálon](https://portal.azure.com/), hello [Azure CLI-eszközei](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), vagy hello [kulcstároló felügyeleti REST API-k](https://msdn.microsoft.com/library/azure/mt620024.aspx) tooset hozzáférési házirendek a kulcstároló.

> [!IMPORTANT]
> Vegye figyelembe, hogy a kulcstároló hozzáférési szabályzatainak hello tároló szintjén alkalmazásra. Például megadásakor a felhasználó engedélye toocreate és a delete billentyűt, ő hajthat végre e műveleteket minden kulcsok, hogy a key vaultban.
> 
> 

## <a name="example"></a>Példa
Tegyük fel, hogy Ön egy olyan alkalmazást fejleszt, amely az SSL-hez tanúsítványt használ, adattárolásra Azure-tárolást, az aláírási műveletekhez pedig RSA 2048 bites kulcsot. Tegyük fel azt is, hogy ez az alkalmazás virtuális gépen fut (vagy virtuálisgép-méretezési csoportban). A kulcstároló toostore összes alkalmazás titkok és kulcstároló toostore hello bootstrap tanúsítvány használata az Azure Active Directoryval hello alkalmazás tooauthenticate által használt hello is használhatja.

Igen Ez minden hello kulcsok és titkos toobe key vaultban tárolt összegzését.

* **SSL-tanúsítvány** – az SSL-hez használatos
* **Biztonságitár-kulcs** -tooget hozzáférési tooStorage fiók használható
* **RSA 2048 bites kulcs** – az aláírási műveletekhez használatos
* **A rendszerindítási tanúsítvány** -aláíráshoz használt tooauthenticate tooAzure Active Directory, tooget tookey tároló toofetch hello tárolási hívóbetűt és hello RSA-kulcs használata.

Most adjuk meg hello személyek kezelését, telepítése és az alkalmazás naplózási. Ebben a példában három szerepkört használunk.

* **Biztonsági csoport** – ezek általában olyan informatikai munkatársak hello CSO (fő biztonsági tisztviselő) hello "részleg", vagy ezzel egyenértékű, felelős hello megfelelő van őrizve a titkos kulcsok, mint az SSL-tanúsítványok, kapcsolati karakterláncainak aláírásra használt RSA-kulcsok adatbázisok, a tárfiók kulcsait.
* **A fejlesztők/operátorok** -ezek a hello segítsen az alkalmazás fejlesztése és majd telepítenie kell azt az Azure-ban. Általában nincsenek tagja hello biztonsági csoportnak, és ezért azok nem rendelkezhet hozzáférés tooany bizalmas adatokat, például az SSL-tanúsítványokat, RSA-kulcsok, de hello alkalmazás, hogy hozzáférési toothose kell rendelkeznie.
* **Ellenőrök** – Ez általában olyan személyek, hello fejlesztők és általános informatikai munkatársak különítve különböző készlete. Felelősségi tooreview megfelelő használata és karbantartása a tanúsítványok, a kulcsok, stb., és biztosítsa az adatok biztonsági követelményeket. 

Van egy további szerepkör, amely alkalmazás, de a megfelelő ide toobe említett külső hello hatókörét, és, amely lenne hello előfizetés (vagy az erőforráscsoport) rendszergazdájához. Előfizetési rendszergazda állít be hello biztonsági csoport kezdeti hozzáférési engedélyeit. Itt azt feltételezzük, hogy hello előfizetés rendszergazda adott hozzáférési toohello biztonsági csoport tooa erőforráscsoport, amelyben az összes hello az alkalmazás lakóhelye szükséges erőforrásokat.

Most nézzük meg, milyen műveletek szerepkörönként hajt végre, az alkalmazás hello környezetében.

* **Biztonsági csapat**
  * Kulcstartók létrehozása
  * Kulcstartónaplózás bekapcsolása
  * Kulcsok/titkos kulcsok hozzáadása
  * Kulcsok biztonsági másolatának létrehozása vészhelyreállításhoz
  * Kulcstároló hozzáférési házirend toogrant engedélyek toousers és az alkalmazások tooperform műveleteket beállítása
  * Kulcsok/titkos kulcsok időnkénti összegzése
* **Fejlesztők/operátorok**
  * Hivatkozások toobootstrap és SSL Tanúsítványos (ujjlenyomatok) biztonságitár-kulcs (titkos kulcs URI) és az aláírási kulcs (kulcs URI-val) a biztonsági csoportból
  * A kulcsokhoz és titkos kulcsokhoz programozott módon hozzáférő alkalmazás fejlesztése és üzembe helyezése
* **Ellenőrök**
  * Tekintse át a használati naplók tooconfirm megfelelő kulcs vagy titkos kulcs használatát és az adatok biztonsági előírásoknak való megfelelés

Most nézzük meg, milyen hozzáférési engedélyek tookey tároló szükséges szerepköröket (és hello alkalmazás) tooperform hozzárendelt feladataikat. 

| Felhasználói szerepkör | Felügyeleti sík engedélyei | Adatsík engedélyei |
| --- | --- | --- |
| Biztonsági csapat |Kulcstartó-közreműködő |Kulcsok: biztonsági mentése, létrehozása, törlése, beolvasása, importálása, listázása, visszaállítása <br> Titkos kulcsok: összes |
| Fejlesztők/operátor |Kulcstároló telepítése engedéllyel, így hello virtuális gépeket, hogy titkos kulcsok beolvasni a hello kulcstároló |None |
| Ellenőrök |None |Kulcsok: listája<br>Titkos kulcsok: listája |
| Alkalmazás |None |Kulcsok: aláírása<br>Titkos kulcsok: beolvasása |

> [!NOTE]
> Ellenőrök kell listában engedély kulcsok és titkos, így azok vizsgálhatja meg attribútumok kulcsok és titkos kulcsok nem kibocsátott hello naplófájlokban, például a címkék, aktiválás és lejárati ideje.
> 
> 

Engedély tookey tárolóban, mellett minden három szerepkört is kell férhetnek hozzá tooother erőforrásokhoz. Például toobe képes toodeploy virtuális gépek (vagy a Web Apps stb.) Fejlesztői/operátorok kell "Közreműködői" hozzáférési toothose erőforrástípusok esetében is. Ellenőrök olvasási hozzáférés toohello tárfiók hello kulcstároló naplóit tároló kell.

Mivel ez a cikk hello fókusz adminisztrálásakor hozzáférés tooyour kulcstároló, azt csak hello toothis témakör vonatkozó megfelelő részei bemutatják, és hagyja ki a tanúsítványok telepítését, a kulcsok és titkos elérése programozott módon stb kapcsolatos részleteket. Ezeket a részleteket máshol már tárgyaltuk. Kulcstároló tooVMs tárolt tanúsítványok telepítésével kapcsolatban lásd a [blogbejegyzés](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/), és nincs [példakód](https://www.microsoft.com/download/details.aspx?id=45343) érhető el, amely bemutatja, hogyan toouse bootstrap tanúsítvány tooauthenticate tooAzure AD tooget hozzáférés tookey tárolóban.

Hello hozzáférési engedélyeket a legtöbb Azure-portálon is meg kell adni, de szükség lehet az Azure PowerShell (vagy Azure CLI) toouse tooachieve hello toogrant részletes engedélyeket szükséges eredménye. 

a következő PowerShell-kódtöredékek hello használata:

* hello Azure Active Directory-rendszergazda hozott létre hello három szerepkörök, nevezetesen Contoso biztonsági csoportja, Contoso alkalmazást Devops, Contoso alkalmazást ellenőrök megfelelő biztonsági csoportokat. hello rendszergazda felhasználók toohello csoportok tartoznak is hozzá van adva.
* **ContosoAppRG** hello erőforráscsoportban van, ahol minden hello erőforrások vannak. **contosologstorage** hello naplók tárolására van. 
* Kulcstároló **ContosoKeyVault** és a kulcstároló naplóit használt tárfiók **contosologstorage** hello kell lennie ugyanazon Azure-beli hely

Első hello előfizetési rendszergazda rendeli hozzá a "kulcsot tároló közreműködői" és "felhasználói hozzáférés adminisztrátora" szerepkörök toohello biztonsági csoportja. Ez lehetővé teszi, hogy a hello biztonsági csoport toomanage tooother erőforrások eléréséhez, és hello erőforráscsoport ContosoAppRG kulcstárolójának kezelése.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

hello alábbi parancsfájl bemutatja, hogyan hello biztonsági csoportja is hozzon létre egy kulcstartót naplózás beállítása és egyéb szerepköröket és hello alkalmazás hozzáférési engedélyeinek beállítása. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $devopsrole

# Assign this newly defined role tooDev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

egyéni szerepkör hello definiált, csak hozzárendelhető toohello előfizetés ahol hello ContosoAppRG erőforráscsoportban jön létre. Hello azonos egyéni szerepkörök használandó más projektek más előfizetésekben, ha azok hatókör lehet hozzá legalább egy előfizetésre.

egyéni szerepkör-hozzárendelés hello hello fejlesztők/operátorok hello "telepítése/action" engedélyt az hatókörön belüli toohello erőforráscsoportban. Ezzel a módszerrel csak hello virtuális gépek létrehozása "ContosoAppRG" kap hello titkos kulcsokat (SSL-tanúsítványt és a rendszerindítási cert) hello erőforráscsoportban. A fejlesztői/ops csapat egyik tagja egy másik erőforráscsoportban hozható létre virtuális gépek nem lesz képes tooget ezeknek a kulcsoknak akkor is, ha azok tudtak hello titkos kulcs URI-azonosítók.

Ez a példa egy egyszerű forgatókönyvet mutat be. Lehet, hogy a valós forgatókönyvekben összetettebb és tooadjust engedélyek tooyour kulcstároló a igények alapján szükség lehet. Például a fenti példában feltételezzük, hogy a biztonsági csoport biztosít hello kulcsot és titkos hivatkozik (URI-k és ujjlenyomatok), hogy a fejlesztők/operátorok csoport kell tooreference az alkalmazásokban. Emiatt, nem kell toogrant fejlesztők/operátorok vezérlősík adatok elérését. Fontos megjegyezni, hogy ebben a példában legfőképpen a kulcstartó biztonságossá tételére összpontosítunk. Hasonló figyelembe kell venni toosecure [a virtuális gépek](https://azure.microsoft.com/services/virtual-machines/security/), [tárfiókok](../storage/common/storage-security-guide.md) és más Azure-erőforrások túl.

> [!NOTE]
> Megjegyzés: Ez a példa bemutatja, hogyan lesz zárolva a kulcstartó-hozzáférés üzemi környezetben. hello fejlesztők számára a saját előfizetés vagy resourcegroup rendelkezik teljes körű engedélyekkel toomanage a tárolóhoz, a virtuális gépek és a tárfiók amelyen ők fejlesztik ki az alkalmazás hello kell rendelkeznie.
> 
> 

## <a name="resources"></a>Erőforrások
* [Azure Active Directory szerepköralapú hozzáférés-vezérlése](../active-directory/role-based-access-control-configure.md)
  
  Ez a cikk ismerteti a hello Azure Active Directory szerepköralapú hozzáférés-vezérlés és annak működéséről.
* [RBAC: Beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md)
  
  Ez a cikk részletek minden hello beépített szerepkörök RBAC érhető el.
* [A Resource Manager-alapú és a klasszikus üzembe helyezés ismertetése](../azure-resource-manager/resource-manager-deployment-model.md)
  
  Ez a cikk azt ismerteti, hello Resource Manager üzembe helyezési és a klasszikus üzembe helyezési modellel, és hello erőforrás-kezelő és az erőforrás-csoportok használatának előnyei hello ismerteti
* [Szerepköralapú hozzáférés-vezérlés kezelése az Azure PowerShell-lel](../active-directory/role-based-access-control-manage-access-powershell.md)
  
  Ez a cikk azt ismerteti, hogyan toomanage szerepköralapú hozzáférés az Azure PowerShell
* [Szerepköralapú hozzáférés-vezérlés a REST API hello kezelése](../active-directory/role-based-access-control-manage-access-rest.md)
  
  Ez a cikk bemutatja, hogyan toouse hello REST API toomanage RBAC.
* [Szerepköralapú hozzáférés-vezérlés az Ignite-tól a Microsoft Azure számára](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  Ez az a hivatkozás tooa a hello 2015-ös MS ignite-on konferencia a Channel 9 videót. Ebben a munkamenetben, azok szolgáltatással kapcsolatban hozzáférhet a felügyeleti és jelentéskészítési lehetőségeket az Azure-ban, és vizsgálja meg körül tooAzure előfizetések az Azure Active Directoryval hozzáférés biztosítása érdekében ajánlott eljárások.
* [Engedélyezi a hozzáférést tooweb alkalmazások OAuth 2.0 és az Azure Active Directory használatával](../active-directory/active-directory-protocols-oauth-code.md)
  
  A cikk az Azure Active Directoryval történő hitelesítés teljes OAuth 2.0-s folyamatát ismerteti.
* [Kulcstartókezelési REST API-k](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Ez a dokumentum hello referencia hello REST API-k toomanage tároló programozott módon, a kulcs beállítása a kulcstároló hozzáférési házirend.
* [kulcstartó REST API-k](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  Hivatkozás tookey tárolói REST API referenciadokumentációt tartalmaz.
* [Kulcshozzáférés-vezérlés](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  Hivatkozás tooSecret access control ismertető dokumentációjában.
* [Titkoskulcs-hozzáférés vezérlése](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  Hivatkozás tooKey access control ismertető dokumentációjában.
* Kulcstartó-hozzáférési házirend [beállítása](https://msdn.microsoft.com/library/mt603625.aspx) és [eltávolítása](https://msdn.microsoft.com/library/mt619427.aspx) PowerShell használatával
  
  PowerShell parancsmagok toomanage kulcstároló hozzáférési házirend hivatkozások tooreference dokumentációját.

## <a name="next-steps"></a>Következő lépések
A rendszergazdáknak szóló bevezető oktatóanyag a [Bevezetés az Azure Key Vault használatába](key-vault-get-started.md) című cikkben érhető el.

A kulcstartó használatának naplózásával kapcsolatos további információkért tekintse meg [Az Azure Key Vault naplózása](key-vault-logging.md) című cikket.

A kulcsok és a titkos kulcsok Azure Key Vaulttal történő használatával kapcsolatos további információkért tekintse meg az [About Keys and Secrets](https://msdn.microsoft.com/library/azure/dn903623.aspx) (Kulcsok és titkos kulcsok) című cikket.

Ha kérdései a kulcstároló, látogasson el a hello [az Azure key vault fórumok](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)

