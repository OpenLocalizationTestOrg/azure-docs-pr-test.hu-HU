<span data-ttu-id="3394a-101">hello lépéseket toounregister egy folyamatkiszolgálóra eltér attól függően, hogy a konfigurációs kiszolgáló hello kapcsolat állapotát.</span><span class="sxs-lookup"><span data-stu-id="3394a-101">hello steps toounregister a process server differs depending on its connection status with hello Configuration Server.</span></span>

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a><span data-ttu-id="3394a-102">Csatlakoztatott állapotban lévő folyamatkiszolgáló regisztrációjának visszavonása</span><span class="sxs-lookup"><span data-stu-id="3394a-102">Unregister a process server that is in a connected state</span></span>

1. <span data-ttu-id="3394a-103">Távoli az hello folyamat kiszolgálóra rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3394a-103">Remote into hello process server as an Administrator.</span></span>
2. <span data-ttu-id="3394a-104">Indítsa el a hello **Vezérlőpult** , és nyissa meg **programok > program eltávolítása**</span><span class="sxs-lookup"><span data-stu-id="3394a-104">Launch hello **Control Panel** and open **Programs > Uninstall a program**</span></span>
3. <span data-ttu-id="3394a-105">Program eltávolítása hello nevű **Microsoft Azure Recovery konfigurációs/folyamat helykiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="3394a-105">Uninstall a program by hello name **Microsoft Azure Site Recovery Configuration/Process Server**</span></span>
4. <span data-ttu-id="3394a-106">A 3. lépés befejezése után eltávolíthatja a **Microsoft Azure Site Recovery Configuration/Process Server Dependencies** elemet.</span><span class="sxs-lookup"><span data-stu-id="3394a-106">Once step 3 is completed, you can uninstall **Microsoft Azure Site Recovery Configuration/Process Server Dependencies**</span></span>

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a><span data-ttu-id="3394a-107">Leválasztott állapotban lévő folyamatkiszolgáló regisztrációjának visszavonása</span><span class="sxs-lookup"><span data-stu-id="3394a-107">Unregister a process server that is in a disconnected state</span></span>

> [!WARNING]
> <span data-ttu-id="3394a-108">Hello használja a következő lépések kell használni, ha nincs módja toorevive hello virtuális gép mely hello folyamat kiszolgáló lett telepítve.</span><span class="sxs-lookup"><span data-stu-id="3394a-108">Use hello below steps should be used if there is no way toorevive hello virtual machine on which hello Process Server was installed.</span></span>

1. <span data-ttu-id="3394a-109">Rendszergazdaként jelentkezzen tooyour konfigurációs kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="3394a-109">Log on tooyour configuration server as an Administrator.</span></span>
2. <span data-ttu-id="3394a-110">Nyisson meg egy rendszergazdai parancssort, és keresse meg a toohello directory `%ProgramData%\ASR\home\svsystems\bin`.</span><span class="sxs-lookup"><span data-stu-id="3394a-110">Open an Administrative command prompt and browse toohello directory `%ProgramData%\ASR\home\svsystems\bin`.</span></span>
3. <span data-ttu-id="3394a-111">Most futtassa hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="3394a-111">Now run hello command.</span></span>

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. <span data-ttu-id="3394a-112">Hello rendszer hello hello folyamat-kiszolgáló adatait az törli.</span><span class="sxs-lookup"><span data-stu-id="3394a-112">This will purge hello details of hello process server from hello system.</span></span>
