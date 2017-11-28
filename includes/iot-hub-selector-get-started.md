> [!div class="op_single_selector"]
> * [<span data-ttu-id="6eefd-101">C#</span><span class="sxs-lookup"><span data-stu-id="6eefd-101">C#</span></span>](../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md)
> * [<span data-ttu-id="6eefd-102">Java</span><span class="sxs-lookup"><span data-stu-id="6eefd-102">Java</span></span>](../articles/iot-hub/iot-hub-java-java-getstarted.md)
> * [<span data-ttu-id="6eefd-103">Node.js</span><span class="sxs-lookup"><span data-stu-id="6eefd-103">Node.js</span></span>](../articles/iot-hub/iot-hub-node-node-getstarted.md)
> * [<span data-ttu-id="6eefd-104">Python</span><span class="sxs-lookup"><span data-stu-id="6eefd-104">Python</span></span>](../articles/iot-hub/iot-hub-python-getstarted.md)
> 
> 

## <a name="introduction"></a><span data-ttu-id="6eefd-105">Introduction (Bevezetés)</span><span class="sxs-lookup"><span data-stu-id="6eefd-105">Introduction</span></span>
<span data-ttu-id="6eefd-106">Az Azure IoT Hub egy teljesen felügyelt szolgáltatás, amely megbízható és biztonságos kétirányú kommunikációt tesz lehetővé az eszközök internetes hálózatához (IoT) csatlakozó több millió eszköz között, valamint megoldást biztosít a háttérrendszer kialakításához.</span><span class="sxs-lookup"><span data-stu-id="6eefd-106">Azure IoT Hub is a fully managed service that enables reliable and secure bi-directional communications between millions of Internet of Things (IoT) devices and a solution back end.</span></span> <span data-ttu-id="6eefd-107">Az IoT-projektek számára az egyik legnagyobb kihívást az jelenti, hogyan lehet megbízható és biztonságos módon csatlakoztatni az eszközöket a megoldás háttérrendszeréhez.</span><span class="sxs-lookup"><span data-stu-id="6eefd-107">One of the biggest challenges that IoT projects face is how to reliably and securely connect devices to the solution back end.</span></span> <span data-ttu-id="6eefd-108">A kihívás megoldására az IoT Hub a következőket kínálja:</span><span class="sxs-lookup"><span data-stu-id="6eefd-108">To address this challenge, IoT Hub:</span></span>

* <span data-ttu-id="6eefd-109">Megbízható nagy kapacitású üzenetkezelést kínál az eszközök és a felhő között mindkét irányban.</span><span class="sxs-lookup"><span data-stu-id="6eefd-109">Offers reliable device-to-cloud and cloud-to-device hyper-scale messaging.</span></span>
* <span data-ttu-id="6eefd-110">Az eszközönkénti biztonsági hitelesítő adatok és hozzáférés-vezérlés segítségével lehetővé teszi a biztonságos kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="6eefd-110">Enables secure communications using per-device security credentials and access control.</span></span>
* <span data-ttu-id="6eefd-111">Tartalmazza a legnépszerűbb nyelvek és platformok eszközkönyvtárait.</span><span class="sxs-lookup"><span data-stu-id="6eefd-111">Includes device libraries for the most popular languages and platforms.</span></span>

<span data-ttu-id="6eefd-112">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="6eefd-112">This tutorial shows you how to:</span></span>

* <span data-ttu-id="6eefd-113">Egy IoT Hub létrehozása az Azure Portallal.</span><span class="sxs-lookup"><span data-stu-id="6eefd-113">Use the Azure portal to create an IoT hub.</span></span>
* <span data-ttu-id="6eefd-114">Eszközidentitás létrehozása az IoT Hubban.</span><span class="sxs-lookup"><span data-stu-id="6eefd-114">Create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="6eefd-115">Egy szimulált eszközalkalmazás létrehozása, amely telemetriai adatokat küld a megoldás háttérrendszerének.</span><span class="sxs-lookup"><span data-stu-id="6eefd-115">Create a simulated device app that sends telemetry to your solution back end.</span></span>

