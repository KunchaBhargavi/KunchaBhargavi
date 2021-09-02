---Creating Tables for movie database:

---Create Table Actor

(
ACT_ID INT(10) PRIMARY KEY,
ACT_NAME VARCHAR(20),
ACT_GENDER CHAR(1)
);

---Create Table Director

(
DIR_ID INT(10) PRIMARY KEY,
DIR_NAME VARCHAR(20),
);

---Create Table Movies

(
MOV_ID INT(10) PRIMARY KEY,
MOV_TITLE VARCHAR(25),
MOV_YEAR INT(4),
MOV_LANG VARCHAR(10),
DIR_ID INT(10),
FOREIGN KEY(DIR_ID) REFERENCES DIRECTOR(DIR_ID)
);

-----Create table Movie_cast
(
ACT_ID INT(10),
MOV_ID INT(10),
MROLE VARCHAR(10),
PRIMARY KEY (ACT_ID,MOV_ID),
FOREIGN KEY(ACT_ID) REFERENCES ACTOR (ACT_ID),
FOREIGN KEY(MOV_ID) REFERENCES MOVIES(MOV_ID)
);

------Inserting Data into tables
Insert into actor values(1,”Akshay Kumar”,”M”)
Insert into actor values(2,”Sonakshi Sinha”,”F”)

Insert into director values (1,”Yash Raj”)
Insert into director values(2,”Pawan Vadeyar”)

Insert into movies values(1,”Mission Mangal”,2019,”Hindu”,5)
Insert into movies values(1,”Hum Dil de chuke sanam”,1994,”Hindu”,5)

Insert into movie_cast values(1,1,”Hero”)
Insert into movie_cast values(2,1,”Heroine”)

-------SELECT STATEMENTS:

----Find the movies names where one or more actors acted in two or more movies.

SELECT DISTINCT M.MOV_TITLE
FROM MOVIES M, MOVIE_CAST MV
WHERE M.MOV_ID=MV.MOV_ID
AND ACT_ID IN(SELECT ACT_ID FROM MOVIE_CAST GROUP BY ACT_ID HAVING COUNT(MOV_ID)>1);

---First lets find who acted before 2000.

SELECT A.ACT_NAME
FROM (ACTOR A JOIN MOVIE_CAST C ON A.ACT_ID=C.ACT_ID) JOIN MOVIES M ON C.MOV_ID=M.MOV_ID
WHERE M.MOV_YEAR<2000;

---Now lets find out before 2000  movie and after 2017 using JOIN. 

SELECT A.ACT_NAME
FROM (ACTOR A JOIN MOVIE_CAST C ON A.ACT_ID) JOIN MOVIES M ON C.MOV_ID=M.MOV_ID
WHERE M.MOV_YEAR>2017;

---List all actors who acted in a movie before 2000 and also in a movie after 2017 using JOIN.

SELECT A.ACT_NAME
FROM (ACTOR A JOIN MOVIE_CAST C ON A.ACT_ID) JOIN MOVIES M ON C.MOV_ID=M.MOV_ID
AND EXISTS(SELECT A.ACT_NAME
FROM (ACTOR A JOIN MOVIE_CAST C ON A.ACT_ID=C.ACT_ID) JOIN MOVIES M ON C.MOV_ID=M.MOV_ID WHERE M.MOV_YEAR>2017);





