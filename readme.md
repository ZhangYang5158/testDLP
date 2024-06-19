https://devopscube.com/create-self-signed-certificates-openssl/


openssl req -x509 \
            -sha256 -days 730 \
            -nodes \
            -newkey rsa:2048 \
            -subj "/CN=Zhizhangyi/C=CN/L=Nanjing" \
            -keyout rootCA.key -out rootCA.crt

openssl genrsa -out server.key 2048

openssl req -new -key server.key -out server.csr -config csr.conf

openssl x509 -req \
    -in server.csr \
    -CA rootCA.crt -CAkey rootCA.key \
    -CAcreateserial -out server.crt \
    -days 730 \
    -sha256 -extfile cert.conf


https://support.kerioconnect.gfi.com/hc/en-us/articles/360015200119-Adding-Trusted-Root-Certificates-to-the-Server

macos
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain rootCA.crt


windows
Import-Certificate -FilePath "C:\CA-PublicKey.Cer" -CertStoreLocation Cert:\LocalMachine\Root
or
certutil.exe -addstore root c:\capublickey.cer

linux
dir /usr/local/share/ca-certificates/
sudo cp foo.crt /usr/local/share/ca-certificates/foo.crt
sudo update-ca-certificates