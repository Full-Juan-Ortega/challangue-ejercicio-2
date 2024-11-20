

# EJ 2 - Terraform:

## Consigna 

Caso 2 - Implementecion de Terraform con estado remoto y migración de recursos
El equipo quiere empezar a utilizar terraform, para esto necesita guardar el estado de terraform en AWS, entonces necesitaremos todos los componentes para guardar estos estados. El proceso debe ser documentado para poder replicarse en otras cuentas.
Tambien el equipo tiene recursos que necesita exportar a IaC, entre ellos IAM Roles, VPC y S3 buckets(no perder contenido).
Lista de recursos a importar de la cuenta #471112631123 us-east-1 :
 - VPC: vpc-node-app
 - IAM Role: role-node-app
 - S3 Bucket: node-app-backup

## Proceso de solucion.

### 1 - Conectarse a aws.
  Ya estaba logueado asique solo verifique que sea la cuenta correcta.
  `aws sts get-caller-identity #verificar login.`
### 2 - Guardar el estado de tf en aws.
  cree un bucket manual y lo llame : juan-ej02.
  `terraform init`
  [Bucket AWS](https://juan-ej02.s3.us-east-1.amazonaws.com/./terraform.tfstate)
  
### 3 - Importar : 
  - **VPC: vpc-node-app IAM Role:**
    generar el recurso vacio.
    `terraform import aws_vpc.vpc-node-app vpc-0da1ea0e28f817b81`
    `terraform state show y copiar las propiedades.`
    ver manualmente de las propiedades cuales son configurables.( las que configura aws te dan error en tf)

  - **IAM : role-node-app**
    obtener el id del rol : (no pude verlo atraves de ui de aws ).
    ``aws iam get-role --role-name role-node-app --query 'Role.RoleId' --output text`
    En este recurso prove creando el recurso y luego importandolo pero no me dejo , vi la doc oficial y decia que debia crear un bloque para importar.
    ```
    import {
      to = aws_iam_role.role-node-app
      id = "role-node-app"
    }
    ```	
    Ya con esto pude realizar el mismo procedimiento que con la vpc.

  - **S3 Bucket: node-app-backup**
    Mismo proceso que la vpc, manualmente hice algunos ajustes en la importacion por diferencia al ver el plan.

