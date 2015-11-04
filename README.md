# test

--- Create DB(createDB.bat-------------
set ONEWIFI_HOME=E:\Personal\Projects\OneWiFi
set PATH="C:\Program Files\Java\jdk1.8.0_25\bin";%ONEWIFI_HOME%\mysql-cluster-gpl-7.4.7-win32\bin;%PATH%

cd %ONEWIFI_HOME%\mysql-cluster-gpl-7.4.7-win32\bin
mysqladmin -u root shutdown
start mysqld

cd %ONEWIFI_HOME%\OneWiFi_Web\DB_Scripts

mysql --user root < create_db.sql
----------------------
drop user 'onewifi'@'localhost';

drop DATABASE onewifi;

CREATE USER 'onewifi'@'localhost' identified by 'onewifi';

CREATE DATABASE onewifi;

GRANT ALL PRIVILEGES ON onewifi.* TO 'onewifi' with grant option;
flush privileges;

Use onewifi;

CREATE TABLE StatusCodeMaster(StatusCode varchar(3), StatusDetail varchar(20) NOT NULL, PRIMARY KEY (StatusCode));
	
CREATE TABLE TopupTypes(TopupType varchar(3), TopupType_Name varchar(10), PRIMARY KEY (TopupType)); 

CREATE TABLE PackageMaster(PackageCode varchar(3), PackageName varchar(20) NOT NULL, Country varchar(20) NOT NULL, Price_Per_KB double NOT NULL, PRIMARY KEY (PackageCode));

CREATE TABLE KeyValueMaster(Id varchar(3) NOT NULL, Type varchar(5) NOT NULL, keyInfo varchar(25) NOT NULL, Value varchar(25) NOT NULL, SortOrder integer NOT NULL);

CREATE TABLE DeviceMaster(DeviceId varchar(10), UserCode varchar(50) NOT NULL, Password varchar(20) NOT NULL, PartnerCode varchar(50) NOT NULL, IMEI varchar(25) NOT NULL, DeviceSerialNo varchar(25) NOT NULL, StatusCode varchar(3) NOT NULL, PRIMARY KEY (DeviceID), CONSTRAINT unique_IMEI_SNo UNIQUE (IMEI, DeviceSerialNo), FOREIGN KEY (StatusCode) REFERENCES StatusCodeMaster(StatusCode) ON DELETE CASCADE);

CREATE TABLE RetailerMaster(RetailerId varchar(50), LoginId varchar(50) NOT NULL, UserName varchar(50) NOT NULL, Password varchar(64) NOT NULL, COY_Retailer_Name varchar(100) NOT NULL, COY_Contact_Number integer, DirectorName varchar(100) NOT NULL, IdentityId varchar(10) NOT NULL, Address varchar(200) NOT NULL, Retailer_Contact_Number integer, CreationDate datetime NOT NULL, SecretQuestion1 varchar(50) NOT NULL, SecretQuestionAnswer1 varchar(50) NOT NULL, SecretQuestion2 varchar(50) NOT NULL, SecretQuestionAnswer2 varchar(50) NOT NULL, PRIMARY KEY (RetailerId), CONSTRAINT unique_LoginId_IdentityId UNIQUE (LoginId, IdentityId));

CREATE TABLE RetailerDevice(AssignerId varchar(50) NOT NULL, DeviceSerialNo varchar(25) NOT NULL, Purpose varchar(25) NOT NULL, Days double NOT NULL, Price double NOT NULL);
	
CREATE TABLE CustomerMaster(CustomerId varchar(10), LoginId varchar(50) NOT NULL, Password varchar(64) NOT NULL, FullName varchar(50) NOT NULL, IdentityType varchar(15) NOT NULL, IdentityId varchar(20) NOT NULL, Identity_Image BLOB NOT NULL, Address varchar(100) NOT NULL, DOB datetime, ContactNo integer, CreationDate datetime NOT NULL, SecretQuestion1 varchar(50) NOT NULL, SecretQuestionAnswer1 varchar(50) NOT NULL, SecretQuestion2 varchar(50) NOT NULL, SecretQuestionAnswer2 varchar(50) NOT NULL, StatusCode varchar(3) NOT NULL, PRIMARY KEY (CustomerId), CONSTRAINT unique_LoginId_IdentityId UNIQUE (LoginId, IdentityId), FOREIGN KEY (StatusCode) REFERENCES StatusCodeMaster(StatusCode) ON DELETE CASCADE); 

CREATE TABLE PartnerMaster(PartnerId varchar(10), LoginId varchar(50) NOT NULL, Password varchar(64) NOT NULL, FirstName varchar(50) NOT NULL, LastName varchar(50) NOT NULL, Address varchar(100) NOT NULL, CreationDate datetime NOT NULL, PRIMARY KEY (PartnerId), CONSTRAINT unique_LoginId UNIQUE (LoginId)); 

CREATE TABLE UserRole(LoginId varchar(50) NOT NULL, RoleName varchar(20) NOT NULL);

CREATE TABLE CustomerDevice(CustomerId varchar(10) NOT NULL, DeviceId varchar(10) NOT NULL, StatusCode varchar(3) NOT NULL, FOREIGN KEY (CustomerId) REFERENCES CustomerMaster(CustomerId) ON DELETE CASCADE, FOREIGN KEY (DeviceId) REFERENCES DeviceMaster(DeviceId) ON DELETE CASCADE, FOREIGN KEY (StatusCode) REFERENCES StatusCodeMaster(StatusCode) ON DELETE CASCADE);

CREATE TABLE CustomerTopup(CustomerId varchar(10) NOT NULL, DeviceId varchar(10) NOT NULL, TopupType VARCHAR(3) NOT NULL, TopupDate datetime NOT NULL, PriceListDate datetime NOT NULL, Package varchar(50) NOT NULL, Currency double NOT NULL, TopupAmount double NOT NULL, DateQueried datetime NOT NULL, StTimeQueried datetime NOT NULL, EndTimeQueried datetime NOT NULL, loopCnt integer, UsedCurrentAmt double NOT NULL, UsedAccAmt double NOT NULL, ExhaustedStatus varchar(30) NOT NULL, FOREIGN KEY (CustomerId) REFERENCES CustomerMaster(CustomerId) ON DELETE CASCADE, FOREIGN KEY (DeviceId) REFERENCES DeviceMaster(DeviceId) ON DELETE CASCADE, FOREIGN KEY (TopupType) REFERENCES TopupTypes(TopupType) ON DELETE CASCADE);

CREATE TABLE CustomerBalance(CustomerId varchar(10) NOT NULL, DeviceId varchar(10) NOT NULL, BalanceUpdatedDate datetime NOT NULL, BalanceAmount double NOT NULL, FOREIGN KEY (CustomerId) REFERENCES CustomerMaster(CustomerId) ON DELETE CASCADE, FOREIGN KEY (DeviceId) REFERENCES DeviceMaster(DeviceId) ON DELETE CASCADE);

CREATE TABLE Pricing(PricingId varchar(20), ProductCode varchar(10) NOT NULL, Country varchar(30) NOT NULL, PayAsYouGo double, Daily_150MB double, Days_7_450mb double, Days_30_1gb double, Days_90_2gb double, Days_180_3gb double, ValidDate datetime); 

CREATE TABLE ThresHold(ThresHoldPercent double, ThresHoldAmount double, Duration integer);

insert into statuscodemaster values('S1', 'UnAssigned');
insert into statuscodemaster values('S2', 'Operatable');
insert into statuscodemaster values('S3', 'Assigned');
insert into statuscodemaster values('S4', 'Bought');
insert into statuscodemaster values('S5', 'CustomerUsing');
insert into statuscodemaster values('S6', 'Maintenance');
insert into statuscodemaster values('S7', 'Discontinued');
insert into statuscodemaster values('S8', 'Expired');
insert into statuscodemaster values('S9', 'Disrupted');
insert into statuscodemaster values('S10', 'Dismantled');
insert into statuscodemaster values('S11', 'Failure');
insert into statuscodemaster values('S12', 'User_Active');
insert into statuscodemaster values('S13', 'User_InActive');

insert into PartnerMaster values('P1', 'onewifi@bcc.com.sg', '/5i7akkZeR+y/6EWs5dhO4aYod/Dgx2jRkHWhRV7TTE=', 'OneWifi', 'BCC', 'Singapore', '18/10/2015');

insert into UserRole values('onewifi@bcc.com.sg', 'Partner');

------------------------------------------------------------------
