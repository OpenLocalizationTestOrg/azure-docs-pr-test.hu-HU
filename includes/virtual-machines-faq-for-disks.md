# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a><span data-ttu-id="d4a43-101">Azure IaaS virtuális gép és a kezelt és kezeletlen premium lemezek kapcsolatos gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="d4a43-101">Frequently asked questions about Azure IaaS VM disks and managed and unmanaged premium disks</span></span>

<span data-ttu-id="d4a43-102">Ebben a cikkben megválaszolunk néhány Azure felügyelt lemezek és a prémium szintű Azure Storage kapcsolatos gyakori kérdésekre.</span><span class="sxs-lookup"><span data-stu-id="d4a43-102">This article answers some frequently asked questions about Azure Managed Disks and Azure Premium Storage.</span></span>

## <a name="managed-disks"></a><span data-ttu-id="d4a43-103">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="d4a43-103">Managed Disks</span></span>

<span data-ttu-id="d4a43-104">**Mi az Azure Managed lemezek?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-104">**What is Azure Managed Disks?**</span></span>

<span data-ttu-id="d4a43-105">Felügyelt lemezek olyan szolgáltatás, amely egyszerűbbé teszi a Lemezkezelés Azure IaaS virtuális gépek kezelése a tárolási fiók kezelése az Ön által.</span><span class="sxs-lookup"><span data-stu-id="d4a43-105">Managed Disks is a feature that simplifies disk management for Azure IaaS VMs by handling storage account management for you.</span></span> <span data-ttu-id="d4a43-106">További információkért lásd: a [felügyelt lemezekhez – áttekintés](../articles/virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d4a43-106">For more information, see the [Managed Disks overview](../articles/virtual-machines/windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="d4a43-107">**Ha egy standard szintű felügyelt lemezes I készíteni egy meglévő VHD-t, amely 80 GB, mennyi lesz, amely költségeket, me?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-107">**If I create a standard managed disk from an existing VHD that's 80 GB, how much will that cost me?**</span></span>

<span data-ttu-id="d4a43-108">80 GB-os virtuális merevlemezről létrehozott egy standard szintű felügyelt lemezes a rendszer a következő elérhető standard méretű lemez méretet, amelyet egy S10 lemez.</span><span class="sxs-lookup"><span data-stu-id="d4a43-108">A standard managed disk created from an 80-GB VHD is treated as the next available standard disk size, which is an S10 disk.</span></span> <span data-ttu-id="d4a43-109">Most a S10 lemez díjszabás alapján számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="d4a43-109">You're charged according to the S10 disk pricing.</span></span> <span data-ttu-id="d4a43-110">További tájékoztatás a [díjszabási lapon](https://azure.microsoft.com/pricing/details/storage) olvasható.</span><span class="sxs-lookup"><span data-stu-id="d4a43-110">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="d4a43-111">**Vannak-e a standard szintű felügyelt lemez tranzakciós költségeket?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-111">**Are there any transaction costs for standard managed disks?**</span></span>

<span data-ttu-id="d4a43-112">Igen.</span><span class="sxs-lookup"><span data-stu-id="d4a43-112">Yes.</span></span> <span data-ttu-id="d4a43-113">Minden tranzakció még fizetnie.</span><span class="sxs-lookup"><span data-stu-id="d4a43-113">You're charged for each transaction.</span></span> <span data-ttu-id="d4a43-114">További tájékoztatás a [díjszabási lapon](https://azure.microsoft.com/pricing/details/storage) olvasható.</span><span class="sxs-lookup"><span data-stu-id="d4a43-114">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="d4a43-115">**Az egy standard szintű felügyelt lemezes I megterheljük a lemezen lévő adatok tényleges mérete, vagy a lemez kiosztott kapacitást?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-115">**For a standard managed disk, will I be charged for the actual size of the data on the disk or for the provisioned capacity of the disk?**</span></span>

<span data-ttu-id="d4a43-116">A lemez kiosztott kapacitást alapján még fel.</span><span class="sxs-lookup"><span data-stu-id="d4a43-116">You're charged based on the provisioned capacity of the disk.</span></span> <span data-ttu-id="d4a43-117">További tájékoztatás a [díjszabási lapon](https://azure.microsoft.com/pricing/details/storage) olvasható.</span><span class="sxs-lookup"><span data-stu-id="d4a43-117">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="d4a43-118">**Hogyan azonos a nem felügyelt lemezek felügyelt premium lemezek árképzési?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-118">**How is pricing of premium managed disks different from unmanaged disks?**</span></span>

<span data-ttu-id="d4a43-119">A felügyelt premium lemezek árképzési megegyezik a nem felügyelt premium lemezek.</span><span class="sxs-lookup"><span data-stu-id="d4a43-119">The pricing of premium managed disks is the same as unmanaged premium disks.</span></span>

<span data-ttu-id="d4a43-120">**A tárfiók típusa (Standard vagy prémium) a kezelt lemezek is megváltozik?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-120">**Can I change the storage account type (Standard or Premium) of my managed disks?**</span></span>

<span data-ttu-id="d4a43-121">Igen.</span><span class="sxs-lookup"><span data-stu-id="d4a43-121">Yes.</span></span> <span data-ttu-id="d4a43-122">Az Azure-portálon, a PowerShell vagy az Azure parancssori felület használatával módosíthatja a tárfiók típusa a felügyelt lemezek.</span><span class="sxs-lookup"><span data-stu-id="d4a43-122">You can change the storage account type of your managed disks by using the Azure portal, PowerShell, or the Azure CLI.</span></span>

<span data-ttu-id="d4a43-123">**Van mód, hogy lehet másolni, vagy egy felügyelt lemezes exportálása a titkos storage-fiók?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-123">**Is there a way that I can copy or export a managed disk to a private storage account?**</span></span>

<span data-ttu-id="d4a43-124">Igen.</span><span class="sxs-lookup"><span data-stu-id="d4a43-124">Yes.</span></span> <span data-ttu-id="d4a43-125">A felügyelt lemezek exportálhatja az Azure-portálon, a PowerShell vagy az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="d4a43-125">You can export your managed disks by using the Azure portal, PowerShell, or the Azure CLI.</span></span>

<span data-ttu-id="d4a43-126">**Az Azure-tárfiók VHD-fájl használatával hozzon létre egy felügyelt lemezes egy másik előfizetést?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-126">**Can I use a VHD file in an Azure storage account to create a managed disk with a different subscription?**</span></span>

<span data-ttu-id="d4a43-127">Nem.</span><span class="sxs-lookup"><span data-stu-id="d4a43-127">No.</span></span>

<span data-ttu-id="d4a43-128">**Az Azure-tárfiók VHD-fájl használatával hozzon létre egy felügyelt lemezes egy másik régióban?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-128">**Can I use a VHD file in an Azure storage account to create a managed disk in a different region?**</span></span>

<span data-ttu-id="d4a43-129">Nem.</span><span class="sxs-lookup"><span data-stu-id="d4a43-129">No.</span></span>

<span data-ttu-id="d4a43-130">**Vannak-e a felügyelt lemezeket használó ügyfelek skálázási korlátozások?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-130">**Are there any scale limitations for customers that use managed disks?**</span></span>

<span data-ttu-id="d4a43-131">Kezelt lemezek megszünteti a társított tárfiókok korlátok.</span><span class="sxs-lookup"><span data-stu-id="d4a43-131">Managed Disks eliminates the limits associated with storage accounts.</span></span> <span data-ttu-id="d4a43-132">Azonban a előfizetésenként felügyelt lemezek számát korlátozódik 2000 alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="d4a43-132">However, the number of managed disks per subscription is limited to 2,000 by default.</span></span> <span data-ttu-id="d4a43-133">Ezt a számot növelheti a támogatási hívása.</span><span class="sxs-lookup"><span data-stu-id="d4a43-133">You can call support to increase this number.</span></span>

<span data-ttu-id="d4a43-134">**Eltarthat egy kezelt lemez növekményes pillanatképet?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-134">**Can I take an incremental snapshot of a managed disk?**</span></span>

<span data-ttu-id="d4a43-135">Nem.</span><span class="sxs-lookup"><span data-stu-id="d4a43-135">No.</span></span> <span data-ttu-id="d4a43-136">Az aktuális pillanatkép-készítés révén felügyelt lemezes teljes másolata.</span><span class="sxs-lookup"><span data-stu-id="d4a43-136">The current snapshot capability makes a full copy of a managed disk.</span></span> <span data-ttu-id="d4a43-137">Azonban azt tervezi, hogy a jövőben támogatja a növekményes pillanatképek.</span><span class="sxs-lookup"><span data-stu-id="d4a43-137">However, we are planning to support incremental snapshots in the future.</span></span>

<span data-ttu-id="d4a43-138">**Virtuális gépek rendelkezésre állási csoport által felügyelt és nem kezelt lemezek kombinációja állhat?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-138">**Can VMs in an availability set consist of a combination of managed and unmanaged disks?**</span></span>

<span data-ttu-id="d4a43-139">Nem.</span><span class="sxs-lookup"><span data-stu-id="d4a43-139">No.</span></span> <span data-ttu-id="d4a43-140">A virtuális gépek rendelkezésre állási csoportba kell használnia, vagy az összes felügyelt lemeznek, vagy a nem felügyelt összes lemez.</span><span class="sxs-lookup"><span data-stu-id="d4a43-140">The VMs in an availability set must use either all managed disks or all unmanaged disks.</span></span> <span data-ttu-id="d4a43-141">Amikor létrehoz egy rendelkezésre állási csoportot, kiválaszthatja a használni kívánt lemezek milyen típusú.</span><span class="sxs-lookup"><span data-stu-id="d4a43-141">When you create an availability set, you can choose which type of disks you want to use.</span></span>

<span data-ttu-id="d4a43-142">**Felügyelt lemezek van az alapértelmezett beállítás, az Azure-portálon?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-142">**Is Managed Disks the default option in the Azure portal?**</span></span>

<span data-ttu-id="d4a43-143">Jelenleg nem de ez a jövőben lesz az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="d4a43-143">Not currently, but it will become the default in the future.</span></span>

<span data-ttu-id="d4a43-144">**Hozhat létre egy üres felügyelt lemezes?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-144">**Can I create an empty managed disk?**</span></span>

<span data-ttu-id="d4a43-145">Igen.</span><span class="sxs-lookup"><span data-stu-id="d4a43-145">Yes.</span></span> <span data-ttu-id="d4a43-146">Üres lemez hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="d4a43-146">You can create an empty disk.</span></span> <span data-ttu-id="d4a43-147">Felügyelt lemezes hozhatók létre függetlenül a virtuális gépek, például egy virtuális Géphez való csatlakoztatás nélkül.</span><span class="sxs-lookup"><span data-stu-id="d4a43-147">A managed disk can be created independently of a VM, for example, without attaching it to a VM.</span></span>

<span data-ttu-id="d4a43-148">**Mi a támogatott tartalék tartományok számának a rendelkezésre állási beállítása felügyelt lemezeket használó?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-148">**What is the supported fault domain count for an availability set that uses Managed Disks?**</span></span>

<span data-ttu-id="d4a43-149">Attól függően, hogy a régió, ahol a rendelkezésre állási csoport által felügyelt lemezeket használó a támogatott tartalék tartományok számának, 2 vagy 3.</span><span class="sxs-lookup"><span data-stu-id="d4a43-149">Depending on the region where the availability set that uses Managed Disks is located, the supported fault domain count is 2 or 3.</span></span>

<span data-ttu-id="d4a43-150">**Hogyan van a standard szintű tárfiók beállítása diagnosztikai?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-150">**How is the standard storage account for diagnostics set up?**</span></span>

<span data-ttu-id="d4a43-151">A Virtuálisgép-diagnosztika titkos tárolási fiók beállítása.</span><span class="sxs-lookup"><span data-stu-id="d4a43-151">You set up a private storage account for VM diagnostics.</span></span> <span data-ttu-id="d4a43-152">A jövőben tervezzük diagnosztika váltani kezelt lemezeken is.</span><span class="sxs-lookup"><span data-stu-id="d4a43-152">In the future, we plan to switch diagnostics to Managed Disks as well.</span></span>

<span data-ttu-id="d4a43-153">**Milyen típusú szerepköralapú hozzáférés-vezérlés támogatásának felügyelt lemezek érhető el?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-153">**What kind of Role-Based Access Control support is available for Managed Disks?**</span></span>

<span data-ttu-id="d4a43-154">Felügyelt lemezek által támogatott három fő alapértelmezett szerepkörök:</span><span class="sxs-lookup"><span data-stu-id="d4a43-154">Managed Disks supports three key default roles:</span></span>

* <span data-ttu-id="d4a43-155">Tulajdonos: Mindent felügyelhetnek, beleértve a hozzáférést</span><span class="sxs-lookup"><span data-stu-id="d4a43-155">Owner: Can manage everything, including access</span></span>
* <span data-ttu-id="d4a43-156">Társszerző: Hozzáférés kivételével mindent felügyelhetnek</span><span class="sxs-lookup"><span data-stu-id="d4a43-156">Contributor: Can manage everything except access</span></span>
* <span data-ttu-id="d4a43-157">Olvasó: Mindent megtekinthetnek, de nem módosítható</span><span class="sxs-lookup"><span data-stu-id="d4a43-157">Reader: Can view everything, but can't make changes</span></span>

<span data-ttu-id="d4a43-158">**Van mód, hogy lehet másolni, vagy egy felügyelt lemezes exportálása a titkos storage-fiók?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-158">**Is there a way that I can copy or export a managed disk to a private storage account?**</span></span>

<span data-ttu-id="d4a43-159">A csak olvasható közös hozzáférésű jogosultságkód URI lekérése a felügyelt lemezes és a segítségével a saját tárolási fiók vagy a helyszíni tárolási másolja.</span><span class="sxs-lookup"><span data-stu-id="d4a43-159">You can get a read-only shared access signature URI for the managed disk and use it to copy the contents to a private storage account or on-premises storage.</span></span>

<span data-ttu-id="d4a43-160">**A felügyelt lemezes másolatát hozhat létre?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-160">**Can I create a copy of my managed disk?**</span></span>

<span data-ttu-id="d4a43-161">Ügyfelek pillanatkép készítése a felügyelt lemezek és a pillanatkép segítségével hozzon létre egy másik kezelt lemezre.</span><span class="sxs-lookup"><span data-stu-id="d4a43-161">Customers can take a snapshot of their managed disks and then use the snapshot to create another managed disk.</span></span>

<span data-ttu-id="d4a43-162">**Nem felügyelt lemezek továbbra is támogatottak?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-162">**Are unmanaged disks still supported?**</span></span>

<span data-ttu-id="d4a43-163">Igen.</span><span class="sxs-lookup"><span data-stu-id="d4a43-163">Yes.</span></span> <span data-ttu-id="d4a43-164">Nem felügyelt és a felügyelt támogatjuk.</span><span class="sxs-lookup"><span data-stu-id="d4a43-164">We support unmanaged and managed disks.</span></span> <span data-ttu-id="d4a43-165">Javasoljuk, hogy új munkaterhelések esetén használja a felügyelt lemezeit, és az aktuális munkaterheléseket telepít át kezelt lemezek.</span><span class="sxs-lookup"><span data-stu-id="d4a43-165">We recommend that you use managed disks for new workloads and migrate your current workloads to managed disks.</span></span>


<span data-ttu-id="d4a43-166">**Ha megkezdtem 128 GB-os lemez létrehozása, és növelje a 130 GB-os méret, I megterheljük a következő lemez méretét (512 GB)?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-166">**If I create a 128-GB disk and then increase the size to 130 GB, will I be charged for the next disk size (512 GB)?**</span></span>

<span data-ttu-id="d4a43-167">Igen.</span><span class="sxs-lookup"><span data-stu-id="d4a43-167">Yes.</span></span>

<span data-ttu-id="d4a43-168">**Létrehozhatok helyileg redundáns tárolás, a georedundáns tárolás és zónaredundáns tárolás által kezelt lemezeken?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-168">**Can I create locally redundant storage, geo-redundant storage, and zone-redundant storage managed disks?**</span></span>

<span data-ttu-id="d4a43-169">Azure-lemezeket felügyelt jelenleg csak a helyileg redundáns tárolás felügyelt lemezeit támogatja.</span><span class="sxs-lookup"><span data-stu-id="d4a43-169">Azure Managed Disks currently supports only locally redundant storage managed disks.</span></span>

<span data-ttu-id="d4a43-170">**Csökkenhessen vagy a felügyelt lemezek downsize?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-170">**Can I shrink or downsize my managed disks?**</span></span>

<span data-ttu-id="d4a43-171">Nem.</span><span class="sxs-lookup"><span data-stu-id="d4a43-171">No.</span></span> <span data-ttu-id="d4a43-172">Ez a funkció jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="d4a43-172">This feature is not supported currently.</span></span> 

<span data-ttu-id="d4a43-173">**Módosítható a számítógép neve tulajdonság amikor egy speciális (nem hozta létre a rendszer-előkészítő eszközzel vagy általánosított) operációsrendszer-lemez a virtuális gép létrehozásához használt?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-173">**Can I change the computer name property when a specialized (not created by using the System Preparation tool or generalized) operating system disk is used to provision a VM?**</span></span>

<span data-ttu-id="d4a43-174">Nem.</span><span class="sxs-lookup"><span data-stu-id="d4a43-174">No.</span></span> <span data-ttu-id="d4a43-175">A számítógép name tulajdonság nem frissíthető.</span><span class="sxs-lookup"><span data-stu-id="d4a43-175">You can't update the computer name property.</span></span> <span data-ttu-id="d4a43-176">Az új virtuális gép örökli a szülő virtuális gép, amelyen az operációsrendszer-lemez létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="d4a43-176">The new VM inherits it from the parent VM, which was used to create the operating system disk.</span></span> 

<span data-ttu-id="d4a43-177">**Hol találnak a minta Azure Resource Manager-sablonok felügyelt lemezzel rendelkező virtuális gépek létrehozásához?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-177">**Where can I find sample Azure Resource Manager templates to create VMs with managed disks?**</span></span>
* [<span data-ttu-id="d4a43-178">Felügyelt lemezekkel sablonok listája</span><span class="sxs-lookup"><span data-stu-id="d4a43-178">List of templates using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="d4a43-179">https://github.com/chagarw/MDPP</span><span class="sxs-lookup"><span data-stu-id="d4a43-179">https://github.com/chagarw/MDPP</span></span>

## <a name="managed-disks-and-storage-service-encryption"></a><span data-ttu-id="d4a43-180">Lemezek és a Storage szolgáltatás titkosítási felügyelt</span><span class="sxs-lookup"><span data-stu-id="d4a43-180">Managed Disks and Storage Service Encryption</span></span> 

<span data-ttu-id="d4a43-181">**Azure Storage szolgáltatás titkosítási alapértelmezés szerint engedélyezve van egy felügyelt lemezes létrehozásakor?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-181">**Is Azure Storage Service Encryption enabled by default when I create a managed disk?**</span></span>

<span data-ttu-id="d4a43-182">Igen.</span><span class="sxs-lookup"><span data-stu-id="d4a43-182">Yes.</span></span>

<span data-ttu-id="d4a43-183">**A titkosítási kulcsok kezelő?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-183">**Who manages the encryption keys?**</span></span>

<span data-ttu-id="d4a43-184">A Microsoft kezeli a titkosítási kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="d4a43-184">Microsoft manages the encryption keys.</span></span>

<span data-ttu-id="d4a43-185">**Képes-e letiltása Storage szolgáltatás titkosítási a felügyelt lemezek?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-185">**Can I disable Storage Service Encryption for my managed disks?**</span></span>

<span data-ttu-id="d4a43-186">Nem.</span><span class="sxs-lookup"><span data-stu-id="d4a43-186">No.</span></span>

<span data-ttu-id="d4a43-187">**Érhető Storage szolgáltatás titkosítási csak meghatározott régióiba?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-187">**Is Storage Service Encryption only available in specific regions?**</span></span>

<span data-ttu-id="d4a43-188">Nem.</span><span class="sxs-lookup"><span data-stu-id="d4a43-188">No.</span></span> <span data-ttu-id="d4a43-189">Érhető el minden olyan régióban, amennyiben rendelkezésre áll-e felügyelt lemezek.</span><span class="sxs-lookup"><span data-stu-id="d4a43-189">It's available in all the regions where Managed Disks is available.</span></span> <span data-ttu-id="d4a43-190">Felügyelt lemezek minden nyilvános régiók és Németországban érhető el.</span><span class="sxs-lookup"><span data-stu-id="d4a43-190">Managed Disks is available in all public regions and Germany.</span></span>

<span data-ttu-id="d4a43-191">**Hogyan tudhatom meg titkosítottak, ha a felügyelt lemezes?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-191">**How can I find out if my managed disk is encrypted?**</span></span>

<span data-ttu-id="d4a43-192">A felügyelt lemezes az Azure-portálon az Azure CLI és PowerShell létrehozásának ideje talál.</span><span class="sxs-lookup"><span data-stu-id="d4a43-192">You can find out the time when a managed disk was created from the Azure portal, the Azure CLI, and PowerShell.</span></span> <span data-ttu-id="d4a43-193">Ha az időpont után 2017. június 9., a lemez titkosítva.</span><span class="sxs-lookup"><span data-stu-id="d4a43-193">If the time is after June 9, 2017, then your disk is encrypted.</span></span> 

<span data-ttu-id="d4a43-194">**Hogyan titkosíthatja a meglévő lemezek 2017. június 10. előtt létrehozott?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-194">**How can I encrypt my existing disks that were created before June 10, 2017?**</span></span>

<span data-ttu-id="d4a43-195">Től 2017. június 10. a meglévő felügyelt lemezekre írt új adatokat automatikusan titkosítva.</span><span class="sxs-lookup"><span data-stu-id="d4a43-195">As of June 10, 2017, new data written to existing managed disks is automatically encrypted.</span></span> <span data-ttu-id="d4a43-196">Azt is tervezi, hogy a meglévő adatok titkosítása, és a titkosítás aszinkron módon történik a háttérben.</span><span class="sxs-lookup"><span data-stu-id="d4a43-196">We are also planning to encrypt existing data, and the encryption will happen asynchronously in the background.</span></span> <span data-ttu-id="d4a43-197">Ha meglévő adatokat most titkosítani kell, hozzon létre egy másolatot a lemezen.</span><span class="sxs-lookup"><span data-stu-id="d4a43-197">If you must encrypt existing data now, create a copy of your disk.</span></span> <span data-ttu-id="d4a43-198">Új lemez titkosítva legyen.</span><span class="sxs-lookup"><span data-stu-id="d4a43-198">New disks will be encrypted.</span></span>

* [<span data-ttu-id="d4a43-199">Felügyelt lemezeket másolja az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="d4a43-199">Copy managed disks by using the Azure CLI</span></span>](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)
* [<span data-ttu-id="d4a43-200">Felügyelt lemezeket másolja a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="d4a43-200">Copy managed disks by using PowerShell</span></span>](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="d4a43-201">**Felügyelt pillanatfelvételek és lemezképek titkosítva van?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-201">**Are managed snapshots and images encrypted?**</span></span>

<span data-ttu-id="d4a43-202">Igen.</span><span class="sxs-lookup"><span data-stu-id="d4a43-202">Yes.</span></span> <span data-ttu-id="d4a43-203">Az összes kezelt pillanatképeket és lemezképek létrehozása után 2017. június 9., automatikusan titkosítva vannak.</span><span class="sxs-lookup"><span data-stu-id="d4a43-203">All managed snapshots and images created after June 9, 2017, are automatically encrypted.</span></span> 

<span data-ttu-id="d4a43-204">**Is virtuális gépek is átalakítása nem felügyelt a storage-fiókok vagy kezelt lemezek korábban titkosított lévő lemezek?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-204">**Can I convert VMs with unmanaged disks that are located on storage accounts that are or were previously encrypted to managed disks?**</span></span>

<span data-ttu-id="d4a43-205">Igen</span><span class="sxs-lookup"><span data-stu-id="d4a43-205">Yes</span></span>

<span data-ttu-id="d4a43-206">**A felügyelt lemezes vagy a pillanatképet egy exportált VHD-t is titkosítva lesznek?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-206">**Will an exported VHD from a managed disk or a snapshot also be encrypted?**</span></span>

<span data-ttu-id="d4a43-207">Nem.</span><span class="sxs-lookup"><span data-stu-id="d4a43-207">No.</span></span> <span data-ttu-id="d4a43-208">De ha egy virtuális Merevlemezt egy titkosított tárfiókba történő exportálását egy titkosított kezelt lemez vagy a pillanatképet, majd titkosítva van.</span><span class="sxs-lookup"><span data-stu-id="d4a43-208">But if you export a VHD to an encrypted storage account from an encrypted managed disk or snapshot, then it's encrypted.</span></span> 

## <a name="premium-disks-managed-and-unmanaged"></a><span data-ttu-id="d4a43-209">Prémium szintű lemezekhez: felügyelt és nem felügyelt</span><span class="sxs-lookup"><span data-stu-id="d4a43-209">Premium disks: Managed and unmanaged</span></span>

<span data-ttu-id="d4a43-210">**Egy virtuális gép használ, amely támogatja a prémium szintű Storage, például egy DSv2 mérete több premium és a szabványos adatlemezek csatolható?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-210">**If a VM uses a size series that supports Premium Storage, such as a DSv2, can I attach both premium and standard data disks?**</span></span> 

<span data-ttu-id="d4a43-211">Igen.</span><span class="sxs-lookup"><span data-stu-id="d4a43-211">Yes.</span></span>

<span data-ttu-id="d4a43-212">**Csatolható prémium és standard adatlemezek is, amely nem támogatja a prémium szintű Storage, például a D, Dv2, G vagy F adatsorozat mérete több?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-212">**Can I attach both premium and standard data disks to a size series that doesn't support Premium Storage, such as D, Dv2, G, or F series?**</span></span>

<span data-ttu-id="d4a43-213">Nem.</span><span class="sxs-lookup"><span data-stu-id="d4a43-213">No.</span></span> <span data-ttu-id="d4a43-214">Csak a standard adatlemezek csatolhat a virtuális gépek, amelyek nem használják, amely támogatja a prémium szintű Storage mérete több.</span><span class="sxs-lookup"><span data-stu-id="d4a43-214">You can attach only standard data disks to VMs that don't use a size series that supports Premium Storage.</span></span>

<span data-ttu-id="d4a43-215">**Ha egy meglévő virtuális merevlemezről, de a 80 GB prémium adatlemezt hozok létre, mennyire, amely ára?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-215">**If I create a premium data disk from an existing VHD that was 80 GB, how much will that cost?**</span></span>

<span data-ttu-id="d4a43-216">80 GB-os virtuális merevlemezről létrehozott prémium adatlemezt a rendszer a következő érhető el prémium szintű lemez méretet, amelyet P10 lemez.</span><span class="sxs-lookup"><span data-stu-id="d4a43-216">A premium data disk created from an 80-GB VHD is treated as the next-available premium disk size, which is a P10 disk.</span></span> <span data-ttu-id="d4a43-217">Most a P10 lemez díjszabás alapján számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="d4a43-217">You're charged according to the P10 disk pricing.</span></span>

<span data-ttu-id="d4a43-218">**Vannak-e a prémium szintű Storage tranzakciós költségek?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-218">**Are there transaction costs to use Premium Storage?**</span></span>

<span data-ttu-id="d4a43-219">Nincs minden lemez méretét, amelyet adott határral kiosztott iops-érték és az átvitel meg egy rögzített költséget.</span><span class="sxs-lookup"><span data-stu-id="d4a43-219">There is a fixed cost for each disk size, which comes provisioned with specific limits on IOPS and throughput.</span></span> <span data-ttu-id="d4a43-220">Az egyéb költségekkel kimenő sávszélesség és a pillanatkép-kapacitást, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="d4a43-220">The other costs are outbound bandwidth and snapshot capacity, if applicable.</span></span> <span data-ttu-id="d4a43-221">További tájékoztatás a [díjszabási lapon](https://azure.microsoft.com/pricing/details/storage) olvasható.</span><span class="sxs-lookup"><span data-stu-id="d4a43-221">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="d4a43-222">**Az iops-érték és a teljesítményt, amelyek szeretnék lekérheti a lemezgyorsítótár korlátai**</span><span class="sxs-lookup"><span data-stu-id="d4a43-222">**What are the limits for IOPS and throughput that I can get from the disk cache?**</span></span>

<span data-ttu-id="d4a43-223">A kombinált korlátozhatja a gyorsítótár és a DS-ben több helyi SSD is core 4000 iops-értéket core másodpercenként 33 MB.</span><span class="sxs-lookup"><span data-stu-id="d4a43-223">The combined limits for cache and local SSD for a DS series are 4,000 IOPS per core and 33 MB per second per core.</span></span> <span data-ttu-id="d4a43-224">A GS adatsorozat 5000 iops teljesítményt nyújtanak core és 50 MB / s / core kínál.</span><span class="sxs-lookup"><span data-stu-id="d4a43-224">The GS series offers 5,000 IOPS per core and 50 MB per second per core.</span></span>

<span data-ttu-id="d4a43-225">**A lemezek felügyelt virtuális gépek támogatja a helyi SSD?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-225">**Is the local SSD supported for a Managed Disks VM?**</span></span>

<span data-ttu-id="d4a43-226">A helyi SSD az ideiglenes tároló, a lemezek felügyelt virtuális gépek részét képező.</span><span class="sxs-lookup"><span data-stu-id="d4a43-226">The local SSD is temporary storage that is included with a Managed Disks VM.</span></span> <span data-ttu-id="d4a43-227">Nincs ingyenesen a ideiglenes tárolására.</span><span class="sxs-lookup"><span data-stu-id="d4a43-227">There is no extra cost for this temporary storage.</span></span> <span data-ttu-id="d4a43-228">Azt javasoljuk, hogy nem használja a helyi SSD az alkalmazásadatok tárolására, mert azt az Azure Blob storage nem megőrzött.</span><span class="sxs-lookup"><span data-stu-id="d4a43-228">We recommend that you do not use this local SSD to store your application data because it isn't persisted in Azure Blob storage.</span></span>

<span data-ttu-id="d4a43-229">**Vannak-e bármilyen következményekkel VÁGÁSHOZ a premium lemezek használatát?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-229">**Are there any repercussions for the use of TRIM on premium disks?**</span></span>

<span data-ttu-id="d4a43-230">Nincs vágás használatához Azure-lemezeket vagy Premium vagy a standard lemezek hátránya van.</span><span class="sxs-lookup"><span data-stu-id="d4a43-230">There is no downside to the use of TRIM on Azure disks on either premium or standard disks.</span></span>

## <a name="new-disk-sizes-managed-and-unmanaged"></a><span data-ttu-id="d4a43-231">Új lemezméret: felügyelt és nem felügyelt</span><span class="sxs-lookup"><span data-stu-id="d4a43-231">New disk sizes: Managed and unmanaged</span></span>

<span data-ttu-id="d4a43-232">**Mi az a legnagyobb lemez méretét a támogatott operációs rendszer és az adatlemezek?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-232">**What is the largest disk size supported for operating system and data disks?**</span></span>

<span data-ttu-id="d4a43-233">A partíció típusa, amely Azure támogatja az operációsrendszer-lemez a fő rendszerindító rekord (MBR).</span><span class="sxs-lookup"><span data-stu-id="d4a43-233">The partition type that Azure supports for an operating system disk is the master boot record (MBR).</span></span> <span data-ttu-id="d4a43-234">Egy lemezt a fő rendszertöltő rekord formátum támogatja legfeljebb 2 TB-os méret.</span><span class="sxs-lookup"><span data-stu-id="d4a43-234">The MBR format supports a disk size up to 2 TB.</span></span> <span data-ttu-id="d4a43-235">A legnagyobb, amely Azure támogatja az operációsrendszer-lemez mérete 2 TB.</span><span class="sxs-lookup"><span data-stu-id="d4a43-235">The largest size that Azure supports for an operating system disk is 2 TB.</span></span> <span data-ttu-id="d4a43-236">Azure akár 4 TB adatlemezek támogatja.</span><span class="sxs-lookup"><span data-stu-id="d4a43-236">Azure supports up to 4 TB for data disks.</span></span> 

<span data-ttu-id="d4a43-237">**Mi az a legnagyobb blob mérete támogatott?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-237">**What is the largest page blob size that's supported?**</span></span>

<span data-ttu-id="d4a43-238">A legnagyobb blob mérete, amely támogatja az Azure rendszeren 8 TB (8,191 GB).</span><span class="sxs-lookup"><span data-stu-id="d4a43-238">The largest page blob size that Azure supports is 8 TB (8,191 GB).</span></span> <span data-ttu-id="d4a43-239">Nagyobb, mint 4 TB-os (4095 GB) egy virtuális Géphez csatlakozik, adatok vagy az operációs rendszer lemezek lapblobokat nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="d4a43-239">We don't support page blobs larger than 4 TB (4,095 GB) attached to a VM as data or operating system disks.</span></span>

<span data-ttu-id="d4a43-240">**Kell létrehozni, csatolja, átméretezése és 1 TB-nál nagyobb lemezek feltöltése Azure eszközök új verziójának segítségével?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-240">**Do I need to use a new version of Azure tools to create, attach, resize, and upload disks larger than 1 TB?**</span></span>

<span data-ttu-id="d4a43-241">Nem kell frissíteni a meglévő Azure-eszközök létrehozására, csatolja, és 1 TB-nál nagyobb lemezek átméretezése.</span><span class="sxs-lookup"><span data-stu-id="d4a43-241">You don't need to upgrade your existing Azure tools to create, attach, or resize disks larger than 1 TB.</span></span> <span data-ttu-id="d4a43-242">Töltse fel a VHD-fájl a helyszíni közvetlenül az Azure oldalakra vonatkozó blob vagy nem felügyelt lemezt, kell használnia az eszköz legújabb beállítása:</span><span class="sxs-lookup"><span data-stu-id="d4a43-242">To upload your VHD file from on-premises directly to Azure as a page blob or unmanaged disk, you need to use the latest tool sets:</span></span>

|<span data-ttu-id="d4a43-243">Azure-eszközök</span><span class="sxs-lookup"><span data-stu-id="d4a43-243">Azure tools</span></span>      | <span data-ttu-id="d4a43-244">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="d4a43-244">Supported versions</span></span>                                |
|-----------------|---------------------------------------------------|
|<span data-ttu-id="d4a43-245">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d4a43-245">Azure PowerShell</span></span> | <span data-ttu-id="d4a43-246">4.1.0 verziószám: június 2017 kiadás vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="d4a43-246">Version number 4.1.0: June 2017 release or later</span></span>|
|<span data-ttu-id="d4a43-247">Az Azure CLI 1-es verzió</span><span class="sxs-lookup"><span data-stu-id="d4a43-247">Azure CLI v1</span></span>     | <span data-ttu-id="d4a43-248">Verziószám 0.10.13: Előfordulhat, hogy 2017 kiadás vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="d4a43-248">Version number 0.10.13: May 2017 release or later</span></span>|
|<span data-ttu-id="d4a43-249">AzCopy</span><span class="sxs-lookup"><span data-stu-id="d4a43-249">AzCopy</span></span>           | <span data-ttu-id="d4a43-250">6.1.0 verziószám: június 2017 kiadás vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="d4a43-250">Version number 6.1.0: June 2017 release or later</span></span>|

<span data-ttu-id="d4a43-251">Az Azure parancssori felület v2 és Azure Tártallózó támogatása hamarosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="d4a43-251">The support for Azure CLI v2 and Azure Storage Explorer is coming soon.</span></span> 

<span data-ttu-id="d4a43-252">**Nem felügyelt lemezek vagy lapblobokat támogatott P4 és P6 mérete?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-252">**Are P4 and P6 disk sizes supported for unmanaged disks or page blobs?**</span></span>

<span data-ttu-id="d4a43-253">Nem.</span><span class="sxs-lookup"><span data-stu-id="d4a43-253">No.</span></span> <span data-ttu-id="d4a43-254">P4 (32 GB) és P6 (64 GB-os) lemezméret csak felügyelt lemezek támogatottak.</span><span class="sxs-lookup"><span data-stu-id="d4a43-254">P4 (32 GB) and P6 (64 GB) disk sizes are supported only for managed disks.</span></span> <span data-ttu-id="d4a43-255">Nem felügyelt lemezek és a lapblobokat támogatása hamarosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="d4a43-255">Support for unmanaged disks and page blobs is coming soon.</span></span>

<span data-ttu-id="d4a43-256">**Ha a meglévő prémium szintű felügyelt lemez kevesebb, mint 64 GB-os jött létre a kisebb lemezt (körülbelül 2017. június 15.) engedélyezése előtt, hogyan azt számlázása?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-256">**If my existing premium managed disk less than 64 GB was created before the small disk was enabled (around June 15, 2017), how is it billed?**</span></span>

<span data-ttu-id="d4a43-257">Meglévő kis premium lemezek kisebb, mint 64 GB-os továbbra is fizetnie kell ezért a P10 tarifacsomag alapján.</span><span class="sxs-lookup"><span data-stu-id="d4a43-257">Existing small premium disks less than 64 GB continue to be billed according to the P10 pricing tier.</span></span> 

<span data-ttu-id="d4a43-258">**Hogyan válthat a lemez réteg P4 vagy P6 P10 a 64 GB-nál kisebb kis premium lemezek?**</span><span class="sxs-lookup"><span data-stu-id="d4a43-258">**How can I switch the disk tier of small premium disks less than 64 GB from P10 to P4 or P6?**</span></span>

<span data-ttu-id="d4a43-259">Pillanatkép készítése a kis kapacitású lemezeket, és ezután automatikusan Váltás az árképzési szint P4 vagy a kiosztott mérete alapján P6 lemez létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d4a43-259">You can take a snapshot of your small disks and then create a disk to automatically switch the pricing tier to P4 or P6 based on the provisioned size.</span></span> 


## <a name="what-if-my-question-isnt-answered-here"></a><span data-ttu-id="d4a43-260">Mi történik, ha a fentiekben itt nem választ?</span><span class="sxs-lookup"><span data-stu-id="d4a43-260">What if my question isn't answered here?</span></span>

<span data-ttu-id="d4a43-261">Ha kérdését itt nem látható, ossza meg velünk, és segítünk talált választ.</span><span class="sxs-lookup"><span data-stu-id="d4a43-261">If your question isn't listed here, let us know and we'll help you find an answer.</span></span> <span data-ttu-id="d4a43-262">A megjegyzések is közzétesz egy kérdést, ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="d4a43-262">You can post a question at the end of this article in the comments.</span></span> <span data-ttu-id="d4a43-263">Az Azure Storage csapat és a Közösség más tagjaival kapcsolatos cikkben bevonásához, használja az MSDN [Azure Storage fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span><span class="sxs-lookup"><span data-stu-id="d4a43-263">To engage with the Azure Storage team and other community members about this article, use the MSDN [Azure Storage forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span></span>

<span data-ttu-id="d4a43-264">Kérelem szolgáltatások, a kérelmek és ötleteket a a [Azure Storage-visszajelzési fórumon](https://feedback.azure.com/forums/217298-storage).</span><span class="sxs-lookup"><span data-stu-id="d4a43-264">To request features, submit your requests and ideas to the [Azure Storage feedback forum](https://feedback.azure.com/forums/217298-storage).</span></span>
