# Laboratorio 01: Mi Primera Red (Bases de Networking para Ciberseguridad)

Este es mi primer laboratorio práctico para construir las bases sólidas de redes indispensables en el área de la Ciberseguridad. Diseñé una topología LAN básica en Cisco Packet Tracer con dos hosts y un Switch 2960.

Asigné direccionamiento IPv4 estático (`192.168.1.0/24`) y validé la conectividad local mediante ICMP (`ping`). Este ejercicio es el pilar fundamental para entender el tráfico de red antes de avanzar hacia el análisis de paquetes (Wireshark), escaneo de puertos (Nmap) y reglas de Firewall.

### 📊 Evidencia de Conectividad (Prueba de Ping)
![Prueba de Ping Exitosa](Test.png)

---

## 🛡️ Laboratorio 02: Hub vs. Switch (Análisis de Sniffing en Capa 2)

### 🎯 Objetivo:
Demostrar de forma práctica por qué los Hubs representan un riesgo crítico de seguridad (*Eavesdropping / Sniffing*) debido a su naturaleza de medio compartido, y cómo un Switch mitiga esto mediante la microsegmentación y el uso de su tabla CAM.

### 🔬 Resultados del Experimento:
1. **Lado del Hub (Inseguro):** Al activar el modo simulación y enviar un paquete ICMP (`ping`) desde la PC Administradora al Servidor, el Hub duplicó y transmitió el paquete de forma masiva a todos sus puertos, haciendo que la PC Atacante recibiera una copia exacta del tráfico.
2. **Lado del Switch (Seguro):** El Switch procesó las tramas leyendo las direcciones MAC de origen y destino. Tras el proceso inicial de ARP, el tráfico fluyó de manera exclusiva entre el emisor y el receptor.

### 🔍 Evidencia Técnica (Tabla MAC Dinámica del Switch)
Utilizando el comando `show mac-address-table` en la interfaz de línea de comandos (CLI) del Switch, se verificó cómo este mapea dinámicamente las direcciones MAC físicas a sus puertos correspondientes para tomar decisiones de reenvío unicast, aislando al atacante del canal de comunicación:

![Tabla CAM del Switch](tabla_cam.png)

---

## 🌐 Laboratorio 3: Infraestructura de Red con Servicios DNS y Web (DMZ)

### 📄 Descripción
En este tercer laboratorio se diseñó e implementó una infraestructura de red segmentada que comunica una red local interna (LAN) con una zona de servicios (simulación de DMZ/Internet). El objetivo principal fue configurar el enrutamiento inter-redes y habilitar la resolución de nombres de dominio (DNS) para permitir que un usuario interno acceda a un servidor web mediante una URL amigable en lugar de una dirección IP.

### 🗺️ Tabla de Direccionamiento IP

| Dispositivo | Interfaz | Dirección IP | Máscara de Subred | Gateway por Defecto | Servidor DNS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **PC_Johnny** | FastEthernet0 | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 | 10.1.1.50 |
| **Router_Gateway** | GigabitEthernet0/0 | 192.168.1.1 | 255.255.255.0 | N/A | N/A |
| **Router_Gateway** | GigabitEthernet0/1 | 10.1.1.1 | 255.255.255.0 | N/A | N/A |
| **Servidor_DNS** | FastEthernet0 | 10.1.1.50 | 255.255.255.0 | 10.1.1.1 | N/A |
| **Web_NetworkChuck**| FastEthernet0 | 10.1.1.20 | 255.255.255.0 | 10.1.1.1 | 0.0.0.0 |

### 🛠️ Configuraciones Clave Implementadas
1. **Enrutamiento en Router_Gateway:** Activación y asignación de IPs en interfaces GigabitEthernet para interconectar el segmento `192.168.1.0/24` con el segmento `10.1.1.0/24`.
2. **Servicio DNS:** Configuración de un registro de tipo **A Record** en el `Servidor_DNS` que asocia el dominio `networkchuck.coffee` con la dirección IP del servidor web.
3. **Servicio HTTP/HTTPS:** Activación de los servicios web en el servidor destino utilizando el archivo index por defecto de Cisco Packet Tracer.

### 📸 Evidencias de Funcionamiento

#### 1. Topología de Red Completa
Visualización general del direccionamiento y la interconexión física/lógica de los equipos.
![Topología Completa](lab3_01_topologia_completa.png)

#### 2. Configuración IP de los Host e Interfaces
Validación de los parámetros IP y asignación del servidor DNS en el cliente de la red local.
![Configuración PC Johnny](lab3_02_config_pc_johnny.png)
![Configuración Servidor Web](lab3_03_config_servidor_web.png)

#### 3. Prueba de Resolución DNS y Carga Web Exitosa
Demostración del acceso exitoso desde el navegador web de la PC de Johnny hacia el sitio web resolviendo el nombre de dominio en tiempo real.
![Prueba de Navegación](lab3_04_navegacion_exitosa.png)
