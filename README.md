# Database
Tables
echo # Database >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/JKDhupar/Database.git
git push -u origin master
CREATE TABLE Contacts
  (
    Person_PersonID NUMBER (8) NOT NULL ,
    AddressLine1    VARCHAR2 (50) NOT NULL ,
    AddressLine2    VARCHAR2 (50) ,
    "Town\City"     VARCHAR2 (50) NOT NULL ,
    Postcode        VARCHAR2 (8) NOT NULL ,
    TelephoneNumber NUMBER (11) NOT NULL ,
    Email           VARCHAR2 (254) ,
    MobileNumber    NUMBER (11) NOT NULL
  ) ;
ALTER TABLE Contacts ADD CONSTRAINT Contacts_PK PRIMARY KEY ( Person_PersonID ) ;

CREATE TABLE Course
  (
    CourseID          NUMBER (8) NOT NULL ,
    CourseName        VARCHAR2 (30) NOT NULL ,
    Employee_Staff_ID NUMBER (8) ,
    CoursePrice       NUMBER (5,2)
  ) ;
ALTER TABLE Course ADD CONSTRAINT Course_PK PRIMARY KEY ( CourseID ) ;

CREATE TABLE EmergencyContact
  (
    Person_PersonID       NUMBER (8) NOT NULL ,
    ContactRelation       VARCHAR2 (12) NOT NULL ,
    Title                 VARCHAR2 (4) NOT NULL ,
    Forename              VARCHAR2 (30) NOT NULL ,
    Surname               VARCHAR2 (30) NOT NULL ,
    AddressLine1          VARCHAR2 (50) NOT NULL ,
    AddressLine2          VARCHAR2 (50) ,
    "Town\City"           VARCHAR2 (50) NOT NULL ,
    Postcode              VARCHAR2 (8) NOT NULL ,
    EmergencyHomeNumber   NUMBER (11) NOT NULL ,
    EmergencyWorkNumber   NUMBER (11) NOT NULL ,
    EmergencyMobileNumber NUMBER (11) NOT NULL
  ) ;
ALTER TABLE EmergencyContact ADD CHECK ( Title IN ('MISS', 'MR', 'MRS', 'MS')) ;
ALTER TABLE EmergencyContact ADD CONSTRAINT EmergencyContact_PK PRIMARY KEY ( Person_PersonID ) ;

CREATE TABLE Employee
  (
    Person_PersonID NUMBER (8) NOT NULL ,
    Hire_Date       DATE NOT NULL ,
    LeaveDate       DATE ,
    Role            VARCHAR2 (50) NOT NULL ,
    Annual_Salary   NUMBER (5,2) ,
    --  ERROR: Column name length exceeds maximum allowed length(30)
    Newly_Qualified_NewlyQualifiedID NUMBER (8) NOT NULL
  ) ;
ALTER TABLE Employee ADD CONSTRAINT Employee_PK PRIMARY KEY ( Person_PersonID ) ;

CREATE TABLE EnrolledCourses
  (
    Student_StudentID NUMBER (8) NOT NULL ,
    Course_CourseID   NUMBER (8) NOT NULL ,
    Enrolled          CHAR (1) NOT NULL
  ) ;
ALTER TABLE EnrolledCourses ADD CONSTRAINT EnrolledCourses_PK PRIMARY KEY ( Student_StudentID, Course_CourseID ) ;

CREATE TABLE Grades
  (
    Student_StudentID NUMBER (8) NOT NULL ,
    Module_ModuleID   NUMBER (8) NOT NULL ,
    Grade             CHAR (3)
  ) ;
ALTER TABLE Grades ADD CONSTRAINT Grades_PK PRIMARY KEY ( Student_StudentID, Module_ModuleID ) ;

CREATE TABLE Higher_Education
  (
    --  ERROR: Column name length exceeds maximum allowed length(30)
    experience_in_HE_for_employment VARCHAR2 (5) ,
    --  ERROR: Column name length exceeds maximum allowed length(30)
    experience_in_HE_for_further_study VARCHAR2 (5) ,
    --  ERROR: Column name length exceeds maximum allowed length(30)
    "experience_in_HE_for_being_self-employed" VARCHAR2 (5) ,
    --  ERROR: Column name length exceeds maximum allowed length(30)
    experience_in_HE_for_your_own_business VARCHAR2 (5) ,
    higher_educationID                     NUMBER (8) ,
    Student_Person_PersonID                NUMBER (8) NOT NULL
  ) ;
ALTER TABLE Higher_Education ADD CONSTRAINT Higher_Education_PK PRIMARY KEY ( Student_Person_PersonID ) ;

CREATE TABLE Module
  (
    ModuleID                 NUMBER (8) NOT NULL ,
    Employee_Person_PersonID NUMBER (8) NOT NULL ,
    Room                     VARCHAR2 (10)
  ) ;
ALTER TABLE Module ADD CONSTRAINT Module_PK PRIMARY KEY ( ModuleID ) ;

CREATE TABLE ModuleList
  (
    Course_CourseID NUMBER (8) NOT NULL ,
    Module_ModuleID NUMBER (8) NOT NULL
  ) ;
ALTER TABLE ModuleList ADD CONSTRAINT ModuleList_PK PRIMARY KEY ( Course_CourseID, Module_ModuleID ) ;

CREATE TABLE Newly_Qualified
  (
    EmployeedData      DATE ,
    GTC_teacher        NUMBER ,
    "state-funded"     NUMBER ,
    "non-state-funded" NUMBER ,
    PRIMARY            NUMBER ,
    Second             NUMBER ,
    college            NUMBER ,
    NewlyQualifiedID   NUMBER (8) NOT NULL
  ) ;
ALTER TABLE Newly_Qualified ADD CONSTRAINT Newly_Qualified_PK PRIMARY KEY ( NewlyQualifiedID ) ;

CREATE TABLE Person
  (
    PersonID NUMBER (8) NOT NULL ,
    Title    CHAR (4) NOT NULL ,
    Forename VARCHAR2 (30) NOT NULL ,
    Surname  VARCHAR2 (30) NOT NULL ,
    DOB      DATE NOT NULL ,
    Active   CHAR (1) NOT NULL
  ) ;
ALTER TABLE Person ADD CHECK ( Title IN ('MISS', 'MR', 'MRS', 'MS')) ;
ALTER TABLE Person ADD CONSTRAINT Person_PK PRIMARY KEY ( PersonID ) ;

CREATE TABLE Student
  (
    Person_PersonID   NUMBER (8) NOT NULL ,
    EnrolmentDate     DATE NOT NULL ,
    PassDate          DATE ,
    Disability        CHAR (1) NOT NULL ,
    DisabilityDetails VARCHAR2 (100)
  ) ;
ALTER TABLE Student ADD CONSTRAINT Student_PK PRIMARY KEY ( Person_PersonID ) ;

--  ERROR: Table name length exceeds maximum allowed length(30)
CREATE TABLE "further_study,_training/research"
  (
    number_of_courses               NUMBER (1,5) ,
    Describeofqualification         VARCHAR2 (20) ,
    registedcourse                  VARCHAR2 (20) ,
    "subjectstudied,train&research" VARCHAR2 (40) ,
    "nameUniversity/college"        VARCHAR2 (20) ,
    --  ERROR: Column name length exceeds maximum allowed length(30)
    "mainlyfunding_studying,training_or_research" VARCHAR2 (20) ,
    Student_Person_PersonID                       NUMBER (8) NOT NULL
  ) ;
--  ERROR: PK name length exceeds maximum allowed length(30)
ALTER TABLE "further_study,_training/research" ADD CONSTRAINT "further_study,_training/research_PK" PRIMARY KEY ( Student_Person_PersonID ) ;

ALTER TABLE Contacts ADD CONSTRAINT Contacts_Person_FK FOREIGN KEY ( Person_PersonID ) REFERENCES Person ( PersonID ) ;

ALTER TABLE Course ADD CONSTRAINT Course_Employee_FK FOREIGN KEY ( Employee_Staff_ID ) REFERENCES Employee ( Person_PersonID ) ;

ALTER TABLE EmergencyContact ADD CONSTRAINT EmergencyContact_Person_FK FOREIGN KEY ( Person_PersonID ) REFERENCES Person ( PersonID ) ;

ALTER TABLE Employee ADD CONSTRAINT Employee_Newly_Qualified_FK FOREIGN KEY ( Newly_Qualified_NewlyQualifiedID ) REFERENCES Newly_Qualified ( NewlyQualifiedID ) ;

ALTER TABLE Employee ADD CONSTRAINT Employee_Person_FK FOREIGN KEY ( Person_PersonID ) REFERENCES Person ( PersonID ) ;

ALTER TABLE EnrolledCourses ADD CONSTRAINT EnrolledCourses_Course_FK FOREIGN KEY ( Course_CourseID ) REFERENCES Course ( CourseID ) ;

ALTER TABLE EnrolledCourses ADD CONSTRAINT EnrolledCourses_Student_FK FOREIGN KEY ( Student_StudentID ) REFERENCES Student ( Person_PersonID ) ;

ALTER TABLE Grades ADD CONSTRAINT Grades_Module_FK FOREIGN KEY ( Module_ModuleID ) REFERENCES Module ( ModuleID ) ;

ALTER TABLE Grades ADD CONSTRAINT Grades_Student_FK FOREIGN KEY ( Student_StudentID ) REFERENCES Student ( Person_PersonID ) ;

ALTER TABLE Higher_Education ADD CONSTRAINT Higher_Education_Student_FK FOREIGN KEY ( Student_Person_PersonID ) REFERENCES Student ( Person_PersonID ) ;

ALTER TABLE ModuleList ADD CONSTRAINT ModuleList_Course_FK FOREIGN KEY ( Course_CourseID ) REFERENCES Course ( CourseID ) ;

ALTER TABLE ModuleList ADD CONSTRAINT ModuleList_Module_FK FOREIGN KEY ( Module_ModuleID ) REFERENCES Module ( ModuleID ) ;

ALTER TABLE Module ADD CONSTRAINT Module_Employee_FK FOREIGN KEY ( Employee_Person_PersonID ) REFERENCES Employee ( Person_PersonID ) ;

ALTER TABLE Student ADD CONSTRAINT Student_Person_FK FOREIGN KEY ( Person_PersonID ) REFERENCES Person ( PersonID ) ;

--  ERROR: FK name length exceeds maximum allowed length(30)
ALTER TABLE "further_study,_training/research" ADD CONSTRAINT "further_study,_training/research_Student_FK" FOREIGN KEY ( Student_Person_PersonID ) REFERENCES Student ( Person_PersonID ) ;

