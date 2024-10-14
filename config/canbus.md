Currently canbus is not supported by netplan when using ubuntu.
System-networkd should be used instead:
```shell
cat <<EOF | sudo tee /etc/systemd/network/80-can.network
[Match]
Name=can*

[Link]
RequiredForOnline=no

[CAN]
# Make sure the bit rate is configured correctly, for MCP2515 it's 1Mb/s
BitRate=1M
EOF
```

After can interface is up, go into the klipper tools image and scan for devices:
```shell
sudo docker compose -f docker-compose.extra.bash.yaml run --rm bash
ln -sr /usr/bin/python3 /usr/local/bin/python
source /opt/venv/bin/activate
python scripts/canbus_query.py can0
```