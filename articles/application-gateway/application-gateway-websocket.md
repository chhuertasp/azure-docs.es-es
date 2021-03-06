---
title: Compatibilidad de Application Gateway con WebSocket | Microsoft Docs
description: "En esta página se proporciona información general sobre la compatibilidad de Application Gateway con WebSocket."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 8968dac1-e9bc-4fa1-8415-96decacab83f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/16/2016
ms.author: amsriva
translationtype: Human Translation
ms.sourcegitcommit: 119275f335344858cd20b6a17ef87e3ef32b6e12
ms.openlocfilehash: 5af1b1bacce2fe189a7b557527520795ed622b54
ms.lasthandoff: 03/01/2017


---
# <a name="overview-of-websocket-support-in-application-gateway"></a>Introducción a la compatibilidad de WebSocket en Application Gateway

Application Gateway proporciona una compatibilidad nativa con WebSocket en todas las puertas de enlace, con independencia de su tamaño. No hay ninguna opción de configuración que permita al usuario habilitar o deshabilitar la compatibilidad con WebSocket. Puede seguir usando una clase HTTPListener estándar en el puerto 80 o 443 para recibir tráfico de WebSocket. Después, el tráfico de WebSocket se dirige al servidor back-end con este protocolo habilitado utilizando el grupo back-end adecuado según lo especificado en las reglas de Application Gateway. El protocolo WebSocket, estandarizado como [RFC6455](https://tools.ietf.org/html/rfc6455) , permite una comunicación dúplex completa entre el servidor y el cliente a través de una conexión TCP de larga duración. Gracias a esta característica, la comunicación entre el servidor web y el cliente, que puede ser bidireccional sin necesidad de realizar sondeos como en las implementaciones basadas en HTTP, es más interactiva.  WebSocket tiene una sobrecarga reducida, a diferencia de HTTP, y puede reutilizar la misma conexión TCP para varias solicitudes y respuestas, con lo que se utilizan los recursos de una manera más eficaz. Los protocolos WebSocket están diseñados para utilizarse a través de los puertos HTTP tradicionales 80 y 443.

El servidor back-end debe responder a los sondeos de la puerta de enlace de aplicaciones, que se describen en la sección de [introducción al sondeo de estado](application-gateway-probe-overview.md) . Los sondeos de estado de la puerta de enlace de aplicaciones solo son HTTP o HTTPS; es decir, que todos los servidores back-end que deben responder a los sondeos HTTP de la puerta de enlace de aplicaciones para redirigir el tráfico de WebSocket al servidor.

## <a name="listener-configuration-element"></a>Elemento de configuración de agente de escucha

La clase HTTPListener existente puede utilizarse para admitir WebSocket. A continuación, se muestra un fragmento de código del elemento HttpListeners del archivo de plantilla de ejemplo. Necesitaría los agentes de escucha de HTTP y HTTPS para admitir WebSocket y proteger el tráfico procedente de este protocolo. De forma similar, puede usar el [portal](application-gateway-create-gateway-portal.md) o [PowerShell](application-gateway-create-gateway-arm.md) para crear una puerta de enlace de aplicaciones con agentes de escucha en el puerto 80 o 443, con el fin de permitir el tráfico de WebSocket.

```json
"httpListeners": [
        {
            "name": "appGatewayHttpsListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                },
                "Protocol": "Https",
                "SslCertificate": {
                    "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                },
            }
        },
        {
            "name": "appGatewayHttpListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                },
                "Protocol": "Http",
            }
        }
    ],
```

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>Configuración de reglas de enrutamiento, BackendHttpSetting y BackendAddressPool

Debe utilizarse BackendAddressPool para definir un grupo back-end con servidores que tengan WebSocket habilitado. BackendHttpSetting debe definirse solo con el puerto back-end 80 o 443. Las propiedades de afinidad basada en cookies y requestTimeouts no son pertinentes para el tráfico de WebSocket. No se requiere ningún cambio en la regla de enrutamiento. La regla de enrutamiento básica debe seguir eligiéndose para vincular el agente de escucha adecuado al grupo de direcciones back-end correspondiente. 

```json
"requestRoutingRules": [{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}, {
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }

    }
}]
```

## <a name="websocket-enabled-backend"></a>Back-end con WebSocket habilitado

El back-end debe tener un servidor web HTTP o HTTPS que se esté ejecutando en el puerto configurado (normalmente, el 80 o 443) para que WebSocket funcione. Este requisito se debe a que el protocolo WebSocket necesita que el protocolo de enlace inicial sea HTTP con el protocolo Actualizar a WebSocket como campo de encabezado.

```
    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
```

Otro de los motivos es que el sondeo de estado back-end de la puerta de enlace de aplicaciones solo admite los protocolos HTTP y HTTPS. Si el servidor back-end no responde a sondeos HTTP ni HTTPS, se eliminaría del grupo de back-end y ninguna solicitud, entre ellas las de WebSocket, llegarían a este back-end.

## <a name="next-steps"></a>Pasos siguientes

Cuando haya terminado de leer la información sobre compatibilidad con WebSocket, vaya al artículo sobre [cómo crear una puerta de enlace de aplicaciones](application-gateway-create-gateway.md) para empezar a trabajar con una aplicación web con WebSocket habilitado.


