### <a name="prerequisites"></a><span data-ttu-id="b084b-101">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b084b-101">Prerequisites</span></span>
* <span data-ttu-id="b084b-102">Az Azure-fiók; létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free)</span><span class="sxs-lookup"><span data-stu-id="b084b-102">An Azure account; you can create a [free account](https://azure.microsoft.com/free)</span></span>
* <span data-ttu-id="b084b-103">Egy [Azure Blob Storage-fiók](../articles/storage/common/storage-create-storage-account.md) például hello tárfiók neve és a hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="b084b-103">An [Azure Blob Storage account](../articles/storage/common/storage-create-storage-account.md) including hello storage account name, and its access key.</span></span> <span data-ttu-id="b084b-104">Ez az információ hello tulajdonságainak hello storage-fiókot hello Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b084b-104">This information is listed in hello properties of hello storage account in hello Azure portal.</span></span> <span data-ttu-id="b084b-105">Tudjon meg többet az [Azure Storage](../articles/storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b084b-105">Read more about [Azure Storage](../articles/storage/common/storage-introduction.md).</span></span>

<span data-ttu-id="b084b-106">A logikai alkalmazást az Azure Blob Storage-fiók használatához csatlakozás tooyour Azure Blob Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="b084b-106">Before using your Azure Blob Storage account in a logic app, connect tooyour Azure Blob Storage account.</span></span> <span data-ttu-id="b084b-107">Ehhez egyszerűen a logikai alkalmazásban a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b084b-107">You can do this easily within your logic app on hello Azure  portal.</span></span>  

<span data-ttu-id="b084b-108">Csatlakozás tooyour Azure Blob Storage-fiók használatával hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b084b-108">Connect tooyour Azure Blob Storage account using hello following steps:</span></span>  

1. <span data-ttu-id="b084b-109">Logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b084b-109">Create a logic app.</span></span> <span data-ttu-id="b084b-110">Hello Logic Apps-tervezőben adja hozzá egy eseményindító, és adja hozzá az műveletet.</span><span class="sxs-lookup"><span data-stu-id="b084b-110">In hello Logic Apps designer, add a trigger, and then add an action.</span></span> <span data-ttu-id="b084b-111">Válassza ki **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, és adja meg "blob" hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="b084b-111">Select **Show Microsoft managed APIs** in hello drop down list, and then enter "blob" in hello search box.</span></span> <span data-ttu-id="b084b-112">Hello műveletek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="b084b-112">Select one of hello actions:</span></span>  
   
    ![Az Azure Blob Storage kapcsolat létrehozását lépést](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  
2. <span data-ttu-id="b084b-114">Ha nem korábban hozott létre a kapcsolatokat tooAzure tárolási, kéri hello csatlakozási adatait:</span><span class="sxs-lookup"><span data-stu-id="b084b-114">If you haven't previously created any connections tooAzure storage, you are prompted for hello connection details:</span></span>   
   
    ![Az Azure Blob Storage kapcsolat létrehozását lépést](./media/connectors-create-api-azureblobstorage/connection-details.png)  
3. <span data-ttu-id="b084b-116">Adja meg a hello tárfiókadatok.</span><span class="sxs-lookup"><span data-stu-id="b084b-116">Enter hello storage account details.</span></span> <span data-ttu-id="b084b-117">Tulajdonságok csillaggal szükség.</span><span class="sxs-lookup"><span data-stu-id="b084b-117">Properties with an asterisk are required.</span></span>
   
   | <span data-ttu-id="b084b-118">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b084b-118">Property</span></span> | <span data-ttu-id="b084b-119">Részletek</span><span class="sxs-lookup"><span data-stu-id="b084b-119">Details</span></span> |
   | --- | --- |
   | <span data-ttu-id="b084b-120">Kapcsolat neve *</span><span class="sxs-lookup"><span data-stu-id="b084b-120">Connection Name *</span></span> |<span data-ttu-id="b084b-121">Adjon meg egy tetszőleges nevet a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="b084b-121">Enter any name for your connection.</span></span> |
   | <span data-ttu-id="b084b-122">Az Azure Storage-fiók neve *</span><span class="sxs-lookup"><span data-stu-id="b084b-122">Azure Storage Account Name *</span></span> |<span data-ttu-id="b084b-123">Adja meg a hello tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="b084b-123">Enter hello storage account name.</span></span> <span data-ttu-id="b084b-124">hello tárfióknév hello tárolási tulajdonságok hello Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b084b-124">hello storage account name is displayed in hello storage properties in hello Azure portal.</span></span> |
   | <span data-ttu-id="b084b-125">Azure Storage-fiók hozzáférési kulcs *</span><span class="sxs-lookup"><span data-stu-id="b084b-125">Azure Storage Account Access Key *</span></span> |<span data-ttu-id="b084b-126">Adja meg a hello tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="b084b-126">Enter hello storage account key.</span></span> <span data-ttu-id="b084b-127">hello hívóbetűk hello tárolási tulajdonságok hello Azure-portálon jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b084b-127">hello access keys are displayed in hello storage properties in hello Azure portal.</span></span> |
   
    <span data-ttu-id="b084b-128">Ezek a hitelesítő adatok használt tooauthorize a logic app tooconnect, és hozzáférhet az adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="b084b-128">These credentials are used tooauthorize your logic app tooconnect, and access your data.</span></span> 
4. <span data-ttu-id="b084b-129">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b084b-129">Select **Create**.</span></span>
5. <span data-ttu-id="b084b-130">Értesítés hello kapcsolat létrejött.</span><span class="sxs-lookup"><span data-stu-id="b084b-130">Notice hello connection has been created.</span></span> <span data-ttu-id="b084b-131">Most, folytassa a Logic Apps alkalmazást hello más szükséges lépések:</span><span class="sxs-lookup"><span data-stu-id="b084b-131">Now, proceed with hello other steps in your logic app:</span></span> 
   
    ![Az Azure Blob Storage kapcsolat létrehozását lépést](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  
