# Transfer ZRC Tokens from Zil EVM to other Zil wallets!

!!!warning Warning
Please ensure you only transfer ZRC2 tokens back to the scilla side of the network, any native ERC20s cannot be retreived to the other side.
!!!

![](../../.gitbook/assets/PS_HT_ZIL_tokens.png)

Unfortunately we havent yet gotten around to building the nice transfer tool to be able to transfer from Zil EVM back to any native scilla Wallet as yet.  But dont dispair, we can give you the instructions of how to do it!

1. Firstly, you will need to know the 0x underlying address for your Zilpay/Torch wallet.  To get this, you can use the devex explorer! [**HERE**](https://devex.zilliqa.com/?network=https%3A%2F%2Fapi.zilliqa.com)

2. Search for your zil1 wallet address in the "search for a transaction or an address" box, and click search.

![](../../.gitbook/assets/TFEVM1.png)

3. With the web page that shows, next to your zil1 wallet address, there is a little recycle type icon, click this to transpose the zil1 address to the underlying 0x address!  You can copy this address with the button to the right of the recycle button.

![](../../.gitbook/assets/TFEVM2.png)
![](../../.gitbook/assets/TFEVM3.png)

NOTE: For a helpful video explaining this process above, see HERE

[!embed](../../.gitbook/assets/DevEx.mp4)

4. Go to your Zil EVM Wallet (Metamask is the example im using), and click "Send"

![](../../.gitbook/assets/TFEVM4.png)

5. Paste the 0x address you copied from Dev Ex in step 3, into the "Enter public address (0x)" 

![](../../.gitbook/assets/TFEVM5.png)

6. Choose the Token you wish to send, the amount, then click Next.  In this example, im sending 10 ZIL.  Click Next.

!!!warning Warning
If you are transferring any token apart from ZIL, ensure you change the GasLimit to 400,000 (At the moment, there is a bug with scilla contracts and Gas Estimation.  Unfort for now, need to set this manually.  If you use the default gas limit, the transaction will fail!)
!!!

![](../../.gitbook/assets/TFEVM6.png)

7. You will get a confirmation screen as per the below, check over the details, and click Confirm.

![](../../.gitbook/assets/TFEVM7.png)

8. If should go back to your main Metamask wallet, and show the TX as pending.  Once it has been sent, it will then show as Confirmed.  Your tokens should now be in the wallet you sent them to - Congrats!

![](../../.gitbook/assets/TFEVM8.png)
![](../../.gitbook/assets/TFEVM9.png)
