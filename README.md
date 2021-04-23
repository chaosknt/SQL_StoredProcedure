# SQL_StoredProcedure

### Create

```
El mismo stored en vez de Create si ponemos ALTER, lo estariamos modificando, solo se puede crear una vez.

CREATE PROCEDURE SP_Registrarusuario
	/*PARAMETERS*/
	@id int, @nombre VARCHAR(50), @pass VARCHAR(10), @email VARCHAR (20), @idPerfil int, @usuario varchar(20)
	
AS
BEGIN /* Inicio */
	declare @msg varchar(120);
	set NOCOUNT ON;
	Begin Tran Tadd 
	
		BEGIN TRY
			INSERT INTO Usuarios values(@id, @nombre, @pass, @email, @idPerfil, @usuario)
			set @msg = 'El usuario se registro correctamente'
			commit tran tadd
		end try
	
		begin Catch
			set @msg = 'Ocurrio un error:' + ERROR_MESSAGE() + ' en la linea' + CONVERT(NVARCHAR(255), ERROR_LINE() ) + '.' 
			Rollback TRAN Tadd     
		End Catch 
	
	PRINT @msg 
END /* END */ 
	GO

```

# EXECUTE

```
EXECUTE SP_Registrarusuario 
	@id = 111,
	@nombre = 'jose',
	@pass = '421',
	@email = 'josee@jose.com',  
	@idPerfil = 1,
	@usuario = 'soyjose'
```	
