## SP with Cursor

borra todas las Notificaciones y Pagos para todas las ordenes que sean menor o igual a una fecha pasada por parametro

```
CREATE  PROCEDURE SP_DeleteOldOrdersByDate @Date datetime2(7)

AS
	BEGIN

		DECLARE @id AS nvarchar(max)
		DECLARE @msg as nvarchar(max)
		DECLARE db_cursor CURSOR FOR 

		select Id  from Orders where LastUpdatedDate >= @Date

			BEGIN TRY

				OPEN db_cursor 

					FETCH NEXT from db_cursor INTO @id 

					WHILE @@FETCH_STATUS = 0 

					BEGIN 
						delete from Notifications where OrderId = @id
						delete from Payments where OrderId = @id
						FETCH NEXT from db_cursor INTO @id
					END

				CLOSE db_cursor --
					delete from Orders where  LastUpdatedDate >= @Date
				set @msg = 'SP_DeleteOldOrdersByDate executed successfully' 

			end try

			begin Catch
				set @msg = 'Exception:' + ERROR_MESSAGE() + ' on line' + CONVERT(NVARCHAR(255), ERROR_LINE() ) + '.' 
				Rollback TRAN Tadd     
			End Catch
	
		DEALLOCATE db_cursor 

		PRINT @msg 

	END
  
 ``` 
  
  
  how to use:
  
  ```
  
  EXECUTE SP_DeleteOldOrdersByDate @date = '2021-09-23'
  
```
