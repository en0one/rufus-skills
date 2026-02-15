---
name: ngrok
description: Experto en ngrok para exponer servicios locales, crear tunnels seguros, y gestionar acceso remoto. Usa esta skill cuando necesites compartir localhost, configurar webhooks, crear tunnels TCP/SSH, o exponer APIs locales.
---

# ngrok - Exponer Servicios Locales al Mundo

ngrok es una plataforma de networking en la nube que asegura, transforma y enruta tu tráfico a servicios corriendo en cualquier lugar. Desde compartir localhost hasta production API gateways.

## Cuándo Usar Esta Skill

- Exponer servicios locales al internet público
- Compartir localhost con otros durante desarrollo
- Recibir webhooks de servicios externos
- Crear túneles seguros para SSH/RDP
- Testing de APIs y webhooks localmente
- Conectar servicios desde fuera de la red
- API Gateway para aplicaciones

## Comandos Esenciales

### Instalación

```bash
# Linux/macOS
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.zip -o /tmp/ngrok.zip
unzip /tmp/ngrok.zip -d /usr/local/bin/

# O con npm
npm install -g ngrok
```

### Configuración Inicial

```bash
# Añadir tu authtoken
ngrok config add-authtoken TU_TOKEN

# Ver configuración
ngrok config edit
```

### Túneles Básicos

```bash
# HTTP - Exponer puerto local
ngrok http 80
ngrok http 8080
ngrok http 3000

# HTTPS local
ngrok http https://localhost:8443

# Con URL específica
ngrok http 8080 --url mi-app.ngrok.io

# TCP - Para servicios no-HTTP
ngrok tcp 22        # SSH
ngrok tcp 5432      # PostgreSQL
ngrok tcp 27017     # MongoDB

# TLS
ngrok tls 443
```

## Casos de Uso Comunes

### 1. Desarrollo Webhook

```bash
# Exponer endpoint para webhooks
ngrok http 3000

# Ver requests en tiempo real
# Visitar http://localhost:4040

# Replay requests
ngrok http 3000 --traffic-policy-file replay.yml
```

### 2. Desarrollo con队友 (Pair Programming)

```bash
# Compartir tu localhost
ngrok http 5173  # Vite/React dev server
ngrok http 4200  # Angular
ngrok http 8000  # Django
```

### 3. Testing de APIs

```bash
# Exponer API local
ngrok http 8000

# Probar desde externo
curl https://tu-subdomain.ngrok.io/api/endpoint
```

### 4. SSH Remoto

```bash
# Crear tunnel SSH
ngrok tcp 22

# Conectar desde otra máquina
ssh user@0.tcp.ngrok.io -p XXXXX
```

### 5. Base de Datos Remota

```bash
# Exponer PostgreSQL
ngrok tcp 5432

# Conectar desde otro lugar
psql -h 0.tcp.ngrok.io -p XXXXX -U user dbname
```

### 6. IoT / Dispositivos

```bash
# Device Gateway
ngrok http 8080 --device-id device-001
```

## Configuración Avanzada

### Archivo de Configuración

```yaml
# ~/.ngrok2/ngrok.yml
version: 2
authtoken: TU_TOKEN

tunnels:
  web:
    addr: 3000
    proto: http
    hostname: mi-app.ngrok.io
    
  api:
    addr: 8000
    proto: http
    auth: "user:password"
    
  ssh:
    addr: 22
    proto: tcp
    
  postgres:
    addr: 5432
    proto: tcp
    remote_addr: 1.tcp.ngrok.io:12345
```

### Variables de Entorno

```bash
export NGROK_AUTHTOKEN=TU_TOKEN
export NGROK_LOG=stdout
export NGROK_LOG_LEVEL=debug
```

### Traffic Policy

```yaml
# tp.yml
on_http_request:
  - name: add_header
    add_header:
      X-Custom-Header: "value"
  - name: log_request
    print:
      body: true
```

## Inspección y Debug

### Web Inspector

```bash
# Visitar
http://localhost:4040

# Endpoints:
# /inspect/http - HTTP requests
# /inspect/tls - TLS handshakes
# /replay - Replay requests
```

### API del Agent

```bash
# Listar túneles
curl localhost:4040/api/tunnels

# Metrics
curl localhost:4040/api/requests/http
```

## Autenticación y Seguridad

### Authtoken

```bash
# Obtener de https://dashboard.ngrok.com/get-started/your-authtoken
ngrok config add-authtoken TU_TOKEN
```

### Autenticación HTTP Basic

```bash
ngrok http 3000 --auth "username:password"
```

### OAuth

```bash
ngrok http 3000 --oauth google --oauth-allow-email "*.gmail.com"
```

### IP Restrictions

```bash
ngrok http 3000 --allow-cidr 10.0.0.0/8
ngrok http 3000 --deny-cidr 192.168.1.100/32
```

## Conceptos Clave

### Endpoints vs Tunnels

- **Endpoint**: URL pública asignada por ngrok
- **Tunnel**: Conexión entre endpoint y servicio local

### Protocols

- **http/https**: Para web apps
- **tcp**: Para cualquier servicio TCP (DB, SSH, etc)
- **tls**: Para terminación TLS

### Regions

```bash
ngrok http 3000 --region us      # US (default)
ngrok http 3000 --region eu      # Europe
ngrok http 3000 --region ap     # Asia Pacific
ngrok http 3000 --region au     # Australia
ngrok http 3000 --region sa     # South America
ngrok http 3000 --region jp     # Japan
ngrok http 3000 --region in     # India
```

## Troubleshooting

### Tunnel no conecta

```bash
# Diagnóstico
ngrok diagnose

# Ver logs
ngrok http 3000 --log stdout --log-level debug
```

### Error: "failed to bind port"

```bash
# El puerto ya está en uso
# Cambiar puerto
ngrok http 3001
```

### Session Expired

```bash
# Actualizar token
ngrok config add-authtoken NUEVO_TOKEN
```

## Recursos

- [Docs](https://ngrok.com/docs)
- [Dashboard](https://dashboard.ngrok.com)
- [API Reference](https://ngrok.com/docs/agent/api)
- [LLMs.txt](https://ngrok.com/docs/llms.txt)
