


## <a name="tagging-a-virtual-machine-through-templates"></a><span data-ttu-id="0849f-101">A sablonok használatával virtuálisgép-címkézés</span><span class="sxs-lookup"><span data-stu-id="0849f-101">Tagging a Virtual Machine through Templates</span></span>
<span data-ttu-id="0849f-102">Első lépésként nézzük címkézés sablonok keresztül.</span><span class="sxs-lookup"><span data-stu-id="0849f-102">First, let’s look at tagging through templates.</span></span> <span data-ttu-id="0849f-103">[Ez a sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) címkék helyez el a következőket: számítási (virtuális gép), a tároló (Tárfiók) és a hálózaton (a nyilvános IP-cím, a virtuális hálózati és a hálózati adapter).</span><span class="sxs-lookup"><span data-stu-id="0849f-103">[This template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) places tags on the following resources: Compute (Virtual Machine), Storage (Storage Account), and Network (Public IP Address, Virtual Network, and Network Interface).</span></span> <span data-ttu-id="0849f-104">Ez a sablon a Windows virtuális gépek azonban Linux virtuális gépek módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="0849f-104">This template is for a Windows VM but can be adapted for Linux VMs.</span></span>

<span data-ttu-id="0849f-105">Kattintson a **az Azure telepítéséhez** gombot a [Sablonhivatkozás](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags).</span><span class="sxs-lookup"><span data-stu-id="0849f-105">Click the **Deploy to Azure** button from the [template link](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags).</span></span> <span data-ttu-id="0849f-106">Ez jelenik meg, hogy a [Azure-portálon](https://portal.azure.com/) sablon telepíthető.</span><span class="sxs-lookup"><span data-stu-id="0849f-106">This will navigate to the [Azure portal](https://portal.azure.com/) where you can deploy this template.</span></span>

![Egyszerű telepítés címkékkel](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

<span data-ttu-id="0849f-108">Ez a sablon tartalmazza a következő címkékkel: *részleg*, *alkalmazás*, és *létrehozta*.</span><span class="sxs-lookup"><span data-stu-id="0849f-108">This template includes the following tags: *Department*, *Application*, and *Created By*.</span></span> <span data-ttu-id="0849f-109">Akkor is felvehet és szerkeszthet ezekkel a címkékkel, közvetlenül a sablonban, ha azt szeretné, hogy különböző címkenevek.</span><span class="sxs-lookup"><span data-stu-id="0849f-109">You can add/edit these tags directly in the template if you would like different tag names.</span></span>

![Egy Azure címkék](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

<span data-ttu-id="0849f-111">Ahogy látja, a címkék is meg van adva kulcs/érték párok, egymástól kettősponttal (:).</span><span class="sxs-lookup"><span data-stu-id="0849f-111">As you can see, the tags are defined as key/value pairs, separated by a colon (:).</span></span> <span data-ttu-id="0849f-112">A címkék a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="0849f-112">The tags must be defined in this format:</span></span>

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

<span data-ttu-id="0849f-113">A sablonfájl mentési a címkékkel, az Ön által választott a Szerkesztés befejezése után.</span><span class="sxs-lookup"><span data-stu-id="0849f-113">Save the template file after you finish editing it with the tags of your choice.</span></span>

<span data-ttu-id="0849f-114">Ezután a **paraméterek szerkesztése** szakaszban kitöltheti az értékeket a címkék.</span><span class="sxs-lookup"><span data-stu-id="0849f-114">Next, in the **Edit Parameters** section, you can fill out the values for your tags.</span></span>

![Címkék szerkesztése az Azure-portálon](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

<span data-ttu-id="0849f-116">Kattintson a **létrehozása** a címke értékekkel a sablon telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0849f-116">Click **Create** to deploy this template with your tag values.</span></span>

## <a name="tagging-through-the-portal"></a><span data-ttu-id="0849f-117">A portálon keresztül címkézés</span><span class="sxs-lookup"><span data-stu-id="0849f-117">Tagging through the Portal</span></span>
<span data-ttu-id="0849f-118">Miután létrehozta az erőforrások címkékkel, megtekintheti, vegye fel, és a portál címkék törlése.</span><span class="sxs-lookup"><span data-stu-id="0849f-118">After creating your resources with tags, you can view, add, and delete tags in the portal.</span></span>

<span data-ttu-id="0849f-119">Válassza ki a címkék ikonra a címkék megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="0849f-119">Select the tags icon to view your tags:</span></span>

![Azure-portálon címkék ikon](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

<span data-ttu-id="0849f-121">A portálon keresztül új címke hozzáadása a saját kulcs/érték pár meghatározásával, és mentse.</span><span class="sxs-lookup"><span data-stu-id="0849f-121">Add a new tag through the portal by defining your own Key/Value pair, and save it.</span></span>

![Új címke hozzáadása az Azure-portálon](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

<span data-ttu-id="0849f-123">Az új címke ekkor meg kell jelennie az erőforrás címkék listájában.</span><span class="sxs-lookup"><span data-stu-id="0849f-123">Your new tag should now appear in the list of tags for your resource.</span></span>

![Új címke az Azure portálon mentése](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

