# IT Help Desk - Home Lab (Active Directory Ecosistema)

---


## Módulo 1: Diseño de Infraestructura Virtual y Red Interna (VirtualBox y DNS)

- **Hipervisor:** Oracle VM VirtualBox
- **Tipo de Red:** Red Interna (`MiRedLab`)
- **Servidor (DC1):** 
  - **OS:** Windows Server 2022
  - **Hostname:** `WIN_VNAG53S3C27`
  - **IP Estática:** `192.168.10.2 /24`
  - **Roles:** Active Directory Domain Services (AD DS) y DNS Server
- **Cliente (PC-01):** 
  - **OS:** Windows 11 Pro
  - **IP Estática:** `192.168.10.15 /24`
  - **DNS Preferido:** `192.168.10.2`
- **Dominio:** `erick.local`
- **Herramientas de Diagnóstico y Soporte:** PowerShell, CMD (`ping`, `nslookup`, `ipconfig`), Panel de Control (`sysdm.cpl`, `ncpa.cpl`, `wf.msc`)

---


## Fases del Proyecto

### 1. Configuración de Red e Aislamiento
- Creación de un entorno aislado de red (*Internal Network*) en VirtualBox para evitar interferencias con la red física local.
- Identificación de direcciones autoconfiguradas APIPA (`169.254.x.x`) y transición a un esquema de direccionamiento IPv4 estático para garantizar la persistencia de comunicación entre nodos.

### 2. Despliegue de Active Directory & DNS
- Promoción del servidor Windows Server 2022 a Controlador de Dominio (DC) bajo el bosque `erick.local`.
- Configuración del servicio DNS para la resolución interna de nombres de host y dominio.

### 3. Unión del Cliente Windows 11 al Dominio
- Configuración de los parámetros de red y asignación del servidor DNS local (`192.168.10.2`) en la estación de trabajo cliente.
- Autenticación con credenciales del dominio (`erick.local\Administrator`) e integración exitosa del equipo al dominio corporativo.

---

## Troubleshooting & Resolución de Incidencias (N1)

Durante la implementación se presentaron y resolvieron diversos retos técnicos comunes en entornos de infraestructura:

| Incidencia | Causa Raíz | Solución Aplicada |
| :--- | :--- | :--- |
| **Cliente recibe IP APIPA (`169.254.x.x`) / Pérdida del 100% de paquetes** | Adaptador en Red Interna sin DHCP activo / Bloqueo de tráfico ICMP por Firewall de Windows. | 1. Asignación manual de IPs estáticas en ambos extremos.<br>2. Ajuste o desactivación temporal de perfiles en el Firewall de Windows (`wf.msc`). |
| **Error: *"Domain cannot be contacted"* / *"DNS server not responding"*** | El cliente consultaba un DNS incorrecto o la interfaz del servidor DNS no estaba escuchando en la IP estática correcta. | 1. Configuración explícita de `192.168.10.2` como DNS preferido en Windows 11.<br>2. Verificación de servicios (`DNS`, `NTDS`) y ajuste en el **Administrador DNS** para escuchar únicamente en la IP asignada. |
| **Fallo de resolución de nombre de dominio** | Confusión entre el Hostname físico del servidor y el nombre FQDN del Dominio (`erick.local`). | Validación mediante PowerShell (`Get-ADDomain`) y pruebas con `nslookup` y `ipconfig /flushdns` para confirmar la correcta resolución. |

---

## Evidencias del Laboratorio

- **Configuración de Red y Conectividad:** Comprobación mediante `ping` entre nodos.
- **Administrador DNS / AD DS:** Verificación del dominio activo `erick.local`.
- **Unión Exitosa:** Ventana de propiedades del sistema en Windows 11 mostrando el equipo integrado al dominio.

### Conectividad y Resolución DNS desde el Cliente

 <img alt="Prueba de Ping y Nslookup desde Windows 11" width="839" height="656" src="https://github.com/user-attachments/assets/40d3ebd9-a412-4020-bf54-333877db27a7" />

 
<img width="738" height="227" src="https://github.com/user-attachments/assets/6fe4ae52-cdf9-4968-8c0a-f134da502df4" />

### Estado del Controlador de Dominio (DC1)

<img width="884" height="547" alt="Active Directory Domain Services Activo" src="https://github.com/user-attachments/assets/d46ea5f6-0b9a-45b4-a017-a14387d6bfd8" />

### Confirmación de Integración al Dominio

<img width="729" height="569" alt="Windows 11 unido exitosamente a erick.local" src="https://github.com/user-attachments/assets/eb953f72-fd73-43e1-9425-c1a03efe72f3" />

---

## Aprendizajes Clave

- **Fundamentos de Redes:** Asignación de IPs estáticas, subredes, comprensión de direcciones APIPA e importancia del protocolo ICMP.
- **Servicios de Red:** Comprensión de la dependencia crítica entre Active Directory y el servicio DNS para la autenticación de equipos.
- **Administración Básica:** Manejo de herramientas administrativas de Windows Server y comandos esenciales de diagnóstico (`ping`, `ipconfig`, `nslookup`).
- **Resolución de Problemas:** Enfoque metodológico (troubleshooting) para identificar y corregir fallos de conectividad y unión a dominios reales.

---

## Módulo 2: Gestión de Identidades y Usuarios (Active Directory)

### Descripción de la Fase
En esta etapa se implementó una estructura empresarial dentro del directorio activo para administrar identidades, aplicando las mejores prácticas de soporte de TI y control de accesos (N1).

---

### Tareas Realizadas y Casos de Soporte

1. **Diseño de Unidades Organizativas (OUs):**
   - Creación de la OU principal `Empresa_OU` y sub-OUs departamentales (`Sistemas`, `Ventas`, `RRHH`) para una correcta segmentación administrativa.
2. **Creación de Cuentas de Usuario:**
   - Alta de usuarios de prueba (ej. `jperez` en el departamento de Ventas) configurados con la directiva de cambio obligado de contraseña en el primer inicio de sesión.
3. **Simulación de Incidencias Help Desk N1:**
   - **Password Reset:** Resolución de casos por olvido de credenciales mediante el restablecimiento temporal de contraseñas desde la consola `dsa.msc`.
   - **Desbloqueo de Cuentas:** Gestión de cuentas bloqueadas por múltiples intentos fallidos de autenticación en el dominio.
4. **Asignación de Grupos de Seguridad:**
   - Creación de grupos (ej. `GRP_Ventas_RW`) e inclusión de usuarios para facilitar la futura asignación de permisos a recursos de red.

---

## Evidencias del Módulo 2

### Estructura de OUs y Usuarios en Active Directory
<img width="753" height="525" alt="Estructura de Unidades Organizativas" src="https://github.com/user-attachments/assets/53548747-3966-49c2-8741-a3a04e49cf97" />

### Validación de Inicio de Sesión Corporativo en Windows 11
<img width="695" height="638" alt="Inicio de sesión exitoso con usuario de dominio" src="https://github.com/user-attachments/assets/a5b171c8-fa22-4710-a208-36871b61bea2" />

---


## Módulo 3: Diseño y Configuración de Red Multi-Departamental (Cisco Packet Tracer)

### Descripción de la Fase
Diseño y simulación de una topología de red corporativa ruteada para 4 departamentos, enfocada en la segmentación de red, interconexión de dispositivos finales y configuración de equipos Cisco mediante línea de comandos (CLI).

---

### Especificaciones Técnicas

- **Dispositivos en Red:** 1 Router Cisco, 4 Switches, 12 Workstations y 1 Servidor central.
- **Topología Lógica:** Segmentación por 4 departamentos conectados a sus respectivos switches y centralizados mediante el router.
- **Configuración CLI:** Asignación dinámica de direcciones IPv4 mediante DHCP, configuración de interfaces del router y ruteo inter-departamental.
- **Pruebas de Diagnóstico:** Validación de conectividad end-to-end con tráfico ICMP (`ping`).

---

## Evidencias del Laboratorio de Redes

### Topología General de la Red en Cisco Packet Tracer
<img width="1362" height="720" alt="Topología completa de red multi-departamental" src="https://github.com/user-attachments/assets/7897fcbc-4dba-4b7e-9238-e7e127f4282d" />

### Configuración de Interfaces y Ruteo por CLI (Cisco Router)
<img width="1040" height="722" alt="Configuración CLI en Router Cisco" src="https://github.com/user-attachments/assets/89eb991e-3b38-4325-a001-9bbf421ce783" />


### Pruebas de Conectividad End-to-End (Ping Exitoso entre Departamentos)

<img width="1364" height="716" alt="Prueba de Ping exitoso en Packet Tracer" src="https://github.com/user-attachments/assets/1243708f-8675-4f97-8f78-a80aec66eaf0" />




 *Proyecto documentado por [Erick J. Orozco] como parte de un proceso de autoaprendizaje en Infraestructura, Redes y Soporte de TI.*
