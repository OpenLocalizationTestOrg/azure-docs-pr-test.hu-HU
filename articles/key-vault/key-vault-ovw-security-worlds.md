---
ms.assetid: 
title: "Az Azure Key Vault biztonsági világot |} Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 921bbd109c9ea98d8b5c286a7512bed026412d26
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a><span data-ttu-id="c8696-102">Az Azure Key Vault biztonsági világot és földrajzi határok</span><span class="sxs-lookup"><span data-stu-id="c8696-102">Azure Key Vault security worlds and geographic boundaries</span></span>

<span data-ttu-id="c8696-103">Az Azure Key Vault egy több-bérlős szolgáltatás, és az egyes Azure-beli hely használja a hardveres biztonsági modulokkal (HSM) készletét.</span><span class="sxs-lookup"><span data-stu-id="c8696-103">Azure Key Vault is a multi-tenant service and uses a pool of Hardware Security Modules (HSMs) in each Azure location.</span></span> 

<span data-ttu-id="c8696-104">A ugyanabban a földrajzi régióban Azure helyen lévő összes HSM ossza meg az azonos kriptográfiai határ (Thales Biztonságivilág).</span><span class="sxs-lookup"><span data-stu-id="c8696-104">All HSMs at Azure locations in the same geographic region share the same cryptographic boundary (Thales Security World).</span></span> <span data-ttu-id="c8696-105">Például USA keleti régiója, és az USA nyugati régiója megoszthatja a ugyanazt biztonsági világ mert tartoznak az USA földrajzi hely.</span><span class="sxs-lookup"><span data-stu-id="c8696-105">For example, East US and West US share the same security world because they belong to the US geo location.</span></span> <span data-ttu-id="c8696-106">Ehhez hasonlóan japán minden Azure hely ossza meg a ugyanazt biztonsági világ és az összes Azure helyek Ausztrália, India és így tovább.</span><span class="sxs-lookup"><span data-stu-id="c8696-106">Similarly, all Azure locations in Japan share the same security world and all Azure locations in Australia, India, and so on.</span></span> 

## <a name="backup-and-restore-behavior"></a><span data-ttu-id="c8696-107">Biztonsági mentés és helyreállítás viselkedése</span><span class="sxs-lookup"><span data-stu-id="c8696-107">Backup and restore behavior</span></span>

<span data-ttu-id="c8696-108">Egy készült biztonsági másolatok a kulcstároló egyetlen Azure helyen kulcs vissza tudja állítani egy másik Azure-beli hely, a kulcstároló, amíg az alábbi két feltétel teljesül:</span><span class="sxs-lookup"><span data-stu-id="c8696-108">A backup taken of a key from a key vault in one Azure location can be restored to a key vault in another Azure location, as long as both of these conditions are true:</span></span>

- <span data-ttu-id="c8696-109">Az Azure-hely tartoznak ugyanazon a földrajzi helyen</span><span class="sxs-lookup"><span data-stu-id="c8696-109">Both of the Azure locations belong to the same geographical location</span></span>
- <span data-ttu-id="c8696-110">Mind a kulcstárolók az azonos Azure-előfizetés tartozik</span><span class="sxs-lookup"><span data-stu-id="c8696-110">Both of the key vaults belong to the same Azure subscription</span></span>

<span data-ttu-id="c8696-111">Például egy kulcs kulcstároló Nyugat-Indiában, az adott előfizetés által végrehajtott biztonsági csak vissza tudja állítani az ugyanahhoz az előfizetéshez és a földrajzi hely; egy másik kulcstároló Nyugat-Indiában, közép-Indiában és Dél-India.</span><span class="sxs-lookup"><span data-stu-id="c8696-111">For example, a backup taken by a given subscription of a key in a key vault in West India, can only be restored to another key vault in the same subscription and geo location; West India, Central India or South India.</span></span>

## <a name="regions-and-products"></a><span data-ttu-id="c8696-112">Régiók és termékek</span><span class="sxs-lookup"><span data-stu-id="c8696-112">Regions and products</span></span>

- [<span data-ttu-id="c8696-113">Azure-régiók</span><span class="sxs-lookup"><span data-stu-id="c8696-113">Azure regions</span></span>](https://azure.microsoft.com/regions/)
- [<span data-ttu-id="c8696-114">Microsoft-termékek régió szerint</span><span class="sxs-lookup"><span data-stu-id="c8696-114">Microsoft products by region</span></span>](https://azure.microsoft.com/regions/services/)

<span data-ttu-id="c8696-115">Régiók biztonsági világot fő fejlécében a táblázatok látható van leképezve:</span><span class="sxs-lookup"><span data-stu-id="c8696-115">Regions are mapped to security worlds, shown as major headings in the tables:</span></span>

<span data-ttu-id="c8696-116">A termékek régió cikkben, például a **Americas** lap tartalmaz kelet VELÜNK, USA KÖZÉPSŐ RÉGIÓJA, USA nyugati RÉGIÓJA Americas régió összes leképezésére.</span><span class="sxs-lookup"><span data-stu-id="c8696-116">In the products by region article, for example, the **Americas** tab contains EAST US, CENTRAL US, WEST US all mapping to the Americas region.</span></span> 

>[!NOTE]
><span data-ttu-id="c8696-117">Egy kivétel ez alól, Velünk DOD EAST és a US DOD központi rendelkezik-e a saját biztonsági világot.</span><span class="sxs-lookup"><span data-stu-id="c8696-117">An exception is that US DOD EAST and US DOD CENTRAL have their own security worlds.</span></span> 

<span data-ttu-id="c8696-118">Hasonlóképpen, a a **Európa** lap, Észak-Európa, és Nyugat-Európa, mind a Európa régió van leképezve.</span><span class="sxs-lookup"><span data-stu-id="c8696-118">Similarly, on the **Europe** tab, NORTH EUROPE and WEST EUROPE both map to the Europe region.</span></span> <span data-ttu-id="c8696-119">Ugyanez érvényes is a a **Ázsia Csendes-óceáni** fülre.</span><span class="sxs-lookup"><span data-stu-id="c8696-119">The same is also true on the **Asia Pacific** tab.</span></span>



