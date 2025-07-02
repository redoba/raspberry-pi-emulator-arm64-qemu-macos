# 🍓 QEMU Raspberry Pi 3 B+ Emulator

Emula un **Raspberry Pi 3 B+** con **Ubuntu 24.04 ARM64** usando QEMU con firmware UEFI en **macOS con Apple Silicon**. Perfecto para desarrollo ARM64, testing de software y aprendizaje sin hardware físico.

> ⚠️ **IMPORTANTE**: Este emulador está diseñado para **testing de software únicamente**. No emula hardware específico como GPIO, I2C, SPI, cámaras, sensores u otros componentes físicos del Raspberry Pi. Es ideal para desarrollo de aplicaciones, testing de código ARM64 y aprendizaje del sistema operativo.

## ✨ Características

- 🖥️ **Emulación de CPU y sistema** de Raspberry Pi 3 B+ (ARM Cortex-A57, 1GB RAM)
- 🐧 **Ubuntu 24.04 LTS ARM64** con cloud-init
- 🔧 **Boot UEFI** con firmware EDK2
- 🌐 **SSH integrado** (puerto 2223)
- 📦 **Gestión automática** de imágenes y configuración
- 🚀 **Fácil de usar** con comandos simples
- 💻 **Ideal para**: Desarrollo de software, testing de aplicaciones ARM64, CI/CD

### ❌ Limitaciones del emulador
- **No emula GPIO** ni pines de entrada/salida
- **No soporta I2C, SPI, UART** físicos
- **No funciona con cámaras** o módulos de cámara
- **No emula sensores** (temperatura, movimiento, etc.)
- **No soporta HATs** o módulos de expansión físicos

## 🔧 Requisitos

### macOS (Probado y funcional)
```bash
brew install qemu cdrtools
```

> **Nota**: Este script ha sido desarrollado y probado exclusivamente en **macOS con Apple Silicon (M1/M2)**. 
> 
> `cdrtools` incluye `mkisofs` que se usa para crear el archivo cloud-init.iso que configura automáticamente la VM.

### Otros sistemas operativos
❓ **No probado**: Aunque el script debería funcionar en Linux, no ha sido probado. Las contribuciones para soporte multiplataforma son bienvenidas.

## 🚀 Inicio rápido

### 1. Clonar el repositorio
```bash
git clone https://github.com/tu-usuario/qemu-rpi3-ubuntu.git
cd qemu-rpi3-ubuntu
```

### 2. Descargar imagen Ubuntu Cloud
```bash
./qemu download
```

### 3. Crear la VM
```bash
./qemu create
```

### 4. Iniciar la VM
```bash
./qemu start
```

### 5. Conectar por SSH
```bash
ssh ubuntu@localhost -p 2223
# Contraseña: ubuntu
```

## 📋 Comandos disponibles

| Comando | Descripción |
|---------|-------------|
| `./qemu download` | Descarga la imagen Ubuntu Cloud ARM64 |
| `./qemu create` | Crea la VM usando la imagen cloud |
| `./qemu start` | Inicia la VM (SSH en puerto 2223) |
| `./qemu stop` | Apaga la VM gracefully |
| `./qemu status` | Muestra el estado del sistema |
| `./qemu delete` | Elimina la VM y archivos de trabajo |

## 🔍 Especificaciones técnicas

- **Plataforma host**: macOS con Apple Silicon (M1/M2/M3)
- **CPU emulado**: ARM Cortex-A57 quad-core
- **RAM**: 1GB (como Raspberry Pi 3 B+)
- **Almacenamiento**: 8GB expandible
- **Red**: NAT con port forwarding SSH (2223→22)
- **Boot**: UEFI con firmware EDK2
- **OS**: Ubuntu 24.04.2 LTS ARM64

### 🎯 Casos de uso recomendados
- ✅ Desarrollo de aplicaciones web en ARM64
- ✅ Testing de software multiplataforma
- ✅ Compilación cruzada y testing de binarios ARM64
- ✅ Aprendizaje de Linux en arquitectura ARM
- ✅ CI/CD para aplicaciones ARM64
- ✅ Prototipado de software antes del despliegue en hardware real

### ⚠️ NO recomendado para
- ❌ Proyectos que requieren GPIO (LEDs, motores, sensores)
- ❌ Desarrollo de drivers de hardware
- ❌ Testing de módulos específicos del Raspberry Pi
- ❌ Proyectos IoT que requieren interfaces físicas
- ❌ Cámaras, módulos de sonido o HATs

## 📁 Estructura del proyecto

```
qemu-rpi3-ubuntu/
├── qemu                    # Script principal
├── README.md              # Este archivo
├── .gitignore            # Archivos ignorados por Git
├── images/               # Imagen base Ubuntu (creado automáticamente)
├── vms/                  # Archivos de trabajo de la VM
├── config/               # Configuración cloud-init
├── firmware/             # Firmware UEFI ARM64
└── logs/                 # Registros de la VM
```

## 🛠️ Configuración avanzada

### Modificar especificaciones
Edita las variables en el script `qemu`:
```bash
MEMORY=2048        # Cambiar RAM a 2GB
CORES=2            # Cambiar a 2 cores
```

### Acceso a la VM
- **SSH**: `ssh ubuntu@localhost -p 2223`
- **Usuario**: `ubuntu`
- **Contraseña**: `ubuntu`
- **Sudo**: Sin contraseña

### Monitoreo
```bash
# Ver estado
./qemu status

# Ver logs en tiempo real
tail -f logs/vm.log

# Verificar proceso QEMU
ps aux | grep qemu
```

## 🔧 Solución de problemas

### La VM no arranca
```bash
# Verificar logs
tail -50 logs/vm.log

# Recrear la VM
./qemu delete
./qemu create
./qemu start
```

### SSH no funciona
```bash
# Verificar que la VM esté corriendo
./qemu status

# Verificar puerto
lsof -i :2223

# Esperar a que cloud-init termine (puede tomar 1-2 minutos)
```

### "¿Por qué no funciona mi código de GPIO/sensores?"
Este emulador **NO** soporta hardware físico. Si tu proyecto requiere:
- GPIO, I2C, SPI, UART
- Sensores (temperatura, movimiento, etc.)
- Cámaras o módulos de sonido
- HATs o módulos de expansión

Necesitarás usar hardware real o buscar emuladores especializados en IoT.

### Imagen corrupta
```bash
# Descargar imagen nuevamente
./qemu download

# Recrear VM
./qemu delete
./qemu create
```

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo [LICENSE](LICENSE) para más detalles.

## 🙏 Agradecimientos

- [Ubuntu Cloud Images](https://cloud-images.ubuntu.com/) por las imágenes ARM64
- [QEMU Project](https://www.qemu.org/) por el emulador
- [EDK2](https://github.com/tianocore/edk2) por el firmware UEFI
- [Ubuntu Wiki ARM64/QEMU](https://wiki.ubuntu.com/ARM64/QEMU) por la documentación

---

⭐ **¡Dale una estrella si te fue útil!** ⭐ 
