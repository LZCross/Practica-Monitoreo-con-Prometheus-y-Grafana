# Práctica de Monitoreo con Prometheus y Grafana

Lizander Cross 2021-1754

Una práctica sencilla para configurar un sistema de monitoreo con Prometheus y Grafana usando Docker Compose.

## Estructura del proyecto

```
.
├── docker-compose.yml      # Configuración de los servicios Docker
├── prometheus.yml          # Configuración de Prometheus
└── README.md              # Este archivo
```

## Servicios incluidos

- **Prometheus**: Sistema de monitoreo y almacenamiento de métricas (http://localhost:9090)
- **Grafana**: Herramienta de visualización (http://localhost:3000)
- **Node-Exporter**: Exportador de métricas del sistema (http://localhost:9100)

## Requisitos previos

- Docker y Docker Compose instalados en tu sistema

## Pasos para ejecutar la práctica

### 1. Levantar los contenedores

Ejecuta el siguiente comando en la carpeta del proyecto:

```bash
docker-compose up -d
```

### 2. Verificar que los servicios estén corriendo

```bash
docker-compose ps
```

Deberías ver los tres contenedores corriendo: prometheus, grafana y node-exporter.

### 3. Acceder a Prometheus

Abre tu navegador y ve a:
```
http://localhost:9090
```

Aquí puedes:
- Ver los objetivos monitoreados en la pestaña "Targets"
- Ejecutar consultas PromQL en la pestaña "Graph"
- Ver las métricas disponibles

### 4. Acceder a Grafana

Abre tu navegador y ve a:
```
http://localhost:3000
```

Credenciales por defecto:
- **Usuario**: admin
- **Contraseña**: admin

## Configurar Grafana para mostrar métricas

### Paso 1: Agregar fuente de datos Prometheus

1. En el menú izquierdo, ve a **Connections** → **Data sources**
2. Haz clic en **Add data source**
3. Selecciona **Prometheus**
4. En la URL ingresa: `http://prometheus:9090`
5. Haz clic en **Save & test**

### Paso 2: Crear un dashboard

1. Ve a **Dashboards** → **Create** → **New dashboard**
2. Haz clic en **Add panel**
3. En la sección de métricas, puedes usar consultas como:
   - `node_cpu_seconds_total` - Tiempo CPU en segundos
   - `node_memory_MemFree_bytes` - Memoria libre en bytes
   - `node_disk_io_now` - E/S de disco actual
   - `node_network_receive_bytes_total` - Bytes recibidos por red

## Métricas comunes disponibles

Estas son algunas de las métricas que puedes usar en tus paneles:

- `node_cpu_seconds_total` - Tiempo CPU total
- `node_memory_MemTotal_bytes` - Memoria total
- `node_memory_MemFree_bytes` - Memoria libre
- `node_memory_MemAvailable_bytes` - Memoria disponible
- `node_disk_io_now` - I/O de disco actual
- `node_network_receive_bytes_total` - Bytes recibidos
- `node_network_transmit_bytes_total` - Bytes transmitidos
- `up` - Estado del target (1 = activo, 0 = inactivo)

## Comandos útiles

```bash
# Ver logs de los contenedores
docker-compose logs -f prometheus
docker-compose logs -f grafana
docker-compose logs -f node-exporter

# Detener los servicios
docker-compose down

# Detener y eliminar volúmenes
docker-compose down -v

# Reiniciar los servicios
docker-compose restart
```

## Notas

- Los datos de Prometheus se almacenan en la carpeta `prometheus/` del contenedor
- Grafana almacena su configuración en `grafana/` del contenedor
- Para persistencia de datos, considera agregar volúmenes nombrados en docker-compose.yml
- El intervalo de scrape está configurado a 15 segundos

## Troubleshooting

### No se ve ningún target en Prometheus
- Verifica que node-exporter esté corriendo: `docker-compose ps`
- Revisa los logs: `docker-compose logs node-exporter`

### Grafana no se conecta a Prometheus
- Asegúrate de usar `http://prometheus:9090` como URL (nombre del servicio, no localhost)
- Verifica que ambos contenedores estén en la misma red

### Los datos no se actualizan
- Verifica el intervalo de scrape en prometheus.yml (configurado a 15s)
- Revisa el estado de los targets en http://localhost:9090/targets

## Próximos pasos

Una vez que domines esta práctica, puedes:
- Agregar más exportadores (MySQL, PostgreSQL, Redis, etc.)
- Crear dashboards personalizados
- Configurar alertas en Prometheus
- Usar AlertManager para notificaciones
- Implementar Loki para logs centralizados
