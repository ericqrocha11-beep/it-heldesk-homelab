# 🖥️ IT Help Desk & Systems Lab (Active Directory Environment)

## 📌 Descripción del Proyecto
Este laboratorio personal emula una infraestructura de red empresarial completa utilizando entorno virtualizado. El objetivo es practicar tareas habituales de soporte técnico Nivel 1 y administración básica de sistemas.

---

## 🛠️ Tecnologías y Herramientas Utilizadas
* **Hipervisor:** Oracle VM VirtualBox
* **Servidores:** Windows Server 2022 (Domain Controller)
* **Clientes:** Windows 10/11 Enterprise
* **Servicios de Red:** Active Directory Domain Services (AD DS), DNS, DHCP
* **Herramientas de Soporte:** PowerShell, CMD, Remote Desktop (RDP)

---

## ⚙️ Configuración de la Red Virtual
* **Nombre del Dominio:** `empresa.local`
* **Rango IP Interno:** `192.168.10.0/24` (Red Interna en VirtualBox)
* **DC1 (Servidor Principal):** `192.168.10.2`
* **PC-CLIENTE-01:** Obtención de IP vía DHCP

---

## 📸 Prácticas Realizadas y Evidencia

### 1. Despliegue de Active Directory y Estructura Organizativa
Se configuró el rol de AD DS y se creó la estructura de Unidades Organizativas (OUs) dividida por departamentos (Sistemas, Ventas, RRHH).

![Estructura de OUs](images/01-active-directory-ous.png)

### 2. Gestión de Usuarios y Permisos
* Creación de cuentas de usuario con políticas de contraseña predeterminadas.
* Asignación de grupos de seguridad para acceso a recursos compartidos.
* Simulación de incidentes N1: Desbloqueo de cuentas y restablecimiento de credenciales.

![Creación de Usuarios](images/02-users-creation.png)

### 3. Unión de Equipos Cliente al Dominio
Se configuraron los parámetros de red y DNS en la máquina virtual cliente para unirla exitosamente a `empresa.local`.

![Win10 en Dominio](images/03-domain-joined.png)

---

## 🎯 Habilidades Validadas
- Instalación y virtualización de S.O.
- Administración de identidades en AD.
- Diagnóstico de conectividad básica (Ping, Tracert, IPConfig).
- Creación de documentación técnica clara.
