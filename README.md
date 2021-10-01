# Red Hat OpenShift - Load Balancer <img width="26" src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/logo_oc.png">☁

<br />

## Índice  📰
1. [Pre-Requisitos](#Pre-Requisitos-pencil)
2. [Acceder al clúster](#Acceder-al-clúster)
3. [Crear proyecto](#Crear-proyecto)
4. [Desplegar aplicación Angular Web List](#Desplegar-aplicación-Angular-Web-List)
5. [Instalar plugin y clonar repositorio](#Instalar-plugin-y-clonar-repositorio)
6. [Configurar ALB for VPC](#Configurar-ALB-for-VPC-cloud)
7. [Prueba de funcionamiento de ALB for VPC](#Prueba-de-funcionamiento-de-ALB-for-VPC-wrench)
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

2. Asegúrese de estar en la región donde tiene desplegados sus recursos. Si necesita cambiar de región utilice el comando:

   ```
   ibmcloud target -r <region>
   ```   
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/Region.PNG"></p>

   <br />

3. Ingrese a la consola web de OpenShift presionando el botón ```OpenShift web console```. 
<br />

<p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/AccederConsolaOC.PNG"></p>

<br />


4. Posteriormente de click sobre su correo (parte superior derecha) y luego en la opción ```Copy Login Command```. Una vez cargue la nueva ventana, de click en la opción ```Display Token```. Copie el comando que sale en la opción ```Log in with this token``` y colóquelo en el IBM Cloud Shell para iniciar sesión y acceder a su clúster de OpenShift.
<br />

<p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/TokenFinal.gif"></p>

<br />

<p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/TokenAccesoShell.PNG"></p>

<br />

## Crear proyecto
Debe crear un proyecto en el cúal desplegará la aplicación y realizará la configuración para el Load Balancer. Complete los siguientes pasos:
<br />

1. En la consola de OpenShift cree un nuevo proyecto. Para ello, asegúrese de estar en el rol de ```Developer```, de click en la pestaña ```Project``` y luego ```Create Project```. Allí, asígne un nombre y de click en el botón ```Create```.
<br />

<p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/CrearProyecto.gif"></p>

<br />

2. Acceda al proyecto creado en IBM Cloud Shell. Para ello utilice el comando:

   ```
   oc project <nombre_proyecto>
   ```

   Ejemplo:

   ```
   oc project angular-web-list
   ```
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/oc_proyect.PNG"></p>

   <br />
<br />

## Desplegar aplicación Angular Web List
Para realizar la prueba de funcionmaiento del Load Balancer, se desplegará la aplicación <a href="https://github.com/emeloibmco/AngularWebList"> Angular Web List</a> en el clúster de OpenShift. Los pasos que debe realizar son los siguientes:
<br />

1. En el IBM Cloud Shell, clone el repositorio que contiene los archivos de la aplicación. Utilice el comando:
   
   ```
   git clone https://github.com/emeloibmco/AngularWebList
   ```
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/ClonarRepoListas.PNG"></p>

   <br />

2. Acceda a la carpeta *AngularWebList* con el comando:
   
   ```
   cd AngularWebList
   ```
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/cd_carpeta_listas.PNG"></p>

   <br />

3. Despliegue la aplicación en el clúster con el comando:

   ```
   npx nodeshift --strictSSL=false --dockerImage=nodeshift/ubi8-s2i-web-app --imageTag=10.x --build.env OUTPUT_DIR=dist/angular-web-app --expose
   ```
   <br />
   
   > NOTA: Si tiene alguna inquietud sobre el despliegue de la aplicación puede consultar <a href="https://github.com/emeloibmco/ROKS-Angular-HandsOn-4.4#despliegue-aplicaci%C3%B3n-listas-en-angular-%F0%9F%85%B0%EF%B8%8F"> Despliegue Aplicación Listas en Angular</a>.
   
   <br />

4. Verifique que la aplicación se ha desplegado correctamente dentro del proyecto creado.
   
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/App_ok.gif"></p>

   <br />

## Instalar plugin y clonar repositorio
Antes de realizar la respectiva configuración para los Load Balancer debe instalar un plugin y clonar el presente repositorio, el cual contiene los archivos necesarios para llevar a cabo el procedimiento. Para ello, realice lo siguiente:
<br />

1. Salga de la carpeta *AngularWebList* con ```cd ..```.
   
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/salir_carpeta_listas.PNG"></p>

   <br />

2. Instale el plugin ```infrastructure-service```, el cual se necesitará para la configuración del ALB y NLB. Utilice el siguiente comando:

   ```
   ibmcloud plugin install infrastructure-service
   ```
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/plugin.PNG"></p>

   <br />
   
3. Clone el repositorio con el comando:
   
   ```
   git clone https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer
   ```
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/Clonar_RepoLB.PNG"></p>

   <br />

4. Acceda a la carpeta ```Red-Hat-Open-Shift-Load-Balancer``` con:

   ```
   cd Red-Hat-Open-Shift-Load-Balancer/
   ```
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/Cluster%20images/cd_carpeta_LB.PNG"></p>

   <br />


## Configurar ALB for VPC :cloud:
Al configurar un Application Load Balancer (ALB) puede exponer su aplicación a la red pública o privada. Dentro de la carpeta ```Archivos ALB```de este repositorio puede encontrar 2 archivos .yaml que contiene las configuraciones necesarias para cada caso. Siga los pasos que se presentan a continuación, teniendo en cuenta el tipo de solicitud (pública o privada) que su aplicación recibirá:

<br />

1. Acceda a la carpeta ```Archivos ALB``` con el comando:
   
   ```
   cd Archivos\ ALB
   ```
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/carpeta_alb.PNG"></p>

   <br />

2. Dentro de esta carpeta puede encontrar 2 archivos:
   <br />
   
   * **ALB público** ➡ ```myloadbalancer.yaml```. Este archivo contiene lo siguiente:
   <br />
   
   ```cmd
   apiVersion: v1
   kind: Service
   metadata:
     name: myloadbalancer
     annotations:
       service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: "public"
       service.kubernetes.io/ibm-load-balancer-cloud-provider-vpc-subnets: "<subnet_ID>"
       service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"
   spec:
    type: LoadBalancer
    selector:
       <selector_key>: <selector_value>
    ports:
      - name: http
        protocol: TCP
        port: <port>
        targetPort: <port>
   ```
   <br />
   
   * **ALB privado** ➡ ```myloadbalancerprivate.yaml```. Este archivo contiene lo siguiente:
   <br />
   
   ```cmd
   apiVersion: v1
   kind: Service
   metadata:
     name: myloadbalancerprivate
     annotations:
       service.kubernetes.io/ibm-load-balancer-cloud-provider-ip-type: "private"
       service.kubernetes.io/ibm-load-balancer-cloud-provider-vpc-subnets: "<subnet_ID>"
       service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"
   spec:
    type: LoadBalancer
    selector:
       <selector_key>: <selector_value>
    ports:
      - name: http
        protocol: TCP
        port: <port>
        targetPort: <port>
   ```
   
   Deberá realizar una serie de modificaciones en cada archivo con los datos de la subred y aplicación. Para ello con el comando nano abra el archivo y realice los respectivos cambios:
   
   <br />
   
   * **ALB público**
   <br />
    
   ```
   nano myloadbalancer.yaml
   ```
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/nano_publico.PNG"></p>

   <br />
   
   * **ALB privado**
   <br />
   
   ```
   nano myloadbalancerprivate.yaml
   ```
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/nano_privado.PNG"></p>

   <br />
   
   
   Los cambios que debe realizar son:
   * En la línea ```service.kubernetes.io/ibm-load-balancer-cloud-provider-vpc-subnets: "<subnet_ID>"``` coloque entre comillas el ID de su subred. Ejemplo ```service.kubernetes.io/ibm-load-balancer-cloud-provider-vpc-subnets: "0767-5fe01a4c-03bf-419c-a09e-86ae1bb2af1d"```. Para encontrar el ID de la subred, en la consola de IBM dentro del clúster que está trabajando, seleccione la pestaña ```Worker nodes```, de click sobre uno de los nodos trabajadores y visualice el ID en la sección ```Subnet```.
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/Id_Subnet.PNG"></p>

   <br />
   
   * En la línea ```service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"``` coloque entre comillas la zona en la cual está desplegado su clúster. Ejemplo ```service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "us-east-2"```.
   <br />
   
   * En la sección ```selector``` reemplace la línea ```<selector_key>: <selector_value>``` con la clave y valor del selector, que puede localizar en los labels del pod de la aplicación. Ejemplo ```deployment: listas-1```. En la siguiente imagen se muestra donde se ubica el selector.
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/Labels.gif"></p>

   <br />
   
   * En la sección ```ports``` reemplace la variable ```<port>``` con el valor del puerto que se ubica también en el .yaml del pod de la aplicación. En la siguiente imagen puede observar donde se encuentra el puerto.
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/Puerto.gif"></p>

   <br />
   
   Cuando termine de aplicar los cambios presione ```Ctrl s``` para guardar y ```Ctrl x``` para salir del editor.
   
   <br />

3. Cree el servicio del Load Balancer con el comando:
   
   * **ALB público**
   <br />
   
   ```
   oc apply -f myloadbalancer.yaml -n <namespace>
   ```
   
   Ejemplo:
   
   ```
   oc apply -f myloadbalancer.yaml -n angular-web-list
   ```
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/1_oc_apply_publico.PNG"></p>

   <br />
   
   * **ALB privado**
   <br />
   
   ```
   oc apply -f myloadbalancerprivate.yaml -n <namespace>
   ```
   
   Ejemplo:
   
   ```
   oc apply -f myloadbalancerprivate.yaml -n angular-web-list
   ```
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/1_oc_apply_privado.PNG"></p>

   <br />

4. Verifique el servicio ha sido creado. Para ello, cambia el rol a ```Administrator``` y seleccione la opción ```Networking``` ➡ ```services```. Identifique los servicios ```myloadbalancer``` y ```myloadbalancerprivate``` que han sido creados.
   
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/alb-publico-privado-ok.gif"></p>

   <br />
   
   

## Prueba de funcionamiento de ALB for VPC :wrench:
Para proar el funcionamiento del Load Balancer, siga los pasos que se indican para cada caso:
<br />

* [ALB para solicitudes públicas](#ALB-para-solicitudes-públicas-unlock)
* [ALB para solicitudes privadas](#ALB-para-solicitudes-privadas-lock)
<br />

### ALB para solicitudes públicas :unlock:
1. Obtenga el External-IP o Load Balancer Ingress. Para ello puede utilizar cualquiera de los siguientes 2 comandos:
   
   * Obtenga el servicio:
   <br />
   
   ```
   oc get svc myloadbalancer -n n<amespace>
   ```
   
   Ejemplo:
   
   ```
   oc get svc myloadbalancer -n angular-web-list
   ``` 
   <br />
   
   * Decriba el servicio:
   <br />
   
   ```    
   oc describe svc myloadbalancer -n <namespace>
   ```
   
   Ejemplo:
   
   ```
   oc describe svc myloadbalancer -n angular-web-list
   ``` 
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/url_alb_publico.PNG"></p>

   <br />
   
2. Para probar el funcionamiento del Load Balancer puede realizar dos procedimiento:
   <br />
   
   * Use el comando ```curl```, el External-IP y el puerto del Load Balancer y observe la respuesta de la aplicación mediante IBM Coud Shell. Para ello coloque:
   
     ```
     curl <external-ip>:port
     ```

     Ejemplo:

     ```
     curl 123f8e58-us-east.lb.appdomain.cloud:8080
     ``` 
     <br />

     <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/curl_alb_publico.PNG"></p>

     <br />
 
    * En el navegador visualice la aplicación mediante el External-IP y el puerto del Load Balancer. Para ello coloque:
   
     ```
     <external-ip>:port
     ```

     Ejemplo:

     ```
     123f8e58-us-east.lb.appdomain.cloud:8080
     ``` 
     <br />

     <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/browser_alb_publico.PNG"></p>

     <br />
    

### ALB para solicitudes privadas :lock:
1. Obtenga el External-IP o Load Balancer Ingress. Para ello puede utilizar cualquiera de los siguientes 2 comandos:
   
   * Obtenga el servicio:
   <br />
   
   ```
   oc get svc myloadbalancerprivate -n n<amespace>
   ```
   
   Ejemplo:
   
   ```
   oc get svc myloadbalancerprivate -n angular-web-list
   ``` 
   <br />
   
   * Decriba el servicio:
   <br />
   
   ```    
   oc describe svc myloadbalancerprivate -n <namespace>
   ```
   
   Ejemplo:
   
   ```
   oc describe svc myloadbalancerprivate -n angular-web-list
   ``` 
   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/url_alb_privado.PNG"></p>

   <br />
   
2. Haga ping al External-IP de su aplicación y obtenga la IP privada. Para este caso, teniendo en cuenta que la solicitudes son en la red privada, cuando haga ping no obtendrá respuesta, pero podrá identificar la IP.

   <br />

   <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/ping_privada.PNG"></p>

   <br />

3. Tal y como se indicó en los requisitos, necesita una VSI Ubuntu en VPC. Desde esta VSI se realizará la prueba de funcionamiento del Load Balancer, teniendo en cuenta que es red privada y tanto la VSI como el Load Balancer se encuentran en la misma subred. Acceda a la VSI con su respectiva IP y contraseña.
     
     <br />

     <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/vsi_ssh.PNG"></p>

     <br />

4. Se realizarán 2 pruebas que permiten ver el funcionamiento del Load Balancer en la red privada. Complete los siguientes pasos:

   * Dentro de la VSI use el comando ```curl```, la IP privada obtenida y el puerto del Load Balancer. Luego observe la respuesta de la aplicación. Para ello coloque:
   
     ```
     curl <ip-privada>:port
     ```

     Ejemplo:

     ```
     curl 10.241.64.14:8080
     ``` 
     <br />
     
     Si la conexión es correcta podrá observar la respuesta de la aplicación.
     <br />

     <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/curl_alb_privado.PNG"></p>

     <br />
     
   * Use el comando ```telnet```, la IP privada obtenida y el puerto del Load Balancer. Luego observe la respuesta de la aplicación. Para ello coloque:

     ```
     telnet <ip-privada> port
     ```

     Ejemplo:

     ```
     telnet 10.241.64.14 8080
     ``` 
     
     Si la conexión es correcta podrá observar que se ha conectado a la IP.
     <br />

     <p align="center"><img src="https://github.com/emeloibmco/Red-Hat-Open-Shift-Load-Balancer/blob/main/ALB%20images/telnet_alb_publico.PNG"></p>

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
