---
title: "aaaAzure Key Vault fejlesztői útmutatója"
description: "A fejlesztők a Azure Key Vault toomanage titkosítási kulcsok hello Microsoft Azure-környezeten belül."
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: bruceper
ms.openlocfilehash: 631cea1315964cd0b97e8b2cf3311754230fb801
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-developers-guide"></a>Az Azure Key Vault fejlesztői útmutatója

Key Vault lehetővé teszi a toosecurely hozzáférés bizalmas adatok az alkalmazások belül:

- Kulcsok és titkos védettek, saját kezűleg toowrite hello kód nélkül, és könnyen képes toouse őket az alkalmazásokból.
- Képes toohave saját az ügyfelek és a saját kulcsaikat kezelését biztosító hello core szoftverfunkciók összpontosíthat. Így az alkalmazások fog nem saját hello felelősséget és esetleges felelősségre vonást az ügyfelek bérlői kulcsok és titkos kulcsok.
- Az alkalmazás kulcsok aláírására használható, és titkosítási még tartja hello kulcskezelés külső az alkalmazásról, így a megoldás toobe felel meg egy földrajzilag elosztott alkalmazást.
- Hello 2016 szeptemberétől kiadás a Key Vault, az alkalmazások használhatják a Key Vault [tanúsítványok](https://docs.microsoft.com/rest/api/keyvault/certificate-operations). További információkért lásd: [kulcsokat, a titkos kulcsok és a tanúsítványok](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates).

Az Azure Key Vault további általános információkért lásd: [Mi az Key Vault](key-vault-whatis.md).

## <a name="public-previews"></a>Nyilvános előzetes verziójú funkciók

Rendszeres időközönként egy új Key Vault szolgáltatás nyilvános előzetes kiadás azt. Próbálja ki ezeket, és ossza meg velünk mit gondol keresztül azurekeyvault@microsoft.com, a visszajelzési e-mail címet.

### <a name="storage-account-keys---july-10-2017"></a>Tárfiókkulcsok - 2017. július 10.

>[!NOTE]
>A frissítés az Azure Key Vault csak hello **Tárfiókkulcsok** szolgáltatás egyelőre.

Ebben az előzetes verzióban az új Tárfiókkulcsok szolgáltatást tartalmaz, ezek kapcsolódási; keresztül érhető el [.NET / C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault/), [REST](https://docs.microsoft.com/rest/api/keyvault/) és [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/). 

Hello új Tárfiókkulcsok szolgáltatásról további információkért lásd: [Azure Key Vault tárolási fiók kulcsok áttekintése](key-vault-ovw-storage-keys.md).

## <a name="videos"></a>Videók

Ez a videó bemutatja, hogyan toocreate saját kulcsot tároló, és hogyan toouse hello "Hello Key Vault" mintaalkalmazás le.

- [Key Vault fejlesztői – gyors üzembe helyezési útmutató](https://channel9.msdn.com/Blogs/Azure/Azure-Key-Vault-Developer-Quick-Start/player)

A fenti videó erőforrások:

- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Az Azure Key Vault mintakód](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

## <a name="creating-and-managing-key-vaults"></a>Hozza létre és kezelje a kulcstárolók

Mielőtt elkezdene dolgozni az Azure Key Vault a kódban, hozhat létre és REST, a Resource Manager-sablonok, a PowerShell vagy a parancssori felületen keresztül tárolók kezelése, a következő cikkek hello leírtak szerint:

- [Létre és kezelheti a többi kulcstárolójának](https://docs.microsoft.com/rest/api/keyvault/)
- [Létrehozása és kezelése a PowerShell-lel kulcstárolójának](key-vault-get-started.md)
- [Létrehozása és kezelése a parancssori felület kulcstárolójának](key-vault-manage-with-cli2.md)
- [Hozzon létre egy kulcstartót, és adja hozzá a titkos kulcs Azure Resource Manager-sablonok segítségével](../azure-resource-manager/resource-manager-template-keyvault.md)

> [!NOTE]
> Kulcstárolójának műveleteket AAD keresztül hitelesíti és engedélyezi a Key Vault saját hozzáférési házirend, egy tároló definiált keresztül.

## <a name="coding-with-key-vault"></a>A kulcstároló kódolása

Key Vault felügyeleti rendszer t használó programozók számára hello áll több, mint hello foundation többi. Hello REST-felületen keresztül összes kulcstárolójának erőforrást érhetők el; kulcsok, a titkos kulcsok és a tanúsítványokat. [Kulcs tárolói REST API-referencia](https://docs.microsoft.com/rest/api/keyvault/). 

### <a name="supported-programming-languages"></a>Támogatott programozási nyelveket

#### <a name="net"></a>.NET

- [.NET API-es hivatkozási Key vault](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault) 

A .NET SDK hello hello 2.x verzióján további információkért lásd: hello [kibocsátási megjegyzéseket](key-vault-dotnet2api-release-notes.md).

#### <a name="java"></a>Java

- [Java SDK kulcstároló](https://docs.microsoft.com/java/api/com.microsoft.azure.keyvault)

#### <a name="nodejs"></a>Node.js

A node.js hello tároló felügyeleti API és hello tároló objektum API külön. Tároló kulcskezelés lehetővé teszi, hogy létrehozása és frissítése a kulcstartót. Key Vault műveletek API végzett munka esetén van tároló objektumok, például; kulcsok, a titkos kulcsok és a tanúsítványokat. 

- [A Key Vault felügyeleti node.js API-hivatkozás](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest/)
- [Key Vault műveletekhez node.js API-referencia](http://azure.github.io/azure-sdk-for-node/azure-keyvault/latest/) 

### <a name="quick-start"></a>Első lépések

- [Kulcstároló létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
- [Ismerkedés a Key Vault a Node.js-ben](https://azure.microsoft.com/en-us/resources/samples/key-vault-node-getting-started/)

### <a name="code-examples"></a>Kódpéldák

Key Vault használatával az alkalmazások teljes példákért lásd:

- [Az Azure Key Vault-Kódminták](http://www.microsoft.com/download/details.aspx?id=45343) -.NET mintaalkalmazás *HelloKeyVault* és az Azure web service példa. 
- [Használja az Azure Key Vault-webalkalmazások](key-vault-use-from-web-application.md) -oktatóanyag toohelp megtudhatja, hogyan toouse Azure kulcsot tároló egy webalkalmazás az Azure-ban. 

## <a name="how-tos"></a>Használati útmutatók

hello következő cikkeket és forgatókönyvek feladatspecifikus útmutatást nyújtanak az Azure Key Vault használatához:

- [Kulcstároló Bérlőazonosító módosítása után előfizetés áthelyezése](key-vault-subscription-move-fix.md) – az Azure-előfizetéshez a bérlő egy tootenant B-védelemről, a meglévő kulcstárolójának hello tag (a felhasználók és az alkalmazások) bérlőben b javítás Ez az útmutató használatával érhetők el.
- [Tűzfal mögött található kulcstároló eléréséhez](key-vault-access-behind-firewall.md) -tooaccess egy kulcsot tároló a kulcstartót ügyfél alkalmazás igényeinek toobe képes tooaccess a különböző funkciók több végpontok közé.
- [Hogyan tooGenerate és Transfer HSM-Protected kulcsok Azure Key vault](key-vault-hsm-protected-keys.md) – Ez segít a tervezése. létrehozni, majd a saját HSM által védett kulcsokat toouse az Azure Key Vault át.
- [Hogyan toopass biztonságos üzembe helyezése során (például a jelszavak) értékek](../azure-resource-manager/resource-manager-keyvault-parameter.md) – Ha toopass (például jelszót) biztonságos értéket paraméterként központi telepítése során kell ezt az értéket tárolhatja az Azure Key Vault- és referenciainformációkat hello értékének más egy titkos kulcsként Resource Manager-sablonok.
- [Hogyan toouse Key Vault az SQL Server a bővíthető kulcskezelési](https://msdn.microsoft.com/library/dn198405.aspx) -hello SQL Server-összekötő az Azure Key Vault lehetővé teszi, hogy az SQL Server és SQL-az-a-VM tooleverage hello Azure Key Vault szolgáltatás bővíthető Key Management (EKM)-szolgáltatóként tooprotect a titkosítási kulcsok alkalmazások hivatkozás; Átlátható adattitkosítás, a biztonsági másolat titkosításához és oszlop a blokkszintű titkosítás.
- [Hogyan toodeploy tanúsítványok tooVMs a Key Vault](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) – egy felhőben futó alkalmazások egy virtuális gép Azure igényeknek megfelelően egy tanúsítványt. Hogyan meg be ezt a tanúsítványt a virtuális gép ma?
- [Hogyan end tooend a Key Vault mentése tooset kulcs elforgatás és naplózási](key-vault-key-rotation-log-monitoring.md) -Ez az útmutató végigvezeti tooset kulcs elforgatás és az Azure Key Vault naplózását.
- [Azure webalkalmazás tanúsítványának keresztül Key Vault üzembe helyezése]( https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) részletes útmutatást ad a Key Vault részeként tárolja tanúsítványait [App szolgáltatástanúsítvány](https://azure.microsoft.com/blog/internals-of-app-service-certificate/) kínál.
- [Adjon engedélye toomany alkalmazások tooaccess kulcstároló](key-vault-group-permissions-for-apps.md) Key Vault hozzáférés-vezérlési házirend csak a 16 bejegyzések támogatja. Azonban létrehozhat egy Azure Active Directory biztonsági csoport. Adja hozzá az összes hello társított szolgáltatás rendszerbiztonsági tagok toothis biztonsági csoport, és adjon hozzáférést toothis biztonsági csoport tooKey tárolóban.
- További tevékenység-specifikus útmutatást integrálása és az Azure Key tárolók használatával, lásd: [Ryan János Azure Resource Manager sablon példákat Key vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
- [Hogyan toouse Key Vault soft-törlés a CLI](key-vault-soft-delete-cli.md) végigvezeti hello használata és a kulcstároló, és különféle kulcstároló objektumok életciklusát a soft-törlés engedélyezve van.
- [Hogyan toouse Key Vault soft-törlése a PowerShell-lel](key-vault-soft-delete-powershell.md) végigvezeti hello használata és a kulcstároló, és különféle kulcstároló objektumok életciklusát a soft-törlés engedélyezve van.

## <a name="integrated-with-key-vault"></a>Kulcstároló integrálva

Ezek a cikkek más forgatókönyvek és a szolgáltatások, melyek a Key Vault integrálása készül.

- [Az Azure Disk Encryption](../security/azure-security-disk-encryption.md) használja hello iparági szabvány [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) és a Windows hello szolgáltatás [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux tooprovide kötet titkosítási hello az operációs rendszer és az adatok hello szolgáltatás lemezek. hello megoldás integrálva van az Azure Key Vault toohelp vezérlik, és hello lemez titkosítási kulcsok és titkos kódokat a kulcstartót az előfizetést, biztosítva, hogy a virtuális gépek lemezeit hello az összes adat titkosítva legyenek az Azure tárolás közben.
- [Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md) lehetőséget biztosít az hello fiókban tárolt adatok titkosítása számára. Kulcskezelés a Data Lake Store két üzemmódot biztosít a fő titkosítási kulcsok (MEKs), amelyek szükségesek a Data Lake Store hello tárolt adatokat visszafejtése kezeléséhez. Vagy engedélyezheti a Data Lake Store kezelheti a hello MEKs, vagy válassza az Azure Key Vault fiókkal hello MEKs tooretain tulajdonjogát. Kulcskezelés hello módja egy Data Lake Store-fiók létrehozása közben adja meg. 
- [Az Azure Information Protection](/information-protection/plan-design/plan-implement-tenant-key) toomanager lehetővé teszi a saját bérlőkulcsát. Például helyett a Microsoft a bérlőkulcsát (alapértelmezett hello) kezelése, kezelheti a saját bérlői kulcs toocomply rendelkező tooyour szervezet vonatkozó speciális szabályozásoknak. A saját bérlőkulcsát kezelésére is hivatkozott tooas kapcsolja a saját kulcs használata avagy BYOK.

## <a name="key-vault-overviews-and-concepts"></a>Key Vault áttekintése és fogalmak

- [Key Vault soft-törlési viselkedés](key-vault-ovw-soft-delete.md) lehetővé teszi, hogy a törölt objektumok helyreállítási ismerteti, hogy hello törlés véletlen vagy szándékos volt.
- [Sávszélesség-szabályozás Key Vault ügyfél](key-vault-ovw-throttling.md) toohello alapvető fogalmait sávszélesség-szabályozás megismerkedhet csomagjainkkal, és az alkalmazás egy módszert kínál.
- [Key Vault tárolási fiók kulcsok áttekintése](key-vault-ovw-storage-keys.md) hello Key Vault integration Azure Storage-fiókok kulcsok ismerteti.
- [Key Vault biztonsági világot](key-vault-ovw-security-worlds.md) régiók és biztonsági területek hello kapcsolatai ismerteti.

## <a name="social"></a>Közösségi tartalom

- [Kulcstároló Blog](http://aka.ms/kvblog)
- [Kulcstároló-fórum](http://aka.ms/kvforum)

## <a name="supporting-libraries"></a>Szalagtárak támogatása

- [A Microsoft Azure Key Vault alap függvénytár](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) biztosít **IKey** és **IKeyResolver** adaptert használjon a kulcsok azonosítóból keresése és kulccsal rendelkező műveletet hajt végre.
- [Microsoft Azure Key Vault Extensions](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) kiterjesztett képességeket biztosít az Azure Key Vaulthoz.


