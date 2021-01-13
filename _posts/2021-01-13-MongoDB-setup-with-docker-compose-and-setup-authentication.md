## Reference: https://dev.to/efe136/how-to-enable-mongodb-authentication-with-docker-compose-2nbp

By default there is no authentication in MongoDB. It means that it comes with empty authentication. So we should create users and roles manually. There are lots of ways to create MongoDB docker-compose with authentication. The most popular one of them is to write a bash script with user and roles then use it in docker-compose and another way is to create an init-mongodb.js file with users and roles and use it in docker-compose. But in this post, I will show you how you could create MongoDB docker-compose and then add users and roles manually. Lets start...

Run MongoDB docker-compose without authentication
First of all we should run MongoDB `docker-compose` without auth. Using `docker-compose up -d` we could run it.

![](https://res.cloudinary.com/practicaldev/image/fetch/s--yezN4R8G--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/uqd2xlbe7m5mmzlglebp.png)

Connect MongoDB without auth inside container
For doing that firstly we should run `docker exec -it mongodb bash` and enter inside the container. Then simply run `mongo` command to connect to mongodb.

Create user and roles for our MongoDB
Firstly we will create admin user with username root and password root in admin database.

![](https://res.cloudinary.com/practicaldev/image/fetch/s--M6-Mj7_n--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/yvuxdragqm624yqrwj3n.png)

Then we need to create other user. Here I will create a demo user within demo database for our MongoDB:

![](https://res.cloudinary.com/practicaldev/image/fetch/s--8JH0ACXd--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/ac3bqz221coi4t74x1w3.png)

Now we are ready to run MongoDB with auth.

Enable MongoDB auth in docker-compose
In order to enable auth in MongoDB we will use `--auth` flag in docker-compose. After that we could use `docker-compose up -d` command again, to run MongoDB container.

![](https://res.cloudinary.com/practicaldev/image/fetch/s--FpCDa7qX--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/u8e1j6l02m570wrsm4nw.png)

Connect MongoDB with defined authentication
In this step we can connect our db with defined authentication. Firstly again we need to run docker exec -it mongodb bash command in order to enter inside the container. Now we are in so we can connect our db. Here I write 2 command one of them is to connect admin db and another one is to connect demo db.

`mongo -u root -p root --authenticationDatabase admin`

`mongo -u demo -p demo12345 --authenticationDatabase demo`
