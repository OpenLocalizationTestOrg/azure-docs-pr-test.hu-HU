---
title: "aaaAzure Storage szolgáltatás titkosítási kulcsokat az Azure Key Vault ügyfél használatával felügyelt |} Microsoft Docs"
description: "Használjon hello Azure Storage szolgáltatás titkosítási szolgáltatás tooencrypt az Azure Blob Storage hello szolgáltatás oldalán hello adatokat tárolja, és kulcsok hello adatokat használó ügyfél kezelt visszafejteni."
services: storage
documentationcenter: .net
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: lakasa
ms.openlocfilehash: eb2e0ad27df40e61f9c08b9db7ca7a7568adad9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Felügyelt felhasználói kulcsok használata az Azure Key Vault Storage szolgáltatás titkosítási

Microsoft Azure a véglegesített toohelping, és az adatok toomeet megvédeni a szervezeti biztonsági és megfelelőségi jár kötelezettségekkel.  Az adatvédelemben aktívan egyike toouse Storage szolgáltatás titkosítási (SSE), automatikusan titkosítja az adatokat, amikor toostorage alkalmazásokba, és az adatok visszafejtése azt lekérése közben. hello titkosítási és visszafejtési automatikus, teljes mértékben transzparens, és használja 256 bites [AES titkosítási](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), hello legerősebb blokk egyik Rejtjelek érhető el.

A Microsoft által felügyelt titkosítási kulcsok használatával SSE, vagy használhatja a saját titkosítási kulccsal. A cikkből megtudhatja, ez utóbbi hello. További információ a Microsoft által felügyelt kulcsokkal, vagy SSE általában: [Storage szolgáltatás titkosítási az inaktív adatok](../storage-service-encryption.md).

tooprovide hello képességét toouse saját titkosítási kulccsal, a Blob Storage SSE integrálva van az Azure Key Vault (AKV). Létrehozhat saját titkosítási kulccsal, és tárolja őket a AKV, vagy AKV tartozó API-k toogenerate titkosítási kulcsokat is használhat. Nem csak nem AKV toomanage lehetővé teszi, és szabályozhatja a kulcsokat, azt is lehetővé teszi, hogy Ön tooaudit a kulcshasználati. 

Miért érdemes toocreate saját kulcsok? Nagyobb rugalmasságot biztosít, így toocreate, elforgatása, tiltsa le, és adja meg a hozzáférés-vezérlést. Azt is lehetővé teszi, hogy tooaudit hello titkosítási kulcsok használt tooprotect adatait.

## <a name="sse-with-customer-managed-keys-preview"></a>Az ügyfél felügyelt kulcsoknál előzetes verziójú SSE

Ez a szolgáltatás jelenleg előzetes kiadásban elérhető. toouse ezt a beállítást, egy új tárfiókot toocreate van szüksége. Hozhat létre egy új kulcstartó és a kulcs vagy egy meglévő kulcstároló és a kulcs használhatja. hello tárfiók és kulcstároló hello kell hello ugyanabban a régióban, de nem lehetnek különböző előfizetésekhez.

hello preview, lépjen kapcsolatba a tooparticipate [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com). Egy hivatkozásra tooparticipate hello Preview lesz elérhető.

toolearn több, tekintse meg a toohello [gyakran ismételt kérdések](#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).

> [!IMPORTANT]
> Regisztrálnia kell az hello preview előzetes toofollowing hello cikkben leírt lépéseket. Előzetes verzióhoz való hozzáférés nélkül tartalma nem kell tudni tooenable Ez a szolgáltatás hello portálon.

## <a name="getting-started"></a>Első lépések
## <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>1. lépés: [hozzon létre egy új tárfiókot](../storage-create-storage-account.md)

## <a name="step-2-enable-encryption"></a>2. lépés: Engedélyezze titkosítás
Hello tárfiók hello segítségével engedélyezheti az SSE [Azure-portálon](https://portal.azure.com). Ahogy az alábbi ábra, és kattintson a titkosítás hello keressen hello Blob szolgáltatás szakasz a hello-beállítások panelen hello tárfiók.

![A titkosítási beállítással portál ábrázoló képernyőfelvétel](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)
<br/>*Blob szolgáltatás SSE engedélyezése*

Ha szeretné, hogy tooprogrammatically engedélyezése, vagy tiltsa le a Storage szolgáltatás titkosítási hello egy tárfiókon, használhatja a hello [Azure Storage erőforrás szolgáltató REST API felülete](https://docs.microsoft.com/en-us/rest/api/storagerp/?redirectedfrom=MSDN), hello [Storage erőforrás-szolgáltató ügyféloldali kódtár a .NET-hez](https://docs.microsoft.com/en-us/dotnet/api/?redirectedfrom=MSDN), [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview?view=azurermps-4.0.0), vagy hello [Azure CLI](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli).

Ezen a képernyőn Ha nem látható hello "saját kulcs használata" jelölőnégyzet, akkor nem jóváhagyott hello Preview. E-mail küldése túl[ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) és jóváhagyás-igénylést.

![Titkosítási betekintő portál képernyőfelvétel](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

Alapértelmezés szerint az SSE Microsoft által felügyelt kulcsokat használ. toouse saját kulcsok hello jelölőnégyzetet. Majd adja meg a kulcs URI, vagy válassza ki a kulcs és a Key Vault hello objektumválasztó.

## <a name="step-3-select-your-key"></a>3. lépés: Válassza ki a kulcs

![Portál ábrázoló képernyőfelvétel titkosítás használata a saját kulcs lehetőséggel](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)

![Portál képernyőfelvétel: a titkosítási kulcs uri-beállítás megadása](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Ha hello storage-fiók nem rendelkezik hozzáféréssel toohello Key Vault, hello következő futtathatja használata Azure Powershell toogrant hozzáférés toohello tárfiókok toohello kulcstároló szükséges parancsot.

![A hozzáférés megtagadva a kulcstartót portál képernyőfelvétel](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

Szerint hello Azure-portálon az Azure Key Vault toohello fog, és a hozzáférés toohello storage-fiók megadása is engedélyezheti a hozzáférést hello Azure-portálon keresztül.

## <a name="step-4-copy-data-toostorage-account"></a>4. lépés:, Másolja át az toostorage fiókja
Ha azt szeretné tootransfer adatokat az új tárolási figyelembe, hogy titkosítva van, tekintse meg a túl[lépés 3 az első lépések a Storage szolgáltatás titkosítási az inaktív adatok](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption#step-3-copy-data-to-storage-account).

## <a name="step-5-query-hello-status-of-hello-encrypted-data"></a>5. lépés: A titkosított adatok hello hello állapotának lekérdezése
a titkosított adatok hello tooquery hello állapotát, tekintse meg a túl[lépés 4 az első lépések a Storage szolgáltatás titkosítási az inaktív adatok](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption#step-4-query-the-status-of-the-encrypted-data).

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Gyakori kérdések Storage szolgáltatás titkosítási az inaktív adatok
**K: használom a prémium szintű storage; Használhatok SSE felügyelt ügyfél kulcsokkal?**

V: Igen, a Microsoft által felügyelt SSE és az ügyfelek felügyelt kulcsok szabványos tárolás és a prémium szintű storage egyaránt támogatott. 

**K: hozható létre új tárfiókok az SSE engedélyezve van az Azure PowerShell és az Azure parancssori felület használatával felügyelt ügyfél kulcsokkal?**

V: Igen.

**K: hogyan sokkal több does Azure tárolási költségű SSE ügyféllel felügyelt kulcsok Ha engedélyezve van?**

V: nincs az Azure Key Vault használatával költsége. További részletekért látogasson el [Key Vault díjszabása](https://azure.microsoft.com/en-us/pricing/details/key-vault/). Nincs SSE használatára vonatkozó további költség nélkül.

**K: hozzáférés toohello titkosítási kulcsok visszavonása?**

A: visszavonhatja a hozzáférést Igen, tetszőleges időpontban. Számos módon toorevoke tooyour hívóbetűk van. Tekintse meg a túl[Azure Key Vault PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.0.0) és [Azure Key Vault CLI](https://docs.microsoft.com/en-us/cli/azure/keyvault) további részleteket. Visszavonni a hozzáférési hatékonyan blokkolja tooall blobok hello tárfiókban, titkosítási kulcsára hello Azure Storage nem érhetők el.

**K: hozható létre a tárfiók és a kulcs másik régióban?**

A: nem hello tárfiók és a kulcs tárolóbeli/kulccsal kell toobe hello ugyanabban a régióban. 

**K: engedélyezhető az SSE felügyelt ügyfél kulccsal rendelkező hello storage-fiók létrehozása közben?**

V: nem. Ha engedélyezi az SSE hello storage-fiók létrehozása közben, Microsoft által felügyelt kulcsok csak használhatja. Ha szeretné toouse felügyelt ügyfél kulcsokat, frissítenie kell a hello a tárfiók tulajdonságai. Használhatja a többi vagy hello tárolási ügyfél szalagtárak tooprogrammatically egyike a tárfiók módosítása, vagy frissítse a tárfiók tulajdonságai hello hello Azure-portál használatával hello fiók létrehozása után.

**K: letiltani titkosítás közben SSE ügyféllel felügyelt kulcsok?**

V: nem, akkor titkosítás nem tiltható le közben SSE ügyféllel felügyelt kulcsok. toodisable titkosítási, át kell váltania toousing Microsoft által felügyelt kulcsok. Ehhez hello Azure-portálon vagy a PowerShell használatával.

**K: SSE alapértelmezés szerint engedélyezve van, egy új tárfiók létrehozásakor?**

V: SSE; alapértelmezés szerint nincs engedélyezve az Azure portál tooenable hello használhatja azt. Szoftveresen is engedélyezheti ezt a funkciót hello Storage erőforrás-szolgáltató REST API használatával. 

**K: nem engedélyezhető a titkosítás a storage-fiókom.**

V: az azt egy erőforrás-kezelő tárfiókot? Klasszikus tárfiókok nem támogatottak. Az ügyfél által felügyelt kulcsok SSE csak az újonnan létrehozott erőforrás-kezelő storage-fiókok is engedélyezhető.

**K: van SSE ügyféllel felügyelt csak meghatározott régióiba engedélyezett kulcsok?**

V: SSE csak bizonyos régiókban a Blob-tároló ebben az előzetes verzióban érhető el. E-mailek [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) toocheck rendelkezésre állást és a részletek a Preview-ban. 

**K: hogyan do I kapcsolatfelvételre Ha problémák merülnek fel, vagy visszajelzés tooprovide I?**

V: kapcsolattartási [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) pedig problémákkal kapcsolatos tooStorage titkosítását. 

## <a name="next-steps"></a>Következő lépések

*   Bővebben hello választékát biztonsági funkciókat nyújtanak, amelyek segítségével a fejlesztők olyan biztonságos alkalmazásokat hozhat létre című hello [tárolási biztonsági útmutatója](https://docs.microsoft.com/en-us/azure/storage/storage-security-guide).
*   Áttekintés az Azure Key Vault kapcsolatos információkért lásd: [Mi az Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis)?
*   Ismerkedés az Azure Key Vault, lásd: [Ismerkedés az Azure Key Vault](../../key-vault/key-vault-get-started.md).
