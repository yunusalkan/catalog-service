./gradlew bootBuildImage -> Docker image'ı oluşturur. 

docker images catalog-service:0.0.1-SNAPSHOT -> catalog-service isimli imajın bilgilerini getirir.

minikube start -> minikube ile kubernates cluster'ı başlatılır

minikube image load catalog-service:0.0.1-SNAPSHOT -> default olarak minikube 
Docker Hub registry server'ı kullanır imajları pull etmek için kullanır ve local repoya erişimi yoktur.
Bu komutla manuel olarak biz import etmiş oluruz.

kubectl create deployment catalog-service --image=catalog-service:0.0.1-SNAPSHOT -> 
catalog-service adında imajı catalog-service:0.0.1-SNAPSHOT olan bir deployment yaratıyoruz.

kubectl get deployment -> Az önce yarattığımız deployment'ı listeler

kubectl logs deployment/catalog-service -> Bu komut ile oluşturduğumuz deployment'a 
ait application loglarını görebiliriz.

kubectl expose deployment catalog-service --name=catalog-service --port=8080 -> Default olarak kubernetes
üzerindeki uygulamalara dışarıdan erişime izin verilmez. Manuel olarak expose etmemiz gerekir. Bu komut
da catalog-service isimli deployment'ımızı 8080 port üzerinden catalog-service servis adıyla 
cluster içinde diğer componentlere erişime açmamıza izin veriyor.

kubectl port-forward service/catalog-service 8000:8080 -> service dışarıdan erişebilmek için bir 
de port forwarding yapılması gerekiyor. Bu komutla cluster içindeki 8080 portuna catalog-service için
localde 8000 portundan erişilebilmeyi sağlar.

//******* PSQL Tips *******//
docker run -d \
--name polar-postgres \
-e POSTGRES_USER=user \
-e POSTGRES_PASSWORD=password \
-e POSTGRES_DB=polardb_catalog \
-p 5432:5432 \
postgres:14.4

**Docker Command 	Description**
docker stop polar-postgres 	    Stop container.
docker start polar-postgres 	Start container.
docker remove polar-postgres 	Remove container.

**Start an interactive PSQL console:**
docker exec -it polar-postgres psql -U user -d polardb_catalog

**PSQL Command 	Description**
\list 	List all databases.
\connect polardb_catalog 	Connect to specific database.
\dt 	List all tables.
\d book 	Show the book table schema.
\quit 	Quit interactive psql console.

**To Remove postgres container**
docker rm -fv polar-postgres

**Sample Book Post Request**
curl --location 'localhost:9001/books' \
--header 'Content-Type: application/json' \
--data ' {
"isbn"  : "1234567893",
"title" : "Kürk mantolu madonna",
"author" : "Abidin Dino",
"price" : 25.95
}'