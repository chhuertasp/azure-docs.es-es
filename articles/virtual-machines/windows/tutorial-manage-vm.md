---
title: "Creación y administración de máquinas virtuales Windows con el módulo de Azure PowerShell | Microsoft Docs"
description: "Tutorial: Creación y administración de máquinas virtuales Windows con el módulo de Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/21/2017
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 59d6d646d4ab236d1fffad0cd0ec3e9f3ae4c342
ms.contentlocale: es-es
ms.lasthandoff: 05/03/2017

---

# <a name="create-and-manage-windows-vms-with-the-azure-powershell-module"></a>Creación y administración de máquinas virtuales Windows con el módulo de Azure PowerShell

En este tutorial se tratan elementos básicos de la creación de máquinas virtuales de Azure, como la selección de un tamaño de máquina virtual, la selección de una imagen de máquina virtual y la implementación de una máquina virtual. También se cubren las operaciones básicas de administración, como la administración del estado, la eliminación y el cambio del tamaño de una máquina virtual.

Se pueden completar los pasos de este tutorial con la versión más reciente del módulo [Azure PowerShell](/powershell/azure/overview).

## <a name="create-resource-group"></a>Creación de un grupo de recursos

Cree un grupo de recursos con el comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). 

Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. Se debe crear un grupo de recursos antes de una máquina virtual. En este ejemplo, se crea un grupo de recursos denominado *myResourceGroupVM* en la región *westus*. 

```powershell
New-AzureRmResourceGroup -ResourceGroupName miGrupoRecursoVM -Location westeurope
```

Se especifica el grupo de recursos al crear o modificar una máquina virtual, como se ve a lo largo de este tutorial.

## <a name="create-virtual-machine"></a>Create virtual machine

Una máquina virtual debe estar conectada a una red virtual. Se comunica con la máquina virtual mediante una dirección IP pública a través de una tarjeta de interfaz de red.

### <a name="create-virtual-network"></a>Creación de una red virtual

Cree una subred con [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

Cree una red virtual con [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName miGrupoRecursoVM `
  -Location westeurope `
  -Name miRedVirtual `
  -AddressPrefix 192.168.0.0/16 ` 
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a>Creación de una dirección IP pública

Cree una dirección IP pública con [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```powershell
$pip = New-AzureRmPublicIpAddress ` 
  -ResourceGroupName miGrupoRecursoVM `
  -Location westeurope ` 
  -AllocationMethod Static `
  -Name miDireccionIPPublica
```

### <a name="create-network-interface-card"></a>Creación de una tarjeta de interfaz de red

Cree una tarjeta de interfaz de red con [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```powershell
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName miGrupoRecursoVM  `
  -Location westeurope `
  -Name miNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a>Creación de un grupo de seguridad de red

Un [grupo de seguridad de red](../../virtual-network/virtual-networks-nsg.md) de Azure controla el tráfico entrante y saliente para una o varias máquinas virtuales. Las reglas del grupo de seguridad de red permiten o deniegan el tráfico de red en un puerto específico o en un intervalo de puertos. Estas reglas también pueden incluir un prefijo de dirección de origen para que solo el tráfico que se origine en un origen predefinido pueda comunicarse con una máquina virtual. Para tener acceso al servidor web IIS que está instalando, debe agregar una regla de NSG de entrada.

Para crear una regla de NSG de entrada, use [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig). En el ejemplo siguiente se crea una regla de NSG denominada *myNSGRule* que abre el puerto *3389* para la máquina virtual:

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name miNSGRegla `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow
```

Cree el NSG a través de *miNSGRegla* con [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName miGrupoRecursoVM `
    -Location westeurope `
    -Name miGrupoSeguridadRed `
    -SecurityRules $nsgRule
```

Agregue el NSG a la subred en la red virtual con [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name miSubred `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

Actualice la red virtual con [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a>Create virtual machine

Al crear una máquina virtual, están disponibles en varias alternativas, como las imagenes de sistemas operativos, tamaños de disco y credenciales administrativas. En este ejemplo se crea una máquina virtual con el nombre *miMV* y que ejecuta la versión más reciente de Windows Server 2016.

Establezca el nombre de usuario y la contraseña que se necesitan para la cuenta de administrador en la máquina virtual con [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Cree la configuración inicial de la máquina virtual con [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):

```powershell
$vm = New-AzureRmVMConfig -VMName miMV -VMSize Standard_D1
```

Agregue la información del sistema operativo a la configuración de máquina virtual con [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):

```powershell
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName miMV `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

Agregue la información de imagen del sistema operativo a la configuración de máquina virtual con [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

Agregue la configuración de disco del sistema operativo a la configuración de máquina virtual con [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):

```powershell
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name miDiscoSo `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

Agregue la tarjeta de interfaz de red que creó anteriormente a la configuración de máquina virtual con [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Cree la máquina virtual con [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```powershell
New-AzureRmVM -ResourceGroupName miGrupoRecursoVM -Location westeurope -VM $vm
```

## <a name="connect-to-vm"></a>Conexión a una máquina virtual

Una vez finalizada la implementación, cree una conexión del Escritorio remoto con la máquina virtual.

Ejecute los siguientes comandos para devolver la dirección IP pública de la máquina virtual. Tome nota de esta dirección IP para poder conectarse a ella con un explorador para probar la conectividad web en un paso posterior.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName miGrupoRecursoVM  | Select IpAddress
```

Ejecute el siguiente comando para crear una sesión del Escritorio remoto con la máquina virtual. Reemplace la dirección IP con el valor de *DireccionIPPublica* de la máquina virtual. Cuando se le solicite, escriba las credenciales usadas al crear la máquina virtual.

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a>Descripción de las imágenes de máquina virtual

Azure Marketplace incluye diversas imágenes de máquinas virtuales que se pueden usar para crear una nueva máquina virtual. En los pasos anteriores, se creó una máquina virtual mediante la imagen de Windows Server 2016 Datacenter. En este paso, el módulo de PowerShell se usa para buscar en Marketplace otras imágenes de Windows, que también se pueden usar como base para nuevas máquinas virtuales. Este proceso consiste en buscar el publicador, la oferta y el nombre de imagen (SKU). 

Ejecute el comando [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) para devolver una lista de publicadores de imágenes.  

```powersehll
Get-AzureRmVMImagePublisher -Location "westus"
```

Ejecute el comando [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) para devolver una lista de ofertas de imágenes. Con este comando, la lista devuelta se filtra por el publicador especificado. 

```powershell
Get-AzureRmVMImageOffer -Location "westus" -PublisherName "MicrosoftWindowsServer"
```

```powershell
Offer             PublisherName          Location
-----             -------------          -------- 
Windows-HUB       MicrosoftWindowsServer westus 
WindowsServer     MicrosoftWindowsServer westus   
WindowsServer-HUB MicrosoftWindowsServer westus   
```

El comando [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) filtra entonces por el publicador y el nombre de la oferta para devolver una lista de nombres de imágenes.

```powershell
Get-AzureRmVMImageSku -Location "westus" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```powershell
Skus                            Offer         PublisherName          Location
----                            -----         -------------          --------
2008-R2-SP1                     WindowsServer MicrosoftWindowsServer westus  
2008-R2-SP1-BYOL                WindowsServer MicrosoftWindowsServer westus  
2012-Datacenter                 WindowsServer MicrosoftWindowsServer westus  
2012-Datacenter-BYOL            WindowsServer MicrosoftWindowsServer westus  
2012-R2-Datacenter              WindowsServer MicrosoftWindowsServer westus  
2012-R2-Datacenter-BYOL         WindowsServer MicrosoftWindowsServer westus  
2016-Datacenter                 WindowsServer MicrosoftWindowsServer westus  
2016-Datacenter-Server-Core     WindowsServer MicrosoftWindowsServer westus  
2016-Datacenter-with-Containers WindowsServer MicrosoftWindowsServer westus  
2016-Nano-Server                WindowsServer MicrosoftWindowsServer westus
```

Esta información puede usarse para implementar una máquina virtual con una imagen específica. En este ejemplo se establece el nombre de la imagen en el objeto de máquina virtual. Consulte los ejemplos anteriores de este tutorial para los pasos de implementación completos.

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a>Descripción de los tamaños de máquina virtual

El tamaño de la máquina virtual determina la cantidad de recursos de proceso, como memoria, CPU y GPU, que están disponibles para la máquina virtual. Las máquinas virtuales deben crearse con un tamaño adecuado para la carga de trabajo esperada. Si aumenta la carga de trabajo, se puede cambiar el tamaño de una máquina virtual existente.

### <a name="vm-sizes"></a>Tamaños de máquina virtual

En la tabla siguiente se clasifican los tamaños en casos de uso.  

| Tipo                     | Tamaños           |    Descripción       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Uso general         |DSv2, Dv2, DS, D, Av2, A0-7| Uso equilibrado de CPU y memoria. Ideal para desarrollo/pruebas, así como para soluciones de datos y aplicaciones de tamaño pequeño a mediano.  |
| Proceso optimizado      | Fs, F             | Uso elevado de la CPU respecto a la memoria. Adecuado para aplicaciones, dispositivos de red y procesos por lotes de tráfico medio.        |
| Memoria optimizada       | GS, G, DSv2, DS, Dv2 y D   | Uso elevado de memoria respecto al núcleo. Excelente para bases de datos relacionales, memorias caché de capacidad de media a grande y análisis en memoria.                 |
| Almacenamiento optimizado       | LS                | Alto rendimiento de disco y E/S. Perfecto para bases de datos SQL y NoSQL y macrodatos.                                                         |
| GPU           | NV, NC            | Máquinas virtuales especializadas específicas para actividades intensas de representación de gráficos y edición de vídeo.       |
| Alto rendimiento | H, A8-11          | Nuestras máquinas virtuales con CPU más eficaces e interfaces de red de alto rendimiento (RDMA) opcionales. 


### <a name="find-available-vm-sizes"></a>Búsqueda de los tamaños de máquina virtual disponibles

Para ver una lista de tamaños de máquina virtual disponibles en una región determinada, ejecute el comando [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize).

```powershell
Get-AzureRmVMSize -Location westus
```

## <a name="resize-a-vm"></a>Cambiar el tamaño de una máquina virtual

Una vez implementada una máquina virtual, se puede cambiar su tamaño para aumentar o disminuir la asignación de recursos.

Antes de cambiar el tamaño de una máquina virtual, compruebe si el tamaño deseado está disponible en el clúster actual de máquinas virtuales. El comando [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) devuelve una lista de tamaños. 

```powershell
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

Si el tamaño deseado está disponible, puede cambiarlo con la máquina virtual encendida, aunque se reiniciará durante la operación.

```powershell
$vm = Get-AzureRmVM -ResourceGroupName miGrupoRecursoVM  -VMName miMV 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName miGrupoRecursoVM 
```

Si el tamaño deseado no está en el clúster actual, se debe desasignar la máquina virtual para que se pueda llevar a cabo la operación de cambio de tamaño. Tenga en cuenta que cuando la máquina virtual se enciende de nuevo, se quitan todos los datos del disco temporal y la dirección IP pública cambia a menos que se use una dirección IP estática. 

```powershell
Stop-AzureRmVM -ResourceGroupName miGrupoRecursoVM -Name "miMV" -Force
$vm = Get-AzureRmVM -ResourceGroupName miGrupoRecursoVM  -VMName miMV
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName miGrupoRecursoVM 
Start-AzureRmVM -ResourceGroupName miGrupoRecursoVM  -Name $vm.name
```

## <a name="vm-power-states"></a>Estados de energía de la máquina virtual

Una máquina virtual de Azure puede tener uno de muchos estados de energía. Este estado representa el estado actual de la máquina virtual desde el punto de vista del hipervisor. 

### <a name="power-states"></a>Estados de energía

| Estado de energía | Descripción
|----|----|
| Iniciando | Indica que se está iniciando la máquina virtual. |
| Ejecución | Indica que la máquina virtual se está ejecutando. |
| Deteniéndose | Indica que se está deteniendo la máquina virtual. | 
| Stopped | Indica que la máquina virtual se ha detenido. Tenga en cuenta que las máquinas virtuales en estado detenido siguen acumulando cargos de proceso.  |
| Desasignando | Indica que se está desasignando la máquina virtual. |
| Desasignado | Indica que la máquina virtual se quitó completamente del hipervisor pero sigue estando disponible en el plano de control. Las máquinas virtuales en el estado Desasignado no incurren en gastos de proceso. |
| - | Indica que se desconoce el estado de la máquina virtual. |

### <a name="find-power-state"></a>Búsqueda del estado de energía

Para recuperar el estado concreto de una máquina virtual, use el comando [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm). Asegúrese de especificar un nombre válido para la máquina virtual y el grupo de recursos. 

```powershell
Get-AzureRmVM `
    -ResourceGroupName miGrupoRecursoVM `
    -Name miMV `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Salida:

```powershell
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Tareas de administración

Durante el ciclo de vida de una máquina virtual, puede ejecutar tareas de administración como iniciar, detener o eliminar una máquina virtual. Además, puede crear scripts para automatizar las tareas repetitivas o complejas. Con Azure PowerShell, se pueden ejecutar muchas tareas comunes de administración desde la línea de comandos o en scripts.

### <a name="stop-virtual-machine"></a>Detención de la máquina virtual

Detenga y desasigne una máquina virtual con [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):

```powershell
Stop-AzureRmVM -ResourceGroupName miGrupoRecursoVM -Name "miMV" -Force
```

Si desea mantener la máquina virtual en un estado aprovisionado, use el parámetro -StayProvisioned.

### <a name="start-virtual-machine"></a>Inicio de la máquina virtual

```powershell
Start-AzureRmVM -ResourceGroupName miGrupoRecursoVM -Name miMV
```

### <a name="delete-resource-group"></a>Eliminación de un grupo de recursos

Al eliminar un grupo de recursos se eliminan también todos los recursos contenidos en el mismo.

```powershell
Remove-AzureRmResourceGroup -Name miGrupoRecursoVM -Force
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido conceptos básicos sobre la creación y administración de máquinas virtuales. Prosiga con el siguiente tutorial para aprender sobre los discos de máquina virtual.  

[Creación y administración de discos de máquinas virtuales](./tutorial-manage-data-disk.md)

