# Create the necessary triggers to implement database integrity
* using this frotasdb in here : https://github.com/JoseMigCamara/Triggers_SQL/blob/main/frotasdb
### If you have triggers, drop them with this
```
drop trigger  if exists viaturas_before_insert;
drop trigger  if exists frotas_before_insert;
drop trigger  if exists marcas_before_insert;
drop trigger  if exists combustivel_before_insert;
```
## Viaturas triggers
* VIATURA BEFORE INSERT
```
delimiter //
create trigger viaturas_before_insert
before insert on viaturas
for each row
begin
	DECLARE mensagem VARCHAR(255);
    DECLARE _idviaturas VARCHAR(255);
    DECLARE registos VARCHAR(255);
```    
## Check if the viaturas id exists in the viaturas
```
	if (select count(*) from viaturas where idviaturas = new.idviaturas) > 0 then
		set _idviaturas = (select idviaturas from viaturas where idviaturas = new.idviaturas);
		set mensagem = concat('O id da viatura ',new.idviaturas,' - ', _idviaturas,' já existe.');
		signal sqlstate '45000' set message_text = mensagem;
	end if;
```
## Check if the idCombustible of the viatura exists in the combustivel   
```
    if (select count(*) from combustivel where idcombustivel = new.idcombustivel) = 0 then
        set mensagem = concat('O id de combustivel',new.idcombustivel,' não existe na tabela combustivel.');
        signal sqlstate '45000' set message_text = mensagem;
    end if;
```
## Check if the viatura idmarcas exists in the marca 
```
    if (select count(*) from marcas where idmarcas = new.idmarcas) = 0 then
        set mensagem = concat('O id de marcas',new.idmarcas,' não existe na tabela marcas.');
        signal sqlstate '45000' set message_text = mensagem;
    end if;
end;
//
```
## Frotas triggers
* BEFORE INSERT FROTAS
```
delimiter //
create trigger frotas_before_insert
before insert on frotas
for each row
begin
	DECLARE mensagem VARCHAR(255);
    DECLARE _frota VARCHAR(255);
    DECLARE registos VARCHAR(255);
    if (select new.idfrotas, new.idviaturas) in (select idfrotas, idviaturas from frotas) then
		set mensagem = concat('Este registo já existe para esta frota. Criaria um registo duplicado para a mesma frota.');
		signal sqlstate '45000' set message_text = mensagem;
	end if;
```
## Check if the viaturas id exists in the viaturas
```
  if (select count(*) from viaturas where idviaturas = new.idviaturas) = 0 then
        set mensagem = concat('O id da frota ',new.idviaturas,' não existe na tabela viaturas.');
        signal sqlstate '45000' set message_text = mensagem;
    end if;
end;
//
```
## Marcas triggers
* BEFORE INSERT MARCAS
```
delimiter //
create trigger marcas_before_insert
before insert on marcas
for each row
begin
	DECLARE mensagem VARCHAR(255);
    DECLARE _marcas VARCHAR(255);
    DECLARE registos VARCHAR(255);
```
## Check if the marcas id exists in the marcas
```
	if (select count(*) from marcas where idmarcas = new.idmarcas) > 0 then
		set _marcas = (select idmarcas from marcas where idmarcas = new.idmarcas);
		set mensagem = concat('O id da marca ',new.idmarcas,' - ', _marcas,' já existe.');
		signal sqlstate '45000' set message_text = mensagem;
	end if;
end;
//
```
## Combustivel triggers
* BEFORE INSERT COMBUSTIVEL
```
delimiter //
create trigger combustivel_before_insert
before insert on combustivel
for each row
begin
	DECLARE mensagem VARCHAR(255);
    DECLARE _combustivel VARCHAR(255);
    DECLARE registos VARCHAR(255);
```
## Check if the combustivel id exists in the combustivel
```
	if (select count(*) from combustivel where idcombustivel = new.idcombustivel) > 0 then
		set _combustivel = (select idcombustivel from combustivel where idcombustivel = new.idcombustivel);
		set mensagem = concat('O id do combustivel ',new.idcombustivel,' - ', _combustivel,' já existe.');
		signal sqlstate '45000' set message_text = mensagem;
	end if;
end;
//
```
