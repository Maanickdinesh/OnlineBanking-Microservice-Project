[Account Service ]

*start config--8888
*start eureka--8761
*start docker
*open cmd and paste-- ( docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.12-management )
*start rabbitmq-- localhost:5672
*open another cmd and paste-- ( docker run -d -p 9411:9411 openzipkin/zipkin )

*start bank Account(using prod)--
*start bank Account feign--8995 port

*make sure(eureka,config,docker,rabbitmq,bank account,bank account feign running)

postman:
1.method:post
url: http://localhost:8995/account-requests/create
body:
{
  "accountNumber": 123456789,
  "balance": 1000.0,
  "active": false,
  "customerIdNumber": "ABCD123456"
}

output: 
{
    "id": 1,
    "accountNumber": 10000002,
    "balance": 1000.0,
    "active": true,
    "customerIdNumber": "ABCD123456"
}

2.method:put
url: http://localhost:8995/account-requests/disable/1
body: none
output:Account disabled

3.method:put
url: http://localhost:8995/account-requests/enable/1
body:none
output:Account enabled


4.method:get
url: http://localhost:8995/account-requests/1
body:none
output: 
{
    "id": 1,
    "accountNumber": 10000001,
    "balance": 1000.0,
    "active": true,
    "customerIdNumber": "ABCD123456"
}

5.method:get
url: http://localhost:8995/account-requests/active
body: none
output:
[
    {
        "id": 1,
        "accountNumber": 10000001,
        "balance": 1000.0,
        "active": true,
        "customerIdNumber": "ABCD123456"
    },
    {
        "id": 2,
        "accountNumber": 10000002,
        "balance": 1000.0,
        "active": true,
        "customerIdNumber": "ABCD123456"
    }
]

6.method:get
url: http://localhost:8995/account-requests/delete/1\
body: none
output:Account deleted

now stop bank Account.
in postman:
method: get
body:none
url: http:http://localhost:8995/account-requests/1
output:Fallback message : Getting Account Service is unavailable

now start bank Account-main again
url(chrome): http://localhost:8995/actuator/retries
output: {"retries":["account-service"]}
url(chrome): http://localhost:8995/actuator/retryevents
url(chrome): http://localhost:8995/actuator/circuitbreakerevents
zipkin: localhost:9441--click run query.

-----------------------THE END-----------------------

{
  "balance": 1000.0,
  "active": false,
  "customerIdNumber": "ABCD123456"
}

http://localhost:8995/account-requests/create



[ChequeBook service notes]


*start config--8888
*start eureka--8761
*start docker
*open cmd and paste-- ( docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.12-management )
*start rabbitmq-- localhost:5672
*open another cmd and paste-- ( docker run -d -p 9411:9411 openzipkin/zipkin )

*start bank chequebook(using prod)--
*start bank chequebook feign--8997 port

*make sure(eureka,config,docker,rabbitmq,bank chequebook,bank chequebook feign running)

postman:
1.method:post
url: http://localhost:8997/cheque-requests/create
body:
{
  "id": 1,
  "accountNumber": 12345,
  "authorized": false
}
output:
{
    "id": 1,
    "accountNumber": 12345,
    "authorized": false
}

2.method:get
url: http://localhost:8997/cheque-requests/pending
body: none
output:
[
    {
        "id": 1,
        "accountNumber": 12345,
        "authorized": false
    }
]

3.method:get
url: http://localhost:8997/cheque-requests/account/12345
body:none
output:
[
    {
        "id": 1,
        "accountNumber": 12345,
        "authorized": false
    }
]


4.method:post
url: http://localhost:8997/cheque-requests/authorize/1
body:none
output: Chequebook request authorized.


now stop bank Chequebook.
in postman:
method: get
body:none
url: http://localhost:8997/cheque-requests/pending
output:Fallback Message : The Pending Service is unavailable

now start bank transaction-main again
url(chrome): http://localhost:8997/actuator/retries
output: {"retries":["cheque-service"]}
url(chrome): http://localhost:8997/actuator/retryevents
output: u will get some list
url(chrome): http://localhost:8997/actuator/circuitbreakerevents
output: u will get some list
zipkin: localhost:9441--click run query.

-----------------------THE END-----------------------


[Loan service Notes]

*start config--8888
*start eureka--8761
*start docker
*open cmd and paste-- ( docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.12-management )
*start rabbitmq-- localhost:5672
*open another cmd and paste-- ( docker run -d -p 9411:9411 openzipkin/zipkin )

*start bank loan(using prod)--
*start bank loan feign--8998 port

*make sure(eureka,config,docker,rabbitmq,bank loan,bank loan feign running)

postman:
1.method:post
url: http://localhost:8998/loan-requests/create
body:
{
    "accountNumber": 12345,
    "amount": 10000.0,
    "status": false
}
output:
{
    "id": 1,
    "accountNumber": 12345,
    "amount": 10000.0,
    "status": false
}

2.method:get
url: http://localhost:8998/loan-requests/pending
body: none
output:
[
    {
        "id": 1,
        "accountNumber": 12345,
        "amount": 10000.0,
        "status": false
    }
]

3.method:get
url: http://localhost:8998/loan-requests/account/12345
body:none
output:
[
    {
        "id": 1,
        "accountNumber": 12345,
        "amount": 10000.0,
        "status": false
    }
]


4.method:post
url: http://localhost:8998/loan-requests/approve/1
body:none
output: Loan request approved.


now stop bank loan.
in postman:
method: get
body:none
url: http://localhost:8998/loan-requests/pending
output:Fallback Message : The Loan Pending Service is unavailable

now start bank loan-main again
url(chrome): http://localhost:8998/actuator/retries
output: {"retries":["loan-service"]}
url(chrome): http://localhost:8998/actuator/retryevents
output: u will get some list
url(chrome): http://localhost:8998/actuator/circuitbreakerevents
output: u will get some list
zipkin: localhost:9441--click run query.

-----------------------THE END-----------------------


[Transaction service notes]

*start config--8888
*start eureka--8761
*start docker
*open cmd and paste-- ( docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.12-management )
*start rabbitmq-- localhost:5672
*open another cmd and paste-- ( docker run -d -p 9411:9411 openzipkin/zipkin )

*start bank Transaction(using prod)--
*start bank Transaction feign--8992 port

*make sure(eureka,config,docker,rabbitmq,bank Transaction,bank Transaction feign running)

After running both main and feign, goto mysql, inser the below in account table.
INSERT INTO Account (account_number, balance, active, customer_id_number)
VALUES
  (112233, 10000.0, true, 'ABC456'),
  (112244, 5000.0, true, 'XYZ7123');

after inserted, goto postman and perform the below:

postman:
1.method:put
url: http://localhost:8992/transaction-consumer/deposit/112233/100
body:none
output: Deposit successful.

2.method:put
url: http://localhost:8992/transaction-consumer/withdraw/112233/50
body: none
output:Withdrawal successful.

3.method:post
url: http://localhost:8992/transaction-consumer/transfer/112233/445566/10
body:none
output:Transfer successful.


4.method:get
url: http://localhost:8992/transaction-consumer/balance/112233
body:none
output: Your account balance is: 10140.0

5.method:get
url: http://localhost:8992/transaction-consumer/all-transactions
body: none
output: (u will get list of all transactions)


6.method:get
url: http://localhost:8992/transaction-consumer/transaction/1
body: none
output: ( u will get the list of transaction by id)

now stop bank transaction.
in postman:
method: get
body:none
url: http://localhost:8992/transaction-consumer/transaction/1
output:Fallback Message : The Get Transactions By Id Service is unavailable

now start bank transaction-main again
url(chrome): http://localhost:8992/actuator/retries
output: {"retries":["transaction-service"]}
url(chrome): http://localhost:8992/actuator/retryevents
output: u will get some list
url(chrome): http://localhost:8992/actuator/circuitbreakerevents
output: u will get some list
zipkin: localhost:9441--click run query.

-----------------------THE END-----------------------


[User Service Notes]

*start config--8888
*start eureka--8761
*start docker
*open cmd and paste-- ( docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.12-management )
*start rabbitmq-- localhost:5672
*open another cmd and paste-- ( docker run -d -p 9411:9411 openzipkin/zipkin )
http://localhost:9411/zipkin
http://localhost:8761/
*start bank user--
*start bank user feign--8996 port

*make sure(eureka,config,docker,rabbitmq,user,user feign running)

postman:
1.method:post
url: http://localhost:8996/users/create
body:
{
    "username": "janet",
    "password": "12345",
    "approved": false
}
output: User created successfully

2.method:get
url: http://localhost:8996/users/all
output:
[
    {
        "id": 1,
        "cutomerIdNumber": "10000001",
        "username": "janet",
        "password": "12345",
        "approved": false
    }
]

3.method:get
url: http://localhost:8996/users/1
output:
{
    "id": 1,
    "cutomerIdNumber": "10000001",
    "username": "janet",
    "password": "12345",
    "approved": false
}

4.method:post
url: http://localhost:8996/users/approve/1
output: User approved successfully

5.method:delete
url: http://localhost:8996/users/delete/1
output: User deleted successfully

now stop bank user.
in postman:
method: get
url: http://localhost:8996/users/all
output:Fallback Message: Get All Users service is unavailable

now start bank user-main
url: http://localhost:8996/actuator/retries
output: {"retries":["user-service"]}
url: http://localhost:8996/actuator/retryevents
url: http://localhost:8996/actuator/circuitbreakerevents
zipkin: localhost:9441--click run query.

-----------------------THE END-----------------------



http://localhost:8996/users/create







When one microservice needs to communicate with another, it can ask Eureka Server for the location (host and port) of the target microservice. This allows for dynamic service discovery..

. Declarative API: 
 Integration with Spring Cloud: 
 Load Balancing:
 Error Handling: 
Configuration: 
Purpose: The purpose of this class is to enable your Spring Boot application to be run in a servlet container by extending SpringBootServletInitializer and providing a way to configure the application. When you build and package your Spring Boot application as a WAR file, this class allows the application to be initialized properly in a servlet container, ensuring that it can handle HTTP requests and function as a web application.


multiplire is 5 mean

for wiat duration 500ms

500 = 5^0 = 500ms
500 = 5^1 = 2500ms
500 = 5^2 = 12500ms


for wait duration 2s 
 
2 = 5^0 = 2s
2 = 5^1 = 10s
2 = 5^2 = 20s
