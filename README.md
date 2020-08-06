# Rynva 
Project for transfer data to GCP from ESP32

Wrote on [PlatformIO](https://platformio.org/) Arduino Framework



## Configure your project:

1. Open `config/config.h` file
2. Change `*ssid` and `*password` variable to credentials for login in you wifi network
3. Change `*project_id`, `*location`, `*registry_id`, `*device_id` to your credentials from GCP Console
4. Generate Public and Private ES256 key. Get the certificate for your region:
    ```bash
    $ openssl req -x509 -newkey rsa:2048 -keyout iot-sample-device-rsa_private.pem -nodes -out rsa_cert.pem -subj /CN=unused
    $ openssl ecparam -genkey -name prime256v1 -noout -out iot-sample-device-ec_private.pem
    $ openssl ec -in iot-sample-device-ec_private.pem -pubout -out iot-sample-device-ec_public.pem
    $ cat rsa_cert.pem
    ```

5. Create in GCP Console IoT Core device and add public key
    ```bash
    $ gcloud iot devices create iot-sample-device -region=europe-west1 -registry=iot-sample-registry -public-key=path=./iot-sample-device-ec_public.pem,type=es256
    ```

6. Get private key (must)
    ```bash
    $ openssl ec -in iot-sample-device-ec_private.pem -noout -text

    read EC key
    Private-Key: (256 bit)
    priv:
    <PRIVATE KEY HERE>
    pub:
    <pub key>
    ASN1 OID: prime256v1
    NIST CURVE: P-256
    ```

7. Insert Private key to `*private_key_str` and RSA certificate to `*root_cert`

8. Run code on the ESP32

