<span data-ttu-id="c1038-101">Ellenőrizheti, hogy sikerült-e a kapcsolat hello "Get-AzureVNetConnection" parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="c1038-101">You can verify that your connection succeeded by using hello 'Get-AzureVNetConnection' cmdlet.</span></span>

1. <span data-ttu-id="c1038-102">A következő példa parancsmagban, hello értékek toomatch konfigurálása használata hello saját.</span><span class="sxs-lookup"><span data-stu-id="c1038-102">Use hello following cmdlet example, configuring hello values toomatch your own.</span></span> <span data-ttu-id="c1038-103">szóközöket tartalmaz, a hello virtuális hálózat nevét hello idézőjelek között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c1038-103">hello name of hello virtual network must be in quotes if it contains spaces.</span></span>

  ```powershell
  Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
  ```
2. <span data-ttu-id="c1038-104">Hello parancsmag befejeződését követően hello értékeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="c1038-104">After hello cmdlet has finished, view hello values.</span></span> <span data-ttu-id="c1038-105">Hello az alábbi példában a hello kapcsolati állapota jeleníti meg, mint a "Csatlakoztatott", és láthatja a bemenő és kimenő bájtokat.</span><span class="sxs-lookup"><span data-stu-id="c1038-105">In hello example below, hello Connectivity State shows as 'Connected' and you can see ingress and egress bytes.</span></span>

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : hello connectivity state for hello local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal