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

  * Read-List

    * /controllers/admin/admin.ctrl.js

      ```javascript
      exports.post_products_write = ( req , res ) => {
          // res.send(req.body);  
        models.Products.create(req.body).then(()=>{
              res.redirect("/admin/products")
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


  * Write

    * /controllers/admin/admin.ctrl.js

      ```javascript
      
      exports.get_products = ( _ , res) => {
        models.Products.findAll({}).then(products=>{
              res.render('admin/products.html', {products:products})
          })
      }
      ```

  * Read-Detail

    * /controllers/admin/admin.ctrl.js

      ```javascript
      exports.get_products_detail = ( req , res) => {
          models.Products.findByPk(req.params.id).then((product)=>{
               res.render( 'admin/detail.html', {product});
          })
      }
      ```

    * /template/admin/detail.html

      ```html
      <div class="panel panel-default">
              <div class="panel-heading">
                  {{ product.name }}
              </div>
              <div class="panel-body">
                  <div style="padding-bottom: 10px">
                      작성일 : {{ product.dateFormat( product.createdAt ) }}
                  </div>
      
                  {{ product.description }}
      
              </div>
      </div>
      ```

* Update


  * /controllers/admin/admin.ctrl.js

    ```javascript
    exports.get_products_edit = ( req , res) => {
        models.Products.findByPk(req.params.id).then((product)=>{
             res.render( 'admin/write.html', {product});
        })
    }
    
    exports.post_products_edit = ( req , res) => {
        models.Products.update({
            name:req.body.name,
            price:req.body.price,
            description:req.body.description,
        },
        {
            where:{
                id:req.params.id
        }}).then(()=>{
            res.redirect('/admin/products/detail/'+req.params.id)
        })
    }
    ```

  * /template/admin/detail.html

    ```html
    <form action="" method="post">
        <table class="table table-bordered">
            <tr>
                <th>제품명</th>
                <td><input type="text" name="name" class="form-control" value="{{product.name}}"/></td>
            </tr>
            <tr>
                <th>가격</th>
                <td><input type="text" name="price" class="form-control" value="{{product.price}}"/></td>
            </tr>
            <tr>
                <th>설명</th>
                <td><input type="text" name="description" class="form-control" value="{{product.price}}"/></td>
                </tr>
        </table>
        <button class="btn btn-primary">작성하기</button>
    </form>
    ```

    * value 값을 이용하여 write 페이지 재 사용

* Delete

  * /controllers/admin/admin.ctrl.js

    ```javascript
    exports.get_products_delete = ( req , res) => {
        models.Products.destroy({where:{id:req.params.id}}).then(()=>{
             res.redirect( '/admin/products/');
        })
    }
    ```

* CRUD 정리

  ```
  insert -> models.테이블명.create(데이터)
  
  select -> models.테이블명.findAll(조회조건)
            models.테이블명.findByPk(primary key)
            models.테이블명.findOne(조회조건)
            
  update -> models.테이블명.update(데이터, 조회조건)
  
  delete -> models.테이블명.destroy(조회조건)
  ```

  