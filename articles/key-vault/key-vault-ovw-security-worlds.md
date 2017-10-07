---
ms.assetid: 
title: "aaaAzure Key Vault biztonsági világot |} Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 1995119c9e9886829d6c50af921f275d10e382f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a><span data-ttu-id="1f881-102">Az Azure Key Vault biztonsági világot és földrajzi határok</span><span class="sxs-lookup"><span data-stu-id="1f881-102">Azure Key Vault security worlds and geographic boundaries</span></span>

<span data-ttu-id="1f881-103">Az Azure Key Vault egy több-bérlős szolgáltatás, és az egyes Azure-beli hely használja a hardveres biztonsági modulokkal (HSM) készletét.</span><span class="sxs-lookup"><span data-stu-id="1f881-103">Azure Key Vault is a multi-tenant service and uses a pool of Hardware Security Modules (HSMs) in each Azure location.</span></span> 

<span data-ttu-id="1f881-104">Minden HSM-EK a hello Azure helyeken azonos földrajzi régióban megosztás hello ugyanazt a határt kriptográfiai (Thales Biztonságivilág).</span><span class="sxs-lookup"><span data-stu-id="1f881-104">All HSMs at Azure locations in hello same geographic region share hello same cryptographic boundary (Thales Security World).</span></span> <span data-ttu-id="1f881-105">Például USA keleti régiója és megosztása USA nyugati hello ugyanazt biztonsági világ mert toohello USA földrajzi hely tartoznak.</span><span class="sxs-lookup"><span data-stu-id="1f881-105">For example, East US and West US share hello same security world because they belong toohello US geo location.</span></span> <span data-ttu-id="1f881-106">Hasonlóképpen japán megosztáson található összes Azure helyét hello ugyanazt biztonsági világ és Ausztráliában-Indiában, minden Azure helyek, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="1f881-106">Similarly, all Azure locations in Japan share hello same security world and all Azure locations in Australia, India, and so on.</span></span> 

## <a name="backup-and-restore-behavior"></a><span data-ttu-id="1f881-107">Biztonsági mentés és helyreállítás viselkedése</span><span class="sxs-lookup"><span data-stu-id="1f881-107">Backup and restore behavior</span></span>

<span data-ttu-id="1f881-108">Egy készült biztonsági másolatok a kulcstároló egy Azure-beli hely lehet kulcs visszaállítása tooa egy másik Azure-beli hely, a kulcstároló, mindaddig, amíg az alábbi két feltétel teljesülése:</span><span class="sxs-lookup"><span data-stu-id="1f881-108">A backup taken of a key from a key vault in one Azure location can be restored tooa key vault in another Azure location, as long as both of these conditions are true:</span></span>

- <span data-ttu-id="1f881-109">Mindkét hello Azure helyek tartozik toohello ugyanazon a földrajzi helyen</span><span class="sxs-lookup"><span data-stu-id="1f881-109">Both of hello Azure locations belong toohello same geographical location</span></span>
- <span data-ttu-id="1f881-110">Mindkét hello kulcstárolójának tartozik toohello azonos Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="1f881-110">Both of hello key vaults belong toohello same Azure subscription</span></span>

<span data-ttu-id="1f881-111">Például egy kulcs kulcstároló Nyugat-Indiában, az adott előfizetés által végrehajtott biztonsági csak lehet visszaállított tooanother kulcstároló hello az ugyanahhoz az előfizetéshez és a földrajzi hely; Nyugat-Indiában, közép-Indiában és Dél-India.</span><span class="sxs-lookup"><span data-stu-id="1f881-111">For example, a backup taken by a given subscription of a key in a key vault in West India, can only be restored tooanother key vault in hello same subscription and geo location; West India, Central India or South India.</span></span>

## <a name="regions-and-products"></a><span data-ttu-id="1f881-112">Régiók és termékek</span><span class="sxs-lookup"><span data-stu-id="1f881-112">Regions and products</span></span>

- [<span data-ttu-id="1f881-113">Azure-régiók</span><span class="sxs-lookup"><span data-stu-id="1f881-113">Azure regions</span></span>](https://azure.microsoft.com/regions/)
- [<span data-ttu-id="1f881-114">Microsoft-termékek régió szerint</span><span class="sxs-lookup"><span data-stu-id="1f881-114">Microsoft products by region</span></span>](https://azure.microsoft.com/regions/services/)

<span data-ttu-id="1f881-115">Csatlakoztatott toosecurity világot hello táblák fő fejlécében megjelenő régiók a következők:</span><span class="sxs-lookup"><span data-stu-id="1f881-115">Regions are mapped toosecurity worlds, shown as major headings in hello tables:</span></span>

<span data-ttu-id="1f881-116">Hello termékek régió cikkben, például hello **Americas** lap tartalmaz kelet VELÜNK, USA KÖZÉPSŐ RÉGIÓJA, USA nyugati RÉGIÓJA összes leképezési toohello Americas régióban.</span><span class="sxs-lookup"><span data-stu-id="1f881-116">In hello products by region article, for example, hello **Americas** tab contains EAST US, CENTRAL US, WEST US all mapping toohello Americas region.</span></span> 

>[!NOTE]
><span data-ttu-id="1f881-117">Egy kivétel ez alól, Velünk DOD EAST és a US DOD központi rendelkezik-e a saját biztonsági világot.</span><span class="sxs-lookup"><span data-stu-id="1f881-117">An exception is that US DOD EAST and US DOD CENTRAL have their own security worlds.</span></span> 

<span data-ttu-id="1f881-118">Hasonlóképpen, a hello **Európa** lap, Észak-Európa, és Nyugat-Európa toohello Európa régió mindkét képezze le.</span><span class="sxs-lookup"><span data-stu-id="1f881-118">Similarly, on hello **Europe** tab, NORTH EUROPE and WEST EUROPE both map toohello Europe region.</span></span> <span data-ttu-id="1f881-119">hello is is igaz a hello **Ázsia Csendes-óceáni** fülre.</span><span class="sxs-lookup"><span data-stu-id="1f881-119">hello same is also true on hello **Asia Pacific** tab.</span></span>



