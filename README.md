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

We only gonna set our vrfCoordinator one time <br>





















