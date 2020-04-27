# Bike rental implementation

This is an implementation of a transaction processor for Hyperledger sawtooth blockchain.


![bike rental bpmn](bpmn.svg)

Transaction Family : *bike*

### State

The state would contain at this point only one type of addresses. Those addresses would represent the bike types.


The address would contain the following information

```
string bikeType
uint available 
```

For example we can have two different addresses in our state

- one for a mountain bike
- one for a racing bike

Everytime the ```bike_leasing_tx``` would be executed successfully on the processor. The available number of bikes of that type would decrement. The reverse happens when the ```give_bike_back_tx``` is executed. The state so far is only useful for us to know how many bikes we have. We **GET** and **SET** these addresses only in those two transactions.


### Addressing

``` 5a67b6 ``` this it he prefix for the bike family
The rest of the bytes are unique for the bikeType.


### Transactions 


```request_tx``` : is when a customer starts with a bike request. The customer is identified by his signature. The one he signs with his public key. We can track our customers that way. payload contains (string bikeType, randomly generated id for this specific request)

**payload** :  ``` uint request_id, uint bike_type ```


```insurance_tx```: this is an optional choice for the client. If he decides to take an insurance for his bike he can send an insurance transaction payload. The payload must contain the id of the **request**. Technically this transactions is registered as Tx in the ledger after the Insurer and the Customer agreed to an estimated cost by the Bike Center. We always assume that the payment is successfull and that the customer is willing to pay any sum the Insurer requests.

**payload** :  ``` uint request_id, uint insurer_id, double cost```

```bike_leasing_tx``` : When the request has been processed by the Bike center and the bike is physically ready to be given to the customer. the final payment is also assumed to happen now.

**payload** : ``` uint request_id, string bikeId ```

At this point 2 things can happen. During the usage of the bike by the user, there might be some damage caused by the customer. In this eventuality, we have the following transactions. We assume that customer also always leaves a feedback about the bike, even in case of damage.

```feedback_tx``` : simple message from the customer containing a string message. We can identify who left by looking at the public signature.

**payload** : ``` string message ```

In case of damage the customer reports it with a transaction.

```report_damage_tx``` : report of the damaged cause to the bike.

**payload** : ``` uint request_id, string description ```

```damage_evaluation_tx```: the bike center evaluates the damage and can potentially ask for a refund.

**payload** : ``` uint request_id, bool ask, uint amount```

```give_bike_back_tx```: the customer is done with his lease and returns the bike to the bike center.

**payload** : ```uint request_id, string bikeId```


## Special transactions for the Bike center

```create_bike_tx```: only the bike center is able to execute this transaction. It's aim is to initialize a pool of bikes. 

**payload** : ```string bikeType , uint available```












































