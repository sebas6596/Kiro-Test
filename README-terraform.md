# Infraestructura AWS con Terraform

Esta infraestructura crea una arquitectura de alta disponibilidad en AWS con:

- VPC en us-east-1
- 2 Availability Zones (1a y 1b)
- Subnets públicas y privadas en cada AZ
- Internet Gateway
- NAT Gateways para cada AZ
- Application Load Balancer público
- Servidores web EC2 en subnets privadas
- Security Groups configurados

## Requisitos Previos

1. Terraform instalado (>= 1.0)
2. AWS CLI configurado con credenciales válidas
3. Permisos IAM adecuados para crear recursos

## Configuración de Credenciales AWS

```bash
aws configure
```

O exporta las variables de entorno:

```bash
export AWS_ACCESS_KEY_ID="tu-access-key"
export AWS_SECRET_ACCESS_KEY="tu-secret-key"
export AWS_DEFAULT_REGION="us-east-1"
```

## Uso

### 1. Inicializar Terraform

```bash
terraform init
```

### 2. (Opcional) Personalizar Variables

Copia el archivo de ejemplo y ajusta los valores:

```bash
cp terraform.tfvars.example terraform.tfvars
```

Edita `terraform.tfvars` según tus necesidades.

### 3. Revisar el Plan

```bash
terraform plan
```

### 4. Aplicar la Infraestructura

```bash
terraform apply
```

Escribe `yes` cuando se te solicite confirmar.

### 5. Obtener Outputs

Después de aplicar, verás el DNS del ALB:

```bash
terraform output alb_url
```

Visita esa URL en tu navegador para ver los servidores web funcionando.

## Recursos Creados

- 1 VPC
- 1 Internet Gateway
- 2 NAT Gateways
- 2 Elastic IPs
- 2 Subnets públicas
- 2 Subnets privadas
- 3 Route Tables
- 1 Application Load Balancer
- 1 Target Group
- 2 Instancias EC2
- 2 Security Groups

## Limpieza

Para destruir todos los recursos creados:

```bash
terraform destroy
```

## Costos Estimados

Los recursos principales que generan costos son:
- NAT Gateways (~$32/mes cada uno)
- EC2 Instances t3.micro (~$7.50/mes cada una)
- ALB (~$16/mes)

Total aproximado: ~$95/mes

Para reducir costos en desarrollo, considera usar solo 1 AZ.

## Personalización

Puedes modificar las variables en `terraform.tfvars` o `variables.tf`:

- `instance_type`: Tipo de instancia EC2
- `availability_zones`: AZs a utilizar
- `vpc_cidr`: Rango de IPs de la VPC
- `project_name`: Nombre del proyecto para etiquetar recursos

## Seguridad

- Los servidores web están en subnets privadas sin acceso directo desde Internet
- Solo el ALB es público
- Los Security Groups limitan el tráfico al mínimo necesario
- Las instancias acceden a Internet a través de NAT Gateways
