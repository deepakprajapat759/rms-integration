# rms-integration



deepakprajapat759/RMSFinal
Copy only eclipse project.


------------------Mysql

-- MySQL Script generated by MySQL Workbench
-- Thursday 04 July 2019 04:11:27 PM IST
-- Model: New Model    Version: 1.0
-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='TRADITIONAL,ALLOW_INVALID_DATES';

-- -----------------------------------------------------
-- Schema RMS
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema RMS
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `RMS` DEFAULT CHARACTER SET utf8 ;
USE `RMS` ;

-- -----------------------------------------------------
-- Table `RMS`.`USER`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `RMS`.`USER` (
  `USER_ID` INT NOT NULL AUTO_INCREMENT,
  `USER_NAME` VARCHAR(30) NOT NULL,
  `USER_EMAIL` VARCHAR(45) NOT NULL,
  `USER_PASSWORD` VARCHAR(70) NOT NULL,
  `USER_ROLE` VARCHAR(25) NOT NULL,
  `USER_AI_STATUS` INT(1) NOT NULL,
  `CREATION_DATE` DATETIME NOT NULL,
  `MODIFICATION_DATE` DATETIME NOT NULL,
  PRIMARY KEY (`USER_ID`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `RMS`.`LENDER`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `RMS`.`LENDER` (
  `LENDER_ID` INT NOT NULL,
  `LOAN_INTEREST` INT NOT NULL,
  `LENDER_DESCRIPTION` VARCHAR(120) NOT NULL,
  `TENURE_RANGE` VARCHAR(30) NOT NULL,
  `LOAN_AMOUNT_RANGE` VARCHAR(30) NOT NULL,
      CONSTRAINT `LENDER_ID`
    FOREIGN KEY (`LENDER_ID`)
    REFERENCES `RMS`.`USER` (`USER_ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `RMS`.`POLICY`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `RMS`.`POLICY` (
  `POLICY_ID` INT NOT NULL AUTO_INCREMENT,
  `POLICY_NAME` VARCHAR(30) NOT NULL,
  `LENDER_ID` INT NOT NULL,
  `CREATION_DATE` DATETIME NOT NULL,
  `MODIFICATION_DATE` DATETIME NOT NULL,
  PRIMARY KEY (`POLICY_ID`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `RMS`.`MAPPING_LENDER_POLICY`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `RMS`.`MAPPING_LENDER_POLICY` (
  `LENDER_ID` INT NOT NULL,
  `POLICY_ID` INT NOT NULL,
  `THRESHOLD` VARCHAR(20) NOT NULL,
  `POLICY_WEIGHTAGE` INT(1) NOT NULL,
  `POLICY_STATUS` INT(1) NOT NULL,
      FOREIGN KEY (`LENDER_ID`)
    REFERENCES `RMS`.`USER` (`USER_ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
      FOREIGN KEY (`POLICY_ID`)
    REFERENCES `RMS`.`POLICY` (`POLICY_ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `RMS`.`CREDIT_APPLICATION`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `RMS`.`CREDIT_APPLICATION` (
  `APPLICATION_ID` INT NOT NULL AUTO_INCREMENT,
  `CREDIT_SCORE` INT(1) NOT NULL,
  `APPLICATION_STATUS` INT(1) NOT NULL,
  `COMPANY_NAME` VARCHAR(30) NOT NULL,
  `BORROWER_ID` INT NOT NULL,
  `FINANCIAL_ANALYST_ID` INT NOT NULL,
  `LENDER_ID` INT NOT NULL,
  `CREATION_DATE` DATETIME NOT NULL,
  `MODIFICATION_DATE` DATETIME NOT NULL,
  PRIMARY KEY (`APPLICATION_ID`),

        FOREIGN KEY (`BORROWER_ID`)
    REFERENCES `RMS`.`USER` (`USER_ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
     
    FOREIGN KEY (`LENDER_ID`)
    REFERENCES `RMS`.`USER` (`USER_ID`)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `RMS`.`FINANCIAL_ANALYST`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `RMS`.`FINANCIAL_ANALYST` (
  `FINANCIAL_ANALYST_ID` INT NOT NULL PRIMARY KEY,
  `LENDER_ID` INT ,
        FOREIGN KEY (`FINANCIAL_ANALYST_ID`)
    REFERENCES `RMS`.`USER` (`USER_ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
    FOREIGN KEY (`LENDER_ID`)
    REFERENCES `RMS`.`USER` (`USER_ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `RMS`.`MAPPING_CREDITAPP_POLICY`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `RMS`.`MAPPING_CREDITAPP_POLICY` (
  `FIELD_ID` INT NOT NULL AUTO_INCREMENT,
  `APPLICATION_ID` INT NOT NULL,
  `POLICY_ID` INT NOT NULL COMMENT '	',
  `POLICY_VALUE` VARCHAR(20) NOT NULL,
  PRIMARY KEY (`FIELD_ID`),
  INDEX `APPLICATION_ID_idx` (`APPLICATION_ID` ASC),
  INDEX `POLICY_ID_idx` (`POLICY_ID` ASC),
  CONSTRAINT `APPLICATION_ID`
    FOREIGN KEY (`APPLICATION_ID`)
    REFERENCES `RMS`.`CREDIT_APPLICATION` (`APPLICATION_ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `POLICY_ID`
    FOREIGN KEY (`POLICY_ID`)
    REFERENCES `RMS`.`POLICY` (`POLICY_ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `RMS`.`MAPPING_POLICY_BORROWER`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `RMS`.`MAPPING_POLICY_BORROWER` (
  `POLICY_ID` INT NOT NULL,
  `POLICY_VALUE` VARCHAR(20) NOT NULL,
  `BORROWER_ID` INT NOT NULL,
  INDEX `BORROWER_ID_idx` (`BORROWER_ID` ASC),
  INDEX `POLICY_ID_idx` (`POLICY_ID` ASC),
  
    FOREIGN KEY (`POLICY_ID`)
    REFERENCES `RMS`.`POLICY` (`POLICY_ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,

    FOREIGN KEY (`BORROWER_ID`)
    REFERENCES `RMS`.`USER` (`USER_ID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;






use RMS;
insert into USER values(1101,'Deepak','deepak@gmail.com','deepak@123','ROLE_BORROWER',1,'2019-7-5','2019-7-5');
insert into USER values(2101,'Ram','ram@gmail.com','ram@123','ROLE_LENDER',1,'2019-7-5','2019-7-5');
insert into USER values(2102,'Aman','aman@gmail.com','aman@123','ROLE_LENDER',1,'2019-7-5','2019-7-5');
insert into LENDER values(2101,6 ,'Get loan at very less interest','5 yr- 10 yr','5 cr - 10 cr');

insert into POLICY values(4101,'Bankrupt',2101,'2019-7-5','2019-7-5');
insert into POLICY values(4102,'Total Assets',0,'2019-7-5','2019-7-5');

insert into MAPPING_LENDER_POLICY values(2101,4101,'NO', 3,1);

insert into CREDIT_APPLICATION values(5101,90,1, 'Impetus',1101,0,2101,'2019-7-5','2019-7-5');
insert into MAPPING_CREDITAPP_POLICY values(6101,5101,4101, 'NO');
insert into USER values(7101,'Amisha','amisha@gmail.com','amisha@123','ROLE_FINANCIAL_ANALYST',1,'2019-7-5','2019-7-5');
insert into FINANCIAL_ANALYST values(7101,2101);
insert into MAPPING_POLICY_BORROWER values(4101,'Bankrupt',1101);
insert into LENDER values(2102,12 ,'Best loan provider company','10 yr- 15 yr','12 cr - 20 cr');
insert into USER values(2103,'Neeraj','neeraj@gmail.com','neeraj@123','ROLE_LENDER',1,'2019-7-5','2019-7-5');
insert into POLICY values(4103,'Turnover',0,'2019-7-5','2019-7-5');
insert into POLICY values(4104,'Age Limit',0,'2019-7-5','2019-7-5');
insert into POLICY values(4105,'Family Income',2101,'2019-7-5','2019-7-5');
insert into POLICY values(4106,'Car',2102,'2019-7-5','2019-7-5');






insert into USER values(7101,'Amisha','amisha@gmail.com','$2a$10$p0uKwMTqEmD2.VXpvuHuVe3c3Q/u4soM9uD7mkT65VR3rPfqFBjbm','ROLE_FINANCIAL_ANALYST',1,'2019-7-5','2019-7-5');

insert into FINANCIAL_ANALYST values(7101,2101);

insert into POLICY values(4101,'Bankrupt',2101,'2019-7-5','2019-7-5');
insert into POLICY values(4102,'Total Assets',0,'2019-7-5','2019-7-5');


                     
APPLICATION_STATUS        
Range - 0-30         Reject    1
        31 - 60      Hold      2
        61 - 100     Approve   3  
         


insert into CREDIT_APPLICATION values(5101,50,2, 'Impetus',1101, 7101 , 2101 ,'2019-7-5','2019-7-5');
insert into MAPPING_CREDITAPP_POLICY values(6101,5101,4101, 'NO');
insert into MAPPING_CREDITAPP_POLICY values(6102,5101,4102, '20');
insert into MAPPING_CREDITAPP_POLICY values(6103,5101,4103, '1000000');

insert into POLICY values(4101,'Bankrupt',2101,'2019-7-5','2019-7-5');
insert into POLICY values(4102,'Total Assets',0,'2019-7-5','2019-7-5');
insert into POLICY values(4103,'Turnover',0,'2019-7-5','2019-7-5');


insert into POLICY values(4105,'Family Income',2101,'2019-7-5','2019-7-5');
insert into POLICY values(4106,'Car',2102,'2019-7-5','2019-7-5');




insert into USER values(1102,'Pawan Nerkar','pawan@gmail.com','$2a$10$XQ6n0BRsSTyiLehSteUxmu8Cf1/yW9bnBglB2QnOBivP74U21sfVO','ROLE_BORROWER',1,'2019-7-6','2019-7-6');


insert into CREDIT_APPLICATION values(5102,60,2, 'Innoeye',1102, 7101 , 2101 ,'2019-7-6','2019-7-6');
insert into MAPPING_CREDITAPP_POLICY values(6104,5102,4101, 'NO');
insert into MAPPING_CREDITAPP_POLICY values(6105,5102,4102, '25');
insert into MAPPING_CREDITAPP_POLICY values(6106,5102,4103, '1000050');


