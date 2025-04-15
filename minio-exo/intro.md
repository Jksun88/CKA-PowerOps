
<br>

## Welcome !
Install the MinIO Operator using Helm in Namespace *minio*. Then configure and create the Tenant CRD:

    1. Create Namespace minio

    2. Install Helm chart minio/operator into the new Namespace. The Helm Release should be called minio-operator

    3. Update the Tenant resource in /opt/course/2/minio-tenant.yaml to include enableSFTP: true under features

    4. Create the Tenant resource from /opt/course/2/minio-tenant.yaml