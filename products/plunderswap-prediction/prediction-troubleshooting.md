# Prediction Troubleshooting

{% hint style="info" %}
Use the sidebar to quickly find the answers to your questions!
{% endhint %}

## Why aren't the results of my round showing

There’s a 2 block buffer on each round, which can cause delays of up to 45 seconds after the end of a round.\
This buffer is to accommodate for the fact that we may not be able to reliably fetch a price and end a round immediately: various blockchain factors affect the speed in which transactions get confirmed on the network.

## I can’t collect my winnings

Make sure you have enough ZIL in your wallet to pay for gas fees. You’ll need a little ZIL to trigger the smart contract.
