# SqlServerJunk

```sql
CREATE SEQUENCE NextCustomNumber AS BIGINT 
    START WITH 1 
    INCREMENT BY 1 
    NO MAX VALUE 
    NO CACHE;

CREATE TABLE APP_TYPE (
    version int IDENTITY(1,1) PRIMARY KEY,
    perc int,
);

INSERT INTO APP_TYPE (perc) VALUES (50);

CREATE FUNCTION dbo.nextAppNum(@Version int, @Rand float, @Id bigint)
RETURNS bigint
AS
BEGIN
    DECLARE @perc int;
    DECLARE @output bigint;
    SELECT @perc = at.perc
    FROM APP_TYPE at
    WHERE at.version = @Version;
     IF @perc > @Rand * 100
     BEGIN
        SET @output = FORMAT(@Id, '9#########');
     END
     ELSE
     BEGIN
     	SET @output = FORMAT(@Id, '1#########');
     END
    RETURN @output;
END;

CREATE TABLE dbo.APPLICATIONS (
    id bigint DEFAULT dbo.nextAppNum(1, RAND(), NEXT VALUE FOR NextCustomNumber),
    junk varchar(255)
);

INSERT INTO APPLICATIONS (junk) VALUES ('a junk');
INSERT INTO APPLICATIONS (junk) VALUES ('b junk');
INSERT INTO APPLICATIONS (junk) VALUES ('c junk');
INSERT INTO APPLICATIONS (junk) VALUES ('d junk');
INSERT INTO APPLICATIONS (junk) VALUES ('e junk');

SELECT * FROM APPLICATIONS;
```