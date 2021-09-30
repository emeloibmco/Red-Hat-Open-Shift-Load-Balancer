# Red Hat OpenShift - Load Balancer <img width="26" src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/logo_oc.png">☁

<br />

## Índice  📰
1. [Pre-Requisitos](#Pre-Requisitos-pencil)
2. [Acceder al clúster](#Acceder-al-clúster)
3. [Crear proyecto](#Crear-proyecto)
4. [Desplegar aplicación Angular Web List](#Desplegar-aplicación-Angular-Web-List)
5. [Clonar repositorio](#Clonar-repositorio)
6. [Configurar ALB for VPC](#Configurar-ALB-for-VPC-cloud)
7. [Prueba de funcionamiento de ALB for VPC](#CPrueba-de-funcionamiento-de-ALB-for-VPC-wrench)
8. [Configurar NLB for VPC](#Configurar-NLB-for-VPC-closed_lock_with_key)
9. [Prueba de funcionamiento de NLB for VPC](#Prueba-de-funcionamiento-de-NLB-for-VPC-computer)
10. [Referencias](#Referencias-mag)
11. [Autores](#Autores-black_nib)
<br />

## Pre Requisitos :pencil:
* Cuenta en <a href="https://cloud.ibm.com/"> IBM Cloud</a> <img width="25" src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/ibm-cloud-logo.png">.
* Contar con una VPC. Para mayor información puede consultar como<a href="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH#Crear-VPC-cloud"> crear VPC</a>. 
* Contar con una subred en VPC. Para mayor información puede consultar como<a href="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH#Crear-subred-wrench"> crear subred</a>. 
* Contar con una VSI Ubuntu en VPC. Para mayor información puede consultar como<a href="https://github.com/emeloibmco/VPC-Despliegue-VSI-Acceso-SSH#Desplegar-VSI-en-VPC-computer"> desplegar VSI en VPC</a>.
* Tener un clúster de OpenShift en VPC.
<br />

## Acceder al clúster
Para acceder al clúster de OpenShift, complete los siguientes pasos:
<br />

1. Dentro de su cuenta de *IBM Cloud* acceda al ```IBM Cloud Shell``` dando click en la pestaña <a href="https://cloud.ibm.com/shell"> <img width="25" src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/Shell_IBM.png"></a>, que se ubica en la parte superior derecha del portal. 
<br />

<p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/IBMCloudShell.png"></p>

<br />

2. Ingrese a la consola web de OpenShift presionando el botón ```OpenShift web console```. 
<br />

<p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/AccederConsolaOC.PNG"></p>

<br />


3. Posteriormente de click sobre su correo (parte superior derecha) y luego en la opción ```Copy Login Command```. Una vez cargue la nueva ventana, de click en la opción ```Display Token```. Copie el comando que sale en la opción ```Log in with this token``` y colóquelo en el IBM Cloud Shell para iniciar sesión y acceder a su clúster de OpenShift.
<br />

<p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/TokenFinal.gif"></p>

<br />

<p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/TokenAccesoShell.PNG"></p>

<br />

## Crear proyecto
<br />

## Desplegar aplicación Angular Web List
<br />

## Clonar repositorio
<br />

## Configurar ALB for VPC :cloud:
<br />

## Prueba de funcionamiento de ALB for VPC :wrench:
<br />

## Configurar NLB for VPC :closed_lock_with_key:
<br />

## Prueba de funcionamiento de NLB for VPC :computer:
<br />

## Referencias :mag:
* <a href="https://cloud.ibm.com/docs/openshift?topic=openshift-vpc-lbaas">VPC: Exposing apps with load balancers for VPC</a>.
<br />

## Autores :black_nib:
Equipo IBM Cloud Tech Sales Colombia.
<br />
