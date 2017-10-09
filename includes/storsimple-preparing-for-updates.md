<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a><span data-ttu-id="45b20-101">Frissítések előkészítése</span><span class="sxs-lookup"><span data-stu-id="45b20-101">Preparing for updates</span></span>
<span data-ttu-id="45b20-102">Vizsgálat és hello frissítés telepítése előtt az alábbi lépések tooperform hello lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="45b20-102">You will need tooperform hello following steps before you scan and apply hello update:</span></span>

1. <span data-ttu-id="45b20-103">Pillanatkép készítése felhő hello eszközön tárolt adatok.</span><span class="sxs-lookup"><span data-stu-id="45b20-103">Take a cloud snapshot of hello device data.</span></span>
2. <span data-ttu-id="45b20-104">Győződjön meg arról, hogy a vezérlő rögzített IP-címei irányítható, és képes kapcsolódni az toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="45b20-104">Ensure that your controller fixed IPs are routable and can connect toohello Internet.</span></span> <span data-ttu-id="45b20-105">Ezek rögzített IP-címei lesznek használt tooservice frissítések tooyour eszköz.</span><span class="sxs-lookup"><span data-stu-id="45b20-105">These fixed IPs will be used tooservice updates tooyour device.</span></span> <span data-ttu-id="45b20-106">Futtassa a következő parancsmag minden tartományvezérlőn hello Windows PowerShell felületén hello eszköz hello tesztelheti:</span><span class="sxs-lookup"><span data-stu-id="45b20-106">You can test this by running hello following cmdlet on each controller from hello Windows PowerShell interface of hello device:</span></span>
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    <span data-ttu-id="45b20-107">**A Test-Connection csatlakoztatásakor rögzített IP-címek is toohello Internet példa a kimenetre**</span><span class="sxs-lookup"><span data-stu-id="45b20-107">**Sample output for Test-Connection when fixed IPs can connect toohello Internet**</span></span>

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

<span data-ttu-id="45b20-108">Miután ezek manuális előzetes ellenőrzése sikeresen befejeződött, tooscan folytatni, és hello frissítések telepítése.</span><span class="sxs-lookup"><span data-stu-id="45b20-108">After you have successfully completed these manual pre-checks, you can proceed tooscan and install hello updates.</span></span>

