<span data-ttu-id="e8ecc-101">Az Azure virtuális gépek több adatlemez csatolását támogatták.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-101">An Azure virtual machine supports attaching a number of data disks.</span></span> <span data-ttu-id="e8ecc-102">Az optimális teljesítmény érdekében érdemes erősen túlterhelt lemezek számát illetően toolimit hello csatolt toohello virtuális gép tooavoid lehetséges szabályozás.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-102">For optimal performance, you will want toolimit hello number of highly utilized disks attached toohello virtual machine tooavoid possible throttling.</span></span> <span data-ttu-id="e8ecc-103">Ha az összes lemez nem kívánt magas használhatók hello, egy időben hello tárfiók egy nagyobb számú lemezek is támogatja.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-103">If all disks are not being highly utilized at hello same time, hello storage account can support a larger number disks.</span></span>

* <span data-ttu-id="e8ecc-104">**Az Azure Managed lemezek:** felügyelt lemezek maximális száma regionális és hello tárolótípus is függ.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-104">**For Azure Managed Disks:** Managed Disks count limit is regional and also depends on hello storage type.</span></span> <span data-ttu-id="e8ecc-105">alapértelmezett hello és is hello maximális száma 10 000 előfizetésenként, régiónként és egy tárolási típust.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-105">hello default and also hello maximum limit is 10,000 per subscription, per region and per storage type.</span></span> <span data-ttu-id="e8ecc-106">Például be too10 hozhat létre, 000 szabványos felügyelt lemezeket, és 10 000 premium lemezek egy előfizetésben és régióban felügyelt.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-106">For example, you can create up too10,000 standard managed disks and also 10,000 premium managed disks in a subscription and in a region.</span></span> 

    <span data-ttu-id="e8ecc-107">Felügyelt pillanatképek és a képeket elleni felügyelt lemezek korlátozása hello bájtjai számítanak.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-107">Managed Snapshots and Images are counted against hello Managed Disks limit.</span></span>

* <span data-ttu-id="e8ecc-108">**Standard szintű tárfiókok esetén:** A Standard szintű tárfiókok összesen legfeljebb 20 000 IOPS kérelemaránnyal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-108">**For standard storage accounts:** A standard storage account has a maximum total request rate of 20,000 IOPS.</span></span> <span data-ttu-id="e8ecc-109">hello összes egy standard szintű tárfiók a virtuális gép lemezeinek teljes iops-érték nem haladhatja meg ezt a határt.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-109">hello total IOPS across all of your virtual machine disks in a standard storage account should not exceed this limit.</span></span>
  
    <span data-ttu-id="e8ecc-110">Nagyjából kiszámításához hello erősen túlterhelt lemezek hello kérelem sávszélesség-korlátjának alapján egységes tárfiók által támogatott.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-110">You can roughly calculate hello number of highly utilized disks supported by a single standard storage account based on hello request rate limit.</span></span> <span data-ttu-id="e8ecc-111">Például egy virtuális gép, az alapszintű csomag hello maximális száma a magas be a lemezek készül 66 (20 000/300 iops-érték lemezenként), pedig a Standard szint virtuális gépek készül 40 (20 000 500 iops-érték lemezenként), hello az alábbi táblázatban ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-111">For example, for a Basic Tier VM, hello maximum number of highly utilized disks is about 66 (20,000/300 IOPS per disk), and for a Standard Tier VM, it is about 40 (20,000/500 IOPS per disk), as shown in hello table below.</span></span> 
* <span data-ttu-id="e8ecc-112">**Prémium szintű tárfiókok esetén:** A prémium szintű tárfiókok összesen legfeljebb 50 Gbps átviteli sebességgel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-112">**For premium storage accounts:** A premium storage account has a maximum total throughput rate of 50 Gbps.</span></span> <span data-ttu-id="e8ecc-113">hello összes, a virtuális lemezek teljes átviteli sebesség nem haladhatja meg ezt a határt.</span><span class="sxs-lookup"><span data-stu-id="e8ecc-113">hello total throughput across all of your VM disks should not exceed this limit.</span></span>
