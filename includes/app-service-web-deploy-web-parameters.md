<span data-ttu-id="b88eb-101">Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="b88eb-101">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="b88eb-102">hello sablon tartalmazza az összes hello paraméterértékek nevű paraméterek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="b88eb-102">hello template includes a section called Parameters that contains all of hello parameter values.</span></span>
<span data-ttu-id="b88eb-103">Ezeket az értékeket, amelyek eltérőek a hello projektet telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="b88eb-103">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="b88eb-104">Paraméterek nem adják meg, az értékek, amelyek mindig maradnak hello azonos.</span><span class="sxs-lookup"><span data-stu-id="b88eb-104">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="b88eb-105">Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.</span><span class="sxs-lookup"><span data-stu-id="b88eb-105">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span> 

<span data-ttu-id="b88eb-106">Paraméterek definiálásakor használja hello **Storageaccount_accounttype** mező toospecify, amely a felhasználó értékek biztosíthat a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="b88eb-106">When defining parameters, use hello **allowedValues** field toospecify which values a user can provide during deployment.</span></span> <span data-ttu-id="b88eb-107">Használjon hello **defaultValue** mező tooassign érték toohello paraméter, ha nincs érték megadva üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="b88eb-107">Use hello **defaultValue** field tooassign a value toohello parameter, if no value is provided during deployment.</span></span>

<span data-ttu-id="b88eb-108">Azt ismerteti, minden paraméter hello sablonban.</span><span class="sxs-lookup"><span data-stu-id="b88eb-108">We will describe each parameter in hello template.</span></span>

### <a name="sitename"></a><span data-ttu-id="b88eb-109">SiteName</span><span class="sxs-lookup"><span data-stu-id="b88eb-109">siteName</span></span>
<span data-ttu-id="b88eb-110">hello web app, hogy kívánja-e toocreate hello neve.</span><span class="sxs-lookup"><span data-stu-id="b88eb-110">hello name of hello web app that you wish toocreate.</span></span>

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a><span data-ttu-id="b88eb-111">hostingPlanName</span><span class="sxs-lookup"><span data-stu-id="b88eb-111">hostingPlanName</span></span>
<span data-ttu-id="b88eb-112">App Service hello hello neve megtervezése toouse hello a webalkalmazás üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="b88eb-112">hello name of hello App Service plan toouse for hosting hello web app.</span></span>

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a><span data-ttu-id="b88eb-113">Termékváltozat</span><span class="sxs-lookup"><span data-stu-id="b88eb-113">sku</span></span>
<span data-ttu-id="b88eb-114">hello üzemeltetési terv hello tarifacsomagját.</span><span class="sxs-lookup"><span data-stu-id="b88eb-114">hello pricing tier for hello hosting plan.</span></span>

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "hello pricing tier for hello hosting plan."
      }
    }

<span data-ttu-id="b88eb-115">hello sablon hello értékek, amelyeknél engedélyezve van ez a paraméter határozza meg, és hozzárendeli az alapértelmezett értéket (S1), ha nincs érték megadva.</span><span class="sxs-lookup"><span data-stu-id="b88eb-115">hello template defines hello values that are permitted for this parameter, and assigns a default value (S1) if no value is specified.</span></span>

### <a name="workersize"></a><span data-ttu-id="b88eb-116">workerSize</span><span class="sxs-lookup"><span data-stu-id="b88eb-116">workerSize</span></span>
<span data-ttu-id="b88eb-117">hello példányméretének hello üzemeltetési terv (kis, közepes vagy nagy).</span><span class="sxs-lookup"><span data-stu-id="b88eb-117">hello instance size of hello hosting plan (small, medium, or large).</span></span>

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

<span data-ttu-id="b88eb-118">hello sablon határozza meg, amelyeknél engedélyezve van ez a paraméter (0, 1 vagy 2) a hello értékeket, és hozzárendeli az alapértelmezett értéket (0), ha nincs érték megadva.</span><span class="sxs-lookup"><span data-stu-id="b88eb-118">hello template defines hello values that are permitted for this parameter (0, 1, or 2), and assigns a default value (0) if no value is specified.</span></span> <span data-ttu-id="b88eb-119">hello értékek toosmall, közepes és nagy felelnek meg.</span><span class="sxs-lookup"><span data-stu-id="b88eb-119">hello values correspond toosmall, medium and large.</span></span>

