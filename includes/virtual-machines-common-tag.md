


## <a name="tagging-a-virtual-machine-through-templates"></a><span data-ttu-id="9e8a5-101">A sablonok használatával virtuálisgép-címkézés</span><span class="sxs-lookup"><span data-stu-id="9e8a5-101">Tagging a Virtual Machine through Templates</span></span>
<span data-ttu-id="9e8a5-102">Első lépésként nézzük címkézés sablonok keresztül.</span><span class="sxs-lookup"><span data-stu-id="9e8a5-102">First, let’s look at tagging through templates.</span></span> <span data-ttu-id="9e8a5-103">[Ez a sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) címkék helyez el a következő erőforrások hello: számítási (virtuális gép), a tároló (Tárfiók) és a hálózaton (a nyilvános IP-cím, a virtuális hálózati és a hálózati adapter).</span><span class="sxs-lookup"><span data-stu-id="9e8a5-103">[This template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) places tags on hello following resources: Compute (Virtual Machine), Storage (Storage Account), and Network (Public IP Address, Virtual Network, and Network Interface).</span></span> <span data-ttu-id="9e8a5-104">Ez a sablon a Windows virtuális gépek azonban Linux virtuális gépek módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="9e8a5-104">This template is for a Windows VM but can be adapted for Linux VMs.</span></span>

<span data-ttu-id="9e8a5-105">Kattintson a hello **tooAzure telepítése** hello a gomb [Sablonhivatkozás](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags).</span><span class="sxs-lookup"><span data-stu-id="9e8a5-105">Click hello **Deploy tooAzure** button from hello [template link](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags).</span></span> <span data-ttu-id="9e8a5-106">Ez jelenik meg a toohello [Azure-portálon](https://portal.azure.com/) sablon telepíthető.</span><span class="sxs-lookup"><span data-stu-id="9e8a5-106">This will navigate toohello [Azure portal](https://portal.azure.com/) where you can deploy this template.</span></span>

![Egyszerű telepítés címkékkel](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

<span data-ttu-id="9e8a5-108">Ez a sablon tartalmazza a következő címkék hello: *részleg*, *alkalmazás*, és *létrehozta*.</span><span class="sxs-lookup"><span data-stu-id="9e8a5-108">This template includes hello following tags: *Department*, *Application*, and *Created By*.</span></span> <span data-ttu-id="9e8a5-109">Akkor is felvehet és szerkeszthet ezekkel a címkékkel hello sablon közvetlenül a Ha azt szeretné, hogy különböző címkenevek.</span><span class="sxs-lookup"><span data-stu-id="9e8a5-109">You can add/edit these tags directly in hello template if you would like different tag names.</span></span>

![Egy Azure címkék](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

<span data-ttu-id="9e8a5-111">Ahogy látja, hello címkék is meg van adva kulcs/érték párok, egymástól kettősponttal (:).</span><span class="sxs-lookup"><span data-stu-id="9e8a5-111">As you can see, hello tags are defined as key/value pairs, separated by a colon (:).</span></span> <span data-ttu-id="9e8a5-112">hello címkék a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="9e8a5-112">hello tags must be defined in this format:</span></span>

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

<span data-ttu-id="9e8a5-113">Hello sablonfájl mentési, az Ön által választott hello címkékkel a Szerkesztés befejezése után.</span><span class="sxs-lookup"><span data-stu-id="9e8a5-113">Save hello template file after you finish editing it with hello tags of your choice.</span></span>

<span data-ttu-id="9e8a5-114">Ezután hello **paraméterek szerkesztése** szakaszban kitöltheti hello értékek a címkék.</span><span class="sxs-lookup"><span data-stu-id="9e8a5-114">Next, in hello **Edit Parameters** section, you can fill out hello values for your tags.</span></span>

![Címkék szerkesztése az Azure-portálon](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

<span data-ttu-id="9e8a5-116">Kattintson a **létrehozása** toodeploy Ez a sablon a címke értékekkel.</span><span class="sxs-lookup"><span data-stu-id="9e8a5-116">Click **Create** toodeploy this template with your tag values.</span></span>

## <a name="tagging-through-hello-portal"></a><span data-ttu-id="9e8a5-117">Címkézés hello portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="9e8a5-117">Tagging through hello Portal</span></span>
<span data-ttu-id="9e8a5-118">Miután létrehozta az erőforrások címkékkel, megtekintheti, vegye fel, és hello portálon címkék törlése.</span><span class="sxs-lookup"><span data-stu-id="9e8a5-118">After creating your resources with tags, you can view, add, and delete tags in hello portal.</span></span>

<span data-ttu-id="9e8a5-119">Jelölje be hello címkéket ikon tooview a címkéket:</span><span class="sxs-lookup"><span data-stu-id="9e8a5-119">Select hello tags icon tooview your tags:</span></span>

![Azure-portálon címkék ikon](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

<span data-ttu-id="9e8a5-121">Hello portálon keresztül új címke hozzáadása a saját kulcs/érték pár meghatározásával, és mentse.</span><span class="sxs-lookup"><span data-stu-id="9e8a5-121">Add a new tag through hello portal by defining your own Key/Value pair, and save it.</span></span>

![Új címke hozzáadása az Azure-portálon](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

<span data-ttu-id="9e8a5-123">Az új címke ekkor meg kell jelennie az erőforrás címkék hello listáját.</span><span class="sxs-lookup"><span data-stu-id="9e8a5-123">Your new tag should now appear in hello list of tags for your resource.</span></span>

![Új címke az Azure portálon mentése](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

