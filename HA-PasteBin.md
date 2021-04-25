# Ensuring High Avalability


1. Try to prioritize your DB. Like primary db. Then other DBs.
2. Multiple clusters of your app
3. Use Load Balancer
4. Use keepalived to up and running your load balancer
5. Use this whole system in different Cloud Providers 
6. Use DNS level proxy. 

So, we need a software that doesn't go out for a single moment. Like you use the Facebook app. You never feel that the app is broken. You feel sometimes that some very few of it's features aren't working. But overall it isn't broken.

So if you in change to design same kind of High Availability (we may call it HA from now), what would you do? What are the extra works you have to do besides making the software's Frontend, Backend and Business Logics?

Well, for that kind of HA, we always need a backup plan. A backup thing. Like our life teaches us. 

Let's think where we can do some additional works for that HA. 
And to let you know, these things are mine concepts.  I had learned them. I am not saying "That is the only way." I am eager to learn different approaches of yours.

## Database
So for that, the first thing I want to do is, I may shard my DataBase on their priority. Like the Microservices, where different data needs to be in touch with their close one, those are the tables may stand on same database. That would be like closely coupled data. I would break them in maximum number of servers possible. And If needed, I would apply Master Slave architecture here.

## Services
After that, we may point our eyes on our application server. Get up baby. You need to be clusterd too. I may cluster each of them at least 3 in number. 

## Load Balancer
I think Load Balancer (LB) is one of the most important part of this process. 

## KeepAlived
