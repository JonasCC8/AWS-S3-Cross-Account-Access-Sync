🔁 AWS S3 Cross-Account Access & Sync
📖 Descripción

Este laboratorio demuestra cómo configurar acceso entre cuentas AWS para permitir lectura de un bucket S3 desde otra cuenta y posteriormente realizar sincronización de objetos utilizando AWS CloudShell.

Escenario:

Cuenta origen: Bucket s3-aaaa

Cuenta destino: Bucket s3-bbbb

Acceso cross-account mediante Bucket Policy

Rol IAM para permisos de replicación

Sincronización usando AWS CLI

🏗️ Arquitectura

Cuenta A (Origen)
Bucket: s3-aaaa

⬇ Permiso cross-account

Cuenta B (Destino)
Bucket: s3-bbbb
Rol IAM de replicación

⬇

AWS CloudShell
Comando aws s3 sync

🎯 Objetivo

Permitir que una cuenta AWS:

Liste el bucket

Lea objetos

Configure permisos de replicación

Sincronice información entre buckets

🧩 Paso 1 – Agregar Bucket Policy (Cuenta Origen)

Ir a:

S3 → Bucket s3-aaaa → Permissions → Bucket Policy

Agregar la siguiente política:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowPromoERPRead",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::cuenta:root"
      },
      "Action": [
        "s3:ListBucket"
      ],
      "Resource": "arn:aws:s3:::s3-aaaa"
    },
    {
      "Sid": "AllowPromoERPGetObjects",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::cuenta:root"
      },
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::s3-aaaa/*"
    }
  ]
}


✅ Esto permite a la cuenta destino listar y leer objetos del bucket origen.

🧩 Paso 2 – Crear Rol IAM en la Cuenta Destino

Ir a:

IAM → Roles → Create role

Tipo:

Another AWS Account

Agregar la siguiente política al rol:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowReplicationFromS3Sanki",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::ID_CUENTA_A:role/copia-s3-rol"
      },
      "Action": [
        "s3:ReplicateObject",
        "s3:ReplicateDelete",
        "s3:ReplicateTags"
      ],
      "Resource": "arn:aws:s3:::s3-next-cloud/*"
    }
  ]
}


🔐 Buenas prácticas aplicadas:

Principio de mínimo privilegio

Permisos específicos de replicación

Restricción por recurso
