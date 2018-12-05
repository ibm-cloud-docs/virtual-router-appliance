---

copyright:
  years: 2017
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Creación de un perímetro seguro
Un aspecto fundamental del aislamiento de la red es el establecimiento de un perímetro seguro.  Un perímetro seguro controla el tráfico entre internet público y los activos del cliente alojados en IBM Cloud.  El perímetro seguro también permitirá la conectividad directa con la empresa del cliente mediante el uso de túneles de red privada virtual (VPN) y de IBM Cloud Direct Link.

Un perímetro seguro utiliza un segmento de perímetro seguro (SPS) para segregar redes entre activos dentro del perímetro seguro. Esta segregación tiene varias ventajas, incluido el control de accesos y el aislamiento de tráfico de servicios entre los segmentos. Un segmento de perímetro seguro (SPS) comprende dos redes de área local virtual (VLAN), una VLAN frontal y una VLAN de fondo, y una VRA conectada a las VLAN para gestionar el tráfico dentro y fuera de los SPS. Un perímetro seguro puede incluir varios segmentos de perímetro seguros (por ejemplo, para fines de alta disponibilidad).

## Configuración del perímetro seguro

Estos son los pasos a seguir para configurar un perímetro seguro.  Para ver una descripción detallada de estos pasos, consulte [Configuración de un perímetro seguro en IBM Cloud](https://developer.ibm.com/dwblog/2018/ibm-cloud-vyatta-set-up-secure-perimeter).

1. Configure los permisos en IBM Cloud y la infraestructura necesaria para acceder a los servicios de nube y a los componentes de la infraestructura que intervienen en el perímetro seguro.
2. Cree un límite externo en la internet pública mediante una VLAN pública y un cortafuegos. Este límite externo se utilizará para aislar el SPS.
3. Coloque el SPS dentro de dicho límite
4. Configure una pasarela para establecer un puente entre la VLAN pública y SPS
5. Crear un dispositivo de direccionador virtual (VRA) de configuración
6. Cree y configure uno o varios segmentos de perímetro seguros (SPS)
7. Cree una VLAN frontal
8. Cree una VLAN de fondo
9. Cree un par Vyatta
10. Configure Vyatta
11. Asocie las VLANS con una pasarela
12. Configure la interfaz de pasarela para la VLAN pública
13. Configure la pasarela en el nodo maestro Vyatta
14. Configure la pasarela en Vyatta de reserva
15. Configure la interfaz de pasarela para la VLAN privada
16. Configure la pasarela en el nodo maestro Vyatta
17. Configure la pasarela en Vyatta de reserva
18. Habilite el ajuste de red
19. Configure reglas de cortafuegos
20. Configure el clúster Kubernetes
21. Configure las IP proporcionadas por el usuario (opcional).
22. Despliegue el clúster Kubernetes.

## Soluciones dedicadas en IBM Cloud
Un perímetro seguro, junto con el aislamiento de cálculo y el cifrado de datos, contribuye a una solución completa dedicada en IBM Cloud público.  Consulte [Implementación de un patrón de soluciones dedicadas](https://developer.ibm.com/dwblog/2018/ibm-cloud-dedicated-cloud-solution-patterns/) para ver una descripción de los patrones de soluciones dedicados de la nube.
