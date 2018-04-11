CREATE TABLE HospitalPeople (
   fName VARCHAR(20) NOT NULL,
   lName VARCHAR(20) NOT NULL,
   Birthday DATE NOT NULL,
   Address VARCHAR(50),
   contactID VARCHAR(10) NOT NULL,	 	
   CONSTRAINT pk_hospitalPeople PRIMARY KEY (contactID));

CREATE TABLE Care_Center(
   careCenterName VARCHAR(60) NOT NULL,
   location VARCHAR(60),
   CONSTRAINT pk_Care_Center PRIMARY KEY (careCenterName));




CREATE TABLE Employees(
   hireDate DATE NOT NULL,
   contactID VARCHAR(10) NOT NULL,
   CONSTRAINT fk_hospitalPeople_Employees FOREIGN KEY (contactID) REFERENCES HospitalPeople(contactID),
   CONSTRAINT pk_Employees PRIMARY KEY (contactID));	


CREATE TABLE Physicians(
   specialty VARCHAR(30) NOT NULL,
   pagerNumber VARCHAR(20) NOT NULL,
   contactID VARCHAR(10) NOT NULL,
   CONSTRAINT fk_hospitalPeople_Physicians FOREIGN KEY (contactID) REFERENCES HospitalPeople(contactID),
   CONSTRAINT pk_Physicians PRIMARY KEY (contactID));


CREATE TABLE Volunteers(
   careCenterName VARCHAR(60) NOT NULL,
   contactID VARCHAR(10) NOT NULL,
   physicianID VARCHAR(10) NOT NULL,
   CONSTRAINT fk_Volunteers_Care_Center FOREIGN KEY (careCenterName) REFERENCES Care_Center(careCenterName),
   CONSTRAINT fk_HospitalPeople_Volunteers FOREIGN KEY (contactID) REFERENCES HospitalPeople(contactID),
   CONSTRAINT fk_Physicians_Volunteers FOREIGN KEY(physicianID) REFERENCES Physicians(contactID),
   CONSTRAINT pk_Volunteers PRIMARY KEY (contactID));



CREATE TABLE Patients(
   Contact_Date DATE NOT NULL,
   contactID VARCHAR(10) NOT NULL,
   physicianContactID VARCHAR(10) NOT NULL,
   CONSTRAINT fk_phys_patient FOREIGN KEY (physicianContactID) REFERENCES Physicians(contactID),
   CONSTRAINT fk_hospitalPeople_Patients FOREIGN KEY (contactID) REFERENCES HospitalPeople(contactID),
   CONSTRAINT pk_Patients PRIMARY KEY (contactID));

CREATE TABLE Technicians(
   contactID VARCHAR(10) NOT NULL,
   CONSTRAINT fk_Technicians_Employees FOREIGN KEY (contactID) REFERENCES Employees(contactID),
   CONSTRAINT pk_Technicians PRIMARY KEY (contactID));

CREATE TABLE Skills(
   Type VARCHAR(30) NOT NULL,
   contactID VARCHAR(10) NOT NULL,
   CONSTRAINT pk_Skills PRIMARY KEY(Type,contactID));

CREATE TABLE Staff(
   jobClass VARCHAR(30) NOT NULL, -- maybe make this an enumerated domain
   contactID VARCHAR(10) NOT NULL,
   CONSTRAINT fk_Staff_Employees FOREIGN KEY(contactID) REFERENCES Employees(contactID),
   CONSTRAINT pk_Staff PRIMARY KEY(contactID));

CREATE TABLE Nurse(
   contactID VARCHAR(10) NOT NULL,
   careCenterName VARCHAR(60) NOT NULL,
   /*is_Head_Nurse BOOLEAN NOT NULL,*/
   CONSTRAINT fk_Nurse_Care_Center FOREIGN KEY (careCenterName) REFERENCES Care_Center(careCenterName),
   CONSTRAINT fk_Nurse_Employees FOREIGN KEY(contactID) REFERENCES Employees(contactID),
   CONSTRAINT pk_Nurse PRIMARY KEY(contactID));

CREATE TABLE Certificates(
   contactID VARCHAR(10) NOT NULL,
   Date_Recieved DATE NOT NULL,
   Type VARCHAR(30) NOT NULL,
   CONSTRAINT fk_Certificates_Nurse FOREIGN KEY (contactID) REFERENCES Nurse(contactID),
   CONSTRAINT pk_Certificates PRIMARY KEY(contactID,Date_Recieved,Type));

CREATE TABLE Laboratory(
   labName VARCHAR(60) NOT NULL,
   location VARCHAR(60)NOT NULL,
   careCenterName VARCHAR(60) NOT NULL,
   CONSTRAINT fk_Laboratory_Care_Center FOREIGN KEY (careCenterName) REFERENCES Care_Center(careCenterName),
   CONSTRAINT pk_Laboratory PRIMARY KEY(labName));

CREATE TABLE LabInstance(
   contactID VARCHAR(10) NOT NULL,
   labName VARCHAR(60) NOT NULL,
   timeOfUse VARCHAR(60) NOT NULL,
   CONSTRAINT fk_LabInstance_Technician FOREIGN KEY(contactID) REFERENCES Technicians(contactID),
   CONSTRAINT fk_LabInstance_Laboratory FOREIGN KEY(labName) REFERENCES Laboratory(labName),
   CONSTRAINT pk_LabInstance PRIMARY KEY(contactID,timeOfUse,labName));
   
CREATE TABLE Bed(
   careCenterName VARCHAR(60) NOT NULL,
   Bed_Number INT NOT NULL, 
   Room_Number INT NOT NULL, 
   CONSTRAINT fk_Bed_Care_Center FOREIGN KEY(careCenterName) REFERENCES Care_Center(careCenterName),
   CONSTRAINT pk_Bed PRIMARY KEY(careCenterName,Bed_Number,Room_Number));

CREATE TABLE Resident(
   careCenterName VARCHAR(60) NOT NULL,
   Bed_Number INT NOT NULL, 
   Room_Number INT NOT NULL, 
   Contact_Date DATE NOT NULL,
   contactID VARCHAR(10) NOT NULL,
   physicianContactID VARCHAR(10) NOT NULL,
   admitDate DATE,
   CONSTRAINT fk_Resident_Bed FOREIGN KEY(careCenterName,Bed_Number,Room_Number) REFERENCES Bed,
   CONSTRAINT fk_Resident_Patient FOREIGN KEY(contactID) REFERENCES Patients(contactID),
   CONSTRAINT pk_Res PRIMARY KEY(careCenterName,Bed_Number,Room_Number,Contact_Date,contactID,physicianContactID));

CREATE TABLE OutPatients(
   contactID VARCHAR(10) NOT NULL,
   physicianContactID VARCHAR(10) NOT NULL,
   CONSTRAINT fk_Out_Pat FOREIGN KEY(contactID) REFERENCES Patients,
   CONSTRAINT pk_OutPat PRIMARY KEY (contactID));

CREATE TABLE OutPatientVisits(
   contactID VARCHAR(10) NOT NULL,
   physicianContactID VARCHAR(10) NOT NULL,
   visit_Date DATE NOT NULL,
   visit_Comments VARCHAR(300) NOT NULL,
   CONSTRAINT fk_Visits_OutPatent FOREIGN KEY(contactID) REFERENCES OutPatients,
   CONSTRAINT pk_Visits PRIMARY KEY(contactID,physicianContactID,visit_Date, visit_Comments));

CREATE TABLE Visitors(
   contactID VARCHAR(10) NOT NULL,
   visitResidentID VARCHAR(10) NOT NULL,
   CONSTRAINT fk_Visitors_Resident FOREIGN KEY(visitResidentID) REFERENCES Resident(contactID),
   CONSTRAINT fk_Visitors_HospitalPeople FOREIGN KEY(contactID) REFERENCES HospitalPeople(contactID)
   CONSTRAINT pk_visitors PRIMARY KEY(contactID, visitResidentID));


CREATE TABLE Maternity(
   careCenterName VARCHAR(60) NOT NULL,
   numberOfPregnantWomen INT,
   CONSTRAINT fk_Maternity_Care_Center FOREIGN KEY(careCenterName) REFERENCES Care_Center,
   CONSTRAINT pk_Maternity PRIMARY KEY(careCenterName));
   
CREATE TABLE Emergency(
   careCenterName VARCHAR(60) NOT NULL,
   numberOfEmergencyPatients INT,
   CONSTRAINT fk_Emrgency_Care_Center FOREIGN KEY(careCenterName) REFERENCES Care_Center,
   CONSTRAINT pk_Emergency PRIMARY KEY(careCenterName));

CREATE TABLE Cardiology(
   careCenterName VARCHAR(60) NOT NULL,
   numberOfHeartPatients INT,
   CONSTRAINT fk_Cardiology_Care_Center FOREIGN KEY(careCenterName) REFERENCES Care_Center,
   CONSTRAINT pk_Cardiology PRIMARY KEY(careCenterName));

CREATE TABLE Uniform(
    TYPE VARCHAR(25) NOT NULL,
    contactID VARCHAR(10) NOT NULL,
    CONSTRAINT fk_Uniform_Employee FOREIGN KEY(contactID) REFERENCES EMPLOYEES(contactID),
    CONSTRAINT pk_Uniform PRIMARY KEY(TYPE, contactID));

ALTER TABLE Care_Center
ADD NurseInCharge VARCHAR(10);

   
/*Random People Inserted*/




/*Insert Care Centers*/
INSERT INTO Care_Center
VALUES('Kanto Pokemon Center', 'Kanto', 'E225623');

INSERT INTO Care_Center
VALUES('Hoenn Pokemon Center', 'Hoenn', 'E511623');

INSERT INTO Care_Center
VALUES('American Pokemon Center', 'America', 'E050623');




/*Insert Laboritories*/
INSERT INTO Laboratory
VALUES('Dexters Lab', 'Kanto','Kanto Pokemon Center');

INSERT INTO Laboratory
VALUES('Pokemon Lab', 'Hoenn','Hoenn Pokemon Center');


/*Insert Employees*/
/*Staff*/
INSERT INTO HospitalPeople
VALUES('Peter','Schmeichel','1953-07-15','Gladsaxe, Denmark','PY10');
INSERT INTO Employees
VALUES('1999-02-17','PY10');
INSERT INTO STAFF
VALUES('Job Class 1','PY10');
INSERT INTO UNIFORM
VALUES('Staff Uniform 1', 'PY10');

INSERT INTO HospitalPeople
VALUES('Oliver','Kahn','1968-06-03','Munich, Germany','PY15');
INSERT INTO Employees
VALUES('2000-12-17','PY15');
INSERT INTO STAFF
VALUES('Job Class 1','PY15');
INSERT INTO UNIFORM
VALUES('Staff Uniform 1', 'PY15');
INSERT INTO UNIFORM
VALUES('Staff Uniform 2', 'PY15');
INSERT INTO UNIFORM
VALUES('Staff Uniform 3', 'PY15');

INSERT INTO HospitalPeople
VALUES('Eden','Hazzard','1989-12-11','Brussels, Belgium','PY25');
INSERT INTO Employees
VALUES('1998-03-01','PY25');
INSERT INTO STAFF
VALUES('Job Class 2','PY25');
INSERT INTO UNIFORM
VALUES('Staff Uniform 1', 'PY25');
INSERT INTO UNIFORM
VALUES('Staff Uniform 2', 'PY25');


/*Tecnician*/
INSERT INTO HospitalPeople
VALUES('Paul','Pogba','1988-02-05','Paris, France','PY325');
INSERT INTO Employees
VALUES('1999-02-17','PY325');
INSERT INTO Technicians
VALUES('PY325');
INSERT INTO SKILLS
VALUES('Knowledge', 'PY325');
INSERT INTO LabInstance
VALUES ('PY325','Dexters Lab','2005-09-22');
INSERT INTO UNIFORM
VALUES('Technician Uniform 1', 'PY325');


INSERT INTO HospitalPeople
VALUES('Ross','Barkley','1994-11-04','Cheshire, England','E54723');
INSERT INTO Employees
VALUES('2000-11-11','E54723');
INSERT INTO Technicians
VALUES('E54723');
INSERT INTO LabInstance
VALUES ('E54723','Pokemon Lab','2005-09-22');
INSERT INTO UNIFORM
VALUES('Technician Uniform 1', 'E54723');
INSERT INTO UNIFORM
VALUES('Technician Uniform 3', 'E54723');

INSERT INTO HospitalPeople
VALUES('Phil','Jones','1993-02-12','Manchester, England','PA46');
INSERT INTO Employees
VALUES('1933-12-12','PA46');
INSERT INTO Technicians
VALUES('PA46');
INSERT INTO SKILLS
VALUES ('Knowledge','PA46');
INSERT INTO LabInstance
VALUES('PA46', 'Pokemon Lab', '2083-08-06');
INSERT INTO UNIFORM
VALUES('Technician Uniform 2', 'PA46');


/*Nurses*/
INSERT INTO HospitalPeople
VALUES('Matteo','Darmian','1993-10-14','Turin, Italy','E225623');
INSERT INTO Employees
VALUES('2009-04-17','E225623');
INSERT INTO Nurse
VALUES('E225623','Kanto Pokemon Center');
INSERT INTO UNIFORM
VALUES('Nurse Uniform 2', 'E225623');

INSERT INTO HospitalPeople
VALUES('Patrice','Evra','1978-02-08','Monaco, France','E511623');
INSERT INTO Employees
VALUES('2009-05-16','E511623');
INSERT INTO Nurse
VALUES('E511623','Hoenn Pokemon Center');
INSERT INTO UNIFORM
VALUES('Nurse Uniform 1', 'E511623');
INSERT INTO UNIFORM
VALUES('Nurse Uniform 2', 'E511623');

INSERT INTO HospitalPeople
VALUES('Didier','Drogba','1974-04-10','Abidjan, Ivory Coast','E050623');
INSERT INTO Employees
VALUES('2012-07-07','E050623');
INSERT INTO Nurse
VALUES('E050623','American Pokemon Center');
INSERT INTO UNIFORM
VALUES('Nurse Uniform 4', 'E050623');

INSERT INTO HospitalPeople
VALUES('Marco','Reus','1990-01-24','Dortmund, Germany','E445623');
INSERT INTO Employees
VALUES('2012-09-09','E445623');
INSERT INTO Nurse
VALUES('E445623','American Pokemon Center');
INSERT INTO UNIFORM
VALUES('Nurse Uniform 5', 'E445623');
INSERT INTO UNIFORM
VALUES('Nurse Uniform 4', 'E445623');

INSERT INTO HospitalPeople
VALUES('Kevin','Trapp','1990-05-25','Stuttgart, Germany','E335623');
INSERT INTO Employees
VALUES('2012-08-08','E335623');
INSERT INTO Nurse
VALUES('E335623','American Pokemon Center');
INSERT INTO UNIFORM
VALUES('Nurse Uniform 4', 'E335623');

/*Certificate*/
INSERT INTO Certificates
VALUES('E225623','1993-10-14','RN');

INSERT INTO Certificates
VALUES('E511623','1994-11-13','RN');

INSERT INTO Certificates
VALUES('E050623','1998-01-22','RN');

INSERT INTO Certificates
VALUES('E445623','2000-01-22','RN');



/*Insert Care Center Beds*/
INSERT INTO BED
VALUES('Kanto Pokemon Center',1,1);
INSERT INTO BED
VALUES('Kanto Pokemon Center',2,1);
INSERT INTO BED
VALUES('Kanto Pokemon Center',3,1);

INSERT INTO BED
VALUES('Hoenn Pokemon Center',1,1);
INSERT INTO BED
VALUES('Hoenn Pokemon Center',2,1);
INSERT INTO BED
VALUES('Hoenn Pokemon Center',3,1);

INSERT INTO BED
VALUES('American Pokemon Center',1,1);
INSERT INTO BED
VALUES('American Pokemon Center',2,1);
INSERT INTO BED
VALUES('American Pokemon Center',3,1);




/*Insert Physicians*/
INSERT INTO HospitalPeople
VALUES('Blake','Hice','1993-07-20','6771 Trask Avenue Westminster,CA','PY4444');
INSERT INTO Physicians
VALUES('Lung Specialty','111-1111','PY4444');

INSERT INTO HospitalPeople
VALUES('Evan','McNaughtan','1994-04-04','14102 Hereford Street Westminster,CA','E4444');
INSERT INTO Physicians
VALUES('Heart Specialty','222-2222','E4444');

INSERT INTO HospitalPeople
VALUES('Adam','Wills','1994-01-05','7241 Rockmont Avenue Westminster,CA','PA7777777');
INSERT INTO Physicians
VALUES('Stomach Specialty','333-3333','PA7777777');

INSERT INTO HospitalPeople
VALUES('Nick','Grant','1995-03-02','HB','PY1251');
INSERT INTO Physicians
VALUES('Ear Wax Specialty','444-4444','PY1251');



/*Insert Volunteers*/
INSERT INTO HospitalPeople
VALUES('David','James','1983-03-10','London, England','PY01');
INSERT INTO VOLUNTEERS
VALUES('Kanto Pokemon Center','PY01', 'PA7777777');
INSERT INTO SKILLS
VALUES('Strength', 'PY01');

INSERT INTO HospitalPeople
VALUES('Joe','Hart','1980-02-22','Manchester, England','PY05');
INSERT INTO VOLUNTEERS
VALUES('Kanto Pokemon Center','PY05', 'PA7777777');
INSERT INTO SKILLS
VALUES('Speed', 'PY05');

INSERT INTO HospitalPeople
VALUES('Edwin','Van Der Sar','1973-09-29','Eidenhoven, Netherlands','PY06');
INSERT INTO VOLUNTEERS
VALUES('American Pokemon Center','PY06', 'PY1251');

INSERT INTO HospitalPeople
VALUES('Eric','Cantona','1963-08-12','Marseille, France','PY08');
INSERT INTO VOLUNTEERS
VALUES('American Pokemon Center','PY08','PY1251');




/*Insert Patients*/
/*Resident*/

INSERT INTO HospitalPeople
VALUES('Chris','Smalling','1989-06-03','Manchester, England','PA456');
INSERT INTO Patients
VALUES('2013-01-01', 'PA456','E4444');
INSERT INTO Resident
VALUES('Kanto Pokemon Center',3,1,'2013-01-01', 'PA456','E4444', '2015-01-01');

INSERT INTO HospitalPeople
VALUES('Sergio','Aguero','1985-03-20','Buenos Aires, Argentina','PA045546');
INSERT INTO Patients
VALUES('2000-12-15','PA045546','PY4444');
INSERT INTO Resident
VALUES('Kanto Pokemon Center',2,1,'2000-12-15','PA045546','PY4444','2000-12-18');

INSERT INTO HospitalPeople
VALUES('Ander','Herrera','1991-11-15','Bilbao, Spain','PA14466');
INSERT INTO Patients
VALUES('2014-11-15','PA14466','PY4444');
INSERT INTO Resident
VALUES('Kanto Pokemon Center',1,1,'2014-11-15','PA14466','PY4444','2014-11-25');

INSERT INTO HospitalPeople
VALUES('Shinji','Kagawa','1990-01-20','Osaka, Japan','PA26');
INSERT INTO Patients
VALUES('2018-10-10','PA26','E4444');
INSERT INTO Resident
VALUES('American Pokemon Center', 1, 1,'2018-10-10','PA26','E4444', '2018-10-12');

INSERT INTO HospitalPeople
VALUES('James','Jones','1963-01-20','1234 Lincoln Blvd. Long Beach,CA','PA06');
INSERT INTO Patients
VALUES('1994-06-06','PA06','PY4444');
INSERT INTO Resident
VALUES('Hoenn Pokemon Center',1,1,'1994-06-06','PA06','PY4444','1994-06-07');


/*OutPatient*/
INSERT INTO HospitalPeople
VALUES('David','De Gea','1989-10-12','Madrid, Spain','E56234');
INSERT INTO Patients
VALUES('2002-11-05','E56234','E4444');
INSERT INTO OutPatients
VALUES('E56234','E4444');
INSERT INTO OUTPATIENTVISITS
VALUES('E56234','E4444', '2018-11-19','Very UGLY! Just Kidding'); /*At least one visit*/


INSERT INTO HospitalPeople
VALUES('Hugo','Lorris','1987-05-14','Marseille, France','E56236');
INSERT INTO Patients
VALUES('2017-11-15','E56236','PY4444');
INSERT INTO OutPatients
VALUES('E56236','PY4444');
INSERT INTO VOLUNTEERS
VALUES('American Pokemon Center','E56236','PY4444');/*Also a Volunteer as well as Patient*/
INSERT INTO OUTPATIENTVISITS
VALUES('E56236','PY4444', '2017-11-19','Very Healthy Young Lad!');
INSERT INTO OUTPATIENTVISITS
VALUES('E56236','PY4444', '2017-11-19','Looks even more healthy!');
INSERT INTO OUTPATIENTVISITS
VALUES('E56236','PY4444', '2017-11-19','I take back everything I said!');
INSERT INTO OUTPATIENTVISITS
VALUES('E56236','PY4444', '2017-11-19','Nevermind...');/*QUERY #13, >3 visits from physician in one day*/


INSERT INTO HospitalPeople
VALUES('Adam''s','Friend','1994-04-04','14102 Hereford Street Westminster,CA','PA4444');
INSERT INTO Patients
VALUES('2009-12-04','PA4444','E4444');
INSERT INTO OutPatients
VALUES('PA4444','E4444');
INSERT INTO OUTPATIENTVISITS
VALUES('PA4444','PY4444','2017-11-19','Damn...');/*QUERY #15 where physician not responsible for patient has visit*/


INSERT INTO HospitalPeople
VALUES('Kevin','Smith','1974-10-04','1502 Za Street Long Beach,CA','E5623');
INSERT INTO Patients
VALUES('2019-01-02','E5623','E4444');
INSERT INTO OutPatients
VALUES('E5623','E4444');

INSERT INTO HospitalPeople
VALUES('Puto','Jones','1993-07-20','6771 Trask Avenue Westminster,CA','PA04');
INSERT INTO Patients
VALUES('2002-12-02','PA04','E4444');
INSERT INTO OutPatients
VALUES('PA04','E4444');



/*View 1*/
CREATE VIEW EmployeesHiredView AS
SELECT HospitalPeople.FNAME, HospitalPeople.LNAME, Employees.HIREDATE 
FROM Employees 
INNER JOIN HospitalPeople 
ON Employees.contactID = HospitalPeople.contactID;

/*View 2*/
CREATE VIEW NursesInChargeView AS
SELECT HospitalPeople.FNAME,HospitalPeople.LNAME
FROM Care_Center 
INNER JOIN Nurse
ON Care_Center.careCenterName = Nurse.careCenterName
INNER JOIN HospitalPeople
ON Nurse.contactID = HospitalPeople.contactID
WHERE Nurse.contactID = Care_Center.NurseINCHARGE;

/*View 3*/
CREATE VIEW goodTechnicianView AS
SELECT fname, lname
FROM HospitalPeople
INNER JOIN Technicians
ON HospitalPeople.contactID=Technicians.contactID
INNER JOIN SKILLS
ON skills.contactID=technicians.contactID;

/*View 4*/
CREATE VIEW Care_CenterTotalBeds AS
SELECT Bed.careCenterName, COUNT(Bed.careCenterName) As "TotalBeds" 
FROM Care_Center 
INNER JOIN Bed
ON Care_Center.careCenterName = bed.careCenterName
GROUP BY Bed.careCenterName;

CREATE VIEW Care_CenterOccupiedBeds AS
SELECT resident.careCenterName, COUNT(resident.careCenterName) As "OccupiedBeds" 
FROM Care_Center 
INNER JOIN Bed
ON Care_Center.careCenterName = Bed.careCenterName
INNER JOIN Resident
ON Bed.BED_NUMBER = Resident.BED_NUMBER AND Bed.careCenterName = resident.careCenterName
GROUP BY resident.careCenterName;

    /*possibly remove quotation marks for mysql*/
CREATE VIEW Care_CenterBedsView AS
SELECT Care_CenterTotalBeds.careCenterName, Care_CenterTotalBeds."TotalBeds", 
coalesce(Care_CenterOccupiedBeds."OccupiedBeds",0) 
AS "OccupiedBeds",Care_CenterTotalBeds."TotalBeds" - coalesce(Care_CenterOccupiedBeds."OccupiedBeds",0) AS "FreeBeds" 
FROM Care_CenterTotalBeds LEFT OUTER JOIN Care_CenterOccupiedBeds
ON Care_CenterTotalBeds.careCenterName = Care_CenterOccupiedBeds.careCenterName;

/*View 5*/
CREATE VIEW OutPatientsNotVisitedView AS 
SELECT HospitalPeople.FNAME, HospitalPeople.LNAME, HospitalPeople.contactID 
FROM HospitalPeople 
INNER JOIN Patients 
ON HospitalPeople.contactID = Patients.contactID
INNER JOIN OutPatients 
ON OutPatients.contactID = Patients.contactID
WHERE OutPatients.contactID 
NOT IN
(SELECT Patients.contactID 
FROM Patients
INNER JOIN OutPatients 
ON OutPatients.contactID = Patients.contactID
INNER JOIN OutPatientVisits 
ON OutPatientVisits.contactID = OutPatients.contactID);



/*Query 1*/

SELECT fname, lname, 'Job Class 1' AS source
FROM HospitalPeople
INNER JOIN STAFF
ON HospitalPeople.CONTACTID=Staff.CONTACTID
WHERE staff.JOBCLASS='Job Class 1' 
UNION
SELECT fname, lname, 'Job Class 2' AS source
FROM HospitalPeople
INNER JOIN STAFF
ON HospitalPeople.CONTACTID=Staff.CONTACTID
WHERE staff.JOBCLASS='Job Class 2'; 


/*Query 2*/
SELECT fname, lname
FROM HospitalPeople
INNER JOIN Volunteers
ON HospitalPeople.CONTACTID=Volunteers.CONTACTID
EXCEPT
SELECT FNAME, LNAME
FROM HOSPITALPEOPLE
INNER JOIN Volunteers
ON Volunteers.CONTACTID=HospitalPeople.CONTACTID
INNER JOIN SKILLS
ON SKILLS.CONTACTID=Volunteers.CONTACTID;


/*Query 3*/
SELECT fname,lname
FROM HospitalPeople
INNER JOIN Volunteers
ON hospitalpeople.CONTACTID=volunteers.CONTACTID
INNER JOIN Patients
ON hospitalPeople.CONTACTID=Patients.CONTACTID;

/*Query 4*/
SELECT fname,lname
FROM HospitalPeople 
INNER JOIN Patients
ON HospitalPeople.CONTACTID = Patients.CONTACTID
INNER JOIN OutPatients
ON Patients.CONTACTID = outPatients.CONTACTID
INNER JOIN OutPatientVisits
ON outPatients.CONTACTID = outpatientvisits.CONTACTID
GROUP BY fname,lname
HAVING COUNT(outpatientvisits.CONTACTID) = 1;

/*Query 5*/
SELECT DISTINCT skills."TYPE", COUNT(skills."TYPE") AS "Number of People With skill"
FROM skills
GROUP BY skills."TYPE";

/*Query 6*/
/*Possibly remove Quotation marks for sql*/
SELECT Care_Center.careCenterName
FROM (SELECT * FROM Care_CenterBedsView) t1 
INNER JOIN Care_Center
ON t1.careCenterName = Care_Center.careCenterName
WHERE t1."OccupiedBeds" = t1."TotalBeds";

/*Query 7*/
SELECT HospitalPeople.FNAME, HospitalPeople.LNAME, Care_Center.careCenterName 
FROM HospitalPeople 
INNER JOIN Employees 
ON HospitalPeople.contactID = Employees.contactID
INNER JOIN Nurse 
ON Nurse.contactID = Employees.contactID
INNER JOIN Care_Center 
ON Nurse.careCenterName = Care_Center.careCenterName
INNER JOIN Certificates
ON Certificates.CONTACTID=Nurse.CONTACTID
WHERE Care_Center.NurseInCharge != Nurse.contactID AND Certificates.TYPE = 'RN';

/*Query 8*/
SELECT HospitalPeople.FNAME,HospitalPeople.LNAME, Care_Center.CARECENTERNAME
FROM HospitalPeople 
INNER JOIN Employees 
ON HospitalPeople.contactID = Employees.contactID
INNER JOIN Nurse 
ON Nurse.contactID = Employees.contactID
INNER JOIN Care_Center 
ON Nurse.careCenterName = Care_Center.careCenterName
INNER JOIN Certificates
ON Certificates.CONTACTID=Nurse.CONTACTID
WHERE Care_Center.NURSEINCHARGE=Nurse.CONTACTID AND Nurse.CARECENTERNAME=Care_Center.CARECENTERNAME;

/*Query 9*/
SELECT Laboratory.LABNAME, Count(Technicians.CONTACTID) AS "NumOfTechs"
FROM LABORATORY
INNER JOIN LabInstance
ON LABINSTANCE.LABNAME=laboratory.LABNAME
INNER JOIN TECHNICIANS
ON Technicians.CONTACTID=labinstance.CONTACTID
GROUP BY Laboratory.LABNAME
EXCEPT
SELECT Laboratory.LABNAME, Count(Technicians.CONTACTID) AS "NumOfTechs"
FROM LABORATORY
INNER JOIN LabInstance
ON LABINSTANCE.LABNAME=laboratory.LABNAME
INNER JOIN TECHNICIANS
ON Technicians.CONTACTID=labinstance.CONTACTID
INNER JOIN SKILLS
ON SKILLS.CONTACTID=LabInstance.CONTACTID
GROUP BY LABORATORY.LABNAME;

/*Query 10*/
SELECT HospitalPeople.FNAME, HospitalPeople.LNAME
FROM HospitalPeople 
INNER JOIN Resident
ON HospitalPeople.contactID = Resident.contactID
WHERE Resident.ADMITDATE >  (
SELECT MAX(EMPLOYEES.HIREDATE)
FROM Employees);

/*Query 11*/
SELECT HospitalPeople.FNAME, HospitalPeople.LNAME 
FROM HospitalPeople 
INNER JOIN Patients 
ON HospitalPeople.contactID = Patients.contactID
INNER JOIN Resident 
ON Resident.contactID = Patients.contactID
WHERE {fn timestampdiff(SQL_TSI_DAY,RESIDENT.CONTACT_DATE, Resident.ADMITDATE)} < 7;

/*Query 12*/
SELECT HospitalPeople.FNAME, HospitalPeople.LNAME, Patients.Contact_Date,OutPatientVisits.VISIT_DATE
FROM HospitalPeople 
INNER JOIN Patients 
ON HospitalPeople.contactID = Patients.contactID
INNER JOIN OutPatients 
ON Patients.contactID = outpatients.contactID
INNER JOIN OutPatientVisits 
ON outpatients.contactID = OutPatientVisits.contactID
WHERE HospitalPeople.contactID NOT IN
(SELECT HospitalPeople.contactID 
FROM HospitalPeople 
INNER JOIN Patients 
ON HospitalPeople.contactID = Patients.contactID
INNER JOIN Outpatients 
ON Patients.contactID = outpatients.contactID
INNER JOIN OutPatientVisits 
ON outpatients.contactID = OutPatientVisits.contactID
WHERE {fn timestampdiff(SQL_TSI_DAY,Patients.Contact_Date, OutPatientVisits.VISIT_DATE)} < 7);

/*Query 13*/
SELECT HospitalPeople.FNAME, HospitalPeople.LNAME 
FROM(
SELECT Physicians.contactID 
FROM HospitalPeople 
INNER JOIN Physicians 
ON HospitalPeople.contactID = Physicians.contactID
INNER JOIN OUTPATIENTVISITS 
ON Physicians.contactID = OUTPATIENTVISITS.PHYSICIANCONTACTID
GROUP BY OUTPATIENTVISITS.VISIT_DATE,Physicians.contactID
HAVING COUNT(Physicians.contactID)>3) p3
INNER JOIN HospitalPeople
ON HospitalPeople.contactID = p3.contactID;

/*Query 14*/
SELECT HospitalPeople.FNAME, HospitalPeople.LNAME, HospitalPeople.contactID 
FROM HospitalPeople 
NATURAL JOIN Physicians 
NATURAL JOIN(
SELECT Physicians.contactID, coalesce(pO."outPatientCount",0) - coalesce(pR."residentCount",0) AS "patientDiff" FROM
Physicians LEFT OUTER JOIN
(SELECT patients.PHYSICIANCONTACTID, COUNT(patients.PHYSICIANCONTACTID) AS "outPatientCount" FROM
HospitalPeople INNER JOIN patients ON HospitalPeople.contactID = patients.contactID
INNER JOIN outpatients ON outpatients.contactID = patients.contactID
GROUP BY patients.PHYSICIANCONTACTID) pO
ON Physicians.contactID = pO.PHYSICIANCONTACTID
LEFT OUTER JOIN
(SELECT patients.PHYSICIANCONTACTID, COUNT(patients.PHYSICIANCONTACTID) AS "residentCount" FROM
HospitalPeople INNER JOIN patients ON HospitalPeople.contactID = patients.contactID
INNER JOIN resident ON resident.contactID = patients.contactID
GROUP BY patients.PHYSICIANCONTACTID) pR
ON Physicians.contactID = pR.PHYSICIANCONTACTID) pD
WHERE pD."patientDiff" > 0;

/*Query 15*/
SELECT hospitalpeople.FNAME, hospitalpeople.LNAME, hospitalpeople.contactID FROM
hospitalpeople INNER JOIN OutPatientVisits ON hospitalpeople.contactID = OutPatientVisits.PHYSICIANCONTACTID
INNER JOIN outpatients ON outpatientvisits.contactID = outpatients.contactID
INNER JOIN patients ON patients.contactID = outpatients.contactID
WHERE outpatientvisits.PHYSICIANCONTACTID != patients.PHYSICIANCONTACTID;

/*Query 16 BUSINESS RULES*/

/* A. How many uniforms does each employee have?*/
SELECT HospitalPeople.CONTACTID, COUNT(Uniform."TYPE")AS "NumberOfUniforms"
FROM HospitalPeople
INNER JOIN Employees
ON Employees.CONTACTID=HospitalPeople.CONTACTID
INNER JOIN Uniform
ON Employees.CONTACTID=Uniform.CONTACTID
GROUP BY HospitalPeople.CONTACTID;

/*B. How many Physicians have Volunteers assigned to them*/
SELECT Physicians.CONTACTID, COUNT(Volunteers.PHYSICIANID) AS "NumberOfVolunteersAssigned"
FROM Physicians
INNER JOIN Volunteers
ON Volunteers.PHYSICIANID=Physicians.CONTACTID
GROUP BY Physicians.CONTACTID;

/*C. Find which Care Centers have volunteers and how many*/
SELECT Care_Center.CARECENTERNAME, COUNT(Volunteers.CARECENTERNAME) AS "NumberOfVolunteersAssigned"
FROM Care_Center
INNER JOIN Volunteers
ON Volunteers.CARECENTERNAME=Care_Center.CARECENTERNAME
GROUP BY Care_Center.CARECENTERNAME;




