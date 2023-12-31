![c25](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/d9d11d33-eb8f-439b-b0d8-6fa59525bbde)# Ali_Khatami_Lottery3(Learning from the video of patrick Collins)

###  Implementing Chainlink VRF(The Request)  

Instead of writing ```yarn hardhat compile``` we can use shortcut and for that hardhat comes with shorthand amd autocomplete <br>

![c12](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/3ed084f4-2f2d-4eee-ac01-d2e72e194b21)

We can find it by going to the link below:
https://hardhat.org/hardhat-runner/docs/guides/command-line-completion

![c13](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/dd9c5bf3-827d-4687-ab0f-51e962dc1936)
On VRFCoordinator address we gonna call this requestRandomWords() function <br>
source:https://docs.chain.link/vrf/v2/subscription/examples/get-a-random-number

![c14](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/273dfb29-92d1-45e8-8a98-b8c9e2f4349b)

We gonna call thsi function on Coordinator Contract <br>

We gonna get Coordinator Contract when we use   VRFCoordinatorV2Interface and 0x8103B0A8A00be2DDC778e6e7eaa21791Cd364625 address    <br> 

```
COORDINATOR = VRFCoordinatorV2Interface(
            0x8103B0A8A00be2DDC778e6e7eaa21791Cd364625
        );

```


![c15](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/3d00b8b7-8f16-4f4a-a387-8b30a60a5ea9)

To get the  VRFCoordinatorV2Interface contract we will copy the above code <br>


![c16](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/cc3218fb-343d-472d-9d22-1901c75ffc10)

And we will paste it here <br>


![c17](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/13a495f2-17d2-461c-9276-bde04a70b47f)
just like price feed we will create a variable vrfCoordinator of type VRFCoordinatorV2Interface <br>

Then we can save this vrfCoordinator using the address <br>


![c18](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/e7185f52-7bc8-4ecc-897e-c2ae40920c44)


For that we write the above line of code <br>

We only gonna set our vrfCoordinator one time rigt in the constaructor <br>


![c19](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/93ed5ed8-c981-4275-9f29-0799d7814f59)

Here our vrfCoordinator is immutable <br>


![c20](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/12b2048b-a38d-4f53-af8e-19691fa18721)

In order to request random words we gonna give it some parameters <br>
For that we gonna copy the above line of code <br>

![c21](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/8d0cd404-1969-4e3b-a1dd-1c2805f8adcb)

Then we change it from Coordiantor to i_vrfCoordinator   <br>


![c22](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/900932b4-47e5-4ac5-8398-6290c5cc4402)

Here the above key hash is known as gas lane <br>


![c23](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/04c00fe8-796f-420f-9f32-e3ed306b2f11)

In the above link https://docs.chain.link/getting-started/intermediates-tutorial we can get the definition<br>
of key hash
The gas lane key hash value, which is the maximum gas price 
you are willing to pay for a request in wei.
It functions as an ID of the off-chain VRF job that runs in response to requests.<br>

Fisrt to pick a gas lane we proabbly gonna have this gas lane or key hash store somewhere <br>


![c24](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/0d9869c0-e403-46d7-8cbc-c660a693623f)

for that we gonna have it as a parameter of our main constructor and gonna save it as <br>
 also a state variable <br>


![c25](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/4459bccc-5e5a-475c-9819-ed4ff95e1741)

 
Then we will declare a new state variable and we only gonna set this once and so we will amke this private  <br>

Now

The subscription ID that this contract uses for funding requests. Initialized in the constructor.<br>

It is actually a contract on blockchain which we can used to fund any subscription for any of this external <br>
data or external computation bits <br>

And in this contract there is a list of this subscription for people to make request to <br>

So we need the ID of the subscription that we are using to request our random numbers <br>

And pay the link oracle gas <br>


![c26](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/b857dc16-9b12-4c6f-84c2-04e488420e11)


The subscription ID is also gonna be something that we are going to pass <br>

as a parameter to our lottery <br>





```solidity

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@chainlink/contracts/src/v0.8/VRFConsumerBaseV2.sol";
import "@chainlink/contracts/src/v0.8/interfaces/VRFCoordinatorV2Interface.sol";

error akrkLottery_NotEnoughEthEntered();

contract akrkLottery is VRFConsumerBaseV2 {
    //below we gonna pick minimum price and it gonna be storage variable
    //visibiliy will be private but it will be configurable
    //We will cover our both storage and non storage variables under state variables section

    /* State variables */

    uint256 private immutable i_welcomeFee;
    //We probably also nedd to track of all the users who entered the lottery
    //participants is a storage variable because we gonna modify this a lot
    address payable[] private s_participants;

    VRFCoordinatorV2Interface private immutable i_vrfCoordinator;

    bytes32 private immutable i_gasLane;

    /* Events */

    event LotteryEnter(address indexed participants);

    // to configure it we st constructor below

    constructor(
        address vrfCoordinatorV2,
        uint256 welcomeFee,
        bytes32 gasLane
    ) VRFConsumerBaseV2(vrfCoordinatorV2) {
        i_welcomeFee = welcomeFee;

        i_vrfCoordinator = VRFCoordinatorV2Interface(vrfCoordinatorV2);

        i_gasLane = gasLane;
    }

    //to enter the lottery we created a function below

    function enterLottery() public payable {
        if (msg.value < i_welcomeFee) {
            revert akrkLottery_NotEnoughEthEntered();
        }

        s_participants.push(payable(msg.sender));

        emit LotteryEnter(msg.sender);
    }

    //to pick a random champion we created the function below
    //The below function is gonna be called by chainlink keepers network

    function requestRandomchampion() external {
        i_vrfCoordinator.requestRandomWords(
            i_gasLane,
            s_subscriptionId,
            requestConfirmations,
            callbackGasLimit,
            numWords
        );
    }

    function fulfillRandomWords(
        uint256 requestId,
        uint256[] memory randomWords
    ) internal override {}

    //we want other users to see entrance fee so we created the function below
    /*View/Pure Function*/
    function getEntranceFee() public view returns (uint256) {
        return i_welcomeFee;
    }

    //to know who are in the participants array the function is created below
    function getParticipant(uint256 index) public view returns (address) {
        return s_participants[index];
    }
}


```


![c27](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/5573a474-f66f-4c9d-b143-abb0a7371c8b)
it will be private since we gonna use it only once <br>

Now is the request confirmation <br>


Request confirmation is How many confirmations the Chainlink node should wait before responding. <br>
The longer the node waits, the more secure the random value is.<br>
It must be greater than the minimumRequestBlockConfirmations limit on the coordinator contract.<br>

So if you make a request and there is one block of confirmation <br>

Maybe we don't actually send it, because you are afraid of some kind of blockchain reorganization <br>



![c28](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/69c6aa3d-a7bb-46cb-b26d-7b19a7bf3be9)



We don't actually gonna worry abot this , we gonna make it constant of three <br>

We not gonna have this parameterizable we actually gonnna have this constant <br>

Next is callbackgaslimit <br>

Callbackgaslimit is The limit for how much gas to use for the callback request to your contract's fulfillRandomWords function.<br>
It must be less than the maxGasLimit on the coordinator contract.<br>
Adjust this value for larger requests depending on how your fulfillRandomWords function processes<br>
and stores the received random values. <br>
If your callbackGasLimit is not sufficient,<br>
the callback will fail and your subscription is still charged<br>
for the work done to generate your requested random values.<br>

This set a limit of how much computataion our fulfillRandomWords can be <br>

This is a way of protecting ourselves from spending too much gas <br>


For example we ccidently code in awy that function fulfillRandomWords is incredibly gas expensive <br>


It will block the random number from responding <br>


We are gonna make this parameterizable because we gonna code how to fulfill random words <br>

![c29](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/98f11d82-e4f9-4b9a-b469-809da6bc3077)

we declare the variable here <br>


![c30](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/1ee64e4c-b6d2-4ebe-b52b-1308185d7b1a)

Then we have assign it in the above way <br>


Next is the number of words is how may random number we want ot get <br>


![c31](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/8aefa5d2-6c43-47ed-a2d7-225cd1352f74)

here we add it like others <br>

![c32](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/a32918d7-2574-41ee-9fa1-94f63fac15a9)
Here we change it like others <br>

```
 function requestRandomchampion() external {
        i_vrfCoordinator.requestRandomWords(
            i_gasLane,
            i_subscriptionId,
            REQUEST_CONFIRMATIONS,
            i_callbackGaslimit,
            NUM_WORDS
        );
    }

```


here in the above ```requestRandomWords``` function returns an unique ID that defines who is requesting these<br>
and all other information <br>

![c33](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/7b13b6e9-6497-419e-bc62-9a9b6468e3ab)


if we wanna sve it we can do the above <br>


![c34](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/8c053655-dc8d-4619-9b27-fb8c738dab9c)


Now for now we gonna emit event with this request ID <br>

![c35](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/aa040cca-55f2-45d1-82e1-f0aaa4098ca4)

Here we wmit the event <br>

![c36](https://github.com/C191068/Ali_Khatami_lottery3/assets/89090776/5f3b6230-447a-4996-bc51-bb5896e1f76b)

Now we gonna set this up so that chainlink keepers can call it on a time interval <br>


```solidity

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@chainlink/contracts/src/v0.8/VRFConsumerBaseV2.sol";
import "@chainlink/contracts/src/v0.8/interfaces/VRFCoordinatorV2Interface.sol";

error akrkLottery_NotEnoughEthEntered();

contract akrkLottery is VRFConsumerBaseV2 {
    //below we gonna pick minimum price and it gonna be storage variable
    //visibiliy will be private but it will be configurable
    //We will cover our both storage and non storage variables under state variables section

    /* State variables */

    uint256 private immutable i_welcomeFee;
    //We probably also nedd to track of all the users who entered the lottery
    //participants is a storage variable because we gonna modify this a lot
    address payable[] private s_participants;

    VRFCoordinatorV2Interface private immutable i_vrfCoordinator;

    bytes32 private immutable i_gasLane;

    uint64 private immutable i_subscriptionId;

    uint32 private immutable i_callbackGaslimit;

    uint16 private constant REQUEST_CONFIRMATIONS = 3;

    uint32 private constant NUM_WORDS = 1;

    /* Events */

    event LotteryEnter(address indexed participants);

    event RequestedLotteryChampion(uint256 indexed requestId);

    // to configure it we st constructor below

    constructor(
        address vrfCoordinatorV2,
        uint256 welcomeFee,
        bytes32 gasLane,
        uint64 subscriptionId,
        uint32 callbackGaslimit
    ) VRFConsumerBaseV2(vrfCoordinatorV2) {
        i_welcomeFee = welcomeFee;

        i_vrfCoordinator = VRFCoordinatorV2Interface(vrfCoordinatorV2);

        i_gasLane = gasLane;

        i_subscriptionId = subscriptionId;

        i_callbackGaslimit = callbackGasLimit;
    }

    //to enter the lottery we created a function below

    function enterLottery() public payable {
        if (msg.value < i_welcomeFee) {
            revert akrkLottery_NotEnoughEthEntered();
        }

        s_participants.push(payable(msg.sender));

        emit LotteryEnter(msg.sender);
    }

    //to pick a random champion we created the function below
    //The below function is gonna be called by chainlink keepers network

    function requestRandomchampion() external {
        uint256 requestId = i_vrfCoordinator.requestRandomWords(
            i_gasLane,
            i_subscriptionId,
            REQUEST_CONFIRMATIONS,
            i_callbackGaslimit,
            NUM_WORDS
        );

        emit RequestedLotteryChampion(requestId);
    }

    function fulfillRandomWords(
        uint256 requestId,
        uint256[] memory randomWords
    ) internal override {}

    //we want other users to see entrance fee so we created the function below
    /*View/Pure Function*/
    function getEntranceFee() public view returns (uint256) {
        return i_welcomeFee;
    }

    //to know who are in the participants array the function is created below
    function getParticipant(uint256 index) public view returns (address) {
        return s_participants[index];
    }
}


```











