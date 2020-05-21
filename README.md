# Zumi-rpc
Remote procedure call server and client for remote control of [Zumi robots](https://www.robolink.com/zumi/).

* *Python* API mirroring the [*Zumi* API](http://docs.robolink.com/zumi-library) from remote 
* allows compute intensive robot algorithms 
* uses the [*RPYC*](https://rpyc.readthedocs.io/en/latest/) remote procedure call lib for efficient and **secure** remote calls 

## Setup

### On the Zumi and the Client PC
Install ``rpyc``:
```
pip install rpyc
```

### On Zumi
Clone this Git repo:
```
git clone https://github.com/keuperj/Zumi-rpc.gi
```

 
#### Generate Certificates
in the ``Zumi-rpc`` folder, generate server credentials 
```
openssl genrsa -out zumi_rpc.key 4096
openssl req -new -x509 -days 3650 -key zumi_rpc.key -out zumi_rpc.crt
chmod 600 zumi_rpc.key
chmod 600 zumi_rpc.crt
```

Then generate client credentials 
```
openssl genrsa -out client.key 4096
openssl req -new -key client.key -out client.csr  -passin pass:""
openssl x509 -req -days 3652 -in client.csr -CA zumi_rpc.crt -CAkey zumi_rpc.key -set_serial 01 -out client.crt
rm client.csr
```
Now copy the client key and cert to your client machine and set proper rights:
```
chmod 700 client.*
```


#### Start Server
```
python3 RPC-Server.py &
```

## API
Zumi-RPC simply reproduces the original [*Zumi* API](http://docs.robolink.com/zumi-library), with some exceptions:

### Camera
We bypass the Zumi camera methods and interface ``picamera`` directly, allowing us to get high resolution images:
```
get_picture(resolution=(1024,768))
```  
returns a *NumPy* array of size (resolution_x, resolution_y,3) -> RGB image.

