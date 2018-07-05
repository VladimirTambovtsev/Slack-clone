# Slack-clone

Slack messenger clone built in React and Apollo GraphQl 

# Development

## Server
```cd server```  
<br>Change Your PostgreSQL local credentials in <code>src/models/index.js</code>
```
const sequelize = new Sequelize(process.env.TEST_DB || 'db_name', 'psql_username', 'psql_password', {
  dialect: 'postgres',
  operatorsAliases: Sequelize.Op,
  host: process.env.DB_HOST || 'localhost',
  define: {
    underscored: true,
  },
});
```

Install packages, start on ```localhost:8081```:   
```yarn start```

## Client
```cd client```  
```yarn```  
```yarn start```

# Production
## Server
```cd server```  
<br>Set Your credentials in <code>docker-compose.yml</code>
```
db:
  environment:
        POSTGRES_PASSWORD: `psql_password`
        POSTGRES_USER: `psql_user`
        POSTGRES_DB: `psql_db`
```
and Server adress
```
 environment:
      DB_HOST: db
      REDIS_HOST: redis
      SERVER_URL: `server_url`
```
Build packages:  
```npm run build```  

Build docker container:  
```docker build -t 'your_username/repo' . ```  

Run:  
```docker-compose up```

## Client
You can run client separately if you want  
Set ```process.env.REACT_APP_SERVER_URL``` in ```src/apollo.js```
```
const httpLink = createFileLink({ uri: `http://${process.env.REACT_APP_SERVER_URL || 'localhost:8081'}/graphql` });
```
And for subscriptions in the same file
 ```
 export const wsLink = new WebSocketLink({
  uri: `ws://${process.env.REACT_APP_SERVER_URL || 'localhost:8081'}/subscriptions`,
  options: {
    reconnect: true,
    lazy: true,
    connectionParams: () => ({
      token: localStorage.getItem('token'),
      refreshToken: localStorage.getItem('refreshToken'),
    }),
  },
});
 ```
Build packages:  
```npm run build```  

Push to any storage
