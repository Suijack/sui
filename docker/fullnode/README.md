# Docker ile  Sui DevNet Fullnode Çalıştırma

## Kurulum

### Kurulum Kodları

```screen -S sui```

```sudo apt update && sudo apt upgrade -y```

```apt-get update && DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get install -y --no-install-recommends tzdata git ca-certificates curl build-essential libssl-dev pkg-config libclang-dev cmake jq```

```curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh```


```docker_compose_version=$(wget -qO- https://api.github.com/repos/docker/compose/releases/latest | jq -r ".tag_name")```

```sudo wget -O /usr/bin/docker-compose "https://github.com/docker/compose/releases/download/${docker_compose_version}/docker-compose-`uname -s`-`uname -m`"```

```sudo chmod +x /usr/bin/docker-compose```

```
mkdir sui && cd sui

wget -qO docker-compose.yaml https://raw.githubusercontent.com/MystenLabs/sui/main/docker/fullnode/docker-compose.yaml

wget -qO fullnode-template.yaml https://github.com/MystenLabs/sui/raw/main/crates/sui-config/data/fullnode-template.yaml

wget -qO genesis.blob https://github.com/MystenLabs/sui-genesis/raw/main/devnet/genesis.blob
```

```sed -i 's/127.0.0.1/0.0.0.0/' fullnode-template.yaml```

```
docker-compose down --volumes

docker-compose up -d

docker logs -f sui-fullnode-1 --tail 50

```
CTRl A+D ile screenden çıkış yapıyoruz.



## Fullnode Kontrol

Sui fullnode started yazısı görünüyor ve https://node.sui.zvalid.com/  ip niz ile arama yaptığınızda herhangi bir hata yoksa kurulum doğru yapılmıştır. 

## Eğer site üzerinde ip kontrolü yapamıyorsanız portlarda sıkıntı vardır. Bu kodları girmelisiniz. (Ayrıca sunucunuz minumum istenen gereksinimi karşılamalıdır.)

```

sudo ufw allow 9000
sudo ufw allow 9184
sudo ufw allow 8080
cd sui
sed -i 's/127.0.0.1/0.0.0.0/' fullnode-template.yaml
docker-compose restart

```

## Güncelleme Kodları

```
cd sui && rm -rf genesis.blob
wget -qO genesis.blob https://github.com/MystenLabs/sui-genesis/raw/main/devnet/genesis.blob
sed -i 's/main/stable/' docker-compose.yaml
docker-compose down --rmi all -v --remove-orphans
docker rmi -f `docker images -qa `
docker-compose up -d
docker logs -f sui-fullnode-1 --tail 50

```

## Kurulumu Silmek için

```
cd sui && docker-compose down --rmi all -v --remove-orphans

cd $home && rm -rf sui

```
                                  






