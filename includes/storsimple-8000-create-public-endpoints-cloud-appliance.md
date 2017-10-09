#### <a name="toocreate-public-endpoints-on-hello-cloud-appliance"></a><span data-ttu-id="d1313-101">nyilvános végpontok toocreate hello felhő készüléken</span><span class="sxs-lookup"><span data-stu-id="d1313-101">toocreate public endpoints on hello cloud appliance</span></span>

1. <span data-ttu-id="d1313-102">Bejelentkezés toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="d1313-102">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="d1313-103">Nyissa meg túl**virtuális gépek**, majd válassza ki, és kattintson a hello virtuális gépet, amely a felhő készülék használatos.</span><span class="sxs-lookup"><span data-stu-id="d1313-103">Go too**Virtual Machines**, and then select and click hello virtual machine that is being used as your cloud appliance.</span></span>
    
3. <span data-ttu-id="d1313-104">A virtuális gép mindkét kell toocreate egy hálózati biztonsági csoport (NSG) szabály toocontrol hello az forgalom áramlását.</span><span class="sxs-lookup"><span data-stu-id="d1313-104">You need toocreate a network security group (NSG) rule toocontrol hello flow of traffic in and out of your virtual machine.</span></span> <span data-ttu-id="d1313-105">Hajtsa végre a következő lépéseket toocreate egy NSG-szabály hello.</span><span class="sxs-lookup"><span data-stu-id="d1313-105">Perform hello following steps toocreate an NSG rule.</span></span>
    1. <span data-ttu-id="d1313-106">Válassza a **Hálózati biztonsági csoport** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d1313-106">Select **Network security group**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt1.png)

    2. <span data-ttu-id="d1313-107">Kattintson a hello alapértelmezett hálózati biztonsági csoportot, amely számára jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d1313-107">Click hello default network security group that is presented.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt2.png)

    3. <span data-ttu-id="d1313-108">Válassza a **Bejövő biztonsági szabályok** elemet.</span><span class="sxs-lookup"><span data-stu-id="d1313-108">Select **Inbound security rules**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt3.png)

    4. <span data-ttu-id="d1313-109">Kattintson a **+ Hozzáadás** toocreate egy bejövő biztonsági szabály.</span><span class="sxs-lookup"><span data-stu-id="d1313-109">Click **+ Add** toocreate an inbound security rule.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt4.png)

        <span data-ttu-id="d1313-110">Hello Hozzáadás bejövő biztonsági szabály panelen:</span><span class="sxs-lookup"><span data-stu-id="d1313-110">In hello Add inbound security rule blade:</span></span>

        1. <span data-ttu-id="d1313-111">A hello **neve**, hello végpont típusa hello következő nevet: WinRMHttps.</span><span class="sxs-lookup"><span data-stu-id="d1313-111">For hello **Name**, type hello following name for hello endpoint: WinRMHttps.</span></span>
        
        2. <span data-ttu-id="d1313-112">A hello **prioritás**, jelölje be több kisebb, mint 1000 (amely hello prioritását hello alapértelmezett szabály).</span><span class="sxs-lookup"><span data-stu-id="d1313-112">For hello **Priority**, select a number lesser than 1000 (which is hello priority for hello default rule).</span></span> <span data-ttu-id="d1313-113">Nagyobb hello érték, hello alacsonyabb prioritású.</span><span class="sxs-lookup"><span data-stu-id="d1313-113">Higher hello value, lower hello priority.</span></span>

        3. <span data-ttu-id="d1313-114">Set hello **forrás** túl**bármely**.</span><span class="sxs-lookup"><span data-stu-id="d1313-114">Set hello **Source** too**Any**.</span></span>

        4. <span data-ttu-id="d1313-115">A hello **szolgáltatás**, jelölje be **WinRM**.</span><span class="sxs-lookup"><span data-stu-id="d1313-115">For hello **Service**, select **WinRM**.</span></span> <span data-ttu-id="d1313-116">Hello **protokoll** automatikusan értéke túl**TCP** és hello **porttartomány** értéke túl**5986-os**.</span><span class="sxs-lookup"><span data-stu-id="d1313-116">hello **Protocol** is automatically set too**TCP** and hello **Port range** is set too**5986**.</span></span>

        5. <span data-ttu-id="d1313-117">Kattintson a **OK** toocreate hello szabály.</span><span class="sxs-lookup"><span data-stu-id="d1313-117">Click **OK** toocreate hello rule.</span></span>

            ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt5.png)

4. <span data-ttu-id="d1313-118">Az utolsó lépés a hálózati biztonsági csoport egy alhálózatot vagy egy adott hálózati illesztőn tooassociate.</span><span class="sxs-lookup"><span data-stu-id="d1313-118">Your final step is tooassociate your network security group with a subnet or a specific network interface.</span></span> <span data-ttu-id="d1313-119">Hajtsa végre a következő lépéseket tooassociate hello egy alhálózatot a hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="d1313-119">Perform hello following steps tooassociate your network security group with a subnet.</span></span>
    1. <span data-ttu-id="d1313-120">Nyissa meg túl**alhálózatok**.</span><span class="sxs-lookup"><span data-stu-id="d1313-120">Go too**Subnets**.</span></span>
    2. <span data-ttu-id="d1313-121">Kattintson a **+ Társítás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d1313-121">Click **+ Associate**.</span></span>
        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt7.png)

    3. <span data-ttu-id="d1313-122">Válassza ki a virtuális hálózatot, majd válassza ki a megfelelő alhálózati hello.</span><span class="sxs-lookup"><span data-stu-id="d1313-122">Select your virtual network, and then select hello appropriate subnet.</span></span>
    4. <span data-ttu-id="d1313-123">Kattintson a **OK** toocreate hello szabály.</span><span class="sxs-lookup"><span data-stu-id="d1313-123">Click **OK** toocreate hello rule.</span></span>

        ![](./media/storsimple-8000-create-public-endpoints-cloud-appliance/sca-create-public-endpt11.png)

<span data-ttu-id="d1313-124">Hello szabály létrehozása után megtekintheti az részletek toodetermine hello nyilvános virtuális IP-cím (VIP) címét.</span><span class="sxs-lookup"><span data-stu-id="d1313-124">After hello rule is created, you can view its details toodetermine hello Public Virtual IP (VIP) address.</span></span> <span data-ttu-id="d1313-125">Jegyezze fel ezt az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="d1313-125">Record this address.</span></span>


