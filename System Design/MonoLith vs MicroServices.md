Monolith 
- All code is placed in one place Auth profiles and payments
- Duplication is lesser
- Best for smaller teams
- Contracts are in one place like if a function expects a string but a int is passed then it will give an error immediately


MicroServices 
- Codebase is split into different services such as Auth Profile and Payments 
- Engineering in microservices is easier as each microservice is independent 
- Deployments are easier as if one goes under deployment others will work fine


How to Migrate from Monolith to MicroService?

- One Approach is to build a separate MicroService and then redirect 10% userbase to this MicroService but it is a massive challenge for engineers as configuring whole DB, endpoints and all is a major task   
