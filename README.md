# Luft
Visualizing air quality in Tromsø

![screenshot](screenshot.png)

# Setup Wiki server

```console
ssh airbit@ifi-web3.ifi.uit.no

cd ./luft

docker stop luft && docker rm luft
git pull && docker build -t luft .
docker run -d -p 80:80 --restart=always --name luft -t luft

exit
```
