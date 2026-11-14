# 🎮 Proyecto GTA VI – SOR 1ª Evaluación

**Gestión Total de Active directory, Varios sistemas e Infraestructura**
Sistemas Operativos en Red (SOR, 0224) · 2º CFGM SMR · IES Miguel de Molinos (Zaragoza)

![Portada del proyecto GTA VI](img/portada-gta-vi.png)

> **🌴 MISIÓN CONFIDENCIAL – NIVEL DE ACCESO: 2º SMR**
>
> Un estudio de videojuegos de Zaragoza (ficticio 😉), **Leonida Labs**, ha recibido un encargo gordo: probar y monitorizar el rendimiento del **GTA VI** en una red controlada antes de las pruebas masivas con jugadores. No pueden usar la red del instituto ni internet: todo tiene que estar **aislado, controlado y documentado**.
>
> Necesitan un **laboratorio de pruebas** montado desde cero, con servidores Windows y Linux, puestos de testers con doble sistema y un portátil de administración que lo controle todo. Y nos han elegido a nosotros. Tenéis hasta el **viernes 20 de noviembre**. Si el lab no está listo… no hay lanzamiento. 🚀

📚 Apuntes: <https://mistikedu.notion.site/Sistemas-Operativos-en-Red-0224-4269af475249434c9fb794eed44774ba> · Acceso rápido: <https://tinyurl.com/sor2627>

---

## 🎯 Qué tenéis que conseguir

Os propongo un proyecto **de toda la clase**: un único laboratorio funcionando, en una red independiente de la del instituto, aplicando todo lo que hemos visto en SOR hasta ahora:

- **UD1 / RA1** – Instala sistemas operativos en red describiendo sus características e interpretando la documentación técnica.
- **UD2 / RA2** – Gestiona usuarios y grupos de sistemas operativos en red, interpretando especificaciones y aplicando herramientas del sistema.
- **UD3 / RA3** – Realiza tareas de gestión sobre dominios identificando necesidades y aplicando herramientas de administración de dominios.

## 🖥️ El laboratorio

| Equipo | Nombre sugerido | Qué debe hacer |
|---|---|---|
| 1 PC – Windows Server 2019 | `LL-DC01` | Controlador de dominio (AD DS + DNS). Dominio del estudio, p. ej. `leonida.lab`. OU, usuarios, grupos, equipos y GPO. |
| 1 PC – Linux Server (Ubuntu Server 22.04 o Zentyal 7) | `LL-SRV-LNX` | Servidor Linux con IP fija, SSH, usuarios y grupos locales del estudio, actualizado. |
| 2 PC – Dual boot Windows 10/11 + Linux Mint/Ubuntu 22 | `LL-TESTER01`, `LL-TESTER02` | Parte Windows: unida al dominio. Parte Linux: cliente del servidor Linux (acceso por SSH con usuarios del servidor). |
| 1 portátil – Administración | `LL-ADMIN` | Puesto del administrador: gestiona el DC en remoto (RSAT / PowerShell / RDP) y el servidor Linux por SSH. Sin tocar los servidores físicamente. |
| 1 switch | – | Red local del laboratorio. |
| 1 router (si hace falta) | – | Puerta de enlace propia. La red del lab **NO** se conecta a la red del instituto. |

> ⚠️ Podéis formatear e instalar lo que queráis en todos los PC y portátiles disponibles. Antes de borrar nada, **planificad** (particiones, nombres, IPs) y dejadlo escrito en la memoria.

### Plan de red de ejemplo (podéis cambiarlo, pero justificadlo)

| Equipo | IP | Máscara | Puerta de enlace | DNS |
|---|---|---|---|---|
| Router | 192.168.60.1 | /24 | – | – |
| LL-DC01 | 192.168.60.10 | /24 | 192.168.60.1 | 127.0.0.1 |
| LL-SRV-LNX | 192.168.60.20 | /24 | 192.168.60.1 | 192.168.60.10 |
| LL-TESTER01 / 02 | DHCP o .101 / .102 | /24 | 192.168.60.1 | 192.168.60.10 |
| LL-ADMIN | 192.168.60.50 | /24 | 192.168.60.1 | 192.168.60.10 |

> 💡 Si el router reparte DHCP, el DNS que entregue debe ser el del DC (192.168.60.10). Si no, los clientes no encontrarán el dominio.

## 👥 Organización: toda la clase, grupos vuestros

Trabajamos como una empresa real. Vosotros formáis los grupos y os repartís el trabajo. Os propongo esta estructura (adaptadla al número de personas):

| Grupo | Responsabilidad |
|---|---|
| 🧭 Coordinación (1-2 personas) | Jefe/a de proyecto: reparte tareas, controla plazos, une la memoria final. |
| 🪟 Equipo Windows Server | Instalación de LL-DC01, AD DS, DNS, OU, usuarios, grupos, GPO. |
| 🐧 Equipo Linux Server | Instalación de LL-SRV-LNX, red, SSH, usuarios y grupos, actualizaciones. |
| 🎮 Equipo Testers (clientes) | Particionado y dual boot de los 2 PC, unión al dominio y acceso al servidor Linux. |
| 🌐 Equipo Red y Admin | Switch, router, plan de IPs, cableado, portátil LL-ADMIN con RSAT y SSH. |

**Regla de oro:** todos tenéis que saber explicar el laboratorio completo, no solo vuestra parte. En la defensa os puedo preguntar por cualquier equipo.

## 📅 Calendario

| Fecha | Horas | Objetivo |
|---|---|---|
| Viernes 13 nov (hecho) | 2 h | Arranque: grupos, reparto, plan de red. |
| Semana 16-19 nov | 4 h | Instalación de servidores y clientes, red y dominio. Usuarios, grupos, OU, GPO, unión al dominio y acceso desde LL-ADMIN. Pruebas y memoria. |
| **Viernes 20 nov** | 2 h | **Entrega y demo en vivo** y preguntas individuales. |

En total: 8 h de clase. El viernes 20 **no se instala nada**: se enseña lo que funciona.

## ✅ Requisitos técnicos (checklist)

### UD1 / RA1 – Instalación

- [ ] Estudio de compatibilidad de cada equipo (CPU, RAM, disco, red) contra los requisitos oficiales. **UD1 / RA1, CE a**
- [ ] Modo de instalación elegido y justificado (limpia, actualización, desatendida, Core/Escritorio). **UD1 / RA1, CE b**
- [ ] Particionado planificado y realizado, especialmente el dual boot. **UD1 / RA1, CE c**
- [ ] Sistemas de archivos elegidos (NTFS, ext4…) y justificados. **UD1 / RA1, CE d**
- [ ] Roles y componentes instalados (AD DS, DNS, OpenSSH…). **UD1 / RA1, CE e**
- [ ] Al menos un procedimiento automatizado (script PowerShell/bash o instalación desatendida). **UD1 / RA1, CE f**
- [ ] Nombre de equipo, IP fija, zona horaria, teclado y entorno configurados. **UD1 / RA1, CE g**
- [ ] Sistemas actualizados. **UD1 / RA1, CE h**
- [ ] Conectividad servidor ↔ clientes comprobada. **UD1 / RA1, CE i**

### UD2 / RA2 – Usuarios y grupos

- [ ] Cuentas de usuario del estudio (dominio y Linux). **UD2 / RA2, CE a**
- [ ] Perfiles de usuario configurados. **UD2 / RA2, CE b**
- [ ] Cuentas de equipo de los testers en el dominio. **UD2 / RA2, CE c**
- [ ] Tipos y ámbitos de grupo explicados en la memoria. **UD2 / RA2, CE d**
- [ ] Grupos creados (p. ej. `G_Testers`, `G_Devs`, `G_Admins`). **UD2 / RA2, CE e**
- [ ] Usuarios metidos en sus grupos. **UD2 / RA2, CE f**
- [ ] Usuarios y grupos predeterminados identificados (Administrador, Invitado, root, sudo…). **UD2 / RA2, CE g**
- [ ] Planificación de perfiles móviles (dónde se guardarían y cómo). **UD2 / RA2, CE h**
- [ ] Uso de herramientas del sistema: consola gráfica **y** comandos. **UD2 / RA2, CE i**

### UD3 / RA3 – Dominio

- [ ] Función del servicio de directorio y nomenclatura explicadas. **UD3 / RA3, CE a**
- [ ] Concepto y funciones del dominio explicados. **UD3 / RA3, CE b**
- [ ] Relaciones de confianza: explicar en la memoria qué haríais si Leonida Labs comprara otro estudio con su propio dominio. **UD3 / RA3, CE c**
- [ ] AD DS instalado y servidor promovido a DC. **UD3 / RA3, CE d**
- [ ] Configuración básica del dominio (DNS, zona, sitio). **UD3 / RA3, CE e**
- [ ] Estructura de OU del estudio (p. ej. `Desarrollo`, `QA-Testers`, `Sistemas`) y al menos 2 GPO aplicadas. **UD3 / RA3, CE f**
- [ ] Estructura del directorio analizada (captura y explicación). **UD3 / RA3, CE g**
- [ ] Herramientas de administración usadas desde LL-ADMIN. **UD3 / RA3, CE h**

<details>
<summary><b>🧪 Comandos de comprobación para la demo</b></summary>

```powershell
# En LL-DC01 o desde LL-ADMIN (RSAT)
Get-ADDomain | Select-Object DNSRoot, PDCEmulator
Get-ADOrganizationalUnit -Filter * | Select-Object Name
Get-ADUser -Filter * -SearchBase "OU=QA-Testers,DC=leonida,DC=lab" | Select-Object SamAccountName
Get-ADGroupMember G_Testers | Select-Object Name
Get-ADComputer -Filter * | Select-Object Name
Get-GPO -All | Select-Object DisplayName
```

```bat
REM En un tester Windows (con usuario del dominio)
whoami
nltest /dsgetdc:leonida.lab
gpresult /r
ping LL-SRV-LNX
```

```bash
# En LL-SRV-LNX o en la parte Linux de un tester
hostnamectl
ip -br a
lsblk -f
getent passwd | tail -n 5
getent group | grep -E 'testers|devs'
id tester01
sudo apt update && apt list --upgradable
ssh tester01@192.168.60.20
```

</details>

## ⭐ Retos extra (suben nota)

- Script PowerShell que cree OU, grupos y usuarios desde un CSV.
- Script bash que cree los usuarios y grupos del servidor Linux.
- Instalación desatendida de Windows (`autounattend.xml`).
- Portátil LL-ADMIN gestionando el DC solo con PowerShell remoto (`Enter-PSSession LL-DC01`).

## 📓 Diario de clase (obligatorio)

Cada día de clase, **cada grupo** escribe una entrada en su diario antes de salir. Sin diario del día, esa sesión no cuenta para vuestra nota de trabajo.

Plantilla: [`plantillas/diario.md`](plantillas/diario.md)

## 📦 Producto final a entregar (viernes 20 nov)

1. **Laboratorio funcionando** y demostrado en vivo.
2. **Memoria técnica** (PDF, una por toda la clase) con: portada, organización y roles, plan de red, instalación de cada equipo (con particionado), estructura del dominio (OU, usuarios, grupos, GPO), comprobaciones con comandos, incidencias y soluciones.
3. **Diarios de clase** de todos los grupos.
4. **Scripts** usados (.ps1 / .sh).
5. **Defensa individual**: os haré preguntas a cada uno durante la demo.

La entrega se sube a Aeducar en la tarea del proyecto.

## 📊 Cómo os voy a evaluar

| Instrumento | Peso | RA |
|---|---|---|
| Laboratorio funcionando (demo) | 40 % | RA1, RA2, RA3 |
| Memoria técnica | 25 % | RA1, RA2, RA3 |
| Diario de clase | 15 % | RA1, RA2, RA3 |
| Defensa individual | 20 % | RA1, RA2, RA3 |

La nota de la demo y la memoria es común; el diario y la defensa son **individuales**. Quien no sepa explicar su parte no se lleva la nota del grupo.

> 🕹️ Recordad: en Leonida Labs no se pierde nada que esté documentado. Planificad, apuntad y probad. ¡A por ello!

---

© 2026 Luis Solsona · [CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/). Proyecto educativo; GTA VI se usa solo como ambientación ficticia.
