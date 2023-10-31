# Commandes Pour SqlServer

---


1. ## Récupérer toutes les tables de la base:
```SQL
SELECT name FROM sysobjects WHERE xtype='U'
```

2. ## Créer une Table si elle n'existe pas:
```SQL
IF object_id('my_sql') is null
    CREATE TABLE my_sql(id1 INTEGER, id2 TEXT, id3 INTEGER, id4 TEXT, id5 INTEGER, id6 TEXT, id7 INTEGER, id8 TEXT, id9 INTEGER, id10 TEXT, id11 INTEGER, id12 TEXT, id13 INTEGER, id14 TEXT);
ELSE
    PRINT 'Existedeja'

# En une seule ligne
IF object_id('my_sql') is null CREATE TABLE my_sql(id1 INTEGER, id2 TEXT, id3 INTEGER, id4 TEXT, id5 INTEGER, id6 TEXT, id7 INTEGER, id8 TEXT, id9 INTEGER, id10 TEXT, id11 INTEGER, id12 TEXT, id13 INTEGER, id14 TEXT); ELSE PRINT 'Existedeja';
```

3. ## Insérer des Valeurs dans la Table de la base:
```SQL
INSERT INTO my_sql VALUES (1, "1", 2, "2", 3, "3", 4, "4", 5, "6", 7, "7", 8, "8", 9, "9", 10, "10", 11, "11", 12, "12", 13, "13", 14, "14")
```

4. ## Retourner Des Valeurs d'une Table de la base:
```SQL
SELECT * FROM my_sql

# Plus Généralement
# SELECT <champ(s)> FROM <nom_table> WHERE <condition>
```


---

<br><br>

# Mise En Application avec PowerShell: (Pour l'instant)



---

1. ## Se connecter a la base SqlServer en PowerShell et énumérer les Tables de la base 'master' (par défaut): 
```PowerShell
# PowerShell | Copy-Paste via ClipBord
$con = [System.Data.SqlClient.SqlConnection]::new("Server=localhost;Database=master;Trusted_Connection=True;")
$con.Open()
$cmd = $con.CreateCommand()
$cmd.CommandText = "SELECT name FROM sysobjects WHERE xtype='U'"
$rd = $cmd.ExecuteReader() ; while($rd.Read()) { $rd.GetValue(0) }
$rd.Close()
$con.Close()
```


2. ## Créer des Tables si elle n'y sont pas: 
```PowerShell
# PowerShell | Copy-Paste via ClipBord
$con = [System.Data.SqlClient.SqlConnection]::new("Server=localhost;Database=master;Trusted_Connection=True;")
$con.Open()
$tr = $con.BeginTransaction()
$cmd = $con.CreateCommand()
$nom_de_la_table  = "my_sql" # éditer
$cmd.CommandText = "IF object_id('$nom_de_la_table') is null CREATE TABLE $nom_de_la_table(id1 INTEGER, id2 TEXT, id3 INTEGER, id4 TEXT, id5 INTEGER, id6 TEXT, id7 INTEGER, id8 TEXT, id9 INTEGER, id10 TEXT, id11 INTEGER, id12 TEXT, id13 INTEGER, id14 TEXT); ELSE PRINT 'Existedeja';"
$cmd.Transaction = $tr
$cmd.ExecuteNonQuery()
$tr.Commit()
$con.Close()

```


3. ## Insérer des Valeurs dans la base en PowerShell:
```PowerShell
# PowerShell | Copy-Paste via ClipBord
$obj = @(100, "Exemple de Valeur que vous Pouvez modifier.100", 200, "Exemple de Valeur que vous Pouvez modifier.200", 300, "Exemple de Valeur que vous Pouvez modifier.300", 400, "Exemple de Valeur que vous Pouvez modifier.400", 500, "Exemple de Valeur que vous Pouvez modifier.500", 600, "Exemple de Valeur que vous Pouvez modifier.600", 700, "Exemple de Valeur que vous Pouvez modifier.700")
$con = [System.Data.SqlClient.SqlConnection]::new("Server=localhost;Database=master;Trusted_Connection=True;")
$con.Open()
$tr = $con.BeginTransaction()
$cmd = $con.CreateCommand()
$cmd.CommandText = "INSERT INTO my_sql VALUES (@id1, @id2, @id3, @id4, @id5, @id6, @id7, @id8, @id9, @id10, @id11, @id12, @id13, @id14)"  # éditer la requête en Fonction du besoin.
$cmd.Parameters.AddWithValue("@id1", $obj[0]).ParameterName
$cmd.Parameters.AddWithValue("@id2", $obj[1]).ParameterName
$cmd.Parameters.AddWithValue("@id3", $obj[2]).ParameterName
$cmd.Parameters.AddWithValue("@id4", $obj[3]).ParameterName
$cmd.Parameters.AddWithValue("@id5", $obj[4]).ParameterName
$cmd.Parameters.AddWithValue("@id6", $obj[5]).ParameterName
$cmd.Parameters.AddWithValue("@id7", $obj[6]).ParameterName
$cmd.Parameters.AddWithValue("@id8", $obj[7]).ParameterName
$cmd.Parameters.AddWithValue("@id9", $obj[8]).ParameterName
$cmd.Parameters.AddWithValue("@id10", $obj[9]).ParameterName
$cmd.Parameters.AddWithValue("@id11", $obj[10]).ParameterName
$cmd.Parameters.AddWithValue("@id12", $obj[11]).ParameterName
$cmd.Parameters.AddWithValue("@id13", $obj[12]).ParameterName
$cmd.Parameters.AddWithValue("@id14", $obj[13]).ParameterName
$cmd.Transaction = $tr
$cmd.ExecuteNonQuery()
$tr.Commit()
$con.Close()
```

4. ## Retourner les Valeurs enregistrées précedement dans la Table:
```PowerShell
# PowerShell | Copy-Paste via ClipBord
$con = [System.Data.SqlClient.SqlConnection]::new("Server=localhost;Database=master;Trusted_Connection=True;")
$con.Open()
$cmd = $con.CreateCommand()
$cmd.CommandText = "SELECT * FROM my_sql WHERE 1=1" # éditer la requête en Fonction du besoin.
$rd = $cmd.ExecuteReader() ; while($rd.Read()) { for($i = 0; $i -lt $rd.FieldCount; $i++) { $rd.GetValue($i) } }
$rd.Close()
$con.Close()
```
