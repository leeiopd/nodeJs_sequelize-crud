# sequelize crud

- Sequelize
  - Sequelize : ORM(Object-Relational Mapping)
  - Nodejs 로 mySql또는 postgreSql (db) 를 제어



- dotenv
  - 환경변수 설정들을 저장하고 관리
  - dotenv pakage 설치 후, process.env로 접근 가능



- mysql 설정 / Database 생성

  ```mysql
  # mysql -u root -p : root 권한으로 mysql 실행
  
  # database 생성
  create database exercise;
  
  # 설정추가
  ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '_루트권한_password'
  
  # 테이블 확인
  show databases
  ```



* Database 접속

  ```shell
  # sequelize 설치 (4.42.0 ver)
  npm intall --save sequelize@4.42.0
  
  # mysql2 설치
  npm install --save mysql2
  ```

  * .env

    ```
    DATABASE = "데이터베이스명"
    DB_USER = "아이디"
    DB_PASSWORD = "패스워드"
    DB_HOST = "DB호스트"
    ```

  * models/index.js

    * .env 의 내용으로 sequelize를 이용하여 mysql Database 로 접근
      * db 접속
      * models 폴더 안의 index.js 파일을 제외한 모든 파일을 참조하여 table을 생성
      * table 관계 설정

  * app.js

    * 서버 실행시 동작

    ```javascript
    dbConnection(){
            // DB authentication
            db.sequelize.authenticate()
            .then(() => {
                console.log('Connection has been established successfully.');
            })
            .then(() => {
                console.log('DB Sync complete.');
            })
            .catch(err => {
                console.error('Unable to connect to the database:', err);
            });
        }
    ```

    

* DB write/read

  * Read

    * /controllers/admin/admin.ctrl.js

      ```javascript
      exports.post_products_write = ( req , res ) => {
          // res.send(req.body);  
        models.Products.create(req.body).then(()=>{
              res.redirect("/admin/products")
          })
      }
      ```

  * Write

    * /controllers/admin/admin.ctrl.js

      ```javascript
      
      exports.get_products = ( _ , res) => {
        models.Products.findAll({}).then(products=>{
              res.render('admin/products.html', {products:products})
          })
      }
      ```

    * template/admin/products.html

      ```html
       {% for product in products %}
              <tr>
                  <td>{{ product.name }}</td>
                  <td>
                      {{ product.createdAt }}
                  </td>
              </tr>
      {% endfor %}
      ```

      