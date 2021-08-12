# Exercise 1
### Mint ERC20 Bitcoin tokens on Ethereum
1.Create a deposit address on Bitcoin
* สร้าง BTC address สำหรับฝาก ด้วยคำสั่ง
```
axelard tx bitcoin link ethereum {ETH address ของเรา} --from validator -y -b block
```
หลังจากรันคำสั่งนี้จะได้ BTC address (สังเกตตรง successfully linked {bitcoin deposit address} and {ethereum Ropsten dst addr})
![Imgur](https://i.imgur.com/PA98n3E.png)
2.send a TEST BTC on Bitcoin testnet to the deposit address specific above, and wait for 6 confirmations
* ฝาก BTC เข้าไปใน address ที่ได้มา โดยไปขอ faucet ที่ [https://bitcoinfaucet.uo1.net/](https://bitcoinfaucet.uo1.net/) เสร็จแล้วรอ 6 confirm นะครับ
* เราสามารถเช็ค tx id ได้ที่ [ https://blockstream.info/testnet/]( https://blockstream.info/testnet/)

3.Confirm the Bitcoin outpoint
* ยืนยัน การฝาก BTC ด้วยคำสั่ง
```
axelard tx bitcoin confirmTxOut "{txID}" "{amount}btc" "{BTC address จากข้อ 1}" --from validator -y -b block
```
เช่น
```
axelard tx bitcoin confirmTxOut 615df0b4d5053630d24bdd7661a13bea28af8bc1eb0e10068d39b4f4f9b6082d:0 0.00088btc tb1qlteveekr7u2qf8faa22gkde37epngsx9d7vgk98ujtzw77c27k7qk2qvup --from validator -y -b block
```

รอจนการทำธุรกรรม confirm เราจะได้ผลลัพธ์แบบนี้

![Imgur](https://i.imgur.com/fwRDoWq.png)

4.Trigger signing of the transfers to Ethereum
* จากนั้น Trigger signing of the transfers to Ethereum ด้วยคำสั่ง
```
axelard tx evm sign-pending-transfers ethereum --from validator -y -b block
```
หลังจากรันคำสั่งข้างบน รออย่างน้อยประมาณ 10 block confirm เพื่อให้ทำการ signing ให้เสร็จก่อน
จากนั้นค้นหา command ID ด้วยคำสั่ง (รันในหน้าต่างใหม่)
```
docker logs -f axelar-core 2>&1 | grep -a -e command
```

![Imgur](https://i.imgur.com/EeLF0lL.png)

5.Get the command data that needs to be sent in an Ethereum transaction in order to execute the mint
* รับข้อมูลคำสั่งที่ต้องใช้เพื่อส่งไปกับธุรกรรมเพื่อใช้ดำเนินการในการ mint ด้วยคำสั่ง
```
axelard q evm command ethereum {commandID ของเรา}
```
เช่น
```
axelard q evm command ethereum 28a523a4d5836df2cdc3af5beffc10ca946e62497d609521504462e043a38fdc
```

จะได้แบบนี้
![Imgur](https://i.imgur.com/qdUeG4b.png)

6.Send the Ethereum transaction wrapping the command data to execute the mint
* ส่งธุรกรรมห่อหุ้มด้วยข้อมูลคำสั่งเพื่อดำเนินการ mint โดยเราจะต้อง ส่ง 0 ETH + command data(ขึ้นต้นด้วย 09c5xxxxx) ไปยัง axelar gate way
* (วิธีส่ง command data metamask ต้องเปิด การใช้งานที่ setting => advance => Hex on)
![Imgur](https://i.imgur.com/xjWlMCM.png)

หลังจากจบขั้นตอนนี้เราจะได้ satoshi กลับมานะครับ

---

### Burn ERC20 wrapped Bitcoin tokens and obtain native Satoshi

1.Create a deposit address on Ethereum
* สร้าง ETH address สำหรับฝากด้วย คำสั่ง
```
axelard tx evm link ethereum bitcoin {BTC address ของเรา} satoshi --from validator -y -b block
```
จะได้แบบนี้ครับ
![Imgur](https://i.imgur.com/Cds9RdV.png)

ดูตรง ```"successfully linked {0x5CFEcE3b659e657E02e31d864ef0adE028a42a8E} and {tb1xxx" ```
เราจะได้ ETH address สำหรับส่ง satoshi กลับไป burn

2.send wrapped tokens to deposit address
* ส่ง satoshi กลับไป burn  จาก address ที่ได้มา
3.Confirm the Ethereum transaction ด้วยคำสั่ง
```
axelard tx evm confirm-erc20-deposit ethereum {txID} {amount} {deposit addr} --from validator -y -b block
```
เช่น
```
axelard tx evm confirm-erc20-deposit ethereum 0x01b00d7ed8f66d558e749daf377ca30ed45f747bbf64f2fd268a6d1ea84f916a 10000 0x5CFEcE3b659e657E02e31d864ef0adE028a42a8E --from validator -y -b block
```

เป็นอันเสร็จสิ้น Exercise 1 ทีนี้ก็รอ tx confirm จาก BTC 
You're done! In the next step, a withdrawal must be signed and submitted to the Bitcoin network. In this release, 
we're triggering these commands about once a day. So come back in 24 hours, 
and check the balance on the Bitcoin testnet address to which you submitted the withdrawal

Ref: [https://github.com/axelarnetwork/axelarate-community/blob/main/Exercises/exercise_1_btc_transfer_cli.md](https://github.com/axelarnetwork/axelarate-community/blob/main/Exercises/exercise_1_btc_transfer_cli.md)


