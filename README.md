<img width="839" height="656" alt="images:01-network-dns-test" src="https://github.com/user-attachments/assets/0ef7d4c2-7e68-4aea-b5af-00ae712d0f1f" />
<img width="839" height="656" alt="images:01-network-dns-test" src="https://github.com/user-attachments/assets/a5ad0d97-28b7-42d7-b1be-312a75f3d789" />
# IT Help Desk & Home Lab (Active Directory Environment)

## Descripción del Proyecto
Este repositorio documenta la construcción paso a paso de un laboratorio de infraestructura de TI enfocado en soporte de nivel N1 y administración de sistemas. El proyecto simula un entorno empresarial controlado utilizando virtualización para practicar la resolución de problemas reales de redes, despliegue de servicios y gestión de identidades.

---

## Tecnologías y Herramientas Utilizadas
- **Hipervisor:** Oracle VM VirtualBox
- **Tipo de Red:** Red Interna (`MiRedLab`)
- **Servidor (DC1):** 
  - **OS:** Windows Server 2022
  - **Hostname:** `WIN_VNAG53S3C27`
  - **IP Estática:** `192.168.10.2 /24`
  - **Roles:** Active Directory Domain Services (AD DS) & DNS Server
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

*(Nota: Puedes agregar aquí capturas de pantalla de tu entorno guardándolas en una carpeta llamada `images/` en tu repositorio)*

- **Configuración de Red y Conectividad:** Comprobación mediante `ping` entre nodos.
- **Administrador DNS / AD DS:** Verificación del dominio activo `erick.local`.
- **Unión Exitosa:** Ventana de propiedades del sistema en Windows 11 mostrando el equipo integrado al dominio.
<img width="839" height="656" alt="images:01-network-dns-test" src="https://github.com/user-attachments/assets/40d3ebd9-a412-4020-bf54-333877db27a7" />
<img width="738" height="227" alt="images:02-network-dns-test" src="https://github.com/user-attachments/assets/6fe4ae52-cdf9-4968-8c0a-f134da502df4" />
<img width="1365" height="767" alt="images:04-domain-joined-win11" src="https://github.com/user-attachments/assets/72db2e49-2622-4f4c-8e02-fde239cebbfd" />
<img width="1365" height="719" alt="images:03-active directory-dc1" src="https://github.com/user-attachments/assets/0a1a2ab8-062b-4c73-9088-993675741595" />

---

## Aprendizajes Clave

- **Fundamentos de Redes:** Asignación de IPs estáticas, subredes, comprensión de direcciones APIPA e importancia del protocolo ICMP.
- **Servicios de Red:** Comprensión de la dependencia crítica entre Active Directory y el servicio DNS para la autenticación de equipos.
- **Administración Básica:** Manejo de herramientas administrativas de Windows Server y comandos esenciales de diagnóstico (`ping`, `ipconfig`, `nslookup`).
- **Resolución de Problemas:** Enfoque metodológico (troubleshooting) para identificar y corregir fallos de conectividad y unión a dominios reales.

---
 *Proyecto documentado por [Erick J. Orozco] como parte de un proceso de autoaprendizaje en Infraestructura, Redes y Soporte de TI.*
