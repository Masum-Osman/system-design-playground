# Ensuring High Availability


1. Try to prioritize your DB. Like primary db. Then other DBs.
2. Multiple clusters of your app
3. Use Load Balancer
4. Use keepalived to up and running your load balancer
5. Use this whole system in different Cloud Providers 


So, we need a software that doesn't go out for a single moment. Like you use the Facebook app. You never feel that the app is broken. You feel sometimes that some very few of it's features aren't working. But overall it isn't broken.

So if you are in charge of designing the same kind of High Availability (we may call it HA from now), what would you do? What are the extra works you have to do besides making the software's Frontend, Backend and Business Logics?

Well, for that kind of HA, we always need a backup plan. A backup thing. Like our life teaches us. 

Let's think about where we can do some additional work for that HA. 
And to let you know, these things are my concepts.  I had learned them. I am not saying "That is the only way." I am eager to learn different approaches of yours.

## Database
So for that, the first thing we need to do is, We may partitioned our DataBase on their priority then the service they may provide. 
Try to separete the data base rather than keeping it one big storage. 
And If possible, I would apply Master Slave architecture here so that the GET and POST may work indevidualy and improve overall DB performance.

## Services
After that, we may point our eyes on our application server. Get up baby. You need to be clustered too. Cluster each of them at least 3 in number. 
And the most important part is, **It Needs to be Stateless.** Otherwise there is no point I guess. And the clusters may be on containers, or in different servers. But I prefer to place them on different servers. It makes the system more reliable. 

## Load Balancer
Yes, Balance your load across your services. That is one crucial part of ensuring HA. If one of our services goes down, another is here to serve your needs. We can use any kind of Load Balancing Algorithms here. We don't want our app to be in one single machine and we pray that that machine never goes down. We are going to pass the requests to our service clusters.

## Keepalived
Here is the thing. **keepalived**
We are going to use it on our load balancer. Remember we always keep a backup plan. For DB, we tried partitioning. We clustered our app on different copies. Now we make sure that we have a backup Load Balancer who monitors all of the things.
High Availability is achieved by the Virtual Router Redundancy Protocol (VRRP). VRRP is a fundamental brick for router failover. In addition, Keepalived implements a set of hooks to the VRRP finite state machine providing low-level and high-speed protocol interactions. They're gonna use tricks like Virtual IP Address that makes our Load Balancer work as a single point entry. 

![alt text](https://github.com/Masum-Osman/system-design-playground/blob/master/High%20Availability/HA-Sketch.JPG?raw=true)

## Resilient Infrastructures
If we have done all of our works mentioned earlier, It is high time to think a little bit more. 
Let's think all of our setups is like an app itself. Let it put on SASS providers like AWS, Azure or GCP to make the app work. 
So now, we may think the whole package as a container (not actually, but for this point only) within the keepalived, LBs, services, DBs inside it. And they are working fine. A request comes, LB powred by keepalived handles it and send it to the appropriate cluster of service. They take and do the required works and send the response back to the user. 
+ If a service/cluster goes down, our system will not melt down because there are other more clusters providing the same type of job. 
+ If there is a problem with the LB itself, keepalive will invoke itself to up and active the other LB with same IP adsress through VRRP. And thus the whole system is up again.

Now, we can be more negetive on here. What if the SASS goes down? like the AWS or Azure itself? What should we do to catch this exception? 

For that kind of disester, we can replicate our whole system/infastructure on different cloud providers. Or even same provider but different region. Like a copy in the Azure while AWS is our primary. And a copy in the Singapore region another on Osaka.