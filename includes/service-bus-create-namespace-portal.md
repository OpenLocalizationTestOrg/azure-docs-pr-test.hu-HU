<span data-ttu-id="9f1f2-101">a Service Bus használatával toobegin várólisták az Azure-ban, akkor először létre kell hoznia egy névtér esetében Azure egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-101">toobegin using Service Bus queues in Azure, you must first create a namespace with a name that is unique across Azure.</span></span> <span data-ttu-id="9f1f2-102">A névtér egy hatókörkezelési tárolót biztosít a Service Bus erőforrásainak címzéséhez az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-102">A namespace provides a scoping container for addressing Service Bus resources within your application.</span></span>

<span data-ttu-id="9f1f2-103">egy névtér toocreate:</span><span class="sxs-lookup"><span data-stu-id="9f1f2-103">toocreate a namespace:</span></span>

1. <span data-ttu-id="9f1f2-104">Jelentkezzen be toohello [Azure-portálon][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="9f1f2-104">Log on toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="9f1f2-105">Hello portal hello bal oldali navigációs ablaktábláján kattintson **új**, majd kattintson a **vállalati integrációs**, és kattintson a **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-105">In hello left navigation pane of hello portal, click **New**, then click **Enterprise Integration**, and then click **Service Bus**.</span></span>
3. <span data-ttu-id="9f1f2-106">A hello **névtér létrehozása** párbeszédpanelen adja meg a névtér nevét.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-106">In hello **Create namespace** dialog, enter a namespace name.</span></span> <span data-ttu-id="9f1f2-107">hello rendszer azonnal ellenőrzi toosee, ha hello neve érhető el.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-107">hello system immediately checks toosee if hello name is available.</span></span>
4. <span data-ttu-id="9f1f2-108">Miután meggyőződött arról hello név nem érhető el, válassza ki azt a hello tarifacsomag (alapszintű, Standard vagy prémium).</span><span class="sxs-lookup"><span data-stu-id="9f1f2-108">After making sure hello namespace name is available, choose hello pricing tier (Basic, Standard, or Premium).</span></span>
5. <span data-ttu-id="9f1f2-109">A hello **előfizetés** mezőben válassza ki az Azure-előfizetés mely toocreate hello névtérben.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-109">In hello **Subscription** field, choose an Azure subscription in which toocreate hello namespace.</span></span>
6. <span data-ttu-id="9f1f2-110">A hello **erőforráscsoport** mezőben válasszon egy meglévő erőforráscsoportot, mely hello névtér lesz élő, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-110">In hello **Resource group** field, choose an existing resource group in which hello namespace will live, or create a new one.</span></span>      
7. <span data-ttu-id="9f1f2-111">A **hely**, válassza ki a hello országban vagy régióban, amelyben a névtér üzemeltetve lesz.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-111">In **Location**, choose hello country or region in which your namespace should be hosted.</span></span>
   
    ![Névtér létrehozása][create-namespace]
8. <span data-ttu-id="9f1f2-113">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-113">Click **Create**.</span></span> <span data-ttu-id="9f1f2-114">hello rendszer most hoz létre a névteret, majd engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-114">hello system now creates your namespace and enables it.</span></span> <span data-ttu-id="9f1f2-115">Lehetséges, hogy toowait hello rendszer kiosztja az erőforrásokat, a fiók néhány percig.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-115">You might have toowait several minutes as hello system provisions resources for your account.</span></span>

### <a name="obtain-hello-management-credentials"></a><span data-ttu-id="9f1f2-116">Hello felügyeleti hitelesítő adatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="9f1f2-116">Obtain hello management credentials</span></span>

1. <span data-ttu-id="9f1f2-117">Hello névterek, kattintson az újonnan létrehozott névtér neve hello.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-117">In hello list of namespaces, click hello newly created namespace name.</span></span>
2. <span data-ttu-id="9f1f2-118">Hello névtér paneljén kattintson **megosztott elérési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-118">In hello namespace blade, click **Shared access policies**.</span></span>
3. <span data-ttu-id="9f1f2-119">A hello **megosztott elérési házirendek** panelen kattintson a **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-119">In hello **Shared access policies** blade, click **RootManageSharedAccessKey**.</span></span>
   
    ![connection-info][connection-info]
4. <span data-ttu-id="9f1f2-121">A hello **házirend: RootManageSharedAccessKey** panelen kattintson a Másolás gombra hello tovább túl**kapcsolati karakterlánc – elsődleges kulcs**, toocopy hello kapcsolati karakterlánc tooyour vágólapra későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-121">In hello **Policy: RootManageSharedAccessKey** blade, click hello copy button next too**Connection string–primary key**, toocopy hello connection string tooyour clipboard for later use.</span></span> <span data-ttu-id="9f1f2-122">Illessze be ezt az értéket a Jegyzettömbbe vagy egy másik ideiglenes helyre.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-122">Paste this value into Notepad or some other temporary location.</span></span>
   
    ![connection-string][connection-string]

5. <span data-ttu-id="9f1f2-124">Ismétlődő hello előző lépést, másolás és beillesztés hello értékének **elsődleges kulcs** tooa ideiglenes helyre későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="9f1f2-124">Repeat hello previous step, copying and pasting hello value of **Primary key** tooa temporary location for later use.</span></span>

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
