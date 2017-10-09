1. <span data-ttu-id="70b3b-101">Jelentkezzen be toohello [Azure-portálon][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="70b3b-101">Log on toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="70b3b-102">Hello portal hello bal oldali navigációs ablaktábláján kattintson **új**, majd kattintson a **vállalati integrációs**, és kattintson a **továbbítási**.</span><span class="sxs-lookup"><span data-stu-id="70b3b-102">In hello left navigation pane of hello portal, click **New**, then click **Enterprise Integration**, and then click **Relay**.</span></span>
3. <span data-ttu-id="70b3b-103">A hello **névtér létrehozása** párbeszédpanelen adja meg a névtér nevét.</span><span class="sxs-lookup"><span data-stu-id="70b3b-103">In hello **Create namespace** dialog, enter a namespace name.</span></span> <span data-ttu-id="70b3b-104">hello rendszer azonnal ellenőrzi toosee, ha hello neve érhető el.</span><span class="sxs-lookup"><span data-stu-id="70b3b-104">hello system immediately checks toosee if hello name is available.</span></span>
4. <span data-ttu-id="70b3b-105">A hello **előfizetés** mezőben válassza ki az Azure-előfizetés mely toocreate hello névtérben.</span><span class="sxs-lookup"><span data-stu-id="70b3b-105">In hello **Subscription** field, choose an Azure subscription in which toocreate hello namespace.</span></span>
5. <span data-ttu-id="70b3b-106">A hello  **[erőforráscsoport](../articles/azure-resource-manager/resource-group-portal.md)**  mezőben válasszon egy meglévő erőforráscsoportot, mely hello névtér lesz élő, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="70b3b-106">In hello **[Resource group](../articles/azure-resource-manager/resource-group-portal.md)** field, choose an existing resource group in which hello namespace will live, or create a new one.</span></span>      
6. <span data-ttu-id="70b3b-107">A **hely**, válassza ki a hello országban vagy régióban, amelyben a névtér üzemeltetve lesz.</span><span class="sxs-lookup"><span data-stu-id="70b3b-107">In **Location**, choose hello country or region in which your namespace should be hosted.</span></span>
   
    ![Névtér létrehozása][create-namespace]
7. <span data-ttu-id="70b3b-109">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="70b3b-109">Click **Create**.</span></span> <span data-ttu-id="70b3b-110">hello rendszer most hoz létre a névteret, majd engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="70b3b-110">hello system now creates your namespace and enables it.</span></span> <span data-ttu-id="70b3b-111">Néhány perc elteltével hello rendszer kiosztja az erőforrásokat a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="70b3b-111">After a few minutes, hello system provisions resources for your account.</span></span>

### <a name="obtain-hello-management-credentials"></a><span data-ttu-id="70b3b-112">Hello felügyeleti hitelesítő adatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="70b3b-112">Obtain hello management credentials</span></span>
1. <span data-ttu-id="70b3b-113">Hello névterek, kattintson az újonnan létrehozott névtér neve hello.</span><span class="sxs-lookup"><span data-stu-id="70b3b-113">In hello list of namespaces, click hello newly created namespace name.</span></span>
2. <span data-ttu-id="70b3b-114">Hello névtér paneljén kattintson **megosztott elérési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="70b3b-114">In hello namespace blade, click **Shared access policies**.</span></span>
3. <span data-ttu-id="70b3b-115">A hello **megosztott elérési házirendek** panelen kattintson a **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="70b3b-115">In hello **Shared access policies** blade, click **RootManageSharedAccessKey**.</span></span>
   
    ![connection-info][connection-info]
4. <span data-ttu-id="70b3b-117">A hello **házirend: RootManageSharedAccessKey** panelen kattintson a Másolás gombra hello tovább túl**kapcsolati karakterlánc – elsődleges kulcs**, toocopy hello kapcsolati karakterlánc tooyour vágólapra későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="70b3b-117">In hello **Policy: RootManageSharedAccessKey** blade, click hello copy button next too**Connection string–primary key**, toocopy hello connection string tooyour clipboard for later use.</span></span> <span data-ttu-id="70b3b-118">Illessze be ezt az értéket a Jegyzettömbbe vagy egy másik ideiglenes helyre.</span><span class="sxs-lookup"><span data-stu-id="70b3b-118">Paste this value into Notepad or some other temporary location.</span></span>
   
    ![connection-string][connection-string]

5. <span data-ttu-id="70b3b-120">Ismétlődő hello előző lépést, másolás és beillesztés hello értékének **elsődleges kulcs** tooa ideiglenes helyre későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="70b3b-120">Repeat hello previous step, copying and pasting hello value of **Primary key** tooa temporary location for later use.</span></span>  

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
