# apip-policies

Políticas y roles IAM compartidos para los repos APIP (`apip-policies`, `apip-service`).

## Estructura de cuentas/ambientes

- `accounts/aws-apip-dev/` → ambiente **dev** (`develop`)
- `accounts/aws-apip-stg/` → ambiente **stg** (`staging`)
- `accounts/aws-apip-prd/` → ambiente **prd** (`main`)

Cada carpeta contiene:

- `service-policies.yml`
- `service-roles.yml`
- `account-id` (placeholder; reemplazar por el ID real de la cuenta)

## Roles/policies relevantes

- `AWSAPIPDeployExecutionPolicy`: permisos del pipeline (GitHub Actions OIDC).
- `AWSAPIPDeployExecutionRole`: rol asumible por GitHub Actions.
- `AWSAPIPCloudFormationExecutionPolicy` / `AWSAPIPCloudFormationExecutionRole`: ejecución de CloudFormation.
- `AWSAPIPLambdaExecutionPolicy` / `AWSAPIPLambdaExecutionRole`: ejecución de Lambda API.

## Workflow de deploy

Archivo: `.github/workflows/deploy.yml`

Dispara en push a:

- `develop` → `dev`
- `staging` → `stg`
- `main` → `prd`

Usa `AWSAPIPDeployExecutionRole` en la cuenta objetivo para desplegar:

1. `apip-iam-policies` (`service-policies.yml`)
2. `apip-iam-roles` (`service-roles.yml`)

## Configuración requerida en GitHub

Crear Environments:

- `dev`
- `stg`
- `prd`

Variables por environment:

- `AWS_REGION` (por defecto sugerido: `us-east-1`)
- `APIP_GITHUB_ORG` — slug de la **organización** o **usuario** de GitHub dueño de los repos (no uses el prefijo `GITHUB_`: GitHub lo bloquea en variables de Actions)

Para `prd`, habilitar **Required reviewers** en el environment (approval obligatorio antes de desplegar).