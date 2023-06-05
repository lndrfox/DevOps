# TP PART 01

## Database

### Adminer container

First run the following command to launch the adminer container

`   docker run \
    -p "8090:8080" \
    --net=app-network \
    --name=adminer \
    -d --rm\
    adminer
    `


### PSQL container

In TP-Part01/Database/psql build the docker image using

`docker build --rm -t database .`

Then run it 

`docker run --name psqldb -v psql-vol:/var/lib/postgresql/data -d -e POSTGRES_PASSWORD="pwd" --rm -p 5432:8888 --net=app-network database`


### Access Adminer

Acces adminer at `http://localhost:8090/`
