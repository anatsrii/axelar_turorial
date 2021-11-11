# axelar_turorial

### Prerequisites
* MacOs Or ubuntu 18.04 ขึ้นไป (ผมทดสอบบน ubuntu 20.04)
* Docker
* Minimum hardware requirements: 4 cores, 8-16GB RAM, 512 GB drive. 
* Recommended 6-8 cores, 16-32 GB RAM, 1 TB+ drive.

### เตรียม server
* ติดตั้ง Docker
```
sudo apt update
sudo apt install docker.io
```
```
sudo systemctl enable --now docker
```

* เพิ่ม username เข้าไปใน docker group (optional)
```
sudo usermod -aG docker ${USER}
```
```
su - ${USER}
```
* ติดตั้ง jq
```
sudo apt install jq
```
* ติดตั้ง git
```
sudo apt install git
```

### Joining the Axelar testnet

* Clone the repository to use the script and configs:
```
git clone https://github.com/axelarnetwork/axelarate-community.git
cd axelarate-community
```
checkout ไป branch v0.6.1
```
git checkout v0.6.1
```

Run the script 
``` 
join/join-testnet.sh --axelar-core v0.7.10 --tofnd v0.6.1
```
เราสามารถเช็คเวอร์ชั่นล่าสุดได้ที่ [TESTNET RELEASE.md](https://github.com/axelarnetwork/axelarate-community/blob/main/TESTNET%20RELEASE.md)

* Check สถานะ node
```
curl localhost:26657/status | jq '.result.sync_info'
```
ถ้าสถานะ เป็น True แสดงว่า node ยัง sync อยู่ รอจน false ก่อนนะครับ
ถ้าเป็น false แล้วก็เริ่ม Exercise 1 ได้เลย

Ref: [https://github.com/axelarnetwork/axelarate-community](https://github.com/axelarnetwork/axelarate-community)


