# exemplo-sqliteplugin
Exemplo usando sqlite plugin. O plugin está disponível em: https://github.com/litehelpers/Cordova-sqlite-storage

Aí está um exemplo pronto, basta baixar e executar. Importante: O plugin só funciona gerando o apk, não funciona rodando no phonegap, demorei descobrir isso. :)

Apesar do projeto estar pronto para execular, vou listar os passos necessários para criar a aplicação e gerar o apk de exemplo.

1. Instalar o phonegap via npm
npm install -g phonegap

2. Instalar o apache ant
no fedora: yum install ant

3. Criar o projeto phonegap
phonegap create testeplugin

4. Adicionar a plataforma
phonegap platform add android

5. Adicionar o plugin
phonegap plugin add https://github.com/litehelpers/Cordova-sqlite-storage

6. Na página index.html, dentro da pasta www do seu projeto, chamar o arquivo de script do plugin
<script type="text/javascript" src="SQLitePlugin.js"></script>

7. Acrescentar ao "js/index.js", arquivo presente na pasta www do projeto, as seguintes linhas para debugar e visualizar algumas informações dos registros que estão sendo inseridos. Esse exemplo eu peguei da própria página do plugin.
        db.transaction(function(tx) {
            //tx.executeSql('DROP TABLE IF EXISTS test_table');
            tx.executeSql('CREATE TABLE IF NOT EXISTS test_table (id integer primary key, data text, data_num integer)');

            // demonstrate PRAGMA:
            db.executeSql("pragma table_info (test_table);", [], function(res) {
            // console.log("PRAGMA res: " + JSON.stringify(res));
                alert("PRAGMA res: " + JSON.stringify(res));
            });

            tx.executeSql("INSERT INTO test_table (data, data_num) VALUES (?,?)", ["test", 100], function(tx, res) {
                // console.log("insertId: " + res.insertId + " -- probably 1");
                // console.log("rowsAffected: " + res.rowsAffected + " -- should be 1");
                alert("insertId: " + res.insertId + " -- probably 1");
                alert("rowsAffected: " + res.rowsAffected + " -- should be 1");

                db.transaction(function(tx) {
                    tx.executeSql("select count(id) as cnt from test_table;", [], function(tx, res) {
                        // console.log("res.rows.length: " + res.rows.length + " -- should be 1");
                        // console.log("res.rows.item(0).cnt: " + res.rows.item(0).cnt + " -- should be 1");
                        alert("res.rows.length: " + res.rows.length + " -- should be 1");
                        alert("res.rows.item(0).cnt: " + res.rows.item(0).cnt + " -- should be 1");
                    });
                });
            }, function(e) {
                alert("ERROR: " + e.message);
            });
        });

8. Por último gerar o apk e em seguida instalar
phonegap build android

