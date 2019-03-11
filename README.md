### Instalando WordPress a partir de servicios EC2 y RDS

#### 

##### 1. Crear un key pair 

> aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem



##### 2. Crear un security group

> aws ec2 create-security-group --group-name Mygroup --description "My security group"



##### 4.Configurar autorizacion para los puertos 22 (SSH), 3306 (MySQL) y 89 (http)



> aws ec2 authorize-security-group-ingress --group-name Mygroup --protocol tcp --port 22 --cidr 0.0.0.0/0

> aws ec2 authorize-security-group-ingress --group-name Mygroup --protocol tcp --port 3306 --cidr 0.0.0.0/0

> aws ec2 authorize-security-group-ingress --group-name Mygroup --protocol tcp --port 80 --cidr 0.0.0.0/0



##### 3. Crear un instance de ec2 utilizando una imagen de Linux

> aws ec2 run-instances --image-id ami-0080e4c5bc078760e --count 1 --instance-type t2.micro --key-name MyKeyPair --security-groups Mygroup



##### 4. Obtener el public DNS de la instancia 

> aws ec2 describe-instance --query "Reservations[*].Instances[*].[InstancesId,State.Name,LaunchTime,PublicIpAddress]" --output table



##### 5. Descargar Putty para conectarse via SSH

Descargar Putty y generar el Key "MyKeyPair.pkk" usando el MyKeyPair.pem siguiendo el siguiente tutorial

> https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html



##### 6. Establecer conexion via SSH con Putty

Connectarse utilizando el .pkk Key y public DNS de la instancia Ec2

> *ec2-user*@ec2-54-208-51-118.compute-1.amazonaws.com



**7. Establecer root access**

> [ec2-user@ip-172-31-86-12 ~]$ sudo su



**8. Instalando Apache Web Server **

> yum install httpd



**9. Inicializar Apache Web Server**

> service httpd start

> yum install php php-mysql

> service httpd restart



**10. Instalar MySQL**

> yum install mysql-server
>
> service mysqld start
>
>

**11. Instalar WordPress**



> cd /var/www/html
>
> wget http://wordpress.org/latest.tar.gz

> tar -xzvf latest.tar.gz



**12. Create a RDS with the MySQL Engine**

> aws rds create-db-instance --db-instance-identifier wp-db --allocated-storage 5 --db-instance-class db.t2.micro  --engine mysql  --master-username GariWordPress --master-user-password GariWordPress



