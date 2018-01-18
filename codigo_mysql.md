# Mysql_ConvenioPac
Convenio empresa
#
create database ConvenioPAC;
alter database ConvenioPAC character set latin1;
DROP TABLE TipoEmpresa;

create table TipoEmpresa(
	CodTipoEmp int(11) not null,
    NomTipoEmp varchar(200) not null
);

create table TipoConvenio(
	CodTipoConv int (11) not null,
    NomTipoConv varchar (200) not null
);

create table Empresa(
	RutEmp int (11) not null,
    NomEmp varchar (200) not null,
    CodTipoEmp int (11) not null
);

create table ConvenioPAC(
	NroConv int (11) not null,
    CodTipoConv int (11) not null, 
    NroCliEmp varchar (200) not null,
    RutEmp int (11) not null,
    RutCli int (11) not null,
    FechaIniConv datetime not null,
    FechaTermConv datetime not null,
    EstadoConv int (1) not null
);

create table Cliente (
	RutCli int (11) not null,
    ApPat varchar (200) not null,
    ApMat varchar (200) not null,
    Nombres varchar (200) not null,
    SueldoLiq int (11) not null,
    FechaIng datetime not null
);

create table CuentaCorriente(
	NroCtaCte int (11) not null,
    RutCli int (11) not null,
    FechaApertura datetime not null,
    SaldoDisp int (11) not null,
    SaldoCont int (11) not null
);

create table CargoPAC(
	NroCargo int (11) not null,
    NroConv int (11) not null,
    NroCtaCte int (11) not null,
    FechaCargo datetime not null,
    FechaRealCargo datetime not null,
    MontoCargo int (11) not null,
    EstadoCargo int (1) not null
);

ALTER TABLE ConvenioPAC DROP COLUMN NroCliEmp;

#Cambiar nombre de la tabla
RENAME TABLE CargoPac TO CargoPAC;

#Se agrega columna segundo nombre después de Nombre;
ALTER TABLE ConvenioPAC ADD COLUMN NroCliEmp int(11) AFTER CodTipoConv;
   
#Quitar llave primaria
alter table ConvenioPAC drop foreign key RelacionEmpresa_Convenio;
alter table Empresa modify RutEmp BigInt not null;
alter table ConvenioPAC modify RutEmp BigInt not null;



#Agregando Llave Primaria a las tablas.
ALTER table TipoEmpresa add PRIMARY KEY (CodTipoEmp);

ALTER table Empresa add PRIMARY KEY (RutEmp);

ALTER table TipoConvenio add PRIMARY KEY (CodTipoConv);

ALTER table ConvenioPAC add PRIMARY KEY (NroConv);

ALTER table Cliente add PRIMARY KEY (RutCli);

ALTER table CuentaCorriente add PRIMARY KEY (NroCtaCte);

ALTER table CargoPAC add PRIMARY KEY (NroCargo);

#Modificar llave principal como autoincrementable.
ALTER TABLE TipoEmpresa MODIFY CodTipoEmp int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT = 1; 
ALTER TABLE TipoConvenio MODIFY CodTipoConv int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT = 1;
ALTER TABLE ConvenioPAC MODIFY NroConv int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT = 1;  
ALTER TABLE CargoPAC MODIFY NroCargo int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT = 1; 

#Añdir llaves foraneas
ALTER TABLE Empresa add constraint RelacionTipoEmp_Empresa foreign key (CodTipoEmp) references TipoEmpresa (CodTipoEmp);

ALTER TABLE ConvenioPAC add constraint RelacionEmpresa_Convenio foreign key (RutEmp) references Empresa (RutEmp);

ALTER TABLE ConvenioPAC add constraint RelacionTipoConvenio_Convenio foreign key (CodTipoConv) references TipoConvenio (CodTipoConv);

ALTER TABLE ConvenioPAC add constraint RelacionCliente_Convenio foreign key (RutCli) references Cliente (RutCli);

ALTER TABLE CuentaCorriente add constraint RelacionCliente_CuentaCorriente foreign key (RutCli) references Cliente (RutCli);

ALTER TABLE CargoPAC add constraint RelacionConvenioPAC_CargoPAC foreign key (NroConv) references ConvenioPAC (NroConv);

ALTER TABLE CargoPAC add constraint RelacionCuentaCorriente_CargoPAC foreign key (NroCtaCte) references CuentaCorriente(NroCtaCte);
# Archivo csv
load data concurrent local infile 'C:/Users/Insuco/Desktop/Basededatos/UnirTablas/Tabla_CargoPAC.csv' into table cargopac
FIELDS TERMINATED BY ','
OPTIONALLY ENCLOSED BY '\"' #Esto es opcional en caso de que no puedan cargar la data
LINES TERMINATED BY '\n';# Si no funciona agregar \r\n

load data concurrent local infile 'C:\Users\Insuco\Desktop\Base de datos\UnirTablas\Tabla_Cliente.csv' into table cliente
FIELDS TERMINATED BY ';'
OPTIONALLY ENCLOSED BY '\"' #Esto es opcional en caso de que no puedan cargar la data
LINES TERMINATED BY '\n';

load data concurrent local infile 'C:\Users\Insuco\Desktop\Base de datos\UnirTablas\Tabla_Convenio.csv' into table conveniopac
FIELDS TERMINATED BY ';'
OPTIONALLY ENCLOSED BY '\"' #Esto es opcional en caso de que no puedan cargar la data
LINES TERMINATED BY '\n';

load data concurrent local infile 'C:\Users\Insuco\Desktop\Base de datos\UnirTablas\Tabla_CuentaCorriente.csv' into table cuentacorriente
FIELDS TERMINATED BY ';'
OPTIONALLY ENCLOSED BY '\"' #Esto es opcional en caso de que no puedan cargar la data
LINES TERMINATED BY '\n';

load data concurrent local infile 'C:\Users\Insuco\Desktop\Base de datos\UnirTablas\Tabla_Empresa.csv' into table empresa
FIELDS TERMINATED BY ';'
OPTIONALLY ENCLOSED BY '\"' #Esto es opcional en caso de que no puedan cargar la data
LINES TERMINATED BY '\n';

load data concurrent local infile 'C:/Users/Insuco/Desktop/Base de datos/UnirTablas/Tabla_TipoConvenio.csv' into table tipoconvenio
FIELDS TERMINATED BY ','
OPTIONALLY ENCLOSED BY '\"' #Esto es opcional en caso de que no puedan cargar la data
LINES TERMINATED BY '\n';

load data concurrent local infile 'C:/Users/Insuco/Desktop/Basededatos/UnirTablas/Tabla_TipoEmpresa.csv' into table tipoempresa
FIELDS TERMINATED BY ','
OPTIONALLY ENCLOSED BY '\"' #Esto es opcional en caso de que no puedan cargar la data
LINES TERMINATED BY '\n';

TRUNCATE tipoempresa;

#De acuerdo al modelo de base de datos CONVENIOPAC , realizar los ejercicios con los datos incluidos en el archivo excel.

# 1) Implemente una consulta SQL que muestre la cantidad de cuentas corrientes que posee cada cliente.
/*
1) Implemente una consulta SQL que muestre 
la cantidad de cuentas corrientes que posee
cada cliente.
*/
SELECT Cliente.RutCli as Cliente, 
COUNT(CuentaCorriente.NroCtaCte) as TotalCtaCtes
from Cliente 
INNER JOIN CuentaCorriente ON Cliente.RutCli = CuentaCorriente.RutCli
group by Cliente.RutCli;  
/*
2) Implemente una consulta SQL que muestre 
los cargos de los convenios de cada cliente a
su cuenta corriente.
*/
SELECT Cliente.RutCli as Cliente, CuentaCorriente.NroCtaCte as CtaCte,
SUM(CargoPAC.MontoCargo) as TotalCargos
from CuentaCorriente
INNER JOIN Cliente ON CuentaCorriente.RutCli = Cliente.RutCli
INNER JOIN CargoPAC ON CuentaCorriente.NroCtaCte = CargoPAC.NroCtaCte
group by CuentaCorriente.NroCtaCte;

/*
3) Implemente una consulta SQL que muestre la cantidad de cuentas 
corrientes que posee
cada cliente, agrupar consulta por apellido paterno.*/

SELECT Cliente.ApPat as ApPat, 
COUNT(CuentaCorriente.NroCtaCte) as TotalCtaCtes
from Cliente 
INNER JOIN CuentaCorriente ON Cliente.RutCli = CuentaCorriente.RutCli
group by Cliente.ApPat;
