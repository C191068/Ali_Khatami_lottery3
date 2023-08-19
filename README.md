# Ali_Khatami_Lottery3(Learning from the video of patrick Collins)

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

 
Then we will declare a new stae variable and we only gonna set this once <br>















